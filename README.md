<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <title>CERITA KITA – Lucu & Menggemaskan</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <style>
    body {
      margin: 0; padding: 0;
      background: #fff8fb;
      font-family: 'Comic Sans MS', cursive, sans-serif;
      display: flex; flex-direction: column;
      align-items: center; overflow-x: hidden;
      min-height: 100vh;
    }
    canvas {
      position: fixed; top:0; left:0; width:100%; height:100%;
      pointer-events:none; z-index:5;
    }
    h1 {
      margin: 20px; font-size: 2.5rem;
      background: linear-gradient(to right, #ff9ac1, #ffccde);
      -webkit-background-clip: text; -webkit-text-fill-color: transparent;
    }
    #loveBtn {
      font-size: 6rem; background:none; border:none;
      cursor:pointer; animation:pulse 2s infinite; z-index:10;
    }
    @keyframes pulse {
      0%,100%{transform:scale(1)}50%{transform:scale(1.2)}
    }
    #startText {
      display:none; font-size:1.2rem; margin-top:20px;
      color:#ff6699; cursor:pointer; z-index:10;
    }
    .photo-card, .closing-card {
      display:none; flex-direction:column; align-items:center;
      text-align:center; margin:20px auto;
      width:90%; max-width:400px; animation:fadeIn 1s ease forwards; z-index:10;
    }
    .photo-card img {
      width:100%; border-radius:15px;
      box-shadow:0 4px 10px rgba(0,0,0,0.15);
      animation:pulseImg 3s infinite; cursor:pointer;
    }
    @keyframes pulseImg {
      0%,100%{transform:scale(1)}50%{transform:scale(1.03)}
    }
    .caption {
      background:#ffeef5; padding:15px; border-radius:12px;
      margin-top:10px; font-size:1.1rem; line-height:1.5;
      color:#810044;
    }
    .click-hint {
      font-style:italic; font-size:0.9rem; color:#aa0066;
      margin-top:5px;
    }
    .closing-text {
      background:#ffdef0; padding:20px; border-radius:16px;
      box-shadow:0 4px 10px rgba(0,0,0,0.1);
      color:#80003f; font-size:1.2rem; line-height:1.8;
    }
    @keyframes fadeIn {from{opacity:0}to{opacity:1}}
    .heart {
      position: fixed; bottom: 0; color: #ff3366;
      animation:rise 4s linear forwards; pointer-events:none;
      opacity:0.8; z-index:8;
    }
    @keyframes rise {
      0%{transform:translateY(0) scale(1);opacity:1}
      100%{transform:translateY(-100vh) scale(1.5);opacity:0}
    }
    .firework {
      position: fixed; width: 4px; height:4px;
      border-radius:50%; opacity:0.8; z-index:7;
      animation:explode 1s ease-out forwards;
    }
    @keyframes explode {
      0%{transform:scale(1);opacity:1}
      100%{transform:scale(5);opacity:0}
    }
    .dim {
      position: fixed; top:0; left:0;
      width:100%; height:100%; background:rgba(0,0,0,0.4);
      z-index:6; opacity:0; transition:opacity 0.5s;
    }
    audio { display: none }
  </style>
