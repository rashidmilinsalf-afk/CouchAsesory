<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>💪 Mi App de Entrenamiento - Coach Edition</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Oculta todas las vistas por defecto, solo mostraremos una a la vez */
        .view { display: none; }
        /* Ajuste para que el botón de volver en la vista de ejercicio no se solape con el header */
        #exercise-view #back-to-details { top: 2rem; left: 1rem; }
        /* Estilo para los días con cita en el calendario */
        .has-appointment {
            background-color: #6366f1; /* Indigo-500 */
            color: white;
            font-weight: bold;
            border-radius: 50%;
        }
        .current-day {
            border: 3px solid #f97316; /* Orange-500 */
            border-radius: 50%;
        }
        /* Cursor para el botón secreto del Modo Coach / Perfil */
        #welcome-message, #user-level-display { cursor: default; }
    </style>
</head>
<body class="bg-gray-100 min-h-screen">
    <header class="bg-indigo-600 text-white p-4 shadow-lg flex justify-between items-center">
        <h1 class="text-2xl font-bold">Gym Guide App</h1>
        <div id="user-info" class="text-right text-sm flex items-center space-x-3">
            <button id="show-profile-btn" class="text-white hover:bg-indigo-100 font-semibold text-sm py-1 px-2 rounded-lg transition hidden" title="Ver Perfil/Progreso">
                👤
            </button>
            <div id="user-display">
                <p id="welcome-message" class="font-semibold">Modo Coach</p>
                <p id="user-level-display" class="text-xs opacity-80">Asignación de Rutinas</p>
            </div>
        </div>
        <button id="show-calendar-btn" class="bg-indigo-400 hover:bg-indigo-500 text-white py-1 px-3 rounded-lg text-sm transition hidden">
            📅 Agenda
        </button>
    </header>

    <div id="routines-view" class="view p-4">
        <h2 class="text-xl font-semibold mb-4" id="routine-view-title">🏋️ Tu Rutina Asignada</h2>
        
        <div id="history-summary" class="bg-indigo-100 p-3 rounded-lg text-sm mb-4">
            </div>

        <div id="routines-list" class="space-y-3">
            </div>
    </div>

    <div id="details-view" class="view p-4">
        <button id="back-to-routines" class="text-indigo-600 mb-4 font-medium">&larr; Volver</button>
        <h2 id="routine-name" class="text-2xl font-bold mb-4"></h2>
        <div id="exercises-list" class="space-y-3">
            </div>
    </div>

    <div id="exercise-view" class="view p-4 text-center">
        <button id="back-to-details" class="text-indigo-600 mb-6 font-medium absolute top-4 left-4">&larr; Volver</button>
        
        <h3 id="current-routine-name" class="text-lg text-gray-500 mt-10"></h3>
        <p class="text-4xl font-extrabold my-8 text-indigo-700">
            <span id="current-set">1</span> / <span id="total-sets">3</span>
        </p>

        <h2 id="current-exercise-name" class="text-4xl font-bold mb-4 mt-8"></h2>
        
        <div id="exercise-visual" class="mb-4">
            <p class="text-sm text-gray-500 italic">Aquí iría el video/modelo 3D del ejercicio.</p>
        </div>
        
        <p id="current-reps" class="text-6xl font-black mb-10 text-indigo-900"></p>
        
        <p id="rest-timer" class="text-6xl font-black mb-6 text-red-600 hidden">
            00:60
        </p>
        
        <div id="timer-controls" class="mb-8 hidden flex justify-center space-x-4">
            <button id="timer-pause-btn" class="bg-yellow-500 hover:bg-yellow-600 text-white font-bold py-2 px-4 rounded-lg shadow transition">
                ⏸️ Pausar
            </button>
            <button id="timer-skip-btn" class="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded-lg shadow transition">
                ⏭️ Adelantar
            </button>
        </div>


        <button id="next-exercise-btn" class="bg-green-500 hover:bg-green-600 text-white text-xl font-bold py-3 px-8 rounded-full shadow-xl transition transform hover:scale-105 w-full max-w-sm">
            TERMINAR SERIE
        </button>
        
        <div id="exercise-progress" class="mt-8 text-left text-gray-700 max-w-sm mx-auto">
            </div>
    </div>
    
    <div id="calendar-view" class="view p-4">
        <button id="back-to-routines-from-calendar" class="text-indigo-600 mb-4 font-medium">&larr; Volver a Rutinas</button>
        <h2 class="text-2xl font-bold mb-6">📅 Agenda de Entrenamiento</h2>
        
        <div class="flex justify-between items-center mb-4 bg-white p-3 rounded-lg shadow">
            <button id="prev-month" class="text-indigo-600 font-bold p-2 hover:bg-indigo-100 rounded">&lt; Anterior</button>
            <h3 id="current-month-year" class="text-xl font-semibold text-indigo-800"></h3>
            <button id="next-month" class="text-indigo-600 font-bold p-2 hover:bg-indigo-100 rounded">Siguiente &gt;</button>
        </div>

        <div id="calendar-grid-container" class="bg-white p-3 rounded-lg shadow">
            <div id="calendar-days-of-week" class="grid grid-cols-7 text-center font-bold text-gray-600 mb-2">
                <span>Dom</span><span>Lun</span><span>Mar</span><span>Mié</span><span>Jue</span><span>Vie</span><span>Sáb</span>
            </div>
            <div id="calendar-grid" class="grid grid-cols-7 text-center">
                </div>
        </div>

        <h4 class="text-xl font-semibold mt-6 mb-3 text-indigo-700" id="selected-date-display"></h4>
        <div id="daily-appointments" class="space-y-3">
            <p id="no-appointments" class="text-gray-500 italic">Selecciona un día para ver o agendar un entrenamiento.</p>
        </div>
        
        <button id="open-agenda-modal" class="mt-6 bg-indigo-600 hover:bg-indigo-700 text-white text-lg font-bold py-3 px-6 rounded-lg w-full shadow-lg transition">
            + Agendar Entrenamiento
        </button>
        
        <div id="agenda-modal" class="fixed inset-0 bg-gray-600 bg-opacity-75 flex items-center justify-center z-50 hidden">
            <div class="bg-white p-6 rounded-lg shadow-xl w-full max-w-sm">
                <h3 class="text-xl font-bold mb-4">Agendar Rutina</h3>
                <p id="modal-date-display" class="mb-3 text-indigo-600 font-semibold"></p>
                <select id="routine-to-schedule" class="w-full p-2 border border-gray-300 rounded-lg mb-4">
                    </select>
                <div class="flex justify-end space-x-3">
                    <button id="cancel-agenda" class="py-2 px-4 border border-gray-300 rounded-lg hover:bg-gray-100">Cancelar</button>
                    <button id="save-agenda" class="py-2 px-4 bg-green-500 text-white rounded-lg hover:bg-green-600">Guardar</button>
                </div>
            </div>
        </div>
    </div>
    
    <div id="coach-view" class="view p-4">
        <button id="back-to-routines-from-coach" class="text-indigo-600 mb-4 font-medium hidden">&larr; Volver a Rutinas</button>
        <h2 class="text-2xl font-bold mb-6">🔑 MODO COACH: Asignar Rutina</h2>
        
        <div class="bg-white p-4 rounded-lg shadow space-y-4">
            <h3 class="text-xl font-semibold text-indigo-700">Asignar Rutinas</h3>
            
            <label for="coach-routine-select" class="block text-md font-medium text-gray-700">Selecciona la Rutina a Asignar:</label>
            <select id="coach-routine-select" class="w-full p-2 border border-gray-300 rounded-lg text-gray-700">
                </select>

            <button id="assign-routine-btn" class="w-full bg-red-500 hover:bg-red-600 text-white text-lg font-bold py-3 rounded-lg shadow-md transition">
                ASIGNAR RUTINA
            </button>

            <p id="assignment-status" class="text-center pt-2 text-sm text-gray-600"></p>
            
            <button id="create-routine-btn-coach" class="w-full bg-blue-500 hover:bg-blue-600 text-white text-lg font-bold py-3 rounded-lg shadow-md transition mt-4">
                🛠️ Crear/Editar Rutina Personalizada
            </button>

            <button id="add-exercise-to-bank-btn" class="w-full bg-indigo-500 hover:bg-indigo-600 text-white text-lg font-bold py-3 rounded-lg shadow-md transition">
                + Añadir Ejercicio al Banco
            </button>
            
            <h3 class="text-xl font-semibold text-indigo-700 mb-2 pt-4 border-t">🔗 Compartir Rutina de Prueba</h3>
            <div>
                <label for="share-user-name" class="block text-sm font-medium text-gray-700">Nombre del Alumno de Prueba:</label>
                <input type="text" id="share-user-name" placeholder="Ej: Maria Lopez" class="w-full p-2 border border-gray-300 rounded-lg mb-2">
                
                <button id="generate-link-btn" class="w-full bg-indigo-400 hover:bg-indigo-500 text-white text-md font-bold py-2 rounded-lg transition mb-2">
                    Generar Enlace Compartible
                </button>
            </div>
            
            <input type="text" id="share-link-input" readonly placeholder="El enlace aparecerá aquí..." class="w-full p-2 border border-gray-300 rounded-lg bg-gray-100 text-sm hidden">
        </div>
    </div>

    <div id="editor-view" class="view p-4">
        <button id="back-to-coach-from-editor" class="text-indigo-600 mb-4 font-medium">&larr; Volver a Modo Coach</button>
        <h2 class="text-2xl font-bold mb-6" id="editor-title">✏️ Crear Nueva Rutina</h2>
        
        <div class="bg-white p-4 rounded-lg shadow space-y-4">
            <h3 class="text-xl font-semibold text-indigo-700 mb-4">Datos Generales</h3>
            
            <div>
                <label for="routine-name-input" class="block text-md font-medium text-gray-700">Nombre de la Rutina:</label>
                <input type="text" id="routine-name-input" placeholder="Ej: Hipertrofia Fase 1" class="w-full p-2 border border-gray-300 rounded-lg">
            </div>

            <div>
                <label for="routine-days-input" class="block text-md font-medium text-gray-700">Días por Semana:</label>
                <input type="number" id="routine-days-input" min="1" max="7" value="4" class="w-full p-2 border border-gray-300 rounded-lg">
            </div>

            <h3 class="text-xl font-semibold text-indigo-700 mb-4 pt-4 border-t">Selección y Filtro de Ejercicios</h3>

            <div class="grid grid-cols-2 gap-2 mb-4">
                <div>
                    <label class="block text-xs font-medium text-gray-600">Grupo:</label>
                    <select id="filter-group" class="w-full p-2 border rounded-lg text-sm">
                        </select>
                </div>
                <div>
                    <label class="block text-xs font-medium text-gray-600">Equipo:</label>
                    <select id="filter-equipment" class="w-full p-2 border rounded-lg text-sm">
                        </select>
                </div>
            </div>

            <div class="flex space-x-2 items-start">
                <select id="exercise-bank-select" class="w-full p-2 border border-gray-300 rounded-lg text-sm">
                    <option value="">— Selecciona un ejercicio de la lista —</option>
                </select>
            </div>


            <h3 class="text-xl font-semibold text-indigo-700 mb-4 pt-4 border-t">Ejercicios Asignados por Día</h3>
            
            <div id="days-container" class="space-y-6">
                </div>
            
            <button id="save-routine-btn" class="w-full bg-indigo-600 hover:bg-indigo-700 text-white text-lg font-bold py-3 rounded-lg shadow-md transition">
                Guardar Rutina Personalizada
            </button>
            <p id="editor-status" class="text-center pt-2 text-sm text-red-500 hidden">Error: Asegúrate de llenar todos los campos.</p>
        </div>
    </div>

    <div id="add-exercise-modal" class="fixed inset-0 bg-gray-600 bg-opacity-75 flex items-center justify-center z-50 hidden">
        <div class="bg-white p-6 rounded-lg shadow-xl w-full max-w-sm">
            <h3 class="text-xl font-bold mb-4">➕ Añadir Ejercicio al Banco</h3>

            <label class="block text-sm font-medium text-gray-700">Nombre:</label>
            <input type="text" id="new-exercise-name" placeholder="Ej: Press Francés" class="w-full p-2 border rounded-lg mb-3">

            <label class="block text-sm font-medium text-gray-700">Grupo Muscular:</label>
            <select id="new-exercise-group" class="w-full p-2 border rounded-lg mb-3">
                <option value="Pierna">Pierna</option>
                <option value="Tren Superior">Tren Superior</option>
                <option value="Core">Core</option>
                <option value="Aislamiento Brazo">Aislamiento Brazo</option>
                <option value="Funcional">Funcional</option>
            </select>

            <label class="block text-sm font-medium text-gray-700">Nivel (Referencia):</label>
            <select id="new-exercise-level" class="w-full p-2 border rounded-lg mb-3">
                <option value="Baja">Baja</option>
                <option value="Principiante">Principiante</option>
                <option value="Intermedio">Intermedio</option>
                <option value="Avanzado">Avanzado</option>
                <option value="Custom">Custom</option>
            </select>

            <label class="block text-sm font-medium text-gray-700">Equipo:</label>
            <select id="new-exercise-equipment" class="w-full p-2 border rounded-lg mb-3">
                <option value="Cuerpo">Cuerpo (Sin equipo)</option>
                <option value="Casero">Casero (Silla, Toalla, Botella)</option>
                <option value="Pesas">Pesas (Barra, Mancuernas)</option>
                <option value="Máquina">Máquina (Poleas, Prensa)</option>
                <option value="Banda">Banda Elástica</option>
                <option value="Trapos">Trapos</option>
                <option value="Pared">Pared</option>
            </select>
            
            <label class="block text-sm font-medium text-gray-700">Link Video (Opcional):</label>
            <input type="url" id="new-exercise-video" placeholder="URL de YouTube (ej: https://youtu.be/xxx)" class="w-full p-2 border rounded-lg mb-4">


            <div class="flex justify-end space-x-3">
                <button id="cancel-new-exercise" class="py-2 px-4 border border-gray-300 rounded-lg hover:bg-gray-100">Cancelar</button>
                <button id="save-new-exercise" class="py-2 px-4 bg-green-500 text-white rounded-lg hover:bg-green-600">Guardar Ejercicio</button>
            </div>
        </div>
    </div>


    <script>
        // --- 1. DATOS DE MUESTRA: BASE DE DATOS INICIALES (VACÍOS) ---
        const ROUTINES_DATA = []; 

        const INITIAL_EXERCISE_BANK = []; // Se inicializa vacío, el usuario lo llenará
        
        // --- 2. ESTADO GLOBAL ---
        let currentRoutine = null;
        let currentExerciseIndex = 0;
        let currentSet = 1;
        const REST_DURATION_SECONDS = 60; 
        let timerInterval = null; 
        let timerTimeLeft = REST_DURATION_SECONDS; 

        let currentUserName = ''; 
        let currentCoachName = "Coach Pro";
        let assignedRoutineName = localStorage.getItem('assignedRoutine') || ''; 
        let CUSTOM_ROUTINES = []; 
        let EXERCISE_BANK = []; 
        let currentDate = new Date(); 
        let selectedDate = new Date(); 


        // --- 3. FUNCIONES DE PERSISTENCIA (LOCAL STORAGE) ---

        function getUrlParameter(name) {
            name = name.replace(/[\[]/, '\\[').replace(/[\]]/, '\\]');
            const regex = new RegExp('[\\?&]' + name + '=([^&#]*)');
            const results = regex.exec(location.search);
            return results === null ? '' : decodeURIComponent(results[1].replace(/\+/g, ' '));
        }
        
        function loadCustomRoutines() {
            const customRoutinesJSON = localStorage.getItem('CUSTOM_ROUTINES');
            CUSTOM_ROUTINES = customRoutinesJSON ? JSON.parse(customRoutinesJSON) : [];
        }

        function loadExerciseBank() {
            const bankJSON = localStorage.getItem('EXERCISE_BANK');
            if (bankJSON) {
                EXERCISE_BANK = JSON.parse(bankJSON);
            } else {
                // Inicializa el banco como vacío, listo para que el Coach agregue el contenido
                EXERCISE_BANK = INITIAL_EXERCISE_BANK;
                localStorage.setItem('EXERCISE_BANK', JSON.stringify(INITIAL_EXERCISE_BANK));
            }
        }
        
        function saveExerciseToBank(newExercise) {
            // Previene duplicados por nombre
            EXERCISE_BANK = EXERCISE_BANK.filter(ej => ej.nombre !== newExercise.nombre);
            EXERCISE_BANK.push(newExercise);
            localStorage.setItem('EXERCISE_BANK', JSON.stringify(EXERCISE_BANK));
        }


        function loadTrainingHistory() {
            const historyJSON = localStorage.getItem('trainingHistory');
            return historyJSON ? JSON.parse(historyJSON) : [];
        }
        
        function saveTrainingSession(routineName) {
            const history = loadTrainingHistory();
            const newSession = {
                name: routineName,
                date: new Date().toLocaleDateString('es-ES'),
                id: Date.now()
            };
            history.push(newSession);
            localStorage.setItem('trainingHistory', JSON.stringify(history));
            renderHistorySummary(); 
        }
        
        function renderHistorySummary() {
            const history = loadTrainingHistory();
            const historyEl = document.getElementById('history-summary');
            
            if (history.length === 0) {
                if (historyEl) historyEl.innerHTML = '✨ ¡Comienza tu primera rutina para ver tu progreso aquí!';
                return;
            }
            
            const lastSession = history[history.length - 1];
            if (historyEl) {
                historyEl.innerHTML = `
                    📈 **Historial:** Has completado **${history.length}** rutinas. 
                    Última sesión: **${lastSession.name}** el ${lastSession.date}.
                `;
            }
        }

        function loadAppointments() {
            const appointmentsJSON = localStorage.getItem('trainingAppointments');
            return appointmentsJSON ? JSON.parse(appointmentsJSON) : [];
        }

        function saveAppointment(dateKey, routineName) {
            const appointments = loadAppointments();
            
            const existingIndex = appointments.findIndex(app => app.dateKey === dateKey);
            
            const newAppointment = {
                dateKey: dateKey,
                name: routineName,
                time: "Pendiente"
            };
            
            if (existingIndex > -1) {
                appointments[existingIndex] = newAppointment;
            } else {
                appointments.push(newAppointment);
            }
            
            localStorage.setItem('trainingAppointments', JSON.stringify(appointments));
            return appointments;
        }

        function deleteAppointment(dateKey) {
             let appointments = loadAppointments();
             appointments = appointments.filter(app => app.dateKey !== dateKey);
             localStorage.setItem('trainingAppointments', JSON.stringify(appointments));
             return appointments;
        }


        // --- 4. FUNCIONES DE NAVEGACIÓN Y RENDERIZADO ---

        function showView(viewId) {
            document.querySelectorAll('.view').forEach(view => {
                view.style.display = 'none';
            });
            document.getElementById(viewId).style.display = 'block';
        }

        function updateUserInfoDisplay() {
            const welcomeEl = document.getElementById('welcome-message');
            const levelEl = document.getElementById('user-level-display');

            if (welcomeEl) welcomeEl.textContent = `Hola, ${currentUserName}`;
            if (levelEl) levelEl.textContent = assignedRoutineName ? assignedRoutineName : 'Sin rutina asignada';
            
            const showCalendarBtn = document.getElementById('show-calendar-btn');
            const urlUserName = getUrlParameter('name');

            if (showCalendarBtn) {
                if (urlUserName) {
                    showCalendarBtn.classList.remove('hidden');
                } else {
                    showCalendarBtn.classList.add('hidden');
                }
            }
        }

        function renderRoutines() {
            renderHistorySummary(); 
            const routinesList = document.getElementById('routines-list');
            if (!routinesList) return;
            routinesList.innerHTML = ''; 
            
            const allRoutines = [...ROUTINES_DATA, ...CUSTOM_ROUTINES];

            // Buscar la rutina asignada
            const assignedRoutine = allRoutines.find(r => r.nombre === assignedRoutineName);

            if (!assignedRoutine) {
                const titleEl = document.getElementById('routine-view-title');
                if (titleEl) titleEl.textContent = `Hola, ${currentUserName} | Sin Rutina Asignada`;

                routinesList.innerHTML = `
                    <div class="p-6 bg-red-100 border-l-4 border-red-500 rounded-lg shadow-md">
                        <h3 class="text-xl font-bold text-red-800 mb-2">⛔ Rutina No Asignada</h3>
                        <p class="text-red-700">Aún no tienes una rutina asignada por tu Coach. Por favor, espera la confirmación.</p>
                    </div>
                `;
                return;
            }

            const titleEl = document.getElementById('routine-view-title');
            if (titleEl) titleEl.textContent = `Hola, ${currentUserName} | Tu Rutina`;
            
            routinesList.innerHTML = `
                <div class="p-4 bg-green-100 rounded-lg mb-4">
                    <h3 class="text-lg font-semibold text-green-800">✅ Tu Rutina Asignada:</h3>
                </div>
            `;
            
            const routine = assignedRoutine;
            const card = document.createElement('div');
            card.className = 'bg-white p-4 rounded-lg shadow cursor-pointer hover:bg-indigo-50 transition border-4 border-indigo-400';
            
            card.addEventListener('click', () => {
                renderRoutineDetails(routine);
                showView('details-view');
            });
            
            routinesList.appendChild(card);
        }

        function renderRoutineDetails(routine) {
            const nameEl = document.getElementById('routine-name');
            if (nameEl) nameEl.textContent = routine.nombre;

            const exercisesList = document.getElementById('exercises-list');
            if (exercisesList) exercisesList.innerHTML = ''; 

            const days = {};
            routine.ejercicios.forEach(ej => {
                if (!days[ej.dia]) {
                    days[ej.dia] = [];
                }
                days[ej.dia].push(ej);
            });

            for (const dia in days) {
                if (exercisesList) exercisesList.innerHTML += `<h4 class="text-xl font-bold mt-4 mb-2 text-indigo-600">Día ${dia}</h4>`;
                
                days[dia].forEach((ejercicio, index) => {
                    const item = document.createElement('div');
                    item.className = 'bg-white p-4 rounded-lg shadow border-l-4 border-indigo-400';
                    item.innerHTML = `
                        <p class="font-bold text-lg">${index + 1}. ${ejercicio.nombre}</p>
                        <p class="text-gray-600">Series: ${ejercicio.series} | Repeticiones: ${ejercicio.repeticiones}</p>
                        <p class="text-xs mt-1">
                            ${ejercicio.video_link ? 
                                `<a href="${ejercicio.video_link}" target="_blank" class="text-blue-500 hover:underline">▶ Ver Video de Ejecución</a>` 
                                : `(Video no disponible)`}
                        </p>
                    `;
                    if (exercisesList) exercisesList.appendChild(item);
                });
            }
            
            const startButton = document.createElement('button');
            startButton.className = 'mt-6 bg-indigo-600 hover:bg-indigo-700 text-white text-lg font-bold py-3 px-6 rounded-lg w-full shadow-lg transition';
            startButton.textContent = '▶ EMPEZAR RUTINA';
            
            startButton.addEventListener('click', () => {
                startRoutine(routine); 
            });
            
            if (exercisesList) exercisesList.appendChild(startButton);
        }

        function renderProfileView() {
            const history = loadTrainingHistory();
            const appointments = loadAppointments();
            
            const profileNameEl = document.getElementById('profile-user-name');
            if (profileNameEl) profileNameEl.textContent = `Perfil de ${currentUserName}`;

            const totalRoutinesEl = document.getElementById('total-routines-completed');
            if (totalRoutinesEl) totalRoutinesEl.textContent = history.length;

            const totalAppointmentsEl = document.getElementById('total-appointments-scheduled');
            if (totalAppointmentsEl) totalAppointmentsEl.textContent = appointments.length;

            const detailedHistoryEl = document.getElementById('detailed-history');
            if (detailedHistoryEl) {
                detailedHistoryEl.innerHTML = '';
                
                if (history.length === 0) {
                    detailedHistoryEl.innerHTML = '<p class="text-gray-500 italic">Aún no hay sesiones registradas.</p>';
                } else {
                    history.reverse().forEach(session => { 
                        detailedHistoryEl.innerHTML += `
                            <div class="bg-gray-50 p-3 rounded-lg border border-gray-200">
                                <p class="font-semibold">${session.name}</p>
                                <p class="text-sm text-gray-500">Fecha: ${session.date}</p>
                            </div>
                        `;
                    });
                }
            }

            showView('profile-view');
        }

        // NUEVAS FUNCIONES PARA EL MODO COACH
        function renderCoachView() {
            const coachNameEl = document.getElementById('coach-user-name');
            if (coachNameEl) coachNameEl.textContent = currentCoachName;
            
            const selectEl = document.getElementById('coach-routine-select');
            if (!selectEl) return; // Salida segura

            selectEl.innerHTML = '<option value="">— Selecciona una Rutina —</option>';

            const allRoutines = [...ROUTINES_DATA, ...CUSTOM_ROUTINES];

            // 1. Añadir Rutinas Personalizadas
            if (CUSTOM_ROUTINES.length > 0) {
                selectEl.innerHTML += `<optgroup label="Tus Rutinas Personalizadas">`;
                CUSTOM_ROUTINES.forEach(routine => {
                    const selected = routine.nombre === assignedRoutineName ? 'selected' : '';
                    selectEl.innerHTML += `<option value="${routine.nombre}" ${selected}>[CUSTOM] ${routine.nombre} (${routine.dias} Días)</option>`;
                });
                selectEl.innerHTML += `</optgroup>`;
            }
            
            // Si no hay rutinas, mostrar un mensaje.
            if (allRoutines.length === 0) {
                selectEl.innerHTML += `<option value="" disabled>No hay rutinas creadas. Usa el editor.</option>`;
            }
            
            // Limpia el input de compartir al renderizar la vista
            const shareInput = document.getElementById('share-link-input');
            if (shareInput) {
                shareInput.value = '';
                shareInput.classList.add('hidden');
            }
        }
        
        // --- FUNCIONES DE EDITOR DE RUTINAS ---

        function setupEditorFilters() {
            const mainGroups = [...new Set(EXERCISE_BANK.map(ej => ej.grupo))].sort();
            const equipment = [...new Set(EXERCISE_BANK.map(ej => ej.equipo))].sort();

            const groupFilter = document.getElementById('filter-group');
            const equipmentFilter = document.getElementById('filter-equipment');

            const levelFilterContainer = document.querySelector('.grid-cols-2 > div:nth-child(2)');
            if (levelFilterContainer && levelFilterContainer.nextElementSibling) {
                levelFilterContainer.nextElementSibling.style.display = 'none';
            }

            if (groupFilter && equipmentFilter) {
                groupFilter.innerHTML = '<option value="">Todos los Grupos</option>' + mainGroups.map(g => `<option value="${g}">${g}</option>`).join('');
                equipmentFilter.innerHTML = '<option value="">Todo el Equipo</option>' + equipment.map(e => `<option value="${e}">${e}</option>`).join('');

                groupFilter.onchange = filterExerciseBank;
                equipmentFilter.onchange = filterExerciseBank;
            
                filterExerciseBank(); 
            }
        }
        
        function filterExerciseBank() {
            const group = document.getElementById('filter-group').value;
            const equipment = document.getElementById('filter-equipment').value;
            const selectEl = document.getElementById('exercise-bank-select');

            if (!selectEl) return;
            
            let filtered = EXERCISE_BANK;
            
            if (group) {
                filtered = filtered.filter(ej => ej.grupo === group);
            }
            
            if (equipment) {
                filtered = filtered.filter(ej => ej.equipo === equipment);
            }
            
            selectEl.innerHTML = '<option value="">— Selecciona un ejercicio de la lista —</option>' + 
                                filtered.map(ej => `<option value="${ej.nombre}">[${ej.grupo} / ${ej.equipo}] ${ej.nombre}</option>`).join('');
        }

        function renderEditor(routine = null) {
            const editorTitleEl = document.getElementById('editor-title');
            if (editorTitleEl) editorTitleEl.textContent = routine ? '✏️ Editar Rutina' : '✏️ Crear Nueva Rutina';
            
            const daysInput = document.getElementById('routine-days-input');
            
            if (daysInput) daysInput.value = routine ? routine.dias : 4; 
            const nameInput = document.getElementById('routine-name-input');
            if (nameInput) nameInput.value = routine ? routine.nombre : '';

            setupEditorFilters(); 
            const daysInputValue = parseInt(daysInput ? daysInput.value : 0) || 1;
            renderDaySelectors(daysInputValue, routine ? routine.ejercicios : null);

            if (daysInput) {
                daysInput.onchange = () => {
                    renderDaySelectors(parseInt(daysInput.value) || 1, null);
                };
            }

            showView('editor-view');
        }

        function renderDaySelectors(numDays, savedExercises = null) {
            const container = document.getElementById('days-container');
            if (!container) return; // Salida defensiva

            container.innerHTML = '';
            
            for (let i = 1; i <= numDays; i++) {
                const dayDiv = document.createElement('div');
                dayDiv.className = 'p-3 border rounded-lg bg-gray-50';
                dayDiv.innerHTML = `<h4 class="font-semibold text-gray-800 mb-2">Día ${i}:</h4>`;
                
                const addButton = document.createElement('button');
                addButton.textContent = `+ Agregar Ejercicio Seleccionado al Día ${i}`;
                addButton.className = 'text-indigo-600 font-medium text-sm hover:text-indigo-800 border p-1 rounded transition';
                dayDiv.appendChild(addButton);
                
                const currentList = document.createElement('div');
                currentList.id = `day-${i}-list`;
                currentList.className = 'mt-3 space-y-2';
                dayDiv.appendChild(currentList);
                
                addButton.onclick = () => {
                    const exerciseSelect = document.getElementById('exercise-bank-select');
                    const selectedName = exerciseSelect ? exerciseSelect.value : '';
                    const selectedExerciseData = EXERCISE_BANK.find(ej => ej.nombre === selectedName);

                    if (selectedName && selectedExerciseData) {
                        addExerciseToDayList(i, selectedName, 3, "10 reps", selectedExerciseData.video_link);
                    } else {
                        alert("Por favor, selecciona un ejercicio del banco antes de añadirlo.");
                    }
                };

                container.appendChild(dayDiv);
            }
            
            if (savedExercises) {
                for (let i = 1; i <= numDays; i++) {
                    const exercisesForDay = savedExercises.filter(ej => ej.dia === i);
                    exercisesForDay.forEach(ej => {
                        const selectedExerciseData = EXERCISE_BANK.find(ex => ex.nombre === ej.nombre);
                        const videoLink = ej.video_link || (selectedExerciseData ? selectedExerciseData.video_link : '');
                        addExerciseToDayList(ej.dia, ej.nombre, ej.series, ej.repeticiones, videoLink);
                    });
                }
            }
        }
        
        function addExerciseToDayList(day, name, defaultSeries = 3, defaultReps = "10 reps", videoLink = '') {
            const list = document.getElementById(`day-${day}-list`);
            if (!list) return;

            const item = document.createElement('div');
            item.className = 'bg-white p-2 border rounded shadow-sm text-sm flex space-x-2 items-center';
            
            const uniqueId = `ej-${day}-${Date.now()}-${Math.random().toString(36).substring(2, 9)}`;

            item.innerHTML = `
                <span class="flex-grow font-bold">${name}</span>
                <input type="number" value="${defaultSeries}" min="1" class="w-12 p-1 border text-center series-input" placeholder="Series" data-day="${day}" data-name="${name}" data-field="series" data-id="${uniqueId}" data-video="${videoLink}">
                <input type="text" value="${defaultReps}" class="w-24 p-1 border text-center reps-input" placeholder="Reps (ej: 10/RPE 8)" data-day="${day}" data-name="${name}" data-field="repeticiones" data-id="${uniqueId}">
                <button type="button" class="text-red-500 hover:text-red-700 remove-exercise" data-id="${uniqueId}">X</button>
            `;
            
            list.appendChild(item);

            item.querySelector('.remove-exercise').onclick = (e) => {
                list.removeChild(item);
            };
        }
        
        function collectAndSaveRoutine() {
            const name = document.getElementById('routine-name-input').value.trim();
            const daysInput = document.getElementById('routine-days-input');
            const days = parseInt(daysInput ? daysInput.value : 0);
            const statusEl = document.getElementById('editor-status');
            
            if (!name || isNaN(days) || days < 1 || days > 7) {
                if (statusEl) statusEl.textContent = 'Error: El nombre y los días (1-7) de la rutina son obligatorios.';
                if (statusEl) statusEl.classList.remove('hidden');
                return;
            }
            
            const exercises = [];
            let allFieldsValid = true;

            document.querySelectorAll('.series-input').forEach(input => {
                const day = parseInt(input.getAttribute('data-day'));
                const series = parseInt(input.value);
                const name = input.getAttribute('data-name');
                const repsInput = document.querySelector(`.reps-input[data-id="${input.getAttribute('data-id')}"]`);
                const repeticiones = repsInput ? repsInput.value.trim() : '';
                const videoLink = input.getAttribute('data-video') || '';

                if (isNaN(series) || series < 1 || !repeticiones) {
                    allFieldsValid = false;
                }
                
                exercises.push({
                    dia: day,
                    nombre: name,
                    series: series,
                    repeticiones: repeticiones,
                    video_link: videoLink
                });
            });
            
            if (!allFieldsValid) {
                if (statusEl) statusEl.textContent = 'Error: Asegúrate de que todos los ejercicios tienen series y repeticiones válidas.';
                if (statusEl) statusEl.classList.remove('hidden');
                return;
            }
            
            const newRoutine = {
                id: Date.now(), 
                nombre: name,
                level: 'CUSTOM', 
                dias: days,
                ejercicios: exercises
            };
            
            CUSTOM_ROUTINES = CUSTOM_ROUTINES.filter(r => r.nombre !== name); 
            CUSTOM_ROUTINES.push(newRoutine);
            localStorage.setItem('CUSTOM_ROUTINES', JSON.stringify(CUSTOM_ROUTINES));
            
            if (statusEl) {
                statusEl.textContent = `✅ Rutina "${name}" guardada con éxito.`;
                statusEl.classList.remove('text-red-500');
                statusEl.classList.add('text-green-500');
                statusEl.classList.remove('hidden');
            }

            setTimeout(() => {
                showView('coach-view');
                renderCoachView(); 
            }, 1500);
        }

        // --- 5. FUNCIONES DE LÓGICA DE ENTRENAMIENTO Y TEMPORIZADOR ---
        
        function startRoutine(routine) {
            currentRoutine = routine;
            const dayOneExercises = routine.ejercicios.filter(ej => ej.dia === 1);
            currentExerciseIndex = 0; 
            currentSet = 1; 
            
            currentRoutine.ejercicios = dayOneExercises; 

            if (timerInterval) clearInterval(timerInterval);
            showRestUI(false); 

            renderCurrentExercise();
            showView('exercise-view');
            
            document.getElementById('timer-pause-btn').onclick = pauseTimer;
            document.getElementById('timer-skip-btn').onclick = skipTimer;
        }
        
        function showRestUI(isResting) {
            const repsEl = document.getElementById('current-reps');
            const timerEl = document.getElementById('rest-timer');
            const nextBtn = document.getElementById('next-exercise-btn');
            const timerControlsEl = document.getElementById('timer-controls');

            if (isResting) {
                if (repsEl) repsEl.classList.add('hidden');
                if (timerEl) timerEl.classList.remove('hidden');
                if (timerControlsEl) timerControlsEl.classList.remove('hidden'); 
                if (nextBtn) {
                    nextBtn.textContent = 'DESCANSANDO...';
                    nextBtn.disabled = true;
                    nextBtn.classList.remove('bg-green-500', 'bg-blue-500', 'hover:bg-green-600', 'hover:bg-blue-600');
                    nextBtn.classList.add('bg-gray-400');
                }
            } else {
                if (repsEl) repsEl.classList.remove('hidden');
                if (timerEl) timerEl.classList.add('hidden');
                if (timerControlsEl) timerControlsEl.classList.add('hidden'); 
                if (nextBtn) {
                    nextBtn.disabled = false;
                    nextBtn.classList.remove('bg-gray-400');
                }
            }
        }
        
        // FUNCIONES DE CONTROL DE TEMPORIZADOR
        function pauseTimer() {
            const pauseBtn = document.getElementById('timer-pause-btn');
            if (timerInterval) {
                clearInterval(timerInterval);
                timerInterval = null;
                if (pauseBtn) pauseBtn.textContent = '▶️ Continuar';
            } else {
                startRestTimer(timerTimeLeft); 
                if (pauseBtn) pauseBtn.textContent = '⏸️ Pausar';
            }
        }

        function skipTimer() {
            if (timerInterval) {
                clearInterval(timerInterval);
                timerInterval = null;
            }
            showRestUI(false);
            renderCurrentExercise(); 
        }

        function startRestTimer(initialTime = REST_DURATION_SECONDS) {
            timerTimeLeft = initialTime; 
            showRestUI(true); 
            const pauseBtn = document.getElementById('timer-pause-btn');
            if (pauseBtn) pauseBtn.textContent = '⏸️ Pausar'; 
            
            const timerEl = document.getElementById('rest-timer');
            if (timerEl) timerEl.textContent = `00:${String(timerTimeLeft).padStart(2, '0')}`;

            timerInterval = setInterval(() => {
                timerTimeLeft--;
                const seconds = String(timerTimeLeft).padStart(2, '0');
                if (timerEl) timerEl.textContent = `00:${seconds}`;

                if (timerTimeLeft <= 0) {
                    clearInterval(timerInterval);
                    timerInterval = null;
                    showRestUI(false); 
                    renderCurrentExercise(); 
                }
            }, 1000); 
        }

        function renderCurrentExercise() {
            const exercise = currentRoutine.ejercicios[currentExerciseIndex];
            
            if (!exercise) {
                saveTrainingSession(currentRoutine.nombre + " (Día completado)"); 
                alert('🎉 ¡Día de entrenamiento completado! El progreso se ha guardado en tu historial.');
                
                checkInitialView(); 
                return;
            }

            const seriesCount = exercise.series;
            const repsText = exercise.repeticiones;
            
            const routineNameEl = document.getElementById('current-routine-name');
            if (routineNameEl) routineNameEl.textContent = `Rutina: ${currentRoutine.nombre} (Día ${exercise.dia})`;

            const exerciseNameEl = document.getElementById('current-exercise-name');
            if (exerciseNameEl) exerciseNameEl.textContent = exercise.nombre;

            const repsEl = document.getElementById('current-reps');
            if (repsEl) repsEl.textContent = repsText; 
            
            // Renderiza el video si existe, sino el placeholder
            const visualEl = document.getElementById('exercise-visual');
            if (visualEl) {
                 if (exercise.video_link) {
                    visualEl.innerHTML = `<a href="${exercise.video_link}" target="_blank" class="text-blue-500 font-semibold hover:underline">▶ Ver Video de Ejecución</a>`;
                 } else {
                    visualEl.innerHTML = `<p class="text-sm text-gray-500 italic">Video no disponible. Concentrarse en la forma.</p>`;
                 }
            }
            
            const totalSetsEl = document.getElementById('total-sets');
            if (totalSetsEl) totalSetsEl.textContent = seriesCount;
            
            const currentSetEl = document.getElementById('current-set');
            if (currentSetEl) currentSetEl.textContent = currentSet;

            const nextBtn = document.getElementById('next-exercise-btn');
            if (nextBtn) {
                nextBtn.classList.remove('bg-green-500', 'hover:bg-green-600', 'bg-blue-500', 'hover:bg-blue-600');

                if (currentSet >= seriesCount) {
                    nextBtn.textContent = 'PASAR AL SIGUIENTE EJERCICIO';
                    nextBtn.classList.add('bg-blue-500', 'hover:bg-blue-600');
                } else {
                    nextBtn.textContent = 'TERMINAR SERIE';
                    nextBtn.classList.add('bg-green-500', 'hover:bg-green-600');
                }
            }
        }
        
        // --- 6. LÓGICA DEL CALENDARIO ---
        
        function checkInitialView() {
            loadCustomRoutines(); 
            loadExerciseBank(); 
            
            const urlUserName = getUrlParameter('name');
            const urlRoutine = getUrlParameter('routine');
            
            if (urlUserName && urlRoutine) {
                // MODO ALUMNO (Activado por Deep Link)
                currentUserName = urlUserName;
                assignedRoutineName = urlRoutine;
                updateUserInfoDisplay();
                
                const profileBtn = document.getElementById('show-profile-btn');
                if (profileBtn) profileBtn.classList.remove('hidden');
                
                const calendarBtn = document.getElementById('show-calendar-btn');
                if (calendarBtn) calendarBtn.classList.remove('hidden');

                renderRoutines(); 
                showView('routines-view');
                return; 
            }

            // MODO COACH (Si no hay Deep Link)
            currentUserName = currentCoachName;
            assignedRoutineName = localStorage.getItem('assignedRoutine') || ''; 
            
            const profileBtn = document.getElementById('show-profile-btn');
            if (profileBtn) profileBtn.classList.add('hidden');
            
            const calendarBtn = document.getElementById('show-calendar-btn');
            if (calendarBtn) calendarBtn.classList.add('hidden');
            
            updateUserInfoDisplay();
            
            renderCoachView(); 
            showView('coach-view');
        }

        document.addEventListener('DOMContentLoaded', checkInitialView);
    </script>
</body>
</html>
