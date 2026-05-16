# Baseball-The-Show-2026
<!DOCTYPE html>
<html>
<head>
  <title>Baseball Career Game</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>

<h1>⚾ Baseball Career Mode</h1>

<div id="ui">
  <p>Move mouse = PCI | X = Contact | D = Power</p>
  <p id="stats">Hits: 0 | HR: 0 | Level: High School</p>
</div>

<canvas id="game" width="700" height="400"></canvas>

<script src="script.js"></script>
</body>
</html>
body {
  background: #0b6623;
  color: white;
  text-align: center;
  font-family: Arial;
}

canvas {
  background: #1e7f3b;
  border: 3px solid white;
  margin-top: 10px;
}
const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

let mouse = { x: 350, y: 300 };

let ball = {
  x: Math.random() * 600 + 50,
  y: 50,
  speed: 3
};

// CAREER STATS
let hits = 0;
let hr = 0;
let level = "High School";

document.addEventListener("mousemove", (e) => {
  const rect = canvas.getBoundingClientRect();
  mouse.x = e.clientX - rect.left;
  mouse.y = e.clientY - rect.top;
});

// SWING CONTROLS
document.addEventListener("keydown", (e) => {
  if (e.key === "x") swing(false); // contact
  if (e.key === "d") swing(true);  // power
});

function swing(power) {
  let dist = Math.hypot(ball.x - mouse.x, ball.y - mouse.y);

  if (dist < 40) {
    if (power) {
      let result = Math.random();
      if (result < 0.4) {
        hr++;
        hits++;
        alert("💥 HOME RUN!");
      } else {
        hits++;
        alert("⚾ HIT!");
      }
    } else {
      hits++;
      alert("⚾ CONTACT HIT!");
    }
  } else {
    alert("❌ MISS");
  }

  updateStats();
  resetBall();
}

function resetBall() {
  ball.x = Math.random() * 600 + 50;
  ball.y = 50;
}

function updateStats() {
  document.getElementById("stats").innerText =
    `Hits: ${hits} | HR: ${hr} | Level: ${level}`;
}

// GAME LOOP
function update() {
  ball.y += ball.speed;

  if (ball.y > 400) {
    resetBall();
  }

  draw();
  requestAnimationFrame(update);
}

function draw() {
  ctx.clearRect(0, 0, 700, 400);

  // BALL
  ctx.fillStyle = "white";
  ctx.beginPath();
  ctx.arc(ball.x, ball.y, 8, 0, Math.PI * 2);
  ctx.fill();

  // PCI (mouse)
  ctx.strokeStyle = "red";
  ctx.strokeRect(mouse.x - 15, mouse.y - 15, 30, 30);
}

update();