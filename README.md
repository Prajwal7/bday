<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Happy Birthday!</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    :root {
      --primary: #8ecae6;
      --secondary: #ffb703;
      --accent: #fb8500;
      --bg: #f1faee;
      --text: #023047;
    }
    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background: linear-gradient(135deg, var(--primary) 0%, var(--bg) 100%);
      color: var(--text);
      min-height: 100vh;
      overflow-x: hidden;
    }
    header {
      text-align: center;
      padding: 2rem 1rem 1rem 1rem;
      position: relative;
      z-index: 2;
    }
    .balloons {
      position: absolute;
      top: 0;
      width: 100%;
      pointer-events: none;
      z-index: 1;
    }
    h1 {
      font-size: 2.5rem;
      color: var(--secondary);
      margin-bottom: 0;
      letter-spacing: 2px;
      text-shadow: 1px 2px 8px rgba(0,0,0,0.08);
    }
    h2 {
      font-size: 1.3rem;
      margin: 0.5rem 0 2rem 0;
      color: var(--accent);
      font-weight: 400;
    }
    main {
      max-width: 480px;
      margin: 0 auto;
      background: rgba(255,255,255,0.85);
      border-radius: 18px;
      box-shadow: 0 6px 36px #0001;
      padding: 2rem 1.5rem 3rem 1.5rem;
      position: relative;
      z-index: 2;
      margin-top: 1.5rem;
    }
    .story-section {
      margin-bottom: 2.5rem;
      opacity: 0;
      transform: translateY(60px);
      transition: all 0.7s cubic-bezier(.27,.8,.6,1.1);
    }
    .story-section.visible {
      opacity: 1;
      transform: translateY(0);
    }
    .confetti-btn {
      display: inline-block;
      background: var(--accent);
      color: #fff;
      font-size: 1.1rem;
      border: none;
      border-radius: 8px;
      padding: 0.7rem 1.5rem;
      cursor: pointer;
      margin-top: 1rem;
      box-shadow: 0 2px 8px #0002;
      transition: background 0.2s;
    }
    .confetti-btn:hover {
      background: var(--secondary);
      color: var(--text);
    }
    @media (max-width: 600px) {
      main { padding: 1rem 0.6rem 2rem 0.6rem; }
      h1 { font-size: 2rem; }
    }
  </style>
