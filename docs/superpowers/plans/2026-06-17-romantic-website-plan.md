# Romantic Website Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Single HTML file romantic website — dark/gold trading-terminal aesthetic with password gate, ATH graph, love messages, heart particles, and background music.

**Architecture:** Single `index.html` — vanilla HTML/CSS/JS. All sections in one scrollable page. No dependencies, no build step.

**Tech Stack:** HTML5, CSS3 (custom properties, animations, flexbox/grid), vanilla JS, SVG for chart, YouTube iframe API.

---

### Task 1: HTML Shell + CSS Variables + Base Styles

**Files:**
- Create: `index.html`

- [ ] **Step 1: Write the base HTML shell**

```html
<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>For You 💛</title>
<style>
:root {
  --bg: #0a0a0c;
  --surface: #111116;
  --gold: #d4a853;
  --gold-glow: #f0c060;
  --rose: #e8a0bf;
  --text: #c8c8cc;
  --text-dim: #666670;
  --font-mono: 'Courier New', monospace;
  --font-body: Georgia, 'Times New Roman', serif;
}

* { margin: 0; padding: 0; box-sizing: border-box; }

html { scroll-behavior: smooth; }

body {
  background: var(--bg);
  color: var(--text);
  font-family: var(--font-body);
  overflow-x: hidden;
  -webkit-font-smoothing: antialiased;
}
</style>
</head>
<body>
</body>
</html>
```

- [ ] **Step 2: Open in browser to verify blank dark page renders**

