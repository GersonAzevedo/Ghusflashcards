<!DOCTYPE html>
<html lang="pt-BR" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Estatísticas Avançadas - Flashcards</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap');
        body { font-family: 'Inter', sans-serif; }
        .control-btn.active { background-color: #4f46e5; color: #ffffff; }
        .chart-wrapper { position: relative; }
        .chart-wrapper:hover .download-btn { opacity: 1; }
        .download-btn {
            position: absolute; top: 0.75rem; right: 0.75rem;
            opacity: 0; transition: opacity 0.2s;
            background-color: rgba(30, 41, 59, 0.7);
            border-radius: 9999px; padding: 0.5rem;
            color: #94a3b8; cursor: pointer;
        }
        .download-btn:hover { background-color: #1e293b; color: #e2e8f0; }
    </style>
</head>
<body class="bg-slate-900 text-slate-200 p-4 md:p-8">

    <div class="max-w-7xl mx-auto">
        <div class="flex justify-between items-center mb-8">
            <a href="flashcards.html" class="text-sm font-semibold text-slate-400 hover:text-indigo-400">← Voltar para o App</a>
        </div>

        <div id="loading" class="text-center p-8 rounded-xl">
            <div class="animate-spin inline-block w-8 h-8 border-4 border-t-4 border-indigo-500 border-opacity-25 rounded-full mr-2"></div>
            <p class="text-lg font-medium text-slate-300 mt-4">Carregando estatísticas...</p>
        </div>

        <div id="stats-container" class="hidden space-y-8">
            <div>
                 <h1 class="text-3xl font-extrabold text-slate-200 mb-4">Painel de Desempenho</h1>
                 <div id="kpi-container" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                    <!-- KPIs serão inseridos aqui -->
                 </div>
            </div>

            <div id="focus-chart-container" class="hidden">
                 <div class="flex justify-between items-center mb-2">
                    <h2 class="text-xl font-bold text-slate-200">Análise em Foco</h2>
                    <button id="close-focus-btn" class="text-sm font-semibold text-slate-400 hover:text-indigo-400">&times; Fechar</button>
                </div>
                <div id="focus-chart-wrapper" class="bg-slate-800 p-4 rounded-xl border border-slate-700">
                    <canvas id="focus-chart"></canvas>
                </div>
            </div>

            <!-- CONTROLES DE VISUALIZAÇÃO -->
            <div class="p-4 bg-slate-800 border border-slate-700 rounded-xl space-y-4">
                 <div class="flex flex-col sm:flex-row gap-4">
                    <div class="flex-1">
                        <label class="text-sm font-semibold text-slate-400 mb-2 block">1. Agrupar por:</label>
                        <div id="view-mode-controls" class="flex flex-wrap gap-2">
                            <button data-mode="specialty" class="control-btn flex-1 sm:flex-none justify-center text-sm font-semibold py-2 px-4 bg-slate-700 rounded-lg hover:bg-slate-600 transition-colors">Especialidade</button>
                            <button data-mode="tags" class="control-btn flex-1 sm:flex-none justify-center text-sm font-semibold py-2 px-4 bg-slate-700 rounded-lg hover:bg-slate-600 transition-colors">Tags</button>
                            <button data-mode="color" class="control-btn flex-1 sm:flex-none justify-center text-sm font-semibold py-2 px-4 bg-slate-700 rounded-lg hover:bg-slate-600 transition-colors">Por Cor</button>
                             <button data-mode="all" class="control-btn flex-1 sm:flex-none justify-center text-sm font-semibold py-2 px-4 bg-slate-700 rounded-lg hover:bg-slate-600 transition-colors">Visão Geral</button>
                        </div>
                    </div>
                    <div class="flex-1">
                        <label class="text-sm font-semibold text-slate-400 mb-2 block">2. Formato do Gráfico:</label>
                        <div id="chart-type-controls" class="flex flex-wrap gap-2">
                            <button data-chart="bar" class="control-btn flex-1 sm:flex-none justify-center text-sm font-semibold py-2 px-4 bg-slate-700 rounded-lg hover:bg-slate-600 transition-colors">Barras</button>
                            <button data-chart="pie" class="control-btn flex-1 sm:flex-none justify-center text-sm font-semibold py-2 px-4 bg-slate-700 rounded-lg hover:bg-slate-600 transition-colors">Pizza</button>
                            <button data-chart="radar" class="control-btn flex-1 sm:flex-none justify-center text-sm font-semibold py-2 px-4 bg-slate-700 rounded-lg hover:bg-slate-600 transition-colors">Radar</button>
                        </div>
                    </div>
                </div>
                 <div id="color-selector-container" class="hidden pt-4 border-t border-slate-700">
                     <label class="text-sm font-semibold text-slate-400 mb-2 block">3. Selecione uma cor para analisar:</label>
                     <div id="color-selector" class="flex flex-wrap gap-2"></div>
                </div>
            </div>

             <!-- CONTAINER DOS GRÁFICOS -->
            <div>
                <h2 id="charts-section-title" class="text-xl font-bold text-slate-200 mb-4">Análise Detalhada</h2>
                <div id="charts-container" class="grid grid-cols-1 lg:grid-cols-2 gap-6"></div>
            </div>
             <div>
                <h2 class="text-xl font-bold text-slate-200 mb-4">Atividade Recente</h2>
                <div class="bg-slate-800 p-4 rounded-xl border border-slate-700">
                    <canvas id="activity-chart"></canvas>
                </div>
            </div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, onSnapshot } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
        
        // --- CONFIG & STATE MANAGEMENT ---
        const Config = {
            MASTERY_LEVELS: {
                'gray': { name: 'Padrão', hex: '#475569' }, 'red': { name: 'Revisar', hex: '#dc2626' },
                'orange': { name: 'Difícil', hex: '#ea580c' }, 'yellow': { name: 'Médio', hex: '#ca8a04' },
                'green': { name: 'Fácil', hex: '#16a34a' }, 'blue': { name: 'Dominado', hex: '#2563eb' }
            },
            COLOR_ORDER: ['red', 'orange', 'yellow', 'green', 'blue', 'gray'],
            VALUE_TO_COLOR_MAP: { 0: 'gray', 1: 'red', 2: 'orange', 3: 'yellow', 4: 'green', 5: 'blue' },
            firebase: {
                apiKey: "AIzaSyAJ7fpCQPimWgcJuYiLbCZ5Rf5YUgMVZ2Q",
                authDomain: "ghus-app.firebaseapp.com",
                projectId: "ghus-app",
                storageBucket: "ghus-app.firebasestorage.app",
                messagingSenderId: "1047907929485",
                appId: "1:1047907929485:web:cba5f2850df61d201c1c4",
            }
        };

        const State = {
            allCards: [],
            viewMode: localStorage.getItem('statsViewMode') || 'specialty',
            chartType: localStorage.getItem('statsChartType') || 'bar',
            selectedColor: null,
            activeCharts: [],
            focusChart: null,
            activityChart: null,
        };

        // --- DOM ELEMENTS ---
        const DOM = {
            loading: document.getElementById('loading'),
            statsContainer: document.getElementById('stats-container'),
            kpiContainer: document.getElementById('kpi-container'),
            chartsContainer: document.getElementById('charts-container'),
            chartsSectionTitle: document.getElementById('charts-section-title'),
            viewModeControls: document.getElementById('view-mode-controls'),
            chartTypeControls: document.getElementById('chart-type-controls'),
            colorSelectorContainer: document.getElementById('color-selector-container'),
            colorSelector: document.getElementById('color-selector'),
            focusChartContainer: document.getElementById('focus-chart-container'),
            focusChartWrapper: document.getElementById('focus-chart-wrapper'),
            closeFocusBtn: document.getElementById('close-focus-btn'),
            activityChartCanvas: document.getElementById('activity-chart'),
        };

        // --- FIREBASE SERVICE ---
        const FirebaseService = {
            init() {
                const app = initializeApp(Config.firebase);
                const auth = getAuth(app);
                const db = getFirestore(app);
                return { auth, db };
            },
            async connect(auth, onData) {
                onAuthStateChanged(auth, user => {
                    if (user) {
                        onSnapshot(collection(getFirestore(), 'flashcards'), snapshot => {
                            const cards = snapshot.docs.map(doc => ({
                                ...doc.data(),
                                masteryColor: this.convertMastery(doc.data().masteryColor),
                                createdAt: doc.data().createdAt?.toDate()
                            }));
                            onData(cards);
                        }, err => console.error("Firebase Error:", err));
                    } else { console.error("Authentication failed."); }
                });
                await signInAnonymously(auth).catch(err => console.error("Sign-in failed", err));
            },
            convertMastery(value) {
                if (typeof value === 'string' && Config.MASTERY_LEVELS[value]) return value;
                if (typeof value === 'number' && Config.VALUE_TO_COLOR_MAP[value]) return Config.VALUE_TO_COLOR_MAP[value];
                return 'gray';
            }
        };

        // --- CHART MANAGER ---
        const ChartManager = {
            render(canvas, title, labels, datasets, chartType) {
                const scalesConfig = {};
                if (chartType === 'radar') scalesConfig.r = { pointLabels: { color: '#cbd5e1' }, grid: { color: '#334155' }, angleLines: { color: '#334155' } };
                if (chartType === 'bar') {
                    scalesConfig.y = { beginAtZero: true, ticks: { color: '#94a3b8', stepSize: 1 }, grid: { color: '#334155' } };
                    scalesConfig.x = { ticks: { color: '#94a3b8' }, grid: { color: '#334155' } };
                }
                const config = {
                    type: chartType, data: { labels, datasets },
                    options: {
                        responsive: true, maintainAspectRatio: chartType !== 'bar',
                        plugins: { legend: { display: chartType !== 'bar', labels: { color: '#cbd5e1' } }, title: { display: true, text: title, color: '#e2e8f0', font: { size: 16 } } },
                        scales: Object.keys(scalesConfig).length > 0 ? scalesConfig : {}
                    }
                };
                return new Chart(canvas, config);
            },
            download(canvas) {
                const link = document.createElement('a');
                link.href = canvas.toDataURL('image/png');
                link.download = `chart-${Date.now()}.png`;
                link.click();
            }
        };

        // --- UI MANAGER ---
        const UI = {
            init() {
                this.setupControls();
                DOM.loading.classList.add('hidden');
                DOM.statsContainer.classList.remove('hidden');
            },
            renderAll() {
                this.renderKPIs(State.allCards);
                this.renderActivityChart();
                this.renderMainCharts();
            },
            renderKPIs(cards) {
                const total = cards.length;
                if(total === 0) { DOM.kpiContainer.innerHTML = `<p>Nenhum card encontrado.</p>`; return; }
                const mastered = cards.filter(c => c.masteryColor === 'blue').length;
                const needsReview = cards.filter(c => c.masteryColor === 'red' || c.masteryColor === 'orange').length;
                const kpis = [
                    { label: 'Total de Cards', value: total },
                    { label: 'Domínio', value: `${((mastered / total) * 100).toFixed(0)}%` },
                    { label: 'Revisão Necessária', value: `${((needsReview / total) * 100).toFixed(0)}%` },
                    { label: 'Cards para Revisar', value: needsReview }
                ];
                DOM.kpiContainer.innerHTML = kpis.map(k => `<div class="bg-slate-800 p-4 rounded-lg border border-slate-700"><p class="text-sm text-slate-400">${k.label}</p><p class="text-2xl font-bold text-white">${k.value}</p></div>`).join('');
            },
            renderActivityChart() {
                if (State.activityChart) State.activityChart.destroy();
                const labels = [];
                const data = [];
                for (let i = 6; i >= 0; i--) {
                    const d = new Date();
                    d.setDate(d.getDate() - i);
                    labels.push(d.toLocaleDateString('pt-BR', { day: '2-digit', month: '2-digit' }));
                    const count = State.allCards.filter(c => c.createdAt && c.createdAt.toDateString() === d.toDateString()).length;
                    data.push(count);
                }
                const dataset = [{
                    label: 'Cards Criados', data,
                    fill: true, backgroundColor: 'rgba(79, 70, 229, 0.2)',
                    borderColor: '#4f46e5', tension: 0.3
                }];
                State.activityChart = ChartManager.render(DOM.activityChartCanvas, 'Novos Cards nos Últimos 7 Dias', labels, dataset, 'line');
            },
            renderMainCharts() {
                State.activeCharts.forEach(c => c.destroy());
                State.activeCharts = [];
                DOM.chartsContainer.innerHTML = '';
                DOM.chartsSectionTitle.textContent = 'Análise Detalhada';

                const createChart = (title, cards, getLabels, getDataset, isFocusable = false) => {
                    const wrapper = document.createElement('div');
                    wrapper.className = 'chart-wrapper bg-slate-800 p-4 rounded-xl border border-slate-700 min-w-0';
                    const canvas = document.createElement('canvas');
                    wrapper.appendChild(canvas);
                    const downloadBtn = document.createElement('div');
                    downloadBtn.className = 'download-btn';
                    downloadBtn.innerHTML = `<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" viewBox="0 0 16 16"><path d="M.5 9.9a.5.5 0 0 1 .5.5v2.5a1 1 0 0 0 1 1h12a1 1 0 0 0 1-1v-2.5a.5.5 0 0 1 1 0v2.5a2 2 0 0 1-2 2H2a2 2 0 0 1-2-2v-2.5a.5.5 0 0 1 .5-.5z"/><path d="M7.646 11.854a.5.5 0 0 0 .708 0l3-3a.5.5 0 0 0-.708-.708L8.5 10.293V1.5a.5.5 0 0 0-1 0v8.793L5.354 8.146a.5.5 0 1 0-.708.708l3 3z"/></svg>`;
                    downloadBtn.onclick = () => ChartManager.download(canvas);
                    wrapper.appendChild(downloadBtn);
                    DOM.chartsContainer.appendChild(wrapper);

                    const labels = getLabels(cards);
                    const dataset = getDataset(cards);
                    const chart = ChartManager.render(canvas, title, labels, dataset, State.chartType);
                    State.activeCharts.push(chart);
                    if (isFocusable) wrapper.style.cursor = 'pointer';
                    wrapper.onclick = isFocusable ? () => this.renderFocusChart(title, labels, dataset) : null;
                };

                if (State.viewMode === 'all') {
                    createChart('Visão Geral', State.allCards, () => Config.COLOR_ORDER.map(c => Config.MASTERY_LEVELS[c].name), (cards) => [{ label: 'Cards', data: Config.COLOR_ORDER.map(color => cards.filter(c => c.masteryColor === color).length), backgroundColor: Config.COLOR_ORDER.map(c => Config.MASTERY_LEVELS[c].hex) }]);
                } else if (State.viewMode === 'specialty' || State.viewMode === 'tags') {
                    const items = [...new Set(State.allCards.flatMap(c => State.viewMode === 'tags' ? (c.tags || '').split(',').map(t => t.trim()).filter(Boolean) : (c.specialty || 'Geral')))].sort();
                    items.forEach(item => {
                        const cards = State.allCards.filter(c => State.viewMode === 'tags' ? (c.tags || '').includes(item) : (c.specialty || 'Geral') === item);
                        if (cards.length > 0) createChart(`${item} (${cards.length})`, cards, () => Config.COLOR_ORDER.map(c => Config.MASTERY_LEVELS[c].name), () => [{ label: 'Cards', data: Config.COLOR_ORDER.map(color => cards.filter(c => c.masteryColor === color).length), backgroundColor: Config.COLOR_ORDER.map(c => Config.MASTERY_LEVELS[c].hex) }], true);
                    });
                } else if (State.viewMode === 'color') {
                    if (!State.selectedColor) { DOM.chartsContainer.innerHTML = `<div class="col-span-full text-center p-8 text-slate-400">Selecione uma cor para análise.</div>`; return; }
                    DOM.chartsSectionTitle.textContent = `Análise da Cor: "${Config.MASTERY_LEVELS[State.selectedColor].name}"`;
                    const cardsOfColor = State.allCards.filter(c => c.masteryColor === State.selectedColor);
                    if(cardsOfColor.length === 0) { DOM.chartsContainer.innerHTML = `<div class="col-span-full text-center p-8 text-slate-400">Nenhum card com esta cor.</div>`; return; }
                    
                    const specialtyCounts = cardsOfColor.reduce((acc, card) => { acc[card.specialty || 'Geral'] = (acc[card.specialty || 'Geral'] || 0) + 1; return acc; }, {});
                    const tagCounts = cardsOfColor.flatMap(c => (c.tags || '').split(',').map(t=>t.trim()).filter(Boolean)).reduce((acc, tag) => { acc[tag] = (acc[tag] || 0) + 1; return acc; }, {});

                    if(Object.keys(specialtyCounts).length > 0) createChart('Especialidades', cardsOfColor, () => Object.keys(specialtyCounts).sort((a,b) => specialtyCounts[b]-specialtyCounts[a]), () => [{ label: 'Cards', data: Object.values(specialtyCounts).sort((a,b)=>b-a), backgroundColor: Config.MASTERY_LEVELS[State.selectedColor].hex }]);
                    if(Object.keys(tagCounts).length > 0) createChart('Tags', cardsOfColor, () => Object.keys(tagCounts).sort((a,b) => tagCounts[b]-tagCounts[a]), () => [{ label: 'Cards', data: Object.values(tagCounts).sort((a,b)=>b-a), backgroundColor: Config.MASTERY_LEVELS[State.selectedColor].hex }]);
                }
            },
            renderFocusChart(title, labels, datasets) {
                if (State.focusChart) State.focusChart.destroy();
                DOM.focusChartContainer.classList.remove('hidden');
                const canvas = DOM.focusChartWrapper.querySelector('canvas');
                State.focusChart = ChartManager.render(canvas, title, labels, datasets, State.chartType);
                DOM.focusChartContainer.scrollIntoView({ behavior: 'smooth' });
            },
            setupControls() {
                DOM.viewModeControls.addEventListener('click', e => {
                    if (e.target.tagName !== 'BUTTON') return;
                    State.viewMode = e.target.dataset.mode;
                    localStorage.setItem('statsViewMode', State.viewMode);
                    this.updateActiveButton(DOM.viewModeControls, State.viewMode, 'mode');
                    DOM.colorSelectorContainer.classList.toggle('hidden', State.viewMode !== 'color');
                    this.renderMainCharts();
                });
                DOM.chartTypeControls.addEventListener('click', e => {
                    if (e.target.tagName !== 'BUTTON') return;
                    State.chartType = e.target.dataset.chart;
                    localStorage.setItem('statsChartType', State.chartType);
                    this.updateActiveButton(DOM.chartTypeControls, State.chartType, 'chart');
                    this.renderMainCharts();
                    if(!DOM.focusChartContainer.classList.contains('hidden')) {
                        const chart = State.activeCharts.find(c => c.options.plugins.title.text === State.focusChart.options.plugins.title.text);
                        if(chart) this.renderFocusChart(chart.options.plugins.title.text, chart.data.labels, chart.data.datasets);
                    }
                });
                DOM.colorSelector.innerHTML = Config.COLOR_ORDER.map(c => `<button data-color="${c}" class="control-btn flex items-center gap-2 text-sm font-semibold py-2 px-4 bg-slate-700 rounded-lg hover:bg-slate-600"><span class="w-3 h-3 rounded-full" style="background-color:${Config.MASTERY_LEVELS[c].hex}"></span>${Config.MASTERY_LEVELS[c].name}</button>`).join('');
                DOM.colorSelector.addEventListener('click', e => {
                    const btn = e.target.closest('button');
                    if (!btn) return;
                    State.selectedColor = btn.dataset.color;
                    this.updateActiveButton(DOM.colorSelector, State.selectedColor, 'color');
                    this.renderMainCharts();
                });
                DOM.closeFocusBtn.addEventListener('click', () => DOM.focusChartContainer.classList.add('hidden'));
                
                // Set initial active buttons from state
                this.updateActiveButton(DOM.viewModeControls, State.viewMode, 'mode');
                this.updateActiveButton(DOM.chartTypeControls, State.chartType, 'chart');
                DOM.colorSelectorContainer.classList.toggle('hidden', State.viewMode !== 'color');
            },
            updateActiveButton(container, value, dataAttr) {
                container.querySelectorAll('button').forEach(btn => btn.classList.remove('active'));
                const activeBtn = container.querySelector(`button[data-${dataAttr}="${value}"]`);
                if (activeBtn) activeBtn.classList.add('active');
            }
        };

        // --- APP INITIALIZATION ---
        window.onload = async () => {
            const { auth, db } = FirebaseService.init();
            UI.init();
            await FirebaseService.connect(auth, (cards) => {
                State.allCards = cards;
                UI.renderAll();
            });
        };
    </script>
</body>
</html>

