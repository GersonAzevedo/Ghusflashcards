<!DOCTYPE html>
<html lang="pt-BR" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flashcards de Estudo Avançado</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Inclui Chart.js para as estatísticas -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Adicionada a biblioteca Marked.js para suporte a Markdown -->
    <script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap');
        html, body, #main-container { height: 100%; }
        body { font-family: 'Inter', sans-serif; }
        .card-container { perspective: 1000px; }
        .flashcard { transform-style: preserve-3d; transition: transform 0.6s; }
        .flashcard.is-flipped { transform: rotateY(180deg); }
        .card-face { backface-visibility: hidden; -webkit-backface-visibility: hidden; }
        .card-back { transform: rotateY(180deg); }
        /* Ícones Phosphor Icons */
        @import url('https://unpkg.com/@phosphor-icons/web@2.1.1/src/css/icons.css');
        .modal {
            background-color: rgba(0, 0, 0, 0.7);
            z-index: 50;
        }
        .color-filter-btn.active {
            transform: scale(1.1);
            border-color: #6366f1; /* indigo-500 */
        }
        /* Estilos para o conteúdo gerado pelo Markdown */
        .markdown-content ul {
            list-style-type: disc;
            padding-left: 20px;
            margin-top: 8px;
        }
        .markdown-content li {
            margin-bottom: 4px;
        }
    </style>
