<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Escáner QR Pro</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html5-qrcode/2.3.8/html5-qrcode.min.js"></script>
    <style>
        :root {
            --primary: #4f46e5;
            --primary-hover: #4338ca;
            --background: #f3f4f6;
            --card: #ffffff;
            --text: #1f2937;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            background-color: var(--background);
            color: var(--text);
            margin: 0;
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .container {
            width: 100%;
            max-width: 500px;
            background: var(--card);
            padding: 20px;
            border-radius: 16px;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
            box-sizing: border-box;
        }

        h1 {
            text-align: center;
            font-size: 1.5rem;
            margin-bottom: 20px;
        }

        .btn {
            display: block;
            width: 100%;
            background-color: var(--primary);
            color: white;
            border: none;
            padding: 12px;
            font-size: 1rem;
            font-weight: bold;
            border-radius: 8px;
            cursor: pointer;
            transition: background 0.2s;
            margin-bottom: 15px;
        }

        .btn:hover {
            background-color: var(--primary-hover);
        }

        .btn-stop {
            background-color: #ef4444;
        }
        .btn-stop:hover {
            background-color: #dc2626;
        }

        #reader {
            width: 100%;
            border-radius: 8px;
            overflow: hidden;
            border: none !important; /* Quita el borde por defecto de la librería */
        }

        .history {
            margin-top: 25px;
        }

        .history h2 {
            font-size: 1.1rem;
            border-bottom: 2px solid var(--background);
            padding-bottom: 8px;
        }

        .history-list {
            list-style: none;
            padding: 0;
            max-height: 300px;
            overflow-y: auto;
        }

        .history-item {
            background: var(--background);
            padding: 10px 12px;
            border-radius: 8px;
            margin-bottom: 8px;
            font-size: 0.9rem;
            word-break: break-all;
        }

        .history-item .date {
            font-size: 0.75rem;
            color: #6b7280;
            display: block;
            margin-bottom: 4px;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>Escáner de Códigos QR</h1>
    
    <button id="btn-toggle" class="btn">Activar Cámara</button>
    
    <div id="reader" style="display: none;"></div>

    <div class="history">
        <h2>Códigos Registrados</h2>
        <ul id="history-list" class="history-list">
            </ul>
    </div>
</div>

<script>
    const btnToggle = document.getElementById('btn-toggle');
    const readerDiv = document.getElementById('reader');
    const historyList = document.getElementById('history-list');
    
    let html5QrCode;
    let isScanning = false;

    // Instanciar el escáner (pasando el ID del contenedor HTML)
    html5QrCode = new Html5Qrcode("reader");

    btnToggle.addEventListener('click', () => {
        if (!isScanning) {
            startScanner();
        } else {
            stopScanner();
        }
    });

    function startScanner() {
        readerDiv.style.display = 'block';
        btnToggle.textContent = 'Apagar Cámara';
        btnToggle.classList.add('btn-stop');

        // Configuración para móviles: 'environment' prioriza la cámara trasera
        const config = { fps: 10, qrbox: { width: 250, height: 250 } };

        html5QrCode.start(
            { facingMode: "environment" }, 
            config,
            onScanSuccess
        ).then(() => {
            isScanning = true;
        }).catch((err) => {
            console.error("Error al iniciar la cámara:", err);
            alert("No se pudo acceder a la cámara. Asegúrate de dar los permisos necesarios.");
            resetUI();
        });
    }

    function stopScanner() {
        if (html5QrCode) {
            html5QrCode.stop().then(() => {
                resetUI();
            }).catch((err) => {
                console.error("Error al detener la cámara:", err);
            });
        }
    }

    function resetUI() {
        readerDiv.style.display = 'none';
        btnToggle.textContent = 'Activar Cámara';
        btnToggle.classList.remove('btn-stop');
        isScanning = false;
    }

    // Función que se ejecuta cuando se detecta un QR con éxito
    function onScanSuccess(decodedText, decodedResult) {
        // Detenemos la cámara momentáneamente para evitar múltiples lecturas del mismo código
        stopScanner();

        // Obtener fecha y hora actual formateada
        const ahora = new Date();
        const fechaHoraFormateada = ahora.toLocaleString('es-ES', {
            day: '2-digit',
            month: '2-digit',
            year: 'numeric',
            hour: '2-digit',
            minute: '2-digit',
            second: '2-digit'
        });

        // Guardar en la lista (puedes adaptarlo para guardar en LocalStorage si lo requieres permanente)
        agregarAlHistorial(decodedText, fechaHoraFormateada);
        
        // Alerta visual de éxito
        alert(`¡Código detectado con éxito!\nContenido: ${decodedText}`);
    }

    function agregarAlHistorial(texto, fechaHora) {
        const li = document.createElement('li');
        li.classList.add('history-item');
        
        li.innerHTML = `
            <span class="date">📅 ${fechaHora}</span>
            <strong>Contenido:</strong> ${texto}
        `;
        
        // Insertar al principio de la lista para ver lo más reciente arriba
        historyList.insertBefore(li, historyList.firstChild);
    }
</script>

</body>
</html>
