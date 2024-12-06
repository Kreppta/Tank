# Tank
Tanks288-game
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Пиксельные Танки</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #f0f0f0;
        }
        canvas {
            background: #eee;
            border: 2px solid #333;
        }
    </style>
</head>
<body>
    <canvas id="gameCanvas" width="800" height="600"></canvas>
    <script src="game.js"></script>
</body>
</html>
const canvas = document.getElementById("gameCanvas");
const ctx = canvas.getContext("2d");

let tank = {
    x: 400,
    y: 300,
    width: 50,
    height: 50,
    speed: 5,
    dx: 0,
    dy: 0
};

let bullets = [];

document.addEventListener("keydown", moveTank);
document.addEventListener("keyup", stopTank);

function moveTank(e) {
    if (e.key === "ArrowUp") tank.dy = -tank.speed;
    if (e.key === "ArrowDown") tank.dy = tank.speed;
    if (e.key === "ArrowLeft") tank.dx = -tank.speed;
    if (e.key === "ArrowRight") tank.dx = tank.speed;
    if (e.key === " " && bullets.length < 5) fireBullet();
}

function stopTank(e) {
    if (e.key === "ArrowUp" || e.key === "ArrowDown") tank.dy = 0;
    if (e.key === "ArrowLeft" || e.key === "ArrowRight") tank.dx = 0;
}

function fireBullet() {
    bullets.push({
        x: tank.x + tank.width / 2,
        y: tank.y,
        width: 5,
        height: 10,
        speed: 7
    });
}

function update() {
    moveTankPosition();
    updateBullets();
    clearCanvas();
    drawTank();
    drawBullets();
    requestAnimationFrame(update);
}

function moveTankPosition() {
    tank.x += tank.dx;
    tank.y += tank.dy;

    if (tank.x < 0) tank.x = 0;
    if (tank.x > canvas.width - tank.width) tank.x = canvas.width - tank.width;
    if (tank.y < 0) tank.y = 0;
    if (tank.y > canvas.height - tank.height) tank.y = canvas.height - tank.height;
}

function updateBullets() {
    for (let i = 0; i < bullets.length; i++) {
        bullets[i].y -= bullets[i].speed;
        if (bullets[i].y < 0) bullets.splice(i, 1);
    }
}

function clearCanvas() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
}

function drawTank() {
    ctx.fillStyle = "green";
    ctx.fillRect(tank.x, tank.y, tank.width, tank.height);
}

function drawBullets() {
    ctx.fillStyle = "red";
    for (let bullet of bullets) {
        ctx.fillRect(bullet.x - bullet.width / 2, bullet.y, bullet.width, bullet.height);
    }
}

update();
