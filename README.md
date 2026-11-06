# car-
BMW
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>CAR RACING GAME</title>

<style>
body {
margin: 0;
background: #222;
color: white;
text-align: center;
font-family: Arial;
}

h1 {
color: yellow;
}

#game {
width: 300px;
height: 500px;
background: #444;
margin: auto;
position: relative;
overflow: hidden;
border: 5px solid white;
}

.car {
width: 45px;
height: 75px;
position: absolute;
border-radius: 10px;
font-size: 35px;
text-align: center;
}

#player {
bottom: 20px;
left: 125px;
}

.enemy {
top: -80px;
}

button {
background: yellow;
padding: 15px 25px;
margin: 10px;
border: none;
border-radius: 10px;
font-size: 20px;
font-weight: bold;
}

#score {
color: yellow;
font-size: 25px;
}
</style>
</head>

<body>

<h1>🏎️ CAR RACING 🏎️</h1>

<h2>Score: <span id="score">0</span></h2>

<div id="game">
<div id="player" class="car">🚗</div>
</div>

<button onclick="move(-1)">⬅️ LEFT</button>
<button onclick="move(1)">RIGHT ➡️</button>

<br>
<button onclick="restart()">RESTART</button>

<h2 id="result">START GAME!</h2>

<script>
const game = document.getElementById("game");
const player = document.getElementById("player");
const scoreText = document.getElementById("score");
const result = document.getElementById("result");

let x = 125;
let score = 0;
let enemies = [];
let running = false;
let animation;

function move(direction) {
if (!running) return;
x += direction * 30;
x = Math.max(5, Math.min(250, x));
player.style.left = x + "px";
}

document.addEventListener("keydown", e => {
if (e.key === "ArrowLeft") move(-1);
if (e.key === "ArrowRight") move(1);
});

function createEnemy() {
const enemy = document.createElement("div");
enemy.className = "car enemy";
enemy.innerHTML = "🚙";
enemy.style.left = Math.floor(Math.random() * 250) + "px";
game.appendChild(enemy);
enemies.push({element: enemy, y: -80});
}

let frame = 0;

function loop() {
if (!running) return;

frame++;

if (frame % 65 === 0) createEnemy();

enemies.forEach((enemy, index) => {
enemy.y += 5;
enemy.element.style.top = enemy.y + "px";

let enemyX = parseInt(enemy.element.style.left);

if (
enemy.y > 405 &&
enemy.y < 490 &&
Math.abs(enemyX - x) < 40
) {
endGame();
}

if (enemy.y > 510) {
enemy.element.remove();
enemies.splice(index, 1);
score++;
scoreText.innerText = score;
}
});

if (running) animation = requestAnimationFrame(loop);
}

function start() {
if (running) return;
running = true;
result.innerText = "GO! 🏁";
loop();
}

function endGame() {
running = false;
cancelAnimationFrame(animation);
result.innerText = "GAME OVER! Score: " + score;
}

function restart() {
cancelAnimationFrame(animation);
enemies.forEach(e => e.element.remove());
enemies = [];
x = 125;
score = 0;
frame = 0;
scoreText.innerText = 0;
player.style.left = x + "px";
result.innerText = "GO! 🏁";
running = false;
start();
}

start();
</script>

</body>
</html>