Run: Open `index.html` in browser. Expected: dark background (#0a0a0c), no errors.

---

### Task 2: Password Gate

**Files:**
- Modify: `index.html` (add inside `<body>` at top, before other content)

- [ ] **Step 1: Add password gate HTML, CSS, and JS**

Insert this inside `<body>` at the very top:

```html
<div id="password-gate">
  <div class="gate-inner">
    <p class="gate-label">ENTER CODE</p>
    <input type="password" id="pw-input" maxlength="6" autofocus placeholder="_ _ _ _ _ _">
    <p class="gate-hint" id="pw-hint"></p>
  </div>
</div>
```

Add these CSS rules inside the existing `<style>` block:

```css
#password-gate {
  position: fixed; inset: 0; z-index: 1000;
  background: var(--bg);
  display: flex; align-items: center; justify-content: center;
  transition: transform 0.8s cubic-bezier(0.4, 0, 0.2, 1);
}
#password-gate.unlocked { transform: translateY(-100%); }

.gate-inner { text-align: center; }
.gate-label {
  font-family: var(--font-mono); font-size: 0.75rem;
  letter-spacing: 0.3em; color: var(--text-dim); margin-bottom: 1.5rem;
}
#pw-input {
  background: transparent; border: none; border-bottom: 1px solid var(--text-dim);
  color: var(--gold); font-size: 2rem; text-align: center;
  letter-spacing: 0.5em; width: 12rem; outline: none; font-family: var(--font-mono);
  caret-color: var(--gold);
}
#pw-input:focus { border-color: var(--gold); }
#pw-input.shake { animation: shake 0.4s ease; }

.gate-hint {
  margin-top: 1rem; font-size: 0.75rem; color: var(--rose);
  font-family: var(--font-mono); min-height: 1.2em;
}

@keyframes shake {
  0%,100% { transform: translateX(0); }
  25% { transform: translateX(-8px); }
  75% { transform: translateX(8px); }
}
```

Add this JS before `</body>`:

```html
<script>
const CORRECT_PW = '070725';
const gate = document.getElementById('password-gate');
const pwInput = document.getElementById('pw-input');
const pwHint = document.getElementById('pw-hint');

pwInput.addEventListener('input', () => {
  if (pwInput.value.length === 6) {
    if (pwInput.value === CORRECT_PW) {
      gate.classList.add('unlocked');
    } else {
      pwInput.classList.add('shake');
      pwHint.textContent = 'ยังไม่ใช่รหัสที่ถูกต้องนะ... 💔';
      setTimeout(() => { pwInput.classList.remove('shake'); pwInput.value = ''; pwHint.textContent = ''; }, 500);
    }
  }
});
</script>
```

- [ ] **Step 2: Open browser, test wrong and correct passwords**

Test: Enter wrong 6-digit code → shakes + clears. Enter `070725` → gate slides up.

---

### Task 3: Hero Section

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add hero section HTML**

Insert after the password gate div (before `</body>`, inside a wrapper):

```html
<div id="main-content">
  <section id="hero">
    <h1 class="hero-title">ถึงเธอ</h1>
    <p class="hero-sub">ทุกวินาทีที่มีเธอคือ All-Time High ของฉัน</p>
    <p class="hero-date">07.07.25 — ∞</p>
  </section>
</div>
```

Add CSS:

```css
#main-content { opacity: 0; animation: fadeIn 1.2s 0.6s forwards; }
@keyframes fadeIn { to { opacity: 1; } }

#hero {
  min-height: 100vh; display: flex; flex-direction: column;
  align-items: center; justify-content: center; text-align: center;
  padding: 2rem;
}
.hero-title {
  font-size: clamp(2.5rem, 8vw, 5rem); color: var(--gold);
  font-family: var(--font-body); margin-bottom: 1rem;
  animation: slideUp 1s ease;
}
.hero-sub {
  font-size: clamp(1rem, 2.5vw, 1.3rem); color: var(--text);
  max-width: 20rem; line-height: 1.8; margin-bottom: 2rem;
  animation: slideUp 1s 0.2s ease both;
}
.hero-date {
  font-family: var(--font-mono); font-size: 0.8rem;
  color: var(--text-dim); letter-spacing: 0.2em;
  animation: slideUp 1s 0.4s ease both;
}
@keyframes slideUp {
  from { opacity: 0; transform: translateY(30px); }
  to { opacity: 1; transform: translateY(0); }
}
```

Add JS to trigger content fade-in after gate unlocks:

```js
// Add after existing gate JS:
gate.addEventListener('transitionend', () => {
  if (gate.classList.contains('unlocked')) {
    document.getElementById('main-content').style.animationPlayState = 'running';
  }
});
```

- [ ] **Step 2: Open browser, enter correct password, verify hero fades in smoothly**

---

### Task 4: All-Time High Graph (SVG Candlestick Chart)

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add graph section HTML**

Insert after hero section:

```html
<section id="chart-section">
  <div id="ticker">
    <div class="ticker-track">
      <span>LOVE</span><span class="ticker-up">▲</span>
      <span class="ticker-positive">+∞</span>
      <span>|</span>
      <span>ATH:</span><span class="ticker-positive">EVERY DAY</span>
      &nbsp;&nbsp;&nbsp;
      <span>LOVE</span><span class="ticker-up">▲</span>
      <span class="ticker-positive">+∞</span>
      <span>|</span>
      <span>ATH:</span><span class="ticker-positive">EVERY DAY</span>
    </div>
  </div>
  <h2 class="section-title">LOVE / THB</h2>
  <svg id="chart" viewBox="0 0 800 400" preserveAspectRatio="xMidYMid meet"></svg>
  <div id="milestone-tooltip"></div>
</section>
```

Add CSS:

```css
#chart-section { padding: 4rem 1rem; max-width: 900px; margin: 0 auto; }

#ticker {
  overflow: hidden; background: var(--surface); border: 1px solid #222;
  padding: 0.5rem 1rem; margin-bottom: 1rem; border-radius: 4px;
  font-family: var(--font-mono); font-size: 0.7rem; letter-spacing: 0.05em;
}
.ticker-track { display: inline-block; white-space: nowrap; animation: ticker 12s linear infinite; }
.ticker-up { color: #4caf50; }
.ticker-positive { color: #4caf50; }

@keyframes ticker { 0% { transform: translateX(0); } 100% { transform: translateX(-50%); } }

.section-title { font-family: var(--font-mono); font-size: 0.8rem; color: var(--text-dim); text-align: center; margin-bottom: 1rem; letter-spacing: 0.2em; }

#chart { width: 100%; height: auto; }

#milestone-tooltip {
  position: fixed; pointer-events: none; opacity: 0;
  background: var(--surface); border: 1px solid var(--gold);
  padding: 0.75rem 1rem; border-radius: 6px; font-size: 0.85rem;
  color: var(--gold); font-family: var(--font-body);
  transform: translate(-50%, -120%); transition: opacity 0.2s;
  z-index: 100; max-width: 14rem; text-align: center;
}
```

- [ ] **Step 2: Add JS to draw candlestick chart with milestone**

Add inside `<script>` tag:

```js
// --- Chart ---
const W = 800, H = 400, PAD_LEFT = 60, PAD_RIGHT = 40, PAD_TOP = 40, PAD_BOT = 50;
const CHART_W = W - PAD_LEFT - PAD_RIGHT;
const CHART_H = H - PAD_TOP - PAD_BOT;

const candles = [];
const N = 36; // 36 candles (~3 years of monthly-ish data)
// ponytail: generate upward-trending data with noise
for (let i = 0; i < N; i++) {
  const trend = 100 + i * 12 + (Math.random() - 0.3) * 15;
  const noise = Math.random() * 20;
  const open = trend + noise * 0.3;
  const close = open + 5 + Math.random() * 25;
  const high = close + Math.random() * 10;
  const low = open - Math.random() * 10;
  candles.push({ open, high, low, close, bullish: close > open });
}

const milestones = [
  { idx: 8, label: '07.07.25\nวันที่เราเริ่มคบกัน 💕', color: '#e8a0bf' },
];

function drawChart() {
  const svg = document.getElementById('chart');
  const maxVal = Math.max(...candles.map(c => c.high));
  const minVal = Math.min(...candles.map(c => c.low));
  const range = maxVal - minVal || 1;

  const toX = (i) => PAD_LEFT + (i / (N - 1)) * CHART_W;
  const toY = (v) => PAD_TOP + CHART_H - ((v - minVal) / range) * CHART_H;

  let html = '';

  // Grid lines
  for (let i = 0; i <= 5; i++) {
    const y = PAD_TOP + (CHART_H / 5) * i;
    const val = Math.round(maxVal - (range / 5) * i);
    html += `<line x1="${PAD_LEFT}" y1="${y}" x2="${PAD_LEFT + CHART_W}" y2="${y}" stroke="#1a1a22" stroke-width="1"/><text x="${PAD_LEFT - 10}" y="${y + 4}" text-anchor="end" fill="#444" font-size="10" font-family="monospace">${val}</text>`;
  }

  // Candles
  const candleW = Math.max(4, CHART_W / N * 0.6);
  candles.forEach((c, i) => {
    const x = toX(i);
    const openY = toY(c.open), closeY = toY(c.close);
    const highY = toY(c.high), lowY = toY(c.low);
    const color = c.bullish ? '#4caf50' : '#f44336';
    const bodyTop = Math.min(openY, closeY);
    const bodyH = Math.abs(closeY - openY) || 1;
    html += `<line x1="${x}" y1="${highY}" x2="${x}" y2="${lowY}" stroke="${color}" stroke-width="1.5"/>`;
    html += `<rect x="${x - candleW / 2}" y="${bodyTop}" width="${candleW}" height="${bodyH}" fill="${color}"/>`;
  });

  // Milestones
  milestones.forEach(m => {
    const c = candles[m.idx];
    const x = toX(m.idx);
    const y = toY(c.high) - 20;
    html += `<circle cx="${x}" cy="${y}" r="6" fill="${m.color}" stroke="var(--gold)" stroke-width="2" class="milestone-dot" data-idx="${m.idx}"/>`;
    html += `<text x="${x}" y="${y - 16}" text-anchor="middle" fill="${m.color}" font-size="12" font-family="monospace">▼</text>`;
  });

  // Labels
  candles.forEach((_, i) => {
    if (i % 6 === 0) {
      const x = toX(i);
      html += `<text x="${x}" y="${PAD_TOP + CHART_H + 20}" text-anchor="middle" fill="#444" font-size="10" font-family="monospace">M${i + 1}</text>`;
    }
  });

  svg.innerHTML = html;

  // Milestone hover
  const tooltip = document.getElementById('milestone-tooltip');
  svg.querySelectorAll('.milestone-dot').forEach(dot => {
    dot.addEventListener('mouseenter', (e) => {
      const m = milestones[parseInt(dot.dataset.idx)];
      tooltip.textContent = m.label;
      tooltip.style.opacity = '1';
    });
    dot.addEventListener('mousemove', (e) => {
      tooltip.style.left = e.clientX + 'px';
      tooltip.style.top = e.clientY + 'px';
    });
    dot.addEventListener('mouseleave', () => { tooltip.style.opacity = '0'; });
  });
}

drawChart();
```

- [ ] **Step 3: Open browser, verify chart draws, hover milestone dot shows "07.07.25 วันที่เราเริ่มคบกัน 💕"**

---

### Task 5: Message Cards (Randomized)

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add messages section HTML**

Insert after chart section:

```html
<section id="messages">
  <h2 class="section-title">💌 ข้อความจากใจ</h2>
  <div class="msg-card" id="msg-card">
    <p id="msg-text"></p>
  </div>
  <button id="msg-btn">กดเพื่ออ่านข้อความอื่น ✨</button>
</section>
```

Add CSS:

```css
#messages { padding: 4rem 1rem; text-align: center; }

.msg-card {
  max-width: 24rem; margin: 2rem auto; padding: 2.5rem 2rem;
  background: var(--surface); border: 1px solid #222; border-radius: 12px;
  min-height: 10rem; display: flex; align-items: center; justify-content: center;
  transition: opacity 0.3s, transform 0.3s;
}
.msg-card.changing { opacity: 0; transform: translateY(-10px); }
#msg-text { font-size: 1.1rem; line-height: 2; color: var(--text); }

#msg-btn {
  background: transparent; border: 1px solid var(--gold); color: var(--gold);
  padding: 0.75rem 2rem; border-radius: 2rem; font-family: var(--font-body);
  font-size: 0.9rem; cursor: pointer; transition: background 0.3s, color 0.3s;
}
#msg-btn:hover { background: var(--gold); color: var(--bg); }
```

Add JS:

```js
// --- Messages ---
const messages = [
  'ขอบคุณที่เข้ามาในชีวิตนะ 🤍',
  'รอยยิ้มของเธอคือสิ่งที่ทำให้วันของฉันสดใสเสมอ',
  'เธอคือ All-Time High ของหัวใจฉัน',
  'ทุกวันที่มีเธอคือวันที่ดีที่สุด',
  'ขอบคุณที่อยู่ข้างกันเสมอมา',
  'ฉันโชคดีที่สุดที่ได้เจอเธอ',
  'ต่อให้เวลาผ่านไปนานแค่ไหน ความรู้สึกนี้ก็ไม่มีวันเปลี่ยน',
  'เธอคือคนที่ทำให้ฉันยิ้มได้แม้ในวันที่แย่ที่สุด',
  'รักเธอมากกว่าที่คำพูดจะบรรยายได้',
  'ขอบคุณที่อดทนกับฉันนะ',
  'สิ่งที่ชอบที่สุดในตัวเธอคือ... ทุกอย่างเลย',
  'เธอทำให้ชีวิตฉันมีความหมายมากขึ้นทุกวัน',
  'กี่ครั้งที่ท้อ เธอก็อยู่ตรงนี้เสมอ ขอบคุณจริงๆ',
  'ไม่ว่าเส้นกราฟชีวิตจะขึ้นหรือลง แต่กราฟความรักฉันมีแต่ขึ้น 📈',
  'แค่อยากให้รู้ว่า... ฉันรักเธอนะ',
];
let msgIdx = -1;

const msgText = document.getElementById('msg-text');
const msgBtn = document.getElementById('msg-btn');
const msgCard = document.getElementById('msg-card');

function showRandomMsg() {
  let idx;
  do { idx = Math.floor(Math.random() * messages.length); } while (idx === msgIdx && messages.length > 1);
  msgIdx = idx;
  msgCard.classList.add('changing');
  setTimeout(() => {
    msgText.textContent = messages[idx];
    msgCard.classList.remove('changing');
  }, 250);
}

showRandomMsg();
msgBtn.addEventListener('click', showRandomMsg);
```

- [ ] **Step 2: Open browser, verify random message shows, click button changes message with fade transition**

---

### Task 6: Heart Particle Animation

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add heart particle system CSS and JS**

Append to `<body>` (before `</body>`):

```html
<div id="hearts-container"></div>
```

Add CSS:

```css
#hearts-container { position: fixed; inset: 0; pointer-events: none; z-index: 999; }

.heart-particle {
  position: absolute; pointer-events: none;
  animation: heartFloat 2s ease-out forwards;
  user-select: none; font-size: 1.5rem;
}
@keyframes heartFloat {
  0% { opacity: 1; transform: translateY(0) scale(1) rotate(0deg); }
  100% { opacity: 0; transform: translateY(-200px) scale(0.3) rotate(30deg); }
}
```

Add JS:

```js
// --- Hearts ---
const heartsContainer = document.getElementById('hearts-container');
const heartEmojis = ['💕', '💗', '💖', '💘', '💝', '❤️', '💛', '✨'];

document.addEventListener('click', (e) => {
  const heart = document.createElement('span');
  heart.className = 'heart-particle';
  heart.textContent = heartEmojis[Math.floor(Math.random() * heartEmojis.length)];
  heart.style.left = (e.clientX - 15) + 'px';
  heart.style.top = (e.clientY - 15) + 'px';
  heart.style.fontSize = (1 + Math.random() * 2) + 'rem';
  heart.style.animationDuration = (1.5 + Math.random() * 2) + 's';
  heartsContainer.appendChild(heart);
  setTimeout(() => heart.remove(), 3000);
});
```

- [ ] **Step 2: Open browser, click around — verify heart emojis float up and fade out**

---

### Task 7: Music Player

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add music player HTML and CSS**

Insert after hearts-container (before `</body>`):

```html
<div id="music-player">
  <button id="music-toggle" title="Play/Pause">🎵</button>
</div>
<div id="yt-player"></div>
```

Add CSS:

```css
#music-player { position: fixed; bottom: 1.5rem; right: 1.5rem; z-index: 998; }

#music-toggle {
  width: 3rem; height: 3rem; border-radius: 50%;
  background: var(--surface); border: 1px solid #333;
  color: var(--text); font-size: 1.2rem; cursor: pointer;
  display: flex; align-items: center; justify-content: center;
  transition: border-color 0.3s, box-shadow 0.3s;
}
#music-toggle:hover { border-color: var(--gold); box-shadow: 0 0 12px rgba(212, 168, 83, 0.3); }
#music-toggle.playing { border-color: var(--gold); }
#yt-player { display: none; }
```

Add JS (load YouTube IFrame API, auto-initialize player as hidden, toggle with button):

```js
// --- Music Player ---
let player;
let isPlaying = false;
const musicToggle = document.getElementById('music-toggle');

// ponytail: YouTube IFrame API — loads async, creates hidden player
window.onYouTubeIframeAPIReady = function() {
  player = new YT.Player('yt-player', {
    height: '0',
    width: '0',
    videoId: '9uZF7BlUQPE',
    playerVars: { autoplay: 0, controls: 0, loop: 1, playlist: '9uZF7BlUQPE' },
    events: {
      onReady: () => { /* ready */ },
    }
  });
};

// Load YT API
const tag = document.createElement('script');
tag.src = 'https://www.youtube.com/iframe_api';
const firstScriptTag = document.getElementsByTagName('script')[0];
firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);

musicToggle.addEventListener('click', () => {
  if (!player || !player.playVideo) return;
  if (isPlaying) {
    player.pauseVideo();
    musicToggle.classList.remove('playing');
    musicToggle.textContent = '🎵';
  } else {
    player.playVideo();
    musicToggle.classList.add('playing');
    musicToggle.textContent = '🎶';
  }
  isPlaying = !isPlaying;
});
```

- [ ] **Step 2: Open browser, click music button → verify YouTube audio plays/pauses, button toggles 🎵/🎶**

---

### Task 8: Responsive Polish + Final Assembly

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add responsive mobile tweaks**

Append inside `<style>`:

```css
@media (max-width: 600px) {
  #pw-input { font-size: 1.5rem; width: 10rem; }
  #chart-section { padding: 2rem 0.5rem; }
  .msg-card { padding: 1.5rem 1rem; margin: 1.5rem 1rem; }
  #msg-text { font-size: 1rem; }
  #music-toggle { width: 2.5rem; height: 2.5rem; font-size: 1rem; bottom: 1rem; right: 1rem; }
  .section-title { font-size: 0.7rem; }
}
```

- [ ] **Step 2: Full end-to-end test**

Open browser, mobile viewport (375px width):
1. Password gate → enter `070725` → slides up
2. Hero fades in
3. Scroll to chart → candles visible, ticker scrolling, hover milestone dot shows label
4. Scroll to messages → random message shows, click button → new message
5. Click anywhere → hearts float up
6. Click music button → music plays, button changes
7. Resize to desktop width → layout still correct

---

### Task 9: Deploy to Vercel

**Files:**
- Modify: (none — deploy step)

- [ ] **Step 1: Deploy single `index.html` to Vercel**

```bash
npx vercel --prod
```

Or use Vercel CLI / drag-and-drop via vercel.com. Single HTML file needs no special config.

- [ ] **Step 2: Open deployed URL, share link**

Verify all features work on the live URL.
