[index.html2g.html](https://github.com/user-attachments/files/28529090/index.html2g.html)
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema de Gestión Escolar - Pro</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>
    <script src="https://rawgit.com/schmich/instascan-builds/master/instascan.min.js"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        .tab-content { display: none; }
        .tab-content.active { display: block; }
    </style>
</head>
<body class="bg-gray-50 font-sans text-gray-800">

    <nav class="bg-blue-600 text-white shadow-md sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex items-center justify-between h-16">
                <div class="flex items-center space-x-2">
                    <i class="gradient-text" data-lucide="graduation-cap"></i>
                    <span class="font-bold text-xl tracking-tight">EduManage</span>
                </div>
                <div class="flex space-x-4">
                    <button onclick="switchTab('estudiantes')" class="tab-btn px-3 py-2 rounded-md text-sm font-medium bg-blue-700">Estudiantes</button>
                    <button onclick="switchTab('asistencia')" class="tab-btn px-3 py-2 rounded-md text-sm font-medium hover:bg-blue-500">Asistencia y QR</button>
                </div>
            </div>
        </div>
    </nav>

    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
        
        <div id="tab-estudiantes" class="tab-content active space-y-6">
            <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 bg-white p-6 rounded-xl shadow-sm">
                <div>
                    <h1 class="text-2xl font-bold text-gray-900">Panel de Estudiantes</h1>
                    <p class="text-sm text-gray-500">Administra el listado, agrega, edita o elimina registros.</p>
                </div>
                <div class="flex flex-wrap gap-2">
                    <button onclick="abrirFormularioEstudiante()" class="bg-green-600 hover:bg-green-700 text-white px-4 py-2 rounded-lg flex items-center gap-2 text-sm font-medium shadow">
                        <i data-lucide="user-plus" class="w-4 h-4"></i> Nuevo Estudiante
                    </button>
                    <button onclick="exportarBackup()" class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded-lg flex items-center gap-2 text-sm font-medium shadow">
                        <i data-lucide="download" class="w-4 h-4"></i> Exportar Backup (JS)
                    </button>
                </div>
            </div>

            <div class="bg-white rounded-xl shadow-sm overflow-hidden">
                <div class="overflow-x-auto">
                    <table class="min-w-full divide-y divide-gray-200 text-left">
                        <thead class="bg-gray-50">
                            <tr>
                                <th class="px-6 py-3 text-xs font-medium text-gray-500 uppercase tracking-wider">DNI</th>
                                <th class="px-6 py-3 text-xs font-medium text-gray-500 uppercase tracking-wider">Estudiante</th>
                                <th class="px-6 py-3 text-xs font-medium text-gray-500 uppercase tracking-wider">Género</th>
                                <th class="px-6 py-3 text-xs font-medium text-gray-500 uppercase tracking-wider">Edad</th>
                                <th class="px-6 py-3 text-xs font-medium text-gray-500 uppercase tracking-wider">Teléfono</th>
                                <th class="px-6 py-3 text-right text-xs font-medium text-gray-500 uppercase tracking-wider">Acciones</th>
                            </tr>
                        </thead>
                        <tbody id="tabla-estudiantes-body" class="bg-white divide-y divide-gray-200">
                            </tbody>
                    </table>
                </div>
            </div>
        </div>

        <div id="tab-asistencia" class="tab-content space-y-6">
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                
                <div class="bg-white p-6 rounded-xl shadow-sm space-y-6">
                    <div>
                        <h2 class="text-xl font-bold text-gray-900 flex items-center gap-2">
                            <i data-lucide="qr-code" class="text-blue-600"></i> Escáner de Asistencia
                        </h2>
                        <p class="text-xs text-gray-500 mt-1">Escanea el DNI del alumno usando la cámara para registrar su ingreso.</p>
                    </div>
                    
                    <div class="relative bg-black rounded-lg overflow-hidden aspect-video shadow-inner flex items-center justify-center text-white text-sm">
                        <video id="preview" class="w-full h-full object-cover"></video>
                        <div id="camera-placeholder" class="absolute inset-0 flex flex-col items-center justify-center bg-gray-900 gap-2">
                            <i data-lucide="video-off" class="w-8 h-8 text-gray-400"></i>
                            <span>Cámara apagada</span>
                        </div>
                    </div>

                    <div class="flex gap-2">
                        <button onclick="encenderCamara()" class="flex-1 bg-blue-600 hover:bg-blue-700 text-white py-2 px-3 rounded-lg text-xs font-medium flex items-center justify-center gap-1">
                            <i data-lucide="camera" class="w-4 h-4"></i> Encender
                        </button>
                        <button onclick="apagarCamara()" class="flex-1 bg-gray-600 hover:bg-gray-700 text-white py-2 px-3 rounded-lg text-xs font-medium flex items-center justify-center gap-1">
                            <i data-lucide="video-off" class="w-4 h-4"></i> Apagar
                        </button>
                    </div>

                    <hr class="border-gray-200">

                    <div class="space-y-3">
                        <h3 class="text-sm font-semibold text-gray-700">Gestión Manual de Asistencias</h3>
                        <button onclick="abrirFormularioAsistenciaManual()" class="w-full bg-indigo-600 hover:bg-indigo-700 text-white py-2.5 px-4 rounded-lg text-sm font-medium flex items-center justify-center gap-2 shadow">
                            <i data-lucide="plus-circle" class="w-4 h-4"></i> Crear/Editar Registro Manual
                        </button>
                    </div>
                </div>

                <div class="lg:col-span-2 bg-white p-6 rounded-xl shadow-sm space-y-4">
                    <div class="flex justify-between items-center">
                        <h2 class="text-xl font-bold text-gray-900 flex items-center gap-2">
                            <i data-lucide="calendar" class="text-blue-600"></i> Calendario de Asistencias
                        </h2>
                        <input type="date" id="fecha-asistencia" onchange="renderizarAsistenciasFecha()" class="border border-gray-300 rounded-lg p-2 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500">
                    </div>

                    <div class="border border-gray-100 rounded-xl overflow-hidden shadow-sm">
                        <div class="bg-gray-50 p-4 border-b border-gray-100 flex justify-between text-sm font-medium text-gray-600">
                            <span>Resumen del Día Seleccionado</span>
                            <span id="contador-asistencias" class="bg-blue-100 text-blue-800 px-2 py-0.5 rounded-full text-xs">0 Asistencias</span>
                        </div>
                        <div class="overflow-y-auto max-h-[350px]">
                            <table class="min-w-full divide-y divide-gray-200 text-left">
                                <thead class="bg-gray-50/50">
                                    <tr>
                                        <th class="px-6 py-2.5 text-xs font-medium text-gray-500 uppercase">Estudiante</th>
                                        <th class="px-6 py-2.5 text-xs font-medium text-gray-500 uppercase">DNI</th>
                                        <th class="px-6 py-2.5 text-xs font-medium text-gray-500 uppercase">Hora</th>
                                        <th class="px-6 py-2.5 text-xs font-medium text-gray-500 uppercase">Estado</th>
                                        <th class="px-6 py-2.5 text-right text-xs font-medium text-gray-500 uppercase">Acción</th>
                                    </tr>
                                </thead>
                                <tbody id="tabla-asistencias-body" class="bg-white divide-y divide-gray-200 text-sm">
                                    </tbody>
                            </table>
                        </div>
                    </div>
                </div>

            </div>
        </div>

    </div>

    <script>
        // Inicialización de los 20 registros requeridos
        let estudiantes = [
            { dni: "74829104", nombres: "Carlos Alberto", apellidos: "Mendoza Ruiz", genero: "Masculino", edad: 19, telefono: "981234567" },
            { dni: "46193820", nombres: "Ana Lucía", apellidos: "Palacios Gómez", genero: "Femenino", edad: 21, telefono: "955781234" },
            { dni: "71048293", nombres: "Mateo Alexander", apellidos: "Quispe Flores", genero: "Masculino", edad: 18, telefono: "923456781" },
            { dni: "48392015", nombres: "Valeria Sofía", apellidos: "Castro Villalobos", genero: "Femenino", edad: 22, telefono: "941823764" },
            { dni: "72910483", nombres: "Luis Fernando", Rojas: "Torres", apellidos: "Rojas Torres", genero: "Masculino", edad: 20, telefono: "973451289" },
            { dni: "45019283", nombres: "María Fernanda", apellidos: "Chávez Salazar", genero: "Femenino", edad: 23, telefono: "962184735" },
            { dni: "76182930", nombres: "Diego Alonso", apellidos: "Benítez Ramos", genero: "Masculino", edad: 19, telefono: "917634289" },
            { dni: "49201834", nombres: "Camila Belén", apellidos: "Villanueva Vega", genero: "Femenino", edad: 20, telefono: "989123456" },
            { dni: "73849201", nombres: "Juan Pablo", apellidos: "Delgado Ortiz", genero: "Masculino", edad: 24, telefono: "934567812" },
            { dni: "44102938", nombres: "Andrea Carolina", apellidos: "Gutiérrez Nuñez", genero: "Femenino", edad: 21, telefono: "952817364" },
            { dni: "75920184", nombres: "Sebastián Noel", apellidos: "Paredes Cárdenas", genero: "Masculino", edad: 18, telefono: "966342819" },
            { dni: "47392018", nombres: "Daniela Alejandra", apellidos: "Silva Morales", genero: "Femenino", edad: 22, telefono: "945182736" },
            { dni: "70291843", nombres: "Nicolás Eduardo", apellidos: "Espinoza Fuentes", genero: "Masculino", edad: 20, telefono: "921743829" },
            { dni: "46819203", nombres: "Claudia Marcela", apellidos: "Herrera Castillo", genero: "Femenino", edad: 25, telefono: "993821746" },
            { dni: "71938402", nombres: "Gabriel Omar", apellidos: "Vargas Machuca", genero: "Masculino", edad: 19, telefono: "954123678" },
            { dni: "49501823", nombres: "Natalia Inés", apellidos: "Miranda Guerrero", genero: "Femenino", edad: 21, telefono: "912837465" },
            { dni: "74019283", nombres: "Santiago José", apellidos: "Medina Farfán", genero: "Masculino", edad: 23, telefono: "976543210" },
            { dni: "43920184", nombres: "Luciana Beatriz", apellidos: "Campos Ríos", genero: "Femenino", edad: 20, telefono: "938472615" },
            { dni: "76829104", nombres: "Matías Ignacio", apellidos: "Romero Cáceres", genero: "Masculino", edad: 22, telefono: "987654321" },
            { dni: "45291038", nombres: "Gabriela Alexandra", apellidos: "Solís Soto", genero: "Femenino", edad: 19, telefono: "949281736" }
        ];

        // Formato estructurado de asistencias: { "YYYY-MM-DD": { "DNI": { hora: "HH:MM", estado: "Presente"/"Tardanza" } } }
        let asistencias = {};
        let scanner = null;

        document.addEventListener("DOMContentLoaded", () => {
            // Establecer fecha de hoy en el input de calendario
            const hoy = new Date().toISOString().split('T')[0];
            document.getElementById('fecha-asistencia').value = hoy;
            
            renderizarEstudiantes();
            renderizarAsistenciasFecha();
            lucide.createIcons();
        });

        // --- SISTEMA NAVEGACIÓN ---
        function switchTab(tabName) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.remove('active'));
            document.querySelectorAll('.tab-btn').forEach(el => el.classList.remove('bg-blue-700'));
            
            document.getElementById(`tab-${tabName}`).classList.add('active');
            event.currentTarget.classList.add('bg-blue-700');
        }

        // --- CRUD ESTUDIANTES ---
        function renderizarEstudiantes() {
            const tbody = document.getElementById('tabla-estudiantes-body');
            tbody.innerHTML = '';
            
            estudiantes.forEach((estudiante) => {
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td class="px-6 py-4 whitespace-nowrap font-mono text-sm text-gray-600">${estudiante.dni}</td>
                    <td class="px-6 py-4 whitespace-nowrap font-medium text-gray-900">${estudiante.apellidos}, ${estudiante.nombres}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${estudiante.genero}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${estudiante.edad} años</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${estudiante.telefono}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-right text-sm font-medium space-x-2">
                        <button onclick="abrirFormularioEstudiante('${estudiante.dni}')" class="text-blue-600 hover:text-blue-900 bg-blue-50 px-2 py-1 rounded">Editar</button>
                        <button onclick="eliminarEstudiante('${estudiante.dni}')" class="text-red-600 hover:text-red-900 bg-red-50 px-2 py-1 rounded">Eliminar</button>
                    </td>
                `;
                tbody.appendChild(tr);
            });
            lucide.createIcons();
        }

        async function abrirFormularioEstudiante(dniEditar = null) {
            const est = dniEditar ? estudiantes.find(e => e.dni === dniEditar) : null;
            
            const { value: formValues } = await Swal.fire({
                title: est ? 'Editar Datos del Estudiante' : 'Registrar Nuevo Estudiante',
                html: `
                    <div class="text-left space-y-3">
                        <div>
                            <label class="block text-xs font-semibold text-gray-600 mb-1">DNI</label>
                            <input id="swal-dni" class="swal2-input m-0 w-full rounded-md border-gray-300 shadow-sm" placeholder="8 dígitos" value="${est ? est.dni : ''}" ${est ? 'disabled' : ''}>
                        </div>
                        <div>
                            <label class="block text-xs font-semibold text-gray-600 mb-1">Nombres</label>
                            <input id="swal-nombres" class="swal2-input m-0 w-full rounded-md border-gray-300" placeholder="Ej. Juan Carlos" value="${est ? est.nombres : ''}">
                        </div>
                        <div>
                            <label class="block text-xs font-semibold text-gray-600 mb-1">Apellidos</label>
                            <input id="swal-apellidos" class="swal2-input m-0 w-full rounded-md border-gray-300" placeholder="Ej. Pérez Gómez" value="${est ? est.apellidos : ''}">
                        </div>
                        <div class="grid grid-cols-2 gap-2">
                            <div>
                                <label class="block text-xs font-semibold text-gray-600 mb-1">Género</label>
                                <select id="swal-genero" class="swal2-input m-0 w-full rounded-md border-gray-300">
                                    <option value="Masculino" ${est && est.genero === 'Masculino' ? 'selected' : ''}>Masculino</option>
                                    <option value="Femenino" ${est && est.genero === 'Femenino' ? 'selected' : ''}>Femenino</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs font-semibold text-gray-600 mb-1">Edad</label>
                                <input id="swal-edad" type="number" class="swal2-input m-0 w-full rounded-md border-gray-300" value="${est ? est.edad : ''}">
                            </div>
                        </div>
                        <div>
                            <label class="block text-xs font-semibold text-gray-600 mb-1">Teléfono</label>
                            <input id="swal-telefono" class="swal2-input m-0 w-full rounded-md border-gray-300" placeholder="9 dígitos" value="${est ? est.telefono : ''}">
                        </div>
                    </div>
                `,
                focusConfirm: false,
                showCancelButton: true,
                confirmButtonText: 'Guardar',
                cancelButtonText: 'Cancelar',
                preConfirm: () => {
                    const dni = document.getElementById('swal-dni').value.trim();
                    const nombres = document.getElementById('swal-nombres').value.trim();
                    const apellidos = document.getElementById('swal-apellidos').value.trim();
                    const genero = document.getElementById('swal-genero').value;
                    const edad = parseInt(document.getElementById('swal-edad').value);
                    const telefono = document.getElementById('swal-telefono').value.trim();

                    if(!dni || !nombres || !apellidos || isNaN(edad) || !telefono) {
                        Swal.showValidationMessage('Por favor completa todos los campos correctamente');
                        return false;
                    }
                    if(!est && estudiantes.some(e => e.dni === dni)) {
                        Swal.showValidationMessage('Este número de DNI ya está registrado');
                        return false;
                    }
                    return { dni, nombres, apellidos, genero, edad, telefono };
                }
            });

            if (formValues) {
                if (est) {
                    // Actualizar
                    Object.assign(est, formValues);
                    Swal.fire('¡Actualizado!', 'Datos modificados con éxito.', 'success');
                } else {
                    // Crear
                    estudiantes.push(formValues);
                    Swal.fire('¡Registrado!', 'Estudiante añadido al sistema.', 'success');
                }
                renderizarEstudiantes();
                renderizarAsistenciasFecha();
            }
        }

        function eliminarEstudiante(dni) {
            Swal.fire({
                title: '¿Estás seguro?',
                text: `Se borrará el registro del estudiante con DNI: ${dni}`,
                icon: 'warning',
                showCancelButton: true,
                confirmButtonColor: '#d33',
                cancelButtonColor: '#3085d6',
                confirmButtonText: 'Sí, borrar',
                cancelButtonText: 'Cancelar'
            }).then((result) => {
                if (result.isConfirmed) {
                    estudiantes = estudiantes.filter(e => e.dni !== dni);
                    // Eliminarlo también de los históricos de asistencia
                    Object.keys(asistencias).forEach(fecha => {
                        delete asistencias[fecha][dni];
                    });
                    Swal.fire('Eliminado', 'El estudiante ha sido removido.', 'success');
                    renderizarEstudiantes();
                    renderizarAsistenciasFecha();
                }
            });
        }

        // --- SISTEMA ASISTENCIA (QR Y MANUAL) ---
        function renderizarAsistenciasFecha() {
            const fechaSeleccionada = document.getElementById('fecha-asistencia').value;
            const tbody = document.getElementById('tabla-asistencias-body');
            tbody.innerHTML = '';

            if(!fechaSeleccionada) return;

            const registrosDelDia = asistencias[fechaSeleccionada] || {};
            const dnisAsistieron = Object.keys(registrosDelDia);
            
            document.getElementById('contador-asistencias').innerText = `${dnisAsistieron.length} Asistencias`;

            if(dnisAsistieron.length === 0) {
                tbody.innerHTML = `<tr><td colspan="5" class="px-6 py-8 text-center text-gray-400">No hay registros de asistencias para esta fecha.</td></tr>`;
                return;
            }

            dnisAsistieron.forEach(dni => {
                const est = estudiantes.find(e => e.dni === dni);
                const infoAsis = registrosDelDia[dni];
                const nombreCompleto = est ? `${est.apellidos}, ${est.nombres}` : "Estudiante Eliminado/No Encontrado";
                
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td class="px-6 py-3 font-medium text-gray-900">${nombreCompleto}</td>
                    <td class="px-6 py-3 font-mono text-xs text-gray-500">${dni}</td>
                    <td class="px-6 py-3 text-gray-600">${infoAsis.hora}</td>
                    <td class="px-6 py-3">
                        <span class="px-2 py-1 rounded-full text-xs font-semibold ${infoAsis.estado === 'Presente' ? 'bg-green-100 text-green-800' : 'bg-amber-100 text-amber-800'}">
                            ${infoAsis.estado}
                        </span>
                    </td>
                    <td class="px-6 py-3 text-right">
                        <button onclick="eliminarAsistenciaIndividual('${fechaSeleccionada}', '${dni}')" class="text-red-500 hover:text-red-700 font-medium">Remover</button>
                    </td>
                `;
                tbody.appendChild(tr);
            });
        }

        function registrarAsistenciaLogica(dni, fecha, hora, estado) {
            if(!estudiantes.some(e => e.dni === dni)) {
                return { success: false, msg: `El DNI ${dni} no pertenece a ningún estudiante.` };
            }
            if(!asistencias[fecha]) {
                asistencias[fecha] = {};
            }
            asistencias[fecha][dni] = { hora, estado };
            return { success: true, msg: `Asistencia guardada para la fecha ${fecha}.` };
        }

        // Interfaz manual integrada solicitada
        async function abrirFormularioAsistenciaManual() {
            const fechaSeleccionada = document.getElementById('fecha-asistencia').value;
            
            // Crear opciones del selector con los estudiantes vigentes
            let opcionesEstudiantes = estudiantes.map(e => `<option value="${e.dni}">${e.apellidos}, ${e.nombres} (${e.dni})</option>`).join('');

            const { value: formValues } = await Swal.fire({
                title: 'Control de Asistencia Manual',
                html: `
                    <div class="text-left space-y-3">
                        <div>
                            <label class="block text-xs font-semibold text-gray-600 mb-1">Fecha de Registro</label>
                            <input id="asis-fecha" type="date" class="swal2-input m-0 w-full" value="${fechaSeleccionada}">
                        </div>
                        <div>
                            <label class="block text-xs font-semibold text-gray-600 mb-1">Seleccionar Estudiante</label>
                            <select id="asis-dni" class="swal2-input m-0 w-full">${opcionesEstudiantes}</select>
                        </div>
                        <div class="grid grid-cols-2 gap-2">
                            <div>
                                <label class="block text-xs font-semibold text-gray-600 mb-1">Hora</label>
                                <input id="asis-hora" type="time" class="swal2-input m-0 w-full" value="${new Date().toTimeString().slice(0,5)}">
                            </div>
                            <div>
                                <label class="block text-xs font-semibold text-gray-600 mb-1">Condición</label>
                                <select id="asis-estado" class="swal2-input m-0 w-full">
                                    <option value="Presente">Presente</option>
                                    <option value="Tardanza">Tardanza</option>
                                </select>
                            </div>
                        </div>
                    </div>
                `,
                focusConfirm: false,
                showCancelButton: true,
                confirmButtonText: 'Registrar',
                cancelButtonText: 'Cancelar',
                preConfirm: () => {
                    return {
                        fecha: document.getElementById('asis-fecha').value,
                        dni: document.getElementById('asis-dni').value,
                        hora: document.getElementById('asis-hora').value,
                        estado: document.getElementById('asis-estado').value
                    }
                }
            });

            if(formValues) {
                const res = registrarAsistenciaLogica(formValues.dni, formValues.fecha, formValues.hora, formValues.estado);
                if(res.success) {
                    Swal.fire('Guardado', 'Registro de asistencia actualizado.', 'success');
                    renderizarAsistenciasFecha();
                } else {
                    Swal.fire('Error', res.msg, 'error');
                }
            }
        }

        function eliminarAsistenciaIndividual(fecha, dni) {
            if(asistencias[fecha] && asistencias[fecha][dni]) {
                delete asistencias[fecha][dni];
                if(Object.keys(asistencias[fecha]).length === 0) delete asistencias[fecha];
                renderizarAsistenciasFecha();
            }
        }

        // --- INTEGRACIÓN CÁMARA & QR (Instascan) ---
        function encenderCamara() {
            document.getElementById('camera-placeholder').classList.add('hidden');
            
            if(!scanner) {
                scanner = new Instascan.Scanner({ video: document.getElementById('preview'), mirror: false });
                
                scanner.addListener('scan', function (content) {
                    // El QR debe contener solo el número de DNI del alumno
                    const dniEscaneado = content.trim();
                    const hoy = new Date().toISOString().split('T')[0];
                    const horaActual = new Date().toTimeString().slice(0,5);

                    // Validar y guardar automáticamente
                    const res = registrarAsistenciaLogica(dniEscaneado, hoy, horaActual, "Presente");
                    
                    if(res.success) {
                        const est = estudiantes.find(e => e.dni === dniEscaneado);
                        Swal.fire({
                            title: '¡Asistencia Registrada!',
                            text: `${est.nombres} ${est.apellidos} ingresó correctamente.`,
                            icon: 'success',
                            timer: 2000,
                            showConfirmButton: false
                        });
                        renderizarAsistenciasFecha();
                    } else {
                        Swal.fire('Lectura fallida', res.msg, 'warning');
                    }
                });
            }

            Instascan.Camera.getCameras().then(function (cameras) {
                if (cameras.length > 0) {
                    // Si se accede desde móvil, usar la cámara trasera (usualmente el último índice)
                    let seleccionada = cameras[0];
                    if(cameras.length > 1) {
                        seleccionada = cameras[cameras.length - 1]; 
                    }
                    scanner.start(seleccionada);
                } else {
                    Swal.fire('Error de Hardware', 'No se detectaron cámaras en el dispositivo.', 'error');
                    document.getElementById('camera-placeholder').classList.remove('hidden');
                }
            }).catch(function (e) {
                console.error(e);
                Swal.fire('Permiso denegado', 'No se pudo acceder a la cámara.', 'error');
                document.getElementById('camera-placeholder').classList.remove('hidden');
            });
        }

        function apagarCamara() {
            if(scanner) {
                scanner.stop();
                scanner = null;
            }
            document.getElementById('camera-placeholder').classList.remove('hidden');
        }

        // --- EXPORTAR BACKUP (JS) ---
        function exportarBackup() {
            // Unimos ambas estructuras de memoria en un objeto formateado
            const backupData = {
                fechaGeneracion: new Date().toLocaleString(),
                estudiantes: estudiantes,
                asistencias: asistencias
            };

            // Lo convertimos en cadena de texto simulando un módulo o declaración JS
            const stringData = `// BACKUP SISTEMA DE GESTIÓN ESCOLAR\nconst BACKUP_DATA = ${JSON.stringify(backupData, null, 4)};`;
            
            const blob = new Blob([stringData], { type: "application/javascript;charset=utf-8;" });
            const link = document.createElement("a");
            
            const fechaArchivo = new Date().toISOString().slice(0,10);
            link.href = URL.createObjectURL(blob);
            link.setAttribute("download", `backup_sistema_${fechaArchivo}.js`);
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
        }
    </script>
</body>
</html>
