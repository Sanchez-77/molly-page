# 狗蛋 Max 发布页 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 构建狗蛋 Max 产品发布页，以视频首屏 + 动画状态展台为核心，温馨极简风格。

**Architecture:** 单文件 `index.html`，所有 CSS/JS 内联。动画引用上层目录 webp 帧序列，通过 Canvas + requestAnimationFrame 驱动，鼠标悬停播放。视频作为 Hero 全屏背景。

**Tech Stack:** 纯 HTML/CSS/JS，Noto Serif SC / Noto Sans SC（Google Fonts），无框架依赖

---

## 文件结构

```
毛毛/Max/
  发布页/
    index.html                    ← 创建，单文件全部内联
    docs/
      2026-04-20-max-landing-design.md   ← 已存在
      2026-04-20-max-landing-plan.md     ← 本文件
  （以下均为现有素材，相对路径引用）
  logo 2.webp                     ← ../logo 2.webp
  素材/jimeng-2026-04-15-1285-...mp4    ← ../素材/[视频名].mp4
  待机/待机_0.webp ... 待机_128.webp    ← ../待机/待机_N.webp
  完成/完成_0.webp ... 完成_80.webp
  搜索/搜索_0.webp ... 搜索_80.webp
  办公/办公_0.webp ... 办公_80.webp
  打字/打字_0.webp ... 打字_112.webp
  喝水/喝水_0.webp ... 喝水_80.webp
  刨地/刨地_0.webp ... 刨地_80.webp
  伸懒腰/伸懒腰_0.webp ... 伸懒腰_80.webp
  提起/提起_0.webp ... 提起_129.webp
  提醒/提醒_0.webp ... 提醒_48.webp
  闻闻/闻闻_0.webp ... 闻闻_112.webp
```

---

## Task 1: 基础 HTML 骨架 + CSS 变量 + 字体

**Files:**
- Create: `发布页/index.html`

- [ ] **Step 1: 创建文件，写入 HTML 骨架和 CSS 变量**

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>狗蛋 Max — Claude Code 桌面伴侣</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Noto+Serif+SC:wght@400;700;900&family=Noto+Sans+SC:wght@300;400;500&display=swap" rel="stylesheet">
<style>
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

:root {
  --bg:       #FDFAF5;
  --bg2:      #F5EDD8;
  --bg3:      #EDE0C8;
  --text:     #2C1A0E;
  --muted:    #8A7260;
  --accent:   #D4874E;
  --accent2:  #B8692E;
  --border:   rgba(180,140,100,0.15);
  --radius:   16px;
  --serif:    'Noto Serif SC', serif;
  --sans:     'Noto Sans SC', sans-serif;
}

html { scroll-behavior: smooth; }
body {
  background: var(--bg);
  color: var(--text);
  font-family: var(--sans);
  font-weight: 300;
  overflow-x: hidden;
}
</style>
</head>
<body>
<!-- 内容将在后续 Task 中填入 -->
<script>
// JS 将在后续 Task 中填入
</script>
</body>
</html>
```

- [ ] **Step 2: 在浏览器中打开，确认空白奶油色背景正常渲染**

用 `open 发布页/index.html` 打开，应看到暖白背景页面，无报错。

---

## Task 2: Nav + Hero 视频首屏

**Files:**
- Modify: `发布页/index.html`

- [ ] **Step 1: 在 `<style>` 中追加 Nav 和 Hero CSS**

```css
/* NAV */
nav {
  position: fixed; top: 0; left: 0; right: 0; z-index: 100;
  display: flex; align-items: center; justify-content: space-between;
  padding: 20px 48px;
  background: linear-gradient(to bottom, rgba(253,250,245,0.85), transparent);
  backdrop-filter: blur(8px);
}
.nav-logo {
  display: flex; align-items: center; gap: 10px;
}
.nav-logo img {
  width: 36px; height: 36px; border-radius: 8px; object-fit: cover;
}
.nav-logo span {
  font-family: var(--serif); font-weight: 900; font-size: 18px; color: var(--text);
}
.nav-links { display: flex; gap: 32px; list-style: none; }
.nav-links a {
  color: var(--muted); text-decoration: none; font-size: 14px;
  transition: color .2s;
}
.nav-links a:hover { color: var(--text); }
.nav-cta {
  background: var(--text); color: var(--bg);
  padding: 9px 22px; border-radius: 100px;
  font-size: 13px; font-weight: 500; text-decoration: none;
  font-family: var(--sans); transition: background .2s, transform .15s;
}
.nav-cta:hover { background: var(--accent2); transform: translateY(-1px); }

