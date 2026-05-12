<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Galaxy Fighter</title>

<style>

body{
    margin:0;
    font-family:Arial;
    background:#06121f;
    color:white;
    overflow-y:auto;
}

h1,p{
    text-align:center;
}

#gameBox{
    display:flex;
    justify-content:center;
    margin-top:20px;
}

canvas{
    background:black;
    border:4px solid white;
}

.info{
    width:80%;
    margin:auto;
    margin-top:40px;
    background:#111827;
    padding:25px;
    border-radius:15px;
    line-height:1.8;
}

button{
    padding:12px 22px;
    font-size:18px;
    border:none;
    border-radius:10px;
    background:lime;
    cursor:pointer;
}

.center{
    text-align:center;
    margin-top:20px;
}

</style>
</head>

<body>

<h1>🚀 Galaxy Fighter</h1>

<p>Made by Drilon Gashi</p>
<p>Teacher: Ambeljeta Ismaili</p>

<div id="gameBox">
    <canvas id="game" width="900" height="500"></canvas>
</div>

<div class="center">
    <button onclick="restartGame()">Restart Game</button>
</div>

<div class="info">

<h2>📖 About This Game</h2>

<p>
This is an advanced space game made using HTML, CSS and JavaScript.
</p>

<p>
Controls:
</p>

<ul>
<li>⬅️ Left Arrow = Move Left</li>
<li>➡️ Right Arrow = Move Right</li>
<li>SPACE = Shoot</li>
</ul>

<p>
You can scroll down normally while the game works above.
</p>

<p>
The enemies become faster when your score increases.
</p>

<p>
Try to survive and get the highest score possible.
</p>

</div>

<script>

const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

let score = 0;
let lives = 3;
let gameOver = false;
let enemySpeed = 2;

const player = {
    x: 420,
    y: 420,
    width: 60,
    height: 60,
    speed: 8
};

const bullets = [];
const enemies = [];

const keys = {};

document.addEventListener("keydown", function(e){

    // stop only left/right scrolling
    if(e.key === "ArrowLeft" || e.key === "ArrowRight"){
        e.preventDefault();
    }

    keys[e.key] = true;

    if(e.key === " " && !gameOver){

        bullets.push({
            x: player.x + 27,
            y: player.y,
            width: 6,
            height: 18
        });
    }
});

document.addEventListener("keyup", function(e){
    keys[e.key] = false;
});

function createEnemy(){

    enemies.push({
        x: Math.random() * 840,
        y: -50,
        width: 50,
        height: 50,
        speed: enemySpeed + Math.random()*2
    });
}

setInterval(function(){

    if(!gameOver){
        createEnemy();
    }

},700);

function update(){

    // movement
    if(keys["ArrowLeft"] && player.x > 0){
        player.x -= player.speed;
    }

    if(keys["ArrowRight"] && player.x < canvas.width-player.width){
        player.x += player.speed;
    }

    // bullets
    for(let i=0;i<bullets.length;i++){

        bullets[i].y -= 12;

        if(bullets[i].y < 0){
            bullets.splice(i,1);
            i--;
        }
    }

    // enemies
    for(let i=0;i<enemies.length;i++){

        enemies[i].y += enemies[i].speed;

        // bottom hit
        if(enemies[i].y > canvas.height){

            enemies.splice(i,1);

            lives--;

            i--;

            if(lives <= 0){
                gameOver = true;
            }

            continue;
        }

        // collision
        for(let j=0;j<bullets.length;j++){

            if(
                bullets[j].x < enemies[i].x + enemies[i].width &&
                bullets[j].x + bullets[j].width > enemies[i].x &&
                bullets[j].y < enemies[i].y + enemies[i].height &&
                bullets[j].y + bullets[j].height > enemies[i].y
            ){

                enemies.splice(i,1);
                bullets.splice(j,1);

                score += 10;

                if(score % 50 === 0){
                    enemySpeed += 0.5;
                }

                i--;
                break;
            }
        }
    }
}

function drawBackground(){

    for(let i=0;i<100;i++){

        ctx.fillStyle = "white";

        ctx.fillRect(
            Math.random()*canvas.width,
            Math.random()*canvas.height,
            2,
            2
        );
    }
}

function draw(){

    ctx.clearRect(0,0,canvas.width,canvas.height);

    drawBackground();

    // player
    ctx.fillStyle = "cyan";

    ctx.fillRect(
        player.x,
        player.y,
        player.width,
        player.height
    );

    // bullets
    ctx.fillStyle = "yellow";

    bullets.forEach(function(b){

        ctx.fillRect(
            b.x,
            b.y,
            b.width,
            b.height
        );
    });

    // enemies
    ctx.fillStyle = "red";

    enemies.forEach(function(e){

        ctx.fillRect(
            e.x,
            e.y,
            e.width,
            e.height
        );
    });

    // score
    ctx.fillStyle = "white";
    ctx.font = "24px Arial";

    ctx.fillText("Score: " + score,20,35);
    ctx.fillText("Lives: " + lives,20,70);

    if(gameOver){

        ctx.fillStyle = "white";
        ctx.font = "60px Arial";

        ctx.fillText("GAME OVER",250,250);

        ctx.font = "30px Arial";

        ctx.fillText(
            "Final Score: " + score,
            330,
            310
        );
    }
}

function gameLoop(){

    if(!gameOver){
        update();
    }

    draw();

    requestAnimationFrame(gameLoop);
}

function restartGame(){

    score = 0;
    lives = 3;
    enemySpeed = 2;
    gameOver = false;

    enemies.length = 0;
    bullets.length = 0;

    player.x = 420;
}

gameLoop();

</script>

</body>
</html>
