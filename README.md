<!DOCTYPE html>
<html lang="uk">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>З Днем Народження!</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            -webkit-tap-highlight-color: transparent; /* Прибирає синю підсвітку при тапі на мобільних */
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            background: linear-gradient(135deg, #1a1a2e, #16213e);
            color: #fff;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden;
            padding: 15px;
        }

        .card {
            background: rgba(255, 255, 255, 0.07);
            backdrop-filter: blur(15px);
            -webkit-backdrop-filter: blur(15px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            padding: 35px 20px;
            border-radius: 24px;
            text-align: center;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.4);
            width: 100%;
            max-width: 380px; /* Ідеальна ширина для мобільних екранів */
            position: relative;
            transition: all 0.3s ease;
        }

        h1 {
            font-size: 2rem;
            margin-bottom: 12px;
            color: #4e9eff;
            text-shadow: 0 0 10px rgba(78, 158, 255, 0.3);
            line-height: 1.2;
        }

        p {
            font-size: 1.3rem;
            margin-bottom: 15px;
            color: #e2e8f0;
        }

        .warning-text {
            font-size: 1.1rem;
            color: #ff4757;
            font-weight: bold;
            margin-bottom: 35px;
            text-transform: uppercase;
            letter-spacing: 1px;
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0% { opacity: 0.6; transform: scale(0.98); }
            50% { opacity: 1; transform: scale(1.02); }
            100% { opacity: 0.6; transform: scale(0.98); }
        }

        .btn-container {
            display: flex;
            justify-content: center;
            gap: 15px;
            height: 50px;
            position: relative;
            width: 100%;
        }

        .btn {
            padding: 12px 30px;
            font-size: 1.1rem;
            font-weight: bold;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            touch-action: manipulation; /* Оптимізація тапу для мобільних */
        }

        .btn-yes {
            background-color: #4cd137;
            color: white;
            box-shadow: 0 4px 15px rgba(76, 209, 55, 0.4);
            width: 45%;
        }

        .btn-no {
            background-color: #e84118;
            color: white;
            box-shadow: 0 4px 15px rgba(232, 65, 24, 0.4);
            width: 45%;
            position: absolute;
            right: 5%; /* Стартове вирівнювання на мобільному */
            z-index: 999;
            will-change: top, left; /* Підказка браузеру для плавності анімації */
        }

        .hidden {
            display: none !important;
        }

        .congrats-text {
            font-size: 1.8rem;
            color: #fbc531;
            animation: bounce 1s infinite alternate;
            margin-bottom: 15px;
            line-height: 1.3;
        }

        @keyframes bounce {
            from { transform: translateY(0); }
            to { transform: translateY(-8px); }
        }
    </style>
</head>
<body>

    <div class="card" id="quiz-card">
        <h1>З Днем Народження! 🎉</h1>
        <p>Тобі вже 17?</p>
        <div class="warning-text">⚠️ Не натискай ТАК!</div>
        
        <div class="btn-container">
            <button class="btn btn-yes" id="yes-btn">Так</button>
            <button class="btn btn-no" id="no-btn">Ні</button>
        </div>
    </div>

    <div class="card hidden" id="congrats-card">
        <h1>Урааа! ✨</h1>
        <p class="congrats-text">Вітаю з 17-річчям! 🎂</p>
        <p style="font-size: 1rem; color: #a4b0be; line-height: 1.4;">Ти все одно натиснув "Так"! І це єдиний правильний вибір! 😉</p>
    </div>

    <script>
        const noBtn = document.getElementById('no-btn');
        const yesBtn = document.getElementById('yes-btn');
        const quizCard = document.getElementById('quiz-card');
        const congratsCard = document.getElementById('congrats-card');

        // Функція переміщення кнопки
        function moveButton(e) {
            if (e) {
                e.preventDefault(); // Повністю блокуємо стандартний клік на мобільних
            }

            // Безпечний відступ від країв екрану в пікселях
            const padding = 40; 
            
            // Отримуємо чисті розміри екрану смартфона/ПК
            const maxX = window.innerWidth - noBtn.offsetWidth - padding;
            const maxY = window.innerHeight - noBtn.offsetHeight - padding;

            // Генеруємо випадкові координати, але не менше ніж наш безпечний відступ
            let randomX = Math.floor(Math.random() * (maxX - padding)) + padding;
            let randomY = Math.floor(Math.random() * (maxY - padding)) + padding;

            // Робимо кнопку фіксованою і змушуємо літати по всьому дисплею
            noBtn.style.position = 'fixed';
            noBtn.style.left = randomX + 'px';
            noBtn.style.top = randomY + 'px';
        }

        // Мобільна оптимізація: спрацьовує в момент першого дотику пальця до екрану
        noBtn.addEventListener('touchstart', moveButton, { passive: false });

        // Для комп'ютерів: спрацьовує при наведенні миші
        noBtn.addEventListener('mouseover', moveButton);

        // На випадок, якщо хтось спробує клацнути дуже швидко кнопками навігації
        noBtn.addEventListener('click', moveButton);

        // Перехід на фінальний екран
        yesBtn.addEventListener('click', () => {
            quizCard.classList.add('hidden');
            congratsCard.classList.remove('hidden');
            
            // Ховаємо кнопку "Ні", щоб вона випадково не зависла поверх привітання
            noBtn.classList.add('hidden');
        });
    </script>
</body>
</html>