/* HERO */
.hero {
  position: relative; height: 100vh;
  display: flex; flex-direction: column;
  align-items: center; justify-content: center;
  text-align: center; overflow: hidden;
}
.hero-video {
  position: absolute; inset: 0; z-index: 0;
  width: 100%; height: 100%; object-fit: cover;
}
.hero-overlay {
  position: absolute; inset: 0; z-index: 1;
  background: linear-gradient(
    to bottom,
    rgba(253,250,245,0.15) 0%,
    rgba(253,250,245,0.05) 40%,
    rgba(253,250,245,0.6) 75%,
    rgba(253,250,245,1) 100%
  );
}
.hero-content {
  position: relative; z-index: 2;
  display: flex; flex-direction: column; align-items: center; gap: 20px;
  padding: 0 24px;
}
.hero-logo {
  width: 88px; height: 88px; border-radius: 20px;
  box-shadow: 0 8px 32px rgba(44,26,14,0.12);
  animation: hero-drop .8s cubic-bezier(.34,1.56,.64,1) both;
}
.hero-title {
  font-family: var(--serif); font-weight: 900;
  font-size: clamp(52px, 10vw, 96px);
  line-height: 1; letter-spacing: -.02em; color: var(--text);
  animation: fade-up .9s ease .1s both;
}
.hero-sub {
  font-size: clamp(15px, 2vw, 18px); color: var(--muted);
  max-width: 440px; line-height: 1.8;
  animation: fade-up 1s ease .2s both;
}
.hero-cta {
  display: inline-block;
  background: var(--text); color: var(--bg);
  padding: 14px 36px; border-radius: 100px;
  font-size: 16px; font-weight: 500; text-decoration: none;
  font-family: var(--sans);
  box-shadow: 0 4px 20px rgba(44,26,14,0.18);
  transition: background .2s, transform .15s, box-shadow .2s;
  animation: fade-up 1s ease .3s both;
}
.hero-cta:hover {
  background: var(--accent2);
  transform: translateY(-2px);
  box-shadow: 0 8px 32px rgba(44,26,14,0.22);
}
.hero-scroll {
  position: absolute; bottom: 32px; left: 50%; transform: translateX(-50%);
  z-index: 2; display: flex; flex-direction: column; align-items: center; gap: 6px;
  color: var(--muted); font-size: 11px; letter-spacing: .1em;
  animation: fade-up 1s ease .5s both;
}
.hero-scroll-arrow {
  width: 20px; height: 20px; border-right: 1.5px solid var(--muted);
  border-bottom: 1.5px solid var(--muted);
  transform: rotate(45deg);
  animation: scroll-bounce 1.5s ease-in-out infinite;
}
@keyframes scroll-bounce {
  0%,100% { transform: rotate(45deg) translateY(0); opacity: 1; }
  50% { transform: rotate(45deg) translateY(5px); opacity: 0.4; }
}
@keyframes hero-drop {
  from { opacity: 0; transform: translateY(-20px) scale(0.9); }
  to   { opacity: 1; transform: none; }
}
@keyframes fade-up {
  from { opacity: 0; transform: translateY(20px); }
  to   { opacity: 1; transform: none; }
}
```

- [ ] **Step 2: 在 `<body>` 中写入 Nav 和 Hero HTML**

视频文件名（需完整）：
`jimeng-2026-04-15-1285-固定镜头，微风让男人的头发、小狗的毛发以及周围的小草都柔和地飘动。男人右手轻摸小....mp4`

```html
<!-- NAV -->
<nav>
  <div class="nav-logo">
    <img src="../logo 2.webp" alt="狗蛋">
    <span>狗蛋 Max</span>
  </div>
  <ul class="nav-links">
    <li><a href="#states">动画状态</a></li>
    <li><a href="#how">工作原理</a></li>
    <li><a href="#download">下载</a></li>
  </ul>
  <a href="#download" class="nav-cta">免费下载</a>
</nav>

