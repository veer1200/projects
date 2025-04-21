<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Happy Birthday!</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      font-family: 'Comic Sans MS', cursive;
      background: linear-gradient(to top right, #ff9a9e, #fad0c4);
      color: #fff;
      text-align: center;
      overflow: hidden;
    }

    h1 {
      font-size: 2.5em;
      margin: 20px;
    }

    button {
      padding: 1em 2em;
      font-size: 1.1em;
      background-color: #ff6f91;
      border: none;
      color: white;
      border-radius: 12px;
      cursor: pointer;
      margin-top: 20px;
    }

    #game-container {
      margin-top: 30px;
      position: relative;
      height: 60vh;
      overflow: hidden;
    }

    .balloon {
      position: absolute;
      width: 50px;
      height: 70px;
      border-radius: 50% 50% 45% 45%;
      cursor: pointer;
      animation: floatUp 6s linear forwards;
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    .string {
      width: 2px;
      height: 20px;
      background: #fff;
      margin-top: -10px;
    }

    @keyframes floatUp {
      from { bottom: -100px; }
      to { bottom: 100%; }
    }

    #scoreboard {
      font-size: 1.2em;
      margin-top: 10px;
    }

    #gift-section {
      display: none;
      margin-top: 30px;
      padding: 20px;
    }

    #gift-message {
      font-size: 1.8em;
      margin-top: 40px;
      color: #fff;
      text-shadow: 2px 2px 8px #ff6f91;
      animation: pulse 2s infinite ease-in-out;
    }

    @keyframes pulse {
      0% { transform: scale(1); opacity: 1; }
      50% { transform: scale(1.1); opacity: 0.8; }
      100% { transform: scale(1); opacity: 1; }
    }

    #message {
      font-size: 1.2em;
      margin: 20px;
    }

    canvas {
      pointer-events: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: 9999;
    }

    /* Responsive font scaling */
    @media (max-width: 600px) {
      h1 {
        font-size: 2em;
      }

      button {
        font-size: 1em;
        padding: 0.8em 1.5em;
      }

      .balloon {
        width: 40px;
        height: 60px;
      }

      #scoreboard {
        font-size: 1em;
      }
    }
  </style>
</head>
<body>
  <h1>Happy Birthday, Kajal!</h1>
  <div id="message">
    <p>Wishing you a day filled with laughter, joy, and all the cake you can handle!</p>
    <p>You're not just a year older, you're a year cooler. 🎉</p>
  </div>

  <!-- Game Section -->
  <div id="game-container">
    <p>Pop the balloons to score! 🎈</p>
    <button id="startBtn" onclick="startGame()">Start Game</button>
    <div id="scoreboard">Score: <span id="score">0</span></div>
  </div>

  <!-- Gift Section -->
  <div id="gift-section">
    <p id="gift-message">🎉 Check Your Pocket 🎉</p>
    <audio id="bg-music" autoplay loop>
      <source src="https://www.bensound.com/bensound-music/bensound-sunny.mp3" type="audio/mpeg">
      Your browser does not support the audio tag.
    </audio>
  </div>

  <canvas id="confetti-canvas"></canvas>

  <!-- Confetti Script -->
  <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>

  <script>
    const scoreDisplay = document.getElementById("score");
    const gameContainer = document.getElementById("game-container");
    const startBtn = document.getElementById("startBtn");
    const giftSection = document.getElementById("gift-section");
    const bgMusic = document.getElementById("bg-music");

    let score = 0;
    let balloonInterval;
    let gameDuration = 15000;

    const balloonColors = ['#FF6F91', '#FF9671', '#FFC75F', '#F9F871', '#A0E7E5', '#B39CD0'];

    function createBalloon() {
      const balloon = document.createElement("div");
      balloon.classList.add("balloon");

      const color = balloonColors[Math.floor(Math.random() * balloonColors.length)];
      balloon.style.background = color;
      balloon.style.left = Math.random() * (window.innerWidth - 60) + "px";

      const string = document.createElement("div");
      string.classList.add("string");
      balloon.appendChild(string);

      balloon.addEventListener('click', popBalloon);
      balloon.addEventListener('touchstart', popBalloon);

      gameContainer.appendChild(balloon);

      setTimeout(() => {
        balloon.remove();
      }, 6000);
    }

    function popBalloon(event) {
      event.target.closest('.balloon')?.remove();
      score += 10;
      scoreDisplay.textContent = score;
    }

    function startGame() {
      score = 0;
      scoreDisplay.textContent = score;
      giftSection.style.display = "none";
      gameContainer.style.display = "block";
      bgMusic.pause();
      bgMusic.currentTime = 0;

      balloonInterval = setInterval(createBalloon, 600);

      setTimeout(() => {
        endGame();
      }, gameDuration);
    }

    function endGame() {
      clearInterval(balloonInterval);
      document.querySelectorAll(".balloon").forEach(b => b.remove());

      if (score > 190) {
        showGift();
      } else {
        alert("Time's up! Your score is: " + score);
        const playAgain = confirm("Want to try again?");
        if (playAgain) {
          startGame();
        } else {
          alert("Thanks for playing! 🎈");
        }
      }
    }

    function showGift() {
      gameContainer.style.display = "none";
      giftSection.style.display = "block";
      bgMusic.play();
      launchConfetti();
    }

    function launchConfetti() {
      const duration = 5 * 1000;
      const end = Date.now() + duration;

      (function frame() {
        confetti({
          particleCount: 5,
          angle: 60,
          spread: 100,
          origin: { x: 0 }
        });
        confetti({
          particleCount: 5,
          angle: 120,
          spread: 100,
          origin: { x: 1 }
        });

        if (Date.now() < end) {
          requestAnimationFrame(frame);
        }
      })();
    }
  </script>
</body>
</html>
