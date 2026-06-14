<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<title>Love Site</title>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    width: 100vw;
    height: 100vh;
    overflow: hidden;
    background: black;
    font-family: Arial;
}

/* ===== LOGIN FULLSCREEN ===== */
#login {
    position: absolute;
    width: 100%;
    height: 100%;
    background: black;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    z-index: 100;
}

#login input {
    padding: 12px;
    font-size: 18px;
    border: none;
    outline: none;
    text-align: center;
}

#login button {
    width: 220px;
    height: 50px;
    margin-top: 15px;

    background-color: #ffffff !important;
    color: #000000 !important;

    border: 2px solid #ffffff;
    border-radius: 10px;

    opacity: 1 !important;
    box-shadow: 0 0 15px rgba(255,255,255,.4);

    font-size: 18px;
    font-weight: bold;
    cursor: pointer;
}


/* ===== CANVAS ===== */
canvas {
    position: absolute;
    top: 0;
    left: 0;
}

/* ===== LOVE TEXT ===== */
.love {
    position: absolute;
    writing-mode: vertical-rl;
    text-orientation: upright;
    font-size: 14px;
    color: white;
    animation: fall linear forwards;
}

@keyframes fall {
    from { transform: translateY(-100px); }
    to { transform: translateY(120vh); opacity: 0; }
}

/* ===== BOOK ===== */
#book {
    position: fixed;
    inset: 0;

    width: 100vw;
    height: 100vh;

    display: none;
    justify-content: center;
    align-items: center;

    background: #000;
}

/* ===== PAGE ===== */
.page {
    width: 80vw;
    height: 80vh;
    max-width: 1200px;
    max-height: 800px;
    perspective: 2000px;
}

.inner {
    width: 100%;
    height: 100%;
    position: relative;
    transform-style: preserve-3d;
    transition: 0.8s;
}

.page.flipped .inner {
    transform: rotateY(-180deg);
}

.face {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    background: #111;
    border: 1px solid #333;
    display: flex;
    justify-content: center;
    align-items: center;
}

.back {
    transform: rotateY(180deg);
}

img {
    width: 95%;
    height: 90%;
    object-fit: cover;
}
</style>
</head>

<body>

<!-- LOGIN -->
<div id="login">
    <input id="pass" type="password" placeholder="password">
    <button onclick="check()">Открыть</button>
</div>

<canvas id="fx"></canvas>

<!-- BOOK -->
<div id="book">
    <div class="page" id="page">
        <div class="inner">

            <div class="face front"></div>

            <div class="face back">
                <img src="photo1.jpg">
                <img src="photo.jpg">
            </div>

        </div>
    </div>
</div>

<script>
/* PASSWORD */
function check(){
    const p = document.getElementById("pass").value;

    if(p === "2020"){   // можешь поменять
        document.getElementById("login").style.display = "none";
        start();
    } else {
        alert("wrong password");
    }
}

/* CANVAS */
const canvas = document.getElementById("fx");
const ctx = canvas.getContext("2d");
canvas.width = innerWidth;
canvas.height = innerHeight;

let particles = [];

function firework(){
    for(let i=0;i<80;i++){
        particles.push({
            x: innerWidth/2,
            y: innerHeight/2,
            vx:(Math.random()-0.5)*6,
            vy:(Math.random()-0.5)*6,
            life:80
        });
    }
}

function loop(){
    ctx.fillStyle = "rgba(0,0,0,0.2)";
    ctx.fillRect(0,0,canvas.width,canvas.height);

    particles.forEach(p=>{
        p.x += p.vx;
        p.y += p.vy;
        p.life--;

        ctx.fillStyle = "white";
        ctx.fillRect(p.x,p.y,2,2);
    });

    particles = particles.filter(p=>p.life>0);
    requestAnimationFrame(loop);
}
loop();

/* LOVE TEXT */
function loveRain(){
    setInterval(()=>{
        const d = document.createElement("div");
        d.className = "love";
        d.innerText = "I LOVE YOU";

        d.style.left = Math.random()*window.innerWidth + "px";

        document.body.appendChild(d);
        setTimeout(()=>d.remove(),4000);
    },120);
}

/* START */
function start(){

    firework();

    setTimeout(()=>{
        loveRain();
    },1000);

    setTimeout(()=>{
        document.getElementById("book").style.display = "flex";
    },3500);
}

/* FLIP */
document.getElementById("page").addEventListener("click",()=>{
    document.getElementById("page").classList.toggle("flipped");
});
</script>

</body>
</html>
