[cs2_case_upgrade_simulator.html](https://github.com/user-attachments/files/32431484/cs2_case_upgrade_simulator.html)
<!DOCTYPE html>
<html lang="ru" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CS2 Case & Upgrade Simulator</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        csdark: '#0f172a',
                        cspanel: '#1e293b',
                        csaccent: '#3b82f6',
                        csglow: '#60a5fa',
                    }
                }
            }
        }
    </script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800;900&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Inter', sans-serif; }
        /* Custom scrollbar for inventory */
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: #0f172a; }
        ::-webkit-scrollbar-thumb { background: #334155; border-radius: 3px; }
        ::-webkit-scrollbar-thumb:hover { background: #475569; }
        
        @keyframes pulse-glow {
            0%, 100% { box-shadow: 0 0 15px rgba(59, 130, 246, 0.4); }
            50% { box-shadow: 0 0 25px rgba(59, 130, 246, 0.8); }
        }
        .glow-box { animation: pulse-glow 3s infinite; }
    </style>
</head>
<body class="bg-csdark text-slate-100 min-h-screen flex flex-col select-none overflow-x-hidden">

    <!-- Header -->
    <header class="bg-cspanel/80 backdrop-blur-md border-b border-slate-800 sticky top-0 z-50 px-4 py-3">
        <div class="max-w-7xl mx-auto flex flex-wrap justify-between items-center gap-4">
            <div class="flex items-center space-x-3">
                <div class="bg-gradient-to-tr from-blue-600 to-indigo-500 p-2.5 rounded-xl shadow-lg shadow-blue-500/30">
                    <svg class="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20 7l-8-4-8 4m16 0l-8 4m8-4v10l-8 4m0-10L4 7m8 4v10M4 7v10l8 4"></path></svg>
                </div>
                <div>
                    <h1 class="text-xl font-black tracking-wider bg-gradient-to-r from-blue-400 to-indigo-300 bg-clip-text text-transparent">CS2 CASE & UPGRADE</h1>
                    <p class="text-xs text-slate-400">Симулятор кейсов и контрактов</p>
                </div>
            </div>

            <div class="flex items-center gap-4 sm:gap-6">
                <div class="bg-slate-900/80 px-4 py-2 rounded-xl border border-slate-700/50 flex items-center space-x-2">
                    <span class="text-amber-400 text-lg">🪙</span>
                    <div>
                        <div class="text-[10px] text-slate-400 uppercase font-semibold">Баланс</div>
                        <div id="user-balance" class="font-bold text-amber-400 text-lg">1000.00 $</div>
                    </div>
                </div>

                <button onclick="addBonusMoney()" class="bg-gradient-to-r from-emerald-600 to-teal-600 hover:from-emerald-500 hover:to-teal-500 text-white font-bold px-4 py-2 rounded-xl shadow-lg shadow-emerald-600/20 transition-all text-sm flex items-center gap-1.5 active:scale-95">
                    <span>+ 500 $</span>
                </button>
            </div>
        </div>
    </header>

    <!-- Navigation Tabs -->
    <nav class="max-w-7xl mx-auto w-full px-4 mt-6">
        <div class="flex space-x-2 bg-cspanel p-1.5 rounded-2xl border border-slate-800">
            <button onclick="switchTab('cases')" id="tab-cases" class="flex-1 py-3 px-4 rounded-xl font-bold text-sm transition-all bg-blue-600 text-white shadow-md shadow-blue-600/30 flex items-center justify-center gap-2">
                📦 <span>Кейсы</span>
            </button>
            <button onclick="switchTab('inventory')" id="tab-inventory" class="flex-1 py-3 px-4 rounded-xl font-bold text-sm transition-all text-slate-400 hover:text-white hover:bg-slate-800/50 flex items-center justify-center gap-2">
                🎒 <span id="inventory-tab-label">Инвентарь (0)</span>
            </button>
            <button onclick="switchTab('upgrade')" id="tab-upgrade" class="flex-1 py-3 px-4 rounded-xl font-bold text-sm transition-all text-slate-400 hover:text-white hover:bg-slate-800/50 flex items-center justify-center gap-2">
                ⚡ <span>Апгрейд скинов</span>
            </button>
        </div>
    </nav>

    <!-- Main Container -->
    <main class="max-w-7xl mx-auto w-full px-4 py-6 flex-grow flex flex-col">

        <!-- SECTION: CASES -->
        <section id="section-cases" class="space-y-6">
            <div class="flex justify-between items-center">
                <h2 class="text-xl font-bold text-slate-200">Доступные кейсы</h2>
                <p class="text-sm text-slate-400">Открывайте кейсы и выбивайте редкие скины!</p>
            </div>

            <!-- Cases Grid -->
            <div id="cases-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
                <!-- Dynamically populated via JS -->
            </div>

            <!-- Case Opening Modal / Area -->
            <div id="opening-container" class="hidden bg-cspanel border border-slate-800 rounded-3xl p-6 sm:p-8 text-center space-y-6 relative overflow-hidden shadow-2xl">
                <div class="flex justify-between items-center">
                    <h3 id="opening-case-title" class="text-lg font-bold text-slate-200">Открытие кейса</h3>
                    <button onclick="closeCaseOpening()" class="text-slate-400 hover:text-white bg-slate-800 p-2 rounded-xl text-sm">✖ Закрыть</button>
                </div>

                <!-- Case Roll Roller Area -->
                <div class="relative w-full overflow-hidden bg-slate-950/80 rounded-2xl border border-slate-800/80 py-10 shadow-inner">
                    <!-- Center Marker Line -->
                    <div class="absolute top-0 bottom-0 left-1/2 w-1 bg-amber-500 z-20 shadow-[0_0_15px_#f59e0b]"></div>
                    <div class="absolute top-2 left-1/2 -translate-x-1/2 text-amber-500 text-xs font-bold uppercase tracking-wider z-25 bg-slate-900 px-2 py-0.5 rounded border border-amber-500/30">Победитель</div>

                    <!-- Roller Track -->
                    <div id="roller-track" class="flex items-center space-x-4 px-4 transition-all duration-0 ease-out" style="transform: translateX(0px);">
                        <!-- Items rendered via JS -->
                    </div>
                </div>

                <div id="case-result-info" class="min-h-[70px] flex flex-col items-center justify-center">
                    <p class="text-slate-400 text-sm">Нажмите кнопку ниже, чтобы запустить рулетку</p>
                </div>

                <div>
                    <button id="start-spin-btn" onclick="spinCase()" class="bg-gradient-to-r from-blue-600 to-indigo-600 hover:from-blue-500 hover:to-indigo-500 text-white font-bold px-8 py-3.5 rounded-2xl shadow-lg shadow-blue-600/30 transition-all text-base active:scale-95">
                        Открыть кейс
                    </button>
                </div>
            </div>
        </section>

        <!-- SECTION: INVENTORY -->
        <section id="section-inventory" class="hidden space-y-6">
            <div class="flex flex-wrap justify-between items-center gap-4 bg-cspanel p-4 rounded-2xl border border-slate-800">
                <div>
                    <h2 class="text-xl font-bold text-slate-200">Ваш инвентарь</h2>
                    <p class="text-sm text-slate-400" id="inventory-subtitle">Всего предметов: 0 | Общая стоимость: $0.00</p>
                </div>
                <div class="flex gap-2">
                    <button onclick="sellAllSkins()" class="bg-rose-600/20 hover:bg-rose-600/30 text-rose-400 border border-rose-500/30 font-semibold px-4 py-2 rounded-xl text-sm transition-all">
                        Продать все
                    </button>
                </div>
            </div>

            <!-- Inventory Grid -->
            <div id="inventory-grid" class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 gap-4">
                <!-- Dynamically populated -->
            </div>
        </section>

        <!-- SECTION: UPGRADE -->
        <section id="section-upgrade" class="hidden space-y-6">
            <div class="bg-cspanel border border-slate-800 rounded-3xl p-6 sm:p-8 space-y-6 shadow-xl">
                <div>
                    <h2 class="text-xl font-bold text-slate-200">Апгрейд скинов (Колесо Фортуны)</h2>
                    <p class="text-sm text-slate-400">Выберите ваш скин для апгрейда, множитель или желаемый скин и испытайте удачу!</p>
                </div>

                <div class="grid grid-cols-1 lg:grid-cols-2 gap-8 items-center">
                    <!-- Left Column: Selection -->
                    <div class="space-y-6 bg-slate-900/60 p-6 rounded-2xl border border-slate-800">
                        <!-- Step 1: Skin to upgrade -->
                        <div>
                            <label class="block text-sm font-semibold text-slate-300 mb-2">1. Выберите скин из инвентаря:</label>
                            <div id="upgrade-selected-skin" onclick="openInventoryPicker()" class="bg-slate-800 hover:bg-slate-750 border-2 border-dashed border-slate-700 rounded-2xl p-4 cursor-pointer text-center transition-all flex items-center justify-center min-h-[100px]">
                                <span class="text-slate-400 text-sm">Нажмите, чтобы выбрать скин для ставки</span>
                            </div>
                        </div>

                        <!-- Step 2: Target multiplier or target skin -->
                        <div class="space-y-3">
                            <label class="block text-sm font-semibold text-slate-300">2. Выберите режим апгрейда:</label>
                            
                            <!-- Multiplier buttons -->
                            <div class="grid grid-cols-4 gap-2">
                                <button onclick="setMultiplier(1.5)" id="mult-1.5" class="mult-btn bg-slate-800 hover:bg-slate-700 border border-slate-700 py-2.5 rounded-xl font-bold text-sm transition-all text-blue-400">1.5x</button>
                                <button onclick="setMultiplier(2)" id="mult-2" class="mult-btn bg-slate-800 hover:bg-slate-700 border border-slate-700 py-2.5 rounded-xl font-bold text-sm transition-all text-blue-400">2x</button>
                                <button onclick="setMultiplier(5)" id="mult-5" class="mult-btn bg-slate-800 hover:bg-slate-700 border border-slate-700 py-2.5 rounded-xl font-bold text-sm transition-all text-blue-400">5x</button>
                                <button onclick="setMultiplier(10)" id="mult-10" class="mult-btn bg-slate-800 hover:bg-slate-700 border border-slate-700 py-2.5 rounded-xl font-bold text-sm transition-all text-blue-400">10x</button>
                            </div>
                        </div>

                        <!-- Upgrade calculation info -->
                        <div id="upgrade-info-box" class="bg-slate-950 p-4 rounded-xl border border-slate-800 text-sm space-y-1 hidden">
                            <div class="flex justify-between text-slate-400">
                                <span>Стоимость ставки:</span>
                                <span id="up-cost" class="font-bold text-slate-200">$0.00</span>
                            </div>
                            <div class="flex justify-between text-slate-400">
                                <span>Желаемый выигрыш:</span>
                                <span id="up-win" class="font-bold text-amber-400">$0.00</span>
                            </div>
                            <div class="flex justify-between text-slate-400">
                                <span>Шанс успеха (Зеленая зона):</span>
                                <span id="up-chance" class="font-bold text-emerald-400">0%</span>
                            </div>
                        </div>
                    </div>

                    <!-- Right Column: Wheel Canvas & Spin -->
                    <div class="flex flex-col items-center justify-center space-y-6 bg-slate-900/60 p-6 rounded-2xl border border-slate-800">
                        <div class="relative w-64 h-64 sm:w-72 sm:h-72 flex items-center justify-center">
                            <!-- Wheel Container -->
                            <canvas id="upgrade-wheel" width="300" height="300" class="w-full h-full drop-shadow-2xl"></canvas>
                            <!-- Wheel Pointer / Arrow top -->
                            <div class="absolute -top-3 left-1/2 -translate-x-1/2 w-0 h-0 border-l-[10px] border-l-transparent border-r-[10px] border-r-transparent border-t-[16px] border-t-amber-400 drop-shadow-[0_2px_4px_rgba(0,0,0,0.8)] z-10"></div>
                        </div>

                        <button id="start-upgrade-btn" onclick="startUpgradeSpin()" disabled class="w-full bg-gradient-to-r from-emerald-600 to-teal-600 hover:from-emerald-500 hover:to-teal-500 disabled:from-slate-800 disabled:to-slate-800 disabled:text-slate-600 text-white font-bold py-3.5 rounded-2xl shadow-lg transition-all text-base active:scale-95 cursor-pointer disabled:cursor-not-allowed">
                            Крутить колесо фортуны
                        </button>
                    </div>
                </div>
            </div>
        </section>

    </main>

    <!-- Modal: Inventory Picker for Upgrade -->
    <div id="modal-inventory-picker" class="fixed inset-0 bg-black/70 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-cspanel border border-slate-800 rounded-3xl max-w-2xl w-full p-6 space-y-4 max-h-[85vh] flex flex-col shadow-2xl">
            <div class="flex justify-between items-center">
                <h3 class="text-lg font-bold text-slate-200">Выберите скин для апгрейда</h3>
                <button onclick="closeInventoryPicker()" class="text-slate-400 hover:text-white bg-slate-800 px-3 py-1.5 rounded-xl text-sm">✖ Закрыть</button>
            </div>
            <div id="picker-items-grid" class="grid grid-cols-2 sm:grid-cols-3 gap-3 overflow-y-auto p-2 flex-grow">
                <!-- Populated via JS -->
            </div>
        </div>
    </div>

    <!-- Modal: Result Popup (Win/Lose) -->
    <div id="modal-result" class="fixed inset-0 bg-black/80 backdrop-blur-md z-50 hidden flex items-center justify-center p-4 animate-fade-in">
        <div class="bg-cspanel border border-slate-800 rounded-3xl max-w-md w-full p-8 text-center space-y-6 shadow-2xl relative overflow-hidden">
            <div id="result-glow" class="absolute -top-24 left-1/2 -translate-x-1/2 w-48 h-48 rounded-full blur-3xl opacity-30 pointer-events-none"></div>
            
            <div id="result-icon" class="text-6xl animate-bounce">🎉</div>
            <div class="space-y-2">
                <h3 id="result-title" class="text-2xl font-black text-white">Успех!</h3>
                <p id="result-desc" class="text-slate-300 text-sm">Скин успешно улучшен!</p>
            </div>

            <div id="result-skin-card" class="bg-slate-900/80 p-4 rounded-2xl border border-slate-800 flex items-center gap-4 text-left">
                <!-- Rendered skin preview -->
            </div>

            <button onclick="closeResultModal()" class="w-full bg-gradient-to-r from-blue-600 to-indigo-600 hover:from-blue-500 hover:to-indigo-500 text-white font-bold py-3.5 rounded-2xl shadow-lg transition-all text-base active:scale-95">
                Отлично
            </button>
        </div>
    </div>

    <!-- Message Box Notification Banner -->
    <div id="notification-banner" class="fixed bottom-6 right-6 z-50 translate-y-20 opacity-0 transition-all duration-300 pointer-events-none">
        <div id="notification-content" class="bg-slate-800 border border-slate-700 text-white px-5 py-3 rounded-2xl shadow-2xl flex items-center gap-3 text-sm font-medium">
            <span id="notification-icon">ℹ️</span>
            <span id="notification-text">Уведомление</span>
        </div>
    </div>

    <!-- JavaScript Application Logic -->
    <script>
        const RARITIES = {
            consumer: { name: 'Ширпотреб', color: '#b0c3d9', badgeBg: 'bg-slate-600/20 text-slate-300 border-slate-500/30', border: 'border-slate-500/40' },
            industrial: { name: 'Промышленное', color: '#5e98d9', badgeBg: 'bg-blue-600/20 text-blue-400 border-blue-500/30', border: 'border-blue-500/40' },
            milspec: { name: 'Армейское', color: '#4b69ff', badgeBg: 'bg-indigo-600/20 text-indigo-400 border-indigo-500/30', border: 'border-indigo-500/40' },
            restricted: { name: 'Запрещенное', color: '#8847ff', badgeBg: 'bg-purple-600/20 text-purple-400 border-purple-500/30', border: 'border-purple-500/40' },
            classified: { name: 'Засекреченное', color: '#d32ce6', badgeBg: 'bg-fuchsia-600/20 text-fuchsia-400 border-fuchsia-500/30', border: 'border-fuchsia-500/40' },
            covert: { name: 'Тайное', color: '#eb4b4b', badgeBg: 'bg-rose-600/20 text-rose-400 border-rose-500/30', border: 'border-rose-500/40' },
            special: { name: 'Кинжал / Перчатки', color: '#ffd700', badgeBg: 'bg-amber-500/20 text-amber-400 border-amber-500/30', border: 'border-amber-500/40' }
        };

        const CASES = [
            {
                id: 'spectrum',
                name: 'Кейс «Spectrum»',
                price: 15.00,
                image: 'https://placehold.co/180x180/1e293b/3b82f6?text=Spectrum',
                items: [
                    { name: 'PP-Bizon | Harvester', rarity: 'industrial', price: 1.50 },
                    { name: 'MAC-10 | Last Dive', rarity: 'industrial', price: 1.80 },
                    { name: 'Galil AR | Čarika', rarity: 'milspec', price: 3.20 },
                    { name: 'MP5-SD | Bamboo Garden', rarity: 'milspec', price: 4.50 },
                    { name: 'USP-S | Neo-Noir', rarity: 'restricted', price: 18.00 },
                    { name: 'M4A4 | Buzz Kill', rarity: 'classified', price: 45.00 },
                    { name: 'AK-47 | Bloodsport', rarity: 'covert', price: 145.00 },
                    { name: 'Butterfly Knife | Doppler', rarity: 'special', price: 1250.00 }
                ]
            },
            {
                id: 'gamma',
                name: 'Кейс «Gamma»',
                price: 25.00,
                image: 'https://placehold.co/180x180/1e293b/10b981?text=Gamma',
                items: [
                    { name: 'P250 | Iron Clad', rarity: 'consumer', price: 0.80 },
                    { name: 'Nova | Goggles', rarity: 'industrial', price: 1.20 },
                    { name: 'AUG | Aristocrat', rarity: 'milspec', price: 5.00 },
                    { name: 'P90 | Grim', rarity: 'restricted', price: 12.50 },
                    { name: 'AWP | Asiimov', rarity: 'classified', price: 85.00 },
                    { name: 'M4A1-S | Mecha Industries', rarity: 'covert', price: 110.00 },
                    { name: 'Karambit | Gamma Doppler', rarity: 'special', price: 1800.00 }
                ]
            },
            {
                id: 'weapon_case_1',
                name: 'Оружейный кейс #1',
                price: 5.00,
                image: 'https://placehold.co/180x180/1e293b/f59e0b?text=Weapon+1',
                items: [
                    { name: 'CZ75-Auto | Tuxedo', rarity: 'consumer', price: 0.40 },
                    { name: 'SG 553 | Waves', rarity: 'industrial', price: 0.90 },
                    { name: 'FAMAS | Valence', rarity: 'milspec', price: 2.50 },
                    { name: 'M4A4 | Desert-Strike', rarity: 'restricted', price: 8.50 },
                    { name: 'AK-47 | Redline', rarity: 'classified', price: 28.00 },
                    { name: 'AWP | Lightning Strike', rarity: 'covert', price: 320.00 },
                    { name: 'Bayonet | Fade', rarity: 'special', price: 950.00 }
                ]
            }
        ];

        let state = {
            balance: 1000.00,
            inventory: [],
            currentCase: null,
            isSpinning: false,
            // Upgrade state
            selectedUpgradeSkinIndex: null,
            selectedMultiplier: 2,
            isUpgrading: false
        };

        // Load saved game from localStorage
        function loadGame() {
            const saved = localStorage.getItem('cs2_case_sim_save');
            if (saved) {
                try {
                    const parsed = JSON.parse(saved);
                    state.balance = parsed.balance ?? 1000.00;
                    state.inventory = parsed.inventory ?? [];
                } catch(e) {
                    console.error("Failed to load save", e);
                }
            }
        }

        function saveGame() {
            localStorage.setItem('cs2_case_sim_save', JSON.stringify({
                balance: state.balance,
                inventory: state.inventory
            }));
        }

        function showNotification(text, icon = 'ℹ️') {
            const banner = document.getElementById('notification-banner');
            const textEl = document.getElementById('notification-text');
            const iconEl = document.getElementById('notification-icon');
            
            textEl.textContent = text;
            iconEl.textContent = icon;
            
            banner.classList.remove('translate-y-20', 'opacity-0', 'pointer-events-none');
            setTimeout(() => {
                banner.classList.add('translate-y-20', 'opacity-0', 'pointer-events-none');
            }, 3000);
        }

        function updateUI() {
            document.getElementById('user-balance').textContent = state.balance.toFixed(2) + ' $';
            const invCount = state.inventory.length;
            document.getElementById('inventory-tab-label').textContent = `Инвентарь (${invCount})`;
            
            const totalVal = state.inventory.reduce((sum, item) => sum + item.price, 0);
            document.getElementById('inventory-subtitle').textContent = `Всего предметов: ${invCount} | Общая стоимость: $${totalVal.toFixed(2)}`;
            
            saveGame();
        }

        function addBonusMoney() {
            state.balance += 500.00;
            updateUI();
            showNotification('Баланс пополнен на $500.00!', '🪙');
        }

        function switchTab(tabName) {
            ['cases', 'inventory', 'upgrade'].forEach(t => {
                document.getElementById(`section-${t}`).classList.add('hidden');
                document.getElementById(`tab-${t}`).className = "flex-1 py-3 px-4 rounded-xl font-bold text-sm transition-all text-slate-400 hover:text-white hover:bg-slate-800/50 flex items-center justify-center gap-2";
            });
            
            document.getElementById(`section-${tabName}`).classList.remove('hidden');
            const activeTabBtn = document.getElementById(`tab-${tabName}`);
            activeTabBtn.className = "flex-1 py-3 px-4 rounded-xl font-bold text-sm transition-all bg-blue-600 text-white shadow-md shadow-blue-600/30 flex items-center justify-center gap-2";

            if (tabName === 'inventory') renderInventory();
            if (tabName === 'cases') renderCases();
            if (tabName === 'upgrade') {
                updateUpgradeUI();
                drawWheel(0, 0.5); // Default draw
            }
        }

        function renderCases() {
            const grid = document.getElementById('cases-grid');
            grid.innerHTML = '';

            CASES.forEach(c => {
                const card = document.createElement('div');
                card.className = 'bg-cspanel border border-slate-800 hover:border-slate-700 rounded-3xl p-6 flex flex-col items-center text-center space-y-4 transition-all hover:shadow-xl group';
                card.innerHTML = `
                    <div class="w-36 h-36 relative flex items-center justify-center group-hover:scale-105 transition-transform duration-300">
                        <img src="${c.image}" alt="${c.name}" class="max-h-full drop-shadow-xl rounded-xl" onerror="this.src='https://placehold.co/180x180/1e293b/ffffff?text=Case'">
                    </div>
                    <div>
                        <h3 class="font-bold text-lg text-slate-100">${c.name}</h3>
                        <p class="text-xs text-slate-400 mt-1">${c.items.length} скинов в коллекции</p>
                    </div>
                    <button onclick="openCaseModal('${c.id}')" class="w-full bg-gradient-to-r from-blue-600 to-indigo-600 hover:from-blue-500 hover:to-indigo-500 text-white font-bold py-3 rounded-xl shadow-lg shadow-blue-600/20 transition-all flex items-center justify-center gap-2 active:scale-95">
                        <span>Открыть за</span>
                        <span class="text-amber-300 font-black">$${c.price.toFixed(2)}</span>
                    </button>
                `;
                grid.appendChild(card);
            });
        }

        function openCaseModal(caseId) {
            const c = CASES.find(x => x.id === caseId);
            if (!c) return;
            state.currentCase = c;

            document.getElementById('opening-case-title').textContent = c.name;
            document.getElementById('opening-container').classList.remove('hidden');
            document.getElementById('case-result-info').innerHTML = `<p class="text-slate-400 text-sm">Стоимость открытия: <span class="text-amber-400 font-bold">$${c.price.toFixed(2)}</span></p>`;
            document.getElementById('start-spin-btn').disabled = false;
            document.getElementById('start-spin-btn').textContent = `Открыть за $${c.price.toFixed(2)}`;

            // Populate initial roller items
            setupRollerItems(c, false);
            
            // Scroll smoothly into view
            document.getElementById('opening-container').scrollIntoView({ behavior: 'smooth' });
        }

        function closeCaseOpening() {
            if (state.isSpinning) return;
            document.getElementById('opening-container').classList.add('hidden');
            state.currentCase = null;
        }

        function setupRollerItems(caseObj, randomize = true) {
            const track = document.getElementById('roller-track');
            track.style.transition = 'none';
            track.style.transform = 'translateX(0px)';
            track.innerHTML = '';

            // Generate 60 items for roller spin effect
            const list = [];
            for (let i = 0; i < 60; i++) {
                const randItem = getRandomItemWeighted(caseObj.items);
                list.push(randItem);
            }

            list.forEach((item, index) => {
                const rInfo = RARITIES[item.rarity];
                const el = document.createElement('div');
                el.className = `flex-shrink-0 w-36 h-36 bg-slate-900 border ${rInfo.border} rounded-2xl flex flex-col items-center justify-center p-3 relative shadow-md`;
                el.innerHTML = `
                    <div class="absolute top-2 left-2 px-1.5 py-0.5 rounded text-[9px] font-bold uppercase ${rInfo.badgeBg}">${rInfo.name}</div>
                    <div class="w-16 h-12 flex items-center justify-center my-2">
                        <span class="text-2xl">🔫</span>
                    </div>
                    <div class="text-xs font-semibold text-slate-200 truncate w-full text-center">${item.name}</div>
                    <div class="text-xs font-bold text-amber-400 mt-1">$${item.price.toFixed(2)}</div>
                `;
                el.dataset.index = index;
                track.appendChild(el);
            });

            // Store winning item at index 45
            window.winningItemForSpin = list[45];
        }

        function getRandomItemWeighted(items) {
            // Give weighted probabilities based on rarity
            const weights = { consumer: 50, industrial: 35, milspec: 25, restricted: 12, classified: 5, covert: 2, special: 0.5 };
            let pool = [];
            items.forEach(item => {
                let w = weights[item.rarity] || 1;
                for (let i = 0; i < w * 10; i++) pool.push(item);
            });
            return pool[Math.floor(Math.random() * pool.length)];
        }

        function spinCase() {
            if (state.isSpinning || !state.currentCase) return;
            const c = state.currentCase;

            if (state.balance < c.price) {
                showNotification('Недостаточно средств на балансе!', '❌');
                return;
            }

            state.balance -= c.price;
            updateUI();
            state.isSpinning = true;
            document.getElementById('start-spin-btn').disabled = true;
            document.getElementById('case-result-info').innerHTML = `<p class="text-blue-400 text-sm font-bold animate-pulse">Рулетка крутится...</p>`;

            // Setup new roller with pre-decided winner at index 45
            setupRollerItems(c, true);
            const track = document.getElementById('roller-track');

            // Calculate offset: each card is 144px width + 16px gap = 160px.
            // Center is index 45. Offset = 45 * 160 - container_half_width + card_half_width
            // Container width ~ approx. Let's compute precisely based on element width.
            const cardWidth = 144 + 16; // width + gap
            const winningIndex = 45;
            // Add some random pixel jitter within the card width
            const jitter = (Math.random() * 100) - 50;
            const targetTranslate = -(winningIndex * cardWidth) + (track.parentElement.clientWidth / 2) - (144 / 2) + jitter;

            track.style.transition = 'transform 4.5s cubic-bezier(0.15, 0.85, 0.15, 1)';
            track.style.transform = `translateX(${targetTranslate}px)`;

            setTimeout(() => {
                state.isSpinning = false;
                const wonItem = window.winningItemForSpin;
                
                // Add to inventory with unique ID
                const skinInstance = { ...wonItem, id: Date.now() + Math.random() };
                state.inventory.push(skinInstance);
                updateUI();

                const rInfo = RARITIES[wonItem.rarity];
                document.getElementById('case-result-info').innerHTML = `
                    <div class="animate-bounce">
                        <span class="text-xs uppercase font-bold text-slate-400">Поздравляем! Вы выиграли:</span>
                        <div class="text-lg font-extrabold text-amber-400">${wonItem.name}</div>
                        <div class="text-sm font-bold text-emerald-400">$${wonItem.price.toFixed(2)}</div>
                    </div>
                `;
                document.getElementById('start-spin-btn').disabled = false;
                document.getElementById('start-spin-btn').textContent = `Открыть еще за $${c.price.toFixed(2)}`;
                showNotification(`Вы выиграли ${wonItem.name}!`, '🎉');
            }, 4700);
        }

        function renderInventory() {
            const grid = document.getElementById('inventory-grid');
            grid.innerHTML = '';

            if (state.inventory.length === 0) {
                grid.innerHTML = `
                    <div class="col-span-full py-16 text-center text-slate-500 space-y-2">
                        <div class="text-4xl">🎒</div>
                        <p class="font-medium text-sm">Ваш инвентарь пуст. Открывайте кейсы или получайте бонусные средства!</p>
                    </div>
                `;
                return;
            }

            state.inventory.forEach((item, index) => {
                const rInfo = RARITIES[item.rarity];
                const card = document.createElement('div');
                card.className = `bg-cspanel border ${rInfo.border} rounded-2xl p-4 flex flex-col justify-between space-y-3 relative group hover:border-slate-500 transition-all shadow-md`;
                card.innerHTML = `
                    <div class="absolute top-2 left-2 px-1.5 py-0.5 rounded text-[8px] font-bold uppercase ${rInfo.badgeBg}">${rInfo.name}</div>
                    <div class="w-full h-24 flex items-center justify-center my-2">
                        <span class="text-3xl">🔫</span>
                    </div>
                    <div>
                        <div class="text-xs font-bold text-slate-200 truncate">${item.name}</div>
                        <div class="text-xs font-black text-amber-400 mt-0.5">$${item.price.toFixed(2)}</div>
                    </div>
                    <button onclick="sellSkin(${index})" class="w-full bg-slate-800 hover:bg-rose-600/20 hover:text-rose-400 hover:border-rose-500/40 border border-slate-700 text-slate-300 font-semibold py-2 rounded-xl text-xs transition-all flex items-center justify-center gap-1 active:scale-95">
                        <span>Продать ($${item.price.toFixed(2)})</span>
                    </button>
                `;
                grid.appendChild(card);
            });
        }

        function sellSkin(index) {
            const item = state.inventory[index];
            if (!item) return;

            state.balance += item.price;
            state.inventory.splice(index, 1);
            updateUI();
            renderInventory();
            showNotification(`Скин ${item.name} продан за $${item.price.toFixed(2)}`, '💵');
        }

        function sellAllSkins() {
            if (state.inventory.length === 0) return;
            const total = state.inventory.reduce((sum, item) => sum + item.price, 0);
            state.balance += total;
            state.inventory = [];
            updateUI();
            renderInventory();
            showNotification(`Все скины проданы за $${total.toFixed(2)}`, '💵');
        }

        function setMultiplier(mult) {
            state.selectedMultiplier = mult;
            document.querySelectorAll('.mult-btn').forEach(btn => {
                btn.className = "mult-btn bg-slate-800 hover:bg-slate-700 border border-slate-700 py-2.5 rounded-xl font-bold text-sm transition-all text-blue-400";
            });
            document.getElementById(`mult-${mult}`).className = "mult-btn bg-blue-600 border border-blue-500 py-2.5 rounded-xl font-bold text-sm transition-all text-white shadow-md shadow-blue-600/30";
            
            updateUpgradeUI();
        }

        function openInventoryPicker() {
            const modal = document.getElementById('modal-inventory-picker');
            const pickerGrid = document.getElementById('picker-items-grid');
            pickerGrid.innerHTML = '';

            if (state.inventory.length === 0) {
                pickerGrid.innerHTML = `<div class="col-span-full py-8 text-center text-slate-400 text-sm">Инвентарь пуст</div>`;
                modal.classList.remove('hidden');
                return;
            }

            state.inventory.forEach((item, index) => {
                const rInfo = RARITIES[item.rarity];
                const card = document.createElement('div');
                card.className = `bg-slate-900 border ${rInfo.border} hover:border-blue-500 rounded-2xl p-3 flex flex-col items-center cursor-pointer transition-all space-y-2`;
                card.innerHTML = `
                    <div class="text-2xl my-1">🔫</div>
                    <div class="text-xs font-bold text-slate-200 truncate w-full text-center">${item.name}</div>
                    <div class="text-xs font-bold text-amber-400">$${item.price.toFixed(2)}</div>
                `;
                card.onclick = () => selectSkinForUpgrade(index);
                pickerGrid.appendChild(card);
            });

            modal.classList.remove('hidden');
        }

        function closeInventoryPicker() {
            document.getElementById('modal-inventory-picker').classList.add('hidden');
        }

        function selectSkinForUpgrade(index) {
            state.selectedUpgradeSkinIndex = index;
            closeInventoryPicker();
            updateUpgradeUI();
        }

        function updateUpgradeUI() {
            const container = document.getElementById('upgrade-selected-skin');
            const infoBox = document.getElementById('upgrade-info-box');
            const startBtn = document.getElementById('start-upgrade-btn');

            if (state.selectedUpgradeSkinIndex === null || !state.inventory[state.selectedUpgradeSkinIndex]) {
                container.innerHTML = `<span class="text-slate-400 text-sm">Нажмите, чтобы выбрать скин для ставки</span>`;
                container.className = "bg-slate-800 hover:bg-slate-750 border-2 border-dashed border-slate-700 rounded-2xl p-4 cursor-pointer text-center transition-all flex items-center justify-center min-h-[100px]";
                infoBox.classList.add('hidden');
                startBtn.disabled = true;
                drawWheel(0, 0.5);
                return;
            }

            const skin = state.inventory[state.selectedUpgradeSkinIndex];
            const rInfo = RARITIES[skin.rarity];
            
            container.innerHTML = `
                <div class="flex items-center gap-4 w-full">
                    <div class="w-16 h-16 bg-slate-900 border ${rInfo.border} rounded-xl flex items-center justify-center text-2xl flex-shrink-0">🔫</div>
                    <div class="text-left flex-grow truncate">
                        <div class="text-xs uppercase font-bold text-slate-400">Ставка</div>
                        <div class="text-sm font-bold text-slate-100 truncate">${skin.name}</div>
                        <div class="text-sm font-black text-amber-400">$${skin.price.toFixed(2)}</div>
                    </div>
                    <div class="text-xs text-blue-400 font-semibold underline">Изменить</div>
                </div>
            `;
            container.className = `bg-slate-900 border-2 ${rInfo.border} rounded-2xl p-4 cursor-pointer text-center transition-all flex items-center justify-center`;

            // Calculate chance
            // Multiplier formula: chance = (1 / multiplier) * 100% with house edge e.g. 95% factor
            const mult = state.selectedMultiplier;
            const rawChance = (1 / mult) * 100;
            const chance = Math.min(Math.max(rawChance * 0.95, 1), 95); // max 95%, min 1%

            document.getElementById('up-cost').textContent = `$${skin.price.toFixed(2)}`;
            document.getElementById('up-win').textContent = `$${(skin.price * mult).toFixed(2)}`;
            document.getElementById('up-chance').textContent = `${chance.toFixed(1)}%`;
            
            infoBox.classList.remove('hidden');
            startBtn.disabled = false;

            // Draw wheel with correct green angle
            drawWheel(chance / 100, 0);
        }

        function drawWheel(successProbability, currentAngle) {
            const canvas = document.getElementById('upgrade-wheel');
            if (!canvas) return;
            const ctx = canvas.getContext('2d');
            const width = canvas.width;
            const height = canvas.height;
            const radius = width / 2;

            ctx.clearRect(0, 0, width, height);

            ctx.save();
            ctx.translate(radius, radius);
            ctx.rotate(currentAngle);

            const greenAngle = successProbability * Math.PI * 2;

            // Draw Green Zone (Success)
            ctx.beginPath();
            ctx.moveTo(0, 0);
            ctx.arc(0, 0, radius - 10, -Math.PI / 2, -Math.PI / 2 + greenAngle, false);
            ctx.lineTo(0, 0);
            ctx.fillStyle = '#10b981';
            ctx.fill();
            ctx.strokeStyle = '#059669';
            ctx.lineWidth = 3;
            ctx.stroke();

            // Draw Red Zone (Failure)
            ctx.beginPath();
            ctx.moveTo(0, 0);
            ctx.arc(0, 0, radius - 10, -Math.PI / 2 + greenAngle, -Math.PI / 2 + Math.PI * 2, false);
            ctx.lineTo(0, 0);
            ctx.fillStyle = '#ef4444';
            ctx.fill();
            ctx.strokeStyle = '#dc2626';
            ctx.lineWidth = 3;
            ctx.stroke();

            // Inner dark circle for style
            ctx.beginPath();
            ctx.arc(0, 0, radius * 0.35, 0, Math.PI * 2);
            ctx.fillStyle = '#0f172a';
            ctx.fill();
            ctx.strokeStyle = '#334155';
            ctx.lineWidth = 4;
            ctx.stroke();

            ctx.restore();
        }

        function startUpgradeSpin() {
            if (state.isUpgrading || state.selectedUpgradeSkinIndex === null) return;
            const skinIndex = state.selectedUpgradeSkinIndex;
            const skin = state.inventory[skinIndex];
            if (!skin) return;

            state.isUpgrading = true;
            document.getElementById('start-upgrade-btn').disabled = true;

            const mult = state.selectedMultiplier;
            const rawChance = (1 / mult) * 100;
            const chance = Math.min(Math.max(rawChance * 0.95, 1), 95); // 0 to 1 ratio is chance / 100
            const prob = chance / 100;

            // Determine if win or loss beforehand
            const isSuccess = Math.random() < prob;

            // Wheel rotation animation setup
            const startAngle = 0;
            // Target angle calculation so pointer (top, -PI/2) lands in green or red zone
            // Green zone spans from -PI/2 to -PI/2 + prob * 2*PI
            let targetStopAngle;
            if (isSuccess) {
                // Random angle inside green zone (with 10% margin from edges)
                const margin = prob * 0.2;
                const minA = prob * margin;
                const maxA = prob * (1 - margin);
                const randomSpot = minA + Math.random() * (maxA - minA);
                targetStopAngle = -randomSpot * Math.PI * 2;
            } else {
                // Random angle inside red zone
                const redFraction = 1 - prob;
                const margin = redFraction * 0.15;
                const minA = prob + redFraction * margin;
                const maxA = 1 - redFraction * margin;
                const randomSpot = minA + Math.random() * (maxA - minA);
                targetStopAngle = -randomSpot * Math.PI * 2;
            }

            // Full rotations (e.g. 6 full turns = 12 * PI) + target
            const totalSpins = (Math.PI * 2) * 6;
            const finalAngle = totalSpins + targetStopAngle;

            const duration = 4000; // 4 seconds
            const startTime = performance.now();

            function animateWheel(currentTime) {
                const elapsed = currentTime - startTime;
                const progress = Math.min(elapsed / duration, 1);
                
                // Ease out cubic
                const easeProgress = 1 - Math.pow(1 - progress, 3);
                const currentRot = startAngle + (finalAngle - startAngle) * easeProgress;

                drawWheel(prob, currentRot);

                if (progress < 1) {
                    requestAnimationFrame(animateWheel);
                } else {
                    // Animation finished
                    state.isUpgrading = false;
                    
                    // Remove skin from inventory (bet consumed)
                    state.inventory.splice(skinIndex, 1);
                    state.selectedUpgradeSkinIndex = null;

                    if (isSuccess) {
                        // Create upgraded prize item
                        const prizeName = `${skin.name} (Upgraded)`;
                        const prizePrice = skin.price * mult;
                        // Find appropriate rarity or keep same/higher
                        const prizeItem = { name: prizeName, rarity: skin.rarity, price: prizePrice, id: Date.now() };
                        state.inventory.push(prizeItem);
                        
                        showResultModal(true, prizeItem, mult);
                    } else {
                        showResultModal(false, skin, mult);
                    }

                    updateUI();
                    updateUpgradeUI();
                }
            }

            requestAnimationFrame(animateWheel);
        }

        function showResultModal(isSuccess, skin, mult) {
            const modal = document.getElementById('modal-result');
            const icon = document.getElementById('result-icon');
            const title = document.getElementById('result-title');
            const desc = document.getElementById('result-desc');
            const glow = document.getElementById('result-glow');
            const card = document.getElementById('result-skin-card');

            const rInfo = RARITIES[skin.rarity];

            if (isSuccess) {
                icon.textContent = '🎉';
                title.textContent = 'Успех!';
                title.className = 'text-2xl font-black text-emerald-400';
                desc.textContent = `Поздравляем! Колесо остановилось в зеленой зоне. Скин успешно улучшен в ${mult}x!`;
                glow.className = 'absolute -top-24 left-1/2 -translate-x-1/2 w-48 h-48 rounded-full blur-3xl opacity-40 pointer-events-none bg-emerald-500';
                showNotification('Успешный апгрейд!', '⭐');
            } else {
                icon.textContent = '💀';
                title.textContent = 'Неудача';
                title.className = 'text-2xl font-black text-rose-500';
                desc.textContent = 'К сожалению, стрелка остановилась в красной зоне. Ставка сгорела.';
                glow.className = 'absolute -top-24 left-1/2 -translate-x-1/2 w-48 h-48 rounded-full blur-3xl opacity-40 pointer-events-none bg-rose-500';
                showNotification('Апгрейд не удался', '❌');
            }

            card.innerHTML = `
                <div class="w-14 h-14 bg-slate-900 border ${rInfo.border} rounded-xl flex items-center justify-center text-2xl flex-shrink-0">🔫</div>
                <div class="truncate">
                    <div class="text-xs font-bold text-slate-400 uppercase">${rInfo.name}</div>
                    <div class="text-sm font-bold text-slate-100 truncate">${skin.name}</div>
                    <div class="text-sm font-black text-amber-400">$${skin.price.toFixed(2)}</div>
                </div>
            `;

            modal.classList.remove('hidden');
        }

        function closeResultModal() {
            document.getElementById('modal-result').classList.add('hidden');
        }

        // Initialize App on Load
        window.onload = function() {
            loadGame();
            updateUI();
            renderCases();
            drawWheel(0.5, 0);
        };
    </script>
</body>
</html>
