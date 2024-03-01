const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');

const paddleWidth = 80;
const paddleHeight = 10;
const ballRadius = 10;

let paddleX = (canvas.width - paddleWidth) / 2;
let ballX = canvas.width / 2;
let ballY = canvas.height - 30;
let ballSpeedX = 5;
let ballSpeedY = -5;

function drawPaddle() {
    ctx.beginPath();
    ctx.rect(paddleX, canvas.height - paddleHeight, paddleWidth, paddleHeight);
    ctx.fillStyle = '#0095DD';
    ctx.fill();
    ctx.closePath();
}

function drawBall() {
    ctx.beginPath();
    ctx.arc(ballX, ballY, ballRadius, 0, Math.PI * 2);
    ctx.fillStyle = '#0095DD';
    ctx.fill();
    ctx.closePath();
}

function draw() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    drawPaddle();
    drawBall();

    ballX += ballSpeedX;
    ballY += ballSpeedY;

    if (ballX + ballRadius > canvas.width || ballX - ballRadius < 0) {
        ballSpeedX = -ballSpeedX;
    }

    if (ballY - ballRadius < 0) {
        ballSpeedY = -ballSpeedY;
    } else if (ballY + ballRadius > canvas.height - paddleHeight &&
        ballX > paddleX &&
        ballX < paddleX + paddleWidth) {
        ballSpeedY = -ballSpeedY;
    }

    if (ballY + ballRadius > canvas.height) {
        // Game over, reset the ball
        ballX = canvas.width / 2;
        ballY = canvas.height - 30;
    }

    requestAnimationFrame(draw);
}

function keyDownHandler(e) {
    if (e.key === 'Right' || e.key === 'ArrowRight') {
        paddleX += 10;
    } else if (e.key === 'Left' || e.key === 'ArrowLeft') {
        paddleX -= 10;
    }
}

document.addEventListener('keydown', keyDownHandler);

draw();

