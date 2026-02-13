<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Валентинка для моей любимой</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            background: linear-gradient(135deg, #ff9a9e 0%, #fecfef 50%, #fecfef 100%);
            font-family: 'Arial', sans-serif;
            overflow: hidden;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            position: relative;
        }

        .background-images {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 2;
            pointer-events: none;
        }

        .bg-image {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            object-fit: cover;
            opacity: 0;
            transform: translateX(100vw) scale(0.8);
            transition: all 1.5s ease-out;
            filter: blur(2px) brightness(0.6) sepia(0.2); /* Романтический фильтр для фото */
        }

        .bg-image.visible {
            opacity: 0.3;
            transform: translateX(0) scale(1);
        }

        .container {
            text-align: center;
            z-index: 10;
            position: relative;
        }

        .text-line {
            opacity: 0;
            transform: translateY(50px);
            transition: all 0.8s ease-out;
            font-size: 1.5em;
            color: #fff;
            text-shadow: 1px 1px 2px rgba(0,0,0,0.3);
            margin: 10px 0;
        }

        .text-line.visible {
            opacity: 1;
            transform: translateY(0);
        }

        h1 {
            font-size: 3em;
            color: #fff;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
            margin-bottom: 20px;
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }

        .heart {
            position: absolute;
            width: 20px;
            height: 18px;
            background: #ff4d6d;
            transform: rotate(-45deg);
            animation: float 6s infinite linear;
            z-index: 5;
        }

        .heart:before,
        .heart:after {
            content: '';
            width: 20px;
            height: 20px;
            position: absolute;
            background: #ff4d6d;
            border-radius: 50%;
        }

        .heart:before {
            top: -10px;
            left: 0;
        }

        .heart:after {
            left: 10px;
            top: 0;
        }

        @keyframes float {
            0% {
                transform: translateY(100vh) rotate(-45deg) scale(0);
                opacity: 1;
            }
            100% {
                transform: translateY(-100px) rotate(405deg) scale(1);
                opacity: 0;
            }
        }

        .btn {
            background: #ff6b9d;
            color: white;
            border: none;
            padding: 15px 30px;
            font-size: 1.2em;
            border-radius: 50px;
            cursor: pointer;
            transition: transform 0.3s, box-shadow 0.3s;
            box-shadow: 0 4px 8px rgba(0,0,0,0.2);
            opacity: 0;
            transform: translateY(50px);
            transition: all 0.8s ease-out;
        }

        .btn.visible {
            opacity: 1;
            transform: translateY(0);
        }

        .btn:hover {
            transform: scale(1.1);
            box-shadow: 0 6px 12px rgba(0,0,0,0.3);
        }

        #message {
            margin-top: 20px;
            font-size: 1.8em;
            color: #fff;
            opacity: 0;
            transition: opacity 1s;
        }

        canvas {
            position: absolute;
            top: 0;
            left: 0;
            z-index: 1;
        }

      
        @media (max-width: 768px) {
            h1 { font-size: 2em; }
            .text-line { font-size: 1.2em; }
            .bg-image { filter: blur(1px) brightness(0.7) sepia(0.1); } 
        }
    </style>
