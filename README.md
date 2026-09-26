<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NekitDrop 2.0</title>
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
  .badge.new { background: rgba(255,77,148,0.12); color: #ff4d94; border: 1px solid rgba(255,77,148,0.3); }
  .footer-copy { text-align: center; color: #6a7080; font-size: 12px; padding-bottom: 30px; }

  #gameScreen { display: none; flex-direction: column; height: 100vh; max-width: 900px; margin: 0 auto; }
  #gameScreen.active { display: flex; }
  #landingScreen.hidden { display: none; }

  .topbar { display: flex; align-items: center; padding: 8px 10px; background: rgba(26,29,40,0.92); border-bottom: 1px solid #262a38; gap: 6px; flex-wrap: wrap; }
  .tb-btn { background: #262a38; border: 1px solid #353a4d; color: #e8e8f0; padding: 7px 12px; border-radius: 10px; cursor: pointer; font-size: 12px; font-weight: 600; font-family: inherit; position: relative; }
  .tb-btn:hover { background: #2f3444; }
  .tb-spacer { flex: 1; }
  .tb-badge {
    position: absolute; top: -6px; right: -6px;
    background: #ff2d2d; color: #fff;
    font-size: 10px; font-weight: 800;
    min-width: 18px; height: 18px;
    border-radius: 9px;
    display: flex; align-items: center; justify-content: center;
    padding: 0 5px;
    box-shadow: 0 2px 8px rgba(255,45,45,0.6);
  }
  .curr-group { display: flex; align-items: center; background: #262a38; border: 1px solid #353a4d; border-radius: 10px; overflow: hidden; font-size: 12px; font-weight: 700; height: 32px; }
  .curr-group .curr-item { padding: 0 10px; white-space: nowrap; }
  .curr-group.lightning .curr-item { color: #ffd93b; }
  .curr-group.dollars .curr-item { color: #4dff88; }
  .plus-btn { background: rgba(255,217,59,0.15); border: none; border-left: 1px solid #353a4d; color: #ffd93b; font-size: 15px; font-weight: 800; padding: 0 10px; height: 100%; cursor: pointer; font-family: inherit; }
  .close-btn { background: #3a1a1a; border: 1px solid #5a2020; color: #ff8080; width: 32px; height: 32px; border-radius: 10px; cursor: pointer; font-size: 13px; font-weight: 700; font-family: inherit; }

  .roulette-wrap { flex: 1; position: relative; overflow: hidden; background: radial-gradient(ellipse at center, #1a1d28 0%, #0a0c12 85%); min-height: 0; }
  #rouletteCanvas { display: block; width: 100%; height: 100%; }

  .bottom { padding: 12px; display: flex; gap: 8px; align-items: center; background: rgba(26,29,40,0.92); border-top: 1px solid #262a38; }
  .help-btn { width: 44px; height: 44px; border-radius: 12px; background: #262a38; border: 1px solid #353a4d; color: #e8e8f0; font-size: 18px; font-weight: 700; cursor: pointer; font-family: inherit; }
  .bonus-btn { height: 44px; padding: 0 14px; border-radius: 12px; background: #262a38; border: 1px solid #353a4d; color: #e8e8f0; font-size: 12px; font-weight: 600; cursor: pointer; font-family: inherit; }
  .spin-btn { flex: 1; height: 48px; border-radius: 12px; background: linear-gradient(135deg, #ffd93b 0%, #ff8c00 100%); border: none; color: #1a1d28; font-size: 15px; font-weight: 900; cursor: pointer; font-family: inherit; box-shadow: 0 4px 20px rgba(255,140,0,0.4); }
  .spin-btn:disabled { opacity: 0.5; cursor: not-allowed; }

  .popup-overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.75); display: flex; align-items: center; justify-content: center; z-index: 100; padding: 20px; }
  .popup { background: #1e222e; border: 1px solid #353a4d; border-radius: 18px; padding: 24px; max-width: 460px; width: 100%; max-height: 85vh; overflow-y: auto; }
  .popup h2 { font-size: 20px; margin-bottom: 14px; text-align: center; color: #fff; font-weight: 800; }
  .popup p { font-size: 14px; color: #b8bcc8; text-align: center; margin-bottom: 18px; line-height: 1.6; white-space: pre-line; }
  .popup .btn-row { display: flex; gap: 10px; margin-top: 14px; }
  .popup button { flex: 1; padding: 12px; border-radius: 10px; border: none; background: #ffd93b; color: #1a1d28; font-size: 14px; font-weight: 800; cursor: pointer; font-family: inherit; }
  .popup button.secondary { background: #262a38; color: #e8e8f0; border: 1px solid #353a4d; }
  .popup button:disabled { opacity: 0.45; cursor: not-allowed; }
  .popup input { width: 100%; background: #262a38; border: 1px solid #353a4d; border-radius: 10px; padding: 12px; color: #fff; font-size: 15px; font-weight: 600; outline: none; margin-bottom: 12px; font-family: inherit; text-align: center; letter-spacing: 1px; }
  .popup input:focus { border-color: #ffd93b; }

  .shop-pack { display: flex; align-items: center; justify-content: space-between; padding: 12px 14px; background: #262a38; border-radius: 10px; margin-bottom: 8px; font-size: 14px; gap: 10px; }
  .shop-pack .sp-left { color: #ffd93b; font-weight: 800; }
  .shop-pack .sp-right { color: #4dff88; font-weight: 800; }
  .shop-pack button { background: #ffd93b; color: #1a1d28; border: none; padding: 7px 12px; border-radius: 8px; font-size: 12px; font-weight: 800; cursor: pointer; font-family: inherit; }
  .shop-pack button:disabled { opacity: 0.4; cursor: not-allowed; }

  .profile-grid { display: grid; grid-template-columns: 96px 1fr; gap: 14px; margin-bottom: 14px; align-items: center; }
  .avatar-box { width: 96px; height: 96px; border-radius: 16px; background: #262a38; border: 2px dashed #4a5068; display: flex; align-items: center; justify-content: center; cursor: pointer; font-size: 36px; color: #4a5068; overflow: hidden; }
  .avatar-box img { width: 100%; height: 100%; object-fit: cover; }
  .profile-stats { background: #262a38; border-radius: 10px; padding: 12px; font-size: 13px; color: #b8bcc8; margin-bottom: 14px; display: flex; justify-content: space-around; text-align: center; }
  .profile-stats b { color: #fff; font-size: 15px; display: block; margin-top: 4px; }
  .inv-title { font-size: 12px; color: #8a90a0; text-transform: uppercase; letter-spacing: 1.2px; margin: 10px 0 8px; font-weight: 700; display: flex; justify-content: space-between; align-items: center; }
  .inv-list { max-height: 240px; overflow-y: auto; display: flex; flex-direction: column; gap: 6px; margin-bottom: 10px; }
  .inv-card { display: flex; align-items: center; justify-content: space-between; padding: 9px 11px; background: #262a38; border-radius: 10px; font-size: 13px; gap: 8px; }
  .inv-card .ic-name { font-weight: 800; color: #fff; flex: 1; }
  .inv-card .ic-rarity { font-size: 11px; font-weight: 700; }
  .inv-card button { background: #4dff88; border: none; color: #12141c; padding: 6px 10px; border-radius: 8px; font-size: 12px; font-weight: 800; cursor: pointer; font-family: inherit; }
  .empty-inv { text-align: center; color: #5a6070; font-size: 13px; padding: 24px; }

  .sell-all-btn {
    width: 100%; padding: 12px; background: #4dff88; color: #12141c;
    border: none; border-radius: 10px; font-size: 14px; font-weight: 800;
    cursor: pointer; font-family: inherit; margin-bottom: 8px;
  }
  .sell-all-btn:disabled { opacity: 0.4; cursor: not-allowed; }

  .ach-card, .quest-card {
    background: #262a38; border-radius: 12px; padding: 14px;
    margin-bottom: 10px; display: flex; gap: 12px; align-items: center;
  }
  .ach-card.locked, .quest-card.locked { opacity: 0.55; }
  .ach-card .ach-icon, .quest-card .quest-icon {
    width: 44px; height: 44px; border-radius: 50%;
    background: rgba(0,0,0,0.35);
    display: flex; align-items: center; justify-content: center;
    font-size: 20px; flex-shrink: 0;
  }
  .ach-card .ach-body, .quest-card .quest-body { flex: 1; }
  .ach-card .ach-name, .quest-card .quest-name { font-weight: 800; color: #fff; font-size: 14px; margin-bottom: 3px; }
  .ach-card .ach-desc, .quest-card .quest-desc { font-size: 12px; color: #8a90a0; line-height: 1.4; }
  .ach-card .ach-reward, .quest-card .quest-reward { font-size: 12px; font-weight: 800; color: #ffd93b; margin-top: 4px; }
  .ach-card .ach-btn, .quest-card .quest-btn {
    background: #ffd93b; color: #1a1d28; border: none;
    padding: 8px 12px; border-radius: 8px;
    font-size: 12px; font-weight: 800; cursor: pointer; font-family: inherit;
    flex-shrink: 0;
  }
  .ach-card .ach-btn:disabled, .quest-card .quest-btn:disabled {
    background: #3a3f52; color: #6a7080; cursor: not-allowed;
  }
  .ach-card .ach-btn.done, .quest-card .quest-btn.done {
    background: #4dff88; color: #12141c;
  }

  .welcome-overlay { position: fixed; inset: 0; background: radial-gradient(circle at center, rgba(255,140,0,0.25) 0%, rgba(0,0,0,0.92) 70%); display: flex; align-items: center; justify-content: center; z-index: 200; padding: 20px; }
  .welcome-card { background: linear-gradient(160deg, #1e222e 0%, #16181f 100%); border: 2px solid rgba(255,217,59,0.5); border-radius: 24px; padding: 36px 28px; max-width: 440px; width: 100%; text-align: center; box-shadow: 0 0 60px rgba(255,140,0,0.5); max-height: 90vh; overflow-y: auto; }
  .welcome-emoji { font-size: 64px; margin-bottom: 14px; display: inline-block; }
  .welcome-title { font-size: 26px; font-weight: 900; margin-bottom: 10px; background: linear-gradient(135deg, #ffd93b 0%, #ff8c00 50%, #ff4d94 100%); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
  .welcome-text { font-size: 14px; color: #b8bcc8; margin-bottom: 20px; line-height: 1.6; }
  .welcome-bonus { display: inline-block; background: linear-gradient(135deg, rgba(255,217,59,0.25) 0%, rgba(255,140,0,0.15) 100%); border: 2px solid #ffd93b; border-radius: 14px; padding: 12px 22px; font-size: 20px; font-weight: 900; color: #ffd93b; margin-bottom: 20px; }
  .welcome-btn { background: linear-gradient(135deg, #ffd93b 0%, #ff8c00 100%); color: #1a1d28; border: none; padding: 14px 36px; border-radius: 14px; font-size: 15px; font-weight: 900; cursor: pointer; font-family: inherit; }

  .whatsnew-card { background: linear-gradient(160deg, #1e222e 0%, #16181f 100%); border: 2px solid rgba(255,77,148,0.5); border-radius: 24px; padding: 32px 26px; max-width: 480px; width: 100%; text-align: left; box-shadow: 0 0 60px rgba(255,77,148,0.4); max-height: 90vh; overflow-y: auto; }
  .whatsnew-title { font-size: 26px; font-weight: 900; margin-bottom: 6px; text-align: center; background: linear-gradient(135deg, #ff4d94 0%, #ff8c00 100%); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
  .whatsnew-sub { font-size: 13px; color: #8a90a0; margin-bottom: 20px; text-align: center; }
  .whatsnew-list { display: flex; flex-direction: column; gap: 10px; margin-bottom: 20px; }
  .whatsnew-item { display: flex; gap: 12px; padding: 12px; background: rgba(38,42,56,0.7); border-radius: 12px; }
  .whatsnew-item .wn-icon { font-size: 22px; flex-shrink: 0; }
  .whatsnew-item .wn-text { font-size: 13px; color: #e8e8f0; line-height: 1.5; }
  .whatsnew-item .wn-text b { color: #ffd93b; }
  .whatsnew-btn { width: 100%; background: linear-gradient(135deg, #ff4d94 0%, #ff8c00 100%); color: #fff; border: none; padding: 14px; border-radius: 14px; font-size: 15px; font-weight: 900; cursor: pointer; font-family: inherit; }
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
    <div class="release-label">Обновление 2.0</div>
    <div class="release-date">29.09.2026</div>
    <div class="release-hint" id="releaseHint">Уже доступно!</div>
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
    <div class="badge new">Обновление 2.0</div>
    <div class="badge deepseek">Сделано с помощью DeepSeek</div>
    <div class="badge fun">Создано в развлекательных целях</div>
  </div>
  <div class="footer-copy">© 2026 NekitDrop. Все совпадения случайны.</div>
</div>

<div id="gameScreen">
  <div class="topbar">
    <button class="tb-btn" id="profileBtn">Профиль</button>
    <button class="tb-btn" id="promoBtn">ПРОМО</button>
    <button class="tb-btn" id="achievementsBtn">Достижения<span class="tb-badge" id="achBadge" style="display:none;">0</span></button>
    <button class="tb-btn" id="questsBtn">Квесты<span class="tb-badge" id="questBadge" style="display:none;">0</span></button>
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

// ==================== ДАННЫЕ ====================
var CHARACTERS = [
  { id: "nekit",     name: "Nekit",       rarity: "Секретный",   price: 67,  weight: 0.5 },
  { id: "sygak",     name: "Sygak",       rarity: "Ультра",      price: 40,  weight: 1.5 },
  { id: "mellstroy", name: "Mellstroy",   rarity: "Ультра",      price: 40,  weight: 1.5 },
  { id: "vozduhan",  name: "Воздухан",    rarity: "Легенда",     price: 26,  weight: 3 },
  { id: "dunduk",    name: "ДунДук",      rarity: "Легенда",     price: 26,  weight: 3 },
  { id: "kautoy",    name: "Типо Каутой", rarity: "Мега",        price: 13,  weight: 8 },
  { id: "floatup",   name: "floatup",     rarity: "Аркана",      price: 10,  weight: 11 },
  { id: "musor",     name: "MUSOR",       rarity: "Мифик",       price: 6,   weight: 14 },
  { id: "67",        name: "67",          rarity: "Эпик",        price: 3,   weight: 16 },
  { id: "mango",     name: "МАНGO",       rarity: "Сверхредкий", price: 1.5, weight: 19 },
  { id: "rozetka",   name: "Розетка",     rarity: "Редкий",      price: 1,   weight: 22.5 }
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

var SHOP_TIERS = [
  { lightning: 10,  price: 18 },
  { lightning: 25,  price: 40 },
  { lightning: 50,  price: 75 },
  { lightning: 100, price: 140 },
  { lightning: 250, price: 325 },
  { lightning: 500, price: 600 }
];

var PROMOS = {
  "#mrtPromBalanc30":  { type: "lightning", amount: 30,  max: 1, label: "30 молний" },
  "#mrtPromDollar20":  { type: "dollars",   amount: 20,  max: 1, label: "20 долларов" },
  "#mrtPromBalanc2":   { type: "lightning", amount: 2,   max: 2, label: "2 молнии" },
  "#mrtPromBalanc10":  { type: "lightning", amount: 10,  max: 2, label: "10 молний" },
  "#mrtPromBalanc15":  { type: "lightning", amount: 15,  max: 2, label: "15 молний" },
  "#mrtPromBalanc100": { type: "lightning", amount: 100, max: 1, label: "100 молний" },
  "#mrtPromDollar10":  { type: "dollars",   amount: 10,  max: 2, label: "10 долларов" },
  "#mrtPromDollar50":  { type: "dollars",   amount: 50,  max: 1, label: "50 долларов" },
  "#stsProm8t":        { type: "none",      amount: 0,   max: 999999, label: "ничего (для квеста)", questOnly: true }
};

var ACHIEVEMENTS = [
  { id: "finger",  name: "Чёткий палец!", icon: "👆", desc: "Прокрутить рулетку 1000 раз",           reward: { type: "dollars", amount: 5 } },
  { id: "legend",  name: "Легенда (ты)",  icon: "👑", desc: "Выбить всех 11 персонажей хотя бы раз", reward: { type: "lightning", amount: 20 } },
  { id: "kak",     name: "КАК",           icon: "🤯", desc: "Выбить Ультра-персонажа 2 раза подряд", reward: { type: "dollars", amount: 50 } },
  { id: "hamam",   name: "Хамам",         icon: "🔥", desc: "Выбить Mellstroy",                      reward: { type: "dollars", amount: 20 } }
];

var QUEST_TEMPLATES = [
  { id: 1, plus: true,  min: 50,  max: 140, base: 100, descTemplate: "Прокрутить дроп {n} раз",               reward: { type: "dollars", amount: 40 },            type: "spins" },
  { id: 2, plus: true,  min: 30,  max: 80,  base: 50,  descTemplate: "Купить/получить {n} молний",           reward: { type: "mixed", lightning: 20, dollars: 5 }, type: "lightning_gained" },
  { id: 3, plus: false, base: 100, descTemplate: "Получить {n}$",                                     reward: { type: "lightning", amount: 10 },          type: "dollars_earned" },
  { id: 4, plus: false, base: 1,   descTemplate: "Воспользоваться ежедневной наградой",               reward: { type: "mixed", dollars: 3, lightning: 5 },  type: "daily_used" },
  { id: 5, plus: false, base: 200, descTemplate: "Крутануть дроп {n} раз",                            reward: { type: "multiplier", mult: 2, duration: 60 }, type: "spins", multiplier: true },
  { id: 6, plus: false, base: 80,  descTemplate: "Крутануть дроп {n} раз",                            reward: { type: "multiplier", mult: 3, duration: 30 }, type: "spins", multiplier: true },
  { id: 7, plus: false, base: 1,   descTemplate: "Использовать все доступные бонусы (ТГ + ТТК + ежедневка)", reward: { type: "dollars", amount: 5 },       type: "all_bonuses" },
  { id: 8, plus: false, base: 1,   descTemplate: "Воспользоваться промокодом #stsProm8t (эксклюзивный, вводить только когда есть этот квест, ничего не даёт)", reward: { type: "dollars", amount: 30 }, type: "sts_used" }
];

var SAVE_KEY = "nekitsave_v7";
var TG_URL   = "https://t.me/ymarat123tube";
var TTK_URL  = "https://www.tiktok.com/@k0tenok500";
var WHATSNEW_KEY = "nekitdrop_whatsnew_2_0";
var MAX_PURCHASE_DOLLARS = 1000;

// ==================== СОСТОЯНИЕ ====================
var state = {
  nickname: "Профиль",
  avatar: "",
  lightning: 3,
  dollars: 0,
  spins: 0,
  lastDaily: 0,
  claimedTg: false,
  claimedTt: false,
  inventory: [],
  promosUsed: {},
  welcomed: false,
  seenChars: {},
  achievements: {},
  lastSpinRarity: "",
  twoUltrasInARow: false,
  mult2Until: 0,
  mult3Until: 0,
  quests: null,
  questsDate: "",
  questProgress: {},
  dailyQuestsDone: {},
  questsCompletedToday: [],
  stsUsedForQuest: false
};

function saveNow() {
  try { localStorage.setItem(SAVE_KEY, JSON.stringify(state)); } catch(e) {}
}
function load() {
  try {
    var raw = localStorage.getItem(SAVE_KEY);
    if (raw) {
      var d = JSON.parse(raw);
      for (var k in d) { state[k] = d[k]; }
    }
    if (!state.promosUsed) state.promosUsed = {};
    if (!state.seenChars) state.seenChars = {};
    if (!state.achievements) state.achievements = {};
    if (!state.questProgress) state.questProgress = {};
    if (!state.dailyQuestsDone) state.dailyQuestsDone = {};
    if (!state.questsCompletedToday) state.questsCompletedToday = [];
  } catch(e) {}
}

function todayStr() {
  var d = new Date();
  return d.getFullYear() + "-" + (d.getMonth()+1) + "-" + d.getDate();
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

function fmtDollars(n) {
  return "$ " + (Math.round(n * 10) / 10).toFixed(1);
}

function timeUntilDaily() {
  var now = Math.floor(Date.now() / 1000);
  return Math.max(86400 - (now - state.lastDaily), 0);
}

function getCurrentMultiplier() {
  var now = Date.now();
  var m = 1;
  if (state.mult3Until > now) m = 3;
  else if (state.mult2Until > now) m = 2;
  return m;
}

function getEffectivePrice(char) {
  var m = getCurrentMultiplier();
  return Math.round(char.price * m * 10) / 10;
}

// ==================== DOM ====================
var el = {
  landingScreen:  document.getElementById("landingScreen"),
  gameScreen:     document.getElementById("gameScreen"),
  playBtn:        document.getElementById("playBtn"),
  profileBtn:     document.getElementById("profileBtn"),
  promoBtn:       document.getElementById("promoBtn"),
  achievementsBtn:document.getElementById("achievementsBtn"),
  questsBtn:      document.getElementById("questsBtn"),
  achBadge:       document.getElementById("achBadge"),
  questBadge:     document.getElementById("questBadge"),
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

// ==================== ЛЕНДИНГ ====================
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

// ==================== CANVAS ====================
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
  state.seenChars[prize.id] = true;

  var mult = getCurrentMultiplier();
  var effectivePrice = Math.round(prize.price * mult * 10) / 10;
  state.inventory.push({ id: prize.id, name: prize.name, rarity: prize.rarity, price: effectivePrice });

  // Ачивка "КАК" — 2 ультра подряд
  if (prize.rarity === "Ультра" && state.lastSpinRarity === "Ультра") {
    state.twoUltrasInARow = true;
  }
  state.lastSpinRarity = prize.rarity;

  // Ачивка "Хамам" — Mellstroy
  if (prize.id === "mellstroy") {
    state.achievements["hamam"] = state.achievements["hamam"] || "unlocked";
  }

  // Квесты — прокруты
  updateQuestProgress("spins", 1);

  saveNow();
  updateCurrency();
  updateBadges();
  showPopup("Выпал: " + prize.name, "Редкость: " + prize.rarity + "\nЦена: $" + effectivePrice, [{ text: "OK" }]);
  needsRedraw = true;
}

function updateCurrency() {
  el.lightningLabel.textContent = "⚡ " + state.lightning;
  el.dollarsLabel.textContent = fmtDollars(state.dollars);
  el.profileBtn.textContent = state.nickname;
}

// ==================== POPUP ====================
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

// ==================== SHOP ====================
function openShop() {
  var overlay = document.createElement("div");
  overlay.className = "popup-overlay";
  var popup = document.createElement("div");
  popup.className = "popup";

  var html = '<h2>Купить молнии</h2>';
  html += '<p style="margin-bottom:12px;">Введи сколько молний нужно</p>';
  html += '<input id="shopInput" type="number" min="1" max="100000" placeholder="Например: 50" autocomplete="off">';
  html += '<div id="shopCalc" style="text-align:center;color:#8a90a0;font-size:13px;margin-bottom:10px;min-height:20px;">Введи количество</div>';
  html += '<div class="btn-row"><button class="secondary" id="shopClose">Отмена</button><button id="shopBuy" disabled>Купить</button></div>';
  html += '<div style="margin-top:14px;font-size:11px;color:#6a7080;text-align:center;line-height:1.6;">' +
    'Курс: ~$1.60 за ⚡, скидка на крупные пакеты.<br>Максимум за раз: $' + MAX_PURCHASE_DOLLARS + '</div>';

  popup.innerHTML = html;
  overlay.appendChild(popup);
  document.body.appendChild(overlay);

  var input = popup.querySelector("#shopInput");
  var calc = popup.querySelector("#shopCalc");
  var buyBtn = popup.querySelector("#shopBuy");

  function calcPrice(n) {
    if (n <= 0) return 0;
    var remaining = n;
    var total = 0;
    // Сначала крупные тиры — самая выгодная цена для игрока
    var tiers = SHOP_TIERS.slice().sort(function(a,b){ return b.lightning - a.lightning; });
    for (var i = 0; i < tiers.length; i++) {
      var t = tiers[i];
      while (remaining >= t.lightning) {
        total += t.price;
        remaining -= t.lightning;
      }
    }
    if (remaining > 0) {
      total += remaining * 1.8;
    }
    return Math.round(total * 100) / 100;
  }

  function updateCalc() {
    var n = parseInt(input.value, 10);
    if (!n || n <= 0) {
      calc.textContent = "Введи количество";
      buyBtn.disabled = true;
      return;
    }
    var price = calcPrice(n);
    if (price > MAX_PURCHASE_DOLLARS) {
      calc.innerHTML = '<span style="color:#ff8080;">Слишком большое кол-во введено (макс $' + MAX_PURCHASE_DOLLARS + ')</span>';
      buyBtn.disabled = true;
      return;
    }
    if (state.dollars < price) {
      calc.innerHTML = 'Цена: <b style="color:#4dff88;">$' + price.toFixed(2) + '</b> — <span style="color:#ff8080;">не хватает $' + (price - state.dollars).toFixed(2) + '</span>';
      buyBtn.disabled = true;
      return;
    }
    calc.innerHTML = 'Цена: <b style="color:#4dff88;">$' + price.toFixed(2) + '</b>';
    buyBtn.disabled = false;
  }

  input.addEventListener("input", updateCalc);
  setTimeout(function() { input.focus(); }, 50);

  buyBtn.addEventListener("click", function() {
    var n = parseInt(input.value, 10);
    if (!n || n <= 0) return;
    var price = calcPrice(n);
    if (price > MAX_PURCHASE_DOLLARS) return;
    if (state.dollars < price) return;
    state.dollars -= price;
    state.lightning += n;
    updateQuestProgress("lightning_gained", n);
    saveNow();
    updateCurrency();
    updateBadges();
    overlay.remove();
    showPopup("Куплено!", "Ты получил " + n + " ⚡ за $" + price.toFixed(2), [{text:"OK"}]);
  });

  popup.querySelector("#shopClose").addEventListener("click", function() { overlay.remove(); });
  overlay.addEventListener("click", function(e) { if (e.target === overlay) overlay.remove(); });
}

// ==================== PROMO ====================
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

    if (code === "#stsProm8t") {
      // Специальный квестовый промокод
      var hasQuest = hasActiveQuest("sts_used");
      if (!hasQuest) {
        showPopup("Недоступно", "Этот промокод работает только во время специального квеста.", [{text:"OK"}]);
        return;
      }
      state.stsUsedForQuest = true;
      updateQuestProgress("sts_used", 1);
      saveNow();
      overlay.remove();
      showPopup("Промокод принят", "Квест засчитан!", [{text:"OK"}]);
      return;
    }

    var used = state.promosUsed[code] || 0;
    if (used >= promo.max) {
      showPopup("Лимит исчерпан", 'Промокод "' + code + '" уже использован ' + used + ' раз(а).', [{text:"OK"}]);
      return;
    }
    state.promosUsed[code] = used + 1;
    if (promo.type === "lightning") {
      state.lightning += promo.amount;
      updateQuestProgress("lightning_gained", promo.amount);
    } else if (promo.type === "dollars") {
      state.dollars += promo.amount;
      updateQuestProgress("dollars_earned", promo.amount);
    }
    saveNow();
    updateCurrency();
    updateBadges();
    overlay.remove();
    showPopup("Промокод активирован!", "Ты получил: " + promo.label, [{text:"Круто!"}]);
  };

  popup.querySelector("#promoApply").addEventListener("click", apply);
  popup.querySelector("#promoCancel").addEventListener("click", function() { overlay.remove(); });
  input.addEventListener("keydown", function(e) { if (e.key === "Enter") apply(); });
  overlay.addEventListener("click", function(e) { if (e.target === overlay) overlay.remove(); });
}

// ==================== PROFILE ====================
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
    '<div class="inv-title"><span>Инвентарь (' + state.inventory.length + ')</span></div>' +
    '<div class="inv-list" id="ppInv"></div>' +
    '<button class="sell-all-btn" id="sellAll" ' + (state.inventory.length === 0 ? "disabled" : "") + '>Продать всё</button>' +
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
          updateQuestProgress("dollars_earned", it.price);
          state.inventory.splice(i, 1);
          saveNow();
          updateCurrency();
          updateBadges();
          overlay.remove();
          openProfile();
        });
      })(btns[j]);
    }
  }

  var sellAllBtn = popup.querySelector("#sellAll");
  if (sellAllBtn) {
    sellAllBtn.addEventListener("click", function() {
      if (state.inventory.length === 0) return;
      var total = 0;
      for (var i = 0; i < state.inventory.length; i++) {
        total += state.inventory[i].price;
      }
      state.dollars += total;
      updateQuestProgress("dollars_earned", total);
      state.inventory = [];
      saveNow();
      updateCurrency();
      updateBadges();
      overlay.remove();
      showPopup("Продано всё!", "Ты получил $" + total.toFixed(2), [{text:"OK"}]);
    });
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

// ==================== BONUS ====================
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
    updateQuestProgress("lightning_gained", 3);
    checkAllBonuses();
    saveNow();
    updateCurrency();
    updateBadges();
    overlay.remove();
    openBonus();
  });
  popup.querySelector("#bnTtk").addEventListener("click", function() {
    if (state.claimedTt) return;
    window.open(TTK_URL, "_blank");
    state.claimedTt = true;
    state.lightning += 3;
    updateQuestProgress("lightning_gained", 3);
    checkAllBonuses();
    saveNow();
    updateCurrency();
    updateBadges();
    overlay.remove();
    openBonus();
  });
  popup.querySelector("#bnDaily").addEventListener("click", function() {
    if (timeUntilDaily() > 0) return;
    state.lastDaily = Math.floor(Date.now() / 1000);
    state.lightning += 2;
    updateQuestProgress("lightning_gained", 2);
    updateQuestProgress("daily_used", 1);
    checkAllBonuses();
    saveNow();
    updateCurrency();
    updateBadges();
    overlay.remove();
    openBonus();
  });
  popup.querySelector("#bnClose").addEventListener("click", function() { overlay.remove(); });
  overlay.addEventListener("click", function(e) { if (e.target === overlay) overlay.remove(); });
}

function checkAllBonuses() {
  if (state.claimedTg && state.claimedTt && timeUntilDaily() > 0 && (Date.now() / 1000 - state.lastDaily) < 60) {
    updateQuestProgress("all_bonuses", 1);
  }
}

// ==================== ACHIEVEMENTS ====================
function computeAchievementStatus() {
  var result = {};
  for (var i = 0; i < ACHIEVEMENTS.length; i++) {
    var a = ACHIEVEMENTS[i];
    var done = false;
    if (a.id === "finger") done = state.spins >= 1000;
    else if (a.id === "legend") {
      done = true;
      for (var j = 0; j < CHARACTERS.length; j++) {
        if (!state.seenChars[CHARACTERS[j].id]) { done = false; break; }
      }
    }
    else if (a.id === "kak") done = state.twoUltrasInARow;
    else if (a.id === "hamam") done = state.seenChars["mellstroy"] === true || state.achievements["hamam"] === "unlocked";

    var claimed = state.achievements[a.id] === "claimed";
    result[a.id] = { done: done, claimed: claimed };
  }
  return result;
}

function openAchievements() {
  var overlay = document.createElement("div");
  overlay.className = "popup-overlay";
  var popup = document.createElement("div");
  popup.className = "popup";
  var statuses = computeAchievementStatus();
  var html = '<h2>Достижения</h2>';

  for (var i = 0; i < ACHIEVEMENTS.length; i++) {
    var a = ACHIEVEMENTS[i];
    var st = statuses[a.id];
    var rewardText = "";
    if (a.reward.type === "dollars") rewardText = "Награда: $" + a.reward.amount;
    else if (a.reward.type === "lightning") rewardText = "Награда: " + a.reward.amount + " ⚡";
    else if (a.reward.type === "mixed") {
      rewardText = "Награда: " + (a.reward.dollars ? "$" + a.reward.dollars + " " : "") + (a.reward.lightning ? a.reward.lightning + " ⚡" : "");
    }

    var btnText = "Получить";
    var btnClass = "";
    var btnDisabled = "";
    if (st.claimed) { btnText = "✓"; btnClass = "done"; btnDisabled = " disabled"; }
    else if (!st.done) { btnDisabled = " disabled"; }

    html += '<div class="ach-card ' + (st.done || st.claimed ? "" : "locked") + '">' +
      '<div class="ach-icon">' + a.icon + '</div>' +
      '<div class="ach-body">' +
        '<div class="ach-name">' + a.name + '</div>' +
        '<div class="ach-desc">' + a.desc + '</div>' +
        '<div class="ach-reward">' + rewardText + '</div>' +
      '</div>' +
      '<button class="ach-btn ' + btnClass + '" data-id="' + a.id + '"' + btnDisabled + '>' + btnText + '</button>' +
    '</div>';
  }

  html += '<div class="btn-row"><button id="achClose">Закрыть</button></div>';
  popup.innerHTML = html;
  overlay.appendChild(popup);
  document.body.appendChild(overlay);

  var btns = popup.querySelectorAll(".ach-btn");
  for (var j = 0; j < btns.length; j++) {
    (function(btn) {
      btn.addEventListener("click", function() {
        var id = btn.getAttribute("data-id");
        claimAchievement(id);
        overlay.remove();
        openAchievements();
      });
    })(btns[j]);
  }

  popup.querySelector("#achClose").addEventListener("click", function() { overlay.remove(); });
  overlay.addEventListener("click", function(e) { if (e.target === overlay) overlay.remove(); });
}

function claimAchievement(id) {
  var statuses = computeAchievementStatus();
  var st = statuses[id];
  if (!st || !st.done || st.claimed) return;
  for (var i = 0; i < ACHIEVEMENTS.length; i++) {
    if (ACHIEVEMENTS[i].id === id) {
      var a = ACHIEVEMENTS[i];
      if (a.reward.type === "dollars") state.dollars += a.reward.amount;
      else if (a.reward.type === "lightning") state.lightning += a.reward.amount;
      state.achievements[id] = "claimed";
      saveNow();
      updateCurrency();
      updateBadges();
      showPopup("Получено!", a.name + " — награда забрана.", [{text:"OK"}]);
      return;
    }
  }
}

// ==================== QUESTS ====================
function generateQuests() {
  var pool = QUEST_TEMPLATES.slice();
  // Перемешиваем
  for (var i = pool.length - 1; i > 0; i--) {
    var j = Math.floor(Math.random() * (i + 1));
    var tmp = pool[i]; pool[i] = pool[j]; pool[j] = tmp;
  }
  var selected = pool.slice(0, 4);
  var quests = [];
  for (var k = 0; k < selected.length; k++) {
    var t = selected[k];
    var n = t.base;
    if (t.plus) {
      n = t.min + Math.floor(Math.random() * (t.max - t.min + 1));
    }
    quests.push({
      id: t.id,
      plus: t.plus,
      type: t.type,
      desc: t.descTemplate.replace("{n}", n),
      target: n,
      reward: t.reward,
      multiplier: t.multiplier || false,
      completed: false,
      claimed: false
    });
  }
  return quests;
}

function ensureDailyQuests() {
  var today = todayStr();
  if (state.questsDate !== today || !state.quests) {
    state.quests = generateQuests();
    state.questsDate = today;
    state.questProgress = {};
    state.questsCompletedToday = [];
  }
}

function hasActiveQuest(type) {
  ensureDailyQuests();
  for (var i = 0; i < state.quests.length; i++) {
    if (state.quests[i].type === type && !state.quests[i].completed) return true;
  }
  return false;
}

function updateQuestProgress(type, amount) {
  ensureDailyQuests();
  for (var i = 0; i < state.quests.length; i++) {
    var q = state.quests[i];
    if (q.type !== type || q.completed) continue;
    var key = "q" + q.id;
    state.questProgress[key] = (state.questProgress[key] || 0) + amount;
    if (state.questProgress[key] >= q.target) {
      q.completed = true;
    }
  }
  saveNow();
}

function openQuests() {
  ensureDailyQuests();
  var overlay = document.createElement("div");
  overlay.className = "popup-overlay";
  var popup = document.createElement("div");
  popup.className = "popup";

  var html = '<h2>Квесты на сегодня</h2>';
  html += '<p style="margin-bottom:14px;font-size:12px;">Обновляются каждый день в 00:00</p>';

  for (var i = 0; i < state.quests.length; i++) {
    var q = state.quests[i];
    var key = "q" + q.id;
    var prog = state.questProgress[key] || 0;
    var progText = Math.min(prog, q.target) + " / " + q.target;

    var rewardText = "";
    if (q.reward.type === "dollars") rewardText = "$" + q.reward.amount;
    else if (q.reward.type === "lightning") rewardText = q.reward.amount + " ⚡";
    else if (q.reward.type === "mixed") {
      rewardText = (q.reward.dollars ? "$" + q.reward.dollars + " " : "") + (q.reward.lightning ? q.reward.lightning + " ⚡" : "");
    } else if (q.reward.type === "multiplier") {
      rewardText = "x" + q.reward.mult + " деньги на " + q.reward.duration + " сек";
    }

    var btnText = "Получить";
    var btnClass = "";
    var btnDisabled = "";
    if (q.claimed) { btnText = "✓"; btnClass = "done"; btnDisabled = " disabled"; }
    else if (!q.completed) { btnDisabled = " disabled"; }

    html += '<div class="quest-card ' + (q.completed || q.claimed ? "" : "locked") + '">' +
      '<div class="quest-icon">' + (q.plus ? "⭐" : "📜") + '</div>' +
      '<div class="quest-body">' +
        '<div class="quest-name">' + q.desc + '</div>' +
        '<div class="quest-desc">Прогресс: ' + progText + '</div>' +
        '<div class="quest-reward">Награда: ' + rewardText + '</div>' +
      '</div>' +
      '<button class="quest-btn ' + btnClass + '" data-i="' + i + '"' + btnDisabled + '>' + btnText + '</button>' +
    '</div>';
  }

  html += '<div class="btn-row"><button id="qClose">Закрыть</button></div>';
  popup.innerHTML = html;
  overlay.appendChild(popup);
  document.body.appendChild(overlay);

  var btns = popup.querySelectorAll(".quest-btn");
  for (var j = 0; j < btns.length; j++) {
    (function(btn) {
      btn.addEventListener("click", function() {
        var i = parseInt(btn.getAttribute("data-i"), 10);
        claimQuest(i);
        overlay.remove();
        openQuests();
      });
    })(btns[j]);
  }

  popup.querySelector("#qClose").addEventListener("click", function() { overlay.remove(); });
  overlay.addEventListener("click", function(e) { if (e.target === overlay) overlay.remove(); });
}

function claimQuest(i) {
  var q = state.quests[i];
  if (!q || !q.completed || q.claimed) return;
  q.claimed = true;

  if (q.reward.type === "dollars") state.dollars += q.reward.amount;
  else if (q.reward.type === "lightning") state.lightning += q.reward.amount;
  else if (q.reward.type === "mixed") {
    if (q.reward.dollars) state.dollars += q.reward.dollars;
    if (q.reward.lightning) state.lightning += q.reward.lightning;
  } else if (q.reward.type === "multiplier") {
    var until = Date.now() + q.reward.duration * 1000;
    if (q.reward.mult === 3) {
      state.mult3Until = Math.max(state.mult3Until, until);
    } else if (q.reward.mult === 2) {
      state.mult2Until = Math.max(state.mult2Until, until);
    }
  }

  state.questsCompletedToday.push(q.id);
  saveNow();
  updateCurrency();
  updateBadges();
  showPopup("Квест выполнен!", q.desc + "\nНаграда получена.", [{text:"OK"}]);
}

// ==================== BADGES ====================
function updateBadges() {
  // Достижения
  var statuses = computeAchievementStatus();
  var ready = 0;
  for (var id in statuses) {
    if (statuses[id].done && !statuses[id].claimed) ready++;
  }
  if (ready > 0) {
    el.achBadge.textContent = ready;
    el.achBadge.style.display = "flex";
  } else {
    el.achBadge.style.display = "none";
  }

  // Квесты
  ensureDailyQuests();
  var qReady = 0;
  for (var i = 0; i < state.quests.length; i++) {
    if (state.quests[i].completed && !state.quests[i].claimed) qReady++;
  }
  if (qReady > 0) {
    el.questBadge.textContent = qReady;
    el.questBadge.style.display = "flex";
  } else {
    el.questBadge.style.display = "none";
  }
}

// ==================== WELCOME / WHAT'S NEW ====================
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

  card.querySelector("#welcomeOk").addEventListener("click", function() {
    state.welcomed = true;
    state.lightning += 1;
    updateQuestProgress("lightning_gained", 1);
    saveNow();
    updateCurrency();
    updateBadges();
    overlay.remove();
  });
}

function showWhatsNew() {
  var overlay = document.createElement("div");
  overlay.className = "welcome-overlay";
  var card = document.createElement("div");
  card.className = "whatsnew-card";
  card.innerHTML =
    '<div class="whatsnew-title">Что нового в 2.0</div>' +
    '<div class="whatsnew-sub">Обновление от 29.09.2026</div>' +
    '<div class="whatsnew-list">' +
      '<div class="whatsnew-item"><div class="wn-icon">👥</div><div class="wn-text"><b>2 новых персонажа:</b> Mellstroy (Ультра) и ДунДук (Легенда). Итого 11 персонажей.</div></div>' +
      '<div class="whatsnew-item"><div class="wn-icon">💸</div><div class="wn-text"><b>Продать всё:</b> в профиле теперь есть кнопка, чтобы продать весь инвентарь разом.</div></div>' +
      '<div class="whatsnew-item"><div class="wn-icon">🎟</div><div class="wn-text"><b>5 новых промокодов:</b> #mrtPromBalanc10, #mrtPromBalanc15, #mrtPromBalanc100, #mrtPromDollar10, #mrtPromDollar50.</div></div>' +
      '<div class="whatsnew-item"><div class="wn-icon">🏆</div><div class="wn-text"><b>Достижения:</b> 4 штуки, за каждое — награда. Красный кружок покажет, что есть что забрать.</div></div>' +
      '<div class="whatsnew-item"><div class="wn-icon">📜</div><div class="wn-text"><b>Квесты:</b> каждый день 4 случайных задания. За некоторые — множитель x2/x3 на продажу.</div></div>' +
      '<div class="whatsnew-item"><div class="wn-icon">🛒</div><div class="wn-text"><b>Новый магазин:</b> вводишь сколько молний нужно — цена считается автоматически, со скидкой за крупные пакеты.</div></div>' +
    '</div>' +
    '<button class="whatsnew-btn" id="wnOk">Погнали!</button>';
  overlay.appendChild(card);
  document.body.appendChild(overlay);

  card.querySelector("#wnOk").addEventListener("click", function() {
    try { localStorage.setItem(WHATSNEW_KEY, "seen"); } catch(e) {}
    overlay.remove();
    if (!state.welcomed) {
      showWelcome();
    }
  });
}

// ==================== GAME ENTER / EXIT ====================
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

    ensureDailyQuests();
    updateBadges();

    var seenWN = false;
    try { seenWN = localStorage.getItem(WHATSNEW_KEY) === "seen"; } catch(e) {}
    if (!seenWN) {
      showWhatsNew();
    } else if (!state.welcomed) {
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

// ==================== INIT ====================
buildChances();

el.playBtn.addEventListener("click", enterGame);
el.profileBtn.addEventListener("click", openProfile);
el.promoBtn.addEventListener("click", openPromo);
el.achievementsBtn.addEventListener("click", openAchievements);
el.questsBtn.addEventListener("click", openQuests);
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
updateBadges();

// Таймер для обновления множителей и кулдауна ежедневки
setInterval(function() {
  if (el.gameScreen.classList.contains("active")) {
    updateBadges();
  }
}, 5000);
</script>
</body>
</html>****
