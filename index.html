<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>❤️ Сердечко</title>
  <script src="https://telegram.org/js/telegram-web-app.js"></script>
  <style>
    :root {
      --bg: #0d0d0d;
      --c1: #ff2d55;
      --c2: #ff6b88;
      --c3: #ffb3c1;
      --text: #ffe0e6;
    }

    * { margin: 0; padding: 0; box-sizing: border-box; }

    html, body {
      width: 100%; height: 100%;
    }

    body {
      background: var(--bg);
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      font-family: 'Courier New', Courier, monospace;
      overflow: hidden;
    }

    .container {
      position: relative;
      z-index: 1;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 28px;
    }

    /* ── Canvas heart ── */
    #heartCanvas {
      display: block;
      /* pulse handled by JS transform */
    }

    .label {
      font-size: clamp(13px, 3.5vw, 19px);
      color: var(--text);
      letter-spacing: 0.35em;
      text-transform: uppercase;
      opacity: 0.7;
      animation: labelFade 3s ease-in-out infinite;
    }

    @keyframes labelFade {
      0%, 100% { opacity: 0.7; }
      50%       { opacity: 0.35; }
    }

    /* ── Floating particles ── */
    .particles {
      position: fixed;
      inset: 0;
      pointer-events: none;
      overflow: hidden;
      z-index: 0;
    }
    .particle {
      position: absolute;
      bottom: -20px;
      opacity: 0;
      color: var(--c1);
      animation: floatUp linear infinite;
    }
    @keyframes floatUp {
      0%   { opacity: 0; transform: translateY(0) scale(0.6) rotate(0deg); }
      10%  { opacity: 0.7; }
      90%  { opacity: 0.2; }
      100% { opacity: 0; transform: translateY(-100vh) scale(1.3) rotate(20deg); }
    }
  </style>
</head>
<body>

<div class="particles" id="particles"></div>

<div class="container">
  <canvas id="heartCanvas"></canvas>
  <div class="label">я очень дорожу тобою ❤</div>
</div>

<script>
  if (window.Telegram?.WebApp) {
    Telegram.WebApp.ready();
    Telegram.WebApp.expand();
    Telegram.WebApp.setHeaderColor('#0d0d0d');
    Telegram.WebApp.setBackgroundColor('#0d0d0d');
  }

  // ── ASCII heart rows ──
  const rows = [
    "  ████▄   ▄████  ",
    " ████████████████ ",
    "██████████████████",
    "██████████████████",
    "██████████████████",
    " ████████████████ ",
    "  ██████████████  ",
    "   ████████████   ",
    "    ██████████    ",
    "     ████████     ",
    "      ██████      ",
    "       ████       ",
    "        ██        ",
  ];

  const canvas = document.getElementById('heartCanvas');
  const ctx = canvas.getContext('2d');

  // Responsive font size
  const BASE = Math.min(window.innerWidth * 0.038, 18);
  const FONT = BASE + 'px "Courier New", monospace';
  ctx.font = FONT;
  const charW = ctx.measureText('█').width;
  const charH = BASE * 1.22;

  const cols = rows[0].length;
  const numRows = rows.length;

  canvas.width  = Math.ceil(cols * charW);
  canvas.height = Math.ceil(numRows * charH);

  // Build char grid with per-cell metadata
  const cells = [];
  for (let r = 0; r < numRows; r++) {
    for (let c = 0; c < cols; c++) {
      const ch = rows[r][c];
      if (ch === '█') {
        cells.push({
          x: c * charW,
          y: r * charH,
          // wave offsets for ripple
          phase: (r + c) * 0.28,
          // random for sparkle
          sparkPhase: Math.random() * Math.PI * 2,
          sparkSpeed: 0.7 + Math.random() * 1.5,
        });
      }
    }
  }

  // ── Colour helpers ──
  function lerpColor(a, b, t) {
    const ah = parseInt(a.slice(1), 16);
    const bh = parseInt(b.slice(1), 16);
    const ar = (ah >> 16) & 0xff, ag = (ah >> 8) & 0xff, ab = ah & 0xff;
    const br = (bh >> 16) & 0xff, bg = (bh >> 8) & 0xff, bb = bh & 0xff;
    const r = Math.round(ar + (br - ar) * t);
    const g = Math.round(ag + (bg - ag) * t);
    const b2 = Math.round(ab + (bb - ab) * t);
    return `rgb(${r},${g},${b2})`;
  }

  // ── Main draw loop ──
  let start = null;

  function draw(ts) {
    if (!start) start = ts;
    const t = (ts - start) / 1000; // seconds

    ctx.clearRect(0, 0, canvas.width, canvas.height);
    ctx.font = FONT;
    ctx.textBaseline = 'top';

    // Global pulse: scale the whole canvas element
    const pulse = 1 + 0.06 * Math.sin(t * 2.1 * Math.PI * 0.8);
    canvas.style.transform = `scale(${pulse})`;

    for (const cell of cells) {
      // 1. Ripple wave: brightness oscillates in a wave from top-left
      const wave = 0.5 + 0.5 * Math.sin(t * 3 - cell.phase);

      // 2. Sparkle: occasional bright flash per cell
      const sparkle = Math.max(0, Math.sin(t * cell.sparkSpeed * Math.PI + cell.sparkPhase));
      const sparkBoost = Math.pow(sparkle, 6); // sharp peaks

      // 3. Global color shift: cycle between rose → deep red → pink
      const cycle = 0.5 + 0.5 * Math.sin(t * 0.6);

      // Compose final brightness
      const brightness = wave * 0.6 + sparkBoost * 0.4;

      // Pick colour along gradient
      const baseColor = lerpColor('#ff2d55', '#ff9aaa', cycle);
      const finalColor = lerpColor('#330011', baseColor, brightness);

      // Glow: draw blurred shadow for lit cells
      if (brightness > 0.4) {
        ctx.shadowColor = '#ff2d55';
        ctx.shadowBlur  = 8 + brightness * 12;
      } else {
        ctx.shadowBlur = 0;
      }

      ctx.fillStyle = finalColor;
      ctx.fillText('█', cell.x, cell.y);
    }

    ctx.shadowBlur = 0;
    requestAnimationFrame(draw);
  }

  requestAnimationFrame(draw);

  // ── Floating particles ──
  const pContainer = document.getElementById('particles');
  const symbols = ['♥', '❤', '♡', '❥'];

  function spawnParticle() {
    const el = document.createElement('span');
    el.className = 'particle';
    el.textContent = symbols[Math.floor(Math.random() * symbols.length)];
    el.style.left = Math.random() * 100 + 'vw';
    el.style.fontSize = (9 + Math.random() * 13) + 'px';
    const dur = 5 + Math.random() * 7;
    el.style.animationDuration = dur + 's';
    el.style.animationDelay = (Math.random() * 2) + 's';
    pContainer.appendChild(el);
    setTimeout(() => el.remove(), (dur + 3) * 1000);
  }

  for (let i = 0; i < 10; i++) setTimeout(spawnParticle, i * 350);
  setInterval(spawnParticle, 650);
</script>
</body>
</html>
