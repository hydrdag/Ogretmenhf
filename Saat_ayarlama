<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Etkileşimli Saat Uygulaması</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
            background-color: #f0f0f0;
            position: relative;
        }
        #digitalClock {
            font-size: 2em;
            margin-bottom: 20px;
        }
        #clockContainer {
            position: relative;
            width: 300px;
            height: 300px;
            background: #fff;
            border-radius: 50%;
            border: 10px solid #333;
            box-shadow: 0 0 20px rgba(0,0,0,0.2);
        }
        .number {
            position: absolute;
            width: 100%;
            height: 100%;
            text-align: center;
            font-size: 0.75em; /* Sayıların boyutu yarıya indirildi (1.5em -> 0.75em) */
            color: #333;
        }
        .minute-line {
            position: absolute;
            width: 2px;
            height: 10px;
            background: #666;
            left: 50%;
            top: 10px;
            transform-origin: center 140px;
        }
        .number1 { transform: rotate(30deg); }
        .number2 { transform: rotate(60deg); }
        .number3 { transform: rotate(90deg); }
        .number4 { transform: rotate(120deg); }
        .number5 { transform: rotate(150deg); }
        .number6 { transform: rotate(180deg); }
        .number7 { transform: rotate(210deg); }
        .number8 { transform: rotate(240deg); }
        .number9 { transform: rotate(270deg); }
        .number10 { transform: rotate(300deg); }
        .number11 { transform: rotate(330deg); }
        .number12 { transform: rotate(0deg); }
        #hourHand, #minuteHand {
            position: absolute;
            background: #333;
            transform-origin: bottom;
            left: 50%;
            bottom: 50%;
            cursor: pointer;
        }
        #hourHand {
            width: 6px;
            height: 80px;
            margin-left: -3px;
        }
        #minuteHand {
            width: 4px;
            height: 120px;
            margin-left: -2px;
        }
        #center {
            position: absolute;
            width: 20px;
            height: 20px;
            background: #333;
            border-radius: 50%;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
        }
        #hdLogo {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 1.2em;
            font-weight: bold;
            color: #fff;
        }
        #buttons {
            margin-top: 20px;
        }
        button {
            padding: 10px 20px;
            font-size: 1em;
            margin: 0 10px;
            cursor: pointer;
        }
        #signature {
            position: absolute;
            bottom: 10px;
            right: 10px;
            font-size: 1em;
            color: #333;
        }
        #result {
            font-size: 1.2em;
            margin-top: 20px;
            text-align: center;
            display: none; /* Başlangıçta gizli */
        }
        @media (max-width: 600px) {
            #clockContainer {
                width: 200px;
                height: 200px;
            }
            #hourHand { height: 50px; }
            #minuteHand { height: 80px; }
            #digitalClock { font-size: 1.5em; }
            .minute-line { transform-origin: center 90px; }
        }
    </style>