<!-- HERO -->
<section class="hero">
  <video class="hero-video" autoplay muted loop playsinline
    src="../素材/jimeng-2026-04-15-1285-固定镜头，微风让男人的头发、小狗的毛发以及周围的小草都柔和地飘动。男人右手轻摸小....mp4">
  </video>
  <div class="hero-overlay"></div>
  <div class="hero-content">
    <img class="hero-logo" src="../logo 2.webp" alt="狗蛋 Max">
    <h1 class="hero-title">狗蛋 Max</h1>
    <p class="hero-sub">住在屏幕角落的小伙伴，实时感知 Claude Code 的每一个状态</p>
    <a href="#download" class="hero-cta">免费下载 →</a>
  </div>
  <div class="hero-scroll">
    <span>向下滚动</span>
    <div class="hero-scroll-arrow"></div>
  </div>
</section>
```

- [ ] **Step 3: 浏览器中验证**

刷新页面，应看到：视频全屏播放（带柔和底部渐变）、logo + 标题 + 按钮居中显示、Nav 固定顶部、滚动箭头动画。

---

## Task 3: 动画状态区 HTML 结构 + CSS

**Files:**
- Modify: `发布页/index.html`

- [ ] **Step 1: 追加 states section CSS**

```css
/* STATES */
.states-section {
  padding: 120px 48px;
  background: var(--bg);
}
.states-inner { max-width: 1100px; margin: 0 auto; }

.section-eyebrow {
  font-size: 12px; letter-spacing: .2em; text-transform: uppercase;
  color: var(--accent); margin-bottom: 16px;
  display: flex; align-items: center; gap: 12px;
}
.section-eyebrow::before {
  content: ''; display: block; width: 24px; height: 1.5px;
  background: var(--accent);
}

.states-title {
  font-family: var(--serif); font-weight: 700;
  font-size: clamp(32px, 5vw, 52px); line-height: 1.2;
  margin-bottom: 12px;
}
.states-subtitle {
  color: var(--muted); font-size: 16px; line-height: 1.8;
  max-width: 480px; margin-bottom: 64px;
}

.states-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 20px;
}

.state-card {
  background: var(--bg);
  border: 1.5px solid var(--border);
  border-radius: var(--radius);
  padding: 24px;
  cursor: default;
  transition: border-color .25s, box-shadow .25s, transform .25s;
  position: relative; overflow: hidden;
}
.state-card:hover {
  border-color: rgba(212,135,78,0.4);
  box-shadow: 0 8px 32px rgba(44,26,14,0.08);
  transform: translateY(-3px);
}

.state-canvas-wrap {
  width: 100%; aspect-ratio: 1;
  background: var(--bg2); border-radius: 10px;
  overflow: hidden; margin-bottom: 16px;
  display: flex; align-items: center; justify-content: center;
}
.state-canvas {
  width: 100%; height: 100%;
}

.state-name {
  font-family: var(--serif); font-weight: 700;
  font-size: 16px; margin-bottom: 6px; color: var(--text);
}
.state-trigger {
  display: inline-block;
  font-size: 11px; color: var(--accent);
  background: rgba(212,135,78,0.1);
  padding: 2px 8px; border-radius: 4px;
  margin-bottom: 10px;
  letter-spacing: .04em;
}
.state-desc {
  font-size: 12px; color: var(--muted); line-height: 1.7;
}
```

- [ ] **Step 2: 在 Hero 之后追加 states section HTML**

```html
<!-- STATES -->
<section id="states" class="states-section">
  <div class="states-inner">
    <div class="section-eyebrow">动画状态</div>
    <h2 class="states-title reveal">它懂你在做什么</h2>
    <p class="states-subtitle reveal">11 种真实帧动画，每一个状态都是它在陪你</p>
    <div class="states-grid" id="states-grid">
      <!-- JS 动态生成卡片 -->
    </div>
  </div>
