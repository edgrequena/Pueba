<!DOCTYPE html>  
<html lang="es">  
<head>  
    <meta charset="UTF-8">  
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">  
    <title>Habit Tracker Pro | Panel de Control</title>  
    <!-- Tailwind CSS para diseño moderno y responsivo -->  
    <script src="https://cdn.tailwindcss.com"></script>  
    <!-- Chart.js para gráficos interactivos -->  
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>  
    <!-- Iconos Lucide -->  
    <script src="https://unpkg.com/lucide@latest"></script>  
    <style>  
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');  
        body {  
            font-family: 'Inter', sans-serif;  
            background-color: #0f172a;  
            color: #f8fafc;  
            -webkit-tap-highlight-color: transparent;  
        }  
        .glass-card {  
            background: rgba(30, 41, 59, 0.7);  
            backdrop-filter: blur(12px);  
            border: 1px solid rgba(255, 255, 255, 0.08);  
        }  
        .check-btn {  
            transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);  
        }  
        .check-btn:active {  
            transform: scale(0.9);  
        }  
        /* Scrollbar personalizada */  
        ::-webkit-scrollbar { width: 6px; height: 6px; }  
        ::-webkit-scrollbar-track { background: #0f172a; }  
        ::-webkit-scrollbar-thumb { background: #334155; border-radius: 3px; }  
    </style>  
</head>  
<body class="min-h-screen pb-12">  
  
    <!-- Barra Superior -->  
    <header class="border-b border-slate-800 bg-slate-900/80 sticky top-0 z-30 backdrop-blur-md">  
        <div class="max-w-6xl mx-auto px-4 py-4 flex justify-between items-center">  
            <div class="flex items-center gap-2.5">  
                <div class="bg-indigo-600 p-2 rounded-xl shadow-lg shadow-indigo-500/30">  
                    <i data-lucide="flame" class="w-6 h-6 text-white"></i>  
                </div>  
                <div>  
                    <h1 class="text-lg font-bold tracking-tight text-white">HabitPulse</h1>  
                    <p id="currentDateDisplay" class="text-xs text-slate-400"></p>  
                </div>  
            </div>  
            <button onclick="resetData()" class="text-xs text-slate-400 hover:text-rose-400 flex items-center gap-1.5 bg-slate-800/80 px-3 py-2 rounded-lg border border-slate-700 transition">  
                <i data-lucide="rotate-ccw" class="w-3.5 h-3.5"></i> Reiniciar Demo  
            </button>  
        </div>  
    </header>  
  
    <main class="max-w-6xl mx-auto px-4 mt-6 space-y-6">  
  
        <!-- Tarjetas de Resumen (KPIs) -->  
        <section class="grid grid-cols-2 md:grid-cols-4 gap-4">  
            <div class="glass-card p-4 rounded-2xl">  
                <div class="flex justify-between items-start">  
                    <span class="text-xs font-medium text-slate-400 uppercase">Progreso Hoy</span>  
                    <i data-lucide="check-circle-2" class="w-5 h-5 text-emerald-400"></i>  
                </div>  
                <div class="mt-3 flex items-baseline gap-2">  
                    <span id="statToday" class="text-2xl font-bold text-white">0%</span>  
                    <span id="statTodayCount" class="text-xs text-slate-400">(0/0)</span>  
                </div>  
            </div>  
  
            <div class="glass-card p-4 rounded-2xl">  
                <div class="flex justify-between items-start">  
                    <span class="text-xs font-medium text-slate-400 uppercase">Mejor Racha</span>  
                    <i data-lucide="zap" class="w-5 h-5 text-amber-400"></i>  
                </div>  
                <div class="mt-3 flex items-baseline gap-1">  
                    <span id="statBestStreak" class="text-2xl font-bold text-white">0</span>  
                    <span class="text-xs text-slate-400">días seguidos</span>  
                </div>  
            </div>  
  
            <div class="glass-card p-4 rounded-2xl">  
                <div class="flex justify-between items-start">  
                    <span class="text-xs font-medium text-slate-400 uppercase">Efectividad Semanal</span>  
                    <i data-lucide="trending-up" class="w-5 h-5 text-indigo-400"></i>  
                </div>  
                <div class="mt-3 flex items-baseline gap-2">  
                    <span id="statWeeklyAvg" class="text-2xl font-bold text-white">0%</span>  
                    <span class="text-xs text-emerald-400">7 días</span>  
                </div>  
            </div>  
  
            <div class="glass-card p-4 rounded-2xl">  
                <div class="flex justify-between items-start">  
                    <span class="text-xs font-medium text-slate-400 uppercase">Metas Activas</span>  
                    <i data-lucide="target" class="w-5 h-5 text-cyan-400"></i>  
                </div>  
                <div class="mt-3 flex items-baseline gap-2">  
                    <span id="statTotalHabits" class="text-2xl font-bold text-white">0</span>  
                    <span class="text-xs text-slate-400">hábitos</span>  
                </div>  
            </div>  
        </section>  
  
        <!-- Formulario para agregar nuevo hábito -->  
        <section class="glass-card p-4 rounded-2xl">  
            <form id="habitForm" class="flex flex-col sm:flex-row gap-3">  
                <input type="text" id="habitInput" placeholder="Ej. Entrenamiento de fuerza, Estudiar vademécum, Practicar código..." required  
                    class="flex-1 bg-slate-900/90 border border-slate-700 rounded-xl px-4 py-2.5 text-sm text-white placeholder-slate-500 focus:outline-none focus:border-indigo-500">  
                <select id="habitColor" class="bg-slate-900/90 border border-slate-700 rounded-xl px-3 py-2.5 text-sm text-white focus:outline-none focus:border-indigo-500">  
                    <option value="#6366f1">🟣 Índigo</option>  
                    <option value="#10b981">🟢 Esmeralda</option>  
                    <option value="#f59e0b">🟠 Ámbar</option>  
                    <option value="#06b6d4">🔵 Cian</option>  
                    <option value="#f43f5e">🔴 Rosa</option>  
                </select>  
                <button type="submit" class="bg-indigo-600 hover:bg-indigo-500 text-white font-medium text-sm px-5 py-2.5 rounded-xl transition flex items-center justify-center gap-2 shadow-lg shadow-indigo-600/20">  
                    <i data-lucide="plus" class="w-4 h-4"></i> Agregar Meta  
                </button>  
            </form>  
        </section>  
  
        <!-- Panel Principal: Matriz de Hábitos + Gráficos -->  
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">  
              
            <!-- Columna Izquierda/Central: Matriz Semanal -->  
            <div class="lg:col-span-2 glass-card rounded-2xl p-5 overflow-x-auto">  
                <div class="flex justify-between items-center mb-4">  
                    <h2 class="text-base font-semibold text-white flex items-center gap-2">  
                        <i data-lucide="calendar-check" class="w-5 h-5 text-indigo-400"></i>  
                        Registro de los Últimos 7 Días  
                    </h2>  
                    <span class="text-xs text-slate-400">Toca cada círculo para registrar</span>  
                </div>  
  
                <table class="w-full text-left border-collapse min-w-[520px]">  
                    <thead>  
                        <tr id="tableHeaderRow" class="border-b border-slate-800 text-xs text-slate-400 uppercase">  
                            <th class="py-3 px-2 font-medium w-2/5">Hábito / Meta</th>  
                            <!-- Días generados dinámicamente -->  
                        </tr>  
                    </thead>  
                    <tbody id="habitsTableBody" class="divide-y divide-slate-800/60 text-sm">  
                        <!-- Filas generadas dinámicamente -->  
                    </tbody>  
                </table>  
            </div>  
  
            <!-- Columna Derecha: Gráfico de Rendimiento y Rachas -->  
            <div class="glass-card rounded-2xl p-5 flex flex-col justify-between space-y-6">  
                <div>  
                    <h2 class="text-base font-semibold text-white flex items-center gap-2 mb-1">  
                        <i data-lucide="bar-chart-3" class="w-5 h-5 text-emerald-400"></i>  
                        Consistencia Semanal  
                    </h2>  
                    <p class="text-xs text-slate-400 mb-4">Porcentaje de metas completadas por día</p>  
                    <div class="relative h-52 w-full">  
                        <canvas id="weeklyChart"></canvas>  
                    </div>  
                </div>  
  
                <!-- Ranking de Rachas por Hábito -->  
                <div class="border-t border-slate-800 pt-4">  
                    <h3 class="text-xs font-semibold uppercase tracking-wider text-slate-400 mb-3">Rachas Actuales</h3>  
                    <div id="streakBarsContainer" class="space-y-3">  
                        <!-- Barras de racha dinámicas -->  
                    </div>  
                </div>  
            </div>  
  
        </div>  
    </main>  
  
    <script>  
        // Utilidades de Fechas  
        const getLast7Days = () => {  
            const days = [];  
            const dayNames = ['Dom', 'Lun', 'Mar', 'Mié', 'Jue', 'Vie', 'Sáb'];  
            for (let i = 6; i >= 0; i--) {  
                const d = new Date();  
                d.setDate(d.getDate() - i);  
                const iso = d.toISOString().split('T')[0];  
                days.push({  
                    date: iso,  
                    label: dayNames[d.getDay()],  
                    dayNum: d.getDate(),  
                    isToday: i === 0  
                });  
            }  
            return days;  
        };  
  
        const daysList = getLast7Days();  
        const todayStr = daysList[6].date;  
  
        // Datos iniciales por defecto  
        const getDefaultHabits = () => {  
            const d = daysList.map(x => x.date);  
            return [  
                { id: 1, name: 'Entrenamiento de fuerza / Gym', color: '#10b981', history: { [d[1]]: true, [d[2]]: true, [d[4]]: true, [d[5]]: true, [d[6]]: true } },  
                { id: 2, name: 'Estudio técnico y lectura (45m)', color: '#6366f1', history: { [d[0]]: true, [d[2]]: true, [d[3]]: true, [d[5]]: true, [d[6]]: true } },  
                { id: 3, name: 'Práctica de programación / IA', color: '#06b6d4', history: { [d[3]]: true, [d[4]]: true, [d[5]]: true } },  
                { id: 4, name: 'Hidratación (2.5L de agua)', color: '#f59e0b', history: { [d[0]]: true, [d[1]]: true, [d[2]]: true, [d[3]]: true, [d[4]]: true, [d[5]]: true, [d[6]]: true } }  
            ];  
        };  
  
        let habits = JSON.parse(localStorage.getItem('habitPulse_data')) || getDefaultHabits();  
        let chartInstance = null;  
  
        // Guardar en LocalStorage  
        const saveHabits = () => {  
            localStorage.setItem('habitPulse_data', JSON.stringify(habits));  
            renderApp();  
        };  
  
        // Calcular la racha consecutiva de un hábito (hacia atrás desde hoy o ayer)  
        const calculateStreak = (history) => {  
            let streak = 0;  
            let checkDate = new Date();  
              
            // Si hoy no está marcado aún, revisamos desde ayer para no romper la racha visualmente a mitad del día  
            const todayIso = checkDate.toISOString().split('T')[0];  
            if (!history[todayIso]) {  
                checkDate.setDate(checkDate.getDate() - 1);  
            }  
  
            while (true) {  
                const iso = checkDate.toISOString().split('T')[0];  
                if (history[iso]) {  
                    streak++;  
                    checkDate.setDate(checkDate.getDate() - 1);  
                } else {  
                    break;  
                }  
            }  
            return streak;  
        };  
  
        // Alternar día completado  
        window.toggleHabitDay = (habitId, dateStr) => {  
            const habit = habits.find(h => h.id === habitId);  
            if (habit) {  
                habit.history[dateStr] = !habit.history[dateStr];  
                saveHabits();  
            }  
        };  
  
        // Eliminar hábito  
        window.deleteHabit = (habitId) => {  
            habits = habits.filter(h => h.id !== habitId);  
            saveHabits();  
        };  
  
        // Reiniciar datos de demostración  
        window.resetData = () => {  
            localStorage.removeItem('habitPulse_data');  
            habits = getDefaultHabits();  
            renderApp();  
        };  
  
        // Renderizar Cabecera de Tabla  
        const renderTableHeader = () => {  
            const headerRow = document.getElementById('tableHeaderRow');  
            headerRow.innerHTML = `<th class="py-3 px-2 font-medium">Hábito / Meta</th>`;  
              
            daysList.forEach(d => {  
                const highlight = d.isToday ? 'text-indigo-400 font-bold' : 'text-slate-400';  
                headerRow.innerHTML += `  
                    <th class="py-3 px-1.5 text-center ${highlight}">  
                        <div class="text-[11px]">${d.label}</div>  
                        <div class="text-xs mt-0.5">${d.dayNum}</div>  
                    </th>  
                `;  
            });  
            headerRow.innerHTML += `<th class="py-3 px-2 text-center">Racha</th><th class="py-3 px-1"></th>`;  
        };  
  
        // Renderizar Cuerpo de la Tabla y Estadísticas  
        const renderApp = () => {  
            renderTableHeader();  
            const tbody = document.getElementById('habitsTableBody');  
            const streakContainer = document.getElementById('streakBarsContainer');  
            tbody.innerHTML = '';  
            streakContainer.innerHTML = '';  
  
            let completedToday = 0;  
            let bestStreak = 0;  
  
            habits.forEach(habit => {  
                const streak = calculateStreak(habit.history);  
                if (streak > bestStreak) bestStreak = streak;  
                if (habit.history[todayStr]) completedToday++;  
  
                const tr = document.createElement('tr');  
                tr.className = 'hover:bg-slate-800/40 transition group';  
  
                // Nombre del hábito  
                let rowHtml = `  
                    <td class="py-3.5 px-2 font-medium text-slate-200 flex items-center gap-2.5">  
                        <span class="w-2.5 h-2.5 rounded-full flex-shrink-0" style="background-color: ${habit.color}"></span>  
                        <span class="truncate max-w-[180px] sm:max-w-[240px]">${habit.name}</span>  
                    </td>  
                `;  
  
                // Botones de los 7 días  
                daysList.forEach(d => {  
                    const isDone = !!habit.history[d.date];  
                    const bgStyle = isDone   
                        ? `background-color: ${habit.color}; border-color: ${habit.color}; color: white;`   
                        : `background-color: rgba(15, 23, 42, 0.6); border-color: #334155; color: transparent;`;  
  
                    rowHtml += `  
                        <td class="py-3 px-1.5 text-center">  
                            <button onclick="toggleHabitDay(${habit.id}, '${d.date}')"   
                                style="${bgStyle}"  
                                class="check-btn w-7 h-7 rounded-lg border flex items-center justify-center mx-auto shadow-sm">  
                                <svg class="w-4 h-4 stroke-[3]" fill="none" stroke="currentColor" viewBox="0 0 24 24">  
                                    <path stroke-linecap="round" stroke-linejoin="round" d="M5 13l4 4L19 7"></path>  
                                </svg>  
                            </button>  
                        </td>  
                    `;  
                });  
  
                // Racha y botón eliminar  
                rowHtml += `  
                    <td class="py-3 px-2 text-center">  
                        <span class="inline-flex items-center gap-1 px-2 py-0.5 rounded-full text-xs font-semibold bg-slate-800 text-amber-400 border border-slate-700">  
                            🔥 ${streak}d  
                        </span>  
                    </td>  
                    <td class="py-3 px-1 text-right">  
                        <button onclick="deleteHabit(${habit.id})" class="text-slate-600 hover:text-rose-400 p-1 opacity-70 group-hover:opacity-100 transition">  
                            <i data-lucide="trash-2" class="w-4 h-4"></i>  
                        </button>  
                    </td>  
                `;  
  
                tr.innerHTML = rowHtml;  
                tbody.appendChild(tr);  
  
                // Agregar barra de progreso de la semana en el panel lateral  
                const weekCompletedCount = daysList.filter(d => habit.history[d.date]).length;  
                const weekPct = Math.round((weekCompletedCount / 7) * 100);  
                streakContainer.innerHTML += `  
                    <div>  
                        <div class="flex justify-between text-xs mb-1">  
                            <span class="text-slate-300 font-medium truncate pr-2">${habit.name}</span>  
                            <span class="text-slate-400 flex-shrink-0">${weekCompletedCount}/7 días (${weekPct}%)</span>  
                        </div>  
                        <div class="w-full h-2 bg-slate-800 rounded-full overflow-hidden">  
                            <div class="h-full rounded-full transition-all duration-500" style="width: ${weekPct}%; background-color: ${habit.color}"></div>  
                        </div>  
                    </div>  
                `;  
            });  
  
            // Actualizar KPIs Superiores  
            const totalHabits = habits.length;  
            const todayPct = totalHabits > 0 ? Math.round((completedToday / totalHabits) * 100) : 0;  
              
            // Calcular efectividad semanal total  
            let totalPossibleChecks = totalHabits * 7;  
            let totalChecksDone = 0;  
            daysList.forEach(d => {  
                habits.forEach(h => { if (h.history[d.date]) totalChecksDone++; });  
            });  
            const weeklyAvg = totalPossibleChecks > 0 ? Math.round((totalChecksDone / totalPossibleChecks) * 100) : 0;  
  
            document.getElementById('statToday').textContent = `${todayPct}%`;  
            document.getElementById('statTodayCount').textContent = `(${completedToday}/${totalHabits})`;  
            document.getElementById('statBestStreak').textContent = bestStreak;  
            document.getElementById('statWeeklyAvg').textContent = `${weeklyAvg}%`;  
            document.getElementById('statTotalHabits').textContent = totalHabits;  
  
            updateChart();  
            lucide.createIcons();  
        };  
  
        // Actualizar Gráfico Semanal en Chart.js  
        const updateChart = () => {  
            const ctx = document.getElementById('weeklyChart').getContext('2d');  
            const totalHabits = habits.length;  
  
            const labels = daysList.map(d => `${d.label} ${d.dayNum}`);  
            const dataPct = daysList.map(d => {  
                if (totalHabits === 0) return 0;  
                const doneCount = habits.filter(h => h.history[d.date]).length;  
                return Math.round((doneCount / totalHabits) * 100);  
            });  
  
            if (chartInstance) {  
                chartInstance.data.labels = labels;  
                chartInstance.data.datasets[0].data = dataPct;  
                chartInstance.update();  
                return;  
            }  
  
            chartInstance = new Chart(ctx, {  
                type: 'bar',  
                data: {  
                    labels: labels,  
                    datasets: [{  
                        label: '% Completado',  
                        data: dataPct,  
                        backgroundColor: 'rgba(99, 102, 241, 0.75)',  
                        hoverBackgroundColor: '#10b981',  
                        borderRadius: 6,  
                        borderSkipped: false  
                    }]  
                },  
                options: {  
                    responsive: true,  
                    maintainAspectRatio: false,  
                    plugins: {  
                        legend: { display: false },  
                        tooltip: {  
                            callbacks: {  
                                label: (context) => ` ${context.raw}% completado`  
                            }  
                        }  
                    },  
                    scales: {  
                        y: {  
                            beginAtZero: true,  
                            max: 100,  
                            grid: { color: 'rgba(255, 255, 255, 0.05)' },  
                            ticks: { color: '#94a3b8', font: { size: 10 }, callback: v => v + '%' }  
                        },  
                        x: {  
                            grid: { display: false },  
                            ticks: { color: '#94a3b8', font: { size: 11 } }  
                        }  
                    }  
                }  
            });  
        };  
  
        // Manejar creación de nuevo hábito  
        document.getElementById('habitForm').addEventListener('submit', (e) => {  
            e.preventDefault();  
            const input = document.getElementById('habitInput');  
            const color = document.getElementById('habitColor').value;  
            const name = input.value.trim();  
  
            if (name) {  
                habits.push({  
                    id: Date.now(),  
                    name: name,  
                    color: color,  
                    history: {}  
                });  
                input.value = '';  
                saveHabits();  
            }  
        });  
  
        // Mostrar fecha actual en el encabezado  
        const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };  
        document.getElementById('currentDateDisplay').textContent = new Date().toLocaleDateString('es-GT', options);  
  
        // Inicializar aplicación  
        renderApp();  
    </script>  
</body>  
</html>  
