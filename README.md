<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NekitDrop</title>
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
  html { scroll-behavior: smooth; }
  body {
    background: #0a0c12;
    color: #e8e8f0;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
    min-height: 100vh;
    overflow-x: hidden;
  }
  .bg-glow { position: fixed; inset: 0; z-index: -1; overflow: hidden; pointer-events: none; }
  .bg-glow::before, .bg-glow::after {
    content: ""; position: absolute; width: 500px; height: 500px;
    border-radius: 50%; opacity: 0.35;
  }
  .bg-glow::before { background: radial-gradient(circle, #ff8c00 0%, transparent 70%); top: -200px; left: -200px; }
  .bg-glow::after  { background: radial-gradient(circle, #c04dff 0%, transparent 70%); bottom: -200px; right: -200px; }
  .landing { max-width: 980px; margin: 0 auto; padding: 60px 20px 40px; }
  .logo { display: flex; align-items: center; justify-content: center; gap: 16px; margin-bottom: 14px; }
  .logo-icon {
    width: 72px; height: 72px; border-radius: 20px;
    background: linear-gradient(135deg, #ffd93b 0%, #ff8c00 100%);
    display: flex; align-items: center; justify-content: center;
    font-size: 38px; font-weight: 900; color: #1a1d28;
    box-shadow: 0 8px 40px rgba(255,140,0,0.5);
  }
  .logo-text {
    font-size: 48px; font-weight: 900; letter-spacing: -2px;
    background: linear-gradient(135deg, #ffd93b 0%, #ff8c00 50%, #ff4d94 100%);
    -webkit-background-clip: text; -webkit-text-fill-color: transparent;
  }
  .tagline { text-align: center; color: #8a90a0; font-size: 15px; margin-bottom: 32px; }
  .release-box {
    background: linear-gradient(135deg, rgba(255,217,59,0.14) 0%, rgba(255,140,0,0.08) 100%);
    border: 1px solid rgba(255,217,59,0.4); border-radius: 20px;
    padding: 28px; text-align: center; margin-bottom: 28px;
  }
  .release-label { font-size: 13px; letter-spacing: 3px; text-transform: uppercase; color: #ffd93b; margin-bottom: 10px; font-weight: 700; }
  .release-date { font-size: 42px; font-weight: 900; color: #fff; }
  .release-hint { font-size: 13px; color: #8a90a0; margin-top: 8px; }
  .cta-btn {
    display: block; margin: 0 auto 40px; max-width: 440px; width: 100%;
    text-align: center; padding: 22px 40px;
    background: linear-gradient(135deg, #ffd93b 0%, #ff8c00 100%);
    color: #1a1d28; border: none; border-radius: 16px;
    font-size: 18px; font-weight: 900; cursor: pointer;
    box-shadow: 0 12px 44px rgba(255,140,0,0.45); font-family: inherit;
  }
  .cta-btn:hover { transform: translateY(-3px); }
  .section-title { font-size: 22px; font-weight: 800; margin: 40px 0 18px; color: #fff; }
  .chances { display: flex; flex-direction: column; gap: 8px; }
  .chance-row {
    display: grid; grid-template-columns: 44px 1fr 140px 90px;
    align-items: center; gap: 12px; padding: 14px 16px;
    background: rgba(26,29,40,0.75); border: 1px solid #262a38; border-radius: 14px;
  }
  .chance-icon { width: 44px; height: 44px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-weight: 800; font-size: 14px; }
  .chance-name { font-size: 15px; font-weight: 700; color: #fff; }
  .chance-rarity { font-size: 12px; font-weight: 700; text-transform: uppercase; text-align: right; }
  .chance-percent { font-size: 15px; font-weight: 800; text-align: right; color: #fff; }
  .links { display: grid; grid-template-columns: repeat(auto-fit, minmax(240px, 1fr)); gap: 14px; }
  .link-card {
    display: flex; align-items: center; gap: 14px; padding: 18px;
    background: rgba(26,29,40,0.75); border: 1px solid #262a38; border-radius: 14px;
    text-decoration: none; color: #e8e8f0;
  }
  .link-icon { width: 48px; height: 48px; border-radius: 12px; display: flex; align-items: center; justify-content: center; font-size: 22px; color: #fff; font-weight: 900; }
  .link-card.tg .link-icon { background: #229ED9; }
  .link-card.tt .link-icon { background: #000; border: 1px solid #2a2a2a; }
  .link-title { font-size: 15px; font-weight: 700; }
  .link-sub { font-size: 12px; color: #8a90a0; margin-top: 3px; }
  .badges { display: flex; justify-content: center; gap: 10px; flex-wrap: wrap; margin: 50px 0 20px; }
  .badge { padding: 8px 14px; border-radius: 10px; font-size: 11px; font-weight: 700; text-transform: uppercase; }
  .badge.deepseek { background: rgba(77,159,255,0.12); color: #4d9fff; border: 1px solid rgba(77,159,255,0.3); }
  .badge.fun { background: rgba(255,217,59,0.1); color: #ffd93b; border: 1px solid rgba(255,217,59,0.3); }
  .footer-copy { text-align: center; color: #6a7080; font-size: 12px; padding-bottom: 30px; }

  #gameScreen { display: none; flex-direction: column; height: 100vh; max-width: 900px; margin: 0 auto; }
  #gameScreen.active { display: flex; }
  #landingScreen.hidden { display: none; }

  .topbar { display: flex; align-items: center; padding: 10px 14px; background: rgba(26,29,40,0.92); border-bottom: 1px solid #262a38; gap: 8px; flex-wrap: wrap; }
  .tb-btn { background: #262a38; border: 1px solid #353a4d; color: #e8e8f0; padding: 8px 14px; border-radius: 10px; cursor: pointer; font-size: 13px; font-weight: 600; font-family: inherit; }
  .tb-btn:hover { background: #2f3444; }
  .tb-spacer { flex: 1; }
  .curr-group { display: flex; align-items: center; background: #262a38; border: 1px solid #353a4d; border-radius: 10px; overflow: hidden; font-size: 13px; font-weight: 700; height: 34px; }
  .curr-group .curr-item { padding: 0 12px; white-space: nowrap; }
  .curr-group.lightning .curr-item { color: #ffd93b; }
  .curr-group.dollars .curr-item { color: #4dff88; }
  .plus-btn { background: rgba(255,217,59,0.15); border: none; border-left: 1px solid #353a4d; color: #ffd93b; font-size: 16px; font-weight: 800; padding: 0 12px; height: 100%; cursor: pointer; font-family: inherit; }
  .close-btn { background: #3a1a1a; border: 1px solid #5a2020; color: #ff8080; width: 34px; height: 34px; border-radius: 10px; cursor: pointer; font-size: 14px; font-weight: 700; font-family: inherit; }

  .roulette-wrap { flex: 1; position: relative; overflow: hidden; background: radial-gradient(ellipse at center, #1a1d28 0%, #0a0c12 85%); min-height: 0; }
  #rouletteCanvas { display: block; width: 100%; height: 100%; }

  .bottom { padding: 14px; display: flex; gap: 10px; align-items: center; background: rgba(26,29,40,0.92); border-top: 1px solid #262a38; }
  .help-btn { width: 48px; height: 48px; border-radius: 12px; background: #262a38; border: 1px solid #353a4d; color: #e8e8f0; font-size: 20px; font-weight: 700; cursor: pointer; font-family: inherit; }
  .bonus-btn { height: 48px; padding: 0 16px; border-radius: 12px; background: #262a38; border: 1px solid #353a4d; color: #e8e8f0; font-size: 13px; font-weight: 600; cursor: pointer; font-family: inherit; }
  .spin-btn { flex: 1; height: 52px; border-radius: 12px; background: linear-gradient(135deg, #ffd93b 0%, #ff8c00 100%); border: none; color: #1a1d28; font-size: 16px; font-weight: 900; cursor: pointer; font-family: inherit; box-shadow: 0 4px 20px rgba(255,140,0,0.4); }
  .spin-btn:disabled { opacity: 0.5; cursor: not-allowed; }

  .popup-overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.75); display: flex; align-items: center; justify-content: center; z-index: 100; padding: 20px; }
  .popup { background: #1e222e; border: 1px solid #353a4d; border-radius: 18px; padding: 26px; max-width: 440px; width: 100%; max-height: 85vh; overflow-y: auto; }
  .popup h2 { font-size: 20px; margin-bottom: 14px; text-align: center; color: #fff; font-weight: 800; }
  .popup p { font-size: 14px; color: #b8bcc8; text-align: center; margin-bottom: 18px; line-height: 1.6; white-space: pre-line; }
  .popup .btn-row { display: flex; gap: 10px; margin-top: 14px; }
  .popup button { flex: 1; padding: 13px; border-radius: 10px; border: none; background: #ffd93b; color: #1a1d28; font-size: 14px; font-weight: 800; cursor: pointer; font-family: inherit; }
  .popup button.secondary { background: #262a38; color: #e8e8f0; border: 1px solid #353a4d; }
  .popup button:disabled { opacity: 0.45; cursor: not-allowed; }
  .popup input { width: 100%; background: #262a38; border: 1px solid #353a4d; border-radius: 10px; padding: 13px; color: #fff; font-size: 15px; font-weight: 600; outline: none; margin-bottom: 12px; font-family: inherit; text-align: center; letter-spacing: 1px; }
  .popup input:focus { border-color: #ffd93b; }

  .shop-pack { display: flex; align-items: center; justify-content: space-between; padding: 12px 14px; background: #262a38; border-radius: 10px; margin-bottom: 8px; font-size: 14px; gap: 10px; }
  .shop-pack .sp-left { color: #ffd93b; font-weight: 800; }
  .shop-pack .sp-right { color: #4dff88; font-weight: 800; }
  .shop-pack button { background: #ffd93b; color: #1a1d28; border: none; padding: 8px 14px; border-radius: 8px; font-size: 12px; font-weight: 800; cursor: pointer; font-family: inherit; }
  .shop-pack button:disabled { opacity: 0.4; cursor: not-allowed; }

  .profile-grid { display: grid; grid-template-columns: 96px 1fr; gap: 14px; margin-bottom: 14px; align-items: center; }
  .avatar-box { width: 96px; height: 96px; border-radius: 16px; background: #262a38; border: 2px dashed #4a5068; display: flex; align-items: center; justify-content: center; cursor: pointer; font-size: 36px; color: #4a5068; overflow: hidden; }
  .avatar-box img { width: 100%; height: 100%; object-fit: cover; }
  .profile-stats { background: #262a38; border-radius: 10px; padding: 14px; font-size: 14px; color: #b8bcc8; margin-bottom: 14px; display: flex; justify-content: space-around; text-align: center; }
  .profile-stats b { color: #fff; font-size: 16px; display: block; margin-top: 4px; }
  .inv-title { font-size: 12px; color: #8a90a0; text-transform: uppercase; letter-spacing: 1.2px; margin: 10px 0 8px; text-align: left; font-weight: 700; }
  .inv-list { max-height: 240px; overflow-y: auto; display: flex; flex-direction: column; gap: 6px; }
  .inv-card { display: flex; align-items: center; justify-content: space-between; padding: 10px 12px; background: #262a38; border-radius: 10px; font-size: 13px; gap: 8px; }
  .inv-card .ic-name { font-weight: 800; color: #fff; flex: 1; }
  .inv-card .ic-rarity { font-size: 11px; font-weight: 700; }
  .inv-card button { background: #4dff88; border: none; color: #12141c; padding: 6px 10px; border-radius: 8px; font-size: 12px; font-weight: 800; cursor: pointer; font-family: inherit; }
  .empty-inv { text-align: center; color: #5a6070; font-size: 13px; padding: 24px; }

  .welcome-overlay { position: fixed; inset: 0; background: radial-gradient(circle at center, rgba(255,140,0,0.25) 0%, rgba(0,0,0,0.92) 70%); display: flex; align-items: center; justify-content: center; z-index: 200; padding: 20px; }
  .welcome-card { background: linear-gradient(160deg, #1e222e 0%, #16181f 100%); border: 2px solid rgba(255,217,59,0.5); border-radius: 24px; padding: 40px 32px; max-width: 420px; width: 100%; text-align: center; box-shadow: 0 0 60px rgba(255,140,0,0.5), 0 0 120px rgba(255,140,0,0.2); animation: welcomePop 0.6s cubic-bezier(0.2, 0.9, 0.3, 1.3); }
  @keyframes welcomePop { 0% { transform: scale(0.5); opacity: 0; } 100% { transform: scale(1); opacity: 1; } }
  .welcome-emoji { font-size: 72px; margin-bottom: 16px; display: inline-block; animation: bounce 1.6s ease-in-out infinite; }
  @keyframes bounce { 0%,100% { transform: translateY(0) rotate(-5deg); } 50% { transform: translateY(-12px) rotate(5deg); } }
  .welcome-title { font-size: 28px; font-weight: 900; margin-bottom: 12px; background: linear-gradient(135deg, #ffd93b 0%, #ff8c00 50%, #ff4d94 100%); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
  .welcome-text { font-size: 15px; color: #b8bcc8; margin-bottom: 24px; line-height: 1.6; }
  .welcome-bonus { display: inline-flex; align-items: center; gap: 10px; background: linear-gradient(135deg, rgba(255,217,59,0.25) 0%, rgba(255,140,0,0.15) 100%); border: 2px solid #ffd93b; border-radius: 14px; padding: 14px 24px; font-size: 22px; font-weight: 900; color: #ffd93b; margin-bottom: 24px; box-shadow: 0 0 30px rgba(255,217,59,0.5); }
  .welcome-btn { background: linear-gradient(135deg, #ffd93b 0%, #ff8c00 100%); color: #1a1d28; border: none; padding: 16px 40px; border-radius: 14px; font-size: 16px; font-weight: 900; cursor: pointer; font-family: inherit; box-shadow: 0 8px 30px rgba(255,140,0,0.5); }
</style>
</head>
<body>

<div class="bg-glow"></div>

<div class="landing" id="landingScreen">
  <div class="logo">
    <div class="logo-icon">N</div>
    <div class="logo-text">NekitDrop</div>
  </div>
  <p class="tagline">Бесплатная рулетка персонажей. Без CS. Без доната. Просто рофл.</p>

  <div class="release-box">
    <div class="release-label">Релиз</div>
    <div class="release-date">26.09.2026</div>
    <div class="release-hint">Уже доступно!</div>
  </div>

  <button class="cta-btn" id="playBtn">ИГРАТЬ СЕЙЧАС</button>

  <h2 class="section-title">Шансы персонажей</h2>
  <div class="chances" id="chancesList"></div>

  <h2 class="section-title">Соцсети</h2>
  <div class="links">
    <a class="link-card tg" href="https://t.me/ymarat123tube" target="_blank" rel="noopener">
      <div class="link-icon">✈</div>
      <div>
        <div class="link-title">Telegram-канал</div>
        <div class="link-sub">Промокоды и новости</div>
      </div>
    </a>
    <a class="link-card tt" href="https://www.tiktok.com/@k0tenok500" target="_blank" rel="noopener">
      <div class="link-icon">♪</div>
      <div>
        <div class="link-title">ТТК друга</div>
        <div class="link-sub">TikTok друга</div>
      </div>
    </a>
  </div>

  <div class="badges">
    <div class="badge deepseek">Сделано с помощью DeepSeek</div>
    <div class="badge fun">Создано в развлекательных целях</div>
  </div>
  <div class="footer-copy">© 2026 NekitDrop. Все совпадения случайны.</div>
</div>

<div id="gameScreen">
  <div class="topbar">
    <button class="tb-btn" id="profileBtn">Nekit</button>
    <button class="tb-btn" id="promoBtn">ПРОМО</button>
    <div class="tb-spacer"></div>
    <div class="curr-group lightning">
      <div class="curr-item" id="lightningLabel">⚡ 3</div>
      <button class="plus-btn" id="plusLightning">+</button>
    </div>
    <div class="curr-group dollars">
      <div class="curr-item" id="dollarsLabel">$ 0.0</div>
    </div>
    <button class="close-btn" id="closeBtn">✕</button>
  </div>

  <div class="roulette-wrap">
    <canvas id="rouletteCanvas"></canvas>
  </div>

  <div class="bottom">
    <button class="help-btn" id="helpBtn">?</button>
    <button class="bonus-btn" id="bonusBtn">Бонусы</button>
    <button class="spin-btn" id="spinBtn">КРУТИТЬ (1 ⚡)</button>
  </div>
</div>

<script>
"use strict";

var CHARACTERS = [
  { id: "nekit",    name: "Nekit",       rarity: "Секретный",   price: 67,  weight: 0.5 },
  { id: "sygak",    name: "Sygak",       rarity: "Ультра",      price: 40,  weight: 2 },
  { id: "vozduhan", name: "Воздухан",    rarity: "Легенда",     price: 26,  weight: 5 },
  { id: "kautoy",   name: "Типо Каутой", rarity: "Мега",        price: 13,  weight: 8 },
  { id: "floatup",  name: "floatup",     rarity: "Аркана",      price: 10,  weight: 12 },
  { id: "musor",    name: "MUSOR",       rarity: "Мифик",       price: 6,   weight: 15 },
  { id: "67",       name: "67",          rarity: "Эпик",        price: 3,   weight: 17 },
  { id: "mango",    name: "МАНGO",       rarity: "Сверхредкий", price: 1.5, weight: 19 },
  { id: "rozetka",  name: "Розетка",     rarity: "Редкий",      price: 1,   weight: 21.5 }
];

var RARITY_COLORS = {
  "Секретный":   "#ff2d2d",
  "Ультра":      "#ff8c00",
  "Легенда":     "#ffd700",
  "Мега":        "#c04dff",
  "Аркана":      "#ff4d94",
  "Мифик":       "#4d9fff",
  "Эпик":        "#7b4dff",
  "Сверхредкий": "#4dff88",
  "Редкий":      "#9e9e9e"
};

var SHOP_PACKS = [[10,20],[20,30],[30,50],[100,180],[250,400]];

var PROMOS = {
  "#mrtPromBalanc30": { type: "lightning", amount: 30, max: 1, label: "30 молний" },
  "#mrtPromDollar20": { type: "dollars",   amount: 20, max: 1, label: "20 долларов" },
  "#mrtPromBalanc2":  { type: "lightning", amount: 2,  max: 2, label: "2 молнии" }
};

var SAVE_KEY = "nekitsave_v5";
var TG_URL   = "https://t.me/ymarat123tube";
var TTK_URL  = "https://www.tiktok.com/@k0tenok500";

var state = {
  nickname: "Nekit",
  avatar: "",
  lightning: 3,
  dollars: 0,
  spins: 0,
  lastDaily: 0,
  claimedTg: false,
  claimedTt: false,
  inventory: [],
  promosUsed: {},
  welcomed: false
};

function saveNow() {
  try { localStorage.setItem(SAVE_KEY, JSON.stringify(state)); } catch(e) {}
}
function load() {
  try {
    var raw = localStorage.getItem(SAVE_KEY);
    if (raw) {
      var d = JSON.parse(raw);
      for (var k in d) state[k] = d[k];
    }
    if (!state.promosUsed) state.promosUsed = {};
  } catch(e) {}
}

function rollCharacter() {
  var r = Math.random() * 100;
  var acc = 0;
  for (var i = 0; i < CHARACTERS.length; i++) {
    acc += CHARACTERS[i].weight;
    if (r <= acc) return CHARACTERS[i];
  }
  return CHARACTERS[CHARACTERS.length - 1];
}
function fmtDollars(n) { return "$ " + (Math.round(n * 10) / 10).toFixed(1); }
function timeUntilDaily() {
  var now = Math.floor(Date.now() / 1000);
  return Math.max(86400 - (now - state.lastDaily), 0);
}

var el = {
  landingScreen:  document.getElementById("landingScreen"),
  gameScreen:     document.getElementById("gameScreen"),
  playBtn:        document.getElementById("playBtn"),
  profileBtn:     document.getElementById("profileBtn"),
  promoBtn:       document.getElementById("promoBtn"),
  lightningLabel: document.getElementById("lightningLabel"),
  dollarsLabel:   document.getElementById("dollarsLabel"),
  plusLightning:  document.getElementById("plusLightning"),
  closeBtn:       document.getElementById("closeBtn"),
  spinBtn:        document.getElementById("spinBtn"),
  helpBtn:        document.getElementById("helpBtn"),
  bonusBtn:       document.getElementById("bonusBtn"),
  canvas:         document.getElementById("rouletteCanvas")
};
var ctx = el.canvas.getContext("2d", { alpha: false });

function buildChances() {
  var list = document.getElementById("chancesList");
  var frag = document.createDocumentFragment();
  for (var i = 0; i < CHARACTERS.length; i++) {
    var c = CHARACTERS[i];
    var col = RARITY_COLORS[c.rarity] || "#666";
    var row = document.createElement("div");
    row.className = "chance-row";
    row.innerHTML =
      '<div class="chance-icon" style="background:rgba(0,0,0,0.35);color:' + col + '">' + c.name.substring(0,2) + '</div>' +
      '<div class="chance-name">' + c.name + '</div>' +
      '<div class="chance-rarity" style="color:' + col + '">' + c.rarity + '</div>' +
      '<div class="chance-percent" style="color:' + col + '">' + c.weight + '%</div>';
    frag.appendChild(row);
  }
  list.appendChild(frag);
}
buildChances();

var ITEM_W = 120, ITEM_H = 160, ITEM_GAP = 8;
var STRIDE = ITEM_W + ITEM_GAP;
var POOL = 24;
var canvasW = 0, canvasH = 0, dpr = 1;
var poolData = [];
var stripOffset = 0;
var spinning = false;
var spinStartTime = 0, spinStartOffset = 0, spinTargetOffset = 0;
var SPIN_DURATION = 4000;
var pendingPrize = null;
var rafId = null;
var needsRedraw = true;
var gameStarted = false;

function resizeCanvas() {
  var rect = el.canvas.parentElement.getBoundingClientRect();
  canvasW = rect.width;
  canvasH = rect.height;
  dpr = Math.min(window.devicePixelRatio || 1, 2);
  el.canvas.width = Math.floor(canvasW * dpr);
  el.canvas.height = Math.floor(canvasH * dpr);
  el.canvas.style.width = canvasW + "px";
  el.canvas.style.height = canvasH + "px";
  ctx.setTransform(dpr, 0, 0, dpr, 0, 0);
}

function buildPool() {
  poolData = [];
  for (var i = 0; i < POOL; i++) {
    poolData.push({ data: rollCharacter() });
  }
}

function roundRect(c, x, y, w, h, r) {
  c.beginPath();
  c.moveTo(x + r, y);
  c.lineTo(x + w - r, y);
  c.quadraticCurveTo(x + w, y, x + w, y + r);
  c.lineTo(x + w, y + h - r);
  c.quadraticCurveTo(x + w, y + h, x + w - r, y + h);
  c.lineTo(x + r, y + h);
  c.quadraticCurveTo(x, y + h, x, y + h - r);
  c.lineTo(x, y + r);
  c.quadraticCurveTo(x, y, x + r, y);
  c.closePath();
}

function drawCard(x, y, data) {
  var col = RARITY_COLORS[data.rarity] || "#666";
  roundRect(ctx, x, y, ITEM_W, ITEM_H, 14);
  ctx.fillStyle = "rgba(0,0,0,0.35)";
  ctx.fill();
  ctx.strokeStyle = col + "66";
  ctx.lineWidth = 1.5;
  ctx.stroke();

  var iconCX = x + ITEM_W / 2;
  var iconCY = y + 12 + 36;
  ctx.beginPath();
  ctx.arc(iconCX, iconCY, 36, 0, Math.PI * 2);
  ctx.fillStyle = "rgba(0,0,0,0.4)";
  ctx.fill();
  ctx.strokeStyle = col + "77";
  ctx.lineWidth = 1;
  ctx.stroke();

  ctx.fillStyle = col;
  ctx.font = "800 24px sans-serif";
  ctx.textAlign = "center";
  ctx.textBaseline = "middle";
  ctx.fillText(data.name.substring(0, 2), iconCX, iconCY);

  ctx.fillStyle = "#ffffff";
  ctx.font = "800 13px sans-serif";
  ctx.textBaseline = "alphabetic";
  ctx.fillText(data.name, iconCX, y + ITEM_H - 38);

  ctx.fillStyle = col;
  ctx.font = "800 10px sans-serif";
  ctx.fillText(data.rarity.toUpperCase(), iconCX, y + ITEM_H - 18);
}

function drawRoulette() {
  ctx.fillStyle = "#0a0c12";
  ctx.fillRect(0, 0, canvasW, canvasH);

  var centerY = canvasH / 2;
  var cardY = centerY - ITEM_H / 2;
  var total = POOL * STRIDE;
  var pointerX = canvasW / 2;

  for (var i = 0; i < POOL; i++) {
    var x = (stripOffset + i * STRIDE) % total;
    if (x < 0) x += total;
    var drawX = x - ITEM_W;
    if (drawX > canvasW + 200 || drawX + ITEM_W < -200) continue;
    drawCard(drawX, cardY, poolData[i].data);
  }

  ctx.fillStyle = "rgba(10,12,18,0.9)";
  ctx.fillRect(0, 0, 80, canvasH);
  ctx.fillRect(canvasW - 80, 0, 80, canvasH);
  ctx.fillStyle = "rgba(10,12,18,0.5)";
  ctx.fillRect(80, 0, 40, canvasH);
  ctx.fillRect(canvasW - 120, 0, 40, canvasH);

  ctx.save();
  var grad = ctx.createLinearGradient(pointerX, cardY - 14, pointerX, cardY + ITEM_H + 14);
  grad.addColorStop(0, "#ffd93b");
  grad.addColorStop(0.5, "#ff8c00");
  grad.addColorStop(1, "#ffd93b");
  ctx.shadowColor = "rgba(255,217,59,0.85)";
  ctx.shadowBlur = 14;
  ctx.fillStyle = grad;
  ctx.fillRect(pointerX - 2, cardY - 14, 4, ITEM_H + 28);
  ctx.shadowBlur = 0;

  ctx.beginPath();
  ctx.arc(pointerX, cardY - 14, 6, 0, Math.PI * 2);
  ctx.fillStyle = "#ffd93b";
  ctx.fill();
  ctx.beginPath();
  ctx.arc(pointerX, cardY + ITEM_H + 14, 6, 0, Math.PI * 2);
  ctx.fillStyle = "#ffd93b";
  ctx.fill();
  ctx.restore();
}

var lastFrameTime = 0;
function gameLoop(now) {
  if (!lastFrameTime) lastFrameTime = now;
  lastFrameTime = now;

  if (spinning) {
    var t = Math.min((now - spinStartTime) / SPIN_DURATION, 1);
    var ease = 1 - Math.pow(1 - t, 3);
    stripOffset = spinStartOffset + (spinTargetOffset - spinStartOffset) * ease;
    needsRedraw = true;
    if (t >= 1) finishSpin();
  }

  if (needsRedraw) {
    drawRoulette();
    needsRedraw = false;
  }

  rafId = requestAnimationFrame(gameLoop);
}

function startSpin() {
  if (spinning) return;
  if (state.lightning < 1) {
    showPopup("Мало молний", "Крутка стоит 1 ⚡.\nЗайди в «Бонусы» или «?».", [{text:"OK"}]);
    return;
  }
  state.lightning -= 1;
  updateCurrency();
  saveNow();

  spinning = true;
  needsRedraw = true;
  el.spinBtn.disabled = true;

  var prize = rollCharacter();
  pendingPrize = prize;

  spinStartOffset = stripOffset;
  spinStartTime = performance.now();

  var turns = 4 + Math.floor(Math.random() * 3);
  var totalPx = turns * POOL * STRIDE;
  spinTargetOffset = spinStartOffset + totalPx + Math.random() * STRIDE;

  var pointerX = canvasW / 2;
  var total = POOL * STRIDE;
  var bestI = 0, bestDist = Infinity;
  for (var i = 0; i < POOL; i++) {
    var x = (spinTargetOffset + i * STRIDE) % total;
    if (x < 0) x += total;
    var cardCenter = x - ITEM_W / 2;
    var dist = Math.abs(cardCenter - pointerX);
    if (dist < bestDist) { bestDist = dist; bestI = i; }
  }
  poolData[bestI].data = prize;
}

function finishSpin() {
  spinning = false;
  el.spinBtn.disabled = false;
  var prize = pendingPrize;
  pendingPrize = null;
  state.spins += 1;
  state.inventory.push({ id: prize.id, name: prize.name, rarity: prize.rarity, price: prize.price });
  saveNow();
  updateCurrency();
  showPopup("Выпал: " + prize.name, "Редкость: " + prize.rarity + "\nЦена: $" + prize.price, [{ text: "OK" }]);
  needsRedraw = true;
}

function updateCurrency() {
  el.lightningLabel.textContent = "⚡ " + state.lightning;
  el.dollarsLabel.textContent = fmtDollars(state.dollars);
  el.profileBtn.textContent = state.nickname;
}

function showPopup(title, body, buttons) {
  var overlay = document.createElement("div");
  overlay.className = "popup-overlay";
  var popup = document.createElement("div");
  popup.className = "popup";

  var buttonsHtml = "";
  if (buttons && buttons.length) {
    buttonsHtml = '<div class="btn-row">';
    for (var i = 0; i < buttons.length; i++) {
      var b = buttons[i];
      var cls = b.secondary ? "secondary" : "";
      buttonsHtml += '<button class="' + cls + '" data-i="' + i + '">' + b.text + '</button>';
    }
    buttonsHtml += "</div>";
  }

  popup.innerHTML = "<h2>" + title + "</h2><p>" + body + "</p>" + buttonsHtml;
  overlay.appendChild(popup);
  document.body.appendChild(overlay);

  var close = function() { overlay.remove(); };
  var btns = popup.querySelectorAll("button");
  for (var j = 0; j < btns.length; j++) {
    (function(btn) {
      btn.addEventListener("click", function() {
        var i = parseInt(btn.getAttribute("data-i"), 10);
        var action = buttons[i].action;
        close();
        if (typeof action === "function") action();
      });
    })(btns[j]);
  }
  overlay.addEventListener("click", function(e) { if (e.target === overlay) close(); });
}

function openShop() {
  var overlay = document.createElement("div");
  overlay.className = "popup-overlay";
  var popup = document.createElement("div");
  popup.className = "popup";

  var packsHtml = "";
  for (var i = 0; i < SHOP_PACKS.length; i++) {
    var lightning = SHOP_PACKS[i][0];
    var price = SHOP_PACKS[i][1];
    var canAfford = state.dollars >= price;
    packsHtml += '<div class="shop-pack">' +
      '<div class="sp-left">⚡ ' + lightning + '</div>' +
      '<div class="sp-right">$ ' + price + '</div>' +
      '<button data-i="' + i + '"' + (canAfford ? "" : " disabled") + '>Купить</button>' +
      '</div>';
  }

  popup.innerHTML = '<h2>Купить молнии</h2><p style="margin-bottom:14px;">Обмен $ на ⚡</p>' +
    '<div id="shopList">' + packsHtml + '</div>' +
    '<div class="btn-row"><button class="secondary" id="shopClose">Закрыть</button></div>';
  overlay.appendChild(popup);
  document.body.appendChild(overlay);

  var btns = popup.querySelectorAll("#shopList button");
  for (var j = 0; j < btns.length; j++) {
    (function(btn) {
      btn.addEventListener("click", function() {
        var i = parseInt(btn.getAttribute("data-i"), 10);
        var lightning = SHOP_PACKS[i][0];
        var price = SHOP_PACKS[i][1];
        if (state.dollars < price) return;
        state.dollars -= price;
        state.lightning += lightning;
        saveNow();
        updateCurrency();
        overlay.remove();
        openShop();
      });
    })(btns[j]);
  }
  popup.querySelector("#shopClose").addEventListener("click", function() { overlay.remove(); });
  overlay.addEventListener("click", function(e) { if (e.target === overlay) overlay.remove(); });
}

function openPromo() {
  var overlay = document.createElement("div");
  overlay.className = "popup-overlay";
  var popup = document.createElement("div");
  popup.className = "popup";
  popup.innerHTML =
    '<h2>Активировать промокод</h2>' +
    '<p>Введи код и получи награду</p>' +
    '<input id="promoInput" placeholder="#ВВЕДИ_КОД" maxlength="40" autocomplete="off">' +
    '<div class="btn-row">' +
    '<button class="secondary" id="promoCancel">Отмена</button>' +
    '<button id="promoApply">Активировать</button>' +
    '</div>';
  overlay.appendChild(popup);
  document.body.appendChild(overlay);

  var input = popup.querySelector("#promoInput");
  setTimeout(function() { input.focus(); }, 50);

  var apply = function() {
    var code = input.value.trim();
    if (!code) return;
    var promo = PROMOS[code];
    if (!promo) {
      showPopup("Неверный код", "Такого промокода не существует.", [{text:"OK"}]);
      return;
    }
    var used = state.promosUsed[code] || 0;
    if (used >= promo.max) {
      showPopup("Лимит исчерпан", 'Промокод "' + code + '" уже использован ' + used + ' раз(а).', [{text:"OK"}]);
      return;
    }
    state.promosUsed[code] = used + 1;
    if (promo.type === "lightning") state.lightning += promo.amount;
    else if (promo.type === "dollars") state.dollars += promo.amount;
    saveNow();
    updateCurrency();
    overlay.remove();
    showPopup("Промокод активирован!", "Ты получил: " + promo.label, [{text:"Круто!"}]);
  };

  popup.querySelector("#promoApply").addEventListener("click", apply);
  popup.querySelector("#promoCancel").addEventListener("click", function() { overlay.remove(); });
  input.addEventListener("keydown", function(e) { if (e.key === "Enter") apply(); });
  overlay.addEventListener("click", function(e) { if (e.target === overlay) overlay.remove(); });
}

function openProfile() {
  var overlay = document.createElement("div");
  overlay.className = "popup-overlay";
  var popup = document.createElement("div");
  popup.className = "popup";
  popup.innerHTML =
    '<h2>Профиль</h2>' +
    '<div class="profile-grid">' +
      '<div class="avatar-box" id="ppAvatar">' + (state.avatar ? '<img src="' + state.avatar + '">' : "📷") + '</div>' +
      '<input id="ppNick" value="' + state.nickname.replace(/"/g, "&quot;") + '" maxlength="20" style="margin:0;">' +
    '</div>' +
    '<div class="profile-stats">' +
      '<div>⚡<b>' + state.lightning + '</b></div>' +
      '<div>$<b>' + fmtDollars(state.dollars).replace("$ ", "") + '</b></div>' +
      '<div>Круток<b>' + state.spins + '</b></div>' +
    '</div>' +
    '<div class="inv-title">Инвентарь (' + state.inventory.length + ')</div>' +
    '<div class="inv-list" id="ppInv"></div>' +
    '<div class="btn-row"><button id="ppClose">Закрыть</button></div>';
  overlay.appendChild(popup);
  document.body.appendChild(overlay);

  var invList = popup.querySelector("#ppInv");
  if (state.inventory.length === 0) {
    invList.innerHTML = '<div class="empty-inv">Пока пусто. Крути рулетку!</div>';
  } else {
    var frag = document.createDocumentFragment();
    for (var idx = 0; idx < state.inventory.length; idx++) {
      var item = state.inventory[idx];
      var col = RARITY_COLORS[item.rarity] || "#888";
      var card = document.createElement("div");
      card.className = "inv-card";
      card.innerHTML =
        '<div class="ic-name">' + item.name + '</div>' +
        '<div class="ic-rarity" style="color:' + col + '">' + item.rarity + '</div>' +
        '<button data-i="' + idx + '">Продать $' + item.price + '</button>';
      frag.appendChild(card);
    }
    invList.appendChild(frag);
    var btns = invList.querySelectorAll("button");
    for (var j = 0; j < btns.length; j++) {
      (function(btn) {
        btn.addEventListener("click", function() {
          var i = parseInt(btn.getAttribute("data-i"), 10);
          var it = state.inventory[i];
          state.dollars += it.price;
          state.inventory.splice(i, 1);
          saveNow();
          updateCurrency();
          overlay.remove();
          openProfile();
        });
      })(btns[j]);
    }
  }

  var avatarBox = popup.querySelector("#ppAvatar");
  var fileInput = document.createElement("input");
  fileInput.type = "file";
  fileInput.accept = "image/*";
  fileInput.style.display = "none";
  popup.appendChild(fileInput);
  avatarBox.addEventListener("click", function() { fileInput.click(); });
  fileInput.addEventListener("change", function() {
    var file = fileInput.files[0];
    if (!file) return;
    var reader = new FileReader();
    reader.onload = function(e) {
      state.avatar = e.target.result;
      saveNow();
      avatarBox.innerHTML = '<img src="' + state.avatar + '">';
    };
    reader.readAsDataURL(file);
  });

  var nickInput = popup.querySelector("#ppNick");
  nickInput.addEventListener("change", function() {
    var t = nickInput.value.trim();
    if (t) { state.nickname = t; saveNow(); updateCurrency(); }
  });

  popup.querySelector("#ppClose").addEventListener("click", function() { overlay.remove(); });
  overlay.addEventListener("click", function(e) { if (e.target === overlay) overlay.remove(); });
}

function openBonus() {
  var dailyReady = timeUntilDaily() === 0;
  var dailyLeft = timeUntilDaily();
  var h = Math.floor(dailyLeft / 3600);
  var m = Math.floor((dailyLeft % 3600) / 60);

  var overlay = document.createElement("div");
  overlay.className = "popup-overlay";
  var popup = document.createElement("div");
  popup.className = "popup";
  popup.innerHTML =
    '<h2>Бонусы</h2>' +
    '<div style="display:flex;flex-direction:column;gap:10px;margin:14px 0;">' +
      '<button class="secondary" id="bnTg"' + (state.claimedTg ? " disabled" : "") + '>' +
        (state.claimedTg ? "✓ Telegram получен" : "+3 ⚡ за подписку на Telegram") +
      '</button>' +
      '<button class="secondary" id="bnTtk"' + (state.claimedTt ? " disabled" : "") + '>' +
        (state.claimedTt ? "✓ ТТК получен" : "+3 ⚡ за подписку на ТТК друга") +
      '</button>' +
      '<button id="bnDaily"' + (dailyReady ? "" : " disabled") + '>' +
        (dailyReady ? "+2 ⚡ Ежедневный бонус" : "⏳ Через " + (h<10?"0":"") + h + ":" + (m<10?"0":"") + m) +
      '</button>' +
    '</div>' +
    '<div class="btn-row"><button class="secondary" id="bnClose">Закрыть</button></div>';
  overlay.appendChild(popup);
  document.body.appendChild(overlay);

  popup.querySelector("#bnTg").addEventListener("click", function() {
    if (state.claimedTg) return;
    window.open(TG_URL, "_blank");
    state.claimedTg = true;
    state.lightning += 3;
    saveNow();
    updateCurrency();
    overlay.remove();
    openBonus();
  });
  popup.querySelector("#bnTtk").addEventListener("click", function() {
    if (state.claimedTt) return;
    window.open(TTK_URL, "_blank");
    state.claimedTt = true;
    state.lightning += 3;
    saveNow();
    updateCurrency();
    overlay.remove();
    openBonus();
  });
  popup.querySelector("#bnDaily").addEventListener("click", function() {
    if (timeUntilDaily() > 0) return;
    state.lastDaily = Math.floor(Date.now() / 1000);
    state.lightning += 2;
    saveNow();
    updateCurrency();
    overlay.remove();
    openBonus();
  });
  popup.querySelector("#bnClose").addEventListener("click", function() { overlay.remove(); });
  overlay.addEventListener("click", function(e) { if (e.target === overlay) overlay.remove(); });
}

function showWelcome() {
  var overlay = document.createElement("div");
  overlay.className = "welcome-overlay";
  var card = document.createElement("div");
  card.className = "welcome-card";
  card.innerHTML =
    '<div class="welcome-emoji">🎉</div>' +
    '<div class="welcome-title">Добро пожаловать!</div>' +
    '<div class="welcome-text">Это твой первый вход в NekitDrop.<br>Держи подарок на старт:</div>' +
    '<div class="welcome-bonus">+1 ⚡</div>' +
    '<div><button class="welcome-btn" id="welcomeOk">Погнали!</button></div>';
  overlay.appendChild(card);
  document.body.appendChild(overlay);

  var confettiColors = ["#ffd93b","#ff8c00","#ff4d94","#4dff88","#4d9fff","#c04dff"];
  var confettiPieces = [];
  for (var i = 0; i < 40; i++) {
    var p = document.createElement("div");
    p.className = "confetti";
    p.style.left = (Math.random() * 100) + "vw";
    p.style.background = confettiColors[Math.floor(Math.random() * confettiColors.length)];
    p.style.animationDuration = (2 + Math.random() * 2) + "s";
    p.style.animationDelay = (Math.random() * 0.8) + "s";
    p.style.width = (6 + Math.random() * 8) + "px";
    p.style.height = (6 + Math.random() * 8) + "px";
    p.style.borderRadius = Math.random() > 0.5 ? "50%" : "2px";
    document.body.appendChild(p);
    confettiPieces.push(p);
  }

  var cleanup = function() {
    for (var k = 0; k < confettiPieces.length; k++) {
      if (confettiPieces[k].parentNode) confettiPieces[k].parentNode.removeChild(confettiPieces[k]);
    }
  };

  card.querySelector("#welcomeOk").addEventListener("click", function() {
    state.welcomed = true;
    state.lightning += 1;
    saveNow();
    updateCurrency();
    overlay.remove();
    cleanup();
  });
}

function enterGame() {
  el.landingScreen.classList.add("hidden");
  el.gameScreen.classList.add("active");

  setTimeout(function() {
    resizeCanvas();
    if (!gameStarted) {
      buildPool();
      gameStarted = true;
    }
    needsRedraw = true;
    if (!rafId) rafId = requestAnimationFrame(gameLoop);

    if (!state.welcomed) {
      showWelcome();
    }
  }, 50);
}

function exitGame() {
  el.gameScreen.classList.remove("active");
  el.landingScreen.classList.remove("hidden");
  saveNow();
  if (rafId) {
    cancelAnimationFrame(rafId);
    rafId = null;
  }
}

el.playBtn.addEventListener("click", enterGame);
el.profileBtn.addEventListener("click", openProfile);
el.promoBtn.addEventListener("click", openPromo);
el.bonusBtn.addEventListener("click", openBonus);
el.spinBtn.addEventListener("click", startSpin);
el.plusLightning.addEventListener("click", openShop);
el.helpBtn.addEventListener("click", function() {
  window.scrollTo({ top: 0, behavior: "smooth" });
  if (el.gameScreen.classList.contains("active")) {
    exitGame();
  }
});
el.closeBtn.addEventListener("click", function() {
  saveNow();
  if (confirm("Выйти в главное меню? Прогресс сохранён.")) {
    exitGame();
  }
});

window.addEventListener("resize", resizeCanvas, { passive: true });
window.addEventListener("beforeunload", saveNow, { passive: true });

load();
updateCurrency();
</script>
</body>
</html>
