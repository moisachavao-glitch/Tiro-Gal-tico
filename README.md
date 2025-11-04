<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<title>Galáxia dos Projetos 🌌</title>
<style>
  html, body {
    margin: 0;
    height: 100%;
    background: radial-gradient(circle at center, #0a0f2c, #000);
    overflow: hidden;
    cursor: crosshair;
  }

  canvas {
    display: block;
  }

  #msg {
    position: absolute;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%);
    font-family: 'Courier New', monospace;
    color: #7dd3fc;
    font-size: 1.1em;
    text-shadow: 0 0 10px #38bdf8;
    opacity: 0;
    transition: opacity 0.6s ease;
  }
</style>
</head>
<body>
<canvas id="galaxy"></canvas>
<div id="msg"></div>

<script>
  const canvas = document.getElementById('galaxy');
  const ctx = canvas.getContext('2d');
  const msg = document.getElementById('msg');

  let width, height;
  function resize() {
    width = canvas.width = window.innerWidth;
    height = canvas.height = window.innerHeight;
  }
  window.addEventListener('resize', resize);
  resize();

  // --- Seus projetos aqui! ---
  const projects = [
    { name: "🚀 Nave Exploradora", url: "https://seuusuario.github.io/nave-exploradora/" },
    { name: "🌌 Galáxia Inteligente", url: "https://seuusuario.github.io/galaxia-inteligente/" },
    { name: "🪐 Portfólio Principal", url: "https://seuusuario.github.io/portfolio/" },
    { name: "💡 Projeto Criativo", url: "https://seuusuario.github.io/projeto-criativo/" }
  ];

  class Star {
    constructor(project) {
      this.x = Math.random() * width;
      this.y = Math.random() * height;
      this.size = Math.random() * 3 + 1;
      this.twinkle = Math.random() * 0.05 + 0.01;
      this.brightness = Math.random();
      this.project = project;
    }
    draw() {
      this.brightness += this.twinkle;
      if (this.brightness > 1 || this.brightness < 0) this.twinkle *= -1;
      const glow = Math.floor(180 + 75 * this.brightness);
      ctx.fillStyle = `rgb(${glow}, ${glow}, 255)`;
      ctx.beginPath();
      ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
      ctx.fill();
    }
  }

  const stars = projects.map(p => new Star(p));

  function animate() {
    ctx.fillStyle = "rgba(0, 0, 20, 0.3)";
    ctx.fillRect(0, 0, width, height);
    stars.forEach(s => s.draw());
    requestAnimationFrame(animate);
  }

  // --- Interação ---
  canvas.addEventListener('click', e => {
    const mx = e.clientX, my = e.clientY;
    const star = stars.find(s => Math.hypot(s.x - mx, s.y - my) < 30);
    if (star) {
      showMessage(`Viajando para ${star.project.name}...`);
      setTimeout(() => window.open(star.project.url, "_blank"), 1500);
    }
  });

  function showMessage(text) {
    msg.textContent = text;
    msg.style.opacity = 1;
    setTimeout(() => msg.style.opacity = 0, 3000);
  }

  animate();
</script>
</body>
</html>