</head>
<body>
    <canvas id="canvas"></canvas>
    <div class="background-images">

        <img src="img1.jpg" alt="Фото 1" class="bg-image" data-delay="0"> 
        <img src="img2.jpg" alt="Фото 2" class="bg-image" data-delay="2"> 
        <img src="img3.jpg" alt="Фото 3" class="bg-image" data-delay="4"> 
        <img src="img4.jpg" alt="Фото 4" class="bg-image" data-delay="6"> 
        <img src="img5.jpg" alt="Фото 5" class="bg-image" data-delay="8"> 
        <img src="img6.jpg" alt="Фото 6" class="bg-image" data-delay="10"> 
 
    </div>
    <div class="container">
        <h1 id="title">С Днём Святого Валентина! ❤️</h1>
        <div class="text-line" data-delay="0">Ты — самое яркое</div>
        <div class="text-line" data-delay="1">и тёплое в моей жизни.</div>
        <div class="text-line" data-delay="2">Я люблю тебя !</div>
        <button class="btn" id="btn" onclick="showPersonalMessage()">Нажми меня</button>
        <div id="message"></div>
    </div>

    <script>
        // Анимация сердечек на canvas (как раньше)
        const canvas = document.getElementById('canvas');
        const ctx = canvas.getContext('2d');
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;

        function createHeart(x, y, size, speed) {
            return {
                x: x,
                y: y,
                size: size,
                speed: speed,
                rotation: 0,
                alpha: 1
            };
        }

        let hearts = [];

        function animate() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            if (Math.random() < 0.1) {
                hearts.push(createHeart(
                    Math.random() * canvas.width,
                    canvas.height + 20,
                    Math.random() * 20 + 10,
                    Math.random() * 2 + 1
                ));
            }

            hearts.forEach((heart, index) => {
                heart.y -= heart.speed;
                heart.rotation += 0.1;
                heart.alpha -= 0.005;

                if (heart.alpha <= 0) {
                    hearts.splice(index, 1);
                    return;
                }

                ctx.save();
                ctx.globalAlpha = heart.alpha;
                ctx.translate(heart.x, heart.y);
                ctx.rotate(heart.rotation);
                ctx.scale(heart.size / 20, heart.size / 20);

                ctx.beginPath();
                ctx.moveTo(10, 0);
                ctx.bezierCurveTo(10, -10, 0, -10, 0, 0);
                ctx.bezierCurveTo(0, 10, 10, 10, 10, 0);
                ctx.bezierCurveTo(10, -10, 20, -10, 20, 0);
                ctx.bezierCurveTo(20, 10, 10, 10, 10, 0);
                ctx.fillStyle = '#ff69b4';
                ctx.fill();

                ctx.restore();
            });

            requestAnimationFrame(animate);
        }

        animate();

        // CSS сердечки
        function createFloatingHeart() {
            const heart = document.createElement('div');
            heart.className = 'heart';
            heart.style.left = Math.random() * 100 + 'vw';
            heart.style.animationDuration = (Math.random() * 3 + 3) + 's';
            heart.style.opacity = Math.random();
            document.body.appendChild(heart);

            setTimeout(() => {
                heart.remove();
            }, 6000);
        }

        setInterval(createFloatingHeart, 300);

        // Интерактивная анимация появления текста и фото
        function animateEverything() {
            const lines = document.querySelectorAll('.text-line');
            const btn = document.getElementById('btn');
            const title = document.getElementById('title');
            const bgImages = document.querySelectorAll('.bg-image');

            // Анимируем заголовок сначала
            setTimeout(() => {
                title.style.opacity = '1';
                title.style.transform = 'translateY(0)';
            }, 500);

            // Анимируем строки текста по очереди
            lines.forEach((line, index) => {
                const delay = parseInt(line.dataset.delay) * 1000 + 1500;
                setTimeout(() => {
                    line.classList.add('visible');
                }, delay);
            });

            // Анимируем фото в фоне: каждое выезжает справа с задержкой, полупрозрачные
            bgImages.forEach((img, index) => {
                const delay = parseInt(img.dataset.delay) * 1000 + 1000; // Начинают раньше текста
                setTimeout(() => {
                    img.classList.add('visible');
                }, delay);
            });

            // Показываем кнопку после всех строк
            setTimeout(() => {
                btn.classList.add('visible');
            }, (lines.length * 1000) + 2000);
        }

        // Запускаем анимацию при загрузке
        window.addEventListener('load', animateEverything);

        // Личное послание (как раньше)
        function showPersonalMessage() {
            const messages = [
                "Твоя улыбка освещает мой мир! 😘",
                "С тобой каждый день — как праздник. ❤️",
                "Обнимаю крепко, целую нежно! 💕",
                "Спасибо, что ты есть. Люблю тебя!"
            ];
            const randomMsg = messages[Math.floor(Math.random() * messages.length)];
            const messageEl = document.getElementById('message');
            messageEl.textContent = randomMsg;
            messageEl.style.opacity = 1;

            // Анимация конфетти
            for (let i = 0; i < 50; i++) {
                setTimeout(() => {
                    createFloatingHeart();
                }, i * 50);
            }
        }

        // Адаптация под размер экрана
        window.addEventListener('resize', () => {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        });
    </script>
</body>
</html>