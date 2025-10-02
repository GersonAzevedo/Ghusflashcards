<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flashcards de Estudo</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Adicionada a biblioteca Marked.js para suporte a Markdown -->
    <script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap');
        html, body { height: 100%; }
        body { font-family: 'Inter', sans-serif; }
        .card-container { perspective: 1000px; }
        .flashcard { transform-style: preserve-3d; transition: transform 0.6s; }
        .flashcard.is-flipped { transform: rotateY(180deg); }
        .card-face { backface-visibility: hidden; -webkit-backface-visibility: hidden; }
        .card-back { transform: rotateY(180deg); }
        @import url('https://unpkg.com/@phosphor-icons/web@2.1.1/src/css/icons.css');
        .modal {
            background-color: rgba(0, 0, 0, 0.7);
            z-index: 50;
        }
        /* Animação de erro para o input da senha */
        @keyframes shake {
            10%, 90% { transform: translate3d(-1px, 0, 0); }
            20%, 80% { transform: translate3d(2px, 0, 0); }
            30%, 50%, 70% { transform: translate3d(-4px, 0, 0); }
            40%, 60% { transform: translate3d(4px, 0, 0); }
        }
        .shake {
            animation: shake 0.82s cubic-bezier(.36,.07,.19,.97) both;
        }
        .color-filter-btn.active {
            transform: scale(1.1);
            border-color: #4f46e5;
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
<body class="bg-slate-100 p-4">

    <!-- TELA DE BLOQUEIO (SENHA) -->
    <div id="lock-screen" class="fixed inset-0 bg-slate-200 z-50 flex items-center justify-center p-4">
        <div class="w-full max-w-sm mx-auto text-center">
            <form id="password-form" class="bg-white p-8 rounded-2xl shadow-lg">
                <label for="password-input" class="block text-2xl font-bold text-slate-700 mb-4">Dom?</label>
                <input type="password" id="password-input" class="w-full p-3 text-center border-2 border-slate-300 rounded-lg text-lg focus:ring-indigo-500 focus:border-indigo-500 transition" autofocus>
                <button type="submit" class="w-full mt-4 py-3 px-4 bg-indigo-600 text-white font-bold rounded-lg shadow-md hover:bg-indigo-700 transition-colors">
                    Entrar
                </button>
            </form>
        </div>
    </div>

    <div id="main-container" class="w-full max-w-7xl mx-auto h-full flex flex-col hidden">

        <!-- TELA INICIAL (HOME SCREEN) -->
        <div id="home-screen" class="w-full h-full bg-white rounded-2xl shadow-xl overflow-hidden grid grid-cols-1 md:grid-cols-[minmax(250px,300px)_1fr]">
            <!-- Coluna da Esquerda (Sidebar de Filtros) -->
            <div class="w-full border-b md:border-r border-slate-200 p-4 flex flex-col">
                <div class="space-y-2 mb-4 flex-shrink-0">
                     <button id="stats-button" class="w-full py-2 px-4 bg-slate-100 text-slate-700 font-bold rounded-lg hover:bg-slate-200 transition-colors text-sm flex items-center justify-center space-x-2">
                        <i class="ph-chart-bar-fill text-lg"></i><span>Estatísticas</span>
                    </button>
                    <button id="add-card-button" class="w-full py-2 px-4 bg-green-500 text-white font-bold rounded-lg shadow-md hover:bg-green-600 transition-colors text-sm flex items-center justify-center space-x-2">
                        <i class="ph-plus-circle-fill text-lg"></i><span>Adicionar Card</span>
                    </button>
                </div>
                
                <div class="flex-shrink-0">
                    <h3 class="text-lg font-bold text-indigo-700 mb-3 border-t border-indigo-100 pt-3">Filtros</h3>
                    <input type="text" id="search-input" placeholder="Buscar por nome ou conteúdo..." 
                           class="w-full p-2 mb-3 border border-slate-300 rounded-lg text-sm focus:ring-indigo-500 focus:border-indigo-500">
                </div>

                <div id="filter-controls" class="flex-grow overflow-y-auto pr-2">
                    <!-- Filtros de checkbox serão renderizados aqui -->
                </div>
                 <div class="flex-shrink-0 pt-2 border-t border-slate-200">
                    <button id="clear-filters-button" class="w-full text-center text-sm text-indigo-600 font-semibold hover:underline">Limpar Filtros</button>
                </div>
            </div>
            <!-- Coluna da Direita (Conteúdo Principal) -->
            <div id="home-content" class="flex-grow p-4 md:p-8 overflow-y-auto">
                <div id="global-controls" class="flex justify-between items-center mb-6">
                    <h1 id="home-title" class="text-3xl font-extrabold text-slate-800">Revisão Ativa</h1>
                    <button id="show-log-button-home" class="text-sm font-bold py-2 px-4 bg-indigo-50 text-indigo-700 rounded-lg hover:bg-indigo-100 transition-colors border border-indigo-200">Ver Log →</button>
                </div>
                
                <div id="stats-screen" class="hidden">
                    <!-- Conteúdo das estatísticas será renderizado aqui -->
                </div>
                
                <div id="folder-view-container">
                    <!-- OPÇÕES DE ESTUDO -->
                    <div id="study-options" class="p-4 bg-slate-50 rounded-lg border border-slate-200 mb-6">
                        <h3 class="text-lg font-bold text-slate-700 mb-3">Opções de Estudo</h3>
                        <div class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-4">
                            <div class="flex items-center">
                                <input type="checkbox" id="shuffle-checkbox" class="h-4 w-4 rounded text-indigo-600 focus:ring-indigo-500">
                                <label for="shuffle-checkbox" class="ml-2 block text-sm font-medium text-slate-700">Embaralhar cards (Shuffle)</label>
                            </div>
                            <div class="flex items-center space-x-2">
                                <span class="text-sm font-medium text-slate-700 flex-shrink-0">Filtrar por cor:</span>
                                <div id="color-filter-buttons" class="flex space-x-2 flex-wrap gap-1">
                                    <!-- Color buttons will be generated by JS -->
                                </div>
                            </div>
                        </div>
                    </div>

                    <p id="folder-view-message" class="text-slate-600 mb-6">Selecione filtros ao lado para começar.</p>
                    <div id="folder-view" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4">
                        <!-- Pastas serão renderizadas aqui -->
                    </div>
                </div>
            </div>
        </div>

        <!-- TELA DE ESTUDO (STUDY SCREEN) -->
        <div id="study-screen" class="w-full max-w-xl mx-auto hidden">
            <div class="flex justify-between items-center w-full mb-6">
                <button id="back-to-home-button" class="text-sm font-semibold text-slate-500 hover:text-indigo-600">← Voltar</button>
                <button id="show-log-button-study" class="text-sm font-bold py-2 px-4 bg-indigo-50 text-indigo-700 rounded-lg hover:bg-indigo-100 transition-colors border border-indigo-200">Ver Log →</button>
            </div>

            <div id="study-title" class="text-center text-indigo-600 font-extrabold text-lg mb-2"></div>
            <div id="card-counter" class="text-center text-slate-500 font-semibold mb-4"></div>
            <div class="card-container w-full h-80 mb-6">
                <div id="flashcard" class="flashcard relative w-full h-full cursor-pointer">
                    <div id="card-front" class="card-face absolute w-full h-full flex items-center justify-center p-8 rounded-2xl shadow-xl transition-colors duration-300">
                        <div class="markdown-content text-2xl md:text-3xl font-semibold text-center text-slate-800"></div>
                    </div>
                    <div id="card-back" class="card-face card-back absolute w-full h-full flex items-center justify-center p-8 rounded-2xl shadow-xl transition-colors duration-300">
                        <div class="markdown-content text-xl md:text-2xl text-center text-slate-700"></div>
                    </div>
                </div>
            </div>

            <div class="flex flex-col space-y-3">
                 <button id="flip-button" class="w-full py-3 px-4 bg-indigo-600 text-white font-bold rounded-xl shadow-md hover:bg-indigo-700 transition-colors">Virar Card</button>
                <div class="flex space-x-3">
                    <button id="prev-button" class="w-full py-3 px-4 bg-white text-slate-700 font-bold rounded-xl shadow-md hover:bg-slate-100 transition-colors border border-slate-300">Anterior</button>
                    <button id="next-button" class="w-full py-3 px-4 bg-white text-slate-700 font-bold rounded-xl shadow-md hover:bg-slate-100 transition-colors border border-slate-300">Próximo</button>
                </div>
            </div>
            
            <div class="mt-6 p-4 bg-white rounded-xl shadow-inner border border-slate-200">
                <h3 class="text-sm font-bold text-slate-600 uppercase tracking-wider mb-3 text-center">Marcar Domínio</h3>
                <div id="mastery-controls" class="flex justify-center space-x-4"></div>
            </div>
        </div>
    </div>

    <!-- MODAL DE NOVO CARD -->
    <div id="new-card-modal" class="modal fixed inset-0 flex items-center justify-center hidden">
        <div class="bg-white p-6 rounded-xl shadow-2xl w-full max-w-lg mx-4">
            <div class="flex justify-between items-center mb-4">
                <h3 class="text-lg font-bold text-slate-800">Criar Novo Flashcard</h3>
                <button id="close-new-card-button" class="text-slate-500 hover:text-slate-800 text-2xl leading-none">&times;</button>
            </div>
            <form id="new-card-form" class="space-y-4">
                <div>
                    <label for="new-card-question" class="block text-sm font-medium text-slate-700">Frente (Pergunta)</label>
                    <textarea id="new-card-question" required rows="4" class="mt-1 w-full p-2 border border-slate-300 rounded-md focus:ring-indigo-500 focus:border-indigo-500"></textarea>
                </div>
                <div>
                    <label for="new-card-answer" class="block text-sm font-medium text-slate-700">Verso (Resposta)</label>
                    <textarea id="new-card-answer" required rows="4" class="mt-1 w-full p-2 border border-slate-300 rounded-md focus:ring-indigo-500 focus:border-indigo-500"></textarea>
                </div>
                <div class="flex space-x-4">
                    <div class="flex-1">
                        <label for="new-card-specialty" class="block text-sm font-medium text-slate-700">Especialidade</label>
                        <input type="text" id="new-card-specialty" required class="mt-1 w-full p-2 border border-slate-300 rounded-md focus:ring-indigo-500 focus:border-indigo-500">
                    </div>
                    <div class="flex-1">
                        <label for="new-card-tags" class="block text-sm font-medium text-slate-700">Tags (Separar por vírgula)</label>
                        <input type="text" id="new-card-tags" class="mt-1 w-full p-2 border border-slate-300 rounded-md focus:ring-indigo-500 focus:border-indigo-500">
                    </div>
                </div>
                <div class="pt-2">
                    <button type="submit" class="w-full py-2 px-4 bg-indigo-600 text-white font-bold rounded-lg shadow-md hover:bg-indigo-700 transition-colors">Adicionar e Salvar</button>
                </div>
            </form>
        </div>
    </div>

    <!-- MODAL DE LOG -->
    <div id="log-modal" class="modal fixed inset-0 flex items-center justify-center hidden">
        <div class="bg-white p-6 rounded-xl shadow-2xl w-full max-w-lg mx-4">
            <div class="flex justify-between items-center mb-4">
                <h3 class="text-lg font-bold text-slate-800">Log de Alterações (Sessão Atual)</h3>
                <button id="close-log-button" class="text-slate-500 hover:text-slate-800 text-2xl leading-none">&times;</button>
            </div>
            <textarea id="log-area" readonly class="w-full h-64 p-2 bg-slate-100 text-slate-800 text-xs rounded-md border border-slate-300 focus:outline-none"></textarea>
            <div class="mt-4 flex justify-end">
                <button id="copy-log-button" class="text-sm font-semibold py-2 px-4 bg-indigo-500 text-white rounded-lg hover:bg-indigo-600 transition-colors">Copiar Log</button>
            </div>
        </div>
    </div>

    <script>
        // --- CONSTANTES E CONFIGURAÇÃO ---
        const originalSheetUrl = 'https://docs.google.com/spreadsheets/d/e/2PACX-1vTuKOdGRgn_JIk3jsVorWCXFtAuewmXj75vNb_2okDq9uG2ladBckiWoB8-26mh1SlndcUxIpo1TB-D/pub?gid=0&single=true&output=csv';
        const GOOGLE_SHEET_URL = 'https://corsproxy.io/?' + encodeURIComponent(originalSheetUrl);
        const MASTERY_LEVELS = {'gray': { name: 'Padrão', cardBg: 'bg-white', btnBg: 'bg-slate-300', ring: 'ring-slate-400', hex: '#cbd5e1'}, 'red': { name: 'Revisar', cardBg: 'bg-red-100', btnBg: 'bg-red-400', ring: 'ring-red-400', hex: '#f87171'}, 'orange': { name: 'Difícil', cardBg: 'bg-orange-100', btnBg: 'bg-orange-400', ring: 'ring-orange-400', hex: '#fb923c'}, 'yellow': { name: 'Médio', cardBg: 'bg-yellow-100', btnBg: 'bg-yellow-400', ring: 'ring-yellow-400', hex: '#facc15'}, 'green': { name: 'Fácil', cardBg: 'bg-green-100', btnBg: 'bg-green-400', ring: 'ring-green-400', hex: '#4ade80'}, 'blue': { name: 'Dominado', cardBg: 'bg-blue-100', btnBg: 'bg-blue-400', ring: 'ring-blue-400', hex: '#60a5fa'}};
        const COLOR_ORDER = ['red', 'orange', 'yellow', 'green', 'blue', 'gray'];

        // --- ESTADO DA APLICAÇÃO ---
        let allCards = [], currentDeckCards = [], currentCardIndex = 0, isFlipped = false, currentSessionKey = null;
        let activeFilters = { specialties: [], tags: [] };
        let activeColorFilters = [];
        let currentChart = null;

        // --- SELETORES DE DOM ---
        const dom = {
            lockScreen: document.getElementById('lock-screen'),
            passwordForm: document.getElementById('password-form'),
            passwordInput: document.getElementById('password-input'),
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
            cardBack: document.getElementById('card-back'),
            frontText: document.querySelector('#card-front .markdown-content'),
            backText: document.querySelector('#card-back .markdown-content'),
            cardCounter: document.getElementById('card-counter'),
            masteryControls: document.getElementById('mastery-controls'),
            flipButton: document.getElementById('flip-button'),
            nextButton: document.getElementById('next-button'),
            prevButton: document.getElementById('prev-button'),
            backToHomeButton: document.getElementById('back-to-home-button'),
            logButtonHome: document.getElementById('show-log-button-home'),
            logButtonStudy: document.getElementById('show-log-button-study'),
            logModal: document.getElementById('log-modal'),
            logArea: document.getElementById('log-area'),
            copyLogButton: document.getElementById('copy-log-button'),
            closeLogButton: document.getElementById('close-log-button'),
            addCardButton: document.getElementById('add-card-button'),
            newCardModal: document.getElementById('new-card-modal'),
            closeNewCardButton: document.getElementById('close-new-card-button'),
            newCardForm: document.getElementById('new-card-form'),
            newCardQuestion: document.getElementById('new-card-question'),
            newCardAnswer: document.getElementById('new-card-answer'),
            newCardSpecialty: document.getElementById('new-card-specialty'),
            newCardTags: document.getElementById('new-card-tags'),
            statsButton: document.getElementById('stats-button'),
            shuffleCheckbox: document.getElementById('shuffle-checkbox'),
            colorFilterButtons: document.getElementById('color-filter-buttons'),
        };

        // --- PERSISTÊNCIA & LOG ---
        const getStorageKey = (sessionKey) => `flashcardState_${sessionKey}`;
        const saveSessionState = () => { if (!currentSessionKey) return; localStorage.setItem(getStorageKey(currentSessionKey), JSON.stringify({ cards: currentDeckCards, log: dom.logArea.value })); };
        function addToLog(message) {
            const timestamp = new Date().toLocaleTimeString('pt-BR');
            dom.logArea.value += `[${timestamp}] ${message}\n`;
            dom.logArea.scrollTop = dom.logArea.scrollHeight;
        }
        function logNewCardForSheet(card) {
            const csvLine = [card.id, `"${card.question}"`, `"${card.answer}"`, card.masteryColor, `"${card.specialty}"`, `"${card.tags}"`].join(',');
            addToLog(`NOVO CARD (COPIE P/ PLANILHA):\n${csvLine}\n`);
        }

        // --- CARGA DE DADOS ---
        function parseCsvLine(line) {
            const values = [];
            let current = '';
            let inQuotes = false;
            for (let i = 0; i < line.length; i++) {
                const char = line[i];
                if (char === '"') {
                    if (inQuotes && line[i+1] === '"') {
                        current += '"';
                        i++; // Skip next quote
                    } else {
                        inQuotes = !inQuotes;
                    }
                } else if (char === ',' && !inQuotes) {
                    values.push(current);
                    current = '';
                } else {
                    current += char;
                }
            }
            values.push(current);
            return values;
        }

        async function fetchAndParseSheetData() {
            try {
                const response = await fetch(GOOGLE_SHEET_URL);
                if (!response.ok) throw new Error(`HTTP error! status: ${response.status}`);
                const csvText = await response.text();
                const lines = csvText.trim().split('\n');
                const header = lines.shift().split(',').map(h => h.trim().replace(/"/g, ''));
                
                const colIndexes = { id: header.indexOf('Número'), question: header.indexOf('Frente'), answer: header.indexOf('Verso'), color: header.indexOf('Cor'), specialty: header.indexOf('Especialidade'), tags: header.indexOf('Tags') };
                if (Object.values(colIndexes).some(index => index < 0)) throw new Error("Colunas essenciais não encontradas na planilha.");

                allCards = lines.map(line => {
                    const values = parseCsvLine(line);
                    const getValue = i => values[i] ? values[i].trim() : '';
                    const color = getValue(colIndexes.color).toLowerCase() || 'gray';
                    return { id: parseInt(getValue(colIndexes.id), 10) || 0, question: getValue(colIndexes.question), answer: getValue(colIndexes.answer), specialty: getValue(colIndexes.specialty) || 'Geral', tags: getValue(colIndexes.tags), masteryColor: MASTERY_LEVELS[color] ? color : 'gray' };
                }).filter(card => card.id && card.question);

            } catch (error) {
                console.error('Erro ao carregar dados da planilha:', error);
                dom.homeContent.innerHTML += `<div class="p-4 mt-6 bg-red-100 border-l-4 border-red-500 text-red-800"><p class="font-bold">Erro de Carga:</p><p>${error.message}</p></div>`;
            }
        }

        // --- LÓGICA DE UI E NAVEGAÇÃO ---
        function switchScreen(screenName) {
            dom.homeScreen.classList.toggle('hidden', screenName !== 'home');
            dom.studyScreen.classList.toggle('hidden', screenName !== 'study');
            if (screenName === 'home') renderHomeScreen();
        }
        
        function switchHomeView(viewName) {
            dom.statsScreen.classList.toggle('hidden', viewName !== 'stats');
            dom.folderViewContainer.classList.toggle('hidden', viewName !== 'folders');
            dom.homeTitle.textContent = viewName === 'stats' ? 'Estatísticas de Domínio' : 'Revisão Ativa';
        }

        function createProgressBar(cards) {
            const count = cards.length;
            if (count === 0) return '<div class="flex w-full h-2 bg-slate-200 rounded-full"></div>';
            const colorCounts = cards.reduce((acc, card) => { acc[card.masteryColor] = (acc[card.masteryColor] || 0) + 1; return acc; }, {});
            const colorBarHtml = COLOR_ORDER.map(color => {
                const percentage = ((colorCounts[color] || 0) / count) * 100;
                return percentage > 0 ? `<div style="width:${percentage}%;" class="h-2 ${MASTERY_LEVELS[color].btnBg}" title="${MASTERY_LEVELS[color].name}: ${colorCounts[color]}"></div>` : '';
            }).join('');
            return `<div class="flex w-full overflow-hidden rounded-full mt-2 border border-slate-200">${colorBarHtml}</div>`;
        }

        function renderHomeScreen() {
            dom.folderView.innerHTML = '';
            
            let filteredCards = allCards;
            if (activeFilters.specialties.length > 0) {
                filteredCards = filteredCards.filter(card => activeFilters.specialties.includes(card.specialty));
            }
            if (activeFilters.tags.length > 0) {
                filteredCards = filteredCards.filter(card => {
                    const cardTags = card.tags.split(',').map(t => t.trim());
                    return cardTags.some(t => activeFilters.tags.includes(t));
                });
            }

            const searchText = dom.searchInput.value.toLowerCase();
            if (searchText) {
                filteredCards = filteredCards.filter(card => 
                    card.question.toLowerCase().includes(searchText) ||
                    card.answer.toLowerCase().includes(searchText) ||
                    card.specialty.toLowerCase().includes(searchText) ||
                    card.tags.toLowerCase().includes(searchText)
                );
            }

            const folders = new Map();
            filteredCards.forEach(card => {
                if (!folders.has(card.specialty)) folders.set(card.specialty, []);
                folders.get(card.specialty).push(card);
            });

            dom.folderViewMessage.classList.toggle('hidden', folders.size > 0);

            if (folders.size === 0) {
                dom.folderView.innerHTML = `<div class="col-span-full text-center p-8 text-slate-500">Nenhum resultado encontrado para os filtros aplicados.</div>`;
                return;
            }

            [...folders.entries()].sort((a,b) => a[0].localeCompare(b[0])).forEach(([specialty, cardsInDeck]) => {
                const folder = document.createElement('button');
                folder.className = 'bg-white p-4 rounded-xl shadow-md border border-slate-200 text-left hover:shadow-lg hover:border-indigo-400 transition transform hover:scale-[1.02]';
                folder.onclick = () => startStudySession(specialty, cardsInDeck);
                folder.innerHTML = `
                    <div class="text-indigo-600 text-3xl mb-2"><i class="ph-folder-open-fill"></i></div>
                    <h3 class="text-base font-bold text-slate-800 truncate mb-1">${specialty}</h3>
                    <p class="text-sm text-slate-500">${cardsInDeck.length} Cards</p>
                    ${createProgressBar(cardsInDeck)}`;
                dom.folderView.appendChild(folder);
            });
        }
        
        function renderFilterControls() {
            const specialties = Array.from(new Set(allCards.map(c => c.specialty))).filter(Boolean).sort();
            const tags = Array.from(new Set(allCards.flatMap(c => c.tags.split(',').map(t => t.trim())))).filter(Boolean).sort();
            
            const createFilterSection = (title, items, type) => {
                let itemsHtml = items.map(item => `
                    <label class="flex items-center space-x-2 text-sm text-slate-700 hover:bg-slate-100 p-1 rounded-md">
                        <input type="checkbox" data-type="${type}" value="${item}" class="rounded text-indigo-600 focus:ring-indigo-500">
                        <span>${item}</span>
                    </label>
                `).join('');

                return `
                    <details class="mb-4" open>
                        <summary class="font-bold text-slate-800 cursor-pointer">${title}</summary>
                        <div class="mt-2 space-y-1">${itemsHtml}</div>
                    </details>
                `;
            };
            
            dom.filterControls.innerHTML = createFilterSection('Especialidades', specialties, 'specialties') + createFilterSection('Tags', tags, 'tags');
        }
        
        function renderStatsScreen() {
            dom.statsScreen.innerHTML = `
                <div class="flex flex-col md:flex-row justify-between items-start md:items-center mb-6 gap-4">
                     <button id="back-to-folders-btn" class="text-sm font-semibold text-slate-500 hover:text-indigo-600 flex items-center"><i class="ph-arrow-left mr-1"></i> Voltar para Revisão</button>
                    <div id="stats-chart-type" class="flex space-x-1 bg-slate-100 p-1 rounded-lg">
                        <button data-type="colunas" class="px-3 py-1 text-sm font-semibold rounded-md bg-white text-indigo-700 shadow">Colunas</button>
                        <button data-type="pizza" class="px-3 py-1 text-sm font-semibold rounded-md text-slate-600">Pizza</button>
                        <button data-type="texto" class="px-3 py-1 text-sm font-semibold rounded-md text-slate-600">Texto</button>
                    </div>
                </div>
                 <div id="stats-filter-mode" class="flex space-x-1 bg-slate-100 p-1 rounded-lg mb-6">
                    <button data-mode="geral" class="px-3 py-1 text-sm font-semibold rounded-md bg-white text-indigo-700 shadow">Geral</button>
                    <button data-mode="specialty" class="px-3 py-1 text-sm font-semibold rounded-md text-slate-600">Por Especialidade</button>
                    <button data-mode="tags" class="px-3 py-1 text-sm font-semibold rounded-md text-slate-600">Por Tag</button>
                </div>
                <div id="stats-content" class="space-y-4"></div>
            `;

            document.getElementById('back-to-folders-btn').addEventListener('click', () => switchHomeView('folders'));

            const updateActiveButton = (containerId, activeValue) => {
                document.querySelectorAll(`#${containerId} button`).forEach(btn => {
                    btn.classList.toggle('bg-white', btn.dataset.mode === activeValue || btn.dataset.type === activeValue);
                    btn.classList.toggle('text-indigo-700', btn.dataset.mode === activeValue || btn.dataset.type === activeValue);
                    btn.classList.toggle('shadow', btn.dataset.mode === activeValue || btn.dataset.type === activeValue);
                    btn.classList.toggle('text-slate-600', !(btn.dataset.mode === activeValue || btn.dataset.type === activeValue));
                });
            };

            let currentStatsMode = 'geral';
            let currentChartType = 'colunas';
            let currentTag = null;

            const renderContent = () => {
                const statsContent = document.getElementById('stats-content');
                statsContent.innerHTML = '';
                
                if (currentChart) currentChart.destroy();
                
                const render = (title, cards) => {
                    const container = document.createElement('div');
                    container.className = 'p-4 bg-white rounded-lg border';
                    let titleHtml = `<h3 class="text-lg font-bold mb-1">${title}</h3><p class="text-sm text-slate-500 mb-3">${cards.length} cards</p>`;

                    if (currentChartType === 'texto') {
                        const colorCounts = cards.reduce((acc, card) => { acc[card.masteryColor] = (acc[card.masteryColor] || 0) + 1; return acc; }, {});
                        const textDetails = '<ul class="mt-2 space-y-1 text-sm text-slate-600">' +
                            COLOR_ORDER.map(color => {
                                const count = colorCounts[color] || 0;
                                return count > 0 ? `<li><strong class="font-semibold text-slate-800">${MASTERY_LEVELS[color].name}:</strong> ${count} cards</li>` : '';
                            }).join('') + '</ul>';
                        container.innerHTML = titleHtml + textDetails;

                    } else {
                        container.innerHTML = titleHtml;
                        const canvas = document.createElement('canvas');
                        canvas.height = 100;
                        container.appendChild(canvas);
                        renderChart(canvas, cards);
                    }
                    statsContent.appendChild(container);
                };

                if (currentStatsMode === 'geral') {
                    render('Visão Geral', allCards);
                } else if (currentStatsMode === 'specialty') {
                    Array.from(new Set(allCards.map(c => c.specialty))).sort().forEach(spec => render(spec, allCards.filter(c => c.specialty === spec)));
                } else if (currentStatsMode === 'tags') {
                    const tags = Array.from(new Set(allCards.flatMap(c => c.tags.split(',').map(t => t.trim())))).filter(Boolean).sort();
                    const tagList = document.createElement('div');
                    tagList.className = 'flex flex-wrap gap-2 mb-4';
                    tagList.innerHTML = tags.map(tag => `<button data-tag="${tag}" class="px-3 py-1 text-sm rounded-full border ${currentTag === tag ? 'bg-indigo-600 text-white' : 'bg-white hover:bg-slate-100'}">${tag}</button>`).join('');
                    statsContent.appendChild(tagList);
                    
                    tagList.querySelectorAll('button').forEach(btn => btn.onclick = () => { currentTag = btn.dataset.tag; renderContent(); });

                    if(currentTag) {
                        render(currentTag, allCards.filter(c => c.tags.includes(currentTag)));
                    }
                }
            };
            
            const renderChart = (canvas, cards) => {
                const ctx = canvas.getContext('2d');
                const counts = COLOR_ORDER.map(color => (cards.filter(c => c.masteryColor === color).length));
                
                currentChart = new Chart(ctx, {
                    type: currentChartType === 'pizza' ? 'pie' : 'bar',
                    data: { labels: COLOR_ORDER.map(c => MASTERY_LEVELS[c].name), datasets: [{ label: 'Número de Cards', data: counts, backgroundColor: COLOR_ORDER.map(c => MASTERY_LEVELS[c].hex), borderColor: '#fff', borderWidth: currentChartType === 'pie' ? 2 : 0 }] },
                    options: { responsive: true, plugins: { legend: { display: currentChartType === 'pie' } }, scales: currentChartType === 'bar' ? { y: { beginAtZero: true } } : {} }
                });
            };

            document.getElementById('stats-filter-mode').addEventListener('click', e => { if(e.target.tagName === 'BUTTON') { currentStatsMode = e.target.dataset.mode; currentTag = null; updateActiveButton('stats-filter-mode', currentStatsMode); renderContent(); } });
            document.getElementById('stats-chart-type').addEventListener('click', e => { if(e.target.tagName === 'BUTTON') { currentChartType = e.target.dataset.type; updateActiveButton('stats-chart-type', currentChartType); renderContent(); } });
            
            renderContent();
        }

        function startStudySession(sessionKey, cardsInDeck) {
            let deckToStudy = [...cardsInDeck];

            if (activeColorFilters.length > 0) {
                deckToStudy = deckToStudy.filter(card => activeColorFilters.includes(card.masteryColor));
            }

            if (deckToStudy.length === 0) {
                alert('Nenhum card encontrado com os filtros de cor selecionados para este deck.');
                return;
            }

            if (dom.shuffleCheckbox.checked) {
                deckToStudy.sort(() => Math.random() - 0.5);
            }
            
            currentSessionKey = sessionKey;
            const savedStateJSON = localStorage.getItem(getStorageKey(currentSessionKey));
            
            if (savedStateJSON) {
                const savedState = JSON.parse(savedStateJSON);
                const savedColors = new Map(savedState.cards.map(c => [c.id, c.masteryColor]));
                
                cardsInDeck.forEach(originalCard => {
                    if(savedColors.has(originalCard.id)){
                         originalCard.masteryColor = savedColors.get(originalCard.id);
                    }
                });

                dom.logArea.value = savedState.log || '';
                addToLog(`Sessão retomada para: ${sessionKey}.`);
            } else {
                dom.logArea.value = '';
                addToLog(`Nova sessão iniciada para: ${sessionKey} (${deckToStudy.length} cards).`);
            }

            currentDeckCards = deckToStudy;
            currentCardIndex = 0;
            isFlipped = false;
            dom.studyTitle.textContent = sessionKey;
            renderCard();
            switchScreen('study');
        }

        // --- LÓGICA DO CARD DE ESTUDO ---
        function setMasteryColor(color) {
            const card = currentDeckCards[currentCardIndex];
            if (card && card.masteryColor !== color) {
                card.masteryColor = color;
                addToLog(`Card ${card.id} - Cor alterada para ${color}.`);
                updateCardAppearance();
                saveSessionState();
            }
        }
        
        function renderMasteryButtons() {
            dom.masteryControls.innerHTML = '';
            const card = currentDeckCards[currentCardIndex];
            for (const color in MASTERY_LEVELS) {
                const button = document.createElement('button');
                const isSelected = card && card.masteryColor === color;
                button.className = `w-10 h-10 rounded-full border-2 shadow-lg transition-all transform hover:scale-110 ${MASTERY_LEVELS[color].btnBg} ${isSelected ? `ring-4 ring-offset-2 ${MASTERY_LEVELS[color].ring}` : 'border-white'}`;
                button.onclick = () => setMasteryColor(color);
                dom.masteryControls.appendChild(button);
            }
        }

        function updateCardAppearance() {
            if (currentDeckCards.length === 0) return;
            const masteryInfo = MASTERY_LEVELS[currentDeckCards[currentCardIndex].masteryColor];
            renderMasteryButtons();
            
            const allBgClasses = Object.values(MASTERY_LEVELS).map(l => l.cardBg).concat(['bg-slate-50', 'bg-white']);
            [dom.cardFront, dom.cardBack].forEach(face => face.classList.remove(...allBgClasses));
            dom.cardFront.classList.add(masteryInfo.cardBg);
            dom.cardBack.classList.add(masteryInfo.cardBg);
        }

        function renderCard() {
            if (currentDeckCards.length === 0) {
                dom.frontText.innerHTML = "Nenhum card neste deck."; 
                dom.backText.innerHTML = "Volte e selecione outro."; 
                dom.cardCounter.textContent = '0 de 0'; 
                return;
            }
            const card = currentDeckCards[currentCardIndex];
            dom.frontText.innerHTML = marked.parse(card.question || '');
            dom.backText.innerHTML = marked.parse(card.answer || '');
            dom.cardCounter.textContent = `Card ${currentCardIndex + 1} de ${currentDeckCards.length}`;
            
            if (isFlipped) {
                dom.cardContainer.classList.remove('is-flipped');
                isFlipped = false;
                dom.flipButton.textContent = 'Virar Card';
            }
            updateCardAppearance();
        }

        function flipCard() {
            if (currentDeckCards.length === 0) return;
            isFlipped = !isFlipped;
            dom.cardContainer.classList.toggle('is-flipped');
            dom.flipButton.textContent = isFlipped ? 'Esconder Resposta' : 'Virar Card';
        }

        const nextCard = () => { if (currentDeckCards.length > 0) { currentCardIndex = (currentCardIndex + 1) % currentDeckCards.length; renderCard(); }};
        const prevCard = () => { if (currentDeckCards.length > 0) { currentCardIndex = (currentCardIndex - 1 + currentDeckCards.length) % currentDeckCards.length; renderCard(); }};
        
        function renderColorFilterButtons() {
            dom.colorFilterButtons.innerHTML = COLOR_ORDER.map(color => `
                <button data-color="${color}" class="color-filter-btn w-6 h-6 rounded-full border-2 border-white shadow-sm transition ${MASTERY_LEVELS[color].btnBg}"></button>
            `).join('');
            
            dom.colorFilterButtons.addEventListener('click', e => {
                if(e.target.matches('.color-filter-btn')) {
                    const color = e.target.dataset.color;
                    e.target.classList.toggle('active');
                    
                    if(activeColorFilters.includes(color)) {
                        activeColorFilters = activeColorFilters.filter(c => c !== color);
                    } else {
                        activeColorFilters.push(color);
                    }
                }
            });
        }


        // --- INICIALIZAÇÃO E LISTENERS ---
        function setupEventListeners() {
            dom.flipButton.addEventListener('click', flipCard);
            dom.cardContainer.addEventListener('click', flipCard);
            dom.nextButton.addEventListener('click', nextCard);
            dom.prevButton.addEventListener('click', prevCard);
            dom.backToHomeButton.addEventListener('click', () => { switchScreen('home'); });
            
            dom.searchInput.addEventListener('input', () => renderHomeScreen());

            [dom.logButtonHome, dom.logButtonStudy].forEach(btn => {
                if (btn) btn.addEventListener('click', () => dom.logModal.classList.remove('hidden'));
            });
            dom.closeLogButton.addEventListener('click', () => dom.logModal.classList.add('hidden'));
            dom.copyLogButton.addEventListener('click', () => { dom.logArea.select(); document.execCommand('copy'); });

            dom.addCardButton.addEventListener('click', () => {
                dom.newCardSpecialty.value = '';
                dom.newCardModal.classList.remove('hidden');
            });
            dom.closeNewCardButton.addEventListener('click', () => dom.newCardModal.classList.add('hidden'));
            dom.newCardForm.addEventListener('submit', (e) => {
                e.preventDefault();
                const newCard = {
                    id: (Math.max(0, ...allCards.map(c => c.id)) + 1),
                    question: dom.newCardQuestion.value.trim(),
                    answer: dom.newCardAnswer.value.trim(),
                    specialty: dom.newCardSpecialty.value.trim() || 'Geral',
                    tags: dom.newCardTags.value.trim().split(',').map(t => t.trim()).join(','),
                    masteryColor: 'gray'
                };
                if (!newCard.question || !newCard.answer) return;
                
                allCards.push(newCard);
                logNewCardForSheet(newCard);
                dom.newCardForm.reset();
                dom.newCardModal.classList.add('hidden');
                renderHomeScreen();
                renderFilterControls();
            });
            
            dom.statsButton.addEventListener('click', () => {
                switchHomeView('stats');
                renderStatsScreen();
            });

            dom.filterControls.addEventListener('change', e => {
                if (e.target.type === 'checkbox') {
                    const type = e.target.dataset.type;
                    const value = e.target.value;
                    if (e.target.checked) {
                        activeFilters[type].push(value);
                    } else {
                        activeFilters[type] = activeFilters[type].filter(item => item !== value);
                    }
                    renderHomeScreen();
                }
            });

            dom.clearFiltersButton.addEventListener('click', () => {
                activeFilters = { specialties: [], tags: [] };
                dom.searchInput.value = '';
                document.querySelectorAll('#filter-controls input[type="checkbox"]').forEach(cb => cb.checked = false);
                renderHomeScreen();
            });
        }
        
        function setupLockScreen() {
            dom.passwordForm.addEventListener('submit', (e) => {
                e.preventDefault();
                if (dom.passwordInput.value.trim().toLowerCase() === 'malek') {
                    dom.lockScreen.classList.add('hidden');
                    dom.mainContainer.classList.remove('hidden');
                    initializeApp();
                } else {
                    dom.passwordInput.classList.add('shake');
                    setTimeout(() => dom.passwordInput.classList.remove('shake'), 820);
                }
            });
        }
        
        async function initializeApp() {
            switchScreen('home');
            switchHomeView('folders');
            await fetchAndParseSheetData();
            renderFilterControls();
            renderHomeScreen();
            renderColorFilterButtons();
            setupEventListeners();
        }

        window.onload = setupLockScreen;
    </script>

</body>
</html>

