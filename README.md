<!DOCTYPE html>
<html lang="ru" data-theme="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>заОкном — Полная версия (Fixed)</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        /* ================= ПЕРЕМЕННЫЕ ТЕМ ================= */
        :root[data-theme="dark"] {
            --bg-color: #000;
            --text-color: #fff;
            --glass-bg: rgba(255, 255, 255, 0.03);
            --glass-border: rgba(255, 255, 255, 0.1);
            --card-sub-bg: rgba(255, 255, 255, 0.02);
            --gradient-start: #1e293b;
            --accent-color: #60a5fa;
            --tab-bg: rgba(255, 255, 255, 0.05);
            --opacity-low: 0.4;
        }

        :root[data-theme="light"] {
            --bg-color: #f1f5f9;
            --text-color: #0f172a;
            --glass-bg: rgba(255, 255, 255, 0.7);
            --glass-border: rgba(15, 23, 42, 0.1);
            --card-sub-bg: rgba(15, 23, 42, 0.05);
            --gradient-start: #e2e8f0;
            --accent-color: #2563eb;
            --tab-bg: rgba(15, 23, 42, 0.05);
            --opacity-low: 0.6;
        }

        /* ================= ГЛОБАЛЬНЫЕ СТИЛИ ================= */
        * { margin:0; padding:0; box-sizing:border-box; -webkit-font-smoothing: antialiased; }

        body {
            font-family: 'Inter', sans-serif;
            background: var(--bg-color);
            color: var(--text-color);
            min-height: 100vh;
            overflow-x: hidden;
            line-height: 1.5;
            transition: background 0.5s ease, color 0.5s ease;
        }

        .bg-overlay {
            position: fixed;
            inset: 0;
            z-index: -1;
            background: radial-gradient(circle at 20% 30%, var(--gradient-start) 0%, var(--bg-color) 100%);
        }

        #particles { position: fixed; inset: 0; z-index: 1; pointer-events: none; }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 50px 20px;
            position: relative;
            z-index: 2;
        }

        header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 60px;
            position: relative;
        }
        .header-center { position: absolute; left: 50%; transform: translateX(-50%); text-align: center; }

        h1 {
            font-size: 5.5rem;
            font-weight: 800;
            letter-spacing: -4px;
            background: linear-gradient(135deg, var(--text-color) 0%, var(--accent-color) 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            margin-bottom: 5px;
        }
        .city-label { font-size: 1.5rem; opacity: var(--opacity-low); font-weight: 300; letter-spacing: 2px; text-transform: uppercase; }

        .theme-switch-container {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-left: auto;
            background: var(--glass-bg);
            padding: 10px 20px;
            border-radius: 20px;
            border: 1px solid var(--glass-border);
            backdrop-filter: blur(10px);
            z-index: 10;
        }

        .glass-card {
            background: var(--glass-bg);
            backdrop-filter: blur(40px);
            -webkit-backdrop-filter: blur(40px);
            border: 1px solid var(--glass-border);
            border-radius: 40px;
            padding: 45px;
            margin-bottom: 30px;
        }

        .current-hero {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 40px;
        }

        .temp-block { display: flex; flex-direction: column; }
        .main-temp { font-size: 10rem; font-weight: 200; line-height: 0.9; letter-spacing: -6px; }
        .hero-feels { font-size: 1.6rem; opacity: var(--opacity-low); font-weight: 300; margin-top: 10px; padding-left: 10px; }

        .main-meta { display: flex; flex-direction: column; align-items: center; text-align: center; min-width: 200px; }
        .hero-emoji { font-size: 7.5rem; line-height: 1; margin-bottom: 10px; }
        .hero-status { font-size: 2.2rem; font-weight: 600; color: var(--accent-color); }

        .today-graph-section { border-top: 1px solid var(--glass-border); padding-top: 40px; }
        .section-title { text-align: center; font-size: 1.1rem; text-transform: uppercase; letter-spacing: 3px; opacity: var(--opacity-low); margin-bottom: 25px; }

        .stats-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 25px;
            margin-bottom: 50px;
        }
        .stat-item {
            background: var(--card-sub-bg);
            border: 1px solid var(--glass-border);
            border-radius: 32px;
            padding: 35px;
            text-align: center;
        }
        .stat-icon { font-size: 2.8rem; margin-bottom: 10px; display: block; }
        .stat-val { font-size: 2.4rem; font-weight: 700; color: var(--text-color); }
        .stat-label { font-size: 1rem; opacity: var(--opacity-low); text-transform: uppercase; margin-top: 5px; }
        .wind-dir-text { font-size: 0.9rem; color: var(--accent-color); font-weight: 600; display: block; margin-top: 5px; }

        .tabs-container { display: flex; justify-content: center; gap: 12px; margin-bottom: 40px; flex-wrap: wrap; }
        .tab-trigger {
            background: var(--tab-bg);
            border: 1px solid var(--glass-border);
            color: var(--text-color);
            padding: 14px 30px;
            border-radius: 24px;
            cursor: pointer;
            font-size: 1.1rem;
            transition: 0.4s;
        }
        .tab-trigger.active { background: var(--accent-color); color: #fff; box-shadow: 0 10px 25px rgba(59, 130, 246, 0.4); }

        /* КАРТОЧКИ ДНЯ */
        .forecast-grid {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 15px;
        }
        .day-card {
            flex: 0 1 140px;
            background: var(--glass-bg);
            border: 1px solid var(--glass-border);
            border-radius: 28px;
            padding: 25px 15px;
            text-align: center;
            cursor: pointer;
            transition: 0.3s ease;
            color: var(--text-color);
        }
        .day-card:hover { border-color: var(--accent-color); }
        .day-card.active { background: var(--accent-color); border-color: var(--accent-color); transform: scale(1.05); color: #fff; }
        .day-card.active * { color: #fff !important; opacity: 1 !important; }

        .card-day { font-size: 0.9rem; opacity: var(--opacity-low); margin-bottom: 12px; font-weight: 700; text-transform: uppercase; }
        .card-t-max { font-size: 2.2rem; font-weight: 800; display: block; }
        .card-t-min { font-size: 1.2rem; opacity: var(--opacity-low); font-weight: 500; }

        /* ДЕТАЛЬНАЯ ПАНЕЛЬ */
        .detail-pane {
            width: 100%;
            margin: 15px 0;
            background: var(--glass-bg);
            border: 1px solid var(--accent-color);
            border-radius: 35px;
            padding: 40px;
            display: none;
            animation: slideDown 0.5s ease;
            order: 0;
            backdrop-filter: blur(20px);
        }
        @keyframes slideDown { from { opacity: 0; transform: translateY(-20px); } to { opacity: 1; transform: translateY(0); } }

        .pane-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 35px;
            border-bottom: 1px solid var(--glass-border);
            padding-bottom: 20px;
        }
        .ps-item { text-align: left; margin-right: 30px; }
        .ps-label { font-size: 0.8rem; opacity: 0.4; text-transform: uppercase; }
        .ps-val { font-size: 1.6rem; font-weight: 700; color: var(--accent-color); }

        .switch-group {
            display: flex;
            align-items: center;
            gap: 15px;
        }

        .switch { position: relative; display: inline-block; width: 54px; height: 28px; }
        .switch input { opacity: 0; width: 0; height: 0; }
        .slider { position: absolute; cursor: pointer; inset: 0; background-color: var(--tab-bg); transition: .4s; border-radius: 34px; border: 1px solid var(--glass-border); }
        .slider:before { position: absolute; content: ""; height: 20px; width: 20px; left: 4px; bottom: 3px; background-color: var(--text-color); transition: .4s; border-radius: 50%; }
        input:checked + .slider { background-color: var(--accent-color); }
        input:checked + .slider:before { transform: translateX(26px); background-color: #fff; }

        @media (max-width: 1000px) {
            header { flex-direction: column; gap: 20px; }
            .header-center { position: static; transform: none; }
            .stats-grid { grid-template-columns: 1fr; }
            .current-hero { flex-direction: column; gap: 30px; }
            .pane-header { flex-direction: column; align-items: flex-start; gap: 20px; }
            .switch-group { width: 100%; justify-content: space-between; }
        }
    </style>
</head>
<body data-theme="dark">

    <div class="bg-overlay"></div>
    <canvas id="particles"></canvas>

    <div class="container">
        <header>
            <div class="header-center">
                <h1>заОкном</h1>
                <div class="city-label">Курган</div>
            </div>

            <div class="theme-switch-container">
                <span style="font-size: 0.8rem; font-weight: 700; text-transform: uppercase;">Тема</span>
                <label class="switch">
                    <input type="checkbox" id="theme-toggle" onchange="toggleTheme()">
                    <span class="slider"></span>
                </label>
            </div>
        </header>

        <div class="glass-card">
            <div class="current-hero">
                <div class="temp-block">
                    <div class="main-temp"><span id="t-now">--</span>°</div>
                    <div class="hero-feels" id="f-now">Ощущается как --°</div>
                </div>
                <div class="main-meta">
                    <span class="hero-emoji" id="e-now">☀️</span>
                    <div class="hero-status" id="s-now">Загрузка...</div>
                </div>
            </div>

            <div class="today-graph-section">
                <div class="section-title">Прогноз по часам</div>
                <div style="height: 300px;"><canvas id="mainChart"></canvas></div>
            </div>
        </div>

        <div class="stats-grid">
            <div class="stat-item">
                <span class="stat-icon" id="wind-dir-icon" style="display:inline-block;">💨</span>
                <div class="stat-val"><span id="wind-s">0</span> м/с</div>
                <span class="wind-dir-text" id="wind-dir-name">--</span>
                <div class="stat-label">Ветер</div>
            </div>
            <div class="stat-item">
                <span class="stat-icon">🌡️</span>
                <div class="stat-val"><span id="press-v">0</span> мм</div>
                <div class="stat-label">Давление</div>
            </div>
            <div class="stat-item">
                <span class="stat-icon">💧</span>
                <div class="stat-val"><span id="hum-v">0</span>%</div>
                <div class="stat-label">Влажность</div>
            </div>
        </div>

        <div class="tabs-container">
            <button class="tab-trigger" onclick="switchPeriod(1, this)">Завтра</button>
            <button class="tab-trigger" onclick="switchPeriod(3, this)">3 дня</button>
            <button class="tab-trigger" onclick="switchPeriod(7, this)">7 дней</button>
            <button class="tab-trigger" onclick="switchPeriod(10, this)">10 дней</button>
            <button class="tab-trigger active" onclick="switchPeriod(14, this)">2 недели</button>
        </div>

        <div class="forecast-grid" id="main-grid"></div>

        <div id="fixed-detail-pane" class="detail-pane">
            <div class="pane-header">
                <div style="display:flex;">
                    <div class="ps-item"><span class="ps-label">Днем</span><span id="p-day-t" class="ps-val">--</span></div>
                    <div class="ps-item"><span class="ps-label">Ночью</span><span id="p-night-t" class="ps-val">--</span></div>
                </div>
                <div class="switch-group">
                    <span style="font-size: 0.9rem; opacity: 0.5;">Температура по ощущениям</span>
                    <label class="switch"><input type="checkbox" id="feel-toggle"><span class="slider"></span></label>
                </div>
            </div>
            <div style="height: 350px;"><canvas id="subChart"></canvas></div>
        </div>
    </div>

    <script>
        const LAT = 55.45, LON = 65.34;
        let globalData = null;
        let mainChart = null;
        let secondaryChart = null;
        let currentOpenIdx = null;

        const WEATHER_CODES = {
            0: { t: "Ясно", e: "☀️" }, 1: { t: "Ясно", e: "🌤️" }, 2: { t: "Облачно", e: "⛅" },
            3: { t: "Пасмурно", e: "☁️" }, 45: { t: "Туман", e: "🌫️" }, 61: { t: "Дождь", e: "🌧️" },
            71: { t: "Снег", e: "❄️" }, 95: { t: "Гроза", e: "⛈️" }
        };

        function initTheme() {
            const savedTheme = localStorage.getItem('theme');
            const theme = savedTheme || 'dark';
            document.documentElement.setAttribute('data-theme', theme);
            document.getElementById('theme-toggle').checked = (theme === 'light');
        }

        function toggleTheme() {
            const isLight = document.getElementById('theme-toggle').checked;
            const newTheme = isLight ? 'light' : 'dark';
            document.documentElement.setAttribute('data-theme', newTheme);
            localStorage.setItem('theme', newTheme);
            if (globalData) {
                renderMainGraph();
                if (currentOpenIdx !== null) refreshSubChart(currentOpenIdx, document.getElementById('feel-toggle').checked);
            }
        }

        function getWindDir(deg) {
            const sectors = ["Северный", "С-В", "Восточный", "Ю-В", "Южный", "Ю-З", "Западный", "С-З"];
            return sectors[Math.round(deg / 45) % 8];
        }

        async function startApp() {
            initTheme();
            try {
                const api = `https://api.open-meteo.com/v1/forecast?latitude=${LAT}&longitude=${LON}&current=temperature_2m,apparent_temperature,weather_code,wind_speed_10m,wind_direction_10m,relative_humidity_2m,pressure_msl&daily=temperature_2m_max,temperature_2m_min,apparent_temperature_max&hourly=temperature_2m,apparent_temperature&timezone=auto&forecast_days=14`;
                const response = await fetch(api);
                globalData = await response.json();
                updateCurrentUI();
                renderMainGraph();
                switchPeriod(14, document.querySelector('.tab-trigger.active'));
            } catch (err) { console.error(err); }
        }

        function updateCurrentUI() {
            const cur = globalData.current;
            document.getElementById('t-now').innerText = Math.round(cur.temperature_2m);
            document.getElementById('s-now').innerText = WEATHER_CODES[cur.weather_code]?.t || "Ясно";
            document.getElementById('e-now').innerText = WEATHER_CODES[cur.weather_code]?.e || "☀️";
            document.getElementById('f-now').innerText = `Ощущается как ${Math.round(cur.apparent_temperature)}°`;
            document.getElementById('wind-s').innerText = Math.round(cur.wind_speed_10m);
            document.getElementById('wind-dir-name').innerText = getWindDir(cur.wind_direction_10m);
            document.getElementById('wind-dir-icon').style.transform = `rotate(${cur.wind_direction_10m}deg)`;
            document.getElementById('press-v').innerText = Math.round(cur.pressure_msl * 0.75006);
            document.getElementById('hum-v').innerText = cur.relative_humidity_2m;
        }

        function getChartColors() {
            const isLight = document.documentElement.getAttribute('data-theme') === 'light';
            return {
                text: isLight ? '#0f172a' : '#ffffff',
                grid: isLight ? 'rgba(15, 23, 42, 0.1)' : 'rgba(255, 255, 255, 0.1)',
                accent: isLight ? '#2563eb' : '#60a5fa'
            };
        }

        function renderMainGraph() {
            const ctx = document.getElementById('mainChart').getContext('2d');
            const colors = getChartColors();
            const labels = globalData.hourly.time.slice(0, 24).map(t => new Date(t).getHours() + ":00");
            const temp = globalData.hourly.temperature_2m.slice(0, 24);
            const feels = globalData.hourly.apparent_temperature.slice(0, 24);

            if (mainChart) mainChart.destroy();
            mainChart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels,
                    datasets: [
                        { label: 'Температура', data: temp, borderColor: colors.accent, borderWidth: 3, tension: 0.4, pointRadius: 4, pointHoverRadius: 8, fill: true, backgroundColor: colors.accent + '1A' },
                        { label: 'Ощущается', data: feels, borderColor: colors.text + '33', borderDash: [5,5], tension: 0.4, pointRadius: 0 }
                    ]
                },
                options: {
                    responsive: true, maintainAspectRatio: false,
                    interaction: { intersect: false, mode: 'index' },
                    plugins: { legend: { display: false } },
                    scales: { x: { grid: { display: false }, ticks: { color: colors.text + '66' } }, y: { display: false } }
                }
            });
        }

        function switchPeriod(days, btn) {
            document.querySelectorAll('.tab-trigger').forEach(t => t.classList.remove('active'));
            btn.classList.add('active');
            const grid = document.getElementById('main-grid');

            // Защита панели: выносим её перед очисткой сетки
            const pane = document.getElementById('fixed-detail-pane');
            pane.style.display = 'none';
            document.body.appendChild(pane);
            currentOpenIdx = null;

            grid.innerHTML = '';
            for (let i = 0; i < days; i++) {
                const date = new Date(globalData.daily.time[i]);
                const card = document.createElement('div');
                card.className = 'day-card';
                card.innerHTML = `<div class="card-day">${date.toLocaleDateString('ru-RU', {weekday:'short', day:'numeric'})}</div><span class="card-t-max">${Math.round(globalData.daily.temperature_2m_max[i])}°</span><span class="card-t-min">${Math.round(globalData.daily.temperature_2m_min[i])}°</span>`;
                card.onclick = () => openDayDetails(i, card);
                grid.appendChild(card);
            }
        }

        function openDayDetails(idx, card) {
            const grid = document.getElementById('main-grid');
            const pane = document.getElementById('fixed-detail-pane');

            if (currentOpenIdx === idx) {
                pane.style.display = 'none';
                card.classList.remove('active');
                currentOpenIdx = null;
                return;
            }

            document.querySelectorAll('.day-card').forEach(c => c.classList.remove('active'));
            card.classList.add('active');
            currentOpenIdx = idx;

            // Логика поиска конца ряда через offsetTop
            const allCards = Array.from(grid.querySelectorAll('.day-card'));
            const cardTop = card.offsetTop;
            let lastCardInRow = card;

            for (let i = allCards.indexOf(card); i < allCards.length; i++) {
                if (allCards[i].offsetTop === cardTop) {
                    lastCardInRow = allCards[i];
                } else {
                    break;
                }
            }

            lastCardInRow.after(pane);
            pane.style.display = 'block';

            document.getElementById('p-day-t').innerText = Math.round(globalData.daily.temperature_2m_max[idx]) + '°';
            document.getElementById('p-night-t').innerText = Math.round(globalData.daily.temperature_2m_min[idx]) + '°';
            refreshSubChart(idx, document.getElementById('feel-toggle').checked);
            document.getElementById('feel-toggle').onchange = (e) => refreshSubChart(idx, e.target.checked);
        }

        function refreshSubChart(idx, showFeels) {
            const canvas = document.getElementById('subChart');
            if (!canvas) return;
            const ctx = canvas.getContext('2d');
            const colors = getChartColors();
            const start = idx * 24;
            const labels = globalData.hourly.time.slice(start, start + 24).map(t => new Date(t).getHours() + ":00");
            const temp = globalData.hourly.temperature_2m.slice(start, start + 24);
            const feels = globalData.hourly.apparent_temperature.slice(start, start + 24);

            if (secondaryChart) secondaryChart.destroy();
            const datasets = [{ label: 'Температура', data: temp, borderColor: colors.accent, borderWidth: 4, tension: 0.4, pointRadius: 5, pointBackgroundColor: colors.text }];
            if (showFeels) datasets.push({ label: 'Ощущается', data: feels, borderColor: colors.text + '66', borderDash: [5,5], borderWidth: 2, tension: 0.4, pointRadius: 0 });

            secondaryChart = new Chart(ctx, {
                type: 'line',
                data: { labels, datasets },
                options: {
                    responsive: true, maintainAspectRatio: false,
                    plugins: { legend: { labels: { color: colors.text } } },
                    scales: {
                        x: { ticks: { color: colors.text + '99' }, grid: { color: colors.grid } },
                        y: { ticks: { color: colors.text + '99' }, grid: { color: colors.grid } }
                    }
                }
            });
        }

        // СНЕГ (Particles)
        const cvs = document.getElementById('particles'), pctx = cvs.getContext('2d');
        let pts = [];
        function initP() {
            cvs.width = window.innerWidth;
            cvs.height = window.innerHeight;
            pts = Array.from({length: 80}, () => ({
                x: Math.random()*cvs.width,
                y: Math.random()*cvs.height,
                s: Math.random()*2 + 1,
                v: Math.random()*0.5 + 0.2
            }));
        }
        function drawP() {
            pctx.clearRect(0,0,cvs.width,cvs.height);
            // Белый цвет снега для темной темы
            pctx.fillStyle = "rgba(255, 255, 255, 0.7)";
            pts.forEach(p => {
                pctx.beginPath();
                pctx.arc(p.x, p.y, p.s, 0, 7);
                pctx.fill();
                p.y += p.v;
                if(p.y > cvs.height) p.y = -5;
            });
            requestAnimationFrame(drawP);
        }
        window.onresize = initP; initP(); drawP();
        startApp();
    </script>
</body>
</html>
