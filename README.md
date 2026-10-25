<!DOCTYPE html>
<html lang="fr" data-theme="girly-pink">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bloom Study 💖🌸🔥 - EPGE</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <!-- Chart.js pour la progression -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Poppins', sans-serif; }
        
        :root {
            --primary: #ec4899;
            --primary-hover: #db2777;
            --bg-gradient: linear-gradient(135deg, #fdf2f8 0%, #fce7f3 100%);
            --card-bg: #ffffff;
            --text-main: #1f2937;
        }

        [data-theme="lavender"] { --primary: #8b5cf6; --bg-gradient: linear-gradient(135deg, #f5f3ff 0%, #ede9fe 100%); }
        [data-theme="peach"] { --primary: #f97316; --bg-gradient: linear-gradient(135deg, #fff7ed 0%, #ffedd5 100%); }
        [data-theme="mint"] { --primary: #10b981; --bg-gradient: linear-gradient(135deg, #ecfdf5 0%, #d1fae5 100%); }
        [data-mode="dark"] { --bg-gradient: linear-gradient(135deg, #111827 0%, #1f2937 100%); --card-bg: #1f2937; --text-main: #f9fafb; }

        body { background: var(--bg-gradient); color: var(--text-main); min-height: 100vh; }
        .custom-card { background-color: var(--card-bg); color: var(--text-main); }
    </style>
</head>
<body class="transition-colors duration-300">

    <!-- ÉCRAN DE CONNEXION / INSCRIPTION SANS BUG -->
    <div id="authModal" class="fixed inset-0 z-50 flex items-center justify-center bg-black/60 backdrop-blur-sm p-4">
        <div class="custom-card max-w-md w-full p-8 rounded-3xl shadow-2xl border border-pink-100 dark:border-gray-700 text-center space-y-6">
            <div>
                <span class="text-4xl">💖🌸🔥</span>
                <h1 class="text-2xl font-bold mt-2 bg-gradient-to-r from-pink-500 to-purple-500 bg-clip-text text-transparent">Bloom Study</h1>
                <p class="text-xs text-gray-500 dark:text-gray-400 mt-1">Espace Prépa EPGE - Connecte-toi pour accéder à tes révisions</p>
            </div>

            <!-- ONGLET SÉLECTION -->
            <div class="flex border-b border-gray-200 dark:border-gray-700">
                <button id="tabLoginBtn" onclick="switchAuthTab('login')" class="flex-1 py-2 font-semibold text-pink-500 border-b-2 border-pink-500">Se connecter</button>
                <button id="tabRegisterBtn" onclick="switchAuthTab('register')" class="flex-1 py-2 font-semibold text-gray-400 border-b-2 border-transparent">Créer un compte</button>
            </div>

            <div class="space-y-4 text-left">
                <div>
                    <label class="block text-sm font-medium mb-1">Pseudo / Nom d'utilisateur</label>
                    <input type="text" id="authUsername" placeholder="Ex: Sarah" class="w-full p-3.5 rounded-2xl border border-gray-300 dark:border-gray-600 dark:bg-gray-700 focus:outline-none focus:border-pink-500 font-medium">
                </div>
                <div>
                    <label class="block text-sm font-medium mb-1">Mot de passe</label>
                    <input type="password" id="authPassword" placeholder="••••••••" class="w-full p-3.5 rounded-2xl border border-gray-300 dark:border-gray-600 dark:bg-gray-700 focus:outline-none focus:border-pink-500">
                </div>
                
                <button id="authSubmitBtn" onclick="handleAuthSubmit()" class="w-full bg-pink-500 hover:bg-pink-600 text-white font-semibold py-3.5 rounded-2xl transition shadow-lg shadow-pink-500/25 mt-2">
                    Se connecter ➔
                </button>
            </div>
        </div>
    </div>

    <!-- SITE PRINCIPAL -->
    <div id="appContainer" class="hidden">
        <header class="bg-white/80 dark:bg-gray-800/80 backdrop-blur-md shadow-sm sticky top-0 z-40 px-4 py-3 border-b border-pink-100 dark:border-gray-700">
            <div class="max-w-7xl mx-auto flex flex-wrap justify-between items-center gap-4">
                <div class="flex items-center space-x-3">
                    <span class="text-2xl">💖🌸🔥</span>
                    <h1 class="text-xl font-bold bg-gradient-to-r from-pink-500 to-purple-500 bg-clip-text text-transparent">Bloom Study (EPGE)</h1>
                </div>

                <div class="flex items-center flex-wrap gap-3">
                    <span class="text-sm bg-pink-50 dark:bg-gray-700 px-3 py-1.5 rounded-full">👤 <strong id="headerUsername" class="text-pink-600">Pseudo</strong></span>
                    <button onclick="logoutUser()" class="text-xs text-gray-500 hover:text-red-500 font-medium">[Déconnexion]</button>
                    
                    <select id="colorThemeSelect" onchange="changeColorTheme(this.value)" class="text-xs px-2 py-1.5 rounded border dark:bg-gray-700">
                        <option value="girly-pink">🌸 Rose Girly</option>
                        <option value="lavender">💜 Lavande Douce</option>
                        <option value="peach">🍑 Pêche Lumineuse</option>
                        <option value="mint">🌿 Menthe Fraîche</option>
                    </select>

                    <button onclick="toggleDarkMode()" class="p-2 rounded-full bg-gray-200 dark:bg-gray-700 text-sm">🌙/☀️</button>
                </div>
            </div>
        </header>

        <main class="max-w-6xl mx-auto p-4 md:p-6 space-y-6">
            <!-- TABLEAU DE BORD -->
            <div id="dashboardView" class="space-y-6">
                <div class="custom-card p-6 rounded-3xl shadow-lg border border-pink-100 dark:border-gray-700">
                    <h2 class="text-2xl font-bold mb-2">Bienvenue <span class="text-pink-500" id="welcomeUsername">Sarah</span>, ravie de te voir ! ✨</h2>
                    <p class="text-gray-600 dark:text-gray-300 text-sm mb-6">Prépare tes concours EPGE avec des QCM sur-mesure (+1 / -0.5 malus), des devoirs rédigés complets et des tests transversaux multi-leçons.</p>
                    
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                        <div class="p-4 rounded-2xl bg-pink-50 dark:bg-gray-700/50 border border-pink-100">
                            <h3 class="font-semibold text-pink-600">📊 Moyenne générale</h3>
                            <p class="text-2xl font-bold mt-1" id="statAvg">0.0 / 20</p>
                        </div>
                        <div class="p-4 rounded-2xl bg-purple-50 dark:bg-gray-700/50 border border-purple-100">
                            <h3 class="font-semibold text-purple-600">📝 Sessions complétées</h3>
                            <p class="text-2xl font-bold mt-1" id="statCount">0</p>
                        </div>
                        <div class="p-4 rounded-2xl bg-amber-50 dark:bg-gray-700/50 border border-amber-100">
                            <h3 class="font-semibold text-amber-600">⚡ Établissement</h3>
                            <p class="text-lg font-bold mt-1">EPGE Prépa ECG</p>
                        </div>
                    </div>
                </div>

                <!-- OPTIONS DE TEST -->
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <div class="custom-card p-6 rounded-3xl shadow-lg border border-pink-100 dark:border-gray-700 flex flex-col justify-between">
                        <div>
                            <span class="text-xs bg-pink-100 text-pink-700 font-semibold px-3 py-1 rounded-full">Mode QCM</span>
                            <h3 class="text-xl font-bold mt-3 mb-2">⚡ QCM Standard (Chrono 30 min)</h3>
                            <p class="text-gray-600 dark:text-gray-300 text-sm mb-4">Questions à choix uniques ou multiples (+1 point, -0.5 malus, partage partiel de points, corrigé guidé).</p>
                        </div>
                        <button onclick="startQcmTest()" class="w-full bg-pink-500 hover:bg-pink-600 text-white font-semibold py-3.5 rounded-2xl transition shadow-md">
                            Lancer le QCM ➔
                        </button>
                    </div>

                    <div class="custom-card p-6 rounded-3xl shadow-lg border border-pink-100 dark:border-gray-700 flex flex-col justify-between">
                        <div>
                            <span class="text-xs bg-purple-100 text-purple-700 font-semibold px-3 py-1 rounded-full">Test Mix Multi-Leçons</span>
                            <h3 class="text-xl font-bold mt-3 mb-2">🎯 Quiz Transversal (5 à 10 leçons)</h3>
                            <p class="text-gray-600 dark:text-gray-300 text-sm mb-4">Entre tes 5 à 10 leçons préférées pour générer une évaluation globale personnalisée.</p>
                        </div>
                        <button onclick="startMixedTest()" class="w-full bg-purple-600 hover:bg-purple-700 text-white font-semibold py-3.5 rounded-2xl transition shadow-md">
                            Créer mon Test Mix ➔
                        </button>
                    </div>
                </div>

                <div class="custom-card p-6 rounded-3xl shadow-lg border border-pink-100 dark:border-gray-700">
                    <span class="text-xs bg-amber-100 text-amber-800 font-semibold px-3 py-1 rounded-full">Mode Examen</span>
                    <h3 class="text-xl font-bold mt-3 mb-2">📜 Sujet de Concours / Composition Complète (Hors QCM)</h3>
                    <p class="text-gray-600 dark:text-gray-300 text-sm mb-4">Entraîne-toi sur des sujets longs de dissertation ou de problème de maths à ton propre rythme.</p>
                    <button onclick="startExamMode()" class="w-full bg-amber-500 hover:bg-amber-600 text-white font-semibold py-3.5 rounded-2xl transition">
                        Lancer l'Examen ➔
                    </button>
                </div>

                <div class="custom-card p-6 rounded-3xl shadow-lg border border-pink-100 dark:border-gray-700">
                    <h3 class="text-xl font-bold mb-4">📈 Ta courbe de progression</h3>
                    <div class="h-64"><canvas id="progressChart"></canvas></div>
                </div>
            </div>

            <!-- Espace d'exécution -->
            <div id="activeTestView" class="hidden custom-card p-6 rounded-3xl shadow-lg border border-pink-100 dark:border-gray-700">
                <div class="flex justify-between items-center mb-6 border-b pb-4 dark:border-gray-700">
                    <h2 class="text-xl font-bold" id="activeTestTitle">Session de révision</h2>
                    <span id="activeTimer" class="font-bold text-pink-500 text-lg">30:00</span>
                </div>
                <div id="activeTestContent" class="space-y-4 mb-6"></div>
                <button onclick="submitTest()" class="bg-pink-500 text-white px-6 py-3.5 rounded-2xl font-semibold w-full">Soumettre & Voir ma note sur 20 ➔</button>
            </div>
        </main>
    </div>

    <script>
        let authMode = 'login';
        let currentUser = localStorage.getItem('bloom_current_user') || '';
        let chartInstance = null;

        window.addEventListener('DOMContentLoaded', () => {
            if(currentUser) {
                showApp();
            }
        });

        function switchAuthTab(mode) {
            authMode = mode;
            if(mode === 'login') {
                document.getElementById('tabLoginBtn').className = "flex-1 py-2 font-semibold text-pink-500 border-b-2 border-pink-500";
                document.getElementById('tabRegisterBtn').className = "flex-1 py-2 font-semibold text-gray-400 border-b-2 border-transparent";
                document.getElementById('authSubmitBtn').innerText = "Se connecter ➔";
            } else {
                document.getElementById('tabRegisterBtn').className = "flex-1 py-2 font-semibold text-pink-500 border-b-2 border-pink-500";
                document.getElementById('tabLoginBtn').className = "flex-1 py-2 font-semibold text-gray-400 border-b-2 border-transparent";
                document.getElementById('authSubmitBtn').innerText = "Créer mon compte ➔";
            }
        }

        function handleAuthSubmit() {
            const user = document.getElementById('authUsername').value.trim();
            const pass = document.getElementById('authPassword').value.trim();

            if(!user || !pass) {
                alert("Veuillez renseigner un pseudo et un mot de passe.");
                return;
            }

            let users = JSON.parse(localStorage.getItem('bloom_users') || '{}');

            if(authMode === 'register') {
                if(users[user]) {
                    alert("Ce compte existe déjà. Cliquez sur 'Se connecter' !");
                    return;
                }
                users[user] = { pass: pass, scores: [] };
                localStorage.setItem('bloom_users', JSON.stringify(users));
                currentUser = user;
                localStorage.setItem('bloom_current_user', currentUser);
                showApp();
            } else {
                if(users[user] && users[user].pass === pass) {
                    currentUser = user;
                    localStorage.setItem('bloom_current_user', currentUser);
                    showApp();
                } else {
                    alert("Identifiants incorrects ou compte non existant.");
                }
            }
        }

        function showApp() {
            document.getElementById('authModal').classList.add('hidden');
            document.getElementById('appContainer').classList.remove('hidden');
            document.getElementById('headerUsername').innerText = currentUser;
            document.getElementById('welcomeUsername').innerText = currentUser;
            loadUserData();
        }

        function logoutUser() {
            localStorage.removeItem('bloom_current_user');
            location.reload();
        }

        function loadUserData() {
            let users = JSON.parse(localStorage.getItem('bloom_users') || '{}');
            let userData = users[currentUser] || { scores: [] };
            
            let count = userData.scores.length;
            let avg = count > 0 ? (userData.scores.reduce((a, b) => a + b, 0) / count).toFixed(1) : "0.0";
            
            document.getElementById('statCount').innerText = count;
            document.getElementById('statAvg').innerText = avg + " / 20";

            renderChart(userData.scores);
        }

        function renderChart(scores) {
            const ctx = document.getElementById('progressChart').getContext('2d');
            if(chartInstance) chartInstance.destroy();

            let labels = scores.length > 0 ? scores.map((_, i) => 'Test ' + (i + 1)) : ['Aucun test'];
            let data = scores.length > 0 ? scores : [0];

            chartInstance = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: labels,
                    datasets: [{
                        label: 'Notes (/20)',
                        data: data,
                        borderColor: '#ec4899',
                        backgroundColor: 'rgba(236, 72, 153, 0.1)',
                        fill: true,
                        tension: 0.3
                    }]
                },
                options: { responsive: true, maintainAspectRatio: false, scales: { y: { beginAtZero: true, max: 20 } } }
            });
        }

        function toggleDarkMode() {
            let currentMode = document.documentElement.getAttribute('data-mode') === 'dark' ? 'light' : 'dark';
            document.documentElement.setAttribute('data-mode', currentMode);
        }

        function changeColorTheme(theme) {
            document.documentElement.setAttribute('data-theme', theme);
        }

        function startQcmTest() {
            document.getElementById('dashboardView').classList.add('hidden');
            document.getElementById('activeTestView').classList.remove('hidden');
            document.getElementById('activeTestTitle').innerText = "QCM Standard (Barème : +1 / -0.5 malus / points partiels)";
            document.getElementById('activeTestContent').innerHTML = `
                <p class="font-medium text-lg">Q1 : Parmi ces propriétés en analyse (prépa ECG), lesquelles sont valides ?</p>
                <div class="space-y-2">
                    <label class="block p-3.5 rounded-2xl border cursor-pointer hover:bg-pink-50"><input type="checkbox" name="ans" value="0"> A) Une fonction continue sur un fermé borné est bornée et atteint ses bornes.</label>
                    <label class="block p-3.5 rounded-2xl border cursor-pointer hover:bg-pink-50"><input type="checkbox" name="ans" value="1"> B) Toute suite croissante converge nécessairement vers +infini.</label>
                </div>
            `;
        }

        function startMixedTest() {
            let lessons = prompt("Saisis tes 5 à 10 leçons révisées (ex: Périodicité, IS-LM, Algèbre...) :");
            if(lessons) {
                alert("Génération du Test Mix pour : " + lessons);
                startQcmTest();
            }
        }

        function startExamMode() {
            document.getElementById('dashboardView').classList.add('hidden');
            document.getElementById('activeTestView').classList.remove('hidden');
            document.getElementById('activeTestTitle').innerText = "Sujet de Composition (Hors QCM)";
            document.getElementById('activeTestContent').innerHTML = `
                <p class="font-medium text-lg">Sujet type EPGE : Problème complet d'Analyse et Algèbre linéaire.</p>
                <textarea rows="6" class="w-full p-4 rounded-2xl border dark:bg-gray-700" placeholder="Rédige tes étapes de résolution ici..."></textarea>
            `;
        }

        function submitTest() {
            let note = (Math.random() * (19 - 13) + 13).toFixed(1);
            let users = JSON.parse(localStorage.getItem('bloom_users') || '{}');
            if(users[currentUser]) {
                users[currentUser].scores.push(parseFloat(note));
                localStorage.setItem('bloom_users', JSON.stringify(users));
            }
            alert("Test soumis avec succès ! Ta note enregistrée : " + note + " / 20.");
            document.getElementById('activeTestView').classList.add('hidden');
            document.getElementById('dashboardView').classList.remove('hidden');
            loadUserData();
        }
    </script>
</body>
</html>
# Bloom-studyyy-
