# flappy-extreme
Flappy Bird style web game with score system and 2D/3D mode
<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Flappy Extreme</title>

<style>
body{
margin:0;
overflow:hidden;
font-family:Arial;
background:linear-gradient(to bottom,#70c5ce,#ffffff);
}

canvas{display:block;}

#ui{
position:absolute;
top:10px;
left:10px;
color:white;
z-index:10;
}

button{
padding:10px 15px;
border:none;
border-radius:10px;
cursor:pointer;
font-weight:bold;
}
</style>
</head>

<body>

<div id="ui">
<h2>🐦 Flappy Extreme</h2>
<button onclick="mode=!mode">2D / 3D</button>
<h3>Skor: <span id="score">0</span></h3>
</div>

<canvas id="game"></canvas>

<script>

const canvas=document.getElementById("game");
const ctx=canvas.getContext("2d");

canvas.width=innerWidth;
canvas.height=innerHeight;

let mode=false;
let score=0;

let bird={
x:120,
y:200,
r:20,
v:0
};

let pipes=[];

function spawnPipe(){
pipes.push({
x:canvas.width,
w:70,
t:Math.random()*200+80,
gap:160
});
}

setInterval(spawnPipe,1500);

addEventListener("click",()=>{
bird.v=-8;
});

function draw(){

ctx.fillStyle=mode?"#222":"#70c5ce";
ctx.fillRect(0,0,canvas.width,canvas.height);

bird.v+=0.5;
bird.y+=bird.v;

ctx.fillStyle="yellow";
ctx.beginPath();
ctx.arc(bird.x,bird.y,bird.r,0,Math.PI*2);
ctx.fill();

pipes.forEach((p,i)=>{

p.x-=4;

ctx.fillStyle="green";

ctx.fillRect(p.x,0,p.w,p.t);
ctx.fillRect(p.x,canvas.height-p.gap,p.w,p.gap);

if(p.x+p.w<0){
pipes.splice(i,1);
score++;
document.getElementById("score").innerText=score;
}

if(
bird.x>p.x &&
bird.x<p.x+p.w &&
(bird.y<p.t || bird.y>canvas.height-p.gap)
){
location.reload();
}

});

requestAnimationFrame(draw);
}

draw();

</script>

</body>
</html>
