
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Water Drop Defender – charity: water mini-game</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;700&display=swap" rel="stylesheet">
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      background: #b3ecff;
      font-family: 'Montserrat', Arial, sans-serif;
      display: flex; justify-content: center; align-items: center;
      height: 100vh;
    }
    #game {
      width: 360px; height: 640px;
      background: linear-gradient(to top, #d0f0ff, #80d4ff);
      border: 10px solid black; border-radius: 30px;
      position: relative; overflow: hidden;
    }
    .notify50 {
      position: absolute;
      left: 50%;
      top: 40%;
      transform: translate(-50%, -50%);
      background: #fff8c6;
      color: #0077cc;
      font-size: 22px;
      font-weight: bold;
      padding: 16px 32px;
      border-radius: 20px;
      box-shadow: 0 2px 16px #0077cc33;
      z-index: 1001;
      opacity: 0;
      pointer-events: none;
      transition: opacity 0.5s;
    }
    h1 { text-align: center; color: #004466; margin-top: 10px; }
    #subtitle {
      text-align: center; font-size: 14px;
      color: #004466; margin-bottom: 15px;
    }
    #scoreboard {
      position: absolute; top: 80px; left: 10px;
      background: #ffffffdd; padding: 6px 12px;
      border-radius: 10px; font-size: 16px;
      color: #003344; display: flex; flex-direction: column; align-items: center;
      gap: 6px; box-shadow: 0 0 5px rgba(0,0,0,0.3);
    }
    #scoreboard img { width: 20px; height: 20px; }
    #lives {
      position: absolute; top: 80px; right: 10px;
      font-size: 20px; color: red;
    }
    #streak {
      margin-top: 4px;
      background: #fff8c6;
      color: #b8860b;
      padding: 2px 10px;
      border-radius: 12px;
      font-size: 15px;
      font-weight: bold;
      box-shadow: 0 2px 8px #ffe06644;
      z-index: 3;
      display: none;
      position: static;
      transform: none;
    }
    .drop {
      position: absolute; top: -50px;
      width: 30px; height: 40px;
      background-size: contain; background-repeat: no-repeat;
      background-position: center;
    }
    #player {
      position: absolute; bottom: 100px;
      left: 50%; transform: translateX(-50%);
      width: 60px; height: 80px;
      background: 
        linear-gradient(to bottom, #e6f3ff 0%, #b3d9ff 30%, #80ccff 70%, #4da6ff 100%),
        url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path fill=\"%23ffffff\" d=\"M12,2A10,10 0 0,1 22,12A10,10 0 0,1 12,22A10,10 0 0,1 2,12A10,10 0 0,1 12,2M12,4A8,8 0 0,0 4,12A8,8 0 0,0 12,20A8,8 0 0,0 20,12A8,8 0 0,0 12,4Z\"/></svg>') no-repeat center/60%;
      border: 3px solid #2c5282;
      border-radius: 12px 12px 8px 8px;
      z-index: 3;
      position: absolute;
    }
    .splash {
      position: absolute;
      width: 40px;
      height: 40px;
      pointer-events: none;
      z-index: 1000;
      background: radial-gradient(circle at 60% 40%, #b3ecff 60%, #4da6ff 100%);
      border-radius: 50%;
      opacity: 0.8;
      animation: splashpop 0.5s ease-out forwards;
    }
    @keyframes splashpop {
      0% { transform: scale(0.5); opacity: 0.8; }
      60% { transform: scale(1.2); opacity: 1; }
      100% { transform: scale(1.5); opacity: 0; }
    }
    #player::before {
      content: "💧";
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      font-size: 24px;
      z-index: 4;
    }
    #sliderBar {
      position: absolute; bottom: 70px; left: 10px; right: 10px;
      height: 20px; background: #888; border-radius: 10px;
      z-index: 1;
    }
    #sliderHandle {
      width: 30px; height: 20px; background: yellow;
      border-radius: 50%; position: absolute; top: 0;
      left: 50%; transform: translateX(-50%);
      cursor: pointer;
      z-index: 2;
    }
    #branding {
      position: absolute; bottom: 10px; width: 100%;
      text-align: center; font-family: 'Montserrat', serif;
      background: #fff1a8; padding: 10px 0;
      display: flex;
      justify-content: center;
      align-items: center;
      gap: 15px;
    }
    #branding a {
      color: #003344;
      text-decoration: none;
      font-weight: bold;
      display: flex;
      align-items: center;
      gap: 5px;
    }
    #branding a:hover {
      text-decoration: underline;
    }
    #branding img { width: 20px; vertical-align: middle; }
    #endScreen {
      position: absolute; top: 0; left: 0;
      width: 100%; height: 100%;
      background: rgba(255,255,255,0.95);
      display: none; flex-direction: column;
      justify-content: center; align-items: center;
      padding: 20px; text-align: center; z-index: 999;
    }
    #endScreen h2 { color: #003344; margin-bottom: 15px; }
    #endScreen p { margin: 10px 0; }
    #restartBtn {
      padding: 10px 20px; background: #0077cc;
      color: white; border: none; border-radius: 10px;
      cursor: pointer; font-size: 16px;
    }
    #winScreen {
      position: absolute; top: 0; left: 0;
      width: 100%; height: 100%;
      background: rgba(200, 255, 200, 0.95);
      display: none; flex-direction: column;
      justify-content: center; align-items: center;
      padding: 20px; text-align: center; z-index: 999;
    }
    #winScreen h2 { color: #006400; font-size: 32px; margin-bottom: 15px; }
    #winScreen p { margin: 10px 0; }
    #winRestartBtn {
      padding: 10px 20px; background: #28a745;
      color: white; border: none; border-radius: 10px;
      cursor: pointer; font-size: 16px;
    }
    #ingame-restart-btn {
      position: absolute;
      top: 115px;
      right: 10px;
      padding: 5px 10px;
      background: #ffc107;
      color: #003344;
      border: 1px solid #003344;
      border-radius: 10px;
      cursor: pointer;
      font-size: 13px;
      font-weight: bold;
      z-index: 100;
      display: none; /* Initially hidden */
    }
    /* Difficulty Modal */
    #difficultyModal {
      position: absolute; top: 0; left: 0; width: 100%; height: 100%;
      background: rgba(255,255,255,0.97);
      display: flex; flex-direction: column; justify-content: center; align-items: center;
      z-index: 2000;
    }
    #difficultyModal h2 {
      color: #003344; margin-bottom: 20px; font-size: 26px;
      font-family: Arial, sans-serif;
    }
    .difficulty-btn {
      padding: 12px 30px; margin: 10px 0;
      font-size: 18px; border: none; border-radius: 10px;
      background: #0077cc; color: white; cursor: pointer;
      font-family: 'Montserrat', Arial, sans-serif;
      transition: background 0.2s;
    }
    .difficulty-btn:hover { background: #005fa3; }
  </style>
</head>
<body>
  <div id="game">
    <div id="difficultyModal">
      <h2>Select Difficulty</h2>
      <button class="difficulty-btn" data-mode="easy">Easy</button>
      <button class="difficulty-btn" data-mode="normal">Normal</button>
      <button class="difficulty-btn" data-mode="hard">Hard</button>
    </div>
    <div id="notify50" class="notify50"></div>
    <h1>WATER DROP</h1>
    <div id="subtitle">Catch clean water, avoid pollution!</div>
    <div id="scoreboard">
      <img src="https://th.bing.com/th/id/R.5da03ddeed93baf0c9402b6f3a017c1e?rik=gfa31ed3XZRSPw&pid=ImgRaw&r=0" alt="Water Drop" />
      <span id="score">0</span>
      <div id="streak"></div>
    </div>
    <div id="lives">❤️❤️❤️</div>
    <button id="ingame-restart-btn">Restart</button>
    <div id="player"></div>
    <div id="sliderBar"><div id="sliderHandle"></div></div>
    <div id="branding">
      <a href="https://www.charitywater.org/" target="_blank">
        <img src="https://cdn-icons-png.flaticon.com/512/1975/1975682.png" alt="charity water logo" /> 
        charity: water
      </a>
      <a href="https://www.charitywater.org/donate" target="_blank">
        Donate
      </a>
    </div>
    <div id="endScreen">
      <h2>Game Over</h2>
      <p id="finalScore">Score: 0</p>
      <p id="timePlayed">Time Played: 0s</p>
      <button id="restartBtn">Restart Game</button>
    </div>
    <div id="winScreen">
        <h2>You Win!</h2>
        <p id="winMessage">You've collected 100 clean water drops!</p>
        <p id="winTimePlayed">Time Played: 0s</p>
        <button id="winRestartBtn">Play Again</button>
    </div>
    <!-- Sound effects -->
    <audio id="sfx-collect" src="collect.mp3" preload="auto"></audio>
    <audio id="sfx-miss" src="miss.mp3" preload="auto"></audio>
    <audio id="sfx-win" src="win.mp3" preload="auto"></audio>
    <audio id="sfx-btn" src="button.mp3" preload="auto"></audio>
    <audio id="sfx-polluted" src="https://cdn.freesound.org/previews/24/24467_136190-lq.mp3" preload="auto"></audio>
  </div>

  <script>
    // Difficulty settings
    const difficulties = {
      easy:   { spawnRate: 1200, pollutedRate: 0.15, winScore: 50 },
      normal: { spawnRate: 800,  pollutedRate: 0.25, winScore: 100 },
      hard:   { spawnRate: 500,  pollutedRate: 0.40, winScore: 150 }
    };
    let currentDifficulty = difficulties.normal;
    let spawnInterval = null;

    const milestones = [
        { score: 25, message: "Great start! 25 drops!" },
        { score: 50, message: "50 drops! Awesome!" },
        { score: 75, message: "75! You're a pro!" },
        { score: 100, message: "100! Incredible!" },
        { score: 125, message: "125! Unstoppable!" },
    ];
    const triggeredMilestones = new Set();

    function playSFX(id) {
      const el = document.getElementById(id);
      if (el) { el.currentTime = 0; el.play(); }
    }

    function showSplash(x, y) {
      const splash = document.createElement('div');
      splash.className = 'splash';
      splash.style.left = (x - game.getBoundingClientRect().left - 20) + 'px';
      splash.style.top = (y - game.getBoundingClientRect().top - 10) + 'px';
      game.appendChild(splash);
      setTimeout(() => {
        if (splash.parentNode) splash.parentNode.removeChild(splash);
      }, 500);
    }

    function showLifeLost(x, y) {
        const lifeLostEl = document.createElement('div');
        lifeLostEl.textContent = '-1 ❤️';
        lifeLostEl.style.position = 'absolute';
        lifeLostEl.style.left = (x - game.getBoundingClientRect().left) + 'px';
        lifeLostEl.style.top = (y - game.getBoundingClientRect().top) + 'px';
        lifeLostEl.style.color = 'red';
        lifeLostEl.style.fontSize = '20px';
        lifeLostEl.style.fontWeight = 'bold';
        lifeLostEl.style.zIndex = 1002;
        lifeLostEl.style.transition = 'transform 1s, opacity 1s';
        lifeLostEl.style.pointerEvents = 'none';
        game.appendChild(lifeLostEl);
        setTimeout(() => {
            lifeLostEl.style.transform = 'translateY(-50px)';
            lifeLostEl.style.opacity = '0';
        }, 100);
        setTimeout(() => {
            if (lifeLostEl.parentElement) game.removeChild(lifeLostEl);
        }, 1100);
    }

    function showExtraLife(x, y) {
        const extraLifeEl = document.createElement('div');
        extraLifeEl.textContent = '+1 ❤️';
        extraLifeEl.style.position = 'absolute';
        extraLifeEl.style.left = (x - game.getBoundingClientRect().left) + 'px';
        extraLifeEl.style.top = (y - game.getBoundingClientRect().top) + 'px';
        extraLifeEl.style.color = 'green';
        extraLifeEl.style.fontSize = '20px';
        extraLifeEl.style.fontWeight = 'bold';
        extraLifeEl.style.zIndex = 1002;
        extraLifeEl.style.transition = 'transform 1s, opacity 1s';
        extraLifeEl.style.pointerEvents = 'none';
        game.appendChild(extraLifeEl);
        setTimeout(() => {
            extraLifeEl.style.transform = 'translateY(-50px)';
            extraLifeEl.style.opacity = '0';
        }, 100);
        setTimeout(() => {
            if (extraLifeEl.parentElement) game.removeChild(extraLifeEl);
        }, 1100);
    }
    
    const notify50 = document.getElementById('notify50');
    function showMilestone(message) {
      notify50.textContent = message;
      notify50.style.opacity = '1';
      setTimeout(() => {
        notify50.style.opacity = '0';
      }, 1800);
    }
    let lastScore = localStorage.getItem('lastScore') || 0;
    const streakEl = document.getElementById('streak');
    function showStreak() {
      if (lastScore > 0) {
        streakEl.textContent = `Last Score: ${lastScore}`;
        streakEl.style.display = 'block';
      } else {
        streakEl.style.display = 'none';
      }
    }
    showStreak();
    const maxDrops = 5;
    let activeDrops = 0;

    const player = document.getElementById('player');
    const handle = document.getElementById('sliderHandle');
    const bar = document.getElementById('sliderBar');
    const scoreEl = document.getElementById('score');
    const livesEl = document.getElementById('lives');
    const game = document.getElementById('game');
    const endScreen = document.getElementById('endScreen');
    const finalScore = document.getElementById('finalScore');
    const timePlayed = document.getElementById('timePlayed');
    const restartBtn = document.getElementById('restartBtn');
    const difficultyModal = document.getElementById('difficultyModal');
    const ingameRestartBtn = document.getElementById('ingame-restart-btn');
    const winScreen = document.getElementById('winScreen');
    const winMessage = document.getElementById('winMessage');
    const winTimePlayed = document.getElementById('winTimePlayed');
    const winRestartBtn = document.getElementById('winRestartBtn');

    let score = 0;
    let lives = 3;
    const maxLives = 5;
    let isDragging = false;
    let gameStartTime = Date.now();

    // Keep track of drop positions so they don't spawn on top of each other
    const dropPositions = [];

    // Difficulty selection
    document.querySelectorAll('.difficulty-btn').forEach(btn => {
      btn.onclick = function() {
        playSFX('sfx-btn');
        const mode = btn.getAttribute('data-mode');
        currentDifficulty = difficulties[mode];
        difficultyModal.style.display = 'none';
        startGame();
      };
    });

    // Mouse events
    handle.addEventListener('mousedown', () => isDragging = true);
    document.addEventListener('mouseup', () => isDragging = false);
    document.addEventListener('mousemove', e => {
      if (!isDragging) return;
      const rect = bar.getBoundingClientRect();
      let x = e.clientX - rect.left;
      x = Math.max(0, Math.min(x, rect.width));
      handle.style.left = x + 'px';
      const leftPercent = (x / rect.width * 100);
      player.style.left = leftPercent + '%';
      player.style.transform = 'translateX(-50%)';
    });

    function canSpawnAt(xPercent) {
      for (const pos of dropPositions) {
        if (Math.abs(pos - xPercent) < 12) {
          return false;
        }
      }
      return true;
    }

    function spawnDrop() {
      if (activeDrops >= maxDrops) return;

      let spawnX;
      let attempts = 0;
      do {
        spawnX = Math.random() * 90;
        attempts++;
        if (attempts > 20) break;
      } while (!canSpawnAt(spawnX));

      dropPositions.push(spawnX);
      activeDrops++;

      const drop = document.createElement('div');
      const isPolluted = Math.random() < currentDifficulty.pollutedRate;
      drop.className = 'drop';
      drop.style.left = `${spawnX}%`;
      drop.collected = false;
      
      if (isPolluted) {
        drop.style.background = 'radial-gradient(circle, #8B4513 0%, #654321 100%)';
        drop.style.borderRadius = '50% 50% 50% 50% / 60% 60% 40% 40%';
      } else {
        drop.style.backgroundImage = "url('https://th.bing.com/th/id/R.5da03ddeed93baf0c9402b6f3a017c1e?rik=gfa31ed3XZRSPw&pid=ImgRaw&r=0')";
        drop.style.backgroundSize = "contain";
        drop.style.backgroundRepeat = "no-repeat";
        drop.style.backgroundPosition = "center";
      }
      game.appendChild(drop);

      let y = 0;
      const fallSpeed = 4;
      const interval = setInterval(() => {
        y += fallSpeed;
        drop.style.top = y + 'px';

        const dr = drop.getBoundingClientRect();
        const pr = player.getBoundingClientRect();

        if (!drop.collected && dr.bottom > pr.top && dr.top < pr.bottom && dr.left < pr.right && dr.right > pr.left && y > 0) {
          drop.collected = true;
          if (isPolluted) {
            lives--;
            updateLives();
            showLifeLost(dr.left, dr.top);
            playSFX('sfx-polluted');
            if (lives === 0) {
              playSFX('sfx-win');
              endGame();
            }
          } else {
            score++;
            scoreEl.textContent = score;
            showSplash(dr.left + dr.width / 2, dr.top + dr.height / 2);
            playSFX('sfx-collect');
            
            const milestone = milestones.find(m => m.score === score);
            if (milestone && !triggeredMilestones.has(score)) {
                showMilestone(milestone.message);
                triggeredMilestones.add(score);
            }

            if (score >= currentDifficulty.winScore) {
              winGame();
            } else {
              if (score % 10 === 0 && lives < maxLives) {
                lives++;
                updateLives();
                showExtraLife(dr.left, dr.top);
              }
            }
          }
          clearDrop();
        } else if (y > game.clientHeight) {
          if (!drop.collected && !isPolluted) {
            playSFX('sfx-miss');
          }
          clearDrop();
        }
      }, 30);

      function clearDrop() {
        clearInterval(interval);
        drop.remove();
        activeDrops--;
        const index = dropPositions.indexOf(spawnX);
        if (index > -1) dropPositions.splice(index, 1);
      }
    }

    function updateLives() {
      let heartsDisplay = '';
      for (let i = 0; i < lives; i++) {
        heartsDisplay += '❤️';
      }
      for (let i = lives; i < maxLives; i++) {
        heartsDisplay += '🤍';
      }
      heartsDisplay += ` (${lives}/${maxLives})`;
      livesEl.innerHTML = heartsDisplay;
    }

    function endGame() {
      const elapsed = Math.floor((Date.now() - gameStartTime) / 1000);
      finalScore.textContent = `Score: ${score}`;
      timePlayed.textContent = `Time Played: ${elapsed}s`;
      endScreen.style.display = 'flex';
      isDragging = false;
      clearInterval(spawnInterval);
      document.querySelectorAll('.drop').forEach(d => d.remove());
      localStorage.setItem('lastScore', score);
      lastScore = score;
      showStreak();
      ingameRestartBtn.style.display = 'none';
    }

    function winGame() {
        playSFX('sfx-win');
        const elapsed = Math.floor((Date.now() - gameStartTime) / 1000);
        winMessage.textContent = `You collected ${currentDifficulty.winScore} clean water drops!`;
        winTimePlayed.textContent = `Time Played: ${elapsed}s`;
        winScreen.style.display = 'flex';
        isDragging = false;
        clearInterval(spawnInterval);
        document.querySelectorAll('.drop').forEach(d => d.remove());
        localStorage.setItem('lastScore', score);
        lastScore = score;
        showStreak();
        ingameRestartBtn.style.display = 'none';
    }

    restartBtn.onclick = () => {
      playSFX('sfx-btn');
      location.reload();
    };

    winRestartBtn.onclick = () => {
      playSFX('sfx-btn');
      location.reload();
    };

    ingameRestartBtn.onclick = () => {
      playSFX('sfx-btn');
      location.reload();
    };

    function startGame() {
      score = 0;
      lives = 3;
      activeDrops = 0;
      triggeredMilestones.clear();
      scoreEl.textContent = score;
      updateLives();
      gameStartTime = Date.now();
      document.getElementById('subtitle').innerHTML = `Catch clean water, avoid pollution!<br><b>Reach ${currentDifficulty.winScore} to win!</b>`;
      showStreak();
      if (spawnInterval) clearInterval(spawnInterval);
      spawnInterval = setInterval(spawnDrop, currentDifficulty.spawnRate);
      ingameRestartBtn.style.display = 'block';
    }

    updateLives();
  </script>
</body>
</html>
