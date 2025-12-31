<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>New Year 2026 Celebration</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>
    <style>
        body {
            font-family: 'Inter', sans-serif;
            transition: background-color 0.5s, color 0.5s;
            overflow: hidden;
        }
        .dark-mode {
            background: linear-gradient(135deg, #0f0f23, #1a1a2e);
            color: white;
        }
        .light-mode {
            background: linear-gradient(135deg, #e0f7fa, #ffffff);
            color: #333;
        }
        .glow {
            text-shadow: 0 0 15px rgba(255, 215, 0, 0.8);
        }
        .glassmorphism {
            backdrop-filter: blur(15px);
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
        }
        .light-mode .glassmorphism {
            background: rgba(0, 0, 0, 0.1);
            border: 1px solid rgba(0, 0, 0, 0.2);
        }
        .intro {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: black;
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1000;
        }
        .ripple {
            position: absolute;
            border: 2px solid rgba(255, 215, 0, 0.5);
            border-radius: 50%;
            animation: ripple 2s infinite;
        }
        @keyframes ripple {
            0% { transform: scale(0); opacity: 1; }
            100% { transform: scale(4); opacity: 0; }
        }
        .spark {
            position: absolute;
            width: 2px;
            height: 2px;
            background: #ffd700;
            border-radius: 50%;
            animation: spark 3s infinite ease-in-out;
        }
        @keyframes spark {
            0%, 100% { opacity: 0; transform: translateY(0); }
            50% { opacity: 1; transform: translateY(-10px); }
        }
        #fireworks {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 1;
        }
        .message {
            opacity: 0;
            transform: translateY(20px);
        }
        .vibrate {
            animation: vibrate 0.3s;
        }
        @keyframes vibrate {
            0% { transform: translateX(0); }
            10% { transform: translateX(-2px); }
            20% { transform: translateX(2px); }
            30% { transform: translateX(-2px); }
            40% { transform: translateX(2px); }
            50% { transform: translateX(0); }
        }
    </style>
</head>
<body class="dark-mode min-h-screen flex flex-col items-center justify-center p-4 relative">
    <!-- Intro Screen -->
    <div id="intro" class="intro">
        <div class="text-center">
            <div id="glow-effect" class="text-6xl glow opacity-0">A New Beginning…</div>
        </div>
    </div>

    <!-- Main Screen -->
    <div id="main-screen" class="opacity-0 min-h-screen flex flex-col items-center justify-center p-4 space-y-6 relative">
        <!-- Sparks Background -->
        <div id="sparks" class="absolute inset-0 pointer-events-none"></div>

        <!-- Fireworks Canvas -->
        <canvas id="fireworks"></canvas>

        <!-- Main Card -->
        <div class="glassmorphism p-8 rounded-2xl max-w-lg w-full text-center space-y-6">
            <h1 class="text-4xl md:text-5xl font-bold glow">Happy New Year 2026</h1>
            <p id="personal-greeting" class="text-xl glow">Dear Friend 🎉</p>

            <!-- Name Input -->
            <div>
                <input id="name-input" type="text" placeholder="Your Name (optional)" class="w-full p-3 rounded-lg bg-transparent border border-white/30 text-center focus:outline-none focus:ring-2 focus:ring-yellow-400 transition">
            </div>

            <!-- Countdown -->
            <div id="countdown-section">
                <p class="text-lg">Live Countdown:</p>
                <div id="countdown" class="text-3xl font-mono glow"></div>
                <p id="new-year-message" class="text-xl glow hidden">🎉 The New Year Has Begun 🎉</p>
            </div>

            <!-- Animated Messages -->
            <div class="space-y-4">
                <p id="msg1" class="message text-lg">May this year bring peace, happiness and success ✨</p>
                <p id="msg2" class="message text-lg">Leave worries behind, carry hope forward 🌱</p>
                <p id="msg3" class="message text-lg">Every ending is a new beginning 🌟</p>
            </div>

            <!-- Celebrate Button -->
            <button id="celebrate-btn" class="bg-gradient-to-r from-yellow-400 to-orange-500 text-black px-8 py-4 rounded-full font-semibold hover:scale-105 transition transform text-xl">🎆 Celebrate Now</button>

            <!-- Controls -->
            <div class="flex justify-center space-x-4 mt-6">
                <button id="music-toggle" class="bg-white/20 p-3 rounded-full hover:bg-white/30 transition">🎵 Music</button>
                <button id="theme-toggle" class="bg-white/20 p-3 rounded-full hover:bg-white/30 transition">🌙 Theme</button>
                <button id="share-btn" class="bg-white/20 p-3 rounded-full hover:bg-white/30 transition">📱 Share</button>
            </div>
        </div>
    </div>

    <!-- Audio -->
    <audio id="bg-music" loop>
        <source src="https://www.soundjay.com/misc/sounds/bell-ringing-05.wav" type="audio/wav"> <!-- Placeholder: Replace with a soft festive instrumental -->
    </audio>

    <script>
        // Intro Animation
        gsap.to('#glow-effect', { opacity: 1, duration: 1, delay: 1 });
        setTimeout(() => {
            confetti({ particleCount: 100, spread: 70, origin: { y: 0.6 } });
            launchFireworks();
        }, 2000);
        setTimeout(() => {
            gsap.to('#intro', { opacity: 0, duration: 1, onComplete: () => {
                document.getElementById('intro').style.display = 'none';
                gsap.to('#main-screen', { opacity: 1, duration: 1 });
            }});
        }, 4000);

        // Sparks
        const sparksContainer = document.getElementById('sparks');
        for (let i = 0; i < 30; i++) {
            const spark = document.createElement('div');
            spark.className = 'spark';
            spark.style.left = Math.random() * 100 + '%';
            spark.style.top = Math.random() * 100 + '%';
            spark.style.animationDelay = Math.random() * 3 + 's';
            sparksContainer.appendChild(spark);
        }

        // Name Personalization
        const nameInput = document.getElementById('name-input');
        const personalGreeting = document.getElementById('personal-greeting');
        nameInput.addEventListener('input', () => {
            const name = nameInput.value.trim() || 'Dear Friend';
            personalGreeting.textContent = `Happy New Year 2026, ${name} 🎉`;
        });

        // Countdown
        const countdownElement = document.getElementById('countdown');
        const countdownSection = document.getElementById('countdown-section');
        const newYearMessage = document.getElementById('new-year-message');
        const targetDate = new Date('2026-01-01T00:00:00').getTime();
        function updateCountdown() {
            const now = new Date().getTime();
            const distance = targetDate - now;
            if (distance > 0) {
                const days = Math.floor(distance / (1000 * 60 * 60 * 24));
                const hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
                const minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
                const seconds = Math.floor((distance % (1000 * 60)) / 1000);
                countdownElement.textContent = `${days}d ${hours}h ${minutes}m ${seconds}s`;
            } else {
                countdownSection.style.display = 'none';
                newYearMessage.classList.remove('hidden');
                gsap.from(newYearMessage, { opacity: 0, scale: 0.5, duration: 1 });
            }
        }
        setInterval(updateCountdown, 1000);
        updateCountdown();

        // Animated Messages
        const messages = [document.getElementById('msg1'), document.getElementById('msg2'), document.getElementById('msg3')];
        messages.forEach((msg, index) => {
            setTimeout(() => {
                gsap.to(msg, { opacity: 1, y: 0, duration: 1, delay: index * 2 });
            }, 5000 + index * 2000);
        });

        // Celebrate Button
        const celebrateBtn = document.getElementById('celebrate-btn');
        celebrateBtn.addEventListener('click', () => {
            // Massive Fireworks and Confetti
            confetti({ particleCount: 500, spread: 180, origin: { y: 0.6 } });
            launchFireworks();
            // Vibration Effect
            if (navigator.vibrate) navigator.vibrate(300);
            document.body.classList.add('vibrate');
            setTimeout(() => document.body.classList.remove('vibrate'), 300);
            // Glow Ripple
            const ripple = document.createElement('div');
            ripple.className = 'ripple';
            ripple.style.left = '50%';
            ripple.style.top = '50%';
            document.body.appendChild(ripple);
            setTimeout(() => ripple.remove(), 2000);
        });

        // Fireworks Function
        function launchFireworks() {
            const canvas = document.getElementById('fireworks');
            const ctx = canvas.getContext('2d');
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
            for (let j = 0; j < 50; j++) {
                setTimeout(() => {
                    const x = Math.random() * canvas.width;
                    const y = Math.random() * canvas.height;
                    ctx.fillStyle = `hsl(${Math.random() * 360}, 100%, 70%)`;
                    ctx.beginPath();
                    ctx.arc(x, y, 5, 0, Math.PI * 2);
                    ctx.fill();
                    gsap.to({}, { duration: 3, onComplete: () => ctx.clearRect(x-15, y-15, 30, 30) });
                }, j * 50);
            }
        }

        // Music Toggle
        const musicToggle = document.getElementById('music-toggle');
        const bgMusic = document.getElementById('bg-music');
        let isPlaying = false;
        musicToggle.addEventListener('click', () => {
            if (isPlaying) {
                bgMusic.pause();
                musicToggle.textContent = '🎵 Music';
            } else {
                bgMusic.play();
                musicToggle.textContent = '🔇 Music';
            }
            isPlaying = !isPlaying;
        });

        // Theme Toggle
        const themeToggle = document.getElementById('theme-toggle');
        let isDark = true;
        themeToggle.addEventListener('click', () => {
            isDark = !isDark;
            document.body.className = isDark ? 'dark-mode' : 'light-mode';
            themeToggle.textContent = isDark ? '🌙 Theme' : '☀️ Theme';
        });

        // Share Button
        const shareBtn = document.getElementById('share-btn');
        shareBtn.addEventListener('click', () => {
            const message = `Wishing you a joyful New Year 🎉 Check this out ✨ ${window.location.href}`;
            const url = `https://wa.me/?text=${encodeURIComponent(message)}`;
            window.open(url, '_blank');
        });
    </script>
</body>
</html>