</head>
<body>
  <canvas class="balloons" id="balloonCanvas"></canvas>
  <header>
    <h1>Happy Birthday, [Sims]!</h1>
    <h2>Wishing you a year full of new adventures and bright beginnings.</h2>
  </header>
  <main>
    <div class="story-section" id="story1">
      <p>
        Once upon a time, the world paused to celebrate a very special soul. Each new year, the universe seemed to whisper gentle reminders: every sunrise is a blank page, every breath a fresh chance.
      </p>
    </div>
    <div class="story-section" id="story2">
      <p>
        On this birthday, the journey ahead sparkles with possibility. Like confetti in the wind, your dreams are ready to soar — vibrant, bold, and joyfully untamed.
      </p>
    </div>
    <div class="story-section" id="story3">
      <p>
        Here’s to you, and to the beautiful adventures waiting just around the corner. May laughter fill your days, love light your path, and may you always find wonder in new beginnings.
      </p>
    </div>
    <button class="confetti-btn" onclick="throwConfetti()">Celebrate with Confetti!</button>
  </main>
  <canvas id="confettiCanvas" style="position:fixed;top:0;left:0;width:100vw;height:100vh;pointer-events:none;z-index:999"></canvas>
  <script>
    // Reveal story sections on scroll
    const storySections = document.querySelectorAll('.story-section');
    function revealSections() {
      const trigger = window.innerHeight * 0.85;
      storySections.forEach(sec => {
        const rect = sec.getBoundingClientRect();
        if (rect.top < trigger) {
          sec.classList.add('visible');
        }
      });
    }
    window.addEventListener('scroll', revealSections);
    window.addEventListener('load', revealSections);

    // Balloons animation
    const balloonCanvas = document.getElementById('balloonCanvas');
    const ctxB = balloonCanvas.getContext('2d');
    function resizeBalloons() {
      balloonCanvas.width = window.innerWidth;
      balloonCanvas.height = 220;
    }
    resizeBalloons();
    window.addEventListener('resize', resizeBalloons);

    const balloonColors = ['#ffb703', '#fb8500', '#8ecae6', '#219ebc', '#ffddd2', '#b5179e'];
    let balloons = [];
    function createBalloons() {
      balloons = [];
      for(let i=0; i<10; i++) {
        balloons.push({
          x: Math.random()*balloonCanvas.width,
          y: 40 + Math.random()*120,
          r: 28+Math.random()*15,
          color: balloonColors[i%balloonColors.length],
          swing: 0.4+Math.random()*1.5
        });
      }
    }
    createBalloons();

    function drawBalloons() {
      ctxB.clearRect(0,0,balloonCanvas.width,balloonCanvas.height);
      balloons.forEach(b=>{
        // String
        ctxB.save();
        ctxB.beginPath();
        ctxB.strokeStyle = '#9994';
        ctxB.moveTo(b.x, b.y+b.r);
        ctxB.lineTo(b.x+10*Math.sin(b.y/18), b.y+b.r+40);
        ctxB.stroke();
        // Balloon
        ctxB.beginPath();
        ctxB.arc(b.x, b.y, b.r, 0, Math.PI*2);
        ctxB.fillStyle = b.color;
        ctxB.shadowColor = "#000";
        ctxB.shadowBlur = 4;
        ctxB.fill();
        ctxB.shadowBlur = 0;
        ctxB.restore();
        // Shine
        ctxB.beginPath();
        ctxB.arc(b.x-b.r/4, b.y-b.r/4, b.r/7, 0, Math.PI*2);
        ctxB.fillStyle = "#fff8";
        ctxB.fill();
      });
    }
    function animateBalloons() {
      balloons.forEach(b=>{
        b.y -= 0.08 + b.r/250;
        b.x += Math.sin(Date.now()/900 + b.y/20)*b.swing;
        if(b.y < -b.r) {
          b.y = balloonCanvas.height+70;
          b.x = Math.random()*balloonCanvas.width;
        }
      });
      drawBalloons();
      requestAnimationFrame(animateBalloons);
    }
    animateBalloons();

    // Confetti animation
    const confettiCanvas = document.getElementById('confettiCanvas');
    const ctxC = confettiCanvas.getContext('2d');
    function resizeConfetti() {
      confettiCanvas.width = window.innerWidth;
      confettiCanvas.height = window.innerHeight;
    }
    resizeConfetti();
    window.addEventListener('resize', resizeConfetti);

    // Simple confetti particles
    let confettiParticles = [];
    function throwConfetti() {
      confettiParticles = [];
      for(let i=0;i<190;i++) {
        confettiParticles.push({
          x: Math.random()*confettiCanvas.width,
          y: -20 - Math.random()*30,
          r: 4+Math.random()*5,
          color: balloonColors[Math.floor(Math.random()*balloonColors.length)],
          vx: (Math.random()-0.5)*4,
          vy: 2+Math.random()*2,
          rot: Math.random()*6.28
        });
      }
      animateConfetti();
    }
    function animateConfetti() {
      ctxC.clearRect(0,0,confettiCanvas.width,confettiCanvas.height);
      confettiParticles.forEach(p=>{
        ctxC.save();
        ctxC.translate(p.x, p.y);
        ctxC.rotate(p.rot);
        ctxC.fillStyle = p.color;
        ctxC.fillRect(-p.r/2, -p.r/2, p.r, p.r*2);
        ctxC.restore();
        p.x += p.vx;
        p.y += p.vy;
        p.rot += (Math.random()-0.5)*0.1;
        p.vy += 0.07;
      });
      confettiParticles = confettiParticles.filter(p=>p.y < confettiCanvas.height+30);
      if(confettiParticles.length > 0) requestAnimationFrame(animateConfetti);
    }
  </script>
</body>
</html>
