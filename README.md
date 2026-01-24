<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>заОкном — Выбор версии</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;800&display=swap" rel="stylesheet">
    <style>
        body {
            margin: 0;
            font-family: 'Inter', sans-serif;
            background: #000;
            color: #fff;
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100vh;
            overflow: hidden;
        }

        .bg-gradient {
            position: fixed;
            inset: 0;
            background: radial-gradient(circle at center, #1e293b 0%, #000 100%);
            z-index: -1;
        }

        .choice-container {
            text-align: center;
            padding: 20px;
            max-width: 500px;
            width: 100%;
        }

        h1 {
            font-size: 3rem;
            font-weight: 800;
            margin-bottom: 10px;
            background: linear-gradient(135deg, #fff 0%, #60a5fa 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        p {
            opacity: 0.6;
            margin-bottom: 40px;
        }

        .buttons-stack {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .btn {
            padding: 20px;
            border-radius: 20px;
            text-decoration: none;
            font-weight: 700;
            font-size: 1.1rem;
            transition: all 0.3s ease;
            border: 1px solid rgba(255,255,255,0.1);
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }

        .btn-mobile {
            background: #60a5fa;
            color: #000;
        }

        .btn-desktop {
            background: rgba(255,255,255,0.05);
            color: #fff;
        }

        .btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 30px rgba(96, 165, 250, 0.3);
        }

        .icon { font-size: 1.5rem; }
    </style>
</head>
<body>
    <div class="bg-gradient"></div>
    
    <div class="choice-container">
        <h1>заОкном</h1>
        <p>Выберите версию интерфейса</p>

        <div class="buttons-stack">
            <a href="mobile.html" class="btn btn-mobile">
                <span class="icon">📱</span> Мобильная версия
            </a>
            <a href="desktop.html" class="btn btn-desktop">
                <span class="icon">💻</span> Полная версия
            </a>
        </div>
    </div>
</body>
</html>
