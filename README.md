<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Френдзона — Roblox Story</title>
<link href="https://fonts.googleapis.com/css2?family=Rubik:wght@300;400;500;700;900&family=Caveat:wght@600;700&family=Space+Mono:wght@400;700&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/qrcodejs@1.0.0/qrcode.min.js"></script>

<style>
  /* ===== БАЗА ===== */
  :root {
    --bg-color: #d9d9d9;
    --text-main: #000000;
    --text-muted: #444;
    --accent: #0000ff;
    --border-color: #000;
    --may-color: #e0e0e0;
    --vitalik-color: #ff8c00;
    --verify-color: #555;
    --font-mono: 'Space Mono', monospace;
    --modal-bg: rgba(0, 0, 0, 0.85);
  }
  * { margin: 0; padding: 0; box-sizing: border-box; }
  
  ::-webkit-scrollbar { width: 10px; }
  ::-webkit-scrollbar-track { background: var(--bg-color); border-left: 1px solid #000; }
  ::-webkit-scrollbar-thumb { background: var(--accent); border: 1px solid #000; }

  body {
    font-family: 'Rubik', sans-serif;
    background-color: var(--bg-color);
    color: var(--text-main);
    line-height: 1.5;
    overflow-x: hidden;
    min-height: 100vh;
    background-image: 
      linear-gradient(rgba(0, 0, 0, 0.05) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0, 0, 0, 0.05) 1px, transparent 1px);
    background-size: 20px 20px;
    transition: background-color 0.3s, color 0.3s;
  }

  /* ===== ФОН ===== */
  body::before {
    content: ''; position: fixed; top: 10%; right: -5%; width: 300px; height: 300px;
    background: var(--accent); border-radius: 50%; z-index: -1; opacity: 0.8; border: 3px solid #000;
    pointer-events: none;
  }
  body::after {
    content: ''; position: fixed; bottom: 5%; left: -10%; width: 250px; height: 250px;
    background: #000; border-radius: 50%; z-index: -1; opacity: 0.9; border: 3px solid #fff;
    pointer-events: none;
  }

  .noise-overlay {
    position: fixed; top: 0; left: 0; width: 100%; height: 100%;
    pointer-events: none; z-index: 9998; opacity: 0.15;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noiseFilter'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.65' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noiseFilter)'/%3E%3C/svg%3E");
  }

  /* ===== ПРОГРЕСС-БАР ===== */
  .progress-bar {
    position: fixed; top: 0; left: 0; height: 6px; background: var(--accent);
    width: 0%; z-index: 9999; border-bottom: 2px solid #000;
  }

  /* ===== ШАПКА ===== */
  header {
    background: #fff; border-bottom: 3px solid var(--border-color);
    padding: 15px 0; position: sticky; top: 0; z-index: 100;
  }
  .header-inner {
    max-width: 1100px; margin: 0 auto; padding: 0 20px;
    display: flex; justify-content: space-between; align-items: center;
  }
  
  .logo { cursor: pointer; user-select: none; }
  .custom-logo {
    width: 80px; height: 60px; background: #000; color: #fff;
    display: flex; flex-direction: column; align-items: center; justify-content: center;
    position: relative; border: 2px solid var(--accent); box-shadow: 4px 4px 0 var(--accent);
    transition: 0.1s;
  }
  .custom-logo:active { box-shadow: 0 0 0 var(--accent); transform: translate(4px, 4px); }
  .logo-text { font-family: var(--font-mono); font-size: 1em; font-weight: 900; text-transform: uppercase; line-height: 1; }

  .header-info { display: flex; align-items: center; gap: 15px; }
  
  /* ===== ВЕРИФИКАЦИЯ ===== */
  .verify-badge {
    display: flex; align-items: center; gap: 5px; font-family: var(--font-mono);
    font-size: 0.75em; font-weight: bold; padding: 5px 10px;
    border: 2px solid var(--verify-color); color: var(--verify-color); background: #fff;
  }
  .verify-badge svg { width: 14px; height: 14px; fill: var(--verify-color); }
  .verify-badge.verified-user { --verify-color: #1DA1F2; }
  .verify-badge.verified-bot { --verify-color: #00ff88; }
  .verify-badge.verified-admin { --verify-color: #ffd700; }

  /* ===== БУРГЕР ===== */
  .burger {
    width: 45px; height: 40px; background: var(--accent);
    border: 2px solid #000; cursor: pointer; position: relative;
    display: flex; justify-content: center; align-items: center; box-shadow: 3px 3px 0 #000; transition: 0.1s;
  }
  .burger:active { box-shadow: 0 0 0 #000; transform: translate(3px, 3px); }
  .burger span { display: block; width: 25px; height: 3px; background: #fff; position: absolute; }
  .burger span:nth-child(1) { transform: translateY(-7px); }
  .burger span:nth-child(2) { transform: translateY(0); width: 15px; right: 10px; }
  .burger span:nth-child(3) { transform: translateY(7px); }
  .burger.active span:nth-child(1) { transform: translateY(0) rotate(45deg); }
  .burger.active span:nth-child(2) { opacity: 0; }
  .burger.active span:nth-child(3) { transform: translateY(0) rotate(-45deg); }

  /* ===== МЕНЮ ===== */
  .menu-overlay {
    position: fixed; top: 0; left: 0; width: 100%; height: 100%;
    background: rgba(0, 0, 0, 0.5); z-index: 150; opacity: 0; pointer-events: none; transition: 0.3s;
  }
  .menu-overlay.active { opacity: 1; pointer-events: all; }

  .side-menu {
    position: fixed; top: 0; right: -400px; width: 380px; height: 100vh;
    background: #fff; border-left: 4px solid #000; z-index: 200;
    transition: right 0.3s cubic-bezier(0.16, 1, 0.3, 1);
    padding: 70px 25px 20px; display: flex; flex-direction: column; gap: 15px; overflow-y: auto;
  }
  .side-menu.open { right: 0; }

  .close-menu-btn {
    position: absolute; top: 15px; right: 15px; background: #000; color: #fff;
    border: none; width: 40px; height: 40px; font-family: var(--font-mono); font-size: 1.2em; cursor: pointer;
  }

  .menu-btn {
    background: #fff; border: 2px solid #000; padding: 12px 15px; color: #000; 
    font-size: 0.95em; font-family: var(--font-mono); text-transform: uppercase; font-weight: 700; 
    cursor: pointer; text-align: left; display: flex; align-items: center; gap: 10px; 
    transition: 0.1s; box-shadow: 4px 4px 0 #000;
  }
  .menu-btn:hover { background: var(--accent); color: #fff; }
  .menu-btn:active { box-shadow: 0 0 0 #000; transform: translate(4px, 4px); }

  /* ===== НАСТРОЙКИ ===== */
  .settings-box { display: none; background: #eee; padding: 15px; border: 2px dashed #000; margin-top: 5px; }
  .settings-box.active { display: block; }
  .settings-title { margin-bottom: 10px; font-size: 0.8em; font-weight: 900; text-transform: uppercase; font-family: var(--font-mono);}
  
  /* 12 тем */
  .theme-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 8px; margin-bottom: 15px; }
  .theme-btn { height: 35px; border: 2px solid #000; cursor: pointer; transition: 0.2s; }
  .theme-btn:hover { transform: scale(1.05); }
  .theme-btn.active { outline: 3px solid var(--accent); }
  
  .theme-light { background: #d9d9d9; }
  .theme-dark { background: #0f0f1a; }
  .theme-acid { background: #ccff00; }
  .theme-blue { background: #0000ff; }
  .theme-blood { background: #8b0000; }
  .theme-pink { background: #ff69b4; }
  .theme-space { background: #1a0b2e; }
  .theme-retro { background: #ffcc00; }
  .theme-lavender { background: #e6e6fa; }
  .theme-mint { background: #98ff98; }
  .theme-ocean { background: #004466; }
  .theme-sunset { background: linear-gradient(135deg, #ff7e5f, #feb47b); }

  .random-theme-btn {
    width: 100%; padding: 10px; border: 2px solid #000; background: #fff;
    font-family: var(--font-mono); font-weight: bold; cursor: pointer; margin-bottom: 15px;
    box-shadow: 3px 3px 0 #000;
  }
  .random-theme-btn:active { box-shadow: 0 0 0 #000; transform: translate(3px, 3px); }

  .helper-settings { border-top: 2px solid #000; padding-top: 15px; margin-top: 5px; }
  .helper-select { display: flex; gap: 10px; margin-top: 10px; }
  .helper-btn {
    flex: 1; padding: 10px; border: 2px solid #000; background: #fff; color: #000;
    cursor: pointer; font-family: var(--font-mono); font-weight: bold; text-transform: uppercase; box-shadow: 3px 3px 0 #000;
  }
  .helper-btn.active { background: var(--accent); color: #fff; }
  .helper-btn:active { box-shadow: 0 0 0 #000; transform: translate(3px, 3px); }

  .toggle-3d-btn { width: 100%; padding: 8px; margin-top: 10px; border: 2px solid #000; background: #fff; font-family: var(--font-mono); font-weight: bold; cursor: pointer; }
  .color-opt { width: 25px; height: 25px; border: 2px solid #000; border-radius: 0; cursor: pointer; }
  
  .qr-style-select { display: flex; gap: 5px; margin-top: 5px; }
  .qr-style-btn { flex:1; padding: 5px; border: 2px solid #000; background: #fff; cursor:pointer; font-family: var(--font-mono); font-size:0.7em; font-weight:bold; }
  .qr-style-btn.active { background: #000; color: #fff; }

  /* ===== МОДАЛЬНЫЕ ОКНА ===== */
  .qr-modal, .admin-modal, .login-modal, .reg-modal {
    display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%;
    background: var(--modal-bg); z-index: 9999; align-items: center; justify-content: center;
    backdrop-filter: blur(5px);
  }
  .qr-modal.show, .admin-modal.show, .login-modal.show, .reg-modal.show { display: flex; }
  
  .qr-content, .login-content, .reg-content, .admin-content {
    background: #fff; border: 4px solid #000; padding: 25px; text-align: center;
    position: relative; max-width: 450px; width: 90%; box-shadow: 8px 8px 0 var(--accent);
    max-height: 90vh; overflow-y: auto;
  }
  .qr-content h2, .login-content h2, .reg-content h2, .admin-content h2 { 
    color: #000; margin-bottom: 15px; font-family: var(--font-mono); text-transform: uppercase; font-weight: 900;
  }
  
  #qrcode { display: flex; justify-content: center; background: #fff; padding: 10px; border: 2px solid #000; width: fit-content; margin: 0 auto; }
  .close-qr, .close-admin, .close-login, .close-reg { position: absolute; top: 10px; right: 15px; font-size: 1.3em; color: #000; cursor: pointer; background: none; border: none; }

  .login-content input, .reg-content input, .admin-content input, .admin-content textarea {
    width: 100%; padding: 12px; margin-bottom: 10px; background: #eee; 
    border: 2px solid #000; color: #000; font-family: var(--font-mono);
  }
  .btn-login, .btn-reg, .btn-publish, .btn-save-timer, .btn-admin-action {
    width: 100%; padding: 12px; border: 2px solid #000; background: var(--accent); color: #fff; 
    font-weight: bold; font-family: var(--font-mono); cursor: pointer; text-transform: uppercase;
    box-shadow: 4px 4px 0 #000; transition: 0.1s;
  }
  .btn-login:active, .btn-reg:active, .btn-publish:active, .btn-save-timer:active, .btn-admin-action:active { box-shadow: 0 0 0 #000; transform: translate(4px, 4px); }
  .btn-exit { flex: 1; padding: 12px; border: 2px solid #ff0000; background: #fff; color: #ff0000; font-weight: bold; cursor: pointer; font-family: var(--font-mono); }

  .admin-buttons { display: flex; gap: 10px; margin-bottom: 15px; }
  .admin-stats { display: flex; gap: 10px; margin-bottom: 15px; }
  .stat-box { flex: 1; border: 2px solid #000; padding: 10px; text-align: center; background: #eee; }
  .stat-box span { display: block; font-size: 1.5em; font-family: var(--font-mono); font-weight: bold; color: var(--accent); }
  .stat-box small { font-size: 0.7em; text-transform: uppercase; font-weight: bold; }
  
  .admin-timer-settings { border: 2px dashed #000; padding: 10px; margin-bottom: 15px; text-align: left;}
  .admin-timer-settings label { display: block; font-size: 0.8em; font-weight: bold; margin-bottom: 5px; font-family: var(--font-mono);}
  .admin-timer-settings input { margin-bottom: 0; color: #000; }
  
  .admin-new-functions { display: flex; flex-direction: column; gap: 8px; margin-bottom: 15px; }
  .btn-admin-danger { background: #ff4757; color: #fff; }
  .btn-admin-action { background: #0077FF; color: #fff; }

  /* ===== СТРАНИЦЫ ===== */
  .page-section { display: none; max-width: 1000px; margin: 0 auto; padding: 40px 20px; position: relative; z-index: 10; }
  .page-section.active { display: block; }

  .section-title { 
    font-size: 2.5em; margin-bottom: 30px; color: #fff; font-weight: 900; text-transform: uppercase;
    background: #000; display: inline-block; padding: 10px 20px; border: 3px solid var(--accent);
    transform: rotate(-2deg); font-family: var(--font-mono);
  }

  .glass-card {
    background: #fff; border: 3px solid #000; padding: 25px; margin-bottom: 20px;
    box-shadow: 8px 8px 0 #000; transition: 0.2s;
  }
  .glass-card:hover { transform: translate(-4px, -4px); box-shadow: 12px 12px 0 var(--accent); }

  .timer-box { display: flex; justify-content: center; gap: 10px; margin-top: 20px; flex-wrap: wrap; }
  .timer-item {
    background: #000; color: #fff; border: 3px solid var(--accent); padding: 15px; min-width: 80px; 
    text-align: center; font-family: var(--font-mono); box-shadow: 5px 5px 0 var(--accent);
  }
  .timer-item span { display: block; font-size: 2em; font-weight: bold; color: var(--accent); }
  .timer-item small { font-size: 0.7em; text-transform: uppercase; color: #fff; }

  .social-buttons { display: flex; justify-content: center; gap: 15px; flex-wrap: wrap; margin-top: 20px; }
  .social-circle {
    width: 50px; height: 50px; border: 3px solid #000; background: #fff;
    display: flex; align-items: center; justify-content: center; box-shadow: 4px 4px 0 #000; transition: 0.1s;
  }
  .social-circle svg { width: 24px; height: 24px; fill: #000; transition: 0.3s; }
  .social-circle:hover { background: var(--accent); }
  .social-circle:hover svg { fill: #fff; }
  .social-circle:active { box-shadow: 0 0 0 #000; transform: translate(4px, 4px); }

  .character-item {
    display: flex; align-items: center; gap: 15px; background: #fff; border: 2px solid #000; 
    padding: 15px; margin-bottom: 10px; box-shadow: 4px 4px 0 #000; transition: 0.1s;
  }
  .character-item:hover { background: #eee; transform: translateX(5px); }
  
  .character-avatar {
    width: 50px; height: 50px; border: 2px solid #000; background: var(--accent);
    display: flex; align-items: center; justify-content: center;
    font-weight: bold; color: #fff; font-size: 1.5em; font-family: var(--font-mono); flex-shrink: 0;
  }
  .character-info h4 { color: #000; font-size: 1.2em; font-family: var(--font-mono); text-transform: uppercase; }
  .character-info p { color: #444; font-size: 0.85em; }

  .characters-list { display: none; margin-top: 15px; }
  .characters-list.show { display: block; }

  .share-btn, .scroll-top {
    position: fixed; width: 50px; height: 50px; border: 3px solid #000; background: #fff; color: #000;
    font-size: 1.5em; cursor: pointer; display: flex; align-items: center; justify-content: center;
    box-shadow: 4px 4px 0 #000; z-index: 90; transition: 0.1s;
  }
  .share-btn { bottom: 90px; right: 20px; }
  .scroll-top { bottom: 30px; right: 20px; opacity: 0; pointer-events: none; }
  .scroll-top.show { opacity: 1; pointer-events: all; }
  .share-btn:active, .scroll-top:active { box-shadow: 0 0 0 #000; transform: translate(4px, 4px); }

  /* ===== ПОМОЩНИКИ (АНИМАЦИИ) ===== */
  .helper-container {
    position: fixed; bottom: 80px; left: 20px; z-index: 1001;
    width: 120px; height: 120px; cursor: grab;
    perspective: 1000px; touch-action: none;
  }
  .helper-container:active { cursor: grabbing; }

  .helper-3d-wrapper {
    width: 100%; height: 100%; transform-style: preserve-3d; transition: transform 0.1s;
  }

  body.use-3d .helper-3d-wrapper { animation: rotate3d 6s infinite linear; }
  @keyframes rotate3d {
    0% { transform: rotateY(0deg) rotateX(5deg); }
    50% { transform: rotateY(15deg) rotateX(-5deg); }
    100% { transform: rotateY(0deg) rotateX(5deg); }
  }

  .helper-container.jump .bars-container, .helper-container.jump .cat-container {
    animation: jumpAnim 0.4s ease;
  }
  @keyframes jumpAnim {
    0% { transform: translateY(0); }
    50% { transform: translateY(-20px) scale(1.05); }
    100% { transform: translateY(0); }
  }

  .bars-container, .cat-container {
    display: flex; justify-content: center; align-items: center; width: 100%; height: 100%;
    filter: drop-shadow(0 15px 15px rgba(0,0,0,0.3));
  }

  .bars-head, .cat-head {
    width: 90px; height: 85px; border-radius: 50%;
    position: relative; border: 3px solid #000; transition: background 0.3s ease;
  }

  /* --- МЕЙ МЕЙ --- */
  .bars-head { background: var(--may-color); }
  .bars-ear { position: absolute; top: -12px; width: 30px; height: 30px; background: var(--may-color); border-radius: 50%; border: 3px solid #000; z-index: -1; }
  .bars-ear.left { left: -5px; }
  .bars-ear.right { right: -5px; }
  .bars-ear::after { content: ''; position: absolute; top: 5px; left: 5px; width: 12px; height: 12px; background: #ffb6c1; border-radius: 50%; }
  
  .bars-eye { position: absolute; top: 25px; width: 24px; height: 24px; background: #000; border-radius: 50%; overflow: hidden; }
  .bars-eye.left { left: 15px; }
  .bars-eye.right { right: 15px; }
  .bars-eye::after { content: ''; position: absolute; top: 2px; left: 2px; width: 8px; height: 8px; background: #fff; border-radius: 50%; }
  
  body.use-3d .bars-eye { animation: blink 4s infinite; }
  @keyframes blink { 0%, 96%, 100% { transform: scaleY(1); } 98% { transform: scaleY(0.1); } }
  
  .bars-nose { position: absolute; top: 50px; left: 50%; transform: translateX(-50%); width: 14px; height: 10px; background: #ff9999; border-radius: 50% 50% 50% 50% / 60% 60% 40% 40%; border: 1px solid #000; }
  .bars-mouth { position: absolute; top: 60px; left: 50%; transform: translateX(-50%); width: 20px; height: 10px; border-bottom: 2px solid #000; border-radius: 0 0 50% 50%; }
  .bars-spot { position: absolute; width: 10px; height: 10px; background: #000; border-radius: 50%; opacity: 0.5; }
  .spot1 { top: 10px; left: 10px; }
  .spot2 { top: 15px; right: 12px; }
  .spot3 { bottom: 20px; left: 5px; }
  .spot4 { bottom: 15px; right: 8px; }
  
  .bars-paw { position: absolute; bottom: -20px; left: 50%; transform: translateX(-50%); width: 40px; height: 40px; background: var(--may-color); border-radius: 50%; border: 3px solid #000; display: flex; justify-content: center; align-items: center; z-index: 2; }
  .bars-paw::before { content: ''; width: 15px; height: 12px; background: #000; border-radius: 50%; position: absolute; }
  .bars-paw::after { content: ''; position: absolute; top: 5px; left: 5px; width: 5px; height: 5px; background: #000; border-radius: 50%; box-shadow: 10px 0 0 #000, 20px 0 0 #000; }

  /* --- ВИТАЛИК --- */
  .cat-head { background: var(--vitalik-color); border-radius: 45% 45% 50% 50%; }
  .cat-ear { position: absolute; top: -15px; width: 32px; height: 32px; background: var(--vitalik-color); border-radius: 50%; border: 3px solid #000; z-index: -1; }
  .cat-ear.left { left: -5px; }
  .cat-ear.right { right: -5px; }
  .cat-ear::after { content: ''; position: absolute; top: 6px; left: 6px; width: 12px; height: 12px; background: #ffb6c1; border-radius: 50%; }
  
  .cat-stripe { position: absolute; width: 20px; height: 5px; background: #000; border-radius: 5px; }
  .stripe1 { top: 15px; left: 10px; transform: rotate(-10deg); }
  .stripe2 { top: 25px; right: 10px; transform: rotate(10deg); }
  
  .cat-eye { position: absolute; top: 28px; width: 22px; height: 22px; background: #000; border-radius: 50%; overflow: hidden; }
  .cat-eye.left { left: 15px; }
  .cat-eye.right { right: 15px; }
  .cat-eye::after { content: ''; position: absolute; top: 2px; left: 2px; width: 8px; height: 8px; background: #fff; border-radius: 50%; }
  
  body.use-3d .cat-eye { animation: blink 4s infinite; }
  
  .cat-nose { position: absolute; top: 52px; left: 50%; transform: translateX(-50%); width: 14px; height: 10px; background: #ff9999; border-radius: 50% 50% 50% 50% / 60% 60% 40% 40%; border: 1px solid #000; }
  .cat-mouth { position: absolute; top: 62px; left: 50%; transform: translateX(-50%); width: 20px; height: 10px; border-bottom: 2px solid #000; border-radius: 0 0 50% 50%; }

  .cat-paw { position: absolute; bottom: -20px; left: 50%; transform: translateX(-50%); width: 40px; height: 40px; background: var(--vitalik-color); border-radius: 50%; border: 3px solid #000; display: flex; justify-content: center; align-items: center; z-index: 2; }
  .cat-paw::before { content: ''; width: 15px; height: 12px; background: #000; border-radius: 50%; position: absolute; }
  .cat-paw::after { content: ''; position: absolute; top: 5px; left: 5px; width: 5px; height: 5px; background: #000; border-radius: 50%; box-shadow: 10px 0 0 #000, 20px 0 0 #000; }

  .helper-container:hover .bars-paw, .helper-container:hover .cat-paw { transform: translateX(-50%) translateY(-15px) scale(1.1); }
  
  .may-bubble {
    position: absolute; bottom: 130px; left: 0; width: 220px;
    background: #fff; color: #000; padding: 10px 15px; border: 3px solid #000;
    font-size: 0.85em; font-weight: bold; text-align: center; font-family: var(--font-mono);
    box-shadow: 4px 4px 0 #000; opacity: 0; pointer-events: none; transform: translateY(10px) scale(0.9);
    transition: 0.3s; z-index: 10;
  }
  .may-bubble::after { content: ''; position: absolute; bottom: -10px; left: 40px; width: 15px; height: 15px; background: #fff; border-bottom: 3px solid #000; border-right: 3px solid #000; transform: rotate(45deg); }
  .may-bubble.show { opacity: 1; transform: translateY(0) scale(1); pointer-events: all; }

  .toast {
    position: fixed; bottom: 20px; left: 50%; transform: translateX(-50%) translateY(50px);
    background: #000; color: #fff; padding: 15px 30px; border: 3px solid var(--accent);
    font-family: var(--font-mono); font-weight: bold; text-transform: uppercase;
    z-index: 10000; opacity: 0; transition: 0.3s;
  }
  .toast.show { transform: translateX(-50%) translateY(0); opacity: 1; }

  /* ===== РЕЖИМ ЧТЕНИЯ ===== */
  body.reading-mode .header-inner,
  body.reading-mode .share-btn,
  body.reading-mode .antivirus-badge,
  body.reading-mode .helper-container,
  body.reading-mode .scroll-top,
  body.reading-mode .timer-box,
  body.reading-mode .social-buttons,
  body.reading-mode footer,
  body.reading-mode .section-title { display: none !important; }
  body.reading-mode .page-section { padding-top: 20px; }
  body.reading-mode { background-color: #000 !important; }

  /* ===== АДАПТИВ ===== */
  @media (max-width: 600px) {
    .side-menu { width: 100%; right: -100%; }
    .helper-container { width: 100px; height: 100px; bottom: 70px; left: 10px; }
    .bars-head, .cat-head { width: 75px; height: 70px; }
    .bars-ear, .cat-ear { width: 25px; height: 25px; }
    .bars-eye, .cat-eye { width: 18px; height: 18px; top: 22px; }
    .bars-eye.left, .cat-eye.left { left: 12px; }
    .bars-eye.right, .cat-eye.right { right: 12px; }
    .bars-nose, .cat-nose { top: 42px; }
    .bars-mouth, .cat-mouth { top: 52px; }
    .bars-paw, .cat-paw { width: 32px; height: 32px; bottom: -15px; }
    .may-bubble { width: 180px; left: -20px; bottom: 110px; }
    .section-title { font-size: 1.5em; }
  }
</style>
</head>
<body class="use-3d">

  <div class="noise-overlay"></div>
  <div class="progress-bar" id="progress-bar"></div>

  <!-- АНТИВИРУС -->
  <div class="antivirus-badge" id="antivirus" onclick="runVirusScan()" style="position:fixed; bottom:20px; left:20px; background:#000; color:#00ff88; border:2px solid #00ff88; padding:5px 10px; font-family:var(--font-mono); font-size:0.7em; cursor:pointer; z-index:1000;">
    <span>🛡️</span> <span id="av-status">СИСТЕМА ЗАЩИЩЕНА</span>
  </div>

  <!-- ПОМОЩНИК -->
  <div class="helper-container" id="helper" onmousedown="startDrag(event)" ontouchstart="startDrag(event)" onclick="helperTalk(event)">
    <div class="may-bubble" id="helper-bubble">Привет!</div>
    
    <div class="helper-3d-wrapper">
      <div id="view-may" style="display: block;">
        <div class="bars-container">
          <div class="bars-head">
            <div class="bars-ear left"></div><div class="bars-ear right"></div>
            <div class="bars-spot spot1"></div><div class="bars-spot spot2"></div>
            <div class="bars-spot spot3"></div><div class="bars-spot spot4"></div>
            <div class="bars-eye left"></div><div class="bars-eye right"></div>
            <div class="bars-nose"></div><div class="bars-mouth"></div>
          </div>
          <div class="bars-paw"></div>
        </div>
      </div>
      <div id="view-vitalik" style="display: none;">
        <div class="cat-container">
          <div class="cat-head">
            <div class="cat-ear left"></div><div class="cat-ear right"></div>
            <div class="cat-stripe stripe1"></div><div class="cat-stripe stripe2"></div>
            <div class="cat-eye left"></div><div class="cat-eye right"></div>
            <div class="cat-nose"></div><div class="cat-mouth"></div>
          </div>
          <div class="cat-paw"></div>
        </div>
      </div>
    </div>
  </div>

  <button class="share-btn" onclick="shareSite()" title="Поделиться">📤</button>

  <header>
    <div class="header-inner">
      <div class="logo" id="logo-secret">
        <div class="custom-logo">
          <span class="logo-text">FRNZ<br>STORY</span>
        </div>
      </div>
      <div class="header-info">
        <div class="verify-badge" id="verify-badge">
          <svg viewBox="0 0 24 24"><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-2 15l-5-5 1.41-1.41L10 14.17l7.59-7.59L19 8l-9 9z"/></svg>
          <span id="verify-text">NO VERIFY</span>
        </div>
        <div class="burger" id="burger"><span></span><span></span><span></span></div>
      </div>
    </div>
  </header>

  <div class="menu-overlay" id="menu-overlay"></div>

  <div class="side-menu" id="side-menu">
    <button class="close-menu-btn" onclick="toggleMenu()">X</button>
    
    <button class="menu-btn" onclick="toggleSettings()">⚙️ НАСТРОЙКИ</button>
    <div class="settings-box" id="settings-box">
      <div class="settings-title">ЦВЕТ ТЕМЫ</div>
      <div class="theme-grid">
        <div class="theme-btn theme-light active" onclick="applyTheme('#d9d9d9', '#000000', '#0000ff', this)" title="Светлая"></div>
        <div class="theme-btn theme-dark" onclick="applyTheme('#0f0f1a', '#ffffff', '#00ffff', this)" title="Тёмная"></div>
        <div class="theme-btn theme-acid" onclick="applyTheme('#ccff00', '#000000', '#ff00ff', this)" title="Кислотная"></div>
        <div class="theme-btn theme-blue" onclick="applyTheme('#0000ff', '#ffffff', '#ffff00', this)" title="Синяя"></div>
        <div class="theme-btn theme-blood" onclick="applyTheme('#8b0000', '#ffffff', '#000000', this)" title="Кровавая"></div>
        <div class="theme-btn theme-pink" onclick="applyTheme('#ff69b4', '#000000', '#ffffff', this)" title="Розовая"></div>
        <div class="theme-btn theme-space" onclick="applyTheme('#1a0b2e', '#ffffff', '#ff6b9d', this)" title="Космос"></div>
        <div class="theme-btn theme-retro" onclick="applyTheme('#ffcc00', '#000000', '#ff0000', this)" title="Ретро"></div>
        <div class="theme-btn theme-lavender" onclick="applyTheme('#e6e6fa', '#000000', '#9370DB', this)" title="Лавандовая"></div>
        <div class="theme-btn theme-mint" onclick="applyTheme('#98ff98', '#000000', '#0077FF', this)" title="Мятная"></div>
        <div class="theme-btn theme-ocean" onclick="applyTheme('#004466', '#ffffff', '#00aaff', this)" title="Океан"></div>
        <div class="theme-btn theme-sunset" onclick="applyTheme('#ff7e5f', '#000000', '#ffdd00', this)" title="Закат"></div>
      </div>
      <button class="random-theme-btn" onclick="randomTheme()">СЛУЧАЙНАЯ ТЕМА</button>

      <div class="helper-settings">
        <div class="settings-title">ПОМОЩНИК</div>
        <div class="helper-select">
          <button class="helper-btn active" id="btn-may" onclick="setHelper('may')">МЕЙ МЕЙ</button>
          <button class="helper-btn" id="btn-vitalik" onclick="setHelper('vitalik')">ВИТАЛИК</button>
        </div>
        
        <div class="settings-title" style="margin-top:10px;">ЦВЕТ ПОМОЩНИКА</div>
        <div style="display:flex; gap:5px; justify-content:center; margin-bottom:10px;">
          <button class="color-opt" style="background: #e0e0e0;" onclick="setHelperColor('#e0e0e0', 'may')"></button>
          <button class="color-opt" style="background: #ffffff;" onclick="setHelperColor('#ffffff', 'may')"></button>
          <button class="color-opt" style="background: #a0a0a0;" onclick="setHelperColor('#a0a0a0', 'may')"></button>
          <button class="color-opt" style="background: #ffcc99;" onclick="setHelperColor('#ffcc99', 'may')"></button>
          
          <button class="color-opt" style="background: #ff8c00;" onclick="setHelperColor('#ff8c00', 'vitalik')"></button>
          <button class="color-opt" style="background: #ff4500;" onclick="setHelperColor('#ff4500', 'vitalik')"></button>
          <button class="color-opt" style="background: #8a2be2;" onclick="setHelperColor('#8a2be2', 'vitalik')"></button>
          <button class="color-opt" style="background: #ff69b4;" onclick="setHelperColor('#ff69b4', 'vitalik')"></button>
        </div>

        <button class="toggle-3d-btn" onclick="toggle3D()" id="btn-3d">ОТКЛЮЧИТЬ 3D</button>

        <div class="settings-title" style="margin-top:15px;">СТИЛЬ QR</div>
        <div class="qr-style-select">
          <button class="qr-style-btn active" onclick="setQRStyle('normal', this)">ОБЫЧНЫЙ</button>
          <button class="qr-style-btn" onclick="setQRStyle('invert', this)">ИНВЕРТ</button>
          <button class="qr-style-btn" onclick="setQRStyle('color', this)">ЦВЕТНОЙ</button>
        </div>

        <button class="random-theme-btn" style="margin-top:15px; background:#ff4757; color:#fff;" onclick="resetAll()">СБРОСИТЬ ВСЁ</button>
      </div>
    </div>

    <button class="menu-btn" onclick="toggleQR()">📱 QR-КОД</button>
    <button class="menu-btn" onclick="showPage('home')">💔 ИСТОРИЯ</button>
    <button class="menu-btn" onclick="showPage('characters')">👥 ПЕРСОНАЖИ</button>
    
    <button class="menu-btn" onclick="openLogin()" style="border-color: var(--accent); color: var(--accent);">🔑 ВХОД</button>
    <button class="menu-btn" onclick="openReg()" style="border-color: #000; background:#fff;">📝 РЕГИСТРАЦИЯ</button>

    <div class="support-box" style="border: 2px dashed #000; padding: 15px; margin-top: 5px; text-align: center;">
      <div style="font-weight: bold; font-size: 0.9em; margin-bottom: 5px; font-family: var(--font-mono);">ПОДДЕРЖКА</div>
      <a href="mailto:help.frendzona.offs@gmail.com" style="display: block; color: #000; font-size: 0.8em; text-decoration: none; margin-bottom: 10px; word-break: break-all;">help.frendzona.offs@gmail.com</a>
      <div style="display: flex; gap: 5px;">
        <a href="mailto:help.frendzona.offs@gmail.com" style="flex: 1; padding: 8px; border: 2px solid #000; background: #fff; color: #000; text-decoration: none; font-size: 0.7em; font-weight:bold;">НАПИСАТЬ</a>
        <button onclick="copyEmail()" style="flex: 1; padding: 8px; border: 2px solid #000; background: var(--accent); color: #fff; font-size: 0.7em; font-weight:bold; cursor:pointer;">КОПИРОВАТЬ</button>
      </div>
    </div>
  </div>

  <!-- МОДАЛЬНЫЕ ОКНА -->
  <div class="login-modal" id="login-modal">
    <div class="login-content">
      <button class="close-login" onclick="closeLogin()">X</button>
      <h2>АДМИН ВХОД</h2>
      <input type="password" id="login-code" placeholder="КОД...">
      <button class="btn-login" id="btn-login-submit" onclick="checkLogin()">ВОЙТИ</button>
      <p id="login-error" style="color:red; font-size:0.8em; margin-top:10px; display:none;"></p>
    </div>
  </div>

  <div class="reg-modal" id="reg-modal">
    <div class="reg-content">
      <button class="close-reg" onclick="closeReg()">X</button>
      <h2>РЕГИСТРАЦИЯ</h2>
      <input type="text" id="reg-login" placeholder="ЛОГИН">
      <input type="password" id="reg-password" placeholder="ПАРОЛЬ">
      <button class="btn-reg" onclick="registerUser()">СОЗДАТЬ</button>
    </div>
  </div>

  <div class="qr-modal" id="qr-modal">
    <div class="qr-content">
      <button class="close-qr" onclick="toggleQR()">X</button>
      <h2>СКАНИРУЙ</h2>
      <div id="qrcode" style="transition: 0.3s;"></div>
    </div>
  </div>

  <div class="admin-modal" id="admin-modal">
    <div class="admin-content">
      <button class="close-admin" onclick="closeAdminPanel()">X</button>
      <h2>ПАНЕЛЬ</h2>
      <div class="admin-stats">
        <div class="stat-box"><span id="views-count">0</span><small>ПРОСМОТРЫ</small></div>
        <div class="stat-box"><span id="admin-news-count">0</span><small>НОВОСТИ</small></div>
      </div>
      <div class="admin-timer-settings">
        <label>ДАТА ГЛАВЫ:</label>
        <input type="datetime-local" id="timer-date-input">
        <button class="btn-save-timer" onclick="saveTimerDate()" style="margin-top:5px;">СОХРАНИТЬ</button>
      </div>
      
      <!-- НОВЫЕ ФУНКЦИИ АДМИНА -->
      <div class="admin-new-functions">
        <button class="btn-admin-action" onclick="toggleReadingMode()">📖 РЕЖИМ ЧТЕНИЯ</button>
        <button class="btn-admin-action" onclick="deleteAllNews()" style="background:#ff4757; color:#fff;">🗑 УДАЛИТЬ ВСЕ НОВОСТИ</button>
        <button class="btn-admin-action" onclick="showLogs()" style="background:#00ff88; color:#000;">📜 ПОКАЗАТЬ ЛОГИ</button>
      </div>

      <input type="text" id="news-title" placeholder="ЗАГОЛОВОК...">
      <textarea id="news-text" placeholder="ТЕКСТ..."></textarea>
      <input type="text" id="news-tags" placeholder="ТЕГИ...">
      <div class="admin-buttons">
        <button class="btn-publish" onclick="addNews()">ДОБАВИТЬ</button>
        <button class="btn-exit" onclick="closeAdminPanel()">ВЫЙТИ</button>
      </div>
    </div>
  </div>

  <!-- ГЛАВНАЯ -->
  <section class="page-section active" id="page-home">
    <h1 class="section-title">НОВОСТИ</h1>
    
    <div style="font-family: var(--font-mono); font-weight: bold; margin-bottom: 20px; text-transform: uppercase;">
      <span style="background: #000; color: #fff; padding: 2px 5px;">FRENDZONA</span> // 
      <span style="background: var(--accent); color: #fff; padding: 2px 5px;">FRIENDZONE</span> // 
      <span style="background: #fff; border: 1px solid #000; padding: 2px 5px;">ФРЕНДЗОНА</span><br><br>
      <span style="background: #fff; border: 1px solid #000; padding: 2px 5px;">LAVENDER SPRING</span> // 
      <span style="background: var(--accent); color: #fff; padding: 2px 5px;">ЛАВАНДОВАЯ ВЕСНА</span><br><br>
      <span style="background: #000; color: #fff; padding: 2px 5px;">LOVE HAS NO BORDERS</span> // 
      <span style="background: #fff; border: 1px solid #000; padding: 2px 5px;">У ЛЮБВИ НЕТ ГРАНИЦ</span>
    </div>

    <div class="timer-box" id="timer">
      <div class="timer-item"><span id="days">00</span><small>ДНЕЙ</small></div>
      <div class="timer-item"><span id="hours">00</span><small>ЧАСОВ</small></div>
      <div class="timer-item"><span id="minutes">00</span><small>МИНУТ</small></div>
      <div class="timer-item"><span id="seconds">00</span><small>СЕКУНД</small></div>
    </div>
    <p style="text-align: center; font-family: var(--font-mono); font-weight:bold; margin-bottom: 20px; text-transform:uppercase;">ДО ВЫХОДА ГЛАВЫ</p>
    
    <div id="news-list"></div>
    
    <div class="glass-card">
      <h3 style="margin-bottom: 15px; font-family: var(--font-mono);">СОЦСЕТИ</h3>
      <div class="social-buttons">
        <a class="social-circle" href="https://vk.ru/club243505000" target="_blank"><svg viewBox="0 0 24 24"><path d="M12.785 16.241s.288-.032.436-.194c.136-.148.132-.427.132-.427s-.02-1.304.576-1.496c.588-.19 1.341 1.26 2.14 1.817.605.421 1.064.329 1.064.329l2.141-.03s1.12-.07.589-.955c-.043-.073-.31-.66-1.595-1.87-1.346-1.267-1.165-1.061.455-3.25.988-1.333 1.383-2.146 1.259-2.494-.118-.331-.85-.243-.85-.243l-2.41.015s-.179-.025-.311.055c-.13.078-.213.26-.213.26s-.381 1.02-.888 1.888c-1.07 1.83-1.498 1.927-1.673 1.813-.407-.263-.305-1.06-.305-1.626 0-1.769.267-2.506-.521-2.697-.261-.063-.454-.105-1.122-.112-.858-.008-1.583.003-1.994.206-.274.135-.485.435-.356.452.159.021.518.098.708.358.246.336.237 1.09.237 1.09s.142 2.082-.331 2.34c-.324.177-.77-.184-1.727-1.834-.49-.85-.86-1.79-.86-1.79s-.071-.175-.198-.269c-.154-.113-.369-.149-.369-.149l-2.291.015s-.343.01-.469.16c-.111.134-.009.41-.009.41s1.791 4.207 3.821 6.326c1.86 1.943 3.973 1.815 3.973 1.815h.957z"/></svg></a>
        <a class="social-circle" href="https://max.ru/join/ylMjxKflnID_KjXrCiA-0Erc5qG4X993ndCYjbxTeOg" target="_blank"><svg viewBox="0 0 24 24"><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-1.5 14.5v-9l6 4.5-6 4.5z"/></svg></a>
        <a class="social-circle" href="https://t.me/frendzonar" target="_blank"><svg viewBox="0 0 24 24"><path d="M9.78 18.65l.28-4.23 7.68-6.92c.34-.31-.07-.46-.52-.19L7.74 13.3 3.64 12c-.88-.25-.89-.86.2-1.3l15.97-6.16c.73-.33 1.43.18 1.15 1.3l-2.72 12.81c-.19.91-.74 1.13-1.5.71L12.6 16.3l-1.99 1.93c-.23.23-.42.42-.83.42z"/></svg></a>
        <a class="social-circle" href="https://chat.whatsapp.com/CJL6KOsRKWFKA7dbCtxuaA" target="_blank"><svg viewBox="0 0 24 24"><path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L0 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893A11.821 11.821 0 0020.46 3.488"/></svg></a>
        <a class="social-circle" href="https://www.tiktok.com/@frenddzona" target="_blank"><svg viewBox="0 0 24 24"><path d="M19.59 6.69a4.83 4.83 0 0 1-3.77-4.25V2h-3.45v13.67a2.89 2.89 0 0 1-5.2 1.74 2.89 2.89 0 0 1 2.31-4.64 2.93 2.93 0 0 1 .88.13V9.4a6.84 6.84 0 0 0-1-.05A6.33 6.33 0 0 0 5 20.1a6.34 6.34 0 0 0 10.86-4.43v-7a8.16 8.16 0 0 0 4.77 1.52v-3.4a4.85 4.85 0 0 1-1-.1z"/></svg></a>
        <a class="social-circle" href="https://www.youtube.com/@frendzonar" target="_blank"><svg viewBox="0 0 24 24"><path d="M23.498 6.186a3.016 3.016 0 0 0-2.122-2.136C19.505 3.545 12 3.545 12 3.545s-7.505 0-9.377.505A3.017 3.017 0 0 0 .502 6.186C0 8.07 0 12 0 12s0 3.93.502 5.814a3.016 3.016 0 0 0 2.122 2.136c1.871.505 9.376.505 9.376.505s7.505 0 9.377-.505a3.015 3.015 0 0 0 2.122-2.136C24 15.93 24 12 24 12s0-3.93-.502-5.814zM9.545 15.568V8.432L15.818 12l-6.273 3.568z"/></svg></a>
      </div>
    </div>
  </section>

  <!-- ПЕРСОНАЖИ -->
  <section class="page-section" id="page-characters">
    <h1 class="section-title">ПЕРСОНАЖИ</h1>
    
    <!-- Френдзона -->
    <div class="glass-card" onclick="toggleCharacters('friendzone')" style="cursor: pointer;">
      <h2 style="font-family: var(--font-mono);">💔 ФРЕНДЗОНА</h2>
      <p style="margin-top: 10px; color: #555; font-size:0.8em;">[ НАЖМИ ДЛЯ СПИСКА ]</p>
    </div>
    <div class="characters-list" id="list-friendzone">
      <div class="character-item" style="flex-direction: column; align-items: flex-start; gap: 8px;">
        <div style="display: flex; align-items: center; gap: 15px;"><div class="character-avatar">И</div><div class="character-info"><h4>ИВАН</h4></div></div>
        <p style="color: #444; font-size: 0.85em; line-height: 1.5; margin-top: 5px; padding-left: 65px;">Ивану 18 лет и он учится в медицинском колледже. Он из Украины (2015 год). Главный герой, общительный. Дружит с Игорем с детства, когда приехал в Беларусь к бабушке.</p>
      </div>
      <div class="character-item" style="flex-direction: column; align-items: flex-start; gap: 8px;">
        <div style="display: flex; align-items: center; gap: 15px;"><div class="character-avatar">М</div><div class="character-info"><h4>МОНИКА</h4></div></div>
        <p style="color: #444; font-size: 0.85em; line-height: 1.5; margin-top: 5px; padding-left: 65px;">Монике 17 лет, она живет в Испании и учится в медицинском колледже. Второй главный герой. Общительная, но не слишком дружелюбная. Познакомилась с Иваном в чат-рулетке. Плохо относится к Игорю.</p>
      </div>
      <div class="character-item"><div class="character-avatar">Н</div><div class="character-info"><h4>НАСТЯ</h4></div></div>
      <div class="character-item"><div class="character-avatar">П</div><div class="character-info"><h4>ПОЛИНА</h4></div></div>
      <div class="character-item"><div class="character-avatar">И</div><div class="character-info"><h4>ИГОРЬ</h4></div></div>
      <div class="character-item"><div class="character-avatar">А</div><div class="character-info"><h4>АВРОРА</h4></div></div>
    </div>

    <!-- Лавандовая весна -->
    <div class="glass-card" onclick="toggleCharacters('spring')" style="cursor: pointer; margin-top: 20px;">
      <h2 style="font-family: var(--font-mono);">🌸 ЛАВАНДОВАЯ ВЕСНА</h2>
      <p style="margin-top: 10px; color: #555; font-size:0.8em;">[ НАЖМИ ДЛЯ СПИСКА ]</p>
    </div>
    <div class="characters-list" id="list-spring">
      <div class="character-item"><div class="character-avatar" style="background: #000;">Д</div><div class="character-info"><h4>ДЭН</h4></div></div>
      <div class="character-item"><div class="character-avatar" style="background: #000;">Н</div><div class="character-info"><h4>НАЦУКИ</h4></div></div>
      <div class="character-item"><div class="character-avatar" style="background: #000;">Ю</div><div class="character-info"><h4>ЮКИ</h4></div></div>
      <div class="character-item"><div class="character-avatar" style="background: #000;">К</div><div class="character-info"><h4>КАМЕЛЯ</h4></div></div>
      <div class="character-item"><div class="character-avatar" style="background: #000;">С</div><div class="character-info"><h4>САША (ЭРНЕСТ)</h4></div></div>
      <div class="character-item"><div class="character-avatar" style="background: #000;">С</div><div class="character-info"><h4>САНЯ (АЗУЛИН)</h4></div></div>
      <div class="character-item"><div class="character-avatar" style="background: #000;">П</div><div class="character-info"><h4>ПИКМИ (ВИКА)</h4></div></div>
      <div class="character-item"><div class="character-avatar" style="background: #000;">Р</div><div class="character-info"><h4>РЕЙ</h4></div></div>
    </div>

    <!-- У любви нет границ -->
    <div class="glass-card" onclick="toggleCharacters('love')" style="cursor: pointer; margin-top: 20px;">
      <h2 style="font-family: var(--font-mono);">❤️ У ЛЮБВИ НЕТ ГРАНИЦ</h2>
      <p style="margin-top: 10px; color: #555; font-size:0.8em;">[ СКОРО ]</p>
    </div>
    <div class="characters-list" id="list-love">
      <div class="character-item"><div class="character-avatar" style="background: #ff0000;">?</div><div class="character-info"><h4>СКОРО...</h4></div></div>
    </div>
  </section>

  <footer style="text-align: center; padding: 25px; border-top: 3px solid #000; font-family: var(--font-mono); font-size: 0.8em; font-weight: bold;">
    © 2026 FRNZ STORY
  </footer>

  <button class="scroll-top" id="scroll-top" onclick="window.scrollTo({top: 0, behavior: 'smooth'})">↑</button>
  <div id="toast-container"></div>

  <script>
    // ================= 1. ПРОГРЕСС-БАР =================
    window.addEventListener('scroll', () => {
      const scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
      const scrollHeight = document.documentElement.scrollHeight - document.documentElement.clientHeight;
      const scrolled = (scrollTop / scrollHeight) * 100;
      document.getElementById('progress-bar').style.width = scrolled + '%';
      const btn = document.getElementById('scroll-top');
      if (window.scrollY > 300) btn.classList.add('show'); else btn.classList.remove('show');
    });

    // ================= 2. ПРОСМОТРЫ И ЛОГИ =================
    let views = parseInt(localStorage.getItem('frendzona_views')) || 0;
    views++;
    localStorage.setItem('frendzona_views', views);
    document.getElementById('views-count').textContent = views;
    
    // Логирование действий (для админа)
    function logAction(action) {
      let logs = JSON.parse(localStorage.getItem('frendzona_logs')) || [];
      logs.push({ time: new Date().toLocaleString(), action: action });
      if(logs.length > 50) logs.shift(); // Храним только последние 50
      localStorage.setItem('frendzona_logs', JSON.stringify(logs));
    }
    logAction('Открытие сайта');

    function showLogs() {
      let logs = JSON.parse(localStorage.getItem('frendzona_logs')) || [];
      let logStr = logs.map(l => `[${l.time}] ${l.action}`).join('\n');
      alert("ЛОГИ ДЕЙСТВИЙ:\n\n" + logStr);
    }

    // ================= 3. ВЕРИФИКАЦИЯ =================
    let currentStatus = localStorage.getItem('frendzona_verify_status') || 'none'; 

    function updateVerifyUI() {
      const badge = document.getElementById('verify-badge');
      const text = document.getElementById('verify-text');
      badge.className = 'verify-badge';
      
      if(currentStatus === 'admin') {
        badge.classList.add('verified-admin');
        text.textContent = 'ADMIN';
      } else if(currentStatus === 'bot') {
        badge.classList.add('verified-bot');
        text.textContent = 'BOT';
      } else if(currentStatus === 'user') {
        badge.classList.add('verified-user');
        text.textContent = 'USER';
      } else {
        text.textContent = 'NO VERIFY';
      }
    }
    updateVerifyUI();

    // ================= 4. МЕЙ МЕЙ / ВИТАЛИК (НОВЫЕ ФРАЗЫ) =================
    const mayPhrases = [
      "Мяу! Я Мей Мей 🐾", "Виталик, ты где? 🐈", "Мы с Виталиком друзья! 🤝",
      "Таймер тикает! Скоро новая глава ⏳", "2015 год... Ностальгия ✨",
      "Иван из Украины 🇺🇦", "Моника из Испании 🇪🇸", "Игорь и Иван дружат с детства 🤝",
      "Настя, Полина и Аврора скоро появятся! 🌸",
      "Ты знал(а), что я снежный барс? 🐆", "Попробуй перетащить меня! 🖱️",
      "12 тем на любой вкус! 🎨", "Виталик любит рыбу 🐟", "Мей Мей любит мурчать 🎵",
      "Я умею прыгать! Нажми на меня! 🦘", "QR-код теперь подстраивается под тему! 📱",
      "Не забудь подписаться на соцсети! 💖", "Лавандовая весна уже здесь! 🌿",
      "Саша, Саня, Вика и Рей! 🌟", "Дэн, Нацуки, Юки и Камеля! 🌸"
    ];
    const vitalikPhrases = [
      "Мяу! Я Виталик 🐈", "Мей Мей, ты где? 🐆", "Мы с Мей Меем друзья! 🤝",
      "Я пушистый и рыжий! 🟠", "Люблю погреться на солнышке ☀️",
      "Таймер тикает! Скоро новая глава ⏳", "Иван и Моника... интересно 👀",
      "Попробуй перетащить меня! 🖱️", "Мяу-мяу! 🐾", "Виталик одобряет этот сайт! ✅",
      "Я тоже умею прыгать! Попробуй! 🦘", "Мей Мей, давай играть! 🎾",
      "У меня самый красивый хвост! 🐈", "Рыжие коты приносят удачу! 🍀",
      "Не забудь про QR-код! 📱", "Люблю смотреть на звезды ✨"
    ];
    
    let currentHelper = localStorage.getItem('frendzona_helper') || 'may';
    let mayTimeout;

    function setHelper(type) {
      currentHelper = type;
      localStorage.setItem('frendzona_helper', type);
      document.getElementById('view-may').style.display = type === 'may' ? 'block' : 'none';
      document.getElementById('view-vitalik').style.display = type === 'vitalik' ? 'block' : 'none';
      document.getElementById('btn-may').classList.toggle('active', type === 'may');
      document.getElementById('btn-vitalik').classList.toggle('active', type === 'vitalik');
      showToast(type === 'may' ? 'ВЫБРАН МЕЙ МЕЙ' : 'ВЫБРАН ВИТАЛИК');
      hideBubble();
      
      const helper = document.getElementById('helper');
      helper.style.transform = 'scale(1.2)';
      setTimeout(() => { helper.style.transform = 'scale(1)'; }, 200);
      logAction('Смена помощника на ' + type);
    }
    
    if(currentHelper === 'vitalik') setHelper('vitalik'); else setHelper('may');

    // Цвета для каждого отдельно
    function setHelperColor(color, type) {
      if(type === 'may') {
        document.documentElement.style.setProperty('--may-color', color);
        localStorage.setItem('frendzona_may_color', color);
      } else {
        document.documentElement.style.setProperty('--vitalik-color', color);
        localStorage.setItem('frendzona_vitalik_color', color);
      }
      showToast('ЦВЕТ ИЗМЕНЕН');
      logAction('Смена цвета ' + type);
    }
    const savedMayColor = localStorage.getItem('frendzona_may_color');
    if(savedMayColor) document.documentElement.style.setProperty('--may-color', savedMayColor);
    const savedVitColor = localStorage.getItem('frendzona_vitalik_color');
    if(savedVitColor) document.documentElement.style.setProperty('--vitalik-color', savedVitColor);

    // 3D toggle
    function toggle3D() {
      document.body.classList.toggle('use-3d');
      const is3D = document.body.classList.contains('use-3d');
      document.getElementById('btn-3d').textContent = is3D ? 'ОТКЛЮЧИТЬ 3D' : 'ВКЛЮЧИТЬ 3D';
      localStorage.setItem('frendzona_3d', is3D);
    }
    if(localStorage.getItem('frendzona_3d') === 'false') {
      document.body.classList.remove('use-3d');
      document.getElementById('btn-3d').textContent = 'ВКЛЮЧИТЬ 3D';
    }

    function helperTalk(e) {
      if (isDragging) return;
      const bubble = document.getElementById('helper-bubble');
      const helper = document.getElementById('helper');
      clearTimeout(mayTimeout);
      
      helper.classList.add('jump');
      setTimeout(() => { helper.classList.remove('jump'); }, 400);

      const phrases = currentHelper === 'may' ? mayPhrases : vitalikPhrases;
      const randomPhrase = phrases[Math.floor(Math.random() * phrases.length)];
      bubble.textContent = randomPhrase;
      bubble.classList.add('show');
      mayTimeout = setTimeout(() => {
        bubble.classList.remove('show');
      }, 3000);
    }

    function hideBubble() { document.getElementById('helper-bubble').classList.remove('show'); }

    // ================= 5. ПЕРЕТАСКИВАНИЕ =================
    let isDragging = false;
    let offsetX, offsetY;
    const helper = document.getElementById('helper');

    const savedPosX = localStorage.getItem('frendzona_helper_x');
    const savedPosY = localStorage.getItem('frendzona_helper_y');
    if (savedPosX && savedPosY) {
      helper.style.left = savedPosX + 'px';
      helper.style.top = savedPosY + 'px';
      helper.style.bottom = 'auto';
    }

    function startDrag(e) {
      isDragging = true;
      hideBubble();
      const clientX = e.type.includes('mouse') ? e.clientX : e.touches[0].clientX;
      const clientY = e.type.includes('mouse') ? e.clientY : e.touches[0].clientY;
      const rect = helper.getBoundingClientRect();
      offsetX = clientX - rect.left;
      offsetY = clientY - rect.top;
      document.addEventListener('mousemove', onDrag);
      document.addEventListener('touchmove', onDrag, { passive: false });
      document.addEventListener('mouseup', stopDrag);
      document.addEventListener('touchend', stopDrag);
    }

    function onDrag(e) {
      if (!isDragging) return;
      e.preventDefault();
      const clientX = e.type.includes('mouse') ? e.clientX : e.touches[0].clientX;
      const clientY = e.type.includes('mouse') ? e.clientY : e.touches[0].clientY;
      let newX = clientX - offsetX;
      let newY = clientY - offsetY;
      const maxX = window.innerWidth - helper.offsetWidth;
      const maxY = window.innerHeight - helper.offsetHeight;
      newX = Math.max(0, Math.min(newX, maxX));
      newY = Math.max(0, Math.min(newY, maxY));
      helper.style.left = newX + 'px';
      helper.style.top = newY + 'px';
      helper.style.bottom = 'auto';
    }

    function stopDrag() {
      if (!isDragging) return;
      isDragging = false;
      localStorage.setItem('frendzona_helper_x', parseInt(helper.style.left));
      localStorage.setItem('frendzona_helper_y', parseInt(helper.style.top));
      document.removeEventListener('mousemove', onDrag);
      document.removeEventListener('touchmove', onDrag);
      document.removeEventListener('mouseup', stopDrag);
      document.removeEventListener('touchend', stopDrag);
    }

    // ================= 6. СЕКРЕТ ЛОГОТИПА (ГЛИТЧ) =================
    let logoClicks = 0;
    document.getElementById('logo-secret').addEventListener('click', () => {
      logoClicks++;
      if(logoClicks === 5) {
        showToast("🎉 СЕКРЕТНЫЙ БОНУС!");
        document.body.style.animation = "glitch 0.5s";
        setTimeout(() => { document.body.style.animation = ""; }, 500);
        logoClicks = 0;
      }
      setTimeout(() => { logoClicks = 0; }, 2000);
    });

    // ================= 7. АДАПТИВНЫЙ QR-КОД =================
    let qrStyle = localStorage.getItem('frendzona_qr_style') || 'normal';
    let qrCodeObj = null;
    let currentBg = localStorage.getItem('frendzona_bg') || '#d9d9d9';
    let currentAccent = localStorage.getItem('frendzona_accent') || '#0000ff';

    function generateQR() {
      const container = document.getElementById('qrcode');
      if(!container) return;
      container.innerHTML = '';
      
      let darkColor = "#000000";
      let lightColor = "#ffffff";
      
      if(qrStyle === 'invert') {
        darkColor = "#ffffff";
        lightColor = "#000000";
      } else if(qrStyle === 'color') {
        darkColor = currentAccent;
        lightColor = "#ffffff";
      } else {
        const isDarkTheme = currentBg !== '#d9d9d9' && currentBg !== '#ccff00' && currentBg !== '#ff69b4' && currentBg !== '#ffcc00';
        if(isDarkTheme) {
          darkColor = "#ffffff";
          lightColor = "#000000";
        } else {
          darkColor = "#000000";
          lightColor = "#ffffff";
        }
      }

      qrCodeObj = new QRCode(container, {
        text: window.location.href,
        width: 180,
        height: 180,
        colorDark: darkColor,
        colorLight: lightColor,
        correctLevel: QRCode.CorrectLevel.H
      });
    }

    function setQRStyle(style, btn) {
      qrStyle = style;
      localStorage.setItem('frendzona_qr_style', style);
      document.querySelectorAll('.qr-style-btn').forEach(b => b.classList.remove('active'));
      if(btn) btn.classList.add('active');
      generateQR();
      showToast('СТИЛЬ QR ИЗМЕНЕН');
    }

    document.querySelectorAll('.qr-style-btn').forEach(b => {
      if(b.textContent.toLowerCase().includes(qrStyle === 'invert' ? 'инверт' : qrStyle === 'color' ? 'цвет' : 'обычн')) {
        b.classList.add('active');
      }
    });

    function toggleQR() {
      document.getElementById('qr-modal').classList.toggle('show');
      if(burger.classList.contains('active')) toggleMenu();
    }

    // ================= 8. ВХОД / РЕГИСТРАЦИЯ =================
    let failedAttempts = parseInt(localStorage.getItem('frendzona_fails')) || 0;
    let lockoutUntil = parseInt(localStorage.getItem('frendzona_lockout')) || 0;

    function openLogin() {
      document.getElementById('login-modal').classList.add('show');
      if(burger.classList.contains('active')) toggleMenu();
      
      const now = Date.now();
      if(now < lockoutUntil) {
        document.getElementById('btn-login-submit').disabled = true;
        document.getElementById('login-error').style.display = 'block';
        document.getElementById('login-error').textContent = 'Заблокировано на 10 секунд';
      } else {
        document.getElementById('btn-login-submit').disabled = false;
        document.getElementById('login-error').style.display = 'none';
      }
    }
    function closeLogin() { document.getElementById('login-modal').classList.remove('show'); }
    function openReg() {
      document.getElementById('reg-modal').classList.add('show');
      if(burger.classList.contains('active')) toggleMenu();
    }
    function closeReg() { document.getElementById('reg-modal').classList.remove('show'); }

    function checkLogin() {
      const now = Date.now();
      if(now < lockoutUntil) {
        showToast('❌ ПОДОЖДИ 10 СЕКУНД');
        return;
      }

      const code = document.getElementById('login-code').value.trim();
      if (code === 'adminfrzz') {
        isAdmin = true;
        currentStatus = 'admin';
        localStorage.setItem('frendzona_verify_status', 'admin');
        localStorage.setItem('frendzona_fails', 0);
        updateVerifyUI();
        closeLogin();
        openAdminPanel();
        showToast('👑 ВЫ ВОШЛИ КАК АДМИН');
        logAction('Вход админа');
      } else {
        failedAttempts++;
        localStorage.setItem('frendzona_fails', failedAttempts);
        
        if(failedAttempts >= 3) {
          lockoutUntil = Date.now() + 10000;
          localStorage.setItem('frendzona_lockout', lockoutUntil);
          document.getElementById('btn-login-submit').disabled = true;
          document.getElementById('login-error').style.display = 'block';
          document.getElementById('login-error').textContent = 'Заблокировано на 10 секунд';
          showToast('❌ 3 НЕВЕРНЫХ ПОПЫТКИ. БЛОКИРОВКА 10 СЕК');
          setTimeout(() => {
            document.getElementById('btn-login-submit').disabled = false;
            document.getElementById('login-error').style.display = 'none';
            failedAttempts = 0;
            localStorage.setItem('frendzona_fails', 0);
          }, 10000);
        } else {
          showToast(`❌ НЕВЕРНЫЙ КОД (${failedAttempts}/3)`);
        }
      }
    }

    function registerUser() {
      const login = document.getElementById('reg-login').value.trim();
      const pass = document.getElementById('reg-password').value.trim();
      if(login && pass) {
        currentStatus = 'user';
        localStorage.setItem('frendzona_verify_status', 'user');
        updateVerifyUI();
        closeReg();
        showToast('✅ РЕГИСТРАЦИЯ УСПЕШНА');
        logAction('Регистрация пользователя');
      } else {
        showToast('❌ ЗАПОЛНИТЕ ПОЛЯ');
      }
    }

    // ================= 9. ТАЙМЕР =================
    let targetDate = localStorage.getItem('frendzona_target_date') 
      ? new Date(localStorage.getItem('frendzona_target_date')).getTime() 
      : new Date(2026, 11, 31, 23, 59, 59).getTime(); 

    function updateTimer() {
      const now = new Date().getTime();
      const distance = targetDate - now;
      if (distance < 0) {
        document.getElementById('days').innerText = "00"; document.getElementById('hours').innerText = "00";
        document.getElementById('minutes').innerText = "00"; document.getElementById('seconds').innerText = "00";
        return;
      }
      const days = Math.floor(distance / (1000 * 60 * 60 * 24));
      const hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
      const minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
      const seconds = Math.floor((distance % (1000 * 60)) / 1000);
      document.getElementById('days').innerText = days < 10 ? '0' + days : days;
      document.getElementById('hours').innerText = hours < 10 ? '0' + hours : hours;
      document.getElementById('minutes').innerText = minutes < 10 ? '0' + minutes : minutes;
      document.getElementById('seconds').innerText = seconds < 10 ? '0' + seconds : seconds;
    }
    setInterval(updateTimer, 1000);
    updateTimer();

    function saveTimerDate() {
      if (!isAdmin) return;
      const dateInput = document.getElementById('timer-date-input').value;
      if (!dateInput) { showToast('ВЫБЕРИ ДАТУ!'); return; }
      const newDate = new Date(dateInput).getTime();
      if (isNaN(newDate)) { showToast('НЕВЕРНАЯ ДАТА!'); return; }
      targetDate = newDate;
      localStorage.setItem('frendzona_target_date', dateInput);
      updateTimer();
      showToast('✅ ТАЙМЕР ОБНОВЛЕН');
    }
    function fillTimerInput() {
      const savedDate = localStorage.getItem('frendzona_target_date');
      if (savedDate) document.getElementById('timer-date-input').value = savedDate;
      else document.getElementById('timer-date-input').value = '2026-12-31T23:59';
    }

    // ================= 10. РЕЖИМ ЧТЕНИЯ =================
    function toggleReadingMode() {
      document.body.classList.toggle('reading-mode');
      if(document.body.classList.contains('reading-mode')) {
        showToast('РЕЖИМ ЧТЕНИЯ ВКЛ'); closeAdminPanel();
      } else {
        showToast('РЕЖИМ ЧТЕНИЯ ВЫКЛ');
      }
    }

    // ================= 11. АНТИВИРУС =================
    let isScanning = false;
    function runVirusScan() {
      if (isScanning) return;
      isScanning = true;
      const status = document.getElementById('av-status');
      status.textContent = "СКАНИРОВАНИЕ...";
      setTimeout(() => {
        status.textContent = "ЗАЩИЩЕНО";
        showToast("🛡️ СКАНИРОВАНИЕ ЗАВЕРШЕНО");
        isScanning = false;
      }, 1500);
    }

    // ================= 12. ПОДЕЛИТЬСЯ / КОПИРОВАТЬ =================
    function shareSite() {
      if (navigator.share) {
        navigator.share({ title: 'Френдзона', text: 'Смотри новости!', url: window.location.href }).catch(() => {});
      } else {
        navigator.clipboard.writeText(window.location.href);
        showToast('📋 ССЫЛКА СКОПИРОВАНА');
      }
    }
    function copyEmail() {
      navigator.clipboard.writeText('help.frendzona.offs@gmail.com');
      showToast('📋 EMAIL СКОПИРОВАН');
    }

    // ================= 13. УВЕДОМЛЕНИЯ =================
    function showToast(message) {
      const container = document.getElementById('toast-container');
      const toast = document.createElement('div');
      toast.className = 'toast'; toast.textContent = message;
      container.appendChild(toast);
      setTimeout(() => toast.classList.add('show'), 10);
      setTimeout(() => { toast.classList.remove('show'); setTimeout(() => toast.remove(), 300); }, 2500);
    }

    // ================= 14. МЕНЮ =================
    const burger = document.getElementById('burger');
    const sideMenu = document.getElementById('side-menu');
    const menuOverlay = document.getElementById('menu-overlay');

    function toggleMenu() {
      burger.classList.toggle('active');
      sideMenu.classList.toggle('open');
      menuOverlay.classList.toggle('active');
    }
    burger.addEventListener('click', toggleMenu);
    menuOverlay.addEventListener('click', toggleMenu); 

    function showPage(pageId) {
      document.querySelectorAll('.page-section').forEach(p => p.classList.remove('active'));
      document.getElementById('page-' + pageId).classList.add('active');
      if(burger.classList.contains('active')) toggleMenu();
      window.scrollTo(0, 0);
    }

    // ================= 15. ПЕРСОНАЖИ =================
    function toggleCharacters(id) {
      document.getElementById('list-' + id).classList.toggle('show');
    }

    // ================= 16. ТЕМЫ =================
    const settingsBox = document.getElementById('settings-box');
    function toggleSettings() { settingsBox.classList.toggle('active'); }

    function applyTheme(bgColor, textColor, accentColor, btnElement) {
      document.documentElement.style.setProperty('--bg-color', bgColor);
      document.documentElement.style.setProperty('--text-main', textColor);
      document.documentElement.style.setProperty('--accent', accentColor);
      
      const isLight = bgColor === '#d9d9d9' || bgColor === '#ccff00' || bgColor === '#ff69b4' || bgColor === '#ffcc00' || bgColor === '#e6e6fa' || bgColor === '#98ff98' || bgColor === '#ff7e5f';
      if (isLight) {
        document.documentElement.style.setProperty('--text-muted', '#333');
        document.documentElement.style.setProperty('--card-bg', 'rgba(255,255,255,0.8)');
        document.documentElement.style.setProperty('--border-color', '#000');
      } else {
        document.documentElement.style.setProperty('--text-muted', '#aaa');
        document.documentElement.style.setProperty('--card-bg', 'rgba(0,0,0,0.5)');
        document.documentElement.style.setProperty('--border-color', '#fff');
      }
      
      localStorage.setItem('frendzona_bg', bgColor);
      localStorage.setItem('frendzona_text', textColor);
      localStorage.setItem('frendzona_accent', accentColor);
      
      currentBg = bgColor;
      currentAccent = accentColor;
      generateQR();

      document.querySelectorAll('.theme-btn').forEach(btn => btn.classList.remove('active'));
      if (btnElement) btnElement.classList.add('active');
    }

    const savedBg = localStorage.getItem('frendzona_bg') || '#d9d9d9';
    const savedText = localStorage.getItem('frendzona_text') || '#000000';
    const savedAccent = localStorage.getItem('frendzona_accent') || '#0000ff';
    applyTheme(savedBg, savedText, savedAccent, null);

    const themePresets = [
      {bg:'#d9d9d9', text:'#000', accent:'#0000ff'}, {bg:'#0f0f1a', text:'#fff', accent:'#00ffff'},
      {bg:'#ccff00', text:'#000', accent:'#ff00ff'}, {bg:'#0000ff', text:'#fff', accent:'#ffff00'},
      {bg:'#8b0000', text:'#fff', accent:'#000000'}, {bg:'#ff69b4', text:'#000', accent:'#ffffff'},
      {bg:'#1a0b2e', text:'#fff', accent:'#ff6b9d'}, {bg:'#ffcc00', text:'#000', accent:'#ff0000'},
      {bg:'#e6e6fa', text:'#000', accent:'#9370DB'}, {bg:'#98ff98', text:'#000', accent:'#0077FF'},
      {bg:'#004466', text:'#fff', accent:'#00aaff'}, {bg:'#ff7e5f', text:'#000', accent:'#ffdd00'}
    ];
    function randomTheme() {
      const t = themePresets[Math.floor(Math.random() * themePresets.length)];
      applyTheme(t.bg, t.text, t.accent, null);
      showToast('🎲 СЛУЧАЙНАЯ ТЕМА');
    }

    // ================= 17. НОВОСТИ =================
    let newsData = JSON.parse(localStorage.getItem('frendzona_news')) || [{ id: 1, date: "20 ноября 2026", title: "ДОБРО ПОЖАЛОВАТЬ!", text: "Сайт запущен.", tags: ["АНОНС"] }];
    let isAdmin = false; 

    function updateCounter() {
      const c = document.getElementById('news-count');
      if(c) c.textContent = newsData.length + ' новостей';
      const ac = document.getElementById('admin-news-count');
      if(ac) ac.textContent = newsData.length;
    }

    function renderNews() {
      const list = document.getElementById('news-list');
      if(!list) return;
      list.innerHTML = ''; 
      newsData.slice().reverse().forEach((news, index) => {
        const tagsHtml = news.tags.map(t => `<span style="font-size:0.8em; padding: 3px 10px; border: 1px solid #000; background: #fff; margin-right:5px; font-family:var(--font-mono); font-weight:bold;">#${t}</span>`).join('');
        const deleteBtn = isAdmin ? `<br><button class="delete-btn" style="display:inline-block; margin-top:10px; padding:5px 10px; background:red; color:white; border:none; cursor:pointer;" onclick="deleteNews(${news.id})">УДАЛИТЬ</button>` : '';
        list.innerHTML += `
          <div class="glass-card" style="animation-delay: ${index * 0.1}s;">
            <button class="copy-btn" onclick="copyNews('${news.title}', '${news.text.replace(/'/g, "\\'")}')">📋</button>
            <div style="color: var(--accent); font-weight: bold; font-size: 0.8em; text-transform: uppercase; margin-bottom: 5px; font-family:var(--font-mono);">${news.date}</div>
            <h3 style="font-family:var(--font-mono);">${news.title}</h3>
            <p style="color: var(--text-muted);">${news.text}</p>
            <div style="margin-top: 10px;">${tagsHtml}</div>
            ${deleteBtn}
          </div>
        `;
      });
      updateCounter();
    }

    function copyNews(title, text) {
      navigator.clipboard.writeText(`【${title}】\n${text}`);
      showToast('📋 СКОПИРОВАНО');
    }

    function addNews() {
      if (!isAdmin) return; 
      const title = document.getElementById('news-title').value.trim();
      const text = document.getElementById('news-text').value.trim();
      const tagsInput = document.getElementById('news-tags').value.trim();
      if (!title || !text) { showToast('ЗАПОЛНИ ПОЛЯ!'); return; }
      const tags = tagsInput ? tagsInput.split(',').map(t => t.trim()) : [];
      const now = new Date();
      const months = ['января', 'февраля', 'марта', 'апреля', 'мая', 'июня', 'июля', 'августа', 'сентября', 'октября', 'ноября', 'декабря'];
      newsData.push({ id: Date.now(), date: `${now.getDate()} ${months[now.getMonth()]} ${now.getFullYear()}`, title, text, tags });
      localStorage.setItem('frendzona_news', JSON.stringify(newsData)); 
      document.getElementById('news-title').value = ''; document.getElementById('news-text').value = ''; document.getElementById('news-tags').value = '';
      renderNews(); closeAdminPanel(); showToast('НОВОСТЬ ДОБАВЛЕНА');
      logAction('Добавлена новость: ' + title);
    }

    function deleteNews(id) {
      if (!isAdmin) return;
      if (confirm('Удалить?')) {
        newsData = newsData.filter(n => n.id !== id);
        localStorage.setItem('frendzona_news', JSON.stringify(newsData));
        renderNews(); showToast('УДАЛЕНО');
        logAction('Удалена новость');
      }
    }

    // Удалить все новости
    function deleteAllNews() {
      if (!isAdmin) return;
      if (confirm('УДАЛИТЬ ВСЕ НОВОСТИ?')) {
        newsData = [];
        localStorage.setItem('frendzona_news', JSON.stringify(newsData));
        renderNews(); closeAdminPanel(); showToast('ВСЕ НОВОСТИ УДАЛЕНЫ');
        logAction('Удалены все новости');
      }
    }

    // ================= 18. АДМИН-ПАНЕЛЬ =================
    function openAdminPanel() {
      if (!isAdmin) return;
      document.getElementById('admin-modal').classList.add('show');
      fillTimerInput(); 
      renderNews(); 
    }
    function closeAdminPanel() { document.getElementById('admin-modal').classList.remove('show'); }

    // ================= 19. СБРОС ВСЕГО =================
    function resetAll() {
      if(confirm('СБРОСИТЬ ВСЕ НАСТРОЙКИ И ДАННЫЕ?')) {
        localStorage.clear();
        location.reload();
      }
    }

    // ================= 20. ИНИЦИАЛИЗАЦИЯ =================
    renderNews();
    updateVerifyUI();
    generateQR();
  </script>
</body>
</html>
