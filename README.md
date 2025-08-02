<!DOCTYPE html>
<html>
<head>
  <title>Mobile Shooter Game</title>
  <style>
    body { margin: 0; background: #000; overflow: hidden; }
    canvas { display: block; margin: auto; background: #111; }

    .controls {
      display: flex;
      justify-content: space-around;
      margin-top: 10px;
    }

    .controls button {
      flex: 1;
      padding: 20px;
      font-size: 18px;
      margin: 2px;
      background: #333;
      color: #fff;
      border: none;
      border-radius: 5px;
    }
  </style>
</head>
<body>

<canvas id=”gameCanvas” width=”480” height=”640”></canvas>

<div class=”controls”>
  <button id=”leftBtn”>⬅️</button>
  <button id=”fireBtn”>🔥 Fire</button>
  <button id=”rightBtn”>➡️</button>
</div>

<script>
  const canvas = document.getElementById(”gameCanvas”);
  const ctx = canvas.getContext(”2d”);

  let ship = { x: 220, y: 580, width: 40, height: 20 };
  let bullets = [];
  let enemies = [];

  // Touch control buttons
  const leftBtn = document.getElementById(”leftBtn”);
  const rightBtn = document.getElementById(”rightBtn”);
  const fireBtn = document.getElementById(”fireBtn”);

  leftBtn.addEventListener(”touchstart”, () => ship.x -= 20);
  rightBtn.addEventListener(”touchstart”, () => ship.x += 20);
  fireBtn.addEventListener(”touchstart”, shoot);

  fireBtn.addEventListener(”click”, shoot);
  leftBtn.addEventListener(”click”, () => ship.x -= 20);
  rightBtn.addEventListener(”click”, () => ship.x += 20);

  function shoot() {
    bullets.push({ x: ship.x + ship.width / 2 - 2, y: ship.y, width: 4, height: 10 });
  }

  function drawShip() {
    ctx.fillStyle = “lime”;
    ctx.fillRect(ship.x, ship.y, ship.width, ship.height);
  }

  function drawBullets() {
    ctx.fillStyle = “red”;
    bullets.forEach((b, i) => {
      b.y -= 8;
      ctx.fillRect(b.x, b.y, b.width, b.height);
      if (b.y < 0) bullets.splice(i, 1);
    });
  }

  function drawEnemies() {
    if (Math.random() < 0.02) {
      enemies.push({ x: Math.random() * 440, y: 0, width: 40, height: 20 });
    }

    ctx.fillStyle = “yellow”;
    enemies.forEach((e, i) => {
      e.y += 2;
      ctx.fillRect(e.x, e.y, e.width, e.height);
      if (e.y > canvas.height) enemies.splice(i, 1);
    });
  }

  function detectCollision() {
    enemies.forEach((enemy, ei) => {
      bullets.forEach((bullet, bi) => {
        if (
          bullet.x < enemy.x + enemy.width &&
          bullet.x + bullet.width > enemy.x &&
          bullet.y < enemy.y + enemy.height &&
          bullet.y + bullet.height > enemy.y
        ) {
          enemies.splice(ei, 1);
          bullets.splice(bi, 1);
        }
      });
    });
  }

  function gameLoop() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    drawShip();
    drawBullets();
    drawEnemies();
    detectCollision();
    requestAnimationFrame(gameLoop);
  }

  gameLoop();
</script>

</body>
</html>
