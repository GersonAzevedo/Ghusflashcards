<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flashcards de Estudo</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap');
        body { font-family: 'Inter', sans-serif; }
        .card-container { perspective: 1000px; }
        .flashcard { transform-style: preserve-3d; transition: transform 0.6s; }
        .flashcard.is-flipped { transform: rotateY(180deg); }
        .card-face { backface-visibility: hidden; -webkit-backface-visibility: hidden; }
        .card-back { transform: rotateY(180deg); }
        #app { padding-bottom: 7rem; }
        /* Ícones Phosphor - Usados para os botões de pasta */
        @import url('https://unpkg.com/@phosphor-icons/web@2.1.1/src/css/icons.css');
        /* Estilo para o modal de log e novo card */
        .modal {
            background-color: rgba(0, 0, 0, 0.7);
            z-index: 50;
        }
    </style>
</head>
<body class="bg-slate-100 flex items-start justify-center min-h-screen p-4">

    <div id="main-container" class="w-full max-w-6xl h-full flex flex-col items-center">

        <!-- TELA INICIAL (HOME SCREEN) -->
        <div id="home-screen" class="w-full h-auto p-4 md:p-8 bg-white rounded-2xl shadow-xl transition-all duration-500 flex flex-col md:flex-row hidden">
            <div class="w-full md:w-1/4 md:min-w-[200px] border-b md:border-r border-slate-200 pb-4 md:pr-6 mb-6 md:mb-0">
                <h2 class="text-xl font-bold text-indigo-700 mb-4 border-b border-indigo-100 pb-2">Filtro de Cards</h2>
                
                <!-- SELETOR DE MODO -->
                <div class="flex space-x-4 mb-3 justify-between">
                    <label class="flex items-center text-sm font-medium text-slate-700">
                        <input type="radio" name="filter-mode" value="specialty" id="mode-specialty" checked class="text-indigo-600 focus:ring-indigo-500 mr-1">
                        Especialidade
                    </label>
                    <label class="flex items-center text-sm font-medium text-slate-700">
                        <input type="radio" name="filter-mode" value="tags" id="mode-tag" class="text-indigo-600 focus:ring-indigo-500 mr-1">
                        Tag
                    </label>
                </div>

                <input type="text" id="search-input" placeholder="Buscar especialidade..." 
                       class="w-full p-2 mb-3 border border-slate-300 rounded-lg text-sm focus:ring-indigo-500 focus:border-indigo-500">
                
                <!-- Botão Adicionar Card -->
                <button id="add-card-button" class="w-full py-2 px-4 bg-green-500 text-white font-bold rounded-lg shadow-md hover:bg-green-600 transition-colors mb-4 text-sm flex items-center justify-center space-x-2">
                    <i class="ph-plus-circle-fill text-lg"></i><span>Adicionar Card</span>
                </button>


                <div id="specialty-list" class="space-y-2 max-h-[70vh] overflow-y-auto">
                    <p class="text-sm text-slate-500">Carregando...</p>
                </div>
            </div>
            <div id="home-content" class="flex-grow md:pl-6">
                
                <div id="global-controls" class="flex justify-between items-center mb-6">
                    <h1 class="text-3xl font-extrabold text-slate-800">Revisão Ativa</h1>
                    <button id="show-log-button" class="text-sm font-bold py-2 px-4 bg-indigo-50 text-indigo-700 rounded-lg hover:bg-indigo-100 transition-colors border border-indigo-200">Ver Log →</button>
                </div>

                <p class="text-slate-600 mb-6">Selecione o tópico ao lado para começar.</p>
                
                <!-- VISUALIZAÇÃO DE PASTAS -->
                <div id="folder-view" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
                    <!-- Pastas serão renderizadas aqui -->
                </div>
            </div>
        </div>

        <!-- TELA DE ESTUDO (STUDY SCREEN) -->
        <div id="app" class="w-full max-w-xl hidden">
             
            <div id="global-controls-study" class="flex justify-between items-center w-full mb-6">
                <button id="back-to-home" class="text-sm font-semibold text-slate-500 hover:text-indigo-600">← Voltar</button>
                <button id="show-log-button-study" class="text-sm font-bold py-2 px-4 bg-indigo-50 text-indigo-700 rounded-lg hover:bg-indigo-100 transition-colors border border-indigo-200">Ver Log →</button>
            </div>


            <div id="specialty-title" class="text-center text-indigo-600 font-extrabold text-lg mb-2"></div>
            <div id="card-counter" class="text-center text-slate-500 font-semibold mb-4"></div>
            <div class="card-container w-full h-80 mb-6">
                <div id="flashcard" class="flashcard relative w-full h-full cursor-pointer">
                    <div id="card-front" class="card-face absolute w-full h-full flex items-center justify-center p-8 rounded-2xl shadow-xl transition-colors duration-300">
                        <p class="text-2xl md:text-3xl font-semibold text-center text-slate-800"></p>
                    </div>
                    <div id="card-back" class="card-face card-back absolute w-full h-full flex items-center justify-center p-8 rounded-2xl shadow-xl transition-colors duration-300">
                        <p class="text-xl md:text-2xl text-center text-slate-700"></p>
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

    <!-- MODAL DE NOVO CARD (NOVO) -->
    <div id="new-card-modal" class="modal fixed inset-0 flex items-center justify-center hidden">
        <div class="bg-white p-6 rounded-xl shadow-2xl w-full max-w-lg mx-4">
            <div class="flex justify-between items-center mb-4">
                <h3 class="text-lg font-bold text-slate-800">Criar Novo Flashcard</h3>
                <button id="close-new-card" class="text-slate-500 hover:text-slate-800 text-2xl leading-none">&times;</button>
            </div>
            
            <form id="new-card-form" class="space-y-4">
                <div>
                    <label for="new-card-frente" class="block text-sm font-medium text-slate-700">Frente (Pergunta)</label>
                    <textarea id="new-card-frente" required class="mt-1 w-full p-2 border border-slate-300 rounded-md focus:ring-indigo-500 focus:border-indigo-500"></textarea>
                </div>
                <div>
                    <label for="new-card-verso" class="block text-sm font-medium text-slate-700">Verso (Resposta)</label>
                    <textarea id="new-card-verso" required class="mt-1 w-full p-2 border border-slate-300 rounded-md focus:ring-indigo-500 focus:border-indigo-500"></textarea>
                </div>
                <div class="flex space-x-4">
                    <div class="flex-1">
                        <label for="new-card-especialidade" class="block text-sm font-medium text-slate-700">Especialidade</label>
                        <input type="text" id="new-card-especialidade" required class="mt-1 w-full p-2 border border-slate-300 rounded-md focus:ring-indigo-500 focus:border-indigo-500">
                    </div>
                    <div class="flex-1">
                        <label for="new-card-tags" class="block text-sm font-medium text-slate-700">Tags (Separar por espaço)</label>
                        <input type="text" id="new-card-tags" class="mt-1 w-full p-2 border border-slate-300 rounded-md focus:ring-indigo-500 focus:border-indigo-500">
                    </div>
                </div>
                
                <div class="pt-2">
                    <button type="submit" class="w-full py-2 px-4 bg-indigo-600 text-white font-bold rounded-lg shadow-md hover:bg-indigo-700 transition-colors">
                        Adicionar e Salvar
                    </button>
                </div>
            </form>
        </div>
    </div>


    <!-- MODAL DE LOG -->
    <div id="log-modal" class="modal fixed inset-0 flex items-center justify-center hidden">
        <div class="bg-white p-6 rounded-xl shadow-2xl w-full max-w-lg mx-4">
            <div class="flex justify-between items-center mb-4">
                <h3 class="text-lg font-bold text-slate-800">Log de Alterações (Sessão Atual)</h3>
                <button id="close-log" class="text-slate-500 hover:text-slate-800 text-2xl leading-none">&times;</button>
            </div>
            
            <div class="relative h-64">
                <textarea id="log-area" readonly class="w-full h-full p-2 bg-slate-100 text-slate-800 text-xs rounded-md border border-slate-300 focus:outline-none"></textarea>
            </div>

            <div class="mt-4 flex justify-end">
                <button id="copy-log-button" class="text-sm font-semibold py-2 px-4 bg-indigo-500 text-white rounded-lg hover:bg-indigo-600 transition-colors">Copiar Log</button>
            </div>
        </div>
    </div>

    <script>
        // --- DADOS E CONSTANTES ---
        const originalSheetUrl = 'https://docs.google.com/spreadsheets/d/e/2PACX-1vTuKOdGRgn_JIk3jsVorWCXFtAuewmXj75vNb_2okDq9uG2ladBckiWoB8-26mh1SlndcUxIpo1TB-D/pub?gid=0&single=true&output=csv';
        const GOOGLE_SHEET_URL = 'https://corsproxy.io/?' + encodeURIComponent(originalSheetUrl);
        const ML = {'gray': { cardBg: 'bg-white', btnBg: 'bg-slate-300', ring: 'ring-slate-400' }, 'red': { cardBg: 'bg-red-100', btnBg: 'bg-red-400', ring: 'ring-red-400' }, 'orange': { cardBg: 'bg-orange-100', btnBg: 'bg-orange-400', ring: 'ring-orange-400' }, 'yellow': { cardBg: 'bg-yellow-100', btnBg: 'bg-yellow-400', ring: 'ring-yellow-400' }, 'green': { cardBg: 'bg-green-100', btnBg: 'bg-green-400', ring: 'ring-green-400' }, 'blue': { cardBg: 'bg-blue-100', btnBg: 'bg-blue-400', ring: 'ring-blue-400' }};
        
        // --- ESTADO GERAL E DOM ---
        let allC = [], cards = [], currentI = 0, isFlipped = false, currentS = null, currentM = 'specialty';
        const getId = id => document.getElementById(id); 
        const HOME = getId('home-screen'), SIDEBAR = getId('specialty-list'), SEARCH = getId('search-input'), 
        MODE_S = getId('mode-specialty'), MODE_T = getId('mode-tag'), HOME_CONTENT = getId('home-content'), FOLDERS = getId('folder-view'),
        STUDY = getId('app'), S_TITLE = getId('specialty-title'), CARD_CONT = getId('flashcard'), cF = getId('card-front'), cB = getId('card-back'), 
        F_TEXT = document.querySelector('#card-front p'), B_TEXT = document.querySelector('#card-back p'), COUNTER = getId('card-counter'), CONTROLS = getId('mastery-controls'), 
        FLIP_BTN = getId('flip-button'), NEXT_BTN = getId('next-button'), PREV_BTN = getId('prev-button'), BACK_BTN = getId('back-to-home'), 
        LOG_BTN_H = getId('show-log-button'), LOG_BTN_S = getId('show-log-button-study'), 
        LOG_MODAL = getId('log-modal'), L_AREA = getId('log-area'), COPY_BTN = getId('copy-log-button'), CLOSE_LOG = getId('close-log'),
        ADD_CARD_BTN = getId('add-card-button'), NEW_CARD_MODAL = getId('new-card-modal'), CLOSE_NEW_CARD = getId('close-new-card'),
        NEW_CARD_FORM = getId('new-card-form'), NEW_FRENTE = getId('new-card-frente'), NEW_VERSO = getId('new-card-verso'), 
        NEW_ESP = getId('new-card-especialidade'), NEW_TAGS = getId('new-card-tags');


        // --- PERSISTÊNCIA E LOG ---
        const getKey = s => 'flashcardState_' + s;
        const save = () => { if (!currentS) return; localStorage.setItem(getKey(currentS), JSON.stringify({ cards, log: L_AREA.value })); };
        function addLog(m) {
            const now = new Date();
            const ts = now.toLocaleTimeString('pt-BR', { hour: '2-digit', minute: '2-digit', second: '2-digit' });
            L_AREA.value += `[${ts}] ${m}\n`; L_AREA.scrollTop = L_AREA.scrollHeight; 
        }
        
        function logNewCard(card) {
            const csvLine = [
                card.id,
                `"${card.question}"`,
                `"${card.answer}"`,
                card.masteryColor,
                `"${card.specialty}"`,
                `"${card.tags}"`
            ].join('\t');
            
            addLog(`NOVO CARD CRIADO (ADICIONAR À PLANILHA):\n[${csvLine}]\n`);
        }


        // --- CARGA DE DADOS (CSV) ---
        async function fetchCards() {
            try {
                const res = await fetch(GOOGLE_SHEET_URL);
                if (!res.ok) throw new Error(`Falha na requisição. Status: ${res.status}.`);
                const lines = (await res.text()).trim().split('\n');
                const header = lines.shift().split(',').map(h => h.trim().replace(/"/g, ''));
                const idx = { id: header.indexOf('Número'), q: header.indexOf('Frente'), a: header.indexOf('Verso'), c: header.indexOf('Cor'), s: header.indexOf('Especialidade'), t: header.indexOf('Tags') };

                if (idx.id < 0 || idx.q < 0 || idx.a < 0 || idx.s < 0 || idx.t < 0) throw new Error("Colunas necessárias (Número, Frente, Verso, Especialidade, Tags) não encontradas.");

                allC = lines.map(line => {
                    const v = line.split(','); 
                    const gV = i => v[i] ? v[i].trim().replace(/^"|"$/g, '') : '';
                    const color = gV(idx.c).toLowerCase() || 'gray';
                    const specialty = gV(idx.s) || 'Geral';
                    const tags = gV(idx.t) || '';
                    return { id: parseInt(gV(idx.id), 10), question: gV(idx.q), answer: gV(idx.a), specialty, tags, masteryColor: ML[color] ? color : 'gray' };
                }).filter(c => c.id && c.question);
                
                if(allC.length === 0) {
                    HOME_CONTENT.insertAdjacentHTML('beforeend', `<div class="p-4 mt-6 bg-yellow-100 border-l-4 border-yellow-500 rounded-lg text-sm text-yellow-800"><p class="font-semibold">AVISO:</p><p>Planilha carregada, mas nenhum card encontrado. Verifique se há dados nas linhas.</p></div>`);
                }

            } catch (e) {
                const errorHtml = `<div class="p-4 mt-6 bg-red-100 border-l-4 border-red-500 rounded-lg text-sm text-red-800"><p class="font-semibold mb-1">ERRO DE CARGA:</p><p>${e.message}</p></div>`;
                HOME_CONTENT.insertAdjacentHTML('beforeend', errorHtml);
                console.error('Fetch error:', e);
            }
        }
        
        // --- ADICIONAR CARD (NOVO) ---
        function addNewCard(e) {
            e.preventDefault();
            
            const frente = NEW_FRENTE.value.trim();
            const verso = NEW_VERSO.value.trim();
            const especialidade = NEW_ESP.value.trim() || 'Geral';
            const tags = NEW_TAGS.value.trim();

            if (!frente || !verso) {
                alert("Preencha Frente e Verso.");
                return;
            }

            const maxId = allC.length > 0 ? Math.max(...allC.map(c => c.id)) : 0;
            const newId = maxId + 1;

            const newCard = { id: newId, question: frente, answer: verso, specialty: especialidade, tags: tags, masteryColor: 'gray' };

            allC.push(newCard);
            NEW_CARD_FORM.reset();
            NEW_CARD_MODAL.classList.add('hidden');
            
            logNewCard(newCard);
            renderList();
        }

        // --- NAVEGAÇÃO E TELA ---
        function renderScreen(s) {
            HOME.classList.toggle('hidden', s !== 'home'); STUDY.classList.toggle('hidden', s !== 'study'); LOG_MODAL.classList.add('hidden'); NEW_CARD_MODAL.classList.add('hidden');
            if (s === 'home') { SEARCH.value = ''; renderList(); }
        }

        const extractItems = (mode) => Array.from(new Set(allC.map(c => c[mode]))).filter(i => i && i.trim() !== '').sort();
        const applyFilters = () => renderList(SEARCH.value.toLowerCase());

        function renderList(filter = '') {
            SIDEBAR.innerHTML = ''; FOLDERS.innerHTML = '';
            
            const allItems = extractItems(currentM);
            const filteredItems = allItems.filter(item => item.toLowerCase().includes(filter));
            const icon = currentM === 'specialty' ? 'ph-folder-open-fill' : 'ph-tag-fill';

            if (filteredItems.length === 0) {
                const msg = allItems.length > 0 ? `<p class="text-sm text-red-500">Nenhum item encontrado.</p>` : '<p class="text-sm text-slate-500">Nenhum dado carregado.</p>';
                SIDEBAR.innerHTML = msg; FOLDERS.innerHTML = `<div class="col-span-full text-center p-8 text-slate-500">${msg}</div>`;
                return;
            }

            filteredItems.forEach(item => {
                const deckCards = allC.filter(c => c[currentM] === item);
                const count = deckCards.length;
                
                const colorCounts = deckCards.reduce((acc, card) => { acc[card.masteryColor] = (acc[card.masteryColor] || 0) + 1; return acc; }, {});

                let colorBarHtml = ['red', 'orange', 'yellow', 'green', 'blue', 'gray'].map(color => {
                    const cCount = colorCounts[color] || 0;
                    if (cCount > 0) {
                        const perc = Math.round((cCount / count) * 100); 
                        return `<div style="width:${perc}%;" class="h-2 ${ML[color].btnBg}"></div>`;
                    }
                    return '';
                }).join('');
                
                const progressBar = `<div class="flex w-full overflow-hidden rounded-full mt-2 border border-slate-200">${colorBarHtml}</div>`;

                const btnSidebar = document.createElement('button');
                btnSidebar.textContent = `${item} (${count})`;
                btnSidebar.className = 'w-full text-left p-2 rounded-lg text-slate-700 font-medium hover:bg-indigo-100 transition-colors text-sm truncate';
                btnSidebar.onclick = () => startSession(item);
                SIDEBAR.appendChild(btnSidebar);

                const folderCard = document.createElement('button');
                folderCard.className = 'bg-white p-4 rounded-xl shadow-md border border-slate-200 text-left hover:shadow-lg hover:border-indigo-400 transition transform hover:scale-[1.02] cursor-pointer';
                folderCard.onclick = () => startSession(item);
                folderCard.innerHTML = `
                    <div class="text-indigo-600 text-3xl mb-2 flex items-center"><i class="${icon}"></i></div>
                    <h3 class="text-base font-bold text-slate-800 truncate mb-1">${item}</h3>
                    <p class="text-sm text-slate-500">${count} Cards</p>
                    ${progressBar}`;
                FOLDERS.appendChild(folderCard);
            });
        }

        function startSession(iV) {
            currentS = iV; let cTS = allC.filter(c => c[currentM] === iV);
            
            const saved = localStorage.getItem(getKey(currentS));
            if (saved) {
                const s = JSON.parse(saved);
                const sM = new Map(s.cards.map(c => [c.id, c.masteryColor]));
                cards = cTS.map(c => ({ ...c, masteryColor: sM.get(c.id) || c.masteryColor }));
                L_AREA.value = s.log || '';
                addLog(`Sessão retomada para ${currentM === 'specialty' ? 'Especialidade' : 'Tag'}: ${iV}.`);
            } else {
                cards = cTS;
                L_AREA.value = `[${new Date().toLocaleTimeString('pt-BR')}] Iniciada nova sessão por ${currentM === 'specialty' ? 'Especialidade' : 'Tag'}: ${iV} (${cards.length} cards).\n`;
            }

            currentI = 0; isFlipped = false; S_TITLE.textContent = `${currentM.toUpperCase()}: ${iV.toUpperCase()}`; renderCard(); renderScreen('study');
        }

        // --- CARD E DOMÍNIO ---
        function setMColor(c) {
            const card = cards[currentI];
            if (card.masteryColor !== c) {
                card.masteryColor = c; addLog(`Card ${card.id} - Cor alterada para ${c}.`); updateAppearance(); save(); 
            }
        }

        function renderMButtons() {
            CONTROLS.innerHTML = '';
            const card = cards[currentI];
            for (const color in ML) {
                const level = ML[color];
                const btn = document.createElement('button');
                const isS = card && card.masteryColor === color;
                const rC = isS ? `ring-4 ring-offset-2 ${level.ring}` : 'border-white';
                
                btn.className = `w-10 h-10 rounded-full border-2 shadow-lg transition-all transform hover:scale-110 ${level.btnBg} ${rC}`;
                btn.onclick = () => setMColor(color);
                CONTROLS.appendChild(btn);
            }
        }

        function updateAppearance() {
            if (!cards.length) return;
            const m = ML[cards[currentI].masteryColor];
            renderMButtons(); 
            const allBg = Object.values(ML).map(l => l.cardBg).concat(['bg-slate-50', 'bg-white']);
            cF.classList.remove(...allBg); cB.classList.remove(...allBg);
            cF.classList.add(m.cardBg); cB.classList.add(m.cardBg);
        }
        
        function renderCard() {
            if (!cards.length) {
                F_TEXT.textContent = "Nenhum card."; B_TEXT.textContent = "Volte para a tela inicial."; COUNTER.textContent = '0 cards.'; return;
            };

            const card = cards[currentI];
            F_TEXT.textContent = card.question; B_TEXT.textContent = card.answer;
            COUNTER.textContent = `Card ${currentI + 1} de ${cards.length}`;
            
            if (isFlipped) {
                CARD_CONT.classList.remove('is-flipped'); isFlipped = false; FLIP_BTN.textContent = 'Virar Card';
            }
            updateAppearance();
        }

        // --- NAVEGAÇÃO ---
        function flipCard() {
            if (!cards.length) return;
            isFlipped = !isFlipped;
            CARD_CONT.classList.toggle('is-flipped');
            FLIP_BTN.textContent = isFlipped ? 'Esconder Resposta' : 'Virar Card';
        }

        const nextCard = () => { if (!cards.length) return; currentI = (currentI + 1) % cards.length; renderCard(); };
        const prevCard = () => { if (!cards.length) return; currentI = (currentI - 1 + cards.length) % cards.length; renderCard(); };

        // --- LISTENERS E INIT ---
        FLIP_BTN.addEventListener('click', flipCard); CARD_CONT.addEventListener('click', flipCard);
        NEXT_BTN.addEventListener('click', nextCard); PREV_BTN.addEventListener('click', prevCard);
        BACK_BTN.addEventListener('click', () => renderScreen('home')); 
        
        if (MODE_S) MODE_S.addEventListener('change', () => { currentM = 'specialty'; SEARCH.placeholder = 'Buscar especialidade...'; renderList(); });
        if (MODE_T) MODE_T.addEventListener('change', () => { currentM = 'tags'; SEARCH.placeholder = 'Buscar tags...'; renderList(); });
        if (SEARCH) SEARCH.addEventListener('input', applyFilters);
        
        LOG_BTN_H.addEventListener('click', () => LOG_MODAL.classList.remove('hidden'));
        LOG_BTN_S.addEventListener('click', () => LOG_MODAL.classList.remove('hidden'));
        CLOSE_LOG.addEventListener('click', () => LOG_MODAL.classList.add('hidden'));

        COPY_BTN.addEventListener('click', () => { L_AREA.select(); document.execCommand('copy'); });

        ADD_CARD_BTN.addEventListener('click', () => NEW_CARD_MODAL.classList.remove('hidden'));
        CLOSE_NEW_CARD.addEventListener('click', () => NEW_CARD_MODAL.classList.add('hidden'));
        NEW_CARD_FORM.addEventListener('submit', addNewCard);

        async function init() { renderScreen('home'); await fetchCards(); renderList(); }
        init();
    </script>

</body>
</html>