</head>
<body class="bg-slate-900 text-slate-200 p-4">

    <div id="main-container" class="w-full max-w-7xl mx-auto h-full flex flex-col">

        <!-- TELA INICIAL (HOME SCREEN) -->
        <div id="home-screen" class="w-full h-full bg-slate-800 rounded-2xl shadow-xl overflow-hidden grid grid-cols-1 md:grid-cols-[minmax(250px,300px)_1fr]">
            <!-- Coluna da Esquerda (Sidebar de Filtros) -->
            <div class="w-full border-b md:border-r border-slate-700 p-4 flex flex-col">
                <p class="text-xs text-slate-500 mb-2">Usuário: <span id="user-id-display" class="font-mono text-xs">...</span></p>
                <div class="space-y-2 mb-4 flex-shrink-0">
                    <button id="stats-button" class="w-full py-2 px-4 bg-slate-700 text-slate-200 font-bold rounded-lg hover:bg-slate-600 transition-colors text-sm flex items-center justify-center space-x-2">
                        <i class="ph-chart-bar-fill text-lg"></i><span id="stats-button-text">Estatísticas</span>
                    </button>
                    <button id="add-card-button" class="w-full py-2 px-4 bg-green-600 text-white font-bold rounded-lg shadow-md hover:bg-green-700 transition-colors text-sm flex items-center justify-center space-x-2">
                        <i class="ph-plus-circle-fill text-lg"></i><span>Adicionar Card</span>
                    </button>
                </div>
                
                <div id="sidebar-filters-container" class="flex-shrink-0">
                    <h3 class="text-lg font-bold text-indigo-400 mb-3 border-t border-indigo-900 pt-3">Filtros</h3>
                    <input type="text" id="search-input" placeholder="Buscar por nome ou conteúdo..." 
                            class="w-full p-2 mb-3 bg-slate-700 border border-slate-600 rounded-lg text-sm text-slate-200 focus:ring-indigo-500 focus:border-indigo-500">
                </div>

                <div id="filter-controls" class="flex-grow overflow-y-auto pr-2">
                    <!-- Filtros de checkbox serão renderizados aqui -->
                    <p class="text-sm text-slate-400">Carregando filtros...</p>
                </div>
                 <div id="clear-filters-container" class="flex-shrink-0 pt-2 border-t border-slate-700">
                    <button id="clear-filters-button" class="w-full text-center text-sm text-indigo-400 font-semibold hover:underline">Limpar Filtros</button>
                </div>
            </div>
            <!-- Coluna da Direita (Conteúdo Principal) -->
            <div id="home-content" class="flex-grow p-4 md:p-8 overflow-y-auto">
                <div id="global-controls" class="flex justify-between items-center mb-6">
                    <h1 id="home-title" class="text-3xl font-extrabold text-slate-200">Revisão Ativa</h1>
                </div>
                
                <div id="stats-screen" class="hidden">
                    <!-- Conteúdo das estatísticas será renderizado aqui -->
                </div>
                
                <div id="folder-view-container">
                        <!-- Loading State for data -->
                        <div id="loading" class="text-center p-8 rounded-xl">
                            <div class="animate-spin inline-block w-6 h-6 border-4 border-t-4 border-indigo-500 border-opacity-25 rounded-full mr-2"></div>
                            <p class="text-lg font-medium text-slate-300">Carregando flashcards do Firestore...</p>
                        </div>

                        <!-- OPÇÕES DE ESTUDO -->
                        <div id="study-options" class="p-4 bg-slate-700/50 rounded-lg border border-slate-700 mb-6 hidden">
                            <h3 class="text-lg font-bold text-slate-300 mb-3">Opções de Estudo</h3>
                            <div class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-4">
                                <div class="flex items-center">
                                    <input type="checkbox" id="shuffle-checkbox" class="h-4 w-4 rounded bg-slate-600 border-slate-500 text-indigo-500 focus:ring-indigo-500">
                                    <label for="shuffle-checkbox" class="ml-2 block text-sm font-medium text-slate-300">Embaralhar cards (Shuffle)</label>
                                </div>
                                <div class="flex items-center space-x-2">
                                    <span class="text-sm font-medium text-slate-300 flex-shrink-0">Filtrar por cor:</span>
                                    <div id="color-filter-buttons" class="flex space-x-2 flex-wrap gap-1">
                                        <!-- Color buttons will be generated by JS -->
                                    </div>
                                </div>
                            </div>
                        </div>

                        <!-- SESSÕES RÁPIDAS -->
                        <div id="quick-sessions" class="p-4 bg-slate-700/50 rounded-lg border border-slate-700 mb-6 hidden">
                            <h3 class="text-lg font-bold text-slate-300 mb-3">Sessões Rápidas</h3>
                            <div class="flex flex-col sm:flex-row gap-4">
                                <button id="review-red-button" class="flex-1 py-2 px-4 bg-red-900/50 text-red-300 font-bold rounded-lg shadow-sm hover:bg-red-900/80 border border-red-800 transition-colors text-sm flex items-center justify-center space-x-2">
                                    <i class="ph-fire-fill text-lg"></i><span>Revisar Cards "Revisar"</span>
                                </button>
                                <button id="review-orange-red-button" class="flex-1 py-2 px-4 bg-orange-900/50 text-orange-300 font-bold rounded-lg shadow-sm hover:bg-orange-900/80 border border-orange-800 transition-colors text-sm flex items-center justify-center space-x-2">
                                    <i class="ph-warning-fill text-lg"></i><span>Revisar "Difíceis" + "Revisar"</span>
                                </button>
                            </div>
                        </div>

                    <p id="folder-view-message" class="text-slate-400 mb-6 hidden">Selecione filtros ao lado para começar.</p>
                    <div id="folder-view" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4">
                        <!-- Pastas serão renderizadas aqui -->
                    </div>
                </div>
            </div>
        </div>

        <!-- TELA DE ESTUDO (STUDY SCREEN) -->
        <div id="study-screen" class="w-full max-w-xl mx-auto hidden flex flex-col">
             <div class="flex justify-between items-center w-full mb-6">
                <button id="back-to-home-button" class="text-sm font-semibold text-slate-400 hover:text-indigo-400">← Voltar</button>
            </div>

            <div id="study-title" class="text-center text-indigo-400 font-extrabold text-lg mb-2"></div>
            <div id="card-counter" class="text-center text-slate-400 font-semibold mb-4"></div>
            <div class="card-container w-full h-80 mb-6">
                <div id="flashcard" class="flashcard relative w-full h-full cursor-pointer">
                    <!-- Card Front -->
                    <div id="card-front" class="card-face absolute w-full h-full flex items-center justify-center p-8 rounded-2xl shadow-xl transition-colors duration-300 bg-slate-800 border border-slate-700">
                        <p id="card-id-display" class="absolute top-3 left-4 text-xs font-mono text-slate-500"></p>
                        <div class="markdown-content text-2xl md:text-3xl font-semibold text-center text-slate-200"></div>
                    </div>
                    <!-- Card Back -->
                    <div id="card-back" class="card-face card-back absolute w-full h-full flex items-center justify-center p-8 rounded-2xl shadow-xl transition-colors duration-300 bg-indigo-600 text-white">
                        <div class="markdown-content text-xl md:text-2xl text-center text-white"></div>
                    </div>
                </div>
            </div>

            <div class="flex flex-col space-y-3">
                 <button id="flip-button" class="w-full py-3 px-4 bg-indigo-600 text-white font-bold rounded-xl shadow-md hover:bg-indigo-700 transition-colors">Virar Card</button>
                <div class="flex space-x-3">
                    <button id="prev-button" class="w-full py-3 px-4 bg-slate-700 text-slate-200 font-bold rounded-xl shadow-md hover:bg-slate-600 transition-colors border border-slate-600">Anterior</button>
                    <button id="next-button" class="w-full py-3 px-4 bg-slate-700 text-slate-200 font-bold rounded-xl shadow-md hover:bg-slate-600 transition-colors border border-slate-600">Próximo</button>
                </div>
            </div>
            
            <div class="mt-6 p-4 bg-slate-800 rounded-xl shadow-inner border border-slate-700">
                <h3 class="text-sm font-bold text-slate-400 uppercase tracking-wider mb-3 text-center">Marcar Domínio</h3>
                <div id="mastery-controls" class="flex justify-center space-x-2 md:space-x-4">
                    <!-- Mastery buttons will be rendered here -->
                </div>
            </div>
        </div>
    </div>

    <!-- MODAL DE NOVO CARD -->
    <div id="new-card-modal" class="modal fixed inset-0 flex items-center justify-center hidden">
        <div class="bg-slate-800 p-6 rounded-xl shadow-2xl w-full max-w-lg mx-4 border border-slate-700">
            <div class="flex justify-between items-center mb-4">
                <h3 class="text-lg font-bold text-slate-200">Criar Novo Flashcard</h3>
                <button id="close-new-card-button" class="text-slate-500 hover:text-slate-300 text-2xl leading-none">&times;</button>
            </div>
            <form id="new-card-form" class="space-y-4">
                <div>
                    <label for="new-card-question" class="block text-sm font-medium text-slate-400">Frente (Pergunta) - Suporta Markdown</label>
                    <textarea id="new-card-question" required rows="4" class="mt-1 w-full p-2 bg-slate-700 border border-slate-600 rounded-md text-slate-200 focus:ring-indigo-500 focus:border-indigo-500"></textarea>
                </div>
                <div>
                    <label for="new-card-answer" class="block text-sm font-medium text-slate-400">Verso (Resposta) - Suporta Markdown</label>
                    <textarea id="new-card-answer" required rows="4" class="mt-1 w-full p-2 bg-slate-700 border border-slate-600 rounded-md text-slate-200 focus:ring-indigo-500 focus:border-indigo-500"></textarea>
                </div>
                <div class="flex space-x-4">
                    <div class="flex-1">
                        <label for="new-card-specialty" class="block text-sm font-medium text-slate-400">Especialidade</label>
                        <input type="text" id="new-card-specialty" required class="mt-1 w-full p-2 bg-slate-700 border border-slate-600 rounded-md text-slate-200 focus:ring-indigo-500 focus:border-indigo-500">
                    </div>
                    <div class="flex-1">
                        <label for="new-card-tags" class="block text-sm font-medium text-slate-400">Tags (Separar por vírgula)</label>
                        <input type="text" id="new-card-tags" class="mt-1 w-full p-2 bg-slate-700 border border-slate-600 rounded-md text-slate-200 focus:ring-indigo-500 focus:border-indigo-500">
                    </div>
                </div>
                <div class="pt-2">
                    <button type="submit" class="w-full py-2 px-4 bg-indigo-600 text-white font-bold rounded-lg shadow-md hover:bg-indigo-700 transition-colors">Adicionar e Salvar</button>
                </div>
                <p id="new-card-message" class="text-center mt-2 text-sm text-green-400 hidden"></p>
            </form>
        </div>
    </div>

    <script type="module">
        // --- FIREBASE IMPORTS ---
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, onSnapshot, addDoc, updateDoc, doc, serverTimestamp, setLogLevel } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // --- FIREBASE CONFIGURATION ---
        const firebaseConfig = {
            apiKey: "AIzaSyAJ7fpCQPimWgcJuYiLbCZ5Rf5YUgMVZ2Q",
            authDomain: "ghus-app.firebaseapp.com",
            projectId: "ghus-app",
            storageBucket: "ghus-app.firebasestorage.app",
            messagingSenderId: "1047907929485",
            appId: "1:1047907929485:web:cba05f2850df61d201c1c4",
            measurementId: "G-2DPP5746VK"
        };
        
        // --- FIREBASE INITIALIZATION ---
        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        let userId = null;

        // --- CONSTANTS AND CONFIGURATION ---
        const MASTERY_LEVELS = {
            'gray': { name: 'Padrão', cardBg: 'bg-slate-800 border-slate-700', text: 'text-slate-200', btnBg: 'bg-slate-600', ring: 'ring-slate-500', hex: '#475569' },
            'red': { name: 'Revisar', cardBg: 'bg-red-900/20 border-red-800', text: 'text-red-300', btnBg: 'bg-red-600', ring: 'ring-red-500', hex: '#dc2626' },
            'orange': { name: 'Difícil', cardBg: 'bg-orange-900/20 border-orange-800', text: 'text-orange-300', btnBg: 'bg-orange-600', ring: 'ring-orange-500', hex: '#ea580c' },
            'yellow': { name: 'Médio', cardBg: 'bg-yellow-900/20 border-yellow-800', text: 'text-yellow-300', btnBg: 'bg-yellow-600', ring: 'ring-yellow-500', hex: '#ca8a04' },
            'green': { name: 'Fácil', cardBg: 'bg-green-900/20 border-green-800', text: 'text-green-300', btnBg: 'bg-green-600', ring: 'ring-green-500', hex: '#16a34a' },
            'blue': { name: 'Dominado', cardBg: 'bg-blue-900/20 border-blue-800', text: 'text-blue-300', btnBg: 'bg-blue-600', ring: 'ring-blue-500', hex: '#2563eb' }
        };
        const COLOR_ORDER = ['red', 'orange', 'yellow', 'green', 'blue', 'gray'];
        const VALUE_TO_COLOR_MAP = { 0: 'gray', 1: 'red', 2: 'orange', 3: 'yellow', 4: 'green', 5: 'blue' };

        // --- APPLICATION STATE ---
        let allCards = [], currentDeckCards = [], currentCardIndex = 0, isFlipped = false;
        let activeFilters = { specialties: [], tags: [] };
        let activeColorFilters = [];
        let currentChart = null;

        // --- DOM SELECTORS ---
        const dom = {
            mainContainer: document.getElementById('main-container'),
            homeScreen: document.getElementById('home-screen'),
            filterControls: document.getElementById('filter-controls'),
            clearFiltersButton: document.getElementById('clear-filters-button'),
            searchInput: document.getElementById('search-input'),
            homeContent: document.getElementById('home-content'),
            folderView: document.getElementById('folder-view'),
            folderViewContainer: document.getElementById('folder-view-container'),
            folderViewMessage: document.getElementById('folder-view-message'),
            statsScreen: document.getElementById('stats-screen'),
            homeTitle: document.getElementById('home-title'),
            studyScreen: document.getElementById('study-screen'),
            studyTitle: document.getElementById('study-title'),
            cardContainer: document.getElementById('flashcard'),
            cardFront: document.getElementById('card-front'),
            cardIdDisplay: document.getElementById('card-id-display'),
            frontText: document.querySelector('#card-front .markdown-content'),
            backText: document.querySelector('#card-back .markdown-content'),
            cardCounter: document.getElementById('card-counter'),
            masteryControls: document.getElementById('mastery-controls'),
            flipButton: document.getElementById('flip-button'),
            nextButton: document.getElementById('next-button'),
            prevButton: document.getElementById('prev-button'),
            backToHomeButton: document.getElementById('back-to-home-button'),
            addCardButton: document.getElementById('add-card-button'),
            newCardModal: document.getElementById('new-card-modal'),
            closeNewCardButton: document.getElementById('close-new-card-button'),
            newCardForm: document.getElementById('new-card-form'),
            newCardQuestion: document.getElementById('new-card-question'),
            newCardAnswer: document.getElementById('new-card-answer'),
            newCardSpecialty: document.getElementById('new-card-specialty'),
            newCardTags: document.getElementById('new-card-tags'),
            newCardMessage: document.getElementById('new-card-message'),
            statsButton: document.getElementById('stats-button'),
            shuffleCheckbox: document.getElementById('shuffle-checkbox'),
            colorFilterButtons: document.getElementById('color-filter-buttons'),
            reviewRedButton: document.getElementById('review-red-button'),
            reviewOrangeRedButton: document.getElementById('review-orange-red-button'),
            sidebarFiltersContainer: document.getElementById('sidebar-filters-container'),
            clearFiltersContainer: document.getElementById('clear-filters-container'),
            loading: document.getElementById('loading'),
            userIdDisplayEl: document.getElementById('user-id-display'),
            studyOptions: document.getElementById('study-options'),
            quickSessions: document.getElementById('quick-sessions')
        };
        
        // --- LOGIC & FUNCTIONS ---
        function convertMasteryValueToString(value) {
            if (typeof value === 'string' && MASTERY_LEVELS[value]) return value;
            if (typeof value === 'number' && VALUE_TO_COLOR_MAP[value]) return VALUE_TO_COLOR_MAP[value];
            return 'gray';
        }

        async function updateCardMasteryInFirestore(docId, colorName) {
            if (!db || !docId || !colorName) return;
            try {
                const cardRef = doc(db, 'flashcards', docId);
                await updateDoc(cardRef, { masteryColor: colorName, updatedAt: serverTimestamp() });
            } catch (error) {
                console.error("Erro ao atualizar domínio no Firestore:", error);
            }
        }
        
        function switchScreen(screenName) {
            dom.homeScreen.classList.toggle('hidden', screenName !== 'home');
            dom.studyScreen.classList.toggle('hidden', screenName !== 'study');
            if (screenName === 'home') renderHomeScreen();
        }
        
        function switchHomeView(viewName) {
            const isStats = viewName === 'stats';
            dom.statsScreen.classList.toggle('hidden', !isStats);
            dom.folderViewContainer.classList.toggle('hidden', isStats);
            dom.homeTitle.textContent = isStats ? 'Estatísticas de Domínio' : 'Revisão Ativa';
            dom.statsButton.querySelector('span').textContent = isStats ? 'Voltar' : 'Estatísticas';
            [dom.sidebarFiltersContainer, dom.filterControls, dom.clearFiltersContainer].forEach(el => el.classList.toggle('hidden', isStats));
            if (isStats) renderStatsScreen();
        }
        
        function createProgressBar(cards) {
            const count = cards.length;
            if (count === 0) return `<div class="flex w-full h-2 bg-slate-700 rounded-full"></div>`;
            const colorCounts = cards.reduce((acc, card) => { acc[card.masteryColor] = (acc[card.masteryColor] || 0) + 1; return acc; }, {});
            const colorBarHtml = COLOR_ORDER.map(color => {
                const percentage = ((colorCounts[color] || 0) / count) * 100;
                return percentage > 0 ? `<div style="width:${percentage}%;" class="h-2 ${MASTERY_LEVELS[color].btnBg}" title="${MASTERY_LEVELS[color].name}: ${colorCounts[color]}"></div>` : '';
            }).join('');
            return `<div class="flex w-full overflow-hidden rounded-full mt-2 border border-slate-700">${colorBarHtml}</div>`;
        }
        
        function renderHomeScreen() {
            dom.folderView.innerHTML = '';
            dom.loading.classList.add('hidden');
            
            let filteredCards = [...allCards];
            if (activeFilters.specialties.length > 0) {
                filteredCards = filteredCards.filter(card => activeFilters.specialties.includes(card.specialty));
            }
            if (activeFilters.tags.length > 0) {
                filteredCards = filteredCards.filter(card => (card.tags || '').split(',').some(t => activeFilters.tags.includes(t.trim())));
            }
            const searchText = dom.searchInput.value.toLowerCase();
            if (searchText) {
                filteredCards = filteredCards.filter(card => ['question', 'answer', 'specialty', 'tags'].some(key => (card[key] || '').toLowerCase().includes(searchText)));
            }

            const folders = filteredCards.reduce((acc, card) => {
                const specialty = card.specialty || 'Geral';
                if (!acc.has(specialty)) acc.set(specialty, []);
                acc.get(specialty).push(card);
                return acc;
            }, new Map());

            const hasCards = allCards.length > 0;
            dom.studyOptions.classList.toggle('hidden', !hasCards);
            dom.quickSessions.classList.toggle('hidden', !hasCards);
            dom.folderViewMessage.classList.toggle('hidden', folders.size > 0 || !hasCards);
            
            if (folders.size === 0 && hasCards) {
                dom.folderView.innerHTML = `<div class="col-span-full text-center p-8 text-slate-400 bg-slate-800/50 rounded-xl border border-dashed border-slate-700">Nenhum resultado encontrado.</div>`;
            } else if (!hasCards) {
                dom.folderViewMessage.textContent = "Nenhum flashcard carregado. Crie seu primeiro card!";
            } else {
                [...folders.entries()].sort((a,b) => a[0].localeCompare(b[0])).forEach(([specialty, cardsInDeck]) => {
                    const folder = document.createElement('button');
                    folder.className = 'bg-slate-800 p-4 rounded-xl shadow-md border border-slate-700 text-left hover:shadow-lg hover:border-indigo-500 transition transform hover:scale-[1.02]';
                    folder.onclick = () => startStudySession(specialty, cardsInDeck);
                    folder.innerHTML = `
                        <div class="text-indigo-400 text-3xl mb-2"><i class="ph-folder-open-fill"></i></div>
                        <h3 class="text-base font-bold text-slate-200 truncate mb-1">${specialty}</h3>
                        <p class="text-sm text-slate-400">${cardsInDeck.length} Cards</p>
                        ${createProgressBar(cardsInDeck)}`;
                    dom.folderView.appendChild(folder);
                });
            }
        }
        
        function renderFilterControls() {
            if (allCards.length === 0) {
                dom.filterControls.innerHTML = '<p class="text-sm text-slate-500">Nenhum filtro disponível.</p>';
                return;
            }
            const specialties = [...new Set(allCards.map(c => c.specialty).filter(Boolean))].sort();
            const tags = [...new Set(allCards.flatMap(c => (c.tags || '').split(',').map(t => t.trim()).filter(Boolean)))].sort();
            
            const createSection = (title, items, type) => items.length === 0 ? '' : `
                <details class="mb-4" open>
                    <summary class="font-bold text-slate-300 cursor-pointer">${title}</summary>
                    <div class="mt-2 space-y-1">${items.map(item => `
                        <label class="flex items-center space-x-2 text-sm text-slate-300 hover:bg-slate-700 p-1 rounded-md cursor-pointer">
                            <input type="checkbox" data-type="${type}" value="${item}" ${activeFilters[type].includes(item) ? 'checked' : ''} class="rounded bg-slate-600 border-slate-500 text-indigo-500 focus:ring-indigo-500">
                            <span>${item}</span>
                        </label>`).join('')}
                    </div>
                </details>`;
            
            dom.filterControls.innerHTML = createSection('Especialidades', specialties, 'specialties') + createSection('Tags', tags, 'tags');
        }
        
        function renderColorFilterButtons() {
            dom.colorFilterButtons.innerHTML = COLOR_ORDER.map(color => `
                <button data-color="${color}" class="color-filter-btn w-6 h-6 rounded-full border-2 border-transparent shadow-md transition-all duration-150 ${MASTERY_LEVELS[color].btnBg} ${activeColorFilters.includes(color) ? 'active ring-4 ' + MASTERY_LEVELS[color].ring : 'hover:scale-105'}" title="${MASTERY_LEVELS[color].name}"></button>
            `).join('');
        }

        function renderStatsScreen() {
            dom.statsScreen.innerHTML = `<button id="back-to-folders-btn" class="text-sm font-semibold text-slate-400 hover:text-indigo-400 flex items-center mb-4"><i class="ph-arrow-left mr-1"></i> Voltar</button><canvas id="stats-chart"></canvas>`;
            document.getElementById('back-to-folders-btn').addEventListener('click', () => switchHomeView('folders'));
            
            if (currentChart) currentChart.destroy();
            const counts = COLOR_ORDER.map(color => allCards.filter(c => c.masteryColor === color).length);
            currentChart = new Chart(document.getElementById('stats-chart').getContext('2d'), {
                type: 'bar',
                data: { labels: COLOR_ORDER.map(c => MASTERY_LEVELS[c].name), datasets: [{ label: 'Cards', data: counts, backgroundColor: COLOR_ORDER.map(c => MASTERY_LEVELS[c].hex) }] },
                options: { responsive: true, plugins: { legend: { display: false } }, scales: { y: { beginAtZero: true, ticks: { color: '#94a3b8' } }, x: { ticks: { color: '#94a3b8' } } } }
            });
        }

        function startStudySession(title, cards, quickSession = null) {
            let deck = quickSession ? allCards.filter(c => quickSession.colors.includes(c.masteryColor)) : [...cards];
            if (activeColorFilters.length > 0 && !quickSession) {
                deck = deck.filter(card => activeColorFilters.includes(card.masteryColor));
            }
            if (deck.length === 0) { alert(`Nenhum card encontrado para "${title}" com os filtros aplicados.`); return; }
            if (dom.shuffleCheckbox.checked) deck.sort(() => Math.random() - 0.5);
            
            currentDeckCards = deck;
            currentCardIndex = 0; 
            isFlipped = false;
            dom.studyTitle.textContent = quickSession ? quickSession.title : title;
            renderCard();
            switchScreen('study');
        }

        function setMasteryColor(color) { 
            const card = currentDeckCards[currentCardIndex];
            if (!card) return;
            card.masteryColor = color; 
            updateCardMasteryInFirestore(card.id, color);
            updateCardAppearance(); 
        }
        
        function renderMasteryButtons() {
            dom.masteryControls.innerHTML = COLOR_ORDER.map(color => `
                <button data-color="${color}" class="mastery-btn flex flex-col items-center py-2 px-3 md:px-4 rounded-xl shadow-md transition-all transform hover:scale-[1.05] border-2 border-transparent ${MASTERY_LEVELS[color].btnBg} text-white font-semibold text-xs">${MASTERY_LEVELS[color].name}</button>
            `).join('');
            dom.masteryControls.querySelectorAll('.mastery-btn').forEach(btn => btn.addEventListener('click', e => setMasteryColor(e.currentTarget.dataset.color)));
        }
        
        function updateCardAppearance() { 
            const card = currentDeckCards[currentCardIndex];
            if (!card) return;
            const level = MASTERY_LEVELS[card.masteryColor] || MASTERY_LEVELS['gray'];
            dom.cardFront.className = `card-face absolute w-full h-full flex items-center justify-center p-8 rounded-2xl shadow-xl transition-colors duration-300 ${level.cardBg}`;
            dom.frontText.className = `markdown-content text-2xl md:text-3xl font-semibold text-center ${level.text}`;
            dom.masteryControls.querySelectorAll('.mastery-btn').forEach(btn => {
                const isSelected = btn.dataset.color === card.masteryColor;
                btn.classList.toggle('ring-4', isSelected);
                btn.classList.toggle('ring-offset-2', isSelected);
                btn.classList.toggle('ring-offset-slate-800', isSelected);
                btn.classList.toggle(level.ring, isSelected);
            });
        }
        
        function renderCard() {
            if (currentDeckCards.length === 0) return;
            const card = currentDeckCards[currentCardIndex];
            dom.frontText.innerHTML = marked.parse(card.question || '');
            dom.backText.innerHTML = marked.parse(card.answer || '');
            dom.cardIdDisplay.textContent = `ID: ${card.id}`;
            dom.cardCounter.textContent = `Card ${currentCardIndex + 1} de ${currentDeckCards.length}`;
            if (isFlipped) { dom.cardContainer.classList.remove('is-flipped'); isFlipped = false; }
            updateCardAppearance();
        }
        
        const flipCard = () => { if(currentDeckCards.length > 0) { isFlipped = !isFlipped; dom.cardContainer.classList.toggle('is-flipped'); }};
        const nextCard = () => { if (currentDeckCards.length > 0) { currentCardIndex = (currentCardIndex + 1) % currentDeckCards.length; renderCard(); } };
        const prevCard = () => { if (currentDeckCards.length > 0) { currentCardIndex = (currentCardIndex - 1 + currentDeckCards.length) % currentDeckCards.length; renderCard(); } };

        function showModalAlert(message, isError = false) {
            const el = dom.newCardMessage;
            el.textContent = message;
            el.classList.remove('hidden', 'text-red-400', 'text-green-400');
            el.classList.add(isError ? 'text-red-400' : 'text-green-400');
            setTimeout(() => el.classList.add('hidden'), 4000);
        }

        function setupEventListeners() {
            dom.flipButton.addEventListener('click', flipCard);
            dom.cardContainer.addEventListener('click', flipCard);
            dom.nextButton.addEventListener('click', nextCard);
            dom.prevButton.addEventListener('click', prevCard);
            dom.backToHomeButton.addEventListener('click', () => switchScreen('home'));
            dom.searchInput.addEventListener('input', renderHomeScreen);
            dom.filterControls.addEventListener('change', e => { 
                if (e.target.type === 'checkbox') {
                    const {type, value, checked} = e.target.dataset;
                    activeFilters[type] = activeFilters[type].filter(i => i !== value);
                    if (checked) activeFilters[type].push(value);
                    renderHomeScreen();
                }
            });
            dom.clearFiltersButton.addEventListener('click', () => { 
                activeFilters = { specialties: [], tags: [] }; 
                dom.searchInput.value = ''; 
                renderFilterControls();
                renderHomeScreen(); 
            });
            dom.statsButton.addEventListener('click', () => switchHomeView(dom.statsScreen.classList.contains('hidden') ? 'stats' : 'folders'));
            dom.addCardButton.addEventListener('click', () => { dom.newCardForm.reset(); dom.newCardMessage.classList.add('hidden'); dom.newCardModal.classList.remove('hidden'); });
            dom.reviewRedButton.addEventListener('click', () => startStudySession(null, null, { colors: ['red'], title: 'Revisão: Cards para Revisar' }));
            dom.reviewOrangeRedButton.addEventListener('click', () => startStudySession(null, null, { colors: ['red', 'orange'], title: 'Revisão: Pontos Fracos' }));
            dom.closeNewCardButton.addEventListener('click', () => dom.newCardModal.classList.add('hidden'));
            dom.newCardForm.addEventListener('submit', async e => { 
                e.preventDefault(); 
                const data = {
                    question: dom.newCardQuestion.value.trim(), answer: dom.newCardAnswer.value.trim(),
                    specialty: dom.newCardSpecialty.value.trim() || 'Geral', tags: dom.newCardTags.value.trim(),
                    masteryColor: 'gray', createdAt: serverTimestamp(), userId: userId
                };
                if (!data.question || !data.answer) { showModalAlert("Pergunta e Resposta são obrigatórios.", true); return; }
                try {
                    await addDoc(collection(db, 'flashcards'), data);
                    dom.newCardForm.reset(); 
                    showModalAlert('Card adicionado com sucesso!');
                } catch (error) { console.error("Erro ao salvar:", error); showModalAlert('Erro ao salvar o card.', true); }
            });
            dom.colorFilterButtons.addEventListener('click', e => {
                const btn = e.target.closest('.color-filter-btn');
                if (btn) {
                    const { color } = btn.dataset;
                    const index = activeColorFilters.indexOf(color);
                    if (index > -1) activeColorFilters.splice(index, 1);
                    else activeColorFilters.push(color);
                    renderColorFilterButtons();
                    renderHomeScreen();
                }
            });
        }
        
        onAuthStateChanged(auth, user => {
            if (user) {
                userId = user.uid;
                dom.userIdDisplayEl.textContent = userId.substring(0, 10) + '...';
                onSnapshot(collection(db, 'flashcards'), snapshot => {
                    allCards = snapshot.docs.map(doc => ({ 
                        id: doc.id, ...doc.data(),
                        masteryColor: convertMasteryValueToString(doc.data().masteryColor || doc.data().masterycolor),
                    })).sort((a, b) => (b.createdAt?.toMillis() || 0) - (a.createdAt?.toMillis() || 0));
                    renderFilterControls();
                    renderHomeScreen();
                }, error => {
                    console.error("Erro:", error);
                    dom.loading.innerHTML = `<p class="text-red-500 font-bold">Erro ao carregar dados. Verifique suas Regras de Segurança do Firestore.</p>`;
                });
            } else {
                dom.userIdDisplayEl.textContent = "Autenticação falhou";
                dom.loading.innerHTML = `<p class="text-red-500 font-bold">Autenticação Falhou.</p>`;
            }
        });

        window.onload = () => {
            signInAnonymously(auth).catch(err => console.error("Anonymous sign-in failed", err));
            switchScreen('home');
            renderColorFilterButtons();
            renderMasteryButtons();
            setupEventListeners();
        };
    </script>
</body>
</html>