</head>
<body>
  <canvas id="fireworks"></canvas>
  <div id="dim"></div>
  <audio autoplay loop>
    <source src="https://cdn.pixabay.com/download/audio/2022/11/30/audio_6b5cc66f41.mp3" type="audio/mpeg">
  </audio>

  <h1>CERITA KITA</h1>
  <button id="loveBtn">❤️</button>
  <div id="startText" onclick="showStep(1)">Klik di sini untuk mulai 💌</div>

  <div id="step1" class="photo-card">
    <img src="https://i.imgur.com/X9z2N3b.jpeg" onclick="nextStep(1)" />
    <div class="caption">Lucu ya cerita kita... 🥹💘<br>Saling mencintai, tapi cinta ternyata ngga selalu cukup</div>
    <div class="click-hint">Klik fotonya untuk lanjut 👉</div>
  </div>
  <div id="step2" class="photo-card">
    <img src="https://i.imgur.com/pRsT0wG.jpeg" onclick="nextStep(2)" />
    <div class="caption">Kita udah mimpiin masa depan, nyusun rencana kecil 💭💕<br>Ngetawain hal-hal remeh bersama</div>
    <div class="click-hint">Klik fotonya untuk lanjut 👉</div>
  </div>
  <div id="step3" class="photo-card">
    <img src="https://i.imgur.com/UJwNbX0.jpeg" onclick="nextStep(3)" />
    <div class="caption">Jauh dekat jarak udah pernah kita lewati 💌🚂<br>tapi hati tetap saling menunggu</div>
    <div class="click-hint">Klik fotonya untuk lanjut 👉</div>
  </div>
  <div id="step4" class="photo-card">
    <img src="https://i.imgur.com/e6HcdCU.jpeg" onclick="nextStep(4)" />
    <div class="caption">Aku nggak marah 😔<br>aku juga nggak nyalahin siapa‑siapa, hanya saja aku kosong.. 🕊️</div>
    <div class="click-hint">Klik fotonya untuk melihat penutup 🌸</div>
  </div>
  <div id="closing" class="closing-card">
    <div class="closing-text">
      🌙 Rindu yang nggak bisa disampaikan,<br>
      cinta yang harus disimpan…<br><br>
      💔 Terkadang yang paling menyakitkan bukan kehilangan,<br>
      melainkan harus melepas sesuatu<br>
      yang sebenarnya ingin aku genggam selamanya.
    </div>
  </div>

  <script>
    const canvas = document.getElementById('fireworks');
    const ctx = canvas.getContext('2d');
    const dim = document.getElementById('dim');
    canvas.width = innerWidth; canvas.height = innerHeight;

    function random(min,max){return Math.random()*(max-min)+min;}

    class Particle {
      constructor(x,y,color){
        this.x=x;this.y=y;
        this.vel={x:random(-2,2), y:random(-5,-8)};
        this.alpha=1;this.color=color;
      }
      update(){this.vel.y+=0.05; this.x+=this.vel.x; this.y+=this.vel.y; this.alpha-=0.02;}
      draw(){
        ctx.save(); ctx.globalAlpha=this.alpha;
        ctx.beginPath(); ctx.arc(this.x,this.y,2,0,2*Math.PI);
        ctx.fillStyle=this.color; ctx.fill(); ctx.restore();
      }
    }

    let particles = [];
    function createFirework(){
      const x = random(50,canvas.width-50);
      const y = random(canvas.height*0.3, canvas.height*0.6);
      const hue = random(0,360);
      for(let i=0;i<40;i++){
        particles.push(new Particle(x,y, `hsl(${hue},100%,75%)`));
      }
    }

    function animate(){
      ctx.fillStyle='rgba(255,255,255,0.3)'; ctx.fillRect(0,0,canvas.width,canvas.height);
      particles.forEach((p,i)=>{
        if(p.alpha<=0) particles.splice(i,1);
        else {p.update(); p.draw();}
      });
      requestAnimationFrame(animate);
    }
    animate();

    const loveBtn = document.getElementById("loveBtn");
    loveBtn.addEventListener("click",()=>{
      loveBtn.style.display='none';
      dim.style.opacity=1;
      let count=0;
      const inter = setInterval(()=>{
        const heart = document.createElement('div');
        heart.className='heart'; heart.innerText='❤️';
        heart.style.left=random(0,100)+'vw';
        heart.style.fontSize=`${random(15,30)}px`;
        document.body.appendChild(heart);
        setTimeout(()=>heart.remove(),4000);

        createFirework();
        count++;
        if(count>80){
          clearInterval(inter);
          setTimeout(()=>{dim.style.opacity=0; document.getElementById('startText').style.display='block';},1000);
        }
      },30);
    });

    function showStep(step){
      dim.style.opacity=0;
      document.getElementById('startText').style.display='none';
      document.getElementById('step'+step).style.display='flex';
    }

    function nextStep(current){
      document.getElementById('step'+current).style.display='none';
      const nxt = document.getElementById('step'+(current+1));
      if(nxt) nxt.style.display='flex'; else document.getElementById('closing').style.display='flex';
    }

    window.addEventListener('resize',()=>{canvas.width=innerWidth; canvas.height=innerHeight;});
  </script>
</body>
</html>
