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

  .bg-glow { position: fixed; inset: 0; z-index: -1; overflow: hidden; pointer-events: none; contain: strict; }
  .bg-glow::before, .bg-glow::after {
    content: ""; position: absolute; width: 500px; height: 500px;
    border-radius: 50%; opacity: 0.35; will-change: transform;
  }
  .bg-glow::before {
    background: radial-gradient(circle, #ff8c00 0%, transparent 70%);
    top: -200px; left: -200px; animation: float1 22s ease-in-out infinite;
  }
  .bg-glow::after {
    background: radial-gradient(circle, #c04dff 0%, transparent 70%);
    bottom: -200px; right: -200px; animation: float2 26s ease-in-out infinite;
  }
  @keyframes float1 { 0%,100%{transform:translate3d(0,0,0);} 50%{transform:translate3d(80px,60px,0);} }
  @keyframes float2 { 0%,100%{transform:translate3d(0,0,0);} 50%{transform:translate3d(-80px,-60px,0);} }

  .landing { max-width: 980px; margin: 0 auto; padding: 60px 20px 40px; position: relative; }

  .logo { display: flex; align-items: center; justify-content: center; gap: 16px; margin-bottom: 14px; }
  .logo-icon {
    width: 72px; height: 72px; border-radius: 20px;
    background: linear-gradient(135deg, #ffd93b 0%, #ff8c00 100%);
    display: flex; align-items: center; justify-content: center;
    font-size: 38px; font-weight: 900; color: #1a1d28;
    box-shadow: 0 8px 40px rgba(255,140,0,0.5);
    will-change: transform; animation: pulse 4s ease-in-out infinite;
  }
  @keyframes pulse { 0%,100%{transform:scale(1);} 50%{transform:scale(1.04);} }
  .logo-text {
    font-size: 48px; font-weight: 900; letter-spacing: -2px;
    background: linear-gradient(135deg, #ffd93b 0%, #ff8c00 50%, #ff4d94 100%);
    -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text;
  }
  .tagline { text-align: center; color: #8a90a0; font-size: 15px; margin-bottom: 32px; }

  .release-box {
    background: linear-gradient(135deg, rgba(255,217,59,0.14) 0%, rgba(255,140,0,0.08) 100%);
    border: 1px solid rgba(255,217,59,0.4); border-radius: 20px;
    padding: 28px; text-align: center; margin-bottom: 28px;
    box-shadow: 0 12px 60px rgba(255,140,0,0.2);
  }
  .release-label { font-size: 13px; letter-spacing: 3px; text-transform: uppercase; color: #ffd93b; margin-bottom: 10px; font-weight: 700; }
  .release-date { font-size: 42px; font-weight: 900; color: #fff; letter-spacing: -1.5px; font-variant-numeric: tabular-nums; }
  .release-hint { font-size: 13px; color: #8a90a0; margin-top: 8px; }

  .cta-btn {
    display: block; margin: 0 auto 40px; max-width: 440px; width: 100%;
    text-align: center; padding: 22px 40px;
    background: linear-gradient(135deg, #ffd93b 0%, #ff8c00 100%);
    color: #1a1d28; border: none; border-radius: 16px;
    font-size: 18px; font-weight: 900; letter-spacing: 0.5px; cursor: pointer;
    box-shadow: 0 12px 44px rgba(255,140,0,0.45);
    transition: transform 0.15s, box-shadow 0.15s;
    font-family: inherit; will-change: transform;
  }
  .cta-btn:hover:not(:disabled) { transform: translateY(-3px); box-shadow: 0 16px 60px rgba(255,140,0,0.65); }
  .cta-btn:active:not(:disabled) { transform: translateY(-1px); }
  .cta-btn:disabled { background: #262a38; color: #6a7080; box-shadow: none; cursor: not-allowed; border: 1px solid #353a4d; }
  .cta-hint { text-align: center; color: #8a90a0; font-size: 13px; margin-top: -28px; margin-bottom: 32px; }
  .cta-hint.hidden { display: none; }

  .section-title { font-size: 22px; font-weight: 800; margin: 40px 0 18px; color: #fff; display: flex; align-items: center; gap: 10px; }
  .section-title::before { content: ""; width: 4px; height: 22px; background: linear-gradient(180deg, #ffd93b, #ff8c00); border-radius: 2px; }

  .chances { display: flex; flex-direction: column; gap: 8px; }
  .chance-row {
    display: grid; grid-template-columns: 44px 1fr 140px 90px;
    align-items: center; gap: 12px; padding: 14px 16px;
    background: rgba(26,29,40,0.75); border: 1px solid #262a38; border-radius: 14px;
    transition: transform 0.15s, border-color 0.15s; contain: layout style;
  }
  .chance-row:hover { transform: translateX(4px); border-color: #353a4d; }
  .chance-icon {
    width: 44px; height: 44px; border-radius: 50%;
    display: flex; align-items: center; justify-content: center;
    font-size: 14px; font-weight: 800; color: rgba(255,255,255,0.9); background: rgba(0,0,0,0.35);
  }
  .chance-name { font-size: 15px; font-weight: 700; color: #fff; }
  .chance-rarity { font-size: 12px; font-weight: 700; text-transform: uppercase; letter-spacing: 0.5px; text-align: right; }
  .chance-percent { font-size: 15px; font-weight: 800; text-align: right; font-variant-numeric: tabular-nums; color: #fff; }

  .links { display: grid; grid-template-columns: repeat(auto-fit, minmax(240px, 1fr)); gap: 14px; }
  .link-card {
    display: flex; align-items: center; gap: 14px; padding: 18px;
    background: rgba(26,29,40,0.75); border: 1px solid #262a38; border-radius: 14px;
    text-decoration: none; color: #e8e8f0;
    transition: transform 0.15s, border-color 0.15s; contain: layout style;
  }
  .link-card:hover { transform: translateY(-3px); border-color: #353a4d; }
  .link-icon {
    width: 48px; height: 48px; border-radius: 12px;
    display: flex; align-items: center; justify-content: center;
    font-size: 22px; flex-shrink: 0; color: #fff; font-weight: 900;
  }
  .link-card.tg .link-icon { background: #229ED9; }
  .link-card.tt .link-icon { background: #000; border: 1px solid #2a2a2a; }
  .link-title { font-size: 15px; font-weight: 700; }
  .link-sub { font-size: 12px; color: #8a90a0; margin-top: 3px; }

  .badges { display: flex; justify-content: center; gap: 10px; flex-wrap: wrap; margin: 50px 0 20px; }
  .badge { padding: 8px 14px; border-radius: 10px; font-size: 11px; font-weight: 700; letter-spacing: 0.3px; text-transform: uppercase; }
  .badge.deepseek { background: rgba(77,159,255,0.12); color: #4d9fff; border: 1px solid rgba(77,159,255,0.3); }
  .badge.fun { background: rgba(255,217,59,0.1); color: #ffd93b; border: 1px solid rgba(255,217,59,0.3); }
  .footer-copy { text-align: center; color: #6a7080; font-size: 12px; padding-bottom: 30px; }

  #gameScreen { display: none; flex-direction: column; height: 100vh; max-width: 900px; margin: 0 auto; position: relative; }
  #gameScreen.active { display: flex; }
  #landingScreen.hidden { display: none; }

  .topbar {
    display: flex; align-items: center; padding: 10px 14px;
    background: rgba(26,29,40,0.92); border-bottom: 1px solid #262a38;
    gap: 8px; flex-wrap: wrap; z-index: 10; contain: layout;
  }
  .tb-btn {
    background: #262a38; border: 1px solid #353a4d; color: #e8e8f0;
    padding: 8px 14px; border-radius: 10px; cursor: pointer;
    font-size: 13px; font-weight: 600;
    transition: background 0.15s, border-color 0.15s; font-family: inherit;
  }
  .tb-btn:hover { background: #2f3444; border-color: #4a5068; }
  .tb-spacer { flex: 1; }
  .curr-group {
    display: flex; align-items: center; background: #262a38;
    border: 1px solid #353a4d; border-radius: 10px; overflow: hidden;
    font-size: 13px; font-weight: 700; height: 34px;
  }
  .curr-group .curr-item { padding: 0 12px; white-space: nowrap; }
  .curr-group.lightning .curr-item { color: #ffd93b; }
  .curr-group.dollars .curr-item { color: #4dff88; }
  .plus-btn {
    background: rgba(255,217,59,0.15); border: none; border-left: 1px solid #353a4d;
    color: #ffd93b; font-size: 16px; font-weight: 800; padding: 0 12px;
    height: 100%; cursor: pointer; transition: background 0.15s; font-family: inherit;
  }
  .plus-btn:hover { background: rgba(255,217,59,0.3); }
  .close-btn {
    background: #3a1a1a; border: 1px solid #5a2020; color: #ff8080;
    width: 34px; height: 34px; border-radius: 10px; cursor: pointer;
    font-size: 14px; font-weight: 700; font-family: inherit;
  }
  .close-btn:hover { background: #4a2020; }

  .roulette-wrap {
    flex: 1; position: relative; overflow: hidden;
    background: radial-gradient(ellipse at center, #1a1d28 0%, #0a0c12 85%);
    min-height: 0; contain: strict;
  }
  #rouletteCanvas { display: block; width: 100%; height: 100%; }

  .bottom {
    padding: 14px; display: flex; gap: 10px; align-items: center;
    background: rgba(26,29,40,0.92); border-top: 1px solid #262a38;
    z-index: 10; contain: layout;
  }
  .help-btn {
    width: 48px; height: 48px; border-radius: 12px;
    background: #262a38; border: 1px solid #353a4d; color: #e8e8f0;
    font-size: 20px; font-weight: 700; cursor: pointer; font-family: inherit; transition: background 0.15s;
  }
  .help-btn:hover { background: #2f3444; }
  .bonus-btn {
    height: 48px; padding: 0 16px; border-radius: 12px;
    background: #262a38; border: 1px solid #353a4d; color: #e8e8f0;
    font-size: 13px; font-weight: 600; cursor: pointer; white-space: nowrap;
    font-family: inherit; transition: background 0.15s;
  }
  .bonus-btn:hover { background: #2f3444; }
  .spin-btn {
    flex: 1; height: 52px; border-radius: 12px;
    background: linear-gradient(135deg, #ffd93b 0%, #ff8c00 100%);
    border: none; color: #1a1d28; font-size: 16px; font-weight: 900;
    cursor: pointer; letter-spacing: 0.5px;
    transition: transform 0.15s, box-shadow 0.15s;
    box-shadow: 0 4px 20px rgba(255,140,0,0.4);
    font-family: inherit; will-change: transform;
  }
  .spin-btn:hover:not(:disabled) { transform: translateY(-1px); box-shadow: 0 6px 26px rgba(255,140,0,0.6); }
  .spin-btn:disabled { opacity: 0.5; cursor: not-allowed; }

  .popup-overlay {
    position: fixed; inset: 0; background: rgba(0,0,0,0.75);
    display: flex; align-items: center; justify-content: center;
    z-index: 100; padding: 20px;
    animation: fadeIn 0.15s; contain: strict;
  }
  @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
  .popup {
    background: #1e222e; border: 1px solid #353a4d; border-radius: 18px;
    padding: 26px; max-width: 440px; width: 100%;
    box-shadow: 0 24px 80px rgba(0,0,0,0.7);
    animation: popIn 0.2s cubic-bezier(0.2, 0.9, 0.3, 1.3);
    max-height: 85vh; overflow-y: auto;
  }
  @keyframes popIn { from { transform: scale(0.85); opacity: 0; } to { transform: scale(1); opacity: 1; } }
  .popup h2 { font-size: 20px; margin-bottom: 14px; text-align: center; color: #fff; font-weight: 800; }
  .popup p { font-size: 14px; color: #b8bcc8; text-align: center; margin-bottom: 18px; line-height: 1.6; white-space: pre-line; }
  .popup .btn-row { display: flex; gap: 10px; margin-top: 14px; }
  .popup button {
    flex: 1; padding: 13px; border-radius: 10px; border: none;
    background: #ffd93b; color: #1a1d28; font-size: 14px; font-weight: 800;
    cursor: pointer; transition: background 0.15s; font-family: inherit;
  }
  .popup button:hover:not(:disabled) { background: #ffdf55; }
  .popup button.secondary { background: #262a38; color: #e8e8f0; border: 1px solid #353a4d; }
  .popup button.secondary:hover { background: #2f3444; }
  .popup button:disabled { opacity: 0.45; cursor: not-allowed; }

  .popup input {
    width: 100%; background: #262a38; border: 1px solid #353a4d;
    border-radius: 10px; padding: 13px; color: #fff; font-size: 15px;
    font-weight: 600; outline: none; margin-bottom: 12px;
    font-family: inherit; text-align: center; letter-spacing: 1px;
  }
  .popup input:focus { border-color: #ffd93b; }

  .shop-pack {
    display: flex; align-items: center; justify-content: space-between;
    padding: 12px 14px; background: #262a38; border-radius: 10px;
    margin-bottom: 8px; font-size: 14px; gap: 10px;
  }
  .shop-pack .sp-left { color: #ffd93b; font-weight: 800; }
  .shop-pack .sp-right { color: #4dff88; font-weight: 800; }
  .shop-pack button {
    background: #ffd93b; color: #1a1d28; border: none;
    padding: 8px 14px; border-radius: 8px; font-size: 12px; font-weight: 800;
    cursor: pointer; flex: 0 0 auto; font-family: inherit;
  }
  .shop-pack button:disabled { opacity: 0.4; cursor: not-allowed; }

  .profile-grid { display: grid; grid-template-columns: 96px 1fr; gap: 14px; margin-bottom: 14px; align-items: center; }
  .avatar-box {
    width: 96px; height: 96px; border-radius: 16px; background: #262a38;
    border: 2px dashed #4a5068; display: flex; align-items: center; justify-content: center;
    cursor: pointer; font-size: 36px; color: #4a5068; overflow: hidden;
    transition: border-color 0.15s, color 0.15s;
  }
  .avatar-box:hover { border-color: #ffd93b; color: #ffd93b; }
  .avatar-box img { width: 100%; height: 100%; object-fit: cover; }
  .profile-stats {
    background: #262a38; border-radius: 10px; padding: 14px;
    font-size: 14px; color: #b8bcc8; margin-bottom: 14px;
    display: flex; justify-content: space-around; text-align: center;
  }
  .profile-stats b { color: #fff; font-size: 16px; display: block; margin-top: 4px; }
  .inv-title {
    font-size: 12px; color: #8a90a0; text-transform: uppercase;
    letter-spacing: 1.2px; margin: 10px 0 8px; text-align: left; font-weight: 700;
  }
  .inv-list { max-height: 240px; overflow-y: auto; display: flex; flex-direction: column; gap: 6px; }
  .inv-card {
    display: flex; align-items: center; justify-content: space-between;
    padding: 10px 12px; background: #262a38; border-radius: 10px;
    font-size: 13px; gap: 8px;
  }
  .inv-card .ic-name { font-weight: 800; color: #fff; flex: 1; }
  .inv-card .ic-rarity { font-size: 11px; font-weight: 700; }
  .inv-card button {
    background: #4dff88; border: none; color: #12141c;
    padding: 6px 10px; border-radius: 8px; font-size: 12px; font-weight: 800;
    cursor: pointer; white-space: nowrap; flex: 0 0 auto; font-family: inherit;
  }
  .inv-card button:hover { background: #6aff9c; }
  .empty-inv { text-align: center; color: #5a6070; font-size: 13px; padding: 24px; }

  /* ============ WELCOME ============ */
  .welcome-overlay {
    position: fixed; inset: 0;
    background: radial-gradient(circle at center, rgba(255,140,0,0.2) 0%, rgba(0,0,0,0.9) 70%);
    display: flex; align-items: center; justify-content: center;
    z-index: 200; padding: 20px;
    animation: fadeIn 0.4s ease-out;
    contain: strict;
  }
  .welcome-card {
    background: linear-gradient(160deg, #1e222e 0%, #16181f 100%);
    border: 2px solid rgba(255,217,59,0.5);
    border-radius: 24px;
    padding: 40px 32px;
    max-width: 420px; width: 100%;
    text-align: center;
    box-shadow:
      0 0 60px rgba(255,140,0,0.5),
      0 0 120px rgba(255,140,0,0.2),
      inset 0 0 40px rgba(255,217,59,0.05);
    animation: welcomePop 0.6s cubic-bezier(0.2, 0.9, 0.3, 1.3);
    position: relative;
    overflow: hidden;
  }
  .welcome-card::before {
    content: "";
    position: absolute;
    top: -50%; left: -50%; width: 200%; height: 200%;
    background: conic-gradient(from 0deg, transparent, rgba(255,217,59,0.15), transparent 30%);
    animation: rotate 6s linear infinite;
    pointer-events: none;
  }
  @keyframes rotate { to { transform: rotate(360deg); } }
  @keyframes welcomePop {
    0% { transform: scale(0.5) rotate(-5deg); opacity: 0; }
    60% { transform: scale(1.05) rotate(2deg); opacity: 1; }
    100% { transform: scale(1) rotate(0); opacity: 1; }
  }
  .welcome-emoji {
    font-size: 72px;
    margin-bottom: 16px;
    animation: bounce 1.6s ease-in-out infinite;
    display: inline-block;
    filter: drop-shadow(0 0 20px rgba(255,217,59,0.8));
  }
  @keyframes bounce {
    0%,100% { transform: translateY(0) rotate(-5deg); }
    50% { transform: translateY(-12px) rotate(5deg); }
  }
  .welcome-title {
    font-size: 28px;
    font-weight: 900;
    margin-bottom: 12px;
    background: linear-gradient(135deg, #ffd93b 0%, #ff8c00 50%, #ff4d94 100%);
    -webkit-background-clip: text; -webkit-text-fill-color: transparent;
    background-clip: text;
    letter-spacing: -0.5px;
    position: relative;
    z-index: 1;
  }
  .welcome-text {
    font-size: 15px;
    color: #b8bcc8;
    margin-bottom: 24px;
    line-height: 1.6;
    position: relative;
    z-index: 1;
  }
  .welcome-bonus {
    display: inline-flex;
    align-items: center;
    gap: 10px;
    background: linear-gradient(135deg, rgba(255,217,59,0.25) 0%, rgba(255,140,0,0.15) 100%);
    border: 2px solid #ffd93b;
    border-radius: 14px;
    padding: 14px 24px;
    font-size: 22px;
    font-weight: 900;
    color: #ffd93b;
    margin-bottom: 24px;
    box-shadow: 0 0 30px rgba(255,217,59,0.5);
    animation: glow 1.5s ease-in-out infinite;
    position: relative;
    z-index: 1;
  }
  @keyframes glow {
    0%,100% { box-shadow: 0 0 30px rgba(255,217,59,0.5); }
    50% { box-shadow: 0 0 50px rgba(255,217,59,0.9); }
  }
  .welcome-btn {
    background: linear-gradient(135deg, #ffd93b 0%, #ff8c00 100%);
    color: #1a1d28;
    border: none;
    padding: 16px 40px;
    border-radius: 14px;
    font-size: 16px;
    font-weight: 900;
    cursor: pointer;
    letter-spacing: 0.5px;
    box-shadow: 0 8px 30px rgba(255,140,0,0.5);
    transition: transform 0.15s, box-shadow 0.15s;
    font-family: inherit;
    position: relative;
    z-index: 1;
  }
  .welcome-btn:hover {
    transform: translateY(-2px) scale(1.03);
    box-shadow: 0 12px 40px rgba(255,140,0,0.8);
  }
  .welcome-btn:active { transform: translateY(0) scale(1); }

  /* Конфетти */
  .confetti {
    position: fixed;
    top: -20px;
    width: 10px;
    height: 10px;
    z-index: 199;
    pointer-events: none;
    animation: fall linear forwards;
  }
  @keyframes fall {
    to { transform: translateY(110vh) rotate(720deg); opacity: 0.3; }
  }

  @media (max-width: 600px) {
    .logo-text { font-size: 34px; }
    .logo-icon { width: 56px; height: 56px; font-size: 30px; }
    .release-date { font-size: 28px; }
    .chance-row { grid-template-columns: 40px 1fr 80px; padding: 12px; gap: 8px; }
    .chance-icon { width: 40px; height: 40px; font-size: 12px; }
    .chance-rarity { display: none; }
    .topbar { padding: 8px; gap: 6px; }
    .tb-btn { padding: 6px 10px; font-size: 12px; }
    .welcome-emoji { font-size: 56px; }
    .welcome-title { font-size: 22px; }
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
    <div class="release-hint" id="releaseHint">Уже доступно!</div>
  </div>

  <button class="cta-btn" id="playBtn">ИГРАТЬ СЕЙЧАС</button>
  <div class="cta-hint hidden" id="ctaHint"></div>

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

// ==================== КОНСТАНТЫ ====================
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
    const k = Math
