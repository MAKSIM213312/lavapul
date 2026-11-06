[index.html](https://github.com/user-attachments/files/34504101/index.html)
<!doctype html>
<html lang="ru">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no, viewport-fit=cover">
<title>Лавапул</title>
<script src="https://telegram.org/js/telegram-web-app.js"></script>
<style>
  :root {
    --bg: #120c1c; --panel: #1d1428; --card: #2a1d38; --card2: #33244a;
    --accent: #ff7a1a; --accent-dark: #b44f06; --text: #fff; --muted: #b9a9c9;
    --gold: #ffd166; --gem: #7df9ff; --star: #ffcc33;
  }
  html, body {
    margin: 0; height: 100%; overflow: hidden; background: var(--bg); color: var(--text);
    font-family: system-ui, -apple-system, "Segoe UI", Roboto, sans-serif;
    user-select: none; -webkit-user-select: none; -webkit-tap-highlight-color: transparent;
    overscroll-behavior: none; touch-action: manipulation;
  }
  #app { display: flex; flex-direction: column; height: 100%; }
  #top { position: relative; padding: 10px 16px 6px; text-align: center; min-height: 62px; }
  #lava { font-size: 28px; font-weight: 900; letter-spacing: .5px; }
  #rates { font-size: 13px; color: var(--muted); margin-top: 2px; }
  #obsTop { font-size: 12px; color: #c7b3ff; margin-top: 2px; }
  #chips { position: absolute; left: 10px; top: 10px; display: flex; flex-direction: column; gap: 4px; align-items: flex-start; }
  .chip {
    border: 0; border-radius: 12px; padding: 4px 9px; font: inherit; font-size: 13px; font-weight: 800;
    background: var(--card); color: var(--gem);
  }
  #boostChip { color: var(--gold); }
  #onlineChip { color: #7dff9a; }
  .roundBtn {
    position: absolute; top: 12px; width: 34px; height: 34px; border-radius: 50%;
    border: 0; background: var(--card); color: var(--text); font-size: 16px; font-weight: 800;
  }
  #helpBtn { right: 12px; }
  #soundBtn { right: 52px; }
  .hidden { display: none !important; }
  #scene { position: relative; flex: 1; min-height: 200px; }
  #scene canvas { position: absolute; inset: 0; width: 100%; height: 100%; touch-action: none; }
  #wheelBtn {
    position: absolute; right: 12px; top: 42px; width: 48px; height: 48px; border-radius: 50%; border: 0;
    background: rgba(0,0,0,.45); font-size: 26px; box-shadow: 0 0 0 2px rgba(255,209,102,.6);
  }
  .dotted::after {
    content: ''; position: absolute; top: 2px; right: 2px; width: 11px; height: 11px; border-radius: 50%;
    background: #ff3b3b; box-shadow: 0 0 0 2px var(--bg);
  }
  #panel {
    height: 42%; max-height: 400px; min-height: 220px; background: var(--panel);
    border-radius: 18px 18px 0 0; display: flex; flex-direction: column;
    box-shadow: 0 -6px 20px rgba(0,0,0,.4);
  }
  #tabs { display: flex; gap: 4px; padding: 8px 8px 6px; }
  #tabs button {
    position: relative; flex: 1; min-width: 0; border: 0; border-radius: 12px; padding: 5px 2px 4px;
    font: inherit; background: var(--card); color: var(--muted); display: flex; flex-direction: column;
    align-items: center; gap: 1px;
  }
  #tabs button .ti { font-size: 18px; line-height: 1.1; }
  #tabs button .tl { font-size: 10px; font-weight: 800; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; max-width: 100%; }
  #tabs button.on { background: var(--accent); color: #2a1200; }
  #tabs button.dot::after {
    content: ''; position: absolute; top: 4px; right: 6px; width: 9px; height: 9px; border-radius: 50%;
    background: #ff3b3b; box-shadow: 0 0 0 2px var(--panel);
  }
  #list { flex: 1; overflow-y: auto; padding: 4px 10px 14px; touch-action: pan-y; -webkit-overflow-scrolling: touch; }
  .sec { font-size: 13px; font-weight: 800; color: var(--muted); margin: 8px 2px 6px; text-transform: uppercase; letter-spacing: .5px; }
  .item {
    display: flex; align-items: center; gap: 10px; background: var(--card);
    border-radius: 14px; padding: 8px 10px; margin-bottom: 8px;
  }
  .item.locked { opacity: .55; }
  .item.hot { box-shadow: inset 0 0 0 2px var(--gold); }
  .ic { font-size: 28px; width: 40px; height: 40px; display: flex; align-items: center; justify-content: center; flex: none; border-radius: 10px; font-weight: 900; }
  .info { flex: 1; min-width: 0; }
  .nm { font-weight: 800; font-size: 15px; }
  .lv { color: var(--muted); font-weight: 600; font-size: 12px; margin-left: 6px; }
  .ds { font-size: 12px; color: var(--muted); margin-top: 2px; line-height: 1.3; }
  .bar { height: 6px; background: rgba(0,0,0,.35); border-radius: 3px; margin-top: 5px; overflow: hidden; }
  .bar i { display: block; height: 100%; width: 0; background: linear-gradient(90deg, var(--gold), var(--accent)); border-radius: 3px; }
  .buy {
    flex: none; border: 0; border-radius: 12px; padding: 9px 8px; min-width: 82px; font: inherit;
    font-weight: 800; font-size: 14px; background: var(--accent); color: #2a1200;
    box-shadow: 0 3px 0 var(--accent-dark);
  }
  .buy:active:not(:disabled) { transform: translateY(2px); box-shadow: 0 1px 0 var(--accent-dark); }
  .buy.gem { background: #2fd4e8; color: #04262b; box-shadow: 0 3px 0 #168896; }
  .buy.star { background: var(--star); color: #3a2600; box-shadow: 0 3px 0 #a87d00; }
  .buy.ok { background: #46c46a; color: #06260f; box-shadow: 0 3px 0 #23803f; }
  .buy:disabled, .buy.gem:disabled, .buy.star:disabled, .buy.ok:disabled { background: #3a2c48; color: #8d7c9e; box-shadow: none; }
  .ghost { background: var(--card2) !important; color: var(--text) !important; box-shadow: none !important; }
  .card { background: var(--card); border-radius: 14px; padding: 14px; text-align: center; line-height: 1.5; margin-bottom: 8px; }
  .card .big { font-size: 30px; font-weight: 900; }
  .card p { margin: 6px 0; font-size: 14px; color: var(--muted); }
  .card b { color: var(--text); }
  .wide { width: 100%; margin-top: 8px; padding: 12px; font-size: 16px; }
  .note { font-size: 12px; color: var(--muted); text-align: center; margin: 4px 4px 10px; line-height: 1.4; }
  .overlay {
    position: fixed; inset: 0; background: rgba(5,2,10,.8); display: flex;
    align-items: center; justify-content: center; padding: 20px; z-index: 10;
  }
  .box {
    background: var(--panel); border-radius: 18px; padding: 20px; max-width: 360px; width: 100%;
    box-shadow: 0 10px 40px rgba(0,0,0,.6); max-height: 90vh; overflow-y: auto;
  }
  #modal { z-index: 20; }
  #modalTitle, .wtitle { font-size: 22px; font-weight: 900; margin-bottom: 10px; text-align: center; }
  #modalBody { font-size: 14px; line-height: 1.5; color: #e6dcef; }
  #modalBody ul { padding-left: 18px; margin: 0; }
  #modalBody li { margin-bottom: 6px; }
  #modalBody p { margin: 10px 0 0; }
  #modalBtns { display: flex; gap: 8px; margin-top: 16px; }
  #modalBtns button { flex: 1; padding: 12px; font-size: 16px; }
  .dgrid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 6px; }
  .dcell { background: var(--card); border-radius: 10px; padding: 6px 2px; text-align: center; font-size: 11px; font-weight: 700; color: var(--muted); }
  .dcell .dico { font-size: 20px; margin: 2px 0; }
  .dcell.done { opacity: .45; }
  .dcell.today { background: var(--accent); color: #2a1200; }
  .dcell:last-child { grid-column: span 2; }
  .wwrap { position: relative; width: 260px; height: 260px; margin: 6px auto 10px; }
  .wwrap canvas { width: 260px; height: 260px; }
  .wptr { position: absolute; left: 50%; top: -6px; transform: translateX(-50%); font-size: 30px; color: #fff; text-shadow: 0 2px 4px #000; }
  .wres { text-align: center; min-height: 24px; font-weight: 800; font-size: 17px; color: var(--gold); }
</style>
</head>
<body>
<div id="app">
  <div id="top">
    <div id="chips">
      <button id="gemChip" class="chip">💎 0</button>
      <span id="boostChip" class="chip hidden"></span>
      <span id="onlineChip" class="chip hidden"></span>
    </div>
    <div id="lava">🔥 0</div>
    <div id="rates"></div>
    <div id="obsTop"></div>
    <button id="soundBtn" class="roundBtn">🔊</button>
    <button id="helpBtn" class="roundBtn">?</button>
  </div>
  <div id="scene">
    <canvas id="c"></canvas>
    <button id="wheelBtn">🎡</button>
  </div>
  <div id="panel">
    <div id="tabs"></div>
    <div id="list"></div>
  </div>
</div>

<div id="wheel" class="overlay hidden">
  <div class="box">
    <div class="wtitle">🎡 Колесо удачи</div>
    <div class="wwrap"><canvas id="wc" width="520" height="520"></canvas><div class="wptr">▼</div></div>
    <div id="wres" class="wres"></div>
    <button id="spinBtn" class="buy wide"></button>
    <button id="wclose" class="buy wide ghost">Закрыть</button>
  </div>
</div>

<div id="modal" class="overlay hidden">
  <div class="box">
    <div id="modalTitle"></div>
    <div id="modalBody"></div>
    <div id="modalBtns"></div>
  </div>
</div>

<script>
(() => {
  // =====================================================================
  //  НАСТРОЙКИ МАГАЗИНА (ссылки создаёт бот: python bot.py links)
  // =====================================================================
  const INVOICE_LINKS = {"starter": "https://t.me/$oifzSFW3eEvOGAAATH8t2EOsNpU", "gems100": "https://t.me/$_8jJ_lW3eEvPGAAAWCPcV5lI5Fc", "gems550": "https://t.me/$zQ_3vVW3eEvQGAAA7-CzKWOn_-E", "gems1200": "https://t.me/$I7L91FW3eEvRGAAAgmBT3Zq_xuE", "golem": "https://t.me/$Ya6aAlW3eEvSGAAA8Z7W7syEm4w", "cosmic": "https://t.me/$Ck7IslW3eEvTGAAAAPrl4Fex5MI", "x2": "https://t.me/$YTXZ8FW3eEvUGAAApsUf_P_A1cE", "bp": "https://t.me/$z19QC1W3eEvgGAAAqDaVow0MRag"};
  // Вне Telegram (в обычном браузере) покупки выдаются бесплатно для проверки.
  const TEST_PURCHASES = false;
  const BOT_USERNAME = 'lavapulbot';

  // ================= Telegram =================
  const tg = window.Telegram && window.Telegram.WebApp;
  const inTelegram = !!(tg && tg.initData);
  const tgOk = (v) => { try { return inTelegram && tg.isVersionAtLeast(v); } catch (e) { return false; } };
  if (tg) {
    try { tg.ready(); tg.expand(); } catch (e) {}
    try { if (tgOk('7.7')) tg.disableVerticalSwipes(); } catch (e) {}
    try { if (tgOk('6.1')) { tg.setHeaderColor('#120c1c'); tg.setBackgroundColor('#120c1c'); } } catch (e) {}
  }
  const tgUser = (() => { try { return (inTelegram && tg.initDataUnsafe && tg.initDataUnsafe.user) || null; } catch (e) { return null; } })();
  let lastHaptic = 0;
  const haptic = (k) => {
    if (!tgOk('6.1')) return;
    const now = performance.now();
    if (k === 'light' && now - lastHaptic < 70) return;
    lastHaptic = now;
    try {
      if (k === 'success' || k === 'error') tg.HapticFeedback.notificationOccurred(k);
      else tg.HapticFeedback.impactOccurred(k);
    } catch (e) {}
  };

  // ================= Данные игры =================
  const GENS = [
    { id: 'crack',   icon: '🕳️', name: 'Трещина',              base: 20,     rate: 0.5 },
    { id: 'geyser',  icon: '♨️', name: 'Гейзер',               base: 200,    rate: 4 },
    { id: 'chamber', icon: '🔥', name: 'Магматическая камера', base: 2400,   rate: 30 },
    { id: 'river',   icon: '🌋', name: 'Лавовая река',         base: 30000,  rate: 200 },
    { id: 'core',    icon: '🌍', name: 'Ядро планеты',         base: 450000, rate: 1500 },
    { id: 'star',    icon: '☀️', name: 'Звёздный жар',         base: 7e6,    rate: 12000 },
    { id: 'void',    icon: '🌌', name: 'Лава из другого мира', base: 1.3e8,  rate: 110000 },
  ];
  const GEN_GROWTH = 1.17;
  const MILESTONES = [25, 50, 100, 200, 300];

  const UPS = [
    { id: 'tap',    icon: '⛏️', name: 'Кирка',             base: 15,   mult: 1.75, max: Infinity },
    { id: 'crit',   icon: '✨', name: 'Искры жара',        base: 250,  mult: 2.3,  max: 20 },
    { id: 'heat',   icon: '🌡️', name: 'Чуткий кратер',     base: 400,  mult: 2.5,  max: 10 },
    { id: 'erupt',  icon: '⏱️', name: 'Долгое извержение', base: 600,  mult: 2.7,  max: 10 },
    { id: 'magnet', icon: '🧲', name: 'Лавовый магнит',    base: 2000, mult: 3,    max: 10 },
    { id: 'combo',  icon: '⚡', name: 'Ритм земли',         base: 1000, mult: 2.8,  max: 8 },
  ];

  const SKINS = {
    classic: { name: 'Классический', sky: ['#120a24', '#3d1b3c'], mnt: ['#5b3a30', '#22140f'], lava: '#ff7a1a', hi: '#ffd166', glow: '255,120,40', parts: ['#ffde59', '#ff9a2e', '#ff5a1f'], ground: '#1a100d', unlock: { free: true } },
    ash:     { name: 'Пепельный',    sky: ['#17161d', '#3a3540'], mnt: ['#5e5d66', '#1d1d22'], lava: '#ff4d2e', hi: '#ffc2b0', glow: '255,80,50',  parts: ['#ffc2b0', '#ff4d2e', '#9a9a9a'], ground: '#141418', unlock: { stat: 'eruptions', need: 25, text: 'Устрой 25 извержений' } },
    ice:     { name: 'Ледяной',      sky: ['#081a33', '#1d4b6e'], mnt: ['#d8eeff', '#4a7aa8'], lava: '#38d6ff', hi: '#e6fbff', glow: '90,200,255', parts: ['#e6fbff', '#38d6ff', '#7aa8ff'], ground: '#0d2236', unlock: { gems: 150 } },
    emerald: { name: 'Изумрудный',   sky: ['#04140d', '#123a2a'], mnt: ['#2f6a45', '#0f2318'], lava: '#3dff8a', hi: '#d0ffe0', glow: '60,255,140', parts: ['#d0ffe0', '#3dff8a', '#1fbf63'], ground: '#08170f', unlock: { gems: 250 } },
    neon:    { name: 'Неоновый',     sky: ['#0b0221', '#2a0b4a'], mnt: ['#3a1a6a', '#0e0624'], lava: '#ff2bd6', hi: '#7dfcff', glow: '255,43,214', parts: ['#ff2bd6', '#7dfcff', '#b26bff'], ground: '#0a031a', unlock: { gems: 400 } },
    gold:    { name: 'Золотой',      sky: ['#1f1405', '#4a3108'], mnt: ['#d4a23a', '#5a3a0c'], lava: '#fff1a8', hi: '#ffffff', glow: '255,215,90', parts: ['#ffffff', '#fff1a8', '#ffcc33'], ground: '#1f1405', bonus: 0.1, unlock: { gems: 800 } },
    flame:   { name: 'Пламя сезона', sky: ['#1a0505', '#4a0d0d'], mnt: ['#3a1210', '#0a0303'], lava: '#ff3300', hi: '#ffcc00', glow: '255,60,0',   parts: ['#ffcc00', '#ff3300', '#ff8800'], ground: '#0a0303', bonus: 0.15, unlock: { bp: true } },
    cosmic:  { name: 'Космический',  sky: ['#05010f', '#1a0838'], mnt: ['#3b2b6b', '#120a2a'], lava: '#b98cff', hi: '#ffffff', glow: '170,120,255', parts: ['#b98cff', '#ffffff', '#7dd3ff'], ground: '#07031a', bonus: 0.2, unlock: { stars: 'cosmic' } },
  };

  const PETS = [
    { id: 'salamander', icon: '🦎', name: 'Огненная саламандра', gems: 40,  lava: 3000 },
    { id: 'dragon',     icon: '🐉', name: 'Лавовый дракончик',   gems: 90,  lava: 8000 },
    { id: 'eagle',      icon: '🦅', name: 'Огненный орёл',       gems: 120, lava: 12000 },
    { id: 'turtle',     icon: '🐢', name: 'Каменная черепаха',   gems: 70,  lava: 5000 },
  ];
  const PET_MAX = 10;

  const STAGES = [
    { name: 'Кочка',           at: 0 },
    { name: 'Холм',            at: 1e4 },
    { name: 'Вулкан',          at: 1e6 },
    { name: 'Огненная гора',   at: 1e8 },
    { name: 'Великий вулкан',  at: 1e10 },
    { name: 'Титан',           at: 1e12 },
  ];

  const BOSSES = [
    { icon: '🧊', name: 'Ледяной голем' },
    { icon: '🐙', name: 'Морской кракен' },
    { icon: '👾', name: 'Космический захватчик' },
    { icon: '⛄', name: 'Снежный великан' },
  ];

  const WHEEL = [
    { label: '5 мин',  r: { lavaMin: 5 },    w: 22, c: '#ff7a1a' },
    { label: '1 💎',   r: { gems: 1 },       w: 20, c: '#2fd4e8' },
    { label: '15 мин', r: { lavaMin: 15 },   w: 15, c: '#ff9a2e' },
    { label: 'XP +40', r: { xp: 40 },        w: 15, c: '#b26bff' },
    { label: '×2 15м', r: { boostMin: 15 },  w: 12, c: '#ffd166' },
    { label: '3 💎',   r: { gems: 3 },       w: 8,  c: '#38d6ff' },
    { label: '1 час',  r: { lavaMin: 60 },   w: 6,  c: '#ff5a1f' },
    { label: '10 💎',  r: { gems: 10 },      w: 2,  c: '#7df9ff' },
  ];
  const WHEEL_EXTRA_COST = 10;
  const WHEEL_EXTRA_MAX = 3;

  const GEM_ITEMS = [
    { id: 'boost', icon: '⚡', name: 'Буст ×2 на 30 минут', cost: 30 },
    { id: 'erupt', icon: '🌋', name: 'Извержение прямо сейчас', cost: 15 },
    { id: 'time',  icon: '⏩', name: '2 часа дохода сразу', cost: 40 },
  ];

  // Цены в звёздах должны совпадать с ценами в bot.py
  const PRODUCTS = [
    { id: 'starter',  icon: '🎁', name: 'Стартовый набор',    desc: '300 💎 + Ледяной скин + 2 часа дохода. Один раз!', stars: 99,  once: true, grant: { gems: 300, skin: 'ice', hours: 2 } },
    { id: 'bp',       icon: '🎟️', name: 'Премиум-пропуск',    desc: 'Премиум-награды сезона: алмазы, дракончик, буст и скин «Пламя сезона»', stars: 199, season: true, grant: { bp: true } },
    { id: 'gems100',  icon: '💎', name: '100 алмазов',        desc: 'Для скинов, питомцев и бустов', stars: 50, grant: { gems: 100 } },
    { id: 'gems550',  icon: '💎', name: '550 алмазов',        desc: '+10% алмазов в подарок', stars: 250, grant: { gems: 550 } },
    { id: 'gems1200', icon: '💎', name: '1200 алмазов',       desc: '+20% алмазов в подарок', stars: 500, grant: { gems: 1200 } },
    { id: 'golem',    icon: '🗿', name: 'Лавовый голем',      desc: 'Сам тапает вулкан 4 раза в секунду. Навсегда', stars: 149, once: true, grant: { perm: 'golem' } },
    { id: 'cosmic',   icon: '🌌', name: 'Космический вулкан', desc: 'Эксклюзивный скин, +20% к лаве', stars: 199, once: true, grant: { skin: 'cosmic' } },
    { id: 'x2',       icon: '🔥', name: 'Вечный ×2',          desc: 'Вся лава ×2 навсегда, даже после нового острова', stars: 299, once: true, grant: { perm: 'x2' } },
  ];

  const ACH = [
    { id: 'taps',   icon: '👆', name: 'Тапы по вулкану',     stat: 'taps',      goals: [100, 1000, 5000, 25000, 100000], gems: [1, 2, 4, 8, 15] },
    { id: 'erupt',  icon: '🌋', name: 'Извержения',          stat: 'eruptions', goals: [1, 10, 50, 200, 1000],          gems: [1, 2, 4, 8, 15] },
    { id: 'bombs',  icon: '💥', name: 'Пойманные бомбы',     stat: 'bombs',     goals: [10, 100, 500, 2000, 10000],     gems: [1, 2, 4, 8, 15] },
    { id: 'boss',   icon: '👹', name: 'Побеждённые боссы',   stat: 'bosses',    goals: [1, 5, 25, 100],                 gems: [2, 4, 8, 15] },
    { id: 'meteor', icon: '☄️', name: 'Пойманные метеориты', stat: 'meteors',   goals: [1, 5, 25, 100],                 gems: [2, 4, 8, 15] },
    { id: 'cloud',  icon: '☁️', name: 'Разогнанные тучи',    stat: 'clouds',    goals: [1, 5, 25, 100],                 gems: [2, 4, 8, 15] },
    { id: 'crit',   icon: '✨', name: 'Крит-тапы',           stat: 'crits',     goals: [10, 100, 1000, 5000],           gems: [1, 3, 6, 12] },
    { id: 'combo',  icon: '⚡', name: 'Рекорд комбо',        stat: 'maxCombo',  goals: [50, 100, 200, 400],             gems: [1, 3, 6, 12] },
    { id: 'lava',   icon: '🔥', name: 'Лавы за всё время',   stat: 'allTime',   goals: [1e3, 1e5, 1e7, 1e9, 1e12],      gems: [1, 3, 6, 12, 25] },
    { id: 'isle',   icon: '🏝️', name: 'Новые острова',       stat: 'islands',   goals: [1, 3, 10, 25],                  gems: [5, 10, 20, 40] },
  ];

  const DAILY = [{ min: 10 }, { min: 20 }, { gems: 2 }, { min: 40 }, { gems: 4 }, { min: 90 }, { gems: 10 }];

  const DQ_POOL = [
    { id: 'taps',      goals: [300, 600],  text: (g) => `Сделай ${g} тапов` },
    { id: 'eruptions', goals: [3, 6],      text: (g) => `Устрой ${g} извержений` },
    { id: 'bombs',     goals: [15, 40],    text: (g) => `Поймай ${g} лавовых бомб` },
    { id: 'meteors',   goals: [1, 3],      text: (g) => `Поймай метеориты: ${g}` },
    { id: 'bosses',    goals: [1, 2],      text: (g) => `Победи боссов: ${g}`, min: 5000 },
    { id: 'spins',     goals: [1, 2],      text: (g) => `Крутани колесо удачи: ${g}` },
    { id: 'buys',      goals: [5, 15],     text: (g) => `Купи ${g} улучшений или источников` },
    { id: 'maxCombo',  goals: [60, 120],   text: (g) => `Набери комбо ${g}` },
    { id: 'crits',     goals: [10, 25],    text: (g) => `Сделай ${g} крит-тапов`, min: 3000 },
    { id: 'clouds',    goals: [1, 2],      text: (g) => `Разгони тучи: ${g}` },
  ];

  const BP_LEVELS = 30, BP_XP = 100;
  const REF_GEMS = 15;
  const REF_MILESTONES = [{ n: 3, gems: 30 }, { n: 10, gems: 100 }];

  // ================= Сохраняемое состояние =================
  const RUN_KEYS = ['lava', 'total', 'up', 'gen', 'last'];
  function fresh(k) {
    const d = {
      v: 3, lava: 0, total: 0, up: {}, gen: {}, last: Date.now(),
      allTime: 0, obsidian: 0, gems: 0, skins: { classic: true }, skin: 'classic',
      perm: {}, bought: {}, stats: {}, ach: {}, daily: { last: '', streak: 0 },
      boostUntil: 0, seenHelp: false, pets: {}, bp: null, dq: null,
      wheel: { day: '', xday: '', extra: 0 }, refs: 0, refsClaimed: 0, refMs: {},
      welcomeDone: false, sound: true, bossLevel: 0, uid: 0,
    };
    if (k) for (const key of Object.keys(d)) if (!RUN_KEYS.includes(key) && k[key] !== undefined) d[key] = k[key];
    return d;
  }
  let S = fresh();
  const lv = (obj, id) => obj[id] || 0;
  function inc(k, n) {
    n = n || 1;
    S.stats[k] = (S.stats[k] || 0) + n;
    if (S.dq) S.dq.c[k] = (S.dq.c[k] || 0) + n;
  }
  const statVal = (k) => (k === 'allTime' ? S.allTime : S.stats[k] || 0);

  // ================= Даты =================
  const dayStr = (d) => `${d.getFullYear()}-${d.getMonth() + 1}-${d.getDate()}`;
  const today = () => dayStr(new Date());
  const yesterday = () => { const d = new Date(); d.setDate(d.getDate() - 1); return dayStr(d); };
  const dailyAvailable = () => S.daily.last !== today();
  const nextStreak = () => (S.daily.last === yesterday() ? (S.daily.streak % 7) + 1 : 1);
  const seasonKey = () => { const d = new Date(); return d.getFullYear() + '-' + (d.getMonth() + 1); };
  const MONTHS = ['Январь', 'Февраль', 'Март', 'Апрель', 'Май', 'Июнь', 'Июль', 'Август', 'Сентябрь', 'Октябрь', 'Ноябрь', 'Декабрь'];
  const seasonDaysLeft = () => { const d = new Date(); const end = new Date(d.getFullYear(), d.getMonth() + 1, 1); return Math.max(1, Math.ceil((end - d) / 86400000)); };

  // ================= Формулы =================
  const skin = () => SKINS[S.skin] || SKINS.classic;
  const boostOn = () => Date.now() < S.boostUntil;
  const petLv = (id) => S.pets[id] || 0;
  function stageIdx() { let i = 0; for (let k = 0; k < STAGES.length; k++) if (S.allTime >= STAGES[k].at) i = k; return i; }
  const globalMult = () => (1 + 0.1 * S.obsidian) * (1 + (skin().bonus || 0)) * (S.perm.x2 ? 2 : 1) * (boostOn() ? 2 : 1) * (1 + 0.05 * stageIdx());
  function genRateRaw(g, n) {
    if (!n) return 0;
    let m = 1;
    for (const t of MILESTONES) if (n >= t) m *= 2;
    return g.rate * n * m;
  }
  const genRate = (g) => genRateRaw(g, lv(S.gen, g.id)) * globalMult() * (1 + 0.06 * petLv('dragon'));
  function passive() { let s = 0; for (const g of GENS) s += genRate(g); return s; }
  const tapRaw = (n) => (1 + n * 0.8 + n * n * 0.12) * globalMult() * (1 + 0.08 * petLv('salamander'));
  const tapBase = () => tapRaw(lv(S.up, 'tap')) + passive() * 0.02;
  const critChance = () => lv(S.up, 'crit') * 0.025;
  const heatPerTap = () => 2.4 * (1 + 0.15 * lv(S.up, 'heat'));
  const eruptDur = () => 5 + lv(S.up, 'erupt');
  const magnetMult = () => 1 + 0.5 * lv(S.up, 'magnet');
  const comboMax = () => 3 + 0.5 * lv(S.up, 'combo');
  const comboMult = () => 1 + Math.min(combo / 60, comboMax() - 1);
  const genCost = (g) => Math.ceil(g.base * Math.pow(GEN_GROWTH, lv(S.gen, g.id)));
  const upCost = (u) => Math.ceil(u.base * Math.pow(u.mult, lv(S.up, u.id)));
  const petCost = (p) => Math.ceil(p.lava * Math.pow(7, petLv(p.id) - 1));
  const obsidianGain = () => Math.floor(Math.sqrt(S.total / 1e7));
  const offlineCapH = () => 2 + 0.5 * petLv('turtle');
  const offlineRate = () => 0.3 + 0.03 * petLv('turtle');
  const eagleChance = () => 0.06 * petLv('eagle');
  const bombValue = () => (tapBase() * 5 + passive() * 2.5) * magnetMult();
  const lavaFor = (min) => Math.max(100 * min, passive() * 60 * min);

  function fmt(n) {
    if (!isFinite(n)) return '∞';
    if (n < 1000) return n < 10 && n % 1 > 0.05 ? n.toFixed(1) : String(Math.floor(n));
    const u = ['K', 'M', 'B', 'T', 'Qa', 'Qi', 'Sx', 'Sp', 'Oc', 'No', 'Dc'];
    let i = -1;
    while (n >= 1000 && i < u.length - 1) { n /= 1000; i++; }
    return (n < 10 ? n.toFixed(2) : n < 100 ? n.toFixed(1) : Math.floor(n)) + u[i];
  }
  const mmss = (ms) => { const s = Math.max(0, Math.ceil(ms / 1000)); return Math.floor(s / 60) + ':' + String(s % 60).padStart(2, '0'); };

  let lastStage = -1;
  function earn(x) { S.lava += x; S.total += x; S.allTime += x; }

  // ================= Звуки (синтез, без файлов) =================
  let actx = null;
  function audio() {
    if (!S.sound) return null;
    try {
      if (!actx) actx = new (window.AudioContext || window.webkitAudioContext)();
      if (actx.state === 'suspended') actx.resume();
    } catch (e) { return null; }
    return actx;
  }
  function tone(freq, dur, type, vol, slideTo) {
    const a = audio(); if (!a) return;
    const t = a.currentTime, o = a.createOscillator(), g = a.createGain();
    o.type = type || 'sine';
    o.frequency.setValueAtTime(freq, t);
    if (slideTo) o.frequency.exponentialRampToValueAtTime(slideTo, t + dur);
    g.gain.setValueAtTime(vol || 0.1, t);
    g.gain.exponentialRampToValueAtTime(0.001, t + dur);
    o.connect(g); g.connect(a.destination);
    o.start(t); o.stop(t + dur + 0.02);
  }
  function noise(dur, vol, freq) {
    const a = audio(); if (!a) return;
    const t = a.currentTime, len = Math.floor(a.sampleRate * dur);
    const buf = a.createBuffer(1, len, a.sampleRate), d = buf.getChannelData(0);
    for (let i = 0; i < len; i++) d[i] = Math.random() * 2 - 1;
    const src = a.createBufferSource(), f = a.createBiquadFilter(), g = a.createGain();
    src.buffer = buf; f.type = 'lowpass'; f.frequency.value = freq || 500;
    g.gain.setValueAtTime(vol, t); g.gain.exponentialRampToValueAtTime(0.001, t + dur);
    src.connect(f); f.connect(g); g.connect(a.destination);
    src.start(t);
  }
  const later = (ms, fn) => setTimeout(fn, ms);
  const SFX = {
    tap: () => tone(170 + Math.random() * 40, 0.08, 'triangle', 0.12, 90),
    crit: () => tone(900, 0.14, 'square', 0.05, 1500),
    erupt: () => { noise(1.4, 0.35, 380); tone(70, 1.1, 'sawtooth', 0.1, 38); },
    bomb: () => tone(500, 0.1, 'sine', 0.14, 950),
    gem: () => { tone(1250, 0.08, 'sine', 0.09); later(70, () => tone(1650, 0.14, 'sine', 0.09)); },
    buy: () => { tone(660, 0.07, 'square', 0.05); later(60, () => tone(990, 0.1, 'square', 0.05)); },
    boss: () => { tone(120, 0.7, 'sawtooth', 0.12, 60); noise(0.6, 0.15, 250); },
    hit: () => tone(260, 0.05, 'square', 0.04, 140),
    win: () => [523, 659, 784, 1046].forEach((f, i) => later(i * 110, () => tone(f, 0.2, 'triangle', 0.12))),
    lose: () => [400, 300, 200].forEach((f, i) => later(i * 140, () => tone(f, 0.2, 'triangle', 0.1))),
    tick: () => tone(1500, 0.025, 'square', 0.035),
    meteor: () => tone(1600, 0.35, 'sine', 0.07, 400),
    level: () => [660, 880, 1320].forEach((f, i) => later(i * 90, () => tone(f, 0.15, 'sine', 0.1))),
  };
  const sfx = (k) => { try { SFX[k](); } catch (e) {} };

  // ================= Состояние сцены =================
  let heat = 0, erupting = 0, combo = 0, lastTap = -10, bump = 0, flash = 0, golemBob = 0;
  let bombs = [], parts = [], floats = [], meteor = null, cloud = null, boss = null;
  let bombT = 0, eventT = 25, bossT = 150, smokeT = 0, golemT = 0, time = 0;

  // ================= Холст =================
  const sceneEl = document.getElementById('scene');
  const cv = document.getElementById('c');
  const ctx = cv.getContext('2d');
  let W = 300, H = 300, DPR = 1, G = null, BASE = null, stars = [], streaks = [], trees = [], houses = [];

  function applyStage() {
    const k = 0.86 + 0.028 * stageIdx();
    const vh = BASE.vh * k, bw = BASE.bw * k;
    G = { cx: W / 2, gy: BASE.gy, vh, top: BASE.gy - vh, bw, cw: bw * 0.22 };
  }
  function resize() {
    const r = sceneEl.getBoundingClientRect();
    W = Math.max(1, r.width); H = Math.max(1, r.height);
    DPR = Math.min(window.devicePixelRatio || 1, 2);
    cv.width = Math.round(W * DPR); cv.height = Math.round(H * DPR);
    ctx.setTransform(DPR, 0, 0, DPR, 0, 0);
    const gy = H * 0.94;
    const vh = Math.min(H * 0.6, W * 0.78);
    BASE = { gy, vh, bw: Math.min(W * 1.02, vh * 2) };
    applyStage();
    stars = [];
    for (let i = 0; i < 60; i++) stars.push({ x: Math.random() * W, y: Math.random() * H * 0.6, r: Math.random() * 1.4 + 0.3, p: Math.random() * 6 });
    streaks = [];
    for (let i = 0; i < 8; i++) streaks.push({ off: (i / 7) * 1.6 - 0.8 + (Math.random() - 0.5) * 0.15, len: 0.35 + Math.random() * 0.5, wig: Math.random() * 2 - 1 });
    streaks.sort(() => Math.random() - 0.5);
    trees = [];
    for (let i = 0; i < 10; i++) {
      const side = i % 2 ? 1 : -1;
      trees.push({ x: W / 2 + side * (W * 0.3 + Math.random() * W * 0.2), s: 10 + Math.random() * 8 });
    }
    houses = [0.08, 0.92, 0.17, 0.83].map((f) => f * W);
  }
  new ResizeObserver(resize).observe(sceneEl);
  resize();

  // ================= Эффекты =================
  function lavaBurst(x, y, n, power) {
    const pc = skin().parts;
    for (let i = 0; i < n; i++) {
      if (parts.length > 400) parts.shift();
      parts.push({
        x, y, vx: (Math.random() - 0.5) * power * 0.9, vy: -power * (0.5 + Math.random() * 0.6),
        life: 0.8 + Math.random() * 0.5, max: 1.3, r: 2 + Math.random() * 4,
        c: pc[(Math.random() * pc.length) | 0], g: 1,
      });
    }
  }
  function sparkle(x, y, n, colors) {
    for (let i = 0; i < n; i++) {
      const a = Math.random() * Math.PI * 2, sp = 60 + Math.random() * 200;
      parts.push({ x, y, vx: Math.cos(a) * sp, vy: Math.sin(a) * sp, life: 0.6 + Math.random() * 0.4, max: 1, r: 2 + Math.random() * 3, c: colors[(Math.random() * colors.length) | 0], g: 0.4 });
    }
  }
  function smoke(x, y, big) {
    parts.push({ x: x + (Math.random() - 0.5) * 20, y, vx: (Math.random() - 0.5) * 15 + 8, vy: -30 - Math.random() * 25,
      life: 2.5, max: 2.5, r: (big ? 14 : 8) + Math.random() * 8, c: big ? '90,70,80' : '120,110,130', smoke: true, g: 0 });
  }
  function floatText(x, y, text, c, size) {
    floats.push({ x, y, text, c, size: size || 20, life: 1 });
    if (floats.length > 40) floats.shift();
  }

  // ================= Награды (общие для колеса, заданий, пропуска, друзей) =================
  function rewardText(r) {
    const out = [];
    if (r.gems) out.push(`💎 ${r.gems}`);
    if (r.lavaMin) out.push(`🔥 ${fmt(lavaFor(r.lavaMin))}`);
    if (r.boostMin) out.push(`⚡ ×2 на ${r.boostMin} мин`);
    if (r.xp) out.push(`🎟️ ${r.xp} XP`);
    if (r.pet) { const p = PETS.find((x) => x.id === r.pet); out.push(petLv(r.pet) ? `💎 40 (питомец уже есть)` : `${p.icon} ${p.name}`); }
    if (r.skin) out.push(S.skins[r.skin] ? `💎 60 (скин уже есть)` : `🎨 скин «${SKINS[r.skin].name}»`);
    return out.join(' + ');
  }
  function giveReward(r) {
    const txt = rewardText(r);
    if (r.gems) S.gems += r.gems;
    if (r.lavaMin) earn(lavaFor(r.lavaMin));
    if (r.boostMin) S.boostUntil = Math.max(Date.now(), S.boostUntil) + r.boostMin * 60000;
    if (r.xp) addXP(r.xp);
    if (r.pet) { if (petLv(r.pet)) S.gems += 40; else S.pets[r.pet] = 1; }
    if (r.skin) { if (S.skins[r.skin]) S.gems += 60; else { S.skins[r.skin] = true; S.skin = r.skin; } }
    return txt;
  }

  // ================= Боевой пропуск =================
  function ensureSeason() {
    const k = seasonKey();
    if (!S.bp || S.bp.season !== k) S.bp = { season: k, xp: 0, free: {}, prem: {}, premium: false };
  }
  const bpLevel = () => Math.min(BP_LEVELS, Math.floor(S.bp.xp / BP_XP));
  function bpReward(L, prem) {
    if (!prem) return L % 5 === 0 ? { gems: 2 } : { lavaMin: 5 + L };
    if (L === 10) return { pet: 'dragon' };
    if (L === 20) return { boostMin: 120 };
    if (L === 30) return { skin: 'flame' };
    return L % 5 === 0 ? { gems: 15 } : { gems: 3 };
  }
  function addXP(n) {
    ensureSeason();
    const before = bpLevel();
    S.bp.xp += n;
    const after = bpLevel();
    if (after > before) {
      floatText(W / 2, H * 0.35, `🎟️ Пропуск: уровень ${after}!`, '#c7b3ff', 22);
      sfx('level');
    }
  }
  const bpClaimable = () => {
    ensureSeason();
    const L = bpLevel();
    for (let i = 1; i <= L; i++) if (!S.bp.free[i] || (S.bp.premium && !S.bp.prem[i])) return true;
    return false;
  };

  // ================= Ежедневные задания =================
  function ensureDQ() {
    const t = today();
    if (S.dq && S.dq.day === t) return;
    const pool = DQ_POOL.filter((p) => !p.min || S.allTime >= p.min).sort(() => Math.random() - 0.5).slice(0, 3);
    S.dq = { day: t, c: {}, q: pool.map((p) => ({ id: p.id, goal: p.goals[(Math.random() * p.goals.length) | 0], done: false })), bonus: false };
    if (tab === 'tasks' && rows.length) buildList();
  }
  const dqProg = (q) => Math.min(q.goal, S.dq.c[q.id] || 0);
  const dqClaimable = () => { ensureDQ(); return S.dq.q.some((q) => !q.done && dqProg(q) >= q.goal) || (!S.dq.bonus && S.dq.q.every((q) => q.done)); };

  // ================= События =================
  function startEruption() {
    erupting = eruptDur();
    heat = 100;
    bombT = 0.3;
    flash = 1;
    inc('eruptions');
    addXP(3);
    haptic('heavy');
    sfx('erupt');
    floatText(G.cx, G.top - 40, '🌋 ИЗВЕРЖЕНИЕ!', '#ff7a1a', 26);
  }

  function spawnBomb() {
    bombs.push({
      x: G.cx + (Math.random() - 0.5) * G.cw * 0.6, y: G.top,
      vx: (Math.random() - 0.5) * W * 0.55, vy: -(H * 0.55 + Math.random() * H * 0.3),
      r: 22, t: 0, auto: Math.random() < eagleChance() ? 0.5 + Math.random() * 0.4 : 0,
    });
  }
  function collectBomb(b, byPet) {
    const v = bombValue();
    earn(v);
    inc('bombs');
    sparkle(b.x, b.y, 14, ['#ffde59', '#ff9a2e', '#fff']);
    floatText(b.x, b.y - 10, (byPet ? '🦅 +' : '+') + fmt(v), '#ffde59', 22);
    sfx('bomb');
  }

  function spawnEvent() {
    if (Math.random() < 0.6) {
      const fromLeft = Math.random() < 0.5;
      meteor = { x: fromLeft ? -40 : W + 40, y: H * (0.16 + Math.random() * 0.2), vx: (fromLeft ? 1 : -1) * W / 4.5, vy: H * 0.03, r: 32, left: fromLeft };
      sfx('meteor');
    } else {
      cloud = { x: G.cx, y: Math.max(80, G.top - 55), hp: 6, maxHp: 6, t: 15, r: 52, shake: 0 };
    }
  }

  function spawnBoss() {
    const k = BOSSES[(Math.random() * BOSSES.length) | 0];
    const hp = tapBase() * 1.4 * 150 * (1 + 0.1 * Math.min(S.bossLevel, 30));
    boss = { ...k, hp, maxHp: hp, t: 30, shake: 0 };
    haptic('heavy');
    sfx('boss');
    floatText(W / 2, H * 0.3, `${k.icon} ${k.name} напал! Тапай!`, '#9fd3ff', 22);
  }
  function hitBoss(dmg) {
    if (!boss) return;
    boss.hp -= dmg;
    boss.shake = 0.15;
    if (boss.hp <= 0) {
      const lava = passive() * 90 + tapBase() * 120;
      const gems = 1 + Math.min(2, Math.floor(S.bossLevel / 5));
      earn(lava);
      S.gems += gems;
      inc('bosses');
      addXP(60);
      S.bossLevel++;
      sparkle(W / 2, Math.max(70, G.top - 80), 50, ['#9fd3ff', '#ffffff', '#ffd166']);
      floatText(W / 2, H * 0.3, `🏆 Победа! +${fmt(lava)} 🔥 +${gems} 💎`, '#ffd166', 22);
      haptic('success');
      sfx('win');
      boss = null;
      bossT = 150 + Math.random() * 120;
      save(true);
    }
  }

  // ================= Тап =================
  const hit = (o, x, y, extra) => Math.hypot(o.x - x, o.y - y) < o.r + (extra || 14);

  function onTap(x, y) {
    audio();
    for (let i = bombs.length - 1; i >= 0; i--) {
      const b = bombs[i];
      if (hit(b, x, y)) { bombs.splice(i, 1); collectBomb(b); haptic('medium'); return; }
    }
    if (meteor && hit(meteor, x, y, 20)) {
      const v = passive() * 30 + tapBase() * 20;
      earn(v);
      inc('meteors');
      addXP(8);
      if (Math.random() < 0.1) { S.gems += 1; floatText(meteor.x, meteor.y + 30, '+1 💎', '#7df9ff', 20); }
      sparkle(meteor.x, meteor.y, 30, ['#9be7ff', '#ffffff', '#ffd166']);
      floatText(meteor.x, meteor.y, '☄️ +' + fmt(v), '#9be7ff', 24);
      meteor = null;
      haptic('success');
      sfx('gem');
      return;
    }
    if (cloud && hit(cloud, x, y, 10)) {
      cloud.hp--;
      cloud.shake = 0.25;
      haptic('light');
      sfx('hit');
      if (cloud.hp <= 0) {
        const v = passive() * 8 + tapBase() * 8;
        earn(v);
        inc('clouds');
        addXP(8);
        sparkle(cloud.x, cloud.y, 24, ['#cfe8ff', '#ffffff']);
        floatText(cloud.x, cloud.y, '☀️ +' + fmt(v), '#cfe8ff', 22);
        cloud = null;
        haptic('success');
      }
      return;
    }

    // Тап по вулкану
    const now = performance.now() / 1000;
    combo = now - lastTap < 0.7 ? combo + 1 : 1;
    lastTap = now;
    if (combo > (S.stats.maxCombo || 0)) S.stats.maxCombo = combo;
    if (S.dq && combo > (S.dq.c.maxCombo || 0)) S.dq.c.maxCombo = combo;
    inc('taps');
    let v = tapBase() * comboMult() * (erupting > 0 ? 3 : 1);
    const crit = Math.random() < critChance();
    if (crit) { v *= 10; inc('crits'); }
    earn(v);
    if (boss) hitBoss(v);
    else if (!cloud && erupting <= 0) {
      heat += heatPerTap();
      if (heat >= 100) startEruption();
    }
    bump = 1;
    lavaBurst(G.cx, G.top + 4, crit ? 16 : 4, H * 0.9);
    floatText(x + (Math.random() - 0.5) * 20, y - 24, (crit ? '✨ ' : '+') + fmt(v), crit ? '#7df9ff' : '#ffd27a', crit ? 26 : 19);
    haptic(crit ? 'medium' : 'light');
    sfx(boss ? 'hit' : crit ? 'crit' : 'tap');
  }

  function golemTap() {
    const v = tapBase();
    earn(v);
    if (boss) hitBoss(v * 0.5);
    else if (!cloud && erupting <= 0) {
      heat += heatPerTap() * 0.5;
      if (heat >= 100) startEruption();
    }
    bump = Math.max(bump, 0.5);
    golemBob = 1;
    lavaBurst(G.cx, G.top + 4, 2, H * 0.8);
  }

  cv.addEventListener('pointerdown', (e) => {
    e.preventDefault();
    const r = cv.getBoundingClientRect();
    onTap(e.clientX - r.left, e.clientY - r.top);
  });

  // ================= Обновление =================
  function update(dt) {
    time += dt;
    const now = performance.now() / 1000;

    let p = passive();
    if (erupting > 0) p *= 2;
    if (cloud) p *= 0.5;
    earn(p * dt);

    const st = stageIdx();
    if (st !== lastStage) {
      if (lastStage >= 0 && st > lastStage) {
        floatText(W / 2, H * 0.4, `🌋 Вулкан вырос: ${STAGES[st].name}!`, '#ffd166', 24);
        sparkle(W / 2, G.top, 60, skin().parts);
        sfx('level');
        haptic('success');
      }
      lastStage = st;
      applyStage();
    }

    if (erupting > 0) {
      erupting -= dt;
      bombT -= dt;
      if (bombT <= 0) { spawnBomb(); bombT = 0.3; }
      smokeT -= dt;
      if (smokeT <= 0) { smoke(G.cx, G.top, true); lavaBurst(G.cx, G.top + 4, 5, H * 1.1); smokeT = 0.08; }
      if (erupting <= 0) { erupting = 0; heat = 0; }
    } else {
      if (cloud) heat = Math.max(0, heat - 15 * dt);
      else if (now - lastTap > 0.6 && !S.perm.golem) heat = Math.max(0, heat - 7 * dt);
      smokeT -= dt;
      if (smokeT <= 0) { smoke(G.cx, G.top, false); smokeT = 0.9 - heat / 140; }
    }
    if (now - lastTap > 0.7) combo = 0;

    if (S.perm.golem) {
      golemT -= dt;
      if (golemT <= 0) { golemTap(); golemT = 0.25; }
    }
    golemBob = Math.max(0, golemBob - dt * 6);

    for (let i = bombs.length - 1; i >= 0; i--) {
      const b = bombs[i];
      b.vy += H * 0.5 * dt; b.x += b.vx * dt; b.y += b.vy * dt; b.t += dt;
      if (b.auto && b.t >= b.auto) { bombs.splice(i, 1); collectBomb(b, true); }
    }
    bombs = bombs.filter((b) => b.y < H + 40 && b.x > -40 && b.x < W + 40);

    // Метеориты и тучи
    eventT -= dt;
    if (eventT <= 0 && !meteor && !cloud && !boss && erupting <= 0) { spawnEvent(); eventT = 22 + Math.random() * 25; }
    if (meteor) {
      meteor.x += meteor.vx * dt; meteor.y += meteor.vy * dt;
      if (Math.random() < 0.6) parts.push({ x: meteor.x, y: meteor.y, vx: -meteor.vx * 0.2, vy: 0, life: 0.5, max: 0.5, r: 3 + Math.random() * 3, c: '#9be7ff', g: 0 });
      if (meteor.x < -60 || meteor.x > W + 60) meteor = null;
    }
    if (cloud) {
      cloud.t -= dt;
      cloud.shake = Math.max(0, cloud.shake - dt);
      cloud.x = G.cx + Math.sin(time * 0.8) * 20;
      if (cloud.t <= 0) cloud = null;
    }

    // Боссы
    if (!boss) {
      if (S.allTime >= 5000 && erupting <= 0 && !cloud && !meteor) {
        bossT -= dt;
        if (bossT <= 0) spawnBoss();
      }
    } else {
      boss.t -= dt;
      boss.shake = Math.max(0, boss.shake - dt);
      if (boss.t <= 0) {
        floatText(W / 2, H * 0.3, `${boss.icon} ${boss.name} ушёл…`, '#ff8a8a', 22);
        sfx('lose');
        boss = null;
        bossT = 150 + Math.random() * 120;
      }
    }

    for (const q of parts) {
      q.vy += H * 1.1 * q.g * dt; q.x += q.vx * dt; q.y += q.vy * dt; q.life -= dt;
      if (q.smoke) q.r += 8 * dt;
    }
    parts = parts.filter((q) => q.life > 0);
    for (const f of floats) { f.y -= 45 * dt; f.life -= dt; }
    floats = floats.filter((f) => f.life > 0);
    bump = Math.max(0, bump - dt * 7);
    flash = Math.max(0, flash - dt * 1.5);
  }

  // ================= Отрисовка =================
  const EMOJI_FONT = '"Apple Color Emoji","Segoe UI Emoji","Noto Color Emoji",sans-serif';
  function roundRect(x, y, w, h, r) {
    ctx.beginPath();
    ctx.moveTo(x + r, y); ctx.arcTo(x + w, y, x + w, y + h, r); ctx.arcTo(x + w, y + h, x, y + h, r);
    ctx.arcTo(x, y + h, x, y, r); ctx.arcTo(x, y, x + w, y, r); ctx.closePath();
  }
  function emoji(ch, x, y, size) {
    ctx.font = size + 'px ' + EMOJI_FONT;
    ctx.textAlign = 'center'; ctx.textBaseline = 'middle';
    ctx.fillText(ch, x, y);
  }
  function topBar(frac, label, colors) {
    const bx = 16, by = 12, bwid = W - 32, bh = 20;
    ctx.fillStyle = 'rgba(0,0,0,.45)'; roundRect(bx, by, bwid, bh, 10); ctx.fill();
    if (frac > 0) {
      const hg = ctx.createLinearGradient(bx, 0, bx + bwid, 0);
      colors.forEach((c, i) => hg.addColorStop(i / (colors.length - 1), c));
      ctx.fillStyle = hg; roundRect(bx, by, Math.max(bh, bwid * Math.min(1, frac)), bh, 10); ctx.fill();
    }
    ctx.fillStyle = '#fff'; ctx.font = '800 12px system-ui, sans-serif';
    ctx.textAlign = 'center'; ctx.textBaseline = 'middle';
    ctx.fillText(label, W / 2, by + bh / 2 + 1);
  }

  function draw() {
    const K = skin();
    const { cx, gy, vh, top, bw, cw } = G;
    const st = stageIdx();

    const sky = ctx.createLinearGradient(0, 0, 0, H);
    sky.addColorStop(0, K.sky[0]); sky.addColorStop(1, K.sky[1]);
    ctx.fillStyle = sky; ctx.fillRect(0, 0, W, H);
    if (erupting > 0 || flash > 0) {
      ctx.fillStyle = `rgba(${K.glow},${0.12 + 0.08 * Math.sin(time * 10) + flash * 0.35})`;
      ctx.fillRect(0, 0, W, H);
    }
    for (const s of stars) {
      ctx.globalAlpha = 0.4 + 0.4 * Math.sin(time * 1.5 + s.p);
      ctx.fillStyle = '#fff'; ctx.beginPath(); ctx.arc(s.x, s.y, s.r, 0, 7); ctx.fill();
    }
    ctx.globalAlpha = 1;

    if (S.skin === 'cosmic') {
      const px = W * 0.82, py = H * 0.22;
      ctx.fillStyle = '#7d5cff'; ctx.beginPath(); ctx.arc(px, py, 20, 0, 7); ctx.fill();
      ctx.strokeStyle = 'rgba(220,200,255,.8)'; ctx.lineWidth = 3;
      ctx.beginPath(); ctx.ellipse(px, py, 34, 8, -0.4, 0, 7); ctx.stroke();
    }

    // Рост: птицы и дальний вулкан
    if (st >= 3) {
      ctx.strokeStyle = 'rgba(20,10,20,.8)'; ctx.lineWidth = 2;
      for (let i = 0; i < 3; i++) {
        const bx = ((time * 30 + i * 140) % (W + 80)) - 40, by = H * (0.2 + i * 0.07) + Math.sin(time * 2 + i) * 6;
        const f = Math.sin(time * 8 + i) * 4;
        ctx.beginPath(); ctx.moveTo(bx - 7, by - f); ctx.lineTo(bx, by); ctx.lineTo(bx + 7, by - f); ctx.stroke();
      }
    }
    if (st >= 4) {
      ctx.fillStyle = 'rgba(0,0,0,.35)';
      ctx.beginPath(); ctx.moveTo(W * 0.62, gy); ctx.lineTo(W * 0.86, gy - vh * 0.45); ctx.lineTo(W * 0.92, gy - vh * 0.45); ctx.lineTo(W * 1.15, gy); ctx.fill();
    }

    const ga = 0.15 + heat / 100 * 0.45 + (erupting > 0 ? 0.3 : 0);
    const rg = ctx.createRadialGradient(cx, top, 0, cx, top, cw * 2.6);
    rg.addColorStop(0, `rgba(${K.glow},${ga})`); rg.addColorStop(1, `rgba(${K.glow},0)`);
    ctx.fillStyle = rg; ctx.fillRect(cx - cw * 3, top - cw * 3, cw * 6, cw * 6);
    if (st >= 5) {
      ctx.strokeStyle = `rgba(${K.glow},${0.35 + 0.2 * Math.sin(time * 3)})`; ctx.lineWidth = 4;
      ctx.beginPath(); ctx.ellipse(cx, top - 20, cw * 0.9, cw * 0.25, 0, 0, 7); ctx.stroke();
    }

    for (const q of parts) {
      if (!q.smoke) continue;
      ctx.fillStyle = `rgba(${q.c},${0.35 * q.life / q.max})`;
      ctx.beginPath(); ctx.arc(q.x, q.y, q.r, 0, 7); ctx.fill();
    }

    // Вулкан
    ctx.save();
    ctx.translate(cx, gy); ctx.scale(1 + 0.03 * bump, 1 - 0.045 * bump); ctx.translate(-cx, -gy);
    ctx.beginPath();
    ctx.moveTo(cx - bw / 2, gy);
    ctx.quadraticCurveTo(cx - bw * 0.18, gy - vh * 0.3, cx - cw / 2, top);
    ctx.quadraticCurveTo(cx, top + vh * 0.07, cx + cw / 2, top);
    ctx.quadraticCurveTo(cx + bw * 0.18, gy - vh * 0.3, cx + bw / 2, gy);
    ctx.closePath();
    const vg = ctx.createLinearGradient(0, top, 0, gy);
    vg.addColorStop(0, K.mnt[0]); vg.addColorStop(1, K.mnt[1]);
    ctx.fillStyle = vg; ctx.fill();

    ctx.fillStyle = erupting > 0 ? K.hi : K.lava;
    ctx.beginPath(); ctx.ellipse(cx, top + 3, cw / 2 * 0.92, Math.max(3, vh * 0.025), 0, 0, 7); ctx.fill();

    const n = erupting > 0 ? 8 : Math.round(heat / 100 * 6);
    ctx.lineCap = 'round';
    for (let i = 0; i < n; i++) {
      const s = streaks[i];
      const sx = cx + s.off * cw / 2, sy = top + 4;
      const k = s.len * (erupting > 0 ? 1 : 0.4 + heat / 160);
      const ex = sx + s.off * bw * 0.3 * k, ey = top + vh * k * 0.85;
      const qx = (sx + ex) / 2 + s.wig * 14, qy = (sy + ey) / 2;
      ctx.beginPath(); ctx.moveTo(sx, sy); ctx.quadraticCurveTo(qx, qy, ex, ey);
      ctx.strokeStyle = `rgba(${K.glow},.85)`; ctx.lineWidth = 6; ctx.stroke();
      ctx.strokeStyle = K.hi; ctx.lineWidth = 2; ctx.stroke();
    }
    ctx.restore();

    ctx.fillStyle = K.ground; ctx.fillRect(0, gy, W, H - gy);

    // Рост: деревья и домики
    if (st >= 1) {
      const count = st >= 3 ? trees.length : st >= 2 ? 6 : 4;
      for (let i = 0; i < count; i++) {
        const t = trees[i];
        ctx.fillStyle = '#3b2716'; ctx.fillRect(t.x - 1.5, gy - 5, 3, 5);
        ctx.fillStyle = '#1f6a3a';
        ctx.beginPath(); ctx.moveTo(t.x, gy - 5 - t.s * 1.6); ctx.lineTo(t.x - t.s / 2, gy - 4); ctx.lineTo(t.x + t.s / 2, gy - 4); ctx.fill();
      }
    }
    if (st >= 2) {
      const hc = st >= 4 ? 4 : 2;
      for (let i = 0; i < hc; i++) emoji(i % 2 ? '🛖' : '🏠', houses[i], gy - 11, 20);
    }

    if (S.perm.golem) emoji('🗿', cx - bw * 0.36, gy - 16 - golemBob * 6, 30);
    PETS.forEach((p, i) => {
      if (!petLv(p.id)) return;
      const px = cx + bw * (0.22 + i * 0.07) + Math.sin(time * 0.7 + i * 2) * 10;
      emoji(p.icon, Math.min(W - 16, px), gy - 12 - Math.abs(Math.sin(time * 3 + i)) * 4, 22);
    });

    for (const q of parts) {
      if (q.smoke) continue;
      ctx.globalAlpha = Math.max(0, Math.min(1, q.life / q.max * 1.5));
      ctx.fillStyle = q.c; ctx.beginPath(); ctx.arc(q.x, q.y, q.r, 0, 7); ctx.fill();
    }
    ctx.globalAlpha = 1;

    for (const b of bombs) {
      const pulse = 1 + 0.12 * Math.sin(b.t * 14);
      const bg = ctx.createRadialGradient(b.x, b.y, 0, b.x, b.y, b.r * 1.9);
      bg.addColorStop(0, `rgba(${K.glow},.8)`); bg.addColorStop(1, `rgba(${K.glow},0)`);
      ctx.fillStyle = bg; ctx.beginPath(); ctx.arc(b.x, b.y, b.r * 1.9, 0, 7); ctx.fill();
      ctx.fillStyle = '#2a1610'; ctx.beginPath(); ctx.arc(b.x, b.y, b.r * pulse, 0, 7); ctx.fill();
      ctx.fillStyle = K.lava;
      ctx.beginPath(); ctx.arc(b.x - 6, b.y - 4, 5, 0, 7); ctx.arc(b.x + 7, b.y + 3, 4, 0, 7); ctx.arc(b.x - 1, b.y + 8, 3, 0, 7); ctx.fill();
    }

    if (meteor) {
      ctx.save();
      ctx.translate(meteor.x, meteor.y);
      if (!meteor.left) ctx.scale(-1, 1);
      ctx.rotate(-0.9);
      emoji('☄️', 0, 0, 48);
      ctx.restore();
    }

    if (cloud) {
      const sx = cloud.shake > 0 ? (Math.random() - 0.5) * 10 : 0;
      ctx.strokeStyle = 'rgba(150,200,255,.6)'; ctx.lineWidth = 2;
      for (let i = 0; i < 8; i++) {
        const rx = cloud.x - 40 + i * 11, ry = cloud.y + 25 + ((time * 160 + i * 23) % 60);
        ctx.beginPath(); ctx.moveTo(rx, ry); ctx.lineTo(rx - 3, ry + 9); ctx.stroke();
      }
      emoji('☁️', cloud.x + sx, cloud.y, 84);
      ctx.fillStyle = 'rgba(0,0,0,.4)'; roundRect(cloud.x - 30, cloud.y - 52, 60, 7, 3); ctx.fill();
      ctx.fillStyle = '#9fd3ff'; roundRect(cloud.x - 30, cloud.y - 52, 60 * cloud.hp / cloud.maxHp, 7, 3); ctx.fill();
    }

    if (boss) {
      const bx = W / 2 + (boss.shake > 0 ? (Math.random() - 0.5) * 12 : 0);
      const by = Math.max(90, top - 70) + Math.sin(time * 2) * 6;
      emoji(boss.icon, bx, by, 78);
    }

    // Верхняя шкала
    if (boss) {
      topBar(boss.hp / boss.maxHp, `${boss.icon} ${boss.name} · ${Math.ceil(boss.t)}с`, ['#9fd3ff', '#4a90ff']);
    } else if (erupting > 0) {
      topBar(erupting / eruptDur(), '🌋 ИЗВЕРЖЕНИЕ · тапы ×3 · ' + erupting.toFixed(1) + 'с', ['#ffd166', '#ff7a1a', '#ff2e2e']);
    } else {
      topBar(heat / 100, cloud ? '🌧️ Дождь остужает вулкан: разгони тучу!' : 'Давление ' + Math.floor(heat) + '%', ['#ffd166', '#ff7a1a', '#ff2e2e']);
    }

    if (combo >= 5) {
      const m = comboMult();
      ctx.font = `900 ${18 + Math.min(10, combo / 10)}px system-ui, sans-serif`;
      ctx.fillStyle = m >= comboMax() - 0.01 ? '#ff4d4d' : '#ffd166';
      ctx.textAlign = 'left'; ctx.textBaseline = 'middle';
      ctx.fillText('⚡ Комбо ×' + m.toFixed(1), 18, 54);
    }

    ctx.textAlign = 'center'; ctx.textBaseline = 'middle';
    for (const f of floats) {
      ctx.globalAlpha = Math.max(0, Math.min(1, f.life * 1.6));
      ctx.font = `900 ${f.size}px system-ui, sans-serif`;
      ctx.lineWidth = 4; ctx.strokeStyle = 'rgba(0,0,0,.55)';
      ctx.strokeText(f.text, f.x, f.y);
      ctx.fillStyle = f.c; ctx.fillText(f.text, f.x, f.y);
    }
    ctx.globalAlpha = 1;
  }

  // ================= Интерфейс =================
  const $ = (id) => document.getElementById(id);
  const list = $('list');
  const TABS = [['up', '⛏️', 'Прокачка'], ['gen', '🌋', 'Добыча'], ['pets', '🐾', 'Питомцы'], ['tasks', '📜', 'Задания'], ['bp', '🎟️', 'Пропуск'], ['shop', '🛒', 'Магазин']];
  let tab = 'up';
  let rows = [];

  const tabsEl = $('tabs');
  for (const [id, icon, label] of TABS) {
    const b = document.createElement('button');
    b.dataset.tab = id;
    b.innerHTML = `<span class="ti">${icon}</span><span class="tl">${label}</span>`;
    b.addEventListener('click', () => openTab(id));
    tabsEl.appendChild(b);
  }
  function openTab(id) {
    tab = id;
    tabsEl.querySelectorAll('button').forEach((x) => x.classList.toggle('on', x.dataset.tab === id));
    buildList();
    list.scrollTop = 0;
  }
  $('gemChip').addEventListener('click', () => openTab('shop'));

  function setText(e, txt) { if (e.textContent !== txt) e.textContent = txt; }
  function section(title) { const d = document.createElement('div'); d.className = 'sec'; d.textContent = title; list.appendChild(d); }
  function note(html) { const d = document.createElement('div'); d.className = 'note'; d.innerHTML = html; list.appendChild(d); }

  function row(icon, onClick, withBar) {
    const el = document.createElement('div');
    el.className = 'item';
    el.innerHTML = `<div class="ic"></div><div class="info"><div class="nm"><span class="nmt"></span><span class="lv"></span></div><div class="ds"></div>${withBar ? '<div class="bar"><i></i></div>' : ''}</div><button class="buy"></button>`;
    const r = { el, ic: el.querySelector('.ic'), nm: el.querySelector('.nmt'), lv: el.querySelector('.lv'), ds: el.querySelector('.ds'), btn: el.querySelector('.buy'), bar: el.querySelector('.bar i') };
    r.ic.textContent = icon;
    r.btn.addEventListener('click', () => { audio(); onClick(); });
    list.appendChild(el);
    return r;
  }
  function setBtn(btn, label, disabled, kind) {
    setText(btn, label);
    btn.disabled = !!disabled;
    const cls = 'buy' + (kind ? ' ' + kind : '') + (btn.dataset.wide ? ' wide' : '');
    if (btn.className !== cls) btn.className = cls;
  }
  function setBar(r, frac) { if (r.bar) r.bar.style.width = Math.max(0, Math.min(100, frac * 100)) + '%'; }

  function upDesc(u, n) {
    switch (u.id) {
      case 'tap': return `За тап: ${fmt(tapRaw(n))} → ${fmt(tapRaw(n + 1))} (+2% от дохода/с)`;
      case 'crit': return `Шанс тапа ×10: ${(n * 2.5).toFixed(1)}%` + (n < u.max ? ` → ${((n + 1) * 2.5).toFixed(1)}%` : '');
      case 'heat': return `Давление за тап: ${heatPerTap().toFixed(1)}%. Извержения чаще`;
      case 'erupt': return `Извержение длится ${eruptDur()} с`;
      case 'magnet': return `Лавовые бомбы дают ×${magnetMult().toFixed(1)}`;
      case 'combo': return `Максимальное комбо: ×${comboMax().toFixed(1)}`;
    }
    return '';
  }

  function buildList() {
    list.innerHTML = '';
    rows = [];
    ({ up: buildUps, gen: buildGens, pets: buildPets, tasks: buildTasks, bp: buildBP, shop: buildShop })[tab]();
    refreshUI();
  }

  function buildUps() {
    const g = row('🌋', () => {}, true);
    g.btn.classList.add('hidden');
    rows.push(() => {
      const i = stageIdx(), next = STAGES[i + 1];
      setText(g.nm, 'Вулкан: ' + STAGES[i].name);
      setText(g.lv, 'ур. ' + (i + 1) + '/' + STAGES.length);
      setText(g.ds, `+${i * 5}% к лаве` + (next ? ` · растёт при ${fmt(next.at)} лавы за всё время` : ' · максимальный размер!'));
      setBar(g, next ? (Math.log10(S.allTime + 1) - Math.log10(STAGES[i].at + 1)) / (Math.log10(next.at + 1) - Math.log10(STAGES[i].at + 1)) : 1);
    });

    section('Улучшения');
    for (const u of UPS) {
      const r = row(u.icon, () => {
        const n = lv(S.up, u.id), cost = upCost(u);
        if (n >= u.max || S.lava < cost) return;
        S.lava -= cost; S.up[u.id] = n + 1; inc('buys');
        haptic('medium'); sfx('buy'); refreshUI(); save(false);
      });
      r.nm.textContent = u.name;
      rows.push(() => {
        const n = lv(S.up, u.id), maxed = n >= u.max, cost = upCost(u);
        setText(r.lv, n ? 'ур. ' + n : '');
        setText(r.ds, upDesc(u, n));
        setBtn(r.btn, maxed ? 'МАКС' : '🔥 ' + fmt(cost), maxed || S.lava < cost);
      });
    }

    section('Новый остров');
    const box = document.createElement('div');
    box.className = 'card';
    box.innerHTML = `
      <div>Обсидиан: <b id="obsCount">🖤 0</b></div>
      <p>Каждый обсидиан навсегда даёт <b>+10%</b> ко всей лаве. Начни на новом острове: лава, улучшения и источники сбросятся. Остальное останется.</p>
      <p id="obsGain"></p>
      <button class="buy wide" id="prestigeBtn">🏝️ Новый остров</button>`;
    list.appendChild(box);
    $('prestigeBtn').addEventListener('click', askPrestige);
    rows.push(() => {
      const gain = obsidianGain();
      setText($('obsCount'), '🖤 ' + S.obsidian);
      $('obsGain').innerHTML = gain > 0
        ? `Сейчас получишь: <b>+${gain}</b> обсидиана`
        : `Нужно накопить <b>10M</b> лавы на этом острове (сейчас ${fmt(S.total)})`;
      $('prestigeBtn').disabled = gain <= 0;
    });
  }

  function buildGens() {
    GENS.forEach((g, i) => {
      const r = row(g.icon, () => {
        const n = lv(S.gen, g.id), cost = genCost(g);
        if (S.lava < cost) return;
        S.lava -= cost; S.gen[g.id] = n + 1; inc('buys');
        if (MILESTONES.includes(n + 1)) floatText(W / 2, H * 0.4, `${g.icon} ${g.name}: доход ×2!`, '#ffd166', 20);
        haptic('medium'); sfx('buy'); refreshUI(); save(false);
      });
      rows.push(() => {
        const n = lv(S.gen, g.id);
        const locked = i > 0 && lv(S.gen, GENS[i - 1].id) === 0;
        r.el.classList.toggle('locked', locked);
        setText(r.ic, locked ? '🔒' : g.icon);
        setText(r.nm, locked ? '???' : g.name);
        setText(r.lv, n ? 'ур. ' + n : '');
        const next = MILESTONES.find((t) => n < t);
        const each = g.rate * globalMult() * (1 + 0.06 * petLv('dragon'));
        setText(r.ds, locked ? `Купи «${GENS[i - 1].name}», чтобы открыть`
          : `+${fmt(each)}/с за шт.` + (n ? ` · сейчас ${fmt(genRate(g))}/с` : '') + (next ? ` · ×2 на ${next} ур.` : ''));
        const cost = genCost(g);
        setBtn(r.btn, '🔥 ' + fmt(cost), locked || S.lava < cost);
      });
    });
  }

  function petDesc(p, l) {
    const n = Math.max(1, l);
    switch (p.id) {
      case 'salamander': return `+${n * 8}% к тапам` + (l && l < PET_MAX ? ` → +${(n + 1) * 8}%` : '');
      case 'dragon': return `+${n * 6}% к доходу источников` + (l && l < PET_MAX ? ` → +${(n + 1) * 6}%` : '');
      case 'eagle': return `Сам ловит ${n * 6}% лавовых бомб` + (l && l < PET_MAX ? ` → ${(n + 1) * 6}%` : '');
      case 'turtle': return `Доход вне игры: ${Math.round((0.3 + 0.03 * n) * 100)}%, до ${2 + 0.5 * n} ч`;
    }
    return '';
  }
  function buildPets() {
    note('Питомцы живут у подножия вулкана и помогают тебе. Открываются за 💎, прокачиваются за лаву.');
    for (const p of PETS) {
      const r = row(p.icon, () => {
        const l = petLv(p.id);
        if (!l) {
          if (S.gems < p.gems) return;
          S.gems -= p.gems; S.pets[p.id] = 1;
          sparkle(W / 2, G.gy - 20, 30, ['#ffd166', '#ffffff']);
          floatText(W / 2, H * 0.4, `${p.icon} ${p.name} с тобой!`, '#ffd166', 22);
          sfx('win'); haptic('success'); save(true); refreshUI();
          return;
        }
        if (l >= PET_MAX) return;
        const cost = petCost(p);
        if (S.lava < cost) return;
        S.lava -= cost; S.pets[p.id] = l + 1; inc('buys');
        sfx('buy'); haptic('medium'); save(false); refreshUI();
      });
      r.nm.textContent = p.name;
      rows.push(() => {
        const l = petLv(p.id);
        setText(r.lv, l ? 'ур. ' + l : '');
        setText(r.ds, petDesc(p, l));
        if (!l) setBtn(r.btn, '💎 ' + p.gems, S.gems < p.gems, 'gem');
        else if (l >= PET_MAX) setBtn(r.btn, 'МАКС', true);
        else setBtn(r.btn, '🔥 ' + fmt(petCost(p)), S.lava < petCost(p));
      });
    }
  }

  function claimableAch(a) {
    const tier = S.ach[a.id] || 0;
    return tier < a.goals.length && statVal(a.stat) >= a.goals[tier];
  }
  const refsClaimable = () => S.refs > S.refsClaimed || REF_MILESTONES.some((m) => S.refs >= m.n && !S.refMs[m.n]);

  function buildTasks() {
    section('Каждый день');
    const d = row('📅', () => { if (dailyAvailable()) showDaily(); });
    d.nm.textContent = 'Награда за вход';
    rows.push(() => {
      const av = dailyAvailable();
      setText(d.lv, av ? 'день ' + nextStreak() : 'серия ' + S.daily.streak);
      setText(d.ds, av ? 'Награда ждёт! Не пропускай дни' : 'Приходи завтра за следующей');
      d.el.classList.toggle('hot', av);
      setBtn(d.btn, av ? 'Забрать' : 'Завтра', !av, 'ok');
    });

    ensureDQ();
    S.dq.q.forEach((q) => {
      const def = DQ_POOL.find((p) => p.id === q.id);
      const r = row('🎯', () => {
        if (q.done || dqProg(q) < q.goal) return;
        q.done = true;
        const txt = giveReward({ xp: 50, lavaMin: 10 });
        floatText(W / 2, H * 0.45, txt, '#ffd166', 20);
        sfx('gem'); haptic('success'); save(true); refreshUI();
      }, true);
      r.nm.textContent = def ? def.text(q.goal) : q.id;
      rows.push(() => {
        const p = dqProg(q);
        setText(r.ds, `${p} / ${q.goal} · награда: 🎟️ 50 XP + 🔥 10 мин`);
        setBar(r, p / q.goal);
        r.el.classList.toggle('hot', !q.done && p >= q.goal);
        setBtn(r.btn, q.done ? '✓' : 'Забрать', q.done || p < q.goal, 'ok');
      });
    });
    const b = row('🎁', () => {
      if (S.dq.bonus || !S.dq.q.every((q) => q.done)) return;
      S.dq.bonus = true;
      const txt = giveReward({ gems: 2, xp: 50 });
      floatText(W / 2, H * 0.45, txt, '#7df9ff', 22);
      sfx('win'); haptic('success'); save(true); refreshUI();
    });
    b.nm.textContent = 'Все задания дня';
    rows.push(() => {
      const all = S.dq.q.every((q) => q.done);
      setText(b.ds, 'Выполни все 3 задания: 💎 2 + 🎟️ 50 XP. Новые задания каждый день');
      b.el.classList.toggle('hot', all && !S.dq.bonus);
      setBtn(b.btn, S.dq.bonus ? '✓' : 'Забрать', S.dq.bonus || !all, 'gem');
    });

    section('Друзья');
    const f = row('👥', inviteFriend);
    f.nm.textContent = 'Пригласи друга';
    rows.push(() => {
      setText(f.lv, 'приглашено: ' + S.refs);
      setText(f.ds, `За каждого друга 💎 ${REF_GEMS}, другу подарок 🎁. За 3 друзей ещё 💎 30, за 10 друзей 💎 100`);
      setBtn(f.btn, 'Позвать', false, 'ok');
    });
    const fr = row('🎁', () => {
      let gems = 0;
      if (S.refs > S.refsClaimed) { gems += (S.refs - S.refsClaimed) * REF_GEMS; S.refsClaimed = S.refs; }
      for (const m of REF_MILESTONES) if (S.refs >= m.n && !S.refMs[m.n]) { S.refMs[m.n] = true; gems += m.gems; }
      if (!gems) return;
      S.gems += gems;
      floatText(W / 2, H * 0.45, `+${gems} 💎 за друзей`, '#7df9ff', 24);
      sfx('win'); haptic('success'); save(true); refreshUI();
    });
    fr.nm.textContent = 'Награды за друзей';
    rows.push(() => {
      const can = refsClaimable();
      setText(fr.ds, can ? 'Твои друзья пришли в игру!' : 'Награды обновляются, когда открываешь игру из сообщения бота');
      fr.el.classList.toggle('hot', can);
      setBtn(fr.btn, 'Забрать', !can, 'gem');
    });

    section('Достижения');
    for (const a of ACH) {
      const r = row(a.icon, () => {
        if (!claimableAch(a)) return;
        const tier = S.ach[a.id] || 0;
        S.gems += a.gems[tier];
        S.ach[a.id] = tier + 1;
        haptic('success'); sfx('gem');
        floatText(W / 2, H * 0.45, '+' + a.gems[tier] + ' 💎', '#7df9ff', 26);
        save(true); refreshUI();
      }, true);
      r.nm.textContent = a.name;
      rows.push(() => {
        const tier = S.ach[a.id] || 0, done = tier >= a.goals.length;
        const val = statVal(a.stat);
        setText(r.lv, `${Math.min(tier + (done ? 0 : 1), a.goals.length)}/${a.goals.length}`);
        if (done) {
          setText(r.ds, 'Все уровни пройдены ✓');
          setBar(r, 1);
          setBtn(r.btn, '✓', true, 'ok');
          r.el.classList.remove('hot');
          return;
        }
        const goal = a.goals[tier];
        setText(r.ds, `${fmt(Math.min(val, goal))} / ${fmt(goal)}`);
        setBar(r, val / goal);
        const can = val >= goal;
        r.el.classList.toggle('hot', can);
        setBtn(r.btn, (can ? 'Забрать ' : '') + '💎' + a.gems[tier], !can, 'gem');
      });
    }
  }

  function buildBP() {
    ensureSeason();
    const box = document.createElement('div');
    box.className = 'card';
    box.innerHTML = `
      <div>🎟️ Сезон: <b id="bpName"></b> · осталось <b id="bpDays"></b> дн.</div>
      <div class="big" id="bpLvl"></div>
      <div class="bar"><i id="bpBar"></i></div>
      <p id="bpXp"></p>
      <p>XP дают задания дня, боссы, извержения, метеориты и колесо.</p>
      <button class="buy star wide" id="bpBuy"></button>`;
    list.appendChild(box);
    const bpProduct = PRODUCTS.find((p) => p.id === 'bp');
    $('bpBuy').addEventListener('click', () => { audio(); buyStars(bpProduct); });
    rows.push(() => {
      const L = bpLevel();
      setText($('bpName'), MONTHS[new Date().getMonth()]);
      setText($('bpDays'), String(seasonDaysLeft()));
      setText($('bpLvl'), 'Уровень ' + L + ' / ' + BP_LEVELS);
      $('bpBar').style.width = (L >= BP_LEVELS ? 100 : (S.bp.xp % BP_XP)) + '%';
      setText($('bpXp'), L >= BP_LEVELS ? 'Пропуск пройден полностью! 🔥' : `${S.bp.xp % BP_XP} / ${BP_XP} XP до следующего уровня`);
      setText($('bpBuy'), S.bp.premium ? '✓ Премиум активен' : `⭐ ${bpProduct.stars} · Открыть премиум`);
      $('bpBuy').disabled = !!S.bp.premium;
    });
    for (let L = 1; L <= BP_LEVELS; L++) {
      const r = row(String(L), () => {
        if (bpLevel() < L) return;
        const got = [];
        if (!S.bp.free[L]) { S.bp.free[L] = true; got.push(giveReward(bpReward(L, false))); }
        if (S.bp.premium && !S.bp.prem[L]) { S.bp.prem[L] = true; got.push(giveReward(bpReward(L, true))); }
        if (!got.length) return;
        floatText(W / 2, H * 0.45, got.join(' + '), '#ffd166', 18);
        sfx('gem'); haptic('success'); save(true); refreshUI();
      });
      r.nm.textContent = 'Уровень ' + L;
      r.ic.style.background = 'var(--card2)';
      r.ic.style.fontSize = '18px';
      r.ds.style.whiteSpace = 'pre-line';
      rows.push(() => {
        const open = bpLevel() >= L;
        const freeDone = S.bp.free[L], premDone = S.bp.prem[L];
        setText(r.ds, `Бесплатно: ${rewardText(bpReward(L, false))}${freeDone ? ' ✓' : ''}\n` +
          `Премиум: ${rewardText(bpReward(L, true))}${premDone ? ' ✓' : S.bp.premium ? '' : ' 🔒'}`);
        const can = open && (!freeDone || (S.bp.premium && !premDone));
        r.el.classList.toggle('hot', can);
        r.el.classList.toggle('locked', !open);
        setBtn(r.btn, can ? 'Забрать' : open ? '✓' : '🔒', !can, 'ok');
      });
    }
  }

  function buildShop() {
    section('Скины');
    for (const id of Object.keys(SKINS)) {
      const K = SKINS[id], u = K.unlock;
      const product = u.stars ? PRODUCTS.find((p) => p.id === u.stars) : null;
      const r = row('', () => {
        if (S.skins[id]) { S.skin = id; sfx('buy'); save(false); refreshUI(); return; }
        if (u.gems) {
          if (S.gems < u.gems) return;
          S.gems -= u.gems; S.skins[id] = true; S.skin = id;
          haptic('success'); sfx('win'); sparkle(W / 2, G.top, 40, K.parts); save(true); refreshUI();
        } else if (u.stat) {
          if (statVal(u.stat) < u.need) return;
          S.skins[id] = true; S.skin = id; haptic('success'); sfx('win'); save(true); refreshUI();
        } else if (u.bp) {
          openTab('bp');
        } else if (product) {
          buyStars(product);
        }
      });
      r.ic.style.background = `radial-gradient(circle at 50% 36%, ${K.lava} 0 13%, transparent 14%), linear-gradient(180deg, ${K.sky[0]} 0 36%, ${K.mnt[0]} 36% 100%)`;
      r.nm.textContent = K.name;
      rows.push(() => {
        const owned = !!S.skins[id], on = S.skin === id;
        r.el.classList.toggle('hot', on);
        let d = K.bonus ? `+${Math.round(K.bonus * 100)}% к лаве, пока надет` : 'Только внешний вид';
        if (!owned && u.stat) d += ` · ${u.text} (${Math.min(statVal(u.stat), u.need)}/${u.need})`;
        if (!owned && u.bp) d += ' · награда премиум-пропуска, ур. 30';
        if (!owned && product) d += ' · эксклюзив';
        setText(r.ds, d);
        if (on) setBtn(r.btn, 'Надет', true, 'ok');
        else if (owned) setBtn(r.btn, 'Надеть', false, 'ok');
        else if (u.gems) setBtn(r.btn, '💎 ' + u.gems, S.gems < u.gems, 'gem');
        else if (u.stat) setBtn(r.btn, statVal(u.stat) >= u.need ? 'Забрать' : '🔒', statVal(u.stat) < u.need, 'ok');
        else if (u.bp) setBtn(r.btn, '🎟️', false, 'ok');
        else if (product) setBtn(r.btn, '⭐ ' + product.stars, false, 'star');
      });
    }

    section('За алмазы 💎');
    for (const it of GEM_ITEMS) {
      const r = row(it.icon, () => {
        if (S.gems < it.cost || !gemItemOk(it)) return;
        S.gems -= it.cost;
        if (it.id === 'boost') S.boostUntil = Math.max(Date.now(), S.boostUntil) + 30 * 60000;
        if (it.id === 'erupt') startEruption();
        if (it.id === 'time') { const v = passive() * 7200; earn(v); floatText(W / 2, H * 0.4, '+' + fmt(v) + ' 🔥', '#ffd166', 26); }
        haptic('success'); sfx('buy'); save(true); refreshUI();
      });
      r.nm.textContent = it.name;
      rows.push(() => {
        let d = '';
        if (it.id === 'boost') d = boostOn() ? `Активен ещё ${mmss(S.boostUntil - Date.now())}. Купи ещё, чтобы продлить` : 'Вся лава ×2';
        if (it.id === 'erupt') d = erupting > 0 || boss ? 'Сейчас нельзя' : 'Тапы ×3 и лавовые бомбы';
        if (it.id === 'time') d = passive() > 0 ? '≈ 🔥 ' + fmt(passive() * 7200) : 'Сначала купи источники';
        setText(r.ds, d);
        setBtn(r.btn, '💎 ' + it.cost, S.gems < it.cost || !gemItemOk(it), 'gem');
      });
    }

    section('Донат · Telegram Stars ⭐');
    for (const p of PRODUCTS) {
      const r = row(p.icon, () => buyStars(p));
      r.nm.textContent = p.name;
      r.ds.textContent = p.desc;
      if (p.id === 'starter') r.el.classList.add('hot');
      rows.push(() => {
        const owned = productOwned(p);
        setBtn(r.btn, owned ? 'Куплено' : '⭐ ' + p.stars, owned, owned ? 'ok' : 'star');
      });
    }
    note(inTelegram ? 'Оплата проходит внутри Telegram через Stars.' : (TEST_PURCHASES ? 'Игра открыта не в Telegram: покупки работают в тестовом режиме, бесплатно.' : 'Покупки доступны только в Telegram.'));
  }
  const gemItemOk = (it) => (it.id === 'erupt' ? erupting <= 0 && !boss : it.id === 'time' ? passive() > 0 : true);
  const productOwned = (p) => (p.season ? (ensureSeason(), !!S.bp.premium) : !!(p.once && S.bought[p.id]));

  function refreshUI() {
    setText($('lava'), '🔥 ' + fmt(S.lava));
    let p = passive();
    if (erupting > 0) p *= 2;
    if (cloud) p *= 0.5;
    setText($('rates'), `+${fmt(p)}/с · за тап ${fmt(tapBase())}`);
    setText($('obsTop'), S.obsidian > 0 ? `🖤 ${S.obsidian} обсидиана · +${S.obsidian * 10}%` : '');
    setText($('gemChip'), '💎 ' + S.gems);
    const bc = $('boostChip');
    bc.classList.toggle('hidden', !boostOn());
    if (boostOn()) setText(bc, '⚡×2 ' + mmss(S.boostUntil - Date.now()));
    setText($('soundBtn'), S.sound ? '🔊' : '🔇');
    $('wheelBtn').classList.toggle('dotted', wheelFree());
    const taskDot = dailyAvailable() || ACH.some(claimableAch) || dqClaimable() || refsClaimable();
    tabsEl.querySelector('[data-tab=tasks]').classList.toggle('dot', taskDot);
    tabsEl.querySelector('[data-tab=bp]').classList.toggle('dot', bpClaimable());
    for (const f of rows) f();
  }

  // ================= Друзья =================
  function inviteLink() { return S.uid ? `https://t.me/${BOT_USERNAME}?start=ref_${S.uid}` : `https://t.me/${BOT_USERNAME}`; }
  function inviteFriend() {
    const link = inviteLink();
    const text = 'Залетай в Лавапул! 🌋 Тапай вулкан, лови лавовые бомбы и побеждай боссов. По моей ссылке получишь подарок 🎁';
    const share = `https://t.me/share/url?url=${encodeURIComponent(link)}&text=${encodeURIComponent(text)}`;
    if (inTelegram) { try { tg.openTelegramLink(share); return; } catch (e) {} }
    try { navigator.clipboard.writeText(link); } catch (e) {}
    modal('👥 Пригласи друга', `Отправь другу ссылку:<br><b style="word-break:break-all">${link}</b>`, [{ text: 'Ок' }]);
  }
  function readLinkParams() {
    let q;
    try { q = new URLSearchParams(location.search); } catch (e) { return; }
    if (!S.uid || String(q.get('uid')) !== String(S.uid)) return;
    const refs = parseInt(q.get('refs') || '0', 10);
    if (refs > S.refs && refs < 10000) S.refs = refs;
    if (q.get('welcome') === '1' && !S.welcomeDone) {
      S.welcomeDone = true;
      const txt = giveReward({ gems: 10, lavaMin: 30 });
      modal('🎁 Подарок от друга!', `Тебя пригласил друг, держи подарок:<br><b>${txt}</b>`, [{ text: 'Спасибо!' }]);
    }
  }

  // ================= Колесо удачи =================
  const wheelFree = () => S.wheel.day !== today();
  const wheelExtraLeft = () => WHEEL_EXTRA_MAX - (S.wheel.xday === today() ? S.wheel.extra : 0);
  const wc = $('wc'), wctx = wc.getContext('2d');
  let wheelRot = 0, spinning = false;
  $('spinBtn').dataset.wide = '1';
  function drawWheel() {
    const R = 250, c = 260, seg = (Math.PI * 2) / WHEEL.length;
    wctx.clearRect(0, 0, 520, 520);
    WHEEL.forEach((s, i) => {
      const a0 = wheelRot + i * seg - Math.PI / 2;
      wctx.beginPath(); wctx.moveTo(c, c); wctx.arc(c, c, R, a0, a0 + seg); wctx.closePath();
      wctx.fillStyle = s.c; wctx.fill();
      wctx.strokeStyle = '#1d1428'; wctx.lineWidth = 6; wctx.stroke();
      wctx.save();
      wctx.translate(c, c); wctx.rotate(a0 + seg / 2);
      wctx.fillStyle = '#1b0f05'; wctx.font = '900 30px system-ui, sans-serif';
      wctx.textAlign = 'right'; wctx.textBaseline = 'middle';
      wctx.fillText(s.label, R - 22, 0);
      wctx.restore();
    });
    wctx.beginPath(); wctx.arc(c, c, 40, 0, 7); wctx.fillStyle = '#1d1428'; wctx.fill();
    wctx.font = '44px ' + EMOJI_FONT; wctx.textAlign = 'center'; wctx.textBaseline = 'middle';
    wctx.fillText('🌋', c, c + 2);
  }
  function refreshWheelBtn() {
    const b = $('spinBtn');
    if (spinning) { b.disabled = true; return; }
    if (wheelFree()) setBtn(b, '🎡 Крутить бесплатно', false, 'ok');
    else if (wheelExtraLeft() > 0) setBtn(b, `🎡 Ещё раз за 💎${WHEEL_EXTRA_COST} (осталось ${wheelExtraLeft()})`, S.gems < WHEEL_EXTRA_COST, 'gem');
    else setBtn(b, 'Приходи завтра', true);
  }
  function openWheel() {
    audio();
    $('wres').textContent = wheelFree() ? 'Бесплатное вращение каждый день!' : '';
    drawWheel(); refreshWheelBtn();
    $('wheel').classList.remove('hidden');
  }
  function spin() {
    if (spinning) return;
    if (wheelFree()) { S.wheel.day = today(); }
    else {
      if (wheelExtraLeft() <= 0 || S.gems < WHEEL_EXTRA_COST) return;
      if (S.wheel.xday !== today()) { S.wheel.xday = today(); S.wheel.extra = 0; }
      S.gems -= WHEEL_EXTRA_COST; S.wheel.extra++;
    }
    inc('spins');
    addXP(10);
    save(true);
    const total = WHEEL.reduce((s, x) => s + x.w, 0);
    let roll = Math.random() * total, idx = 0;
    for (let i = 0; i < WHEEL.length; i++) { roll -= WHEEL[i].w; if (roll <= 0) { idx = i; break; } }
    const seg = (Math.PI * 2) / WHEEL.length, TAU = Math.PI * 2;
    const jitter = (Math.random() - 0.5) * seg * 0.6;
    const target = ((-(idx + 0.5) * seg + jitter) % TAU + TAU) % TAU;
    const start = wheelRot;
    const startMod = ((start % TAU) + TAU) % TAU;
    const end = start + TAU * 6 + ((target - startMod + TAU) % TAU);
    const dur = 4200, t0 = performance.now();
    let lastSeg = -1;
    spinning = true; refreshWheelBtn();
    $('wres').textContent = '';
    haptic('medium');
    function step(now) {
      const k = Math.min(1, (now - t0) / dur);
      const e = 1 - Math.pow(1 - k, 3);
      wheelRot = start + (end - start) * e;
      const cur = Math.floor((((-wheelRot) % TAU) + TAU) % TAU / seg);
      if (cur !== lastSeg) { lastSeg = cur; sfx('tick'); haptic('light'); }
      drawWheel();
      if (k < 1) { requestAnimationFrame(step); return; }
      spinning = false;
      const txt = giveReward(WHEEL[idx].r);
      $('wres').textContent = '🎉 ' + txt;
      sfx('win'); haptic('success');
      save(true); refreshWheelBtn(); refreshUI();
    }
    requestAnimationFrame(step);
  }
  $('wheelBtn').addEventListener('click', openWheel);
  $('spinBtn').addEventListener('click', () => { audio(); spin(); });
  $('wclose').addEventListener('click', () => { if (!spinning) $('wheel').classList.add('hidden'); });

  // ================= Покупки за Stars =================
  function buyStars(p) {
    if (productOwned(p)) return;
    const link = INVOICE_LINKS[p.id];
    if (inTelegram && link && tgOk('6.1')) {
      tg.openInvoice(link, (status) => {
        if (status === 'paid') grant(p);
        else if (status === 'failed') modal('Оплата не прошла', 'Попробуй ещё раз чуть позже.', [{ text: 'Ок' }]);
      });
      return;
    }
    if (inTelegram) { modal('Магазин скоро откроется', 'Этот товар ещё настраивается. Загляни позже!', [{ text: 'Ок' }]); return; }
    if (!TEST_PURCHASES) { modal('Только в Telegram', 'Покупки работают, когда игра открыта через бота @' + BOT_USERNAME, [{ text: 'Ок' }]); return; }
    modal('🧪 Тестовая покупка', `Игра открыта не в Telegram, поэтому оплата не настоящая.<br>Выдать «${p.name}» бесплатно, чтобы проверить?`, [
      { text: 'Отмена', ghost: true },
      { text: 'Выдать', fn: () => grant(p) },
    ]);
  }

  function grant(p) {
    const g = p.grant;
    const got = [];
    if (g.gems) { S.gems += g.gems; got.push(`💎 ${g.gems} алмазов`); }
    if (g.skin) { S.skins[g.skin] = true; S.skin = g.skin; got.push(`🎨 скин «${SKINS[g.skin].name}»`); }
    if (g.perm) { S.perm[g.perm] = true; got.push(p.icon + ' ' + p.name); }
    if (g.hours) { const v = Math.max(1000, passive() * 3600 * g.hours); earn(v); got.push(`🔥 ${fmt(v)} лавы`); }
    if (g.bp) { ensureSeason(); S.bp.premium = true; got.push('🎟️ Премиум-пропуск сезона'); }
    if (p.once) S.bought[p.id] = true;
    save(true);
    haptic('success'); sfx('win');
    sparkle(W / 2, H * 0.4, 60, ['#ffcc33', '#ffffff', '#7df9ff']);
    buildList();
    modal('🎉 Спасибо за покупку!', 'Ты получил:<br><b>' + got.join('<br>') + '</b>', [{ text: 'Круто!' }]);
  }

  // ================= Окна =================
  const modalQueue = [];
  let modalOpen = false;
  function modal(title, html, buttons) {
    modalQueue.push({ title, html, buttons });
    if (!modalOpen) nextModal();
  }
  function nextModal() {
    const m = modalQueue.shift();
    if (!m) { modalOpen = false; $('modal').classList.add('hidden'); return; }
    modalOpen = true;
    $('modalTitle').textContent = m.title;
    $('modalBody').innerHTML = m.html;
    const box = $('modalBtns');
    box.innerHTML = '';
    for (const b of m.buttons) {
      const el = document.createElement('button');
      el.className = 'buy' + (b.ghost ? ' ghost' : '');
      el.textContent = b.text;
      el.addEventListener('click', () => { audio(); if (b.fn) b.fn(); nextModal(); });
      box.appendChild(el);
    }
    $('modal').classList.remove('hidden');
  }

  function showHelp() {
    modal('🌋 Лавапул: как играть', `
      <ul>
        <li><b>Тапай по вулкану</b>, чтобы добывать лаву 🔥. Без пауз растёт комбо</li>
        <li>Заполни <b>шкалу давления</b>, и начнётся извержение: лови <b>лавовые бомбы</b>!</li>
        <li>Иногда нападают <b>боссы</b>: победи их тапами за 30 секунд</li>
        <li>Лови <b>метеориты ☄️</b> и разгоняй <b>тучи ☁️</b></li>
        <li>Крути <b>колесо удачи 🎡</b> каждый день бесплатно</li>
        <li>Выполняй <b>задания дня</b> и прокачивай <b>пропуск 🎟️</b></li>
        <li>Заводи <b>питомцев 🐾</b> и зови <b>друзей</b> за алмазы</li>
        <li>Чем больше лавы за всё время, тем <b>больше растёт вулкан</b></li>
      </ul>`, [{ text: 'Понятно!' }]);
  }
  $('helpBtn').addEventListener('click', showHelp);
  $('soundBtn').addEventListener('click', () => { S.sound = !S.sound; if (S.sound) sfx('buy'); save(false); refreshUI(); });

  function showDaily() {
    if (!dailyAvailable()) return;
    const day = nextStreak();
    let cells = '';
    DAILY.forEach((r, i) => {
      const n = i + 1;
      const cls = n < day ? 'done' : n === day ? 'today' : '';
      cells += `<div class="dcell ${cls}"><div>День ${n}</div><div class="dico">${r.gems ? '💎' : '🔥'}</div><div>${r.gems ? r.gems + ' 💎' : r.min + ' мин дохода'}</div></div>`;
    });
    modal('📅 Награда за вход', `<div class="dgrid">${cells}</div><p style="text-align:center">Заходи каждый день, и награды будут расти. Пропустишь день, и серия начнётся заново.</p>`,
      [{ text: 'Забрать', fn: claimDaily }]);
  }
  function claimDaily() {
    if (!dailyAvailable()) return;
    const day = nextStreak();
    const r = DAILY[day - 1];
    S.daily = { last: today(), streak: day };
    const txt = giveReward(r.gems ? { gems: r.gems, xp: 20 } : { lavaMin: r.min, xp: 20 });
    floatText(W / 2, H * 0.45, txt, '#ffd166', 22);
    sparkle(W / 2, H * 0.45, 30, ['#ffd166', '#7df9ff', '#ffffff']);
    haptic('success'); sfx('win');
    save(true);
    refreshUI();
  }

  function askPrestige() {
    const gain = obsidianGain();
    if (gain <= 0) return;
    modal('🏝️ Новый остров?', `Лава, улучшения и источники сбросятся.<br>Ты получишь <b>+${gain} 🖤 обсидиана</b>, всего станет ${S.obsidian + gain}: это <b>+${(S.obsidian + gain) * 10}%</b> ко всей лаве навсегда.<p>Алмазы, скины, питомцы, пропуск и покупки останутся.</p>`, [
      { text: 'Отмена', ghost: true },
      { text: 'Переезжаю!', fn: () => {
        const gainNow = obsidianGain();
        S = Object.assign(fresh(S), { obsidian: S.obsidian + gainNow });
        inc('islands');
        heat = 0; erupting = 0; combo = 0; bombs = []; meteor = null; cloud = null; boss = null;
        sparkle(W / 2, H / 2, 60, ['#c7b3ff', '#ffffff', '#ffd166']);
        haptic('success'); sfx('win');
        buildList();
        save(true);
      } },
    ]);
  }

  // ================= Сохранение =================
  function save(toCloud) {
    S.last = Date.now();
    const str = JSON.stringify(S);
    try { localStorage.setItem('volcano_save', str); } catch (e) {}
    if (toCloud && tgOk('6.9')) { try { tg.CloudStorage.setItem('save', str); } catch (e) {} }
  }
  function parseSave(str) {
    try {
      const o = JSON.parse(str);
      if (!o || typeof o !== 'object') return null;
      const s = Object.assign(fresh(), o);
      for (const k of ['up', 'gen', 'stats', 'ach', 'skins', 'perm', 'bought', 'pets', 'refMs']) if (!s[k] || typeof s[k] !== 'object') s[k] = {};
      s.skins.classic = true;
      if (!SKINS[s.skin]) s.skin = 'classic';
      if (!s.daily || typeof s.daily !== 'object') s.daily = { last: '', streak: 0 };
      if (!s.wheel || typeof s.wheel !== 'object') s.wheel = { day: '', xday: '', extra: 0 };
      if (s.dq && (!s.dq.c || !Array.isArray(s.dq.q))) s.dq = null;
      if (s.bp && (typeof s.bp !== 'object' || !s.bp.free)) s.bp = null;
      for (const k of ['lava', 'total', 'allTime', 'obsidian', 'gems', 'boostUntil', 'refs', 'refsClaimed', 'bossLevel']) s[k] = +s[k] || 0;
      return s;
    } catch (e) { return null; }
  }

  let loaded = false;
  function finishLoad() {
    if (loaded) return;
    loaded = true;
    if (tgUser && tgUser.id) S.uid = tgUser.id;
    ensureSeason(); ensureDQ();
    const away = Math.min(offlineCapH() * 3600, Math.max(0, (Date.now() - (S.last || Date.now())) / 1000));
    const p = passive();
    lastStage = stageIdx();
    applyStage();
    openTab('up');
    if (away > 60 && p > 0) {
      const gain = p * away * offlineRate();
      earn(gain);
      const mins = Math.round(away / 60);
      const t = mins >= 60 ? `${Math.floor(mins / 60)} ч ${mins % 60} мин` : `${mins} мин`;
      modal('🌙 С возвращением!', `Пока тебя не было (${t}), вулкан накопил<br><b style="font-size:24px;color:#ffd166">🔥 ${fmt(gain)}</b>`, [{ text: 'Забрать' }]);
    }
    if (!S.seenHelp) { S.seenHelp = true; showHelp(); }
    readLinkParams();
    if (dailyAvailable()) showDaily();
    save(false);
  }

  try { const local = parseSave(localStorage.getItem('volcano_save')); if (local) S = local; } catch (e) {}
  if (tgOk('6.9')) {
    setTimeout(finishLoad, 1500);
    try {
      tg.CloudStorage.getItem('save', (err, v) => {
        if (!loaded && !err && v) {
          const c = parseSave(v);
          if (c && (c.last || 0) > (S.last || 0)) S = c;
        }
        finishLoad();
      });
    } catch (e) { finishLoad(); }
  } else {
    finishLoad();
  }

  // ================= Онлайн-счётчик (работает, когда у бота есть туннель) =================
  const API_URL = (() => {
    const ok = (u) => /^https:\/\/[a-z0-9-]+\.trycloudflare\.com$/.test(u || '');
    try {
      const q = new URLSearchParams(location.search).get('api');
      if (ok(q)) { try { localStorage.setItem('lava_api', q); } catch (e) {} return q; }
      const saved = localStorage.getItem('lava_api');
      return ok(saved) ? saved : '';
    } catch (e) { return ''; }
  })();
  async function pingOnline() {
    if (!API_URL || !inTelegram || document.hidden) return;
    try {
      const r = await fetch(API_URL + '/ping', { method: 'POST', headers: { 'Content-Type': 'text/plain' }, body: JSON.stringify({ initData: tg.initData }) });
      if (!r.ok) return;
      const n = (await r.json()).online | 0;
      const el = $('onlineChip');
      el.classList.toggle('hidden', n <= 0);
      setText(el, '🟢 ' + n + ' онлайн');
    } catch (e) {}
  }
  pingOnline();
  setInterval(pingOnline, 30000);

  setInterval(() => { ensureDQ(); save(false); }, 5000);
  setInterval(() => save(true), 30000);
  document.addEventListener('visibilitychange', () => { if (document.hidden) save(true); else pingOnline(); });

  // ================= Главный цикл =================
  let last = performance.now(), uiT = 0;
  function frame(now) {
    const dt = Math.min(0.05, (now - last) / 1000);
    last = now;
    update(dt);
    draw();
    uiT -= dt;
    if (uiT <= 0) { refreshUI(); uiT = 0.15; }
    requestAnimationFrame(frame);
  }
  requestAnimationFrame(frame);
})();
</script>
</body>
</html>
