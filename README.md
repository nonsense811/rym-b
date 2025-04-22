# rym-b
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Happy Birthday, Rym!</title>
  <style>
    body {
      margin: 0;
      font-family: 'Courier New', monospace;
      background: linear-gradient(to bottom right, #fdf6f0, #ffe4ec);
      color: #333;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      overflow: hidden;
      flex-direction: column;
      text-align: center;
      transition: background 0.5s, color 0.5s;
    }
    body.dark {
      background: linear-gradient(to bottom right, #1e1e1e, #333);
      color: #eee;
    }
    .theme-toggle {
      position: absolute;
      top: 20px;
      right: 20px;
      z-index: 20;
      background: #ff90b3;
      color: white;
      border: none;
      padding: 0.5rem 1rem;
      border-radius: 8px;
      cursor: pointer;
    }
    .hidden {
      display: none;
    }
    .unlock-section, .game-section {
      text-align: center;
    }
    .letter {
      background: white;
      border-radius: 12px;
      padding: 2rem;
      max-width: 600px;
      box-shadow: 0 10px 20px rgba(0,0,0,0.1);
      white-space: pre-line;
      font-size: 1.1rem;
      z-index: 10;
      opacity: 0;
      transform: translateY(20px);
      transition: opacity 1s ease-in-out, transform 1s ease-in-out;
    }
    body.dark .letter {
      background: #2c2c2c;
      color: #eee;
    }
    .letter.show {
      opacity: 1;
      transform: translateY(0);
    }
    .timer {
      font-size: 1.5rem;
      color: #ff90b3;
      font-weight: bold;
      margin-top: 20px;
    }
    .button, .unlock-button {
      background: #ff90b3;
      color: white;
      padding: 0.8rem 1.5rem;
      border: none;
      border-radius: 8px;
      font-size: 1rem;
      cursor: pointer;
      margin-top: 1.5rem;
    }
    input[type="password"] {
      padding: 0.5rem;
      font-size: 1rem;
      border-radius: 8px;
      border: 1px solid #ccc;
      margin-top: 1rem;
    }
    .gift {
      width: 50px;
      position: absolute;
      top: 100px;
      left: 100px;
      cursor: pointer;
      transition: all 0.6s ease-in-out;
    }
    canvas {
      position: fixed;
      top: 0;
      left: 0;
      pointer-events: none;
    }
    #music {
      display: none;
    }
    .floating-hearts {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      pointer-events: none;
      overflow: hidden;
      z-index: 0;
    }
    .heart {
      position: absolute;
      bottom: 0;
      width: 20px;
      height: 20px;
      background: pink;
      transform: rotate(45deg);
      animation: float 6s linear infinite;
    }
    .heart::before, .heart::after {
      content: '';
      position: absolute;
      width: 20px;
      height: 20px;
      background: pink;
      border-radius: 50%;
    }
    .heart::before {
      top: -10px;
      left: 0;
    }
    .heart::after {
      left: -10px;
      top: 0;
    }
    @keyframes float {
      0% { transform: translateY(0) rotate(45deg); opacity: 1; }
      100% { transform: translateY(-100vh) rotate(45deg); opacity: 0; }
    }
    .pet {
      position: fixed;
      width: 40px;
      bottom: 20px;
      left: 50%;
      transform: translateX(-50%);
      transition: transform 0.3s ease;
      z-index: 999;
    }
    .candle {
      position: fixed;
      top: 80px;
      right: 40px;
      width: 20px;
      height: 60px;
      background: #ff90b3;
      border-radius: 10px;
      z-index: 10;
    }
    .flame {
      position: absolute;
      top: -20px;
      left: 5px;
      width: 10px;
      height: 20px;
      background: gold;
      border-radius: 50%;
      animation: flicker 0.2s infinite alternate;
    }
    @keyframes flicker {
      from { transform: scaleY(1); }
      to { transform: scaleY(1.2); }
    }
  </style>
</head>
<body>
  <button class="theme-toggle" onclick="toggleTheme()">Toggle Theme</button>
  <audio id="music" src="https://www.bensound.com/bensound-music/bensound-happyrock.mp3" autoplay></audio>
  <canvas id="confetti"></canvas>
  <div class="floating-hearts" id="hearts"></div>
  <div class="pet" id="pet">🐱</div>
  <div class="candle"><div class="flame" id="flame"></div></div>

  <div class="unlock-section" id="unlockSection">
    <h2>Enter the password or wait for the surprise 🎁</h2>
    <input type="password" id="password" placeholder="Enter password...">
    <br>
    <button class="unlock-button" onclick="checkPassword()">Unlock</button>
    <div class="timer" id="timer"></div>
    <p id="hint" class="hidden" style="color: #d66; margin-top: 1rem;"></p>
  </div>

  <div class="game-section hidden" id="gameSection">
    <h2>Nihahahahaha not yet 😈 catch it if u can 😂🥀</h2>
    <img src="https://i.imgur.com/N2KdfMl.png" alt="Gift" class="gift" id="gift">
  </div>

  <div class="letter hidden" id="letter">
    <h2>Hey Rym,</h2>
    <p id="typewriter"></p>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.5.1/dist/confetti.browser.min.js"></script>
  <script>
    const unlockSection = document.getElementById('unlockSection');
    const gameSection = document.getElementById('gameSection');
    const letter = document.getElementById('letter');
    const timer = document.getElementById('timer');
    const hint = document.getElementById('hint');
    const gift = document.getElementById('gift');
    const music = document.getElementById('music');
    const heartsContainer = document.getElementById('hearts');
    const pet = document.getElementById('pet');
    let attempts = 0;

    const validPasswords = ["+212 693-828659", "17/5/2007"];
    const targetDate = new Date('April 28, 2025 00:00:00').getTime();
    let confettiTriggered = false;

    function toggleTheme() {
      document.body.classList.toggle('dark');
    }

    document.addEventListener('mousemove', (e) => {
      pet.style.transform = `translate(${e.clientX - 20}px, ${e.clientY - 20}px)`;
    });

    function showGame() {
      unlockSection.style.display = 'none';
      gameSection.classList.remove('hidden');
    }

    function checkPassword() {
      const input = document.getElementById('password').value;
      if (validPasswords.includes(input)) {
        showGame();
      } else {
        attempts++;
        if (attempts >= 3) {
          hint.classList.remove('hidden');
          hint.innerText = "Hint: password is nmra 3bdo 😂 or my birthday";
        }
        alert("Wrong password 🤫 Try again or wait 😏");
      }
    }

    function updateTimer() {
      const now = new Date().getTime();
      const timeLeft = targetDate - now;

      if (timeLeft <= 0) {
        showGame();
        return;
      }

      const days = Math.floor(timeLeft / (1000 * 60 * 60 * 24));
      const hours = Math.floor((timeLeft % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
      const minutes = Math.floor((timeLeft % (1000 * 60 * 60)) / (1000 * 60));
      const seconds = Math.floor((timeLeft % (1000 * 60)) / 1000);

      timer.innerHTML = `or wait: ${days}d ${hours}h ${minutes}m ${seconds}s`;
    }

    function moveGiftRandomly() {
      const x = Math.random() * (window.innerWidth - 60);
      const y = Math.random() * (window.innerHeight - 60);
      gift.style.left = `${x}px`;
      gift.style.top = `${y}px`;
    }

    function typeWriterEffect(text, i = 0) {
      const target = document.getElementById('typewriter');
      if (i === 0) startFloatingHearts();
      if (i < text.length) {
        target.innerHTML += text.charAt(i);
        setTimeout(() => typeWriterEffect(text, i + 1), 50);
      }
    }

    function startFloatingHearts() {
      setInterval(() => {
        const heart = document.createElement('div');
        heart.classList.add('heart');
        heart.style.left = `${Math.random() * 100}%`;
        heart.style.animationDuration = `${4 + Math.random() * 4}s`;
        heartsContainer.appendChild(heart);
        setTimeout(() => heart.remove(), 8000);
      }, 300);
    }

    gift.addEventListener('click', () => {
      gameSection.style.display = 'none';
      letter.classList.remove('hidden');
      setTimeout(() => letter.classList.add('show'), 10);
      fireConfetti();
      music.play();
      setTimeout(() => {
        typeWriterEffect(`Happy Birthday! 🎂✨\n\nHope your day is packed with all the happiness and love you deserve ---- Wishing you the best always and may this year bring you amazing moments 💖\n\nMay every step you take lead to more peace, success, and beautiful memories --- kntmna lik bzf dyal khir f hayatk 💫\n\nIt's YOUR day today ---- enjoy every moment 🤞🥀`);
      }, 1000);
    });

    function fireConfetti() {
      if (!confettiTriggered) {
        confettiTriggered = true;
        const duration = 5 * 1000;
        const animationEnd = Date.now() + duration;
        const defaults = { startVelocity: 30, spread: 360, ticks: 60, zIndex: 1000 };

        function randomInRange(min, max) {
          return Math.random() * (max - min) + min;
        }

        const interval = setInterval(() => {
          const timeLeft = animationEnd - Date.now();

          if (timeLeft <= 0) {
            return clearInterval(interval);
          }

          const particleCount = 50 * (timeLeft / duration);
          confetti({
            particleCount,
            origin: {
              x: randomInRange(0.1, 0.9),
              y: Math.random() - 0.2
            },
            ...defaults
          });
        }, 250);
      }
    }

    setInterval(updateTimer, 1000);
    setInterval(moveGiftRandomly, 2000);
  </script>
</body>
</html>
