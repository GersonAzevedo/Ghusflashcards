<!DOCTYPE html>
<html lang="pt-BR" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flashcards de Estudo</title>
    <script src="https://cdn.tailwindcss.com"></script>
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
        .markdown-content ul { list-style-type: disc; padding-left: 20px; margin-top: 8px; }
        .markdown-content li { margin-bottom: 4px; }
    </style>
</head>
<body class="bg-slate-900 text-slate-200">

    <div id="main-container" class="w-full max-w-7xl mx-auto h-full flex flex-col p-4 md:p-8">
        <!-- TELA INICIAL (HOME SCREEN) -->
        <div id="home-screen" class="w-full h-full bg-slate-800/50 border border-slate-700 rounded-2xl shadow-xl overflow-hidden grid grid-cols-1 md:grid-cols-[minmax(250px,300px)_1fr]">
            <!-- Coluna da Esquerda (Sidebar de Filtros) -->
            <div class="w-full border-b md:border-r border-slate-700 p-4 flex flex-col bg-slate-800">
                <div class="space-y-2 mb-4 flex-shrink-0">
                    <a href="stats.html" id="stats-button" class="w-full block text-center py-2 px-4 bg-slate-700 text-slate-200 font-bold rounded-lg hover:bg-slate-600 transition-colors text-sm">
                        Estatísticas
                    </a>
                    <button id="add-card-button" class="w-full py-2 px-4 bg-indigo-600 text-white font-bold rounded-lg shadow-md hover:bg-indigo-700 transition-colors text-sm">
                        Adicionar Card
                    </button>
                </div>
                
                <div id="sidebar-filters-container" class="flex-shrink-0">
                    <h3 class="text-lg font-bold text-indigo-400 mb-3 border-t border-slate-700 pt-3">Filtros</h3>
                    <input type="text" id="search-input" placeholder="Buscar no conteúdo..." class="w-full p-2 mb-3 bg-slate-700 border border-slate-600 rounded-lg text-sm focus:ring-indigo-500 focus:border-indigo-500">
                </div>

                <div id="filter-controls" class="flex-grow overflow-y-auto pr-2">
                    <p class="text-sm text-slate-500">Carregando filtros...</p>
                </div>
                 <div class="flex-shrink-0 pt-2 border-t border-slate-700">
                    <button id="clear-filters-button" class="w-full text-center text-sm text-indigo-400 font-semibold hover:underline">Limpar Filtros</button>
                </div>
            </div>
            <!-- Coluna da Direita (Conteúdo Principal) -->
            <div id="home-content" class="flex-grow p-4 md:p-8 overflow-y-auto">
                <h1 id="home-title" class="text-3xl font-extrabold text-slate-200 mb-6">Revisão Ativa</h1>
                
                <div id="loading" class="text-center p-8">
                    <div class="animate-spin inline-block w-6 h-6 border-4 border-t-4 border-indigo-500 border-opacity-25 rounded-full mr-2"></div>
                    <p class="text-lg font-medium text-slate-400">Carregando flashcards...</p>
                </div>

                <p id="folder-view-message" class="text-slate-400 mb-6 hidden">Selecione filtros ao lado para começar.</p>
                <div id="folder-view" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4">
                    <!-- Pastas serão renderizadas aqui -->
                </div>
            </div>
        </div>

        <!-- TELA DE ESTUDO (STUDY SCREEN) -->
        <div id="study-screen" class="w-full max-w-xl mx-auto hidden">
            <div class="flex justify-between items-center w-full mb-6">
                <button id="back-to-home-button" class="text-sm font-semibold text-slate-400 hover:text-indigo-400">← Voltar</button>
                <div class="flex items-center space-x-2">
                    <button id="edit-card-button" title="Editar Card" class="p-2 rounded-full hover:bg-slate-700 transition-colors">
                        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-pencil-square" viewBox="0 0 16 16">
                            <path d="M15.502 1.94a.5.5 0 0 1 0 .706L14.459 3.69l-2-2L13.502.646a.5.5 0 0 1 .707 0l1.293 1.293zm-1.75 2.456-2-2L4.939 9.21a.5.5 0 0 0-.121.196l-.805 2.414a.25.25 0 0 0 .316.316l2.414-.805a.5.5 0 0 0 .196-.12l6.813-6.814z"/>
                            <path fill-rule="evenodd" d="M1 13.5A1.5 1.5 0 0 0 2.5 15h11a1.5 1.5 0 0 0 1.5-1.5v-6a.5.5 0 0 0-1 0v6a.5.5 0 0 1-.5.5h-11a.5.5 0 0 1-.5-.5v-11a.5.5 0 0 1 .5-.5H9a.5.5 0 0 0 0-1H2.5A1.5 1.5 0 0 0 1 2.5v11z"/>
                        </svg>
                    </button>
                    <button id="delete-card-button" title="Excluir Card" class="p-2 rounded-full hover:bg-slate-700 transition-colors text-red-400">
                         <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-trash3-fill" viewBox="0 0 16 16">
                            <path d="M11 1.5v1h3.5a.5.5 0 0 1 0 1h-.538l-.853 10.66A2 2 0 0 1 11.115 16h-6.23a2 2 0 0 1-1.994-1.84L2.038 3.5H1.5a.5.5 0 0 1 0-1H5v-1A1.5 1.5 0 0 1 6.5 0h3A1.5 1.5 0 0 1 11 1.5Zm-5 0v1h4v-1a.5.5 0 0 0-.5-.5h-3a.5.5 0 0 0-.5.5ZM4.5 5.029l.5 8.5a.5.5 0 1 0 .998-.06l-.5-8.5a.5.5 0 1 0-.998.06Zm6.53-.528a.5.5 0 0 0-.528.47l-.5 8.5a.5.5 0 0 0 .998.058l.5-8.5a.5.5 0 0 0-.47-.528ZM8 4.5a.5.5 0 0 0-.5.5v8.5a.5.5 0 0 0 1 0V5a.5.5 0 0 0-.5-.5Z"/>
                        </svg>
                    </button>
                </div>
            </div>

            <div id="study-title" class="text-center text-indigo-400 font-extrabold text-lg mb-2"></div>
            <div id="card-counter" class="text-center text-slate-400 font-semibold mb-4"></div>
            <div class="card-container w-full h-80 mb-6">
                <div id="flashcard" class="flashcard relative w-full h-full cursor-pointer">
                    <!-- Card Front -->
                    <div id="card-front" class="card-face absolute w-full h-full flex items-center justify-center p-8 rounded-2xl shadow-xl transition-colors duration-300 bg-slate-800 border border-slate-700">
                         <span class="absolute top-4 left-4 text-xs font-mono text-slate-500" id="card-id-display"></span>
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
            
            <div class="mt-6 p-4 bg-slate-800/50 rounded-xl shadow-inner border border-slate-700">
                <h3 class="text-sm font-bold text-slate-400 uppercase tracking-wider mb-3 text-center">Marcar Domínio</h3>
                <div id="mastery-controls" class="flex justify-center flex-wrap gap-2">
                    <!-- Mastery buttons will be rendered here -->
                </div>
            </div>
        </div>
    </div>

    <!-- MODAL DE NOVO CARD -->
    <div id="new-card-modal" class="fixed inset-0 bg-black/70 flex items-center justify-center hidden z-50">
        <div class="bg-slate-800 p-6 rounded-xl shadow-2xl w-full max-w-lg mx-4 border border-slate-700">
            <div class="flex justify-between items-center mb-4">
                <h3 class="text-lg font-bold text-slate-200">Criar Novo Flashcard</h3>
                <button id="close-new-card-button" class="text-slate-500 hover:text-slate-200 text-2xl leading-none">&times;</button>
            </div>
            <form id="new-card-form" class="space-y-4">
                <!-- Form fields will be here -->
            </form>
        </div>
    </div>

    <!-- MODAL DE EDIÇÃO DE CARD -->
    <div id="edit-card-modal" class="fixed inset-0 bg-black/70 flex items-center justify-center hidden z-50">
        <div class="bg-slate-800 p-6 rounded-xl shadow-2xl w-full max-w-lg mx-4 border border-slate-700">
            <div class="flex justify-between items-center mb-4">
                <h3 class="text-lg font-bold text-slate-200">Editar Flashcard</h3>
                <button id="close-edit-card-button" class="text-slate-500 hover:text-slate-200 text-2xl leading-none">&times;</button>
            </div>
            <form id="edit-card-form" class="space-y-4">
                 <!-- Form fields will be here -->
            </form>
        </div>
    </div>

    <!-- MODAL DE CONFIRMAÇÃO DE EXCLUSÃO -->
    <div id="delete-confirm-modal" class="fixed inset-0 bg-black/70 flex items-center justify-center hidden z-50">
        <div class="bg-slate-800 p-6 rounded-xl shadow-2xl w-full max-w-sm mx-4 border border-slate-700">
            <h3 class="text-lg font-bold text-slate-200 mb-2">Confirmar Exclusão</h3>
            <p class="text-slate-400 mb-6">Você tem certeza que deseja excluir este card? Esta ação não pode ser desfeita.</p>
            <div class="flex justify-end space-x-4">
                <button id="cancel-delete-button" class="py-2 px-4 bg-slate-700 text-slate-200 font-bold rounded-lg hover:bg-slate-600">Cancelar</button>
                <button id="confirm-delete-button" class="py-2 px-4 bg-red-600 text-white font-bold rounded-lg hover:bg-red-700">Excluir</button>
            </div>
        </div>
    </div>


    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, onSnapshot, addDoc, updateDoc, doc, serverTimestamp, deleteDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyAJ7fpCQPimWgcJuYiLbCZ5Rf5YUgMVZ2Q",
            authDomain: "ghus-app.firebaseapp.com",
            projectId: "ghus-app",
            storageBucket: "ghus-app.firebasestorage.app",
            messagingSenderId: "1047907929485",
            appId: "1:1047907929485:web:cba05f2850df61d201c1c4",
        };
        
        const MASTERY_LEVELS = {
            'gray': { name: 'Padrão', cardBg: 'bg-slate-800', btnBg: 'bg-slate-600', ring: 'ring-slate-400', text: 'text-slate-200' },
            'red': { name: 'Revisar', cardBg: 'bg-red-900/50', btnBg: 'bg-red-600', ring: 'ring-red-400', text: 'text-red-300' },
            'orange': { name: 'Difícil', cardBg: 'bg-orange-900/50', btnBg: 'bg-orange-600', ring: 'ring-orange-400', text: 'text-orange-300' },
            'yellow': { name: 'Médio', cardBg: 'bg-yellow-900/50', btnBg: 'bg-yellow-600', ring: 'ring-yellow-400', text: 'text-yellow-300' },
            'green': { name: 'Fácil', cardBg: 'bg-green-900/50', btnBg: 'bg-green-600', ring: 'ring-green-400', text: 'text-green-300' },
            'blue': { name: 'Dominado', cardBg: 'bg-blue-900/50', btnBg: 'bg-blue-600', ring: 'ring-blue-400', text: 'text-blue-300' }
        };
        const COLOR_ORDER = ['red', 'orange', 'yellow', 'green', 'blue', 'gray'];
        const VALUE_TO_COLOR_MAP = { 0: 'gray', 1: 'red', 2: 'orange', 3: 'yellow', 4: 'green', 5: 'blue' };

        let allCards = [], currentDeckCards = [], currentCardIndex = 0, isFlipped = false, authUser = null;

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);

        // DOM elements
        const dom = {
            mainContainer: document.getElementById('main-container'),
            homeScreen: document.getElementById('home-screen'),
            studyScreen: document.getElementById('study-screen'),
            loading: document.getElementById('loading'),
            folderView: document.getElementById('folder-view'),
            folderViewMessage: document.getElementById('folder-view-message'),
            filterControls: document.getElementById('filter-controls'),
            searchInput: document.getElementById('search-input'),
            clearFiltersButton: document.getElementById('clear-filters-button'),
            addCardButton: document.getElementById('add-card-button'),
            newCardModal: document.getElementById('new-card-modal'),
            closeNewCardButton: document.getElementById('close-new-card-button'),
            newCardForm: document.getElementById('new-card-form'),
            editCardModal: document.getElementById('edit-card-modal'),
            closeEditCardButton: document.getElementById('close-edit-card-button'),
            editCardForm: document.getElementById('edit-card-form'),
            deleteConfirmModal: document.getElementById('delete-confirm-modal'),
            cancelDeleteButton: document.getElementById('cancel-delete-button'),
            confirmDeleteButton: document.getElementById('confirm-delete-button'),
            studyTitle: document.getElementById('study-title'),
            cardCounter: document.getElementById('card-counter'),
            flashcard: document.getElementById('flashcard'),
            cardFront: document.getElementById('card-front'),
            cardBack: document.getElementById('card-back'),
            frontText: document.querySelector('#card-front .markdown-content'),
            backText: document.querySelector('#card-back .markdown-content'),
            cardIdDisplay: document.getElementById('card-id-display'),
            flipButton: document.getElementById('flip-button'),
            prevButton: document.getElementById('prev-button'),
            nextButton: document.getElementById('next-button'),
            masteryControls: document.getElementById('mastery-controls'),
            backToHomeButton: document.getElementById('back-to-home-button'),
            editCardButton: document.getElementById('edit-card-button'),
            deleteCardButton: document.getElementById('delete-card-button'),
        };
        
        const cardFormHTML = (idPrefix) => `
            <div>
                <label for="${idPrefix}-card-question" class="block text-sm font-medium text-slate-400">Frente (Pergunta) - Suporta Markdown</label>
                <textarea id="${idPrefix}-card-question" required rows="4" class="mt-1 w-full p-2 bg-slate-700 border border-slate-600 rounded-md focus:ring-indigo-500 focus:border-indigo-500"></textarea>
            </div>
            <div>
                <label for="${idPrefix}-card-answer" class="block text-sm font-medium text-slate-400">Verso (Resposta) - Suporta Markdown</label>
                <textarea id="${idPrefix}-card-answer" required rows="4" class="mt-1 w-full p-2 bg-slate-700 border border-slate-600 rounded-md focus:ring-indigo-500 focus:border-indigo-500"></textarea>
            </div>
            <div class="flex space-x-4">
                <div class="flex-1">
                    <label for="${idPrefix}-card-specialty" class="block text-sm font-medium text-slate-400">Especialidade</label>
                    <input type="text" id="${idPrefix}-card-specialty" required class="mt-1 w-full p-2 bg-slate-700 border border-slate-600 rounded-md focus:ring-indigo-500 focus:border-indigo-500">
                </div>
                <div class="flex-1">
                    <label for="${idPrefix}-card-tags" class="block text-sm font-medium text-slate-400">Tags (separar por vírgula)</label>
                    <input type="text" id="${idPrefix}-card-tags" class="mt-1 w-full p-2 bg-slate-700 border border-slate-600 rounded-md focus:ring-indigo-500 focus:border-indigo-500">
                </div>
            </div>
            <div class="pt-2">
                <button type="submit" class="w-full py-2 px-4 bg-indigo-600 text-white font-bold rounded-lg shadow-md hover:bg-indigo-700 transition-colors">${idPrefix === 'new' ? 'Adicionar e Salvar' : 'Salvar Alterações'}</button>
            </div>
            <p id="${idPrefix}-card-message" class="text-center mt-2 text-sm text-green-500 hidden"></p>
        `;

        dom.newCardForm.innerHTML = cardFormHTML('new');
        dom.editCardForm.innerHTML = cardFormHTML('edit');

        function convertMasteryValueToString(value) {
            if (typeof value === 'string' && MASTERY_LEVELS[value]) return value;
            if (typeof value === 'number' && VALUE_TO_COLOR_MAP[value]) return VALUE_TO_COLOR_MAP[value];
            return 'gray';
        };

        function renderFilterControls() {
            if (allCards.length === 0) {
                dom.filterControls.innerHTML = '<p class="text-sm text-slate-500">Nenhum card para filtrar.</p>';
                return;
            }
            const specialties = [...new Set(allCards.map(c => c.specialty || 'Geral'))].sort();
            const tags = [...new Set(allCards.flatMap(c => (c.tags || '').split(',').map(t => t.trim()).filter(Boolean)))].sort();

            const createSection = (title, items, type) => {
                if(items.length === 0) return '';
                const itemsHtml = items.map(item => `
                    <label class="flex items-center space-x-2 text-sm text-slate-300 hover:bg-slate-700 p-1 rounded-md cursor-pointer">
                        <input type="checkbox" data-type="${type}" value="${item}" class="rounded text-indigo-500 focus:ring-indigo-500 bg-slate-600 border-slate-500">
                        <span>${item}</span>
                    </label>`).join('');
                return `<details class="mb-4" open><summary class="font-bold text-slate-300 cursor-pointer">${title}</summary><div class="mt-2 space-y-1">${itemsHtml}</div></details>`;
            };
            dom.filterControls.innerHTML = createSection('Especialidades', specialties, 'specialties') + createSection('Tags', tags, 'tags');
        }

        function renderHomeScreen() {
            dom.folderView.innerHTML = '';
            const filters = {
                specialties: [...dom.filterControls.querySelectorAll('input[data-type="specialties"]:checked')].map(i => i.value),
                tags: [...dom.filterControls.querySelectorAll('input[data-type="tags"]:checked')].map(i => i.value),
                search: dom.searchInput.value.toLowerCase()
            };

            let filteredCards = allCards.filter(card => {
                const specMatch = filters.specialties.length === 0 || filters.specialties.includes(card.specialty || 'Geral');
                const cardTags = (card.tags || '').split(',').map(t => t.trim());
                const tagMatch = filters.tags.length === 0 || filters.tags.some(t => cardTags.includes(t));
                const searchMatch = filters.search === '' || (card.question || '').toLowerCase().includes(filters.search) || (card.answer || '').toLowerCase().includes(filters.search);
                return specMatch && tagMatch && searchMatch;
            });
            
            const folders = filteredCards.reduce((acc, card) => {
                const key = card.specialty || 'Geral';
                if (!acc[key]) acc[key] = [];
                acc[key].push(card);
                return acc;
            }, {});

            dom.folderViewMessage.classList.toggle('hidden', Object.keys(folders).length > 0 || allCards.length === 0);
            
            if (Object.keys(folders).length === 0 && allCards.length > 0) {
                 dom.folderView.innerHTML = `<div class="col-span-full text-center p-8 text-slate-500 bg-slate-800/50 rounded-xl border border-dashed border-slate-700">Nenhum resultado encontrado.</div>`;
                 return;
            }

            Object.keys(folders).sort().forEach(specialty => {
                const cardsInDeck = folders[specialty];
                const folder = document.createElement('button');
                folder.className = 'bg-slate-800 p-4 rounded-xl shadow-md border border-slate-700 text-left hover:shadow-lg hover:border-indigo-500 transition';
                folder.onclick = () => startStudySession(specialty, cardsInDeck);
                
                const progressBar = COLOR_ORDER.map(color => {
                    const count = cardsInDeck.filter(c => c.masteryColor === color).length;
                    const percentage = (count / cardsInDeck.length) * 100;
                    return percentage > 0 ? `<div style="width:${percentage}%;" class="${MASTERY_LEVELS[color].btnBg}" title="${MASTERY_LEVELS[color].name}: ${count}"></div>` : '';
                }).join('');

                folder.innerHTML = `
                    <h3 class="text-base font-bold text-slate-200 truncate mb-1">${specialty}</h3>
                    <p class="text-sm text-slate-400">${cardsInDeck.length} Cards</p>
                    <div class="flex w-full overflow-hidden rounded-full mt-2 border border-slate-600 h-2">${progressBar}</div>
                `;
                dom.folderView.appendChild(folder);
            });
        }
        
        function switchScreen(screen) {
            dom.homeScreen.classList.toggle('hidden', screen === 'study');
            dom.studyScreen.classList.toggle('hidden', screen === 'home');
        }
        
        function startStudySession(title, cards) {
            currentDeckCards = [...cards].sort(() => Math.random() - 0.5); // Shuffle deck
            currentCardIndex = 0;
            isFlipped = false;
            dom.studyTitle.textContent = title;
            renderStudyCard();
            switchScreen('study');
        }

        function renderStudyCard() {
             if (currentDeckCards.length === 0) {
                switchScreen('home'); // No cards left in deck, go home
                return;
             }
             if (currentCardIndex >= currentDeckCards.length) {
                 currentCardIndex = 0; // Loop back to start
             }
             const card = currentDeckCards[currentCardIndex];
             dom.frontText.innerHTML = marked.parse(card.question || '');
             dom.backText.innerHTML = marked.parse(card.answer || '');
             dom.cardIdDisplay.textContent = `${card.id}`;
             dom.cardCounter.textContent = `Card ${currentCardIndex + 1} de ${currentDeckCards.length}`;
             if (isFlipped) { dom.flashcard.classList.remove('is-flipped'); isFlipped = false; }
             updateCardAppearance();
        }

        function updateCardAppearance() {
            const card = currentDeckCards[currentCardIndex];
            if (!card) return;
            const level = MASTERY_LEVELS[card.masteryColor] || MASTERY_LEVELS['gray'];
            Object.values(MASTERY_LEVELS).forEach(lvl => dom.cardFront.classList.remove(lvl.cardBg, lvl.text));
            dom.cardFront.classList.add(level.cardBg, level.text);

            dom.masteryControls.querySelectorAll('button').forEach(btn => {
                const isSelected = btn.dataset.color === card.masteryColor;
                btn.classList.toggle('ring-2', isSelected);
                btn.classList.toggle('ring-offset-2', isSelected);
                btn.classList.toggle('ring-offset-slate-800', isSelected);
                btn.classList.toggle(MASTERY_LEVELS[btn.dataset.color].ring, isSelected);
            });
        }
        
        function renderMasteryButtons() {
            dom.masteryControls.innerHTML = COLOR_ORDER.map(color => {
                const level = MASTERY_LEVELS[color];
                return `<button data-color="${color}" class="mastery-btn flex-1 py-2 px-2 rounded-lg shadow-md transition-all text-white font-semibold text-xs ${level.btnBg} hover:opacity-80">${level.name}</button>`;
            }).join('');
            dom.masteryControls.addEventListener('click', e => {
                if(e.target.tagName !== 'BUTTON') return;
                const color = e.target.dataset.color;
                const card = currentDeckCards[currentCardIndex];
                if(card) {
                    card.masteryColor = color;
                    updateDoc(doc(db, 'flashcards', card.id), { masteryColor: color });
                    updateCardAppearance();
                }
            });
        }

        function showModalMessage(modalPrefix, message, isError = false) {
            const messageEl = document.getElementById(`${modalPrefix}-card-message`);
            messageEl.textContent = message;
            messageEl.classList.remove('hidden');
            messageEl.classList.toggle('text-red-500', isError);
            messageEl.classList.toggle('text-green-500', !isError);
            setTimeout(() => messageEl.classList.add('hidden'), 3000);
        }

        async function handleNewCardSubmit(e) {
            e.preventDefault();
            const form = dom.newCardForm;
            const question = form.querySelector('#new-card-question').value.trim();
            const answer = form.querySelector('#new-card-answer').value.trim();
            if(!question || !answer) return;
            
            const newCard = {
                question, answer,
                specialty: form.querySelector('#new-card-specialty').value.trim() || 'Geral',
                tags: form.querySelector('#new-card-tags').value.trim(),
                masteryColor: 'gray',
                createdAt: serverTimestamp(),
                userId: authUser.uid
            };
            
            try {
                await addDoc(collection(db, 'flashcards'), newCard);
                showModalMessage('new', 'Card salvo com sucesso!');
                form.reset();
            } catch(error) {
                console.error("Error adding card: ", error);
                showModalMessage('new', 'Erro ao salvar o card.', true);
            }
        }

        async function handleEditCardSubmit(e) {
            e.preventDefault();
            const card = currentDeckCards[currentCardIndex];
            if(!card) return;

            const form = dom.editCardForm;
            const updatedData = {
                question: form.querySelector('#edit-card-question').value.trim(),
                answer: form.querySelector('#edit-card-answer').value.trim(),
                specialty: form.querySelector('#edit-card-specialty').value.trim() || 'Geral',
                tags: form.querySelector('#edit-card-tags').value.trim(),
            };

            try {
                await updateDoc(doc(db, 'flashcards', card.id), updatedData);
                showModalMessage('edit', 'Card atualizado com sucesso!');
                setTimeout(() => dom.editCardModal.classList.add('hidden'), 1000);
            } catch(error) {
                console.error("Error updating card: ", error);
                showModalMessage('edit', 'Erro ao atualizar o card.', true);
            }
        }

        async function handleDeleteCard() {
            const card = currentDeckCards[currentCardIndex];
            if(!card) return;
            try {
                await deleteDoc(doc(db, 'flashcards', card.id));
                // Remove from local arrays to reflect change immediately
                currentDeckCards.splice(currentCardIndex, 1);
                allCards = allCards.filter(c => c.id !== card.id);
                dom.deleteConfirmModal.classList.add('hidden');
                renderStudyCard(); // Render next card
            } catch(error) {
                console.error("Error deleting card: ", error);
            }
        }

        // Event Listeners
        dom.filterControls.addEventListener('change', renderHomeScreen);
        dom.searchInput.addEventListener('input', renderHomeScreen);
        dom.clearFiltersButton.addEventListener('click', () => {
            dom.searchInput.value = '';
            dom.filterControls.querySelectorAll('input').forEach(i => i.checked = false);
            renderHomeScreen();
        });
        dom.addCardButton.addEventListener('click', () => dom.newCardModal.classList.remove('hidden'));
        dom.closeNewCardButton.addEventListener('click', () => dom.newCardModal.classList.add('hidden'));
        dom.newCardForm.addEventListener('submit', handleNewCardSubmit);
        
        dom.editCardButton.addEventListener('click', () => {
            const card = currentDeckCards[currentCardIndex];
            if(!card) return;
            const form = dom.editCardForm;
            form.querySelector('#edit-card-question').value = card.question;
            form.querySelector('#edit-card-answer').value = card.answer;
            form.querySelector('#edit-card-specialty').value = card.specialty;
            form.querySelector('#edit-card-tags').value = card.tags;
            dom.editCardModal.classList.remove('hidden');
        });
        dom.closeEditCardButton.addEventListener('click', () => dom.editCardModal.classList.add('hidden'));
        dom.editCardForm.addEventListener('submit', handleEditCardSubmit);

        dom.deleteCardButton.addEventListener('click', () => dom.deleteConfirmModal.classList.remove('hidden'));
        dom.cancelDeleteButton.addEventListener('click', () => dom.deleteConfirmModal.classList.add('hidden'));
        dom.confirmDeleteButton.addEventListener('click', handleDeleteCard);

        dom.backToHomeButton.addEventListener('click', () => switchScreen('home'));
        dom.flashcard.addEventListener('click', () => { isFlipped = !isFlipped; dom.flashcard.classList.toggle('is-flipped'); });
        dom.flipButton.addEventListener('click', () => { isFlipped = !isFlipped; dom.flashcard.classList.toggle('is-flipped'); });
        dom.nextButton.addEventListener('click', () => { currentCardIndex = (currentCardIndex + 1) % currentDeckCards.length; renderStudyCard(); });
        dom.prevButton.addEventListener('click', () => { currentCardIndex = (currentCardIndex - 1 + currentDeckCards.length) % currentDeckCards.length; renderStudyCard(); });
        
        // App Initialization
        onAuthStateChanged(auth, user => {
            if (user) {
                authUser = user;
                onSnapshot(collection(db, 'flashcards'), snapshot => {
                    allCards = snapshot.docs.map(doc => ({ ...doc.data(), id: doc.id, masteryColor: convertMasteryValueToString(doc.data().masteryColor) }));
                    dom.loading.classList.add('hidden');
                    renderFilterControls();
                    renderHomeScreen();
                }, error => {
                    console.error("Firestore snapshot error:", error);
                    dom.loading.innerHTML = `<p class="text-red-500">Erro ao carregar dados. Verifique o console.</p>`;
                });
            } else {
                signInAnonymously(auth).catch(err => console.error("Anonymous sign-in failed", err));
            }
        });
        
        renderMasteryButtons();

    </script>
</body>
</html>