</section>
```

- [ ] **Step 3: 浏览器验证区域布局**

滚动到 states 区域，应看到标题和空 grid 区域，样式正常。

---

## Task 4: Canvas 帧动画引擎 + 状态卡片生成

**Files:**
- Modify: `发布页/index.html`（`<script>` 部分）

- [ ] **Step 1: 在 `<script>` 中写入动画数据和引擎**

```javascript
// ── 动画数据 ──────────────────────────────────────────
const STATES = [
  { id: '待机',   name: '待机',   frames: 129, trigger: '空闲时',     desc: '无事可做时发发呆，随机伸个懒腰' },
  { id: '办公',   name: '努力工作', frames: 81,  trigger: 'working',   desc: '工具调用前后，持续高频触发' },
  { id: '搜索',   name: '搜索文件', frames: 81,  trigger: 'searching', desc: 'Glob / Grep 工具调用时专属动画' },
  { id: '打字',   name: '敲键盘',  frames: 113, trigger: '键盘输入',   desc: '检测到持续输入超过 0.5s 触发' },
  { id: '完成',   name: '任务完成', frames: 81,  trigger: 'attention', desc: 'Claude 停止响应，任务完成时庆祝' },
  { id: '提醒',   name: '消息提醒', frames: 49,  trigger: 'notification', desc: '新通知或飞书角标变化时提醒' },
  { id: '喝水',   name: '喝水',   frames: 81,  trigger: '空闲随机',   desc: '空闲时随机出现的可爱小动作' },
  { id: '伸懒腰', name: '伸懒腰', frames: 81,  trigger: '空闲随机',   desc: '无聊时候偶尔伸个大懒腰' },
  { id: '刨地',   name: '刨地',   frames: 81,  trigger: '空闲随机',   desc: '无聊刨刨地，能量满满' },
  { id: '闻闻',   name: '闻闻',   frames: 113, trigger: '空闲随机',   desc: '好奇心爆棚，到处嗅一嗅' },
  { id: '提起',   name: '被提起', frames: 130, trigger: '拖拽',       desc: '拖动窗口时的挣扎与无奈' },
];

// ── 帧动画引擎 ──────────────────────────────────────────
class SpritePlayer {
  constructor(canvas, state) {
    this.canvas = canvas;
    this.ctx = canvas.getContext('2d');
    this.state = state;
    this.images = [];
    this.currentFrame = 0;
    this.playing = false;
    this.rafId = null;
    this.lastTime = 0;
    this.fps = 24;
    this.loaded = false;
    this._loadFirstFrame();
  }

  _imgPath(n) {
    return `../${this.state.id}/${this.state.id}_${n}.webp`;
  }

  _loadFirstFrame() {
    const img = new Image();
    img.onload = () => {
      this.images[0] = img;
      this._draw(img);
      this.loaded = true;
    };
    img.src = this._imgPath(0);
  }

  preload() {
    if (this.images.length >= this.state.frames) return;
    for (let i = 0; i < this.state.frames; i++) {
      if (this.images[i]) continue;
      const img = new Image();
      img.src = this._imgPath(i);
      this.images[i] = img;
    }
  }

  _draw(img) {
    const c = this.canvas;
    this.ctx.clearRect(0, 0, c.width, c.height);
    this.ctx.drawImage(img, 0, 0, c.width, c.height);
  }

  play() {
    if (this.playing) return;
    this.preload();
    this.playing = true;
    this._tick(0);
  }

  stop() {
    this.playing = false;
    if (this.rafId) cancelAnimationFrame(this.rafId);
    this.rafId = null;
    this.currentFrame = 0;
    if (this.images[0]) this._draw(this.images[0]);
  }

  _tick(ts) {
    if (!this.playing) return;
    const interval = 1000 / this.fps;
    if (ts - this.lastTime >= interval) {
      const img = this.images[this.currentFrame];
      if (img && img.complete) this._draw(img);
      this.currentFrame = (this.currentFrame + 1) % this.state.frames;
      this.lastTime = ts;
    }
    this.rafId = requestAnimationFrame(t => this._tick(t));
  }
}

// ── 卡片生成 ──────────────────────────────────────────
function buildStateCards() {
  const grid = document.getElementById('states-grid');
  if (!grid) return;

  STATES.forEach(state => {
    const card = document.createElement('div');
    card.className = 'state-card reveal';

    const dpr = window.devicePixelRatio || 1;
    const size = 240;

    card.innerHTML = `
      <div class="state-canvas-wrap">
        <canvas class="state-canvas"
          width="${size * dpr}" height="${size * dpr}"
          style="width:${size}px;height:${size}px">
        </canvas>
      </div>
      <div class="state-name">${state.name}</div>
      <span class="state-trigger">${state.trigger}</span>
      <p class="state-desc">${state.desc}</p>
    `;
    grid.appendChild(card);

    const canvas = card.querySelector('canvas');
    const player = new SpritePlayer(canvas, state);

    card.addEventListener('mouseenter', () => player.play());
    card.addEventListener('mouseleave', () => player.stop());
  });
}

