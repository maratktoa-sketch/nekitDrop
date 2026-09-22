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

  .bg-glow {
    position: fixed;
    inset: 0;
    z-index: -1;
    overflow: hidden;
    pointer-events: none;
    contain: strict;
  }
  .bg-glow::before, .bg-glow::after {
    content: "";
    position: absolute;
    width: 500px;
    height: 500px;
    border-radius: 50%;
    opacity: 0.35;
    will-change: transform;
  }
  .bg-glow::before {
    background: radial-gradient(circle, #ff8c00 0%, transparent 70%);
    top: -200px; left: -200px;
    animation: float1 22s ease-in-out infinite;
  }
  .bg-glow::after {
    background: radial-gradient(circle, #c04dff 0%, transparent 70%);
    bottom: -200px; right: -200px;
    animation: float2 26s ease-in-out infinite;
  }
  @keyframes float1 {
    0%,100% { transform: translate3d(0,0,0); }
    50%     { transform: translate3d(80px, 60px, 0); }
  }
  @keyframes float2 {
    0%,100% { transform: translate3d(0,0,0); }
    50%     { transform: translate3d(-80px, -60px, 0); }
  }

  .landing { max-width: 980px; margin: 0 auto; padding: 60px 20px 40px; position: relative; }

  .logo {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 16px;
    margin-bottom: 14px;
  }
  .logo-icon {
    width: 72px; height: 72px;
    border-radius: 20px;
    background: linear-gradient(135deg, #ffd93b 0%, #ff8c00 100%);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 38px;
    font-weight: 900;
    color: #1a1d28;
    box-shadow: 0 8px 40px rgba(255,140,0,0.5);
    will-change: transform;
    animation: pulse 4s ease-in-out infinite;
  }
  @keyframes pulse {
    0%,100% { transform: scale(1); }
    50%     { transform: scale(1.04); }
  }
  .logo-text {
    font-size: 48px;
    font-weight: 900;
    letter-spacing: -2px;
    background: linear-gradient(135deg, #ffd93b 0%, #ff8c00 50%, #ff4d94 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }
  .tagline {
    text-align: center;
    color: #8a90a0;
    font-size: 15px;
    margin-bottom: 32px;
  }

  .release-box {
    background: linear-gradient(135deg, rgba(255,217,59,0.14) 0%, rgba(255,140,0,0.08) 100%);
    border: 1px solid rgba(255,217,59,0.4);
    border-radius: 20px;
    padding: 28px;
    text-align: center;
    margin-bottom: 28px;
    box-shadow: 0 12px 60px rgba(255,140,0,0.2);
  }
  .release-label {
    font-size: 13px;
    letter-spacing: 3px;
    text-transform: uppercase;
    color: #ffd93b;
    margin-bottom: 10px;
    font-weight: 700;
  }
  .release-date {
    font-size: 42px;
    font-weight: 900;
    color: #fff;
    letter-spacing: -1.5px;
    font-variant-numeric: tabular-nums;
  }
  .release-hint {
    font-size: 13px;
    color: #8a90a0;
    margin-top: 8px;
  }

  .cta-btn {
    display: block;
    margin: 0 auto 40px;
    max-width: 440px;
    width: 100%;
    text-align: center;
    padding: 22px 40px;
    background: linear-gradient(135deg, #ffd93b 0%, #ff8c00 100%);
    color: #1a1d28;
    border: none;
    border-radius: 16px;
    font-size: 18px;
    font-weight: 900;
    letter-spacing: 0.5px;
    cursor: pointer;
    box-shadow: 0 12px 44px rgba(255,140,0,0.45);
    transition: transform 0.15s, box-shadow 0.15s;
    font-family: inherit;
    will-change: transform;
  }
  .cta-btn:hover:not(:disabled) {
    transform: translateY(-3px);
    box-shadow: 0 16px 60px rgba(255,140,0,0.65);
  }
  .cta-btn:active:not(:disabled) { transform: translateY(-1px); }
  .cta-btn:disabled {
    background: #262a38;
    color: #6a7080;
    box-shadow: none;
    cursor: not-allowed;
    border: 1px solid #353a4d;
  }
  .cta-hint {
    text-align: center;
    color: #8a90a0;
    font-size: 13px;
    margin-top: -28px;
    margin-bottom: 32px;
  }
  .cta-hint.hidden { display: none; }

  .section-title {
    font-size: 22px;
    font-weight: 800;
    margin: 40px 0 18px;
    color: #fff;
    display: flex;
    align-items: center;
    gap: 10px;
  }
  .section-title::before {
    content: "";
    width: 4px;
    height: 22px;
    background: linear-gradient(180deg, #ffd93b, #ff8c00);
    border-radius: 2px;
  }

  .chances { display: flex; flex-direction: column; gap: 8px; }
  .chance-row {
    display: grid;
    grid-template-columns: 44px 1fr 140px 90px;
    align-items: center;
    gap: 12px;
    padding: 14px 16px;
    background: rgba(26,29,40,0.75);
    border: 1px solid #262a38;
    border-radius: 14px;
    transition: transform 0.15s, border-color 0.15s;
    contain: layout style;
  }
  .chance-row:hover {
    transform: translateX(4px);
    border-color: #353a4d;
  }
  .chance-icon {
    width: 44px; height: 44px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 14px;
    font-weight: 800;
    color: rgba(255,255,255,0.9);
    background: rgba(0,0,0,0.35);
  }
  .chance-name { font-size: 15px; font-weight: 700; color: #fff; }
  .chance-rarity {
    font-size: 12px;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.5px;
    text-align: right;
  }
  .chance-percent {
    font-size: 15px;
    font-weight: 800;
    text-align: right;
    font-variant-numeric: tabular-nums;
    color: #fff;
  }

  .links {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
    gap: 14px;
  }
  .link-card {
    display: flex;
    align-items: center;
    gap: 14px;
    padding: 18px;
    background: rgba(26,29,40,0.75);
    border: 1px solid #262a38;
    border-radius: 14px;
    text-decoration: none;
    color: #e8e8f0;
    transition: transform 0.15s, border-color 0.15s;
    contain: layout style;
  }
  .link-card:hover {
    transform: translateY(-3px);
    border-color: #353a4d;
  }
  .link-icon {
    width: 48px; height: 48px;
    border-radius: 12px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 22px;
    flex-shrink: 0;
    color: #fff;
    font-weight: 900;
  }
  .link-card.tg .link-icon { background: #229ED9; }
  .link-card.tt .link-icon { background: #000; border: 1px solid #2a2a2a; }
  .link-title { font-size: 15px; font-weight: 700; }
  .link-sub { font-size: 12px; color: #8a90a0; margin-top: 3px; }

  .badges {
    display: flex;
    justify-content: center;
    gap: 10px;
    flex-wrap: wrap;
    margin: 50px 0 20px;
  }
  .badge {
    padding: 8px 14px;
    border-radius: 10px;
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 0.3px;
    text-transform: uppercase;
  }
  .badge.deepseek {
    background: rgba(77,159,255,0.12);
    color: #4d9fff;
    border: 1px solid rgba(77,159,255,0.3);
  }
  .badge.fun {
    background: rgba(255,217,59,0.1);
    color: #ffd93b;
    border: 1px solid rgba(255,217,59,0.3);
  }
  .footer-copy {
    text-align: center;
    color: #6a7080;
    font-size: 12px;
    padding-bottom: 30px;
  }

  #gameScreen {
    display: none;
    flex-direction: column;
    height: 100vh;
    max-width: 900px;
    margin: 0 auto;
    position: relative;
  }
  #gameScreen.active { display: flex; }
  #landingScreen.hidden { display: none; }

  .topbar {
    display: flex;
    align-items: center;
    padding: 10px 14px;
    background: rgba(26,29,40,0.92);
    border-bottom: 1px solid #262a38;
    gap: 8px;
    flex-wrap: wrap;
    z-index: 10;
    contain: layout;
  }
  .tb-btn {
    background: #262a38;
    border: 1px solid #353a4d;
    color: #e8e8f0;
    padding: 8px 14px;
    border-radius: 10px;
    cursor: pointer;
    font-size: 13px;
    font-weight: 600;
    transition: background 0.15s, border-color 0.15s;
    font-family: inherit;
  }
  .tb-btn:hover { background: #2f3444; border-color: #4a5068; }
  .tb-spacer { flex: 1; }
  .curr-group {
    display: flex;
    align-items: center;
    background: #262a38;
    border: 1px solid #353a4d;
    border-radius: 10px;
    overflow: hidden;
    font-size: 13px;
    font-weight: 700;
    height: 34px;
  }
  .curr-group .curr-item { padding: 0 12px; white-space: nowrap; }
  .curr-group.lightning .curr-item { color: #ffd93b; }
  .curr-group.dollars .curr-item   { color: #4dff88; }
  .plus-btn {
    background: rgba(255,217,59,0.15);
    border: none;
    border-left: 1px solid #353a4d;
    color: #ffd93b;
    font-size: 16px;
    font-weight: 800;
    padding: 0 12px;
    height: 100%;
    cursor: pointer;
    transition: background 0.15s;
    font-family: inherit;
  }
  .plus-btn:hover { background: rgba(255,217,59,0.3); }
  .close-btn {
    background: #3a1a1a;
    border: 1px solid #5a2020;
    color: #ff8080;
    width: 34px; height: 34px;
    border-radius: 10px;
    cursor: pointer;
    font-size: 14px;
    font-weight: 700;
    font-family: inherit;
  }
  .close-btn:hover { background: #4a2020; }

  .roulette-wrap {
    flex: 1;
    position: relative;
    overflow: hidden;
    background: radial-gradient(ellipse at center, #1a1d28 0%, #0a0c12 85%);
    min-height: 0;
    contain: strict;
  }
  #rouletteCanvas { display: block; width: 100%; height: 100%; }

  .bottom {
    padding: 14px;
    display: flex;
    gap: 10px;
    align-items: center;
    background: rgba(26,29,40,0.92);
    border-top: 1px solid #262a38;
    z-index: 10;
    contain: layout;
  }
  .help-btn {
    width: 48px; height: 48px;
    border-radius: 12px;
    background: #262a38;
    border: 1px solid #353a4d;
    color: #e8e8f0;
    font-size: 20px;
    font-weight: 700;
    cursor: pointer;
    font-family: inherit;
    transition: background 0.15s;
  }
  .help-btn:hover { background: #2f3444; }
  .bonus-btn {
    height: 48px;
    padding: 0 16px;
    border-radius: 12px;
    background: #262a38;
    border: 1px solid #353a4d;
    color: #e8e8f0;
    font-size: 13px;
    font-weight: 600;
    cursor: pointer;
    white-space: nowrap;
    font-family: inherit;
    transition: background 0.15s;
  }
  .bonus-btn:hover { background: #2f3444; }
  .spin-btn {
    flex: 1;
    height: 52px;
    border-radius: 12px;
    background: linear-gradient(135deg, #ffd93b 0%, #ff8c00 100%);
    border: none;
    color: #1a1d28;
    font-size: 16px;
    font-weight: 900;
    cursor: pointer;
    letter-spacing: 0.5px;
    transition: transform 0.15s, box-shadow 0.15s;
    box-shadow: 0 4px 20px rgba(255,140,0,0.4);
    font-family: inherit;
    will-change: transform;
  }
  .spin-btn:hover:not(:disabled) {
    transform: translateY(-1px);
    box-shadow: 0 6px 26px rgba(255,140,0,0.6);
  }
  .spin-btn:disabled { opacity: 0.5; cursor: not-allowed; }

  .popup-overlay {
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.75);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 100;
    padding: 20px;
    animation: fadeIn 0.15s;
    contain: strict;
  }
  @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
  .popup {
    background: #1e222e;
    border: 1px solid #353a4d;
    border-radius: 18px;
    padding: 26px;
    max-width: 440px;
    width: 100%;
    box-shadow: 0 24px 80px rgba(0,0,0,0.7);
    animation: popIn 0.2s cubic-bezier(0.2, 0.9, 0.3, 1.3);
    max-height: 85vh;
    overflow-y: auto;
  }
  @keyframes popIn {
    from { transform: scale(0.85); opacity: 0; }
    to   { transform: scale(1); opacity: 1; }
  }
  .popup h2 {
    font-size: 20px;
    margin-bottom: 14px;
    text-align: center;
    color: #fff;
    font-weight: 800;
  }
  .popup p {
    font-size: 14px;
    color: #b8bcc8;
    text-align: center;
    margin-bottom: 18px;
    line-height: 1.6;
    white-space: pre-line;
  }
  .popup .btn-row { display: flex; gap: 10px; margin-top: 14px; }
  .popup button {
    flex: 1;
    padding: 13px;
    border-radius: 10px;
    border: none;
    background: #ffd93b;
    color: #1a1d28;
    font-size: 14px;
    font-weight: 800;
    cursor: pointer;
    transition: background 0.15s;
    font-family: inherit;
  }
  .popup button:hover:not(:disabled) { background: #ffdf55; }
  .popup button.secondary {
    background: #262a38;
    color: #e8e8f0;
    border: 1px solid #353a4d;
  }
  .popup button.secondary:hover { background: #2f3444; }
  .popup button:disabled { opacity: 0.45; cursor: not-allowed; }

  .popup input {
    width: 100%;
    background: #262a38;
    border: 1px solid #353a4d;
    border-radius: 10px;
    padding: 13px;
    color: #fff;
    font-size: 15px;
    font-weight: 600;
    outline: none;
    margin-bottom: 12px;
    font-family: inherit;
    text-align: center;
    letter-spacing: 1px;
  }
  .popup input:focus { border-color: #ffd93b; }

  .shop-pack {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 12px 14px;
    background: #262a38;
    border-radius: 10px;
    margin-bottom: 8px;
    font-size: 14px;
    gap: 10px;
  }
  .shop-pack .sp-left { color: #ffd93b; font-weight: 800; }
  .shop-pack .sp-right { color: #4dff88; font-weight: 800; }
  .shop-pack button {
    background: #ffd93b;
    color: #1a1d28;
    border: none;
    padding: 8px 14px;
    border-radius: 8px;
    font-size: 12px;
    font-weight: 800;
    cursor: pointer;
    flex: 0 0 auto;
    font-family: inherit;
  }
  .shop-pack button:disabled { opacity: 0.4; cursor: not-allowed; }

  .profile-grid {
    display: grid;
    grid-template-columns: 96px 1fr;
    gap: 14px;
    margin-bottom: 14px;
    align-items: center;
  }
  .avatar-box {
    width: 96px; height: 96px;
    border-radius: 16px;
    background: #262a38;
    border: 2px dashed #4a5068;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    font-size: 36px;
    color: #4a5068;
    overflow: hidden;
    transition: border-color 0.15s, color 0.15s;
  }
  .avatar-box:hover { border-color: #ffd93b; color: #ffd93b; }
  .avatar-box img { width: 100%; height: 100%; object-fit: cover; }
  .profile-stats {
    background: #262a38;
    border-radius: 10px;
    padding: 14px;
    font-size: 14px;
    color: #b8bcc8;
    margin-bottom: 14px;
    display: flex;
    justify-content: space-around;
    text-align: center;
  }
  .profile-stats b { color: #fff; font-size: 16px; display: block; margin-top: 4px; }
  .inv-title {
    font-size: 12px;
    color: #8a90a0;
    text-transform: uppercase;
    letter-spacing: 1.2px;
    margin: 10px 0 8px;
    text-align: left;
    font-weight: 700;
  }
  .inv-list {
    max-height: 240px;
    overflow-y: auto;
    display: flex;
    flex-direction: column;
    gap: 6px;
  }
  .inv-card {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 10px 12px;
    background: #262a38;
    border-radius: 10px;
    font-size: 13px;
    gap: 8px;
  }
  .inv-card .ic-name { font-weight: 800; color: #fff; flex: 1; }
  .inv-card .ic-rarity { font-size: 11px; font-weight: 700; }
  .inv-card button {
    background: #4dff88;
    border: none;
    color: #12141c;
    padding: 6px 10px;
    border-radius: 8px;
    font-size: 12px;
    font-weight: 800;
    cursor: pointer;
    white-space: nowrap;
    flex: 0 0 auto;
    font-family: inherit;
  }
  .inv-card button:hover { background: #6aff9c; }
  .empty-inv {
    text-align: center;
    color: #5a6070;
    font-size: 13px;
    padding: 24px;
  }

  @media (max-width: 600px) {
    .logo-text { font-size: 34px; }
    .logo-icon { width: 56px; height: 56px; font-size: 30px; }
    .release-date { font-size: 28px; }
    .chance-row {
      grid-template-columns: 40px 1fr 80px;
      padding: 12px;
      gap: 8px;
    }
    .chance-icon { width: 40px; height: 40px; font-size: 12px; }
    .chance-rarity { display: none; }
    .topbar { padding: 8px; gap: 6px; }
    .tb-btn { padding: 6px 10px; font-size: 12px; }
  }
</style>
</head>
<body>

<div class="bg-glow"></div>

<!-- ================= LANDING ================= -->
<div class="landing" id="landingScreen">
  <div class="logo">
    <div class="logo-icon">N</div>
    <div class="logo-text">NekitDrop</div>
  </div>
  <p class="tagline">Бесплатная рулетка персонажей. Без CS. Без доната. Просто рофл.</p>

  <div class="release-box" id="releaseBox">
    <div class="release-label">Релиз</div>
    <div class="release-date">26.09.2026</div>
    <div class="release-hint" id="releaseHint">Отмечай в календаре — не пропусти</div>
  </div>

  <button class="cta-btn" id="playBtn" disabled>ИГРАТЬ СЕЙЧАС</button>
  <div class="cta-hint" id="ctaHint">Доступно с 26.09.2026</div>

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

<!-- ================= GAME ================= -->
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

const RELEASE_DATE = new Date(2026, 8, 26, 0, 0, 0);

const CHARACTERS = [
  { id: "nekit",    name: "Nekit",       rarity: "Секретный",   price: 67,  weight: 0.5 },
  { id: "sygak",    name: "Sygak",       rarity: "Ультра",      price: 40,  weight: 2 },
  { id: "vozduhan", name: "Воздухан",    rarity: "Легенда",     price: 26,  weight: 5 },
  { id: "kautoy",   name: "Типо Каутой", rarity: "Мега",        price: 13,  weight: 8 },
  { id: "floatup",  name: "floatup",     rarity: "Аркана",      price: 10,  weight: 12 },
  { id: "musor",    name: "MUSOR",       rarity: "Мифик",       price: 6,   weight: 15 },
  { id: "67",       name: "67",          rarity: "Эпик",        price: 3,   weight: 17 },
  { id: "mango",    name: "МАНGO",       rarity: "Сверхредкий", price: 1.5, weight: 19 },
  { id: "rozetka",  name: "Розетка",     rarity: "Редкий",      price: 1,   weight: 21.5 },
];

const RARITY_COLORS = {
  "Секретный":   "#ff2d2d",
  "Ультра":      "#ff8c00",
  "Легенда":     "#ffd700",
  "Мега":        "#c04dff",
  "Аркана":      "#ff4d94",
  "Мифик":       "#4d9fff",
  "Эпик":        "#7b4dff",
  "Сверхредкий": "#4dff88",
  "Редкий":      "#9e9e9e",
};

function shadeColor(hex, amt) {
  const c = hex.replace("#", "");
  const r = parseInt(c.substring(0,2), 16);
  const g = parseInt(c.substring(2,4), 16);
  const b = parseInt(c.substring(4,6), 16);
  const mix = (v) => {
    const target = amt < 0 ? 0 : 255;
    const k = Math.abs(amt);
    return Math.round(v * (1 - k) + target * k);
  };
  return `rgb(${mix(r)},${mix(g)},${mix(b)})`;
}

const RARITY_BG_CACHE = {};
for (const key in RARITY_COLORS) {
  RARITY_BG_CACHE[key] = shadeColor(RARITY_COLORS[key], -0.75);
}

const SHOP_PACKS = [
  [10,  20],
  [20,  30],
  [30,  50],
  [100, 180],
  [250, 400],
];

const PROMOS = {
  "#mrtPromBalanc30": { type: "lightning", amount: 30, max: 1, label: "30 молний" },
  "#mrtPromDollar20": { type: "dollars",   amount: 20, max: 1, label: "20 долларов" },
  "#mrtPromBalanc2":  { type: "lightning", amount: 2,  max: 2, label: "2 молнии" },
};

const SAVE_KEY = "nekitsave_v4";
const TG_URL   = "https://t.me/ymarat123tube";
const TTK_URL  = "https://www.tiktok.com/@k0tenok500";

let state = {
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
};

let saveTimeout = null;
function save() {
  if (saveTimeout) clearTimeout(saveTimeout);
  saveTimeout = setTimeout(() => {
    try { localStorage.setItem(SAVE_KEY, JSON.stringify(state)); } catch(e) {}
    saveTimeout = null;
  }, 200);
}
function saveNow() {
  if (saveTimeout) { clearTimeout(saveTimeout); saveTimeout = null; }
  try { localStorage.setItem(SAVE_KEY, JSON.stringify(state)); } catch(e) {}
}
function load() {
  try {
    const raw = localStorage.getItem(SAVE_KEY);
    if (raw) Object.assign(state, JSON.parse(raw));
    if (!state.promosUsed) state.promosUsed = {};
  } catch(e) {}
}

function rollCharacter() {
  const r = Math.random() * 100;
  let acc = 0;
  for (let i = 0; i < CHARACTERS.length; i++) {
    acc += CHARACTERS[i].weight;
    if (r <= acc) return CHARACTERS[i];
  }
  return CHARACTERS[CHARACTERS.length - 1];
}
function fmtDollars(n) { return "$ " + (Math.round(n * 10) / 10).toFixed(1); }
function timeUntilDaily() {
  const now = Math.floor(Date.now() / 1000);
  return Math.max(86400 - (now - state.lastDaily), 0);
}

const el = {
  landingScreen:  document.getElementById("landingScreen"),
  gameScreen:     document.getElementById("gameScreen"),
  playBtn:        document.getElementById("playBtn"),
  ctaHint:        document.getElementById("ctaHint"),
  releaseHint:    document.getElementById("releaseHint"),
  profileBtn:     document.getElementById("profileBtn"),
  promoBtn:       document.getElementById("promoBtn"),
  lightningLabel: document.getElementById("lightningLabel"),
  dollarsLabel:   document.getElementById("dollarsLabel"),
  plusLightning:  document.getElementById("plusLightning"),
  closeBtn:       document.getElementById("closeBtn"),
  spinBtn:        document.getElementById("spinBtn"),
  helpBtn:        document.getElementById("helpBtn"),
  bonusBtn:       document.getElementById("bonusBtn"),
  canvas:         document.getElementById("rouletteCanvas"),
};
const ctx = el.canvas.getContext("2d", { alpha: false });

function isReleased() {
  return Date.now() >= RELEASE_DATE.getTime();
}

function updateReleaseState() {
  const released = isReleased();
  el.playBtn.disabled = !released;
  el.ctaHint.classList.toggle("hidden", released);
  if (released) {
    el.releaseHint.textContent = "Игра доступна!";
  } else {
    const left = RELEASE_DATE.getTime() - Date.now();
    const days = Math.floor(left / 86400000);
    const hours = Math.floor((left % 86400000) / 3600000);
    el.releaseHint.textContent = `Осталось ${days} дн. ${hours} ч.`;
  }
}
updateReleaseState();
setInterval(updateReleaseState, 60000);

function buildChances() {
  const list = document.getElementById("chancesList");
  const frag = document.createDocumentFragment();
  CHARACTERS.forEach(c => {
    const col = RARITY_COLORS[c.rarity] || "#666";
    const row = document.createElement("div");
    row.className = "chance-row";
    row.innerHTML = `
      <div class="chance-icon" style="background:${RARITY_BG_CACHE[c.rarity] || "#222"}; color:${col}">
        ${c.name.slice(0,2)}
      </div>
      <div class="chance-name">${c.name}</div>
      <div class="chance-rarity" style="color:${col}">${c.rarity}</div>
      <div class="chance-percent" style="color:${col}">${c.weight}%</div>
    `;
    frag.appendChild(row);
  });
  list.appendChild(frag);
}
buildChances();

const ITEM_W = 120;
const ITEM_H = 160;
const ITEM_GAP = 8;
const STRIDE = ITEM_W + ITEM_GAP;
const POOL = 24;

let canvasW = 0, canvasH = 0;
let dpr = 1;

const poolData = [];
let stripOffset = 0;
let spinning = false;
let spinStartTime = 0;
let spinStartOffset = 0;
let spinTargetOffset = 0;
const SPIN_DURATION = 4000;
let pendingPrize = null;

const FONT_ICON = "800 24px -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif";
const FONT_NAME = "800 13px -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif";
const FONT_RARITY = "800 10px -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif";

function resizeCanvas() {
  const rect = el.canvas.parentElement.getBoundingClientRect();
  canvasW = rect.width;
  canvasH = rect.height;
  dpr = Math.min(window.devicePixelRatio || 1, 2);
  el.canvas.width = Math.floor(canvasW * dpr);
  el.canvas.height = Math.floor(canvasH * dpr);
  el.canvas.style.width = canvasW + "px";
  el.canvas.style.height = canvasH + "px";
  ctx.setTransform(dpr, 0, 0, dpr, 0, 0);
}
window.addEventListener("resize", resizeCanvas, { passive: true });

function buildPool() {
  poolData.length = 0;
  for (let i = 0; i < POOL; i++) {
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
  const col = RARITY_COLORS[data.rarity] || "#666";
  const bg = RARITY_BG_CACHE[data.rarity] || "#222";

  roundRect(ctx, x, y, ITEM_W, ITEM_H, 14);
  ctx.fillStyle = bg;
  ctx.fill();
  ctx.strokeStyle = col + "44";
  ctx.lineWidth = 1.5;
  ctx.stroke();

  const iconCX = x + ITEM_W / 2;
  const iconCY = y + 12 + 36;
  ctx.beginPath();
  ctx.arc(iconCX, iconCY, 36, 0, Math.PI * 2);
  ctx.fillStyle = "rgba(0,0,0,0.4)";
  ctx.fill();
  ctx.strokeStyle = col + "55";
  ctx.lineWidth = 1;
  ctx.stroke();

  ctx.fillStyle = col;
  ctx.font = FONT_ICON;
  ctx.textAlign = "center";
  ctx.textBaseline = "middle";
  ctx.fillText(data.name.slice(0, 2), iconCX, iconCY);

  ctx.fillStyle = "#ffffff";
  ctx.font = FONT_NAME;
  ctx.textAlign = "center";
  ctx.textBaseline = "alphabetic";
  ctx.fillText(data.name, iconCX, y + ITEM_H - 38);

  ctx.fillStyle = col;
  ctx.font = FONT_RARITY;
  ctx.fillText(data.rarity.toUpperCase(), iconCX, y + ITEM_H - 18);
}

function drawRoulette() {
  ctx.fillStyle = "#0a0c12";
  ctx.fillRect(0, 0, canvasW, canvasH);

  const centerY = canvasH / 2;
  const cardY = centerY - ITEM_H / 2;
  const total = POOL * STRIDE;
  const pointerX = canvasW / 2;

  const visibleLeft = -ITEM_W - 50;
  const visibleRight = canvasW + 50;

  for (let i = 0; i < POOL; i++) {
    let x = (stripOffset + i * STRIDE) % total;
    if (x < 0) x += total;
    const drawX = x - ITEM_W;
    if (drawX > visibleRight || drawX + ITEM_W < visibleLeft) continue;
    drawCard(drawX, cardY, poolData[i].data);
  }

  ctx.fillStyle = "rgba(10,12,18,0.9)";
  ctx.fillRect(0, 0, 80, canvasH);
  ctx.fillStyle = "rgba(10,12,18,0.5)";
  ctx.fillRect(80, 0, 40, canvasH);
  ctx.fillStyle = "rgba(10,12,18,0.9)";
  ctx.fillRect(canvasW - 80, 0, 80, canvasH);
  ctx.fillStyle = "rgba(10,12,18,0.5)";
  ctx.fillRect(canvasW - 120, 0, 40, canvasH);

  ctx.save();
  const grad = ctx.createLinearGradient(pointerX, cardY - 14, pointerX, cardY + ITEM_H + 14);
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

let lastFrameTime = 0;
let rafId = null;
let needsRedraw = true;

function gameLoop(now) {
  if (!lastFrameTime) lastFrameTime = now;
  lastFrameTime = now;

  if (spinning) {
    const t = Math.min((now - spinStartTime) / SPIN_DURATION, 1);
    const ease = 1 - Math.pow(1 - t, 3);
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
    showPopup("Мало молний", "Крутка стоит 1 ⚡.\nЗайди в «Бонусы» или «?».",
      [{text:"OK"}]);
    return;
  }
  state.lightning -= 1;
  updateCurrency();
  save();

  spinning = true;
  needsRedraw = true;
  el.spinBtn.disabled = true;

  const prize = rollCharacter();
  pendingPrize = prize;

  spinStartOffset = stripOffset;
  spinStartTime = performance.now();

  const turns = 4 + Math.floor(Math.random() * 3);
  const totalPx = turns * POOL * STRIDE;
  spinTargetOffset = spinStartOffset + totalPx + Math.random() * STRIDE;

  const pointerX = canvasW / 2;
  const total = POOL * STRIDE;
  let bestI = 0, bestDist = Infinity;
  for (let i = 0; i < POOL; i++) {
    let x = (spinTargetOffset + i * STRIDE) % total;
    if (x < 0) x += total;
    const cardCenter = x - ITEM_W / 2;
    const dist = Math.abs(cardCenter - pointerX);
    if (dist < bestDist) { bestDist = dist; bestI = i; }
  }
  poolData[bestI].data = prize;
}

function finishSpin() {
  spinning = false;
  el.spinBtn.disabled = false;
  const prize = pendingPrize;
  pendingPrize = null;
  state.spins += 1;
  state.inventory.push({ id: prize.id, name: prize.name, rarity: prize.rarity, price: prize.price });
  save();
  updateCurrency();

  showPopup(
    `Выпал: ${prize.name}`,
    `Редкость: ${prize.rarity}\nЦена: $${prize.price}`,
    [{ text: "OK" }]
  );
  needsRedraw = true;
}

function updateCurrency() {
  el.lightningLabel.textContent = "⚡ " + state.lightning;
  el.dollarsLabel.textContent = fmtDollars(state.dollars);
  el.profileBtn.textContent = state.nickname;
}

function showPopup(title, body, buttons) {
  const overlay = document.createElement("div");
  overlay.className = "popup-overlay";
  const popup = document.createElement("div");
  popup.className = "popup";

  let buttonsHtml = "";
  if (buttons && buttons.length) {
    buttonsHtml = '<div class="btn-row">';
    for (let i = 0; i < buttons.length; i++) {
      const b = buttons[i];
      const cls = b.secondary ? "secondary" : "";
      buttonsHtml += `<button class="${cls}" data-i="${i}">${b.text}</button>`;
    }
    buttonsHtml += "</div>";
  }

  popup.innerHTML = `<h2>${title}</h2><p>${body}</p>${buttonsHtml}`;
  overlay.appendChild(popup);
  document.body.appendChild(overlay);

  const close = () => overlay.remove();
  popup.querySelectorAll("button").forEach(btn => {
    btn.addEventListener("click", () => {
      const i = parseInt(btn.dataset.i, 10);
      const action = buttons[i].action;
      close();
      if (typeof action === "function") action();
    });
  });
  overlay.addEventListener("click", (e) => { if (e.target === overlay) close(); });
}

function openShop() {
  const overlay = document.createElement("div");
  overlay.className = "popup-overlay";
  const popup = document.createElement("div");
  popup.className = "popup";

  let packsHtml = "";
  SHOP_PACKS.forEach(([lightning, price], i) => {
    const canAfford = state.dollars >= price;
    packsHtml += `
      <div class="shop-pack">
        <div class="sp-left">⚡ ${lightning}</div>
        <div class="sp-right">$ ${price}</div>
        <button data-i="${i}" ${canAfford ? "" : "disabled"}>Купить</button>
      </div>
    `;
  });

  popup.innerHTML = `
    <h2>Купить молнии</h2>
    <p style="margin-bottom:14px;">Обмен $ на ⚡</p>
    <div id="shopList">${packsHtml}</div>
    <div class="btn-row"><button class="secondary" id="shopClose">Закрыть</button></div>
  `;
  overlay.appendChild(popup);
  document.body.appendChild(overlay);

  popup.querySelectorAll("#shopList button").forEach(btn => {
    btn.addEventListener("click", () => {
      const i = parseInt(btn.dataset.i, 10);
      const [lightning, price] = SHOP_PACKS[i];
      if (state.dollars < price) return;
      state.dollars -= price;
      state.lightning += lightning;
      save();
      updateCurrency();
      overlay.remove();
      openShop();
    });
  });
  popup.querySelector("#shopClose").addEventListener("click", () => overlay.remove());
  overlay.addEventListener("click", (e) => { if (e.target === overlay) overlay.remove(); });
}

function openPromo() {
  const overlay = document.createElement("div");
  overlay.className = "popup-overlay";
  const popup = document.createElement("div");
  popup.className = "popup";
  popup.innerHTML = `
    <h2>Активировать промокод</h2>
    <p>Введи код и получи награду</p>
    <input id="promoInput" placeholder="#ВВЕДИ_КОД" maxlength="40" autocomplete="off">
    <div class="btn-row">
      <button class="secondary" id="promoCancel">Отмена</button>
      <button id="promoApply">Активировать</button>
    </div>
  `;
  overlay.appendChild(popup);
  document.body.appendChild(overlay);

  const input = popup.querySelector("#promoInput");
  setTimeout(() => input.focus(), 50);

  const apply = () => {
    const code = input.value.trim();
    if (!code) return;
    const promo = PROMOS[code];
    if (!promo) {
      showPopup("Неверный код", "Такого промокода не существует.", [{text:"OK"}]);
      return;
    }
    const used = state.promosUsed[code] || 0;
    if (used >= promo.max) {
      showPopup("Лимит исчерпан", `Промокод "${code}" уже использован ${used} раз(а).`, [{text:"OK"}]);
      return;
    }
    state.promosUsed[code] = used + 1;
    if (promo.type === "lightning") {
      state.lightning += promo.amount;
    } else if (promo.type === "dollars") {
      state.dollars += promo.amount;
    }
    save();
    updateCurrency();
    overlay.remove();
    showPopup("Промокод активирован!", `Ты получил: ${promo.label}`, [{text:"Круто!"}]);
  };

  popup.querySelector("#promoApply").addEventListener("click", apply);
  popup.querySelector("#promoCancel").addEventListener("click", () => overlay.remove());
  input.addEventListener("keydown", (e) => { if (e.key === "Enter") apply(); });
  overlay.addEventListener("click", (e) => { if (e.target === overlay) overlay.remove(); });
}

function openProfile() {
  const overlay = document.createElement("div");
  overlay.className = "popup-overlay";
  const popup = document.createElement("div");
  popup.className = "popup";
  popup.innerHTML = `
    <h2>Профиль</h2>
    <div class="profile-grid">
      <div class="avatar-box" id="ppAvatar">${state.avatar ? `<img src="${state.avatar}">` : "📷"}</div>
      <input id="ppNick" value="${state.nickname.replace(/"/g, "&quot;")}" maxlength="20" style="margin:0;">
    </div>
    <div class="profile-stats">
      <div>⚡<b>${state.lightning}</b></div>
      <div>$<b>${fmtDollars(state.dollars).replace("$ ", "")}</b></div>
      <div>Круток<b>${state.spins}</b></div>
    </div>
    <div class="inv-title">Инвентарь (${state.inventory.length})</div>
    <div class="inv-list" id="ppInv"></div>
    <div class="btn-row"><button id="ppClose">Закрыть</button></div>
  `;
  overlay.appendChild(popup);
  document.body.appendChild(overlay);

  const invList = popup.querySelector("#ppInv");
  if (state.inventory.length === 0) {
    invList.innerHTML = '<div class="empty-inv">Пока пусто. Крути рулетку!</div>';
  } else {
    const frag = document.createDocumentFragment();
    state.inventory.forEach((item, idx) => {
      const card = document.createElement("div");
      card.className = "inv-card";
      const col = RARITY_COLORS[item.rarity] || "#888";
      card.innerHTML = `
        <div class="ic-name">${item.name}</div>
        <div class="ic-rarity" style="color:${col}">${item.rarity}</div>
        <button data-i="${idx}">Продать $${item.price}</button>
      `;
      frag.appendChild(card);
    });
    invList.appendChild(frag);
    invList.querySelectorAll("button").forEach(btn => {
      btn.addEventListener("click", () => {
        const i = parseInt(btn.dataset.i, 10);
        const item = state.inventory[i];
        state.dollars += item.price;
        state.inventory.splice(i, 1);
        save();
        updateCurrency();
        overlay.remove();
        openProfile();
      });
    });
  }

  const avatarBox = popup.querySelector("#ppAvatar");
  const fileInput = document.createElement("input");
  fileInput.type = "file";
  fileInput.accept = "image/*";
  fileInput.style.display = "none";
  popup.appendChild(fileInput);
  avatarBox.addEventListener("click", () => fileInput.click());
  fileInput.addEventListener("change", () => {
    const file = fileInput.files[0];
    if (!file) return;
    const reader = new FileReader();
    reader.onload = (e) => {
      state.avatar = e.target.result;
      save();
      avatarBox.innerHTML = `<img src="${state.avatar}">`;
    };
    reader.readAsDataURL(file);
  });

  const nickInput = popup.querySelector("#ppNick");
  nickInput.addEventListener("change", () => {
    const t = nickInput.value.trim();
    if (t) { state.nickname = t; save(); updateCurrency(); }
  });

  popup.querySelector("#ppClose").addEventListener("click", () => overlay.remove());
  overlay.addEventListener("click", (e) => { if (e.target === overlay) overlay.remove(); });
}

function openBonus() {
  const dailyReady = timeUntilDaily() === 0;
  const dailyLeft = timeUntilDaily();
  const h = Math.floor(dailyLeft / 3600);
  const m = Math.floor((dailyLeft % 3600) / 60);

  const overlay = document.createElement("div");
  overlay.className = "popup-overlay";
  const popup = document.createElement("div");
  popup.className = "popup";
  popup.innerHTML = `
    <h2>Бонусы</h2>
    <div style="display:flex;flex-direction:column;gap:10px;margin:14px 0;">
      <button class="secondary" id="bnTg" ${state.claimedTg ? "disabled" : ""}>
        ${state.claimedTg ? "✓ Telegram получен" : "+3 ⚡ за подписку на Telegram"}
      </button>
      <button class="secondary" id="bnTtk" ${state.claimedTt ? "disabled" : ""}>
        ${state.claimedTt ? "✓ ТТК получен" : "+3 ⚡ за подписку на ТТК друга"}
      </button>
      <button id="bnDaily" ${dailyReady ? "" : "disabled"}>
        ${dailyReady ? "+2 ⚡ Ежедневный бонус" : `⏳ Через ${String(h).padStart(2,"0")}:${String(m).padStart(2,"0")}`}
      </button>
    </div>
    <div class="btn-row"><button class="secondary" id="bnClose">Закрыть</button></div>
  `;
  overlay.appendChild(popup);
  document.body.appendChild(overlay);

  popup.querySelector("#bnTg").addEventListener("click", () => {
    if (state.claimedTg) return;
    window.open(TG_URL, "_blank");
    state.claimedTg = true;
    state.lightning += 3;
    save();
    updateCurrency();
    overlay.remove();
    openBonus();
  });
  popup.querySelector("#bnTtk").addEventListener("click", () => {
    if (state.claimedTt) return;
    window.open(TTK_URL, "_blank");
    state.claimedTt = true;
    state.lightning += 3;
    save();
    updateCurrency();
    overlay.remove();
    openBonus();
  });
  popup.querySelector("#bnDaily").addEventListener("click", () => {
    if (timeUntilDaily() > 0) return;
    state.lastDaily = Math.floor(Date.now() / 1000);
    state.lightning += 2;
    save();
    updateCurrency();
    overlay.remove();
    openBonus();
  });
  popup.querySelector("#bnClose").addEventListener("click", () => overlay.remove());
  overlay.addEventListener("click", (e) => { if (e.target === overlay) overlay.remove(); });
}

let gameStarted = false;

function enterGame() {
  if (!isReleased()) {
    showPopup("Ещё не вышло!", "Релиз 26.09.2026. Жди!", [{text:"OK"}]);
    return;
  }
  el.landingScreen.classList.add("hidden");
  el.gameScreen.classList.add("active");

  setTimeout(() => {
    resizeCanvas();
    if (!gameStarted) {
      buildPool();
      gameStarted = true;
      if (!rafId) rafId = requestAnimationFrame(gameLoop);
    } else {
      needsRedraw = true;
      if (!rafId) rafId = requestAnimationFrame(gameLoop);
    }
  }, 30);
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
el.helpBtn.addEventListener("click", () => {
  window.scrollTo({ top: 0, behavior: "smooth" });
  if (el.gameScreen.classList.contains("active")) {
    exitGame();
  }
});
el.closeBtn.addEventListener("click", () => {
  saveNow();
  if (confirm("Выйти в главное меню? Прогресс сохранён.")) {
    exitGame();
  }
});

window.addEventListener("beforeunload", saveNow, { passive: true });
document.addEventListener("visibilitychange", () => {
  if (document.hidden) saveNow();
}, { passive: true });

load();
updateCurrency();
</script>
</body>
</html>
