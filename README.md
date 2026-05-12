# Projekt-ne-teknologji
Loje e thjeshte punuar nga Drilon Gashi per shkollen"Jeronim de Rada"
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Space Battle Game</title>

<style>
    body{
        margin:0;
        overflow:hidden;
        background:black;
        color:white;
        font-family:Arial;
        text-align:center;
    }

    canvas{
        background:#111;
        display:block;
        margin:auto;
        border:3px solid white;
    }

    h1{
        margin-top:10px;
    }
</style>
</head>

<body>

<h1>🚀 Space Battle</h1>
<p>Made by Drilon Gashi</p>
<p>Teacher: Ambeljeta Ismaili</p>

<canvas id="game" width="800" height="500"></canvas>

<script>
const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

let score = 0;
let gameOver = false;

const player = {
    x: 370,
    y: 430,
    width: 60,
    height: 60,
    speed: 7
};

const bullets = [];
const enemies = [];

const keys = {};

document.addEventListener("keydown", (e)=>{
    keys[e.key] = true;

    if(e.key === " "){
        bullets.push({
            x: player.x + 27,
            y: player.y,
            width: 6,
            height: 15
        });
    }
});

document.addEventListener("keyup", (e)=>{
    keys[e.key] = false;
});

function spawnEnemy(){
    enemies.push({
        x: Math.random() * 740,
        y: -50,
        width: 50,
        height: 50,
        speed: 2 + Math.random()*3
    });
}

setInterval(()=>{
    if(!gameOver){
        spawnEnemy();
    }
},1000);

function update(){

    if(keys["ArrowLeft"] && player.x > 0){
        player.x -= player.speed;
    }

    if(keys["ArrowRight"] && player.x < canvas.width-player.width){
        player.x += player.speed;
    }

    for(let i=0;i<bullets.length;i++){
        bullets[i].y -= 10;

        if(bullets[i].y < 0){
            bullets.splice(i,1);
            i--;
        }
    }

    for(let i=0;i<enemies.length;i++){

        enemies[i].y += enemies[i].speed;

        if(enemies[i].y > canvas.height){
            gameOver = true;
        }

        for(let j=0;j<bullets.length;j++){

            if(
                bullets[j].x < enemies[i].x + enemies[i].width &&
                bullets[j].x + bullets[j].width > enemies[i].x &&
                bullets[j].y < enemies[i].y + enemies[i].height &&
                bullets[j].y + bullets[j].height > enemies[i].y
            ){
                enemies.splice(i,1);
                bullets.splice(j,1);

                score++;

                i--;
                break;
            }
        }
    }
}

function draw(){

    ctx.clearRect(0,0,canvas.width,canvas.height);

    // stars
    for(let i=0;i<100;i++){
        ctx.fillStyle="white";
        ctx.fillRect(Math.random()*800,Math.random()*500,2,2);
    }

    // player
    ctx.fillStyle = "cyan";
    ctx.fillRect(player.x,player.y,player.width,player.height);

    // bullets
    ctx.fillStyle = "yellow";

    bullets.forEach(b=>{
        ctx.fillRect(b.x,b.y,b.width,b.height);
    });

    // enemies
    ctx.fillStyle = "red";

    enemies.forEach(e=>{
        ctx.fillRect(e.x,e.y,e.width,e.height);
    });

    // score
    ctx.fillStyle="white";
    ctx.font="24px Arial";
    ctx.fillText("Score: " + score, 10, 30);

    if(gameOver){
        ctx.fillStyle="white";
        ctx.font="50px Arial";
        ctx.fillText("GAME OVER", 250, 250);

        ctx.font="30px Arial";
        ctx.fillText("Final Score: " + score, 300, 300);
    }
}

function gameLoop(){

    if(!gameOver){
        update();
    }

    draw();

    requestAnimationFrame(gameLoop);
}

gameLoop();

</script>

</body>
</html>

</body>
</html>