document.addEventListener('DOMContentLoaded', buildStateCards);
```

- [ ] **Step 2: 浏览器验证**

刷新，滚动到 states 区域：应看到 11 张卡片，每张显示第 0 帧静止画面，悬停时播放动画，移开停止回到第 0 帧。

---

## Task 5: 工作原理区

**Files:**
- Modify: `发布页/index.html`

- [ ] **Step 1: 追加 how it works CSS**

```css
/* HOW IT WORKS */
.how-section {
  padding: 120px 48px;
  background: var(--bg2);
}
.how-inner { max-width: 900px; margin: 0 auto; text-align: center; }
.how-title {
  font-family: var(--serif); font-weight: 700;
  font-size: clamp(28px, 4vw, 44px); line-height: 1.2;
  margin-bottom: 12px;
}
.how-subtitle {
  color: var(--muted); font-size: 15px; line-height: 1.8;
  margin-bottom: 64px;
}
.how-steps {
  display: grid; grid-template-columns: 1fr auto 1fr auto 1fr;
  align-items: start; gap: 0;
}
.how-arrow {
  color: var(--border); font-size: 24px; padding-top: 20px;
  color: rgba(180,140,100,0.4);
}
.how-step { display: flex; flex-direction: column; align-items: center; gap: 16px; }
.how-step-num {
  width: 48px; height: 48px;
  background: var(--bg); border: 1.5px solid var(--border);
  border-radius: 50%;
  display: flex; align-items: center; justify-content: center;
  font-family: var(--serif); font-size: 18px; font-weight: 900;
  color: var(--accent);
}
.how-step-label {
  font-weight: 500; font-size: 15px; color: var(--text);
}
.how-step-desc { font-size: 13px; color: var(--muted); line-height: 1.7; }
.how-code {
  margin-top: 56px; text-align: left;
  background: var(--bg3);
  border: 1.5px solid var(--border);
  border-radius: var(--radius);
  overflow: hidden;
  font-family: 'SF Mono', 'Fira Code', monospace;
}
.how-code-header {
  display: flex; align-items: center; gap: 6px;
  padding: 10px 16px; border-bottom: 1px solid var(--border);
  background: rgba(253,250,245,0.5);
}
.how-code-dot { width: 10px; height: 10px; border-radius: 50%; }
.how-code-dot:nth-child(1) { background: #ff5f57; }
.how-code-dot:nth-child(2) { background: #febc2e; }
.how-code-dot:nth-child(3) { background: #28c840; }
.how-code-filename { font-size: 11px; color: var(--muted); margin-left: 6px; }
.how-code-body {
  padding: 20px 24px; font-size: 12.5px; line-height: 2; overflow-x: auto;
}
.c-comment { color: #b0a090; }
.c-key { color: var(--accent); }
.c-str { color: #7ba87a; }
```

- [ ] **Step 2: 在 states section 后追加 HTML**

```html
<!-- HOW IT WORKS -->
<section id="how" class="how-section">
  <div class="how-inner">
    <div class="section-eyebrow" style="justify-content:center">工作原理</div>
    <h2 class="how-title reveal">接入只需 30 秒</h2>
    <p class="how-subtitle reveal">通过 Claude Code 的 Hooks 系统，每个事件实时传递给狗蛋</p>
    <div class="how-steps reveal">
      <div class="how-step">
        <div class="how-step-num">1</div>
        <div class="how-step-label">Claude Code 触发事件</div>
        <div class="how-step-desc">键盘输入、工具调用、任务完成、权限申请…</div>
      </div>
      <div class="how-arrow">→</div>
      <div class="how-step">
        <div class="how-step-num">2</div>
        <div class="how-step-label">Hook 脚本转发</div>
        <div class="how-step-desc">hook.sh 将状态 POST 到本地 23333 端口</div>
      </div>
      <div class="how-arrow">→</div>
      <div class="how-step">
        <div class="how-step-num">3</div>
        <div class="how-step-label">狗蛋收到，即刻表演</div>
        <div class="how-step-desc">SpriteKit 驱动帧动画，实时响应，零延迟</div>
      </div>
    </div>
    <div class="how-code reveal">
      <div class="how-code-header">
        <div class="how-code-dot"></div>
        <div class="how-code-dot"></div>
        <div class="how-code-dot"></div>
        <span class="how-code-filename">~/.claude/settings.json（自动写入）</span>
      </div>
      <div class="how-code-body">
        <span class="c-comment">// 安装脚本自动配置，无需手动编辑</span><br>
        {<br>
        &nbsp;&nbsp;<span class="c-key">"hooks"</span>: {<br>
        &nbsp;&nbsp;&nbsp;&nbsp;<span class="c-key">"UserPromptSubmit"</span>: [{ <span class="c-key">"command"</span>: <span class="c-str">"bash hook.sh thinking"</span> }],<br>
        &nbsp;&nbsp;&nbsp;&nbsp;<span class="c-key">"Stop"</span>: [{ <span class="c-key">"command"</span>: <span class="c-str">"bash hook.sh attention"</span> }],<br>
        &nbsp;&nbsp;&nbsp;&nbsp;<span class="c-key">"PermissionRequest"</span>: [{ <span class="c-key">"command"</span>: <span class="c-str">"curl localhost:23333/permission"</span> }]<br>
        &nbsp;&nbsp;}<br>
        }
      </div>
    </div>
  </div>
</section>
```

- [ ] **Step 3: 浏览器验证**

工作原理区三步横排正常，code 块样式干净，无报错。

---

## Task 6: 下载区 + Footer

**Files:**
- Modify: `发布页/index.html`

- [ ] **Step 1: 追加 download + footer CSS**

```css
/* DOWNLOAD */
.download-section {
  padding: 120px 48px; text-align: center;
}
.download-inner { max-width: 480px; margin: 0 auto; }
.download-title {
  font-family: var(--serif); font-weight: 900;
  font-size: clamp(40px, 7vw, 72px); line-height: 1; margin-bottom: 12px;
}
.download-sub {
  color: var(--muted); font-size: 16px; margin-bottom: 48px;
}
.download-card {
  background: var(--bg2); border: 1.5px solid var(--border);
  border-radius: 24px; padding: 40px 36px;
}
.download-badge {
  display: inline-block;
  background: var(--text); color: var(--bg);
  font-size: 11px; font-weight: 700; letter-spacing: .1em; text-transform: uppercase;
  padding: 5px 14px; border-radius: 100px; margin-bottom: 24px;
}
.download-price {
  font-family: var(--serif); font-size: 72px; font-weight: 900; line-height: 1;
  margin-bottom: 4px;
}
.download-price sup { font-size: 28px; vertical-align: super; }
.download-price-note { color: var(--muted); font-size: 13px; margin-bottom: 32px; }
.download-features {
  list-style: none; text-align: left; margin-bottom: 32px;
}
.download-features li {
  display: flex; gap: 12px; align-items: flex-start;
  font-size: 14px; color: var(--text); line-height: 1.5;
  padding: 10px 0; border-bottom: 1px solid var(--border);
}
.download-features li:last-child { border-bottom: none; }
.download-features li::before {
  content: '✦'; color: var(--accent); font-size: 11px; margin-top: 2px; flex-shrink: 0;
}
.btn-download {
  display: block; width: 100%;
  background: var(--text); color: var(--bg);
  padding: 16px; border-radius: 100px;
  font-size: 16px; font-weight: 700; text-decoration: none;
  font-family: var(--serif);
  transition: background .2s, transform .15s, box-shadow .2s;
  box-shadow: 0 4px 20px rgba(44,26,14,0.15);
}
.btn-download:hover {
  background: var(--accent2);
  transform: translateY(-2px);
  box-shadow: 0 8px 32px rgba(44,26,14,0.2);
}
.download-compat { margin-top: 16px; font-size: 12px; color: var(--muted); }

/* FOOTER */
footer {
  border-top: 1px solid var(--border);
  padding: 40px 48px;
  display: flex; justify-content: space-between; align-items: center;
  flex-wrap: wrap; gap: 20px;
  background: var(--bg2);
}
.footer-logo {
  display: flex; align-items: center; gap: 8px;
  font-family: var(--serif); font-weight: 900; font-size: 18px; color: var(--text);
}
.footer-logo img { width: 28px; height: 28px; border-radius: 6px; }
.footer-note { font-size: 12px; color: var(--muted); }
.footer-links { display: flex; gap: 24px; }
.footer-links a { font-size: 12px; color: var(--muted); text-decoration: none; transition: color .2s; }
.footer-links a:hover { color: var(--text); }
```

- [ ] **Step 2: 追加 HTML**

```html
<!-- DOWNLOAD -->
<section id="download" class="download-section">
  <div class="download-inner">
    <div class="section-eyebrow" style="justify-content:center">下载</div>
    <h2 class="download-title reveal">完全免费</h2>
    <p class="download-sub reveal">开源，永久免费，给所有用 Claude Code 的人</p>
    <div class="download-card reveal">
      <div class="download-badge">永久免费 · 开源</div>
      <div class="download-price"><sup>¥</sup>0</div>
      <p class="download-price-note">macOS 12.4 及以上</p>
      <ul class="download-features">
        <li>11 种 Claude Code 状态真实帧动画</li>
        <li>本地 Webhook 服务，完全离线不联网</li>
        <li>权限申请可视化弹窗确认</li>
        <li>键盘检测 · 摸摸互动 · 拖拽动画</li>
        <li>一键安装脚本，30 秒配置完成</li>
      </ul>
      <a href="#" class="btn-download">下载狗蛋 Max</a>
      <p class="download-compat">macOS 12.4+ · Apple Silicon & Intel · 需辅助功能权限</p>
    </div>
  </div>
</section>

<!-- FOOTER -->
<footer>
  <div class="footer-logo">
    <img src="../logo 2.webp" alt="">
    <span>狗蛋 Max</span>
  </div>
  <p class="footer-note">用 Swift + SpriteKit 构建 · 运行在 macOS 上 · 开源免费</p>
  <div class="footer-links">
    <a href="#">GitHub</a>
    <a href="#">安装说明</a>
    <a href="#">反馈问题</a>
  </div>
</footer>
```

- [ ] **Step 3: 浏览器验证完整页面**

从头到尾滚动，五个区域全部正常，下载卡片样式干净。

---

## Task 7: 滚动动画 + 响应式 + 收尾

**Files:**
- Modify: `发布页/index.html`

- [ ] **Step 1: 追加 reveal 动画 CSS**

```css
/* REVEAL */
.reveal {
  opacity: 0; transform: translateY(28px);
  transition: opacity .7s ease, transform .7s ease;
}
.reveal.visible { opacity: 1; transform: none; }

/* RESPONSIVE */
@media (max-width: 768px) {
  nav { padding: 14px 20px; }
  .nav-links { display: none; }
  .states-section, .how-section, .download-section { padding: 80px 20px; }
  footer { padding: 32px 20px; flex-direction: column; text-align: center; }
  .how-steps {
    grid-template-columns: 1fr; gap: 24px;
  }
  .how-arrow { display: none; }
  .states-grid { grid-template-columns: repeat(2, 1fr); }
}
```

- [ ] **Step 2: 在 `<script>` 末尾追加 scroll reveal JS**

```javascript
// ── Scroll reveal ──────────────────────────────────────────
const revealObserver = new IntersectionObserver((entries) => {
  entries.forEach((entry, i) => {
    if (entry.isIntersecting) {
      entry.target.style.transitionDelay = (i * 0.04) + 's';
      entry.target.classList.add('visible');
    }
  });
}, { threshold: 0.12 });

document.addEventListener('DOMContentLoaded', () => {
  document.querySelectorAll('.reveal').forEach(el => revealObserver.observe(el));
});
```

- [ ] **Step 3: 最终全页验证**

1. 桌面端：从 Hero 滚动到 Footer，reveal 动画正常触发
2. 状态卡片：悬停播放，离开停止回到第 0 帧
3. 移动端（DevTools 375px）：nav 链接隐藏，状态卡片 2 列，工作原理竖排
4. 视频正常全屏覆盖，底部渐变过渡自然

---

## 验收标准

- [ ] 视频全屏播放，底部渐变连接到奶油色背景
- [ ] 11 个状态卡片全部显示首帧，悬停流畅播放
- [ ] 页面整体色调温暖，无暗色元素
- [ ] 移动端 375px 宽度布局正常
- [ ] 单 HTML 文件，无外部 JS 依赖