</head>
<body>
    <div id="digitalClock"></div>
    <div id="clockContainer">
        <div class="number number1">1</div>
        <div class="number number2">2</div>
        <div class="number number3">3</div>
        <div class="number number4">4</div>
        <div class="number number5">5</div>
        <div class="number number6">6</div>
        <div class="number number7">7</div>
        <div class="number number8">8</div>
        <div class="number number9">9</div>
        <div class="number number10">10</div>
        <div class="number number11">11</div>
        <div class="number number12">12</div>
        <!-- Dakika çizgileri -->
        <div id="minuteLines"></div>
        <div id="hourHand"></div>
        <div id="minuteHand"></div>
        <div id="center">
            <span id="hdLogo">HD</span>
        </div>
    </div>
    <div id="buttons">
        <button onclick="checkTime()">Kontrol Et</button>
        <button onclick="refreshClock()">Yenile</button>
    </div>
    <div id="result"></div>
    <div id="signature">Haydar DAĞ</div>

    <audio id="correctSound" src="https://www.soundjay.com/buttons/beep-01a.mp3"></audio>
    <audio id="wrongSound" src="https://www.soundjay.com/buttons/beep-02.mp3"></audio>

    <script>
        const digitalClock = document.getElementById('digitalClock');
        const hourHand = document.getElementById('hourHand');
        const minuteHand = document.getElementById('minuteHand');
        const minuteLines = document.getElementById('minuteLines');
        const correctSound = document.getElementById('correctSound');
        const wrongSound = document.getElementById('wrongSound');
        const resultDiv = document.getElementById('result');
        let targetHour, targetMinute;

        // Dakika çizgilerini ekle, sayılarla çakışanları hariç tut
        for (let i = 0; i < 60; i++) {
            const angle = i * 6;
            if (angle % 30 !== 0) {
                const line = document.createElement('div');
                line.className = 'minute-line';
                line.style.transform = `rotate(${angle}deg)`;
                minuteLines.appendChild(line);
            }
        }

        function setRandomDigitalTime() {
            targetHour = Math.floor(Math.random() * 12) + 1;
            targetMinute = Math.floor(Math.random() * 60);
            digitalClock.textContent = `${targetHour.toString().padStart(2, '0')}:${targetMinute.toString().padStart(2, '0')}`;
            resultDiv.style.display = 'none'; // Yeni saat ayarlandığında sonucu gizle
        }

        function rotateHand(hand, angle) {
            hand.style.transform = `rotate(${angle}deg)`;
        }

        function getAngleFromPosition(x, y, centerX, centerY) {
            const dx = x - centerX;
            const dy = centerY - y;
            let angle = Math.atan2(dx, dy) * 180 / Math.PI;
            if (angle < 0) angle += 360;
            return angle;
        }

        let isDragging = null;
        const clockContainer = document.getElementById('clockContainer');
        let centerX, centerY;

        function updateCenter() {
            const rect = clockContainer.getBoundingClientRect();
            centerX = rect.left + rect.width / 2;
            centerY = rect.top + rect.height / 2;
        }
        window.addEventListener('resize', updateCenter);
        updateCenter();

        hourHand.addEventListener('mousedown', () => isDragging = 'hour');
        minuteHand.addEventListener('mousedown', () => isDragging = 'minute');
        document.addEventListener('mouseup', () => isDragging = null);

        document.addEventListener('mousemove', (e) => {
            if (!isDragging) return;
            const angle = getAngleFromPosition(e.clientX, e.clientY, centerX, centerY);
            if (isDragging === 'hour') {
                rotateHand(hourHand, angle);
            } else if (isDragging === 'minute') {
                rotateHand(minuteHand, angle);
            }
        });

        hourHand.addEventListener('touchstart', (e) => {
            e.preventDefault();
            isDragging = 'hour';
        });
        minuteHand.addEventListener('touchstart', (e) => {
            e.preventDefault();
            isDragging = 'minute';
        });
        document.addEventListener('touchend', () => isDragging = null);

        document.addEventListener('touchmove', (e) => {
            if (!isDragging) return;
            const touch = e.touches[0];
            const angle = getAngleFromPosition(touch.clientX, touch.clientY, centerX, centerY);
            if (isDragging === 'hour') {
                rotateHand(hourHand, angle);
            } else if (isDragging === 'minute') {
                rotateHand(minuteHand, angle);
            }
        });

        function checkTime() {
            const hourAngle = parseFloat(hourHand.style.transform.replace('rotate(', '').replace('deg)', '')) || 0;
            const minuteAngle = parseFloat(minuteHand.style.transform.replace('rotate(', '').replace('deg)', '')) || 0;

            const userHour = Math.round((hourAngle % 360) / 30) || 12;
            const userMinute = Math.round((minuteAngle % 360) / 6);

            resultDiv.style.display = 'block'; // Sonucu göster
            if (userHour === targetHour && Math.abs(userMinute - targetMinute) <= 5) {
                correctSound.play();
                resultDiv.style.color = 'green';
                resultDiv.textContent = `Tebrikler! Dijital saat (${targetHour}:${targetMinute.toString().padStart(2, '0')}) ile analog saat eşleşti.`;
            } else {
                wrongSound.play();
                resultDiv.style.color = 'red';
                resultDiv.textContent = `Yanlış! Dijital saat: ${targetHour}:${targetMinute.toString().padStart(2, '0')}, Analog saat: ${userHour}:${userMinute.toString().padStart(2, '0')}. Tekrar deneyin.`;
            }
        }

        function refreshClock() {
            setRandomDigitalTime();
            rotateHand(hourHand, 0);
            rotateHand(minuteHand, 0);
            updateCenter();
        }

        setRandomDigitalTime();
    </script>
</body>
</html>
