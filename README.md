<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>DEFEND THE CAMPUS: VFSTR VADLAMUDI - ULTRA-FAST BULLET REFILL</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@500;700;900&family=Rajdhani:wght@600;700;800&display=swap');

    * {
      box-sizing: border-box;
      user-select: none;
      margin: 0;
      padding: 0;
    }
    body, html {
      width: 100%;
      height: 100%;
      overflow: hidden;
      background: #020617;
      font-family: 'Rajdhani', sans-serif;
      color: #fff;
    }
    #game-container {
      width: 100%;
      height: 100%;
      position: absolute;
      top: 0;
      left: 0;
      cursor: crosshair;
    }

    /* HUD */
    #hud {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      pointer-events: none;
      display: none;
      z-index: 10;
    }

    /* Top 6-Block Sector Progression Checklist */
    #sector-progress-bar {
      position: absolute;
      top: 8px;
      left: 50%;
      transform: translateX(-50%);
      display: flex;
      align-items: center;
      gap: 6px;
      background: rgba(15, 23, 42, 0.94);
      padding: 6px 14px;
      border: 1px solid rgba(245, 158, 11, 0.6);
      border-radius: 8px;
      box-shadow: 0 4px 25px rgba(0, 0, 0, 0.8), 0 0 15px rgba(245, 158, 11, 0.3);
      backdrop-filter: blur(8px);
      pointer-events: auto;
    }
    .sector-step {
      display: flex;
      align-items: center;
      gap: 5px;
      padding: 4px 9px;
      border-radius: 4px;
      font-family: 'Orbitron', sans-serif;
      font-size: 11px;
      font-weight: 800;
      letter-spacing: 0.5px;
      border: 1px solid rgba(255, 255, 255, 0.15);
      background: rgba(30, 41, 59, 0.7);
      color: #94a3b8;
      transition: all 0.3s ease;
    }
    .sector-step.cleared {
      background: rgba(16, 185, 129, 0.25);
      border-color: #10b981;
      color: #34d399;
      text-shadow: 0 0 8px rgba(16, 185, 129, 0.6);
    }
    .sector-step.active {
      background: linear-gradient(135deg, rgba(245, 158, 11, 0.4), rgba(239, 68, 68, 0.25));
      border-color: #f59e0b;
      color: #fbbf24;
      box-shadow: 0 0 14px rgba(245, 158, 11, 0.6);
      transform: scale(1.05);
      animation: pulseActive 1.4s infinite alternate;
    }
    .sector-step.locked {
      opacity: 0.45;
    }
    @keyframes pulseActive {
      0% { box-shadow: 0 0 8px rgba(245, 158, 11, 0.4); }
      100% { box-shadow: 0 0 18px rgba(245, 158, 11, 0.9); }
    }

    /* Compass Bar */
    #compass-container {
      position: absolute;
      top: 50px;
      left: 50%;
      transform: translateX(-50%);
      width: 440px;
      height: 34px;
      background: linear-gradient(180deg, rgba(15, 23, 42, 0.95), rgba(15, 23, 42, 0.6));
      border: 1px solid rgba(245, 158, 11, 0.65);
      border-radius: 6px;
      overflow: hidden;
      box-shadow: 0 0 20px rgba(245, 158, 11, 0.3);
      backdrop-filter: blur(8px);
    }
    #compass-tape {
      position: absolute;
      top: 0;
      left: 0;
      height: 100%;
      display: flex;
      align-items: center;
      transition: transform 0.04s linear;
      font-family: 'Orbitron', sans-serif;
      font-weight: 700;
      font-size: 12px;
      color: #cbd5e1;
    }
    .compass-marker { width: 36px; text-align: center; }
    .compass-cardinal { color: #f59e0b; font-weight: 900; font-size: 14px; text-shadow: 0 0 10px #f59e0b; }
    #compass-needle {
      position: absolute;
      top: 0;
      left: 50%;
      transform: translateX(-50%);
      width: 0;
      height: 0;
      border-left: 6px solid transparent;
      border-right: 6px solid transparent;
      border-top: 9px solid #f59e0b;
      z-index: 5;
    }

    /* Top Badges */
    #match-stats-badge {
      position: absolute;
      top: 90px;
      left: 50%;
      transform: translateX(-50%);
      display: flex;
      gap: 10px;
      pointer-events: auto;
    }
    .badge-pill {
      background: rgba(15, 23, 42, 0.92);
      border: 1px solid rgba(255, 255, 255, 0.25);
      border-radius: 4px;
      padding: 5px 14px;
      font-size: 13px;
      font-weight: 800;
      display: flex;
      align-items: center;
      gap: 6px;
      box-shadow: 0 4px 15px rgba(0,0,0,0.7);
      text-transform: uppercase;
      letter-spacing: 1px;
    }
    .pill-alive { color: #38bdf8; border-color: rgba(56, 189, 248, 0.7); }
    .pill-kills { color: #ef4444; border-color: rgba(239, 68, 68, 0.7); }
    .pill-zone { color: #f59e0b; border-color: rgba(245, 158, 11, 0.7); font-weight: 900; }
    .pill-weather { color: #a855f7; border-color: rgba(168, 85, 247, 0.7); font-weight: 900; }
    .pill-map { color: #10b981; border-color: rgba(16, 185, 129, 0.7); cursor: pointer; transition: transform 0.15s; }
    .pill-map:hover { transform: scale(1.05); }

    /* Kill Feed */
    #kill-feed {
      position: absolute;
      top: 15px;
      right: 20px;
      display: flex;
      flex-direction: column;
      gap: 6px;
      align-items: flex-end;
    }
    .kill-feed-item {
      background: linear-gradient(90deg, transparent, rgba(15, 23, 42, 0.95));
      border-right: 4px solid #ef4444;
      padding: 6px 14px;
      font-size: 14px;
      font-weight: 800;
      color: #e2e8f0;
      border-radius: 3px;
      animation: slideInRight 0.25s ease-out;
      box-shadow: 0 2px 10px rgba(0,0,0,0.6);
    }
    .kf-killer { color: #38bdf8; }
    .kf-weapon { color: #f59e0b; margin: 0 5px; }
    .kf-victim { color: #ef4444; }

    /* 3-Weapon Inventory HUD */
    #weapon-hud {
      position: absolute;
      bottom: 25px;
      right: 25px;
      display: flex;
      flex-direction: column;
      align-items: flex-end;
      gap: 8px;
    }
    .weapon-slots-bar {
      display: flex;
      gap: 8px;
      pointer-events: auto;
    }
    .slot-pill {
      background: rgba(15, 23, 42, 0.9);
      border: 1px solid rgba(255, 255, 255, 0.3);
      border-radius: 6px;
      padding: 6px 12px;
      font-size: 13px;
      font-weight: 800;
      color: #94a3b8;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 6px;
      transition: all 0.15s ease;
    }
    .slot-pill.active {
      background: linear-gradient(135deg, rgba(245, 158, 11, 0.35), rgba(15, 23, 42, 0.95));
      border-color: #f59e0b;
      color: #fbbf24;
      box-shadow: 0 0 12px rgba(245, 158, 11, 0.4);
      transform: translateY(-2px);
    }
    .weapon-box {
      background: linear-gradient(135deg, rgba(15, 23, 42, 0.95), rgba(30, 41, 59, 0.85));
      border: 2px solid #f59e0b;
      border-radius: 12px;
      padding: 14px 22px;
      display: flex;
      align-items: center;
      gap: 18px;
      box-shadow: 0 8px 30px rgba(0,0,0,0.8), 0 0 20px rgba(245, 158, 11, 0.3);
      backdrop-filter: blur(10px);
      position: relative;
      overflow: hidden;
    }
    .gun-details {
      display: flex;
      flex-direction: column;
      align-items: flex-end;
    }
    .gun-name {
      font-family: 'Orbitron', sans-serif;
      font-size: 18px;
      font-weight: 900;
      color: #f59e0b;
      letter-spacing: 1px;
    }
    .gun-type {
      font-size: 12px;
      font-weight: 700;
      color: #38bdf8;
      text-transform: uppercase;
    }
    .ammo-count {
      font-family: 'Orbitron', sans-serif;
      font-size: 38px;
      font-weight: 900;
      color: #f8fafc;
      letter-spacing: 2px;
      line-height: 1;
      margin-top: 4px;
      transition: color 0.1s;
    }
    .ammo-count.refilling {
      color: #34d399;
      text-shadow: 0 0 12px #10b981;
    }
    .ammo-max {
      font-size: 18px;
      color: #64748b;
      margin-left: 4px;
    }
    .reload-flash-bar {
      position: absolute;
      bottom: 0;
      left: 0;
      height: 4px;
      width: 0%;
      background: linear-gradient(90deg, #10b981, #38bdf8);
      box-shadow: 0 0 10px #10b981;
      transition: width 0.15s ease-out;
    }

    /* Player Health HUD (250 HP) */
    #health-hud {
      position: absolute;
      bottom: 25px;
      left: 25px;
      display: flex;
      flex-direction: column;
      gap: 8px;
      width: 320px;
    }
    .character-tag {
      font-family: 'Orbitron', sans-serif;
      font-size: 14px;
      font-weight: 800;
      color: #38bdf8;
      display: flex;
      align-items: center;
      justify-content: space-between;
      text-shadow: 0 0 10px rgba(56, 189, 248, 0.6);
    }
    .bar-wrapper {
      position: relative;
      width: 100%;
      height: 22px;
      background: rgba(15, 23, 42, 0.9);
      border: 1px solid rgba(255, 255, 255, 0.25);
      border-radius: 4px;
      overflow: hidden;
      box-shadow: 0 4px 15px rgba(0,0,0,0.6);
    }
    .bar-fill {
      height: 100%;
      width: 100%;
      transition: width 0.15s cubic-bezier(0.4, 0, 0.2, 1);
    }
    .hp-fill { background: linear-gradient(90deg, #10b981, #059669); box-shadow: 0 0 15px #10b981; }
    .bar-value {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      display: flex;
      align-items: center;
      justify-content: center;
      font-family: 'Orbitron', sans-serif;
      font-size: 12px;
      font-weight: 900;
      color: #fff;
      text-shadow: 0 1px 4px #000;
    }

    /* Supply Bonus Floating Badge */
    #supply-bonus {
      position: absolute;
      bottom: 95px;
      left: 25px;
      background: linear-gradient(90deg, rgba(16, 185, 129, 0.95), rgba(5, 150, 105, 0.8));
      border: 1px solid #34d399;
      border-radius: 6px;
      padding: 6px 14px;
      font-family: 'Orbitron', sans-serif;
      font-size: 13px;
      font-weight: 800;
      color: #fff;
      display: none;
      align-items: center;
      gap: 6px;
      box-shadow: 0 0 20px rgba(16, 185, 129, 0.6);
      animation: popIn 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    }

    /* Crosshair */
    #crosshair {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: 24px;
      height: 24px;
      pointer-events: none;
      display: flex;
      align-items: center;
      justify-content: center;
    }
    .ch-dot { width: 4px; height: 4px; background: #fff; border-radius: 50%; box-shadow: 0 0 5px #fff; }
    .ch-line { position: absolute; background: rgba(255,255,255,0.85); box-shadow: 0 0 4px #000; }
    .ch-top { width: 2px; height: 7px; top: 0; left: 11px; }
    .ch-bottom { width: 2px; height: 7px; bottom: 0; left: 11px; }
    .ch-left { width: 7px; height: 2px; left: 0; top: 11px; }
    .ch-right { width: 7px; height: 2px; right: 0; top: 11px; }

    #hitmarker {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: 32px;
      height: 32px;
      display: none;
      pointer-events: none;
      color: #ef4444;
      font-size: 24px;
      font-weight: 900;
      line-height: 32px;
      text-align: center;
    }

    /* Wave & Sector Notification Banners */
    #wave-banner {
      position: absolute;
      top: 18%;
      left: 50%;
      transform: translateX(-50%);
      background: linear-gradient(90deg, transparent, rgba(15, 23, 42, 0.95), transparent);
      padding: 10px 40px;
      border-top: 2px solid #a855f7;
      border-bottom: 2px solid #a855f7;
      display: none;
      flex-direction: column;
      align-items: center;
      gap: 4px;
      box-shadow: 0 0 30px rgba(168, 85, 247, 0.5);
      animation: popIn 0.35s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    }
    .wave-title { font-family: 'Orbitron', sans-serif; font-size: 20px; font-weight: 900; color: #c084fc; letter-spacing: 2px; }

    #sector-clear-banner {
      position: absolute;
      top: 26%;
      left: 50%;
      transform: translateX(-50%);
      background: linear-gradient(90deg, transparent, rgba(6, 78, 59, 0.95), transparent);
      padding: 16px 50px;
      border-top: 3px solid #10b981;
      border-bottom: 3px solid #10b981;
      display: none;
      flex-direction: column;
      align-items: center;
      gap: 6px;
      box-shadow: 0 0 40px rgba(16, 185, 129, 0.7);
      animation: popIn 0.35s cubic-bezier(0.175, 0.885, 0.32, 1.275);
      z-index: 20;
    }
    .sector-clear-title { font-family: 'Orbitron', sans-serif; font-size: 24px; font-weight: 900; color: #34d399; letter-spacing: 2px; text-shadow: 0 0 15px #10b981; }
    .sector-clear-sub { font-size: 15px; font-weight: 800; color: #e2e8f0; letter-spacing: 1px; }

    /* Resume Prompt */
    #resume-overlay {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(2, 6, 23, 0.7);
      backdrop-filter: blur(4px);
      display: none;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 15px;
      z-index: 100;
      cursor: pointer;
    }
    .resume-box {
      background: rgba(15, 23, 42, 0.95);
      border: 2px solid #f59e0b;
      border-radius: 12px;
      padding: 24px 36px;
      text-align: center;
      box-shadow: 0 0 40px rgba(245, 158, 11, 0.4);
    }
    .resume-title { font-family: 'Orbitron', sans-serif; font-size: 26px; font-weight: 900; color: #f59e0b; }
    .resume-sub { font-size: 15px; font-weight: 700; color: #cbd5e1; margin-top: 8px; }

    /* Minimap Radar */
    #radar-container {
      position: absolute;
      bottom: 25px;
      left: 360px;
      width: 130px;
      height: 130px;
      background: rgba(15, 23, 42, 0.88);
      border: 2px solid #38bdf8;
      border-radius: 50%;
      overflow: hidden;
      box-shadow: 0 0 20px rgba(56, 189, 248, 0.35);
      backdrop-filter: blur(6px);
    }
    #radar-canvas { width: 100%; height: 100%; }

    /* Full-Screen Military Satellite Map */
    #map-modal {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(2, 6, 23, 0.92);
      backdrop-filter: blur(12px);
      display: none;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      z-index: 50;
      pointer-events: auto;
    }
    .map-frame {
      width: 90%;
      max-width: 900px;
      height: 80vh;
      background: #090d16;
      border: 2px solid #10b981;
      border-radius: 12px;
      position: relative;
      display: flex;
      flex-direction: column;
      overflow: hidden;
      box-shadow: 0 0 50px rgba(16, 185, 129, 0.25);
    }
    .map-header {
      padding: 12px 20px;
      background: rgba(16, 185, 129, 0.15);
      border-bottom: 1px solid rgba(16, 185, 129, 0.4);
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    .map-header-title { font-family: 'Orbitron', sans-serif; font-size: 18px; font-weight: 900; color: #10b981; }
    .map-close-btn {
      background: rgba(239, 68, 68, 0.2);
      border: 1px solid #ef4444;
      color: #fca5a5;
      padding: 5px 12px;
      font-weight: 800;
      border-radius: 4px;
      cursor: pointer;
    }
    #big-map-canvas {
      flex: 1;
      width: 100%;
      height: 100%;
      background: radial-gradient(circle, #0f172a 0%, #020617 100%);
    }

    /* Start Screen & Tutorial */
    #start-screen {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: radial-gradient(circle at center, rgba(15, 23, 42, 0.95), rgba(2, 6, 23, 0.99));
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      z-index: 60;
    }
    .title-banner {
      font-family: 'Orbitron', sans-serif;
      font-size: 44px;
      font-weight: 900;
      background: linear-gradient(135deg, #f59e0b, #ef4444, #ec4899);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      text-shadow: 0 0 40px rgba(245, 158, 11, 0.5);
      letter-spacing: 3px;
      margin-bottom: 4px;
      text-align: center;
    }
    .sub-title {
      font-size: 18px;
      font-weight: 800;
      color: #38bdf8;
      letter-spacing: 2px;
      text-transform: uppercase;
      margin-bottom: 24px;
    }
    .tutorial-card {
      background: rgba(15, 23, 42, 0.85);
      border: 1px solid rgba(255, 255, 255, 0.15);
      border-radius: 12px;
      padding: 20px 30px;
      width: 650px;
      max-width: 90%;
      margin-bottom: 24px;
      box-shadow: 0 10px 40px rgba(0,0,0,0.8);
    }
    .tutorial-grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 12px;
      font-size: 14px;
      font-weight: 700;
      color: #cbd5e1;
    }
    .t-item { display: flex; align-items: center; gap: 8px; }
    .key-badge {
      background: #1e293b;
      border: 1px solid #f59e0b;
      color: #fbbf24;
      font-family: 'Orbitron', sans-serif;
      font-size: 11px;
      font-weight: 900;
      padding: 3px 8px;
      border-radius: 4px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.5);
    }
    .btn-start {
      background: linear-gradient(135deg, #f59e0b, #d97706);
      border: none;
      color: #020617;
      font-family: 'Orbitron', sans-serif;
      font-size: 20px;
      font-weight: 900;
      padding: 14px 48px;
      border-radius: 8px;
      cursor: pointer;
      box-shadow: 0 0 35px rgba(245, 158, 11, 0.6);
      transition: all 0.2s cubic-bezier(0.175, 0.885, 0.32, 1.275);
      letter-spacing: 2px;
    }
    .btn-start:hover {
      transform: scale(1.06);
      box-shadow: 0 0 50px rgba(245, 158, 11, 0.9);
    }

    /* Booyah Victory Screen (U SAVED VIGNAN) */
    #booyah-banner {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: radial-gradient(circle at center, rgba(16, 185, 129, 0.4), rgba(2, 6, 23, 0.97));
      display: none;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      z-index: 80;
    }
    .booyah-text {
      font-family: 'Orbitron', sans-serif;
      font-size: 76px;
      font-weight: 900;
      color: #fbbf24;
      text-shadow: 0 0 50px #f59e0b, 0 0 100px #d97706;
      letter-spacing: 6px;
      animation: booyahPulse 1.2s infinite alternate;
    }
    .saved-vignan-title {
      font-family: 'Orbitron', sans-serif;
      font-size: 38px;
      font-weight: 900;
      color: #34d399;
      letter-spacing: 4px;
      margin-top: 10px;
      text-align: center;
      text-shadow: 0 0 30px #10b981;
    }
    .booyah-sub {
      font-family: 'Orbitron', sans-serif;
      font-size: 20px;
      font-weight: 800;
      color: #38bdf8;
      letter-spacing: 2px;
      margin-top: 6px;
      text-align: center;
    }
    .booyah-card {
      background: rgba(15, 23, 42, 0.92);
      border: 2px solid #10b981;
      border-radius: 12px;
      padding: 24px 44px;
      margin-top: 20px;
      text-align: center;
      font-size: 17px;
      font-weight: 700;
      color: #e2e8f0;
      line-height: 1.9;
      box-shadow: 0 0 50px rgba(16, 185, 129, 0.35);
    }
    .btn-restart {
      margin-top: 24px;
      background: linear-gradient(135deg, #10b981, #059669);
      border: none;
      color: #fff;
      font-family: 'Orbitron', sans-serif;
      font-size: 18px;
      font-weight: 900;
      padding: 12px 36px;
      border-radius: 8px;
      cursor: pointer;
      box-shadow: 0 0 30px rgba(16, 185, 129, 0.5);
    }

    /* Game Over Screen */
    #gameover-banner {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: radial-gradient(circle at center, rgba(239, 68, 68, 0.35), rgba(2, 6, 23, 0.96));
      display: none;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      z-index: 80;
    }
    .gameover-text {
      font-family: 'Orbitron', sans-serif;
      font-size: 64px;
      font-weight: 900;
      color: #ef4444;
      text-shadow: 0 0 40px #dc2626;
      letter-spacing: 5px;
    }

    @keyframes popIn {
      0% { transform: translate(-50%, -10px) scale(0.9); opacity: 0; }
      100% { transform: translate(-50%, 0) scale(1); opacity: 1; }
    }
    @keyframes slideInRight {
      0% { transform: translateX(50px); opacity: 0; }
      100% { transform: translateX(0); opacity: 1; }
    }
    @keyframes booyahPulse {
      0% { transform: scale(1); filter: brightness(1); }
      100% { transform: scale(1.05); filter: brightness(1.25); }
    }
  </style>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
</head>
<body>
  <div id="game-container"></div>

  <!-- HUD -->
  <div id="hud">
    <!-- Top 6-Block Sector Progression Checklist -->
    <div id="sector-progress-bar">
      <div class="sector-step active" id="sec-step-0"><span class="step-num">1</span> A-BLOCK</div>
      <div class="sector-step locked" id="sec-step-1"><span class="step-num">2</span> NTR LIB</div>
      <div class="sector-step locked" id="sec-step-2"><span class="step-num">3</span> H-BLOCK</div>
      <div class="sector-step locked" id="sec-step-3"><span class="step-num">4</span> N-BLOCK</div>
      <div class="sector-step locked" id="sec-step-4"><span class="step-num">5</span> U-BLOCK</div>
      <div class="sector-step locked" id="sec-step-5"><span class="step-num">6</span> STADIUM</div>
    </div>

    <!-- Compass Bar -->
    <div id="compass-container">
      <div id="compass-needle"></div>
      <div id="compass-tape"></div>
    </div>

    <!-- Match Stats -->
    <div id="match-stats-badge">
      <div class="badge-pill pill-alive">👤 REMAINING: <span id="alive-count">6</span></div>
      <div class="badge-pill pill-kills">💀 KILLS: <span id="kill-count">0</span></div>
      <div class="badge-pill pill-zone">📍 <span id="zone-name">A BLOCK</span></div>
      <div class="badge-pill pill-weather">🌤️ <span id="weather-name">MORNING SUN</span></div>
      <div class="badge-pill pill-map" id="btn-toggle-map">🗺️ [M] MAP</div>
      <div class="badge-pill" style="color:#a855f7;">🎥 [V] <span id="cam-mode">TPS</span></div>
    </div>

    <!-- Kill Feed -->
    <div id="kill-feed"></div>

    <!-- Crosshair -->
    <div id="crosshair">
      <div class="ch-dot"></div>
      <div class="ch-line ch-top"></div>
      <div class="ch-line ch-bottom"></div>
      <div class="ch-line ch-left"></div>
      <div class="ch-line ch-right"></div>
    </div>
    <div id="hitmarker">✕</div>

    <!-- Wave Notification Banner -->
    <div id="wave-banner">
      <span class="wave-title" id="wave-banner-text">⚔️ WAVE 1 ENGAGING!</span>
    </div>

    <!-- Sector Clear Celebratory Banner -->
    <div id="sector-clear-banner">
      <span class="sector-clear-title" id="sector-clear-text">🎉 A-BLOCK SECURED!</span>
      <span class="sector-clear-sub" id="sector-clear-sub">+20 HP & AMMO 100% REFILLED • ADVANCING TO NTR LIBRARY</span>
    </div>

    <!-- Minimap Radar -->
    <div id="radar-container">
      <canvas id="radar-canvas"></canvas>
    </div>

    <!-- Player Health Bar (250 HP) -->
    <div id="health-hud">
      <div class="character-tag">
        <span>🛡️ VFSTR GUARDIAN</span>
        <span id="hp-text">250 / 250 HP</span>
      </div>
      <div class="bar-wrapper">
        <div class="bar-fill hp-fill" id="hp-bar" style="width: 100%;"></div>
        <div class="bar-value" id="hp-bar-val">250 HP</div>
      </div>
    </div>

    <!-- Supply Bonus Badge -->
    <div id="supply-bonus">
      <span>💊</span> +20 HP MEDKIT & FULL AMMO REFILL!
    </div>

    <!-- 3-Weapon Inventory HUD with Ultra-Fast Flash Bar -->
    <div id="weapon-hud">
      <div class="weapon-slots-bar">
        <div class="slot-pill active" id="slot-1" onclick="switchWeapon(0)">[1] V-7 AR</div>
        <div class="slot-pill" id="slot-2" onclick="switchWeapon(1)">[2] SHOTGUN</div>
        <div class="slot-pill" id="slot-3" onclick="switchWeapon(2)">[3] AWM SNIPER</div>
      </div>
      <div class="weapon-box">
        <div class="reload-flash-bar" id="reload-flash"></div>
        <div class="gun-details">
          <span class="gun-name" id="gun-name">V-7 TACTICAL RIFLE</span>
          <span class="gun-type" id="gun-type">AUTO • [R] FAST REFILL</span>
        </div>
        <div>
          <span class="ammo-count" id="ammo-clip">45</span>
          <span class="ammo-max">/ <span id="ammo-reserve">360</span></span>
        </div>
      </div>
    </div>
  </div>

  <!-- Full-Screen Military Satellite Map -->
  <div id="map-modal">
    <div class="map-frame">
      <div class="map-header">
        <div class="map-header-title">🛰️ VFSTR VADLAMUDI - CAMPUS TACTICAL MAP</div>
        <button class="map-close-btn" id="map-close-btn">[ESC / M] CLOSE MAP</button>
      </div>
      <canvas id="big-map-canvas"></canvas>
    </div>
  </div>

  <!-- Resume Prompt Overlay -->
  <div id="resume-overlay" onclick="requestLockAndResume()">
    <div class="resume-box">
      <div class="resume-title">⏸️ COMBAT PAUSED</div>
      <div class="resume-sub">CLICK ANYWHERE OR PRESS [ESC] TO RESUME BATTLE & 360° AIM</div>
    </div>
  </div>

  <!-- Start Screen & Tutorial -->
  <div id="start-screen">
    <h1 class="title-banner">DEFEND THE CAMPUS</h1>
    <div class="sub-title">VFSTR VADLAMUDI • OUTBREAK DEFENSE</div>

    <div class="tutorial-card">
      <div class="tutorial-grid">
        <div class="t-item"><span class="key-badge">W A S D</span> Move Survivor & 360° Steer</div>
        <div class="t-item"><span class="key-badge">MOUSE</span> 360° Aim & Turn Smoothly</div>
        <div class="t-item"><span class="key-badge">LEFT CLICK</span> Fire Weapon</div>
        <div class="t-item"><span class="key-badge">RIGHT CLICK</span> ADS Tactical Zoom</div>
        <div class="t-item"><span class="key-badge">R</span> Lightning Fast Ammo Refill</div>
        <div class="t-item"><span class="key-badge">SPACE</span> Physics Jump</div>
        <div class="t-item"><span class="key-badge">1 / 2 / 3</span> Switch V-7 / Shotgun / AWM</div>
        <div class="t-item"><span class="key-badge">M</span> Open Satellite Campus Map</div>
      </div>
    </div>

    <button class="btn-start" id="start-btn">DEPLOY TO VFSTR VADLAMUDI</button>
  </div>

  <!-- Booyah Victory Screen (U SAVED VIGNAN) -->
  <div id="booyah-banner">
    <div class="booyah-text">BOOYAH!</div>
    <div class="saved-vignan-title">🎉 U SAVED VIGNAN! 🎉</div>
    <div class="booyah-sub">🏆 VFSTR VADLAMUDI CAMPUS 100% SECURED & DEFENDED</div>
    <div class="booyah-card" id="booyah-stats">
      ALL 6 VFSTR VADLAMUDI BLOCKS DEFENDED!
    </div>
    <button class="btn-restart" onclick="restartMatch()">PLAY AGAIN</button>
  </div>

  <!-- Game Over Screen -->
  <div id="gameover-banner">
    <div class="gameover-text">DEFEATED</div>
    <p style="font-size: 18px; color: #cbd5e1; margin-top: 10px;">The Outbreak breached the campus perimeter.</p>
    <button class="btn-restart" style="background: #ef4444;" onclick="restartMatch()">TRY AGAIN</button>
  </div>

  <script>
    /* =========================================================================
       1. SYNTHETIC AUDIO ENGINE WITH RAPID REFILL SOUNDS
       ========================================================================= */
    class FFAudio {
      constructor() { this.ctx = null; }
      init() {
        if (!this.ctx) {
          const AC = window.AudioContext || window.webkitAudioContext;
          this.ctx = new AC();
        }
        if (this.ctx.state === 'suspended') this.ctx.resume();
      }
      playShootSound(weaponType = 'ar') {
        if (!this.ctx) return;
        const now = this.ctx.currentTime;
        const osc = this.ctx.createOscillator();
        const gain = this.ctx.createGain();

        if (weaponType === 'sniper') {
          osc.type = 'sawtooth';
          osc.frequency.setValueAtTime(800, now);
          osc.frequency.exponentialRampToValueAtTime(40, now + 0.35);
          gain.gain.setValueAtTime(0.7, now);
          gain.gain.exponentialRampToValueAtTime(0.01, now + 0.35);
          osc.connect(gain);
          gain.connect(this.ctx.destination);
          osc.start(now);
          osc.stop(now + 0.35);
        } else if (weaponType === 'shotgun') {
          osc.type = 'square';
          osc.frequency.setValueAtTime(320, now);
          osc.frequency.exponentialRampToValueAtTime(30, now + 0.25);
          gain.gain.setValueAtTime(0.65, now);
          gain.gain.exponentialRampToValueAtTime(0.01, now + 0.25);
          osc.connect(gain);
          gain.connect(this.ctx.destination);
          osc.start(now);
          osc.stop(now + 0.25);
        } else {
          osc.type = 'sawtooth';
          osc.frequency.setValueAtTime(440, now);
          osc.frequency.exponentialRampToValueAtTime(50, now + 0.12);
          gain.gain.setValueAtTime(0.4, now);
          gain.gain.exponentialRampToValueAtTime(0.01, now + 0.12);
          osc.connect(gain);
          gain.connect(this.ctx.destination);
          osc.start(now);
          osc.stop(now + 0.12);
        }
      }
      playHitSound(isHead = false) {
        if (!this.ctx) return;
        const now = this.ctx.currentTime;
        const osc = this.ctx.createOscillator();
        const gain = this.ctx.createGain();
        osc.type = 'sine';
        osc.frequency.setValueAtTime(isHead ? 2400 : 1600, now);
        gain.gain.setValueAtTime(0.4, now);
        gain.gain.exponentialRampToValueAtTime(0.01, now + 0.08);
        osc.connect(gain);
        gain.connect(this.ctx.destination);
        osc.start(now);
        osc.stop(now + 0.08);
      }
      playRapidRefillSound() {
        if (!this.ctx) return;
        const now = this.ctx.currentTime;
        const osc = this.ctx.createOscillator();
        const gain = this.ctx.createGain();
        osc.type = 'triangle';
        osc.frequency.setValueAtTime(440, now);
        osc.frequency.exponentialRampToValueAtTime(1200, now + 0.15);
        gain.gain.setValueAtTime(0.35, now);
        gain.gain.exponentialRampToValueAtTime(0.01, now + 0.15);
        osc.connect(gain);
        gain.connect(this.ctx.destination);
        osc.start(now);
        osc.stop(now + 0.15);
      }
      playSupplySound() {
        if (!this.ctx) return;
        const now = this.ctx.currentTime;
        [523.25, 659.25, 783.99, 1046.5].forEach((freq, idx) => {
          const osc = this.ctx.createOscillator();
          const gain = this.ctx.createGain();
          osc.type = 'triangle';
          osc.frequency.setValueAtTime(freq, now + idx * 0.08);
          gain.gain.setValueAtTime(0.4, now + idx * 0.08);
          gain.gain.exponentialRampToValueAtTime(0.01, now + idx * 0.08 + 0.25);
          osc.connect(gain);
          gain.connect(this.ctx.destination);
          osc.start(now + idx * 0.08);
          osc.stop(now + idx * 0.08 + 0.25);
        });
      }
      playBooyah() {
        if (!this.ctx) return;
        const chords = [523.25, 659.25, 783.99, 1046.50];
        chords.forEach((freq, idx) => {
          const osc = this.ctx.createOscillator();
          const gain = this.ctx.createGain();
          const now = this.ctx.currentTime + idx * 0.12;
          osc.type = 'triangle';
          osc.frequency.setValueAtTime(freq, now);
          gain.gain.setValueAtTime(0.5, now);
          gain.gain.exponentialRampToValueAtTime(0.01, now + 0.8);
          osc.connect(gain);
          gain.connect(this.ctx.destination);
          osc.start(now);
          osc.stop(now + 0.8);
        });
      }
    }
    const audio = new FFAudio();

    /* =========================================================================
       2. THREE.JS SCENE SETUP & CRISP LIGHTING
       ========================================================================= */
    const container = document.getElementById('game-container');
    const scene = new THREE.Scene();
    scene.background = new THREE.Color(0x87ceeb);
    scene.fog = new THREE.FogExp2(0xa0c4e8, 0.005);

    const camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 0.1, 800);
    const renderer = new THREE.WebGLRenderer({ antialias: true });
    renderer.setSize(window.innerWidth, window.innerHeight);
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
    renderer.shadowMap.enabled = true;
    renderer.shadowMap.type = THREE.PCFSoftShadowMap;
    container.appendChild(renderer.domElement);

    const hemi = new THREE.HemisphereLight(0xffffff, 0x445566, 0.7);
    scene.add(hemi);

    const sun = new THREE.DirectionalLight(0xfffaed, 1.35);
    sun.position.set(70, 120, 50);
    sun.castShadow = true;
    sun.shadow.mapSize.width = 2048;
    sun.shadow.mapSize.height = 2048;
    const d = 160;
    sun.shadow.camera.left = -d; sun.shadow.camera.right = d;
    sun.shadow.camera.top = d; sun.shadow.camera.bottom = -d;
    scene.add(sun);

    /* =========================================================================
       3. CLIMATE & WEATHER PROGRESSION ACROSS ZONES
       ========================================================================= */
    const WEATHER_STATES = [
      { name: "MORNING SUN", sky: 0x87ceeb, fog: 0xa0c4e8, fogDensity: 0.005, sunColor: 0xfffaed, sunIntensity: 1.35 },
      { name: "CLEAR NOON", sky: 0x38bdf8, fog: 0x7dd3fc, fogDensity: 0.004, sunColor: 0xffffff, sunIntensity: 1.4 },
      { name: "GOLDEN SUNSET", sky: 0xf97316, fog: 0xfb923c, fogDensity: 0.0055, sunColor: 0xfed7aa, sunIntensity: 1.3 },
      { name: "DUSK MIST", sky: 0x64748b, fog: 0x475569, fogDensity: 0.008, sunColor: 0xfcd34d, sunIntensity: 0.95 },
      { name: "TWILIGHT NIGHT", sky: 0x0f172a, fog: 0x1e293b, fogDensity: 0.006, sunColor: 0x93c5fd, sunIntensity: 0.65 },
      { name: "STADIUM SHOWDOWN", sky: 0x020617, fog: 0x0f172a, fogDensity: 0.005, sunColor: 0xffffff, sunIntensity: 0.8 }
    ];

    function applyWeather(idx) {
      const w = WEATHER_STATES[idx % WEATHER_STATES.length];
      scene.background.setHex(w.sky);
      scene.fog.color.setHex(w.fog);
      scene.fog.density = w.fogDensity;
      sun.color.setHex(w.sunColor);
      sun.intensity = w.sunIntensity;
      document.getElementById('weather-name').innerText = w.name;
    }

    /* =========================================================================
       4. RESTORED DETAILED VFSTR VADLAMUDI ARCHITECTURE & SURROUNDINGS
       ========================================================================= */
    const ZONES = [
      { name: "VFSTR A-BLOCK (ADMIN)", code: "A-BLOCK", pos: new THREE.Vector3(0, 0, -35), size: { w: 36, d: 24 } },
      { name: "NTR CENTRAL LIBRARY", code: "NTR LIB", pos: new THREE.Vector3(-65, 0, 0), size: { w: 28, d: 22 } },
      { name: "H-BLOCK (HOMI BHABHA)", code: "H-BLOCK", pos: new THREE.Vector3(65, 0, 0), size: { w: 32, d: 22 } },
      { name: "N-BLOCK (NTR CSE/IT)", code: "N-BLOCK", pos: new THREE.Vector3(-65, 0, 60), size: { w: 30, d: 24 } },
      { name: "U-BLOCK (PHARMACY)", code: "U-BLOCK", pos: new THREE.Vector3(65, 0, 60), size: { w: 30, d: 24 } },
      { name: "UNIVERSITY PLAYGROUND", code: "STADIUM",  pos: new THREE.Vector3(0, 0, 110), size: { w: 80, d: 55 } }
    ];

    const mats = {
      grass: new THREE.MeshStandardMaterial({ color: 0x3d7031, roughness: 0.85 }),
      road: new THREE.MeshStandardMaterial({ color: 0x1f2937, roughness: 0.6 }),
      roadLine: new THREE.MeshBasicMaterial({ color: 0xf59e0b }),
      aBlock: new THREE.MeshStandardMaterial({ color: 0xfde047, roughness: 0.5 }),
      brick: new THREE.MeshStandardMaterial({ color: 0xb91c1c, roughness: 0.7 }),
      concrete: new THREE.MeshStandardMaterial({ color: 0xe2e8f0, roughness: 0.6 }),
      metal: new THREE.MeshStandardMaterial({ color: 0x334155, metalness: 0.7, roughness: 0.3 }),
      glass: new THREE.MeshStandardMaterial({ color: 0x38bdf8, transparent: true, opacity: 0.75, roughness: 0.1 }),
      roof: new THREE.MeshStandardMaterial({ color: 0x0f172a, roughness: 0.5 }),
      turf: new THREE.MeshStandardMaterial({ color: 0x22c55e, roughness: 0.8 }),
      track: new THREE.MeshStandardMaterial({ color: 0x991b1b, roughness: 0.8 }),
      palmTrunk: new THREE.MeshStandardMaterial({ color: 0x78350f, roughness: 0.9 }),
      palmLeaves: new THREE.MeshStandardMaterial({ color: 0x15803d, roughness: 0.6 }),
      busYellow: new THREE.MeshStandardMaterial({ color: 0xf59e0b, roughness: 0.3 }),
      carPaintBlue: new THREE.MeshStandardMaterial({ color: 0x0284c7, metalness: 0.8, roughness: 0.2 }),
      carPaintRed: new THREE.MeshStandardMaterial({ color: 0xef4444, metalness: 0.8, roughness: 0.2 }),
      jeepMilitary: new THREE.MeshStandardMaterial({ color: 0x3f6212, roughness: 0.6 }),
      wheelRubber: new THREE.MeshStandardMaterial({ color: 0x111827, roughness: 0.9 })
    };

    // Ground Plane
    const gMesh = new THREE.Mesh(new THREE.PlaneGeometry(350, 350), mats.grass);
    gMesh.rotation.x = -Math.PI / 2; gMesh.receiveShadow = true; scene.add(gMesh);

    // Campus Roads
    function makeRoad(x, z, w, d) {
      const r = new THREE.Mesh(new THREE.PlaneGeometry(w, d), mats.road);
      r.rotation.x = -Math.PI / 2; r.position.set(x, 0.02, z); r.receiveShadow = true; scene.add(r);
      if (d > w) {
        const line = new THREE.Mesh(new THREE.PlaneGeometry(0.35, d), mats.roadLine);
        line.rotation.x = -Math.PI / 2; line.position.set(x, 0.03, z); scene.add(line);
      }
    }
    makeRoad(0, 40, 12, 260);
    makeRoad(0, 0, 240, 12);
    makeRoad(0, 60, 240, 12);
    makeRoad(0, -75, 240, 14);

    // Signboards & Rooftop Labels
    function makeSign(txt, color, pos) {
      const cv = document.createElement('canvas'); cv.width = 512; cv.height = 128;
      const ctx = cv.getContext('2d');
      ctx.fillStyle = color; ctx.fillRect(0,0,512,128);
      ctx.strokeStyle = '#fff'; ctx.lineWidth = 8; ctx.strokeRect(5,5,502,118);
      ctx.fillStyle = '#fff'; ctx.font = 'bold 36px Orbitron, sans-serif'; ctx.textAlign = 'center'; ctx.textBaseline = 'middle';
      ctx.fillText(txt, 256, 64);
      const tex = new THREE.CanvasTexture(cv);
      const m = new THREE.Mesh(new THREE.BoxGeometry(txt.length * 0.65 + 2, 1.6, 0.3), [
        mats.metal, mats.metal, mats.metal, mats.metal,
        new THREE.MeshBasicMaterial({ map: tex }), mats.metal
      ]);
      m.position.copy(pos); m.castShadow = true; scene.add(m);
    }

    function buildDetailedBlock(pos, w, h, d, mainMat, name, signColor) {
      const grp = new THREE.Group(); grp.position.copy(pos);
      const base = new THREE.Mesh(new THREE.BoxGeometry(w + 1, 1, d + 1), mats.concrete);
      base.position.y = 0.5; grp.add(base);
      const body = new THREE.Mesh(new THREE.BoxGeometry(w, h, d), mainMat);
      body.position.y = h / 2 + 0.5; body.castShadow = true; body.receiveShadow = true; grp.add(body);
      const roof = new THREE.Mesh(new THREE.BoxGeometry(w + 1.5, 1, d + 1.5), mats.roof);
      roof.position.y = h + 1; roof.castShadow = true; grp.add(roof);

      for (let f = 0; f < 3; f++) {
        const fy = 3 + f * 4;
        const winFront = new THREE.Mesh(new THREE.BoxGeometry(w - 4, 1.8, 0.4), mats.glass);
        winFront.position.set(0, fy, -d/2 - 0.1); grp.add(winFront);
      }
      scene.add(grp);
      makeSign(name, signColor, new THREE.Vector3(pos.x, h - 1.5, pos.z - d/2 - 0.4));
    }

    buildDetailedBlock(new THREE.Vector3(0, 0, -35), 36, 14, 24, mats.aBlock, "VFSTR A-BLOCK (ADMIN)", "#b45309");
    buildDetailedBlock(new THREE.Vector3(-65, 0, 0), 28, 12, 22, mats.concrete, "NTR CENTRAL LIBRARY", "#0284c7");
    buildDetailedBlock(new THREE.Vector3(65, 0, 0), 32, 15, 22, mats.brick, "H-BLOCK (HOMI BHABHA)", "#b91c1c");
    buildDetailedBlock(new THREE.Vector3(-65, 0, 60), 30, 14, 24, mats.concrete, "N-BLOCK (NTR CSE/IT)", "#0d9488");
    buildDetailedBlock(new THREE.Vector3(65, 0, 60), 30, 14, 24, mats.aBlock, "U-BLOCK (PHARMACY)", "#059669");

    // Playground & Stadium
    const gGrp = new THREE.Group(); gGrp.position.set(0, 0, 110);
    const turf = new THREE.Mesh(new THREE.BoxGeometry(78, 0.1, 52), mats.turf); turf.position.y = 0.05; gGrp.add(turf);
    const track = new THREE.Mesh(new THREE.BoxGeometry(86, 0.04, 60), mats.track); track.position.y = 0.03; gGrp.add(track);
    for (let s = 0; s < 5; s++) {
      const b = new THREE.Mesh(new THREE.BoxGeometry(68, 0.5, 1.2), mats.concrete);
      b.position.set(0, 0.5 + s * 0.5, 32 + s * 1.2); gGrp.add(b);
    }
    [[-38, -26], [38, -26], [-38, 26], [38, 26]].forEach(([lx, lz]) => {
      const pole = new THREE.Mesh(new THREE.CylinderGeometry(0.4, 0.5, 18), mats.metal);
      pole.position.set(lx, 9, lz); gGrp.add(pole);
      const head = new THREE.Mesh(new THREE.BoxGeometry(3.5, 1.6, 1.6), mats.metal);
      head.position.set(lx, 18, lz); gGrp.add(head);
    });
    scene.add(gGrp);
    makeSign("VIGNAN UNIVERSITY PLAYGROUND", "#d97706", new THREE.Vector3(0, 3.5, 79));

    // Main Entrance Gate
    const gateGrp = new THREE.Group(); gateGrp.position.set(0, 0, -75);
    const gatePillarL = new THREE.Mesh(new THREE.BoxGeometry(4, 12, 4), mats.concrete); gatePillarL.position.set(-10, 6, 0); gateGrp.add(gatePillarL);
    const gatePillarR = new THREE.Mesh(new THREE.BoxGeometry(4, 12, 4), mats.concrete); gatePillarR.position.set(10, 6, 0); gateGrp.add(gatePillarR);
    const gateArch = new THREE.Mesh(new THREE.BoxGeometry(24, 3, 4.5), mats.aBlock); gateArch.position.set(0, 12.5, 0); gateGrp.add(gateArch);
    scene.add(gateGrp);
    makeSign("VIGNAN UNIVERSITY (VFSTR) MAIN GATE", "#0284c7", new THREE.Vector3(0, 13.2, -75));

    // Palm Trees
    function makeRealisticPalm(x, z) {
      const t = new THREE.Group(); t.position.set(x, 0, z);
      const tr = new THREE.Mesh(new THREE.CylinderGeometry(0.2, 0.35, 5, 8), mats.palmTrunk);
      tr.position.y = 2.5; tr.castShadow = true; t.add(tr);
      for (let i = 0; i < 7; i++) {
        const leaf = new THREE.Mesh(new THREE.ConeGeometry(1.8, 0.4, 4), mats.palmLeaves);
        leaf.rotation.z = Math.PI / 3; leaf.rotation.y = (i * Math.PI * 2) / 7;
        leaf.position.y = 4.8; leaf.castShadow = true; t.add(leaf);
      }
      scene.add(t);
    }
    for (let z = -65; z <= 100; z += 24) { makeRealisticPalm(-8, z); makeRealisticPalm(8, z); }

    // Campus Vehicles
    function makeCampusBus(x, z, rotY = 0) {
      const bus = new THREE.Group();
      bus.position.set(x, 0, z);
      bus.rotation.y = rotY;

      const body = new THREE.Mesh(new THREE.BoxGeometry(3.2, 3.0, 9.5), mats.busYellow);
      body.position.y = 1.8; body.castShadow = true; bus.add(body);

      const stripe = new THREE.Mesh(new THREE.BoxGeometry(3.25, 0.5, 9.55), mats.carPaintBlue);
      stripe.position.y = 1.2; bus.add(stripe);

      const win = new THREE.Mesh(new THREE.BoxGeometry(3.3, 1.1, 7.5), mats.glass);
      win.position.set(0, 2.3, -0.2); bus.add(win);

      const frontWin = new THREE.Mesh(new THREE.BoxGeometry(2.8, 1.4, 0.2), mats.glass);
      frontWin.position.set(0, 2.2, 4.76); bus.add(frontWin);

      const wheelGeo = new THREE.CylinderGeometry(0.55, 0.55, 0.4, 12);
      [[-1.6, 2.5], [1.6, 2.5], [-1.6, -2.5], [1.6, -2.5]].forEach(([wx, wz]) => {
        const wh = new THREE.Mesh(wheelGeo, mats.wheelRubber);
        wh.rotation.z = Math.PI / 2;
        wh.position.set(wx, 0.55, wz);
        wh.castShadow = true;
        bus.add(wh);
      });
      scene.add(bus);
    }

    function makePatrolJeep(x, z, rotY = 0) {
      const jeep = new THREE.Group();
      jeep.position.set(x, 0, z);
      jeep.rotation.y = rotY;

      const body = new THREE.Mesh(new THREE.BoxGeometry(2.2, 1.4, 4.4), mats.jeepMilitary);
      body.position.y = 1.1; body.castShadow = true; jeep.add(body);

      const cabin = new THREE.Mesh(new THREE.BoxGeometry(2.0, 1.1, 2.2), mats.metal);
      cabin.position.set(0, 2.1, -0.4); jeep.add(cabin);

      const windshield = new THREE.Mesh(new THREE.BoxGeometry(1.9, 0.8, 0.1), mats.glass);
      windshield.position.set(0, 2.1, 0.72); jeep.add(windshield);

      const wheelGeo = new THREE.CylinderGeometry(0.45, 0.45, 0.35, 12);
      [[-1.15, 1.3], [1.15, 1.3], [-1.15, -1.3], [1.15, -1.3]].forEach(([wx, wz]) => {
        const wh = new THREE.Mesh(wheelGeo, mats.wheelRubber);
        wh.rotation.z = Math.PI / 2;
        wh.position.set(wx, 0.45, wz);
        wh.castShadow = true;
        jeep.add(wh);
      });
      scene.add(jeep);
    }

    function makeCar(x, z, rotY = 0, colorMat = mats.carPaintBlue) {
      const car = new THREE.Group();
      car.position.set(x, 0, z);
      car.rotation.y = rotY;

      const body = new THREE.Mesh(new THREE.BoxGeometry(2.0, 0.9, 4.2), colorMat);
      body.position.y = 0.75; body.castShadow = true; car.add(body);

      const cabin = new THREE.Mesh(new THREE.BoxGeometry(1.7, 0.75, 2.0), mats.glass);
      cabin.position.set(0, 1.4, -0.2); car.add(cabin);

      const wheelGeo = new THREE.CylinderGeometry(0.35, 0.35, 0.25, 12);
      [[-1.05, 1.2], [1.05, 1.2], [-1.05, -1.2], [1.05, -1.2]].forEach(([wx, wz]) => {
        const wh = new THREE.Mesh(wheelGeo, mats.wheelRubber);
        wh.rotation.z = Math.PI / 2;
        wh.position.set(wx, 0.35, wz);
        car.add(wh);
      });
      scene.add(car);
    }

    makeCampusBus(-18, -60, Math.PI / 6);
    makeCampusBus(18, -60, -Math.PI / 6);
    makePatrolJeep(-12, -70, 0);
    makeCar(25, -20, Math.PI / 2, mats.carPaintBlue);
    makeCar(25, -12, Math.PI / 2, mats.carPaintRed);
    makeCar(-25, 15, -Math.PI / 2, mats.carPaintBlue);

    /* =========================================================================
       5. RESTORED DETAILED TACTICAL SURVIVOR RIG & WEAPONS
       ========================================================================= */
    const player = {
      pos: new THREE.Vector3(0, 0, -50),
      hp: 250,
      maxHp: 250,
      kills: 0,
      score: 0,
      isGrounded: true,
      vy: 0,
      isADS: false,
      isFiring: false,
      isReloading: false
    };

    let yaw = 0;
    let pitch = 0;
    let isThirdPerson = true;

    const playerGrp = new THREE.Group();
    scene.add(playerGrp);
    const charBody = new THREE.Group();
    playerGrp.add(charBody);

    const vestMat = new THREE.MeshStandardMaterial({ color: 0x0284c7, roughness: 0.4 });
    const armorPlateMat = new THREE.MeshStandardMaterial({ color: 0x0f172a, metalness: 0.8 });
    const skinMat = new THREE.MeshStandardMaterial({ color: 0xfbcfe8, roughness: 0.8 });
    const pantsMat = new THREE.MeshStandardMaterial({ color: 0x1e293b, roughness: 0.6 });
    const bootMat = new THREE.MeshStandardMaterial({ color: 0x020617, roughness: 0.5 });
    const visorMat = new THREE.MeshStandardMaterial({ color: 0x00e5ff, emissive: 0x00e5ff, emissiveIntensity: 0.8 });

    // Torso & Armor
    const survTorso = new THREE.Mesh(new THREE.BoxGeometry(0.5, 0.65, 0.3), vestMat);
    survTorso.position.y = 1.15; survTorso.castShadow = true; charBody.add(survTorso);
    const chestPlate = new THREE.Mesh(new THREE.BoxGeometry(0.42, 0.4, 0.08), armorPlateMat);
    chestPlate.position.set(0, 1.2, 0.16); charBody.add(chestPlate);

    // Head & Visor
    const survHead = new THREE.Mesh(new THREE.BoxGeometry(0.3, 0.3, 0.3), skinMat);
    survHead.position.y = 1.62; charBody.add(survHead);
    const helmet = new THREE.Mesh(new THREE.BoxGeometry(0.34, 0.2, 0.34), armorPlateMat);
    helmet.position.set(0, 1.7, 0); charBody.add(helmet);
    const visor = new THREE.Mesh(new THREE.BoxGeometry(0.24, 0.08, 0.08), visorMat);
    visor.position.set(0, 1.63, 0.16); charBody.add(visor);

    // Arms
    const survArmL = new THREE.Group(); survArmL.position.set(-0.35, 1.35, 0); charBody.add(survArmL);
    const armMeshL = new THREE.Mesh(new THREE.BoxGeometry(0.16, 0.6, 0.16), vestMat); armMeshL.position.y = -0.25; survArmL.add(armMeshL);

    const survArmR = new THREE.Group(); survArmR.position.set(0.35, 1.35, 0); charBody.add(survArmR);
    const armMeshR = new THREE.Mesh(new THREE.BoxGeometry(0.16, 0.6, 0.16), vestMat); armMeshR.position.y = -0.25; survArmR.add(armMeshR);

    // Legs
    const survLegL = new THREE.Group(); survLegL.position.set(-0.16, 0.85, 0); charBody.add(survLegL);
    const legMeshL = new THREE.Mesh(new THREE.BoxGeometry(0.18, 0.8, 0.18), pantsMat); legMeshL.position.y = -0.4; survLegL.add(legMeshL);
    const bootL = new THREE.Mesh(new THREE.BoxGeometry(0.2, 0.18, 0.26), bootMat); bootL.position.set(0, -0.75, 0.04); survLegL.add(bootL);

    const survLegR = new THREE.Group(); survLegR.position.set(0.16, 0.85, 0); charBody.add(survLegR);
    const legMeshR = new THREE.Mesh(new THREE.BoxGeometry(0.18, 0.8, 0.18), pantsMat); legMeshR.position.y = -0.4; survLegR.add(legMeshR);
    const bootR = new THREE.Mesh(new THREE.BoxGeometry(0.2, 0.18, 0.26), bootMat); bootR.position.set(0, -0.75, 0.04); survLegR.add(bootR);

    // Gun Rig
    const gunMesh = new THREE.Group();
    const gunSteelMat = new THREE.MeshStandardMaterial({ color: 0x111827, metalness: 0.9, roughness: 0.2 });
    const gunGoldMat = new THREE.MeshStandardMaterial({ color: 0xf59e0b, metalness: 0.8, roughness: 0.3 });

    const gunReceiver = new THREE.Mesh(new THREE.BoxGeometry(0.06, 0.1, 0.35), gunSteelMat); gunMesh.add(gunReceiver);
    const gunBarrel = new THREE.Mesh(new THREE.CylinderGeometry(0.014, 0.014, 0.32), gunSteelMat); gunBarrel.rotation.x = Math.PI / 2; gunBarrel.position.set(0, 0.02, 0.28); gunMesh.add(gunBarrel);
    const gunHandguard = new THREE.Mesh(new THREE.BoxGeometry(0.05, 0.06, 0.2), gunGoldMat); gunHandguard.position.set(0, 0.02, 0.2); gunMesh.add(gunHandguard);
    const gunMag = new THREE.Mesh(new THREE.BoxGeometry(0.04, 0.16, 0.08), gunSteelMat); gunMag.position.set(0, -0.1, 0.06); gunMag.rotation.x = 0.25; gunMesh.add(gunMag);
    const gunSight = new THREE.Mesh(new THREE.BoxGeometry(0.045, 0.06, 0.07), gunSteelMat); gunSight.position.set(0, 0.08, 0.02); gunMesh.add(gunSight);
    const holoDot = new THREE.Mesh(new THREE.SphereGeometry(0.006, 8, 8), new THREE.MeshBasicMaterial({ color: 0x00e5ff })); holoDot.position.set(0, 0.08, 0.05); gunMesh.add(holoDot);

    const muzzleLight = new THREE.PointLight(0xf59e0b, 0, 5); muzzleLight.position.set(0, 0.02, 0.46); gunMesh.add(muzzleLight);
    charBody.add(gunMesh); gunMesh.position.set(0.3, 1.15, 0.32);

    /* =========================================================================
       6. WEAPONS WITH ULTRA-FAST LIGHTNING REFILL & EXPANDED RESERVES
       ========================================================================= */
    const WEAPONS = [
      {
        name: "V-7 TACTICAL RIFLE",
        typeText: "FULL AUTO • ULTRA-FAST REFILL",
        damage: 42,
        fireRate: 0.11,
        clipSize: 45,
        currentClip: 45,
        reserve: 360,
        maxReserve: 360,
        reloadTime: 0.2, // 0.2s ultra-fast lightning reload
        zoomFov: 50,
        recoil: 0.015,
        typeId: 'ar'
      },
      {
        name: "M1887 COMBAT SHOTGUN",
        typeText: "BUCKSHOT • ULTRA-FAST REFILL",
        damage: 26,
        pellets: 8,
        spread: 0.045,
        fireRate: 0.55,
        clipSize: 8,
        currentClip: 8,
        reserve: 96,
        maxReserve: 96,
        reloadTime: 0.22,
        zoomFov: 58,
        recoil: 0.035,
        typeId: 'shotgun'
      },
      {
        name: "AWM TACTICAL SNIPER",
        typeText: "BOLT-ACTION • ULTRA-FAST REFILL",
        damage: 300,
        fireRate: 0.9,
        clipSize: 10,
        currentClip: 10,
        reserve: 60,
        maxReserve: 60,
        reloadTime: 0.25,
        zoomFov: 24,
        recoil: 0.07,
        typeId: 'sniper'
      }
    ];

    let currentWeaponIdx = 0;
    let lastShotTime = 0;
    let lastFireActionTime = 0;

    function switchWeapon(idx) {
      if (idx < 0 || idx >= WEAPONS.length) return;
      currentWeaponIdx = idx;
      document.querySelectorAll('.slot-pill').forEach((el, i) => {
        if (i === idx) el.classList.add('active');
        else el.classList.remove('active');
      });
      updateWeaponHUD();
    }

    function triggerUltraFastRefill() {
      const w = WEAPONS[currentWeaponIdx];
      if (player.isReloading || w.currentClip >= w.clipSize) return;

      player.isReloading = true;
      audio.playRapidRefillSound();

      const flashBar = document.getElementById('reload-flash');
      const clipEl = document.getElementById('ammo-clip');
      flashBar.style.width = '100%';
      clipEl.classList.add('refilling');

      setTimeout(() => {
        const needed = w.clipSize - w.currentClip;
        const toLoad = Math.min(needed, w.reserve);
        w.currentClip += toLoad;
        w.reserve -= toLoad;

        // If reserve was low, continuously top up reserve
        if (w.reserve < w.clipSize) {
          w.reserve = w.maxReserve;
        }

        player.isReloading = false;
        flashBar.style.width = '0%';
        clipEl.classList.remove('refilling');
        updateWeaponHUD();
      }, w.reloadTime * 1000);
    }

    function updateWeaponHUD() {
      const w = WEAPONS[currentWeaponIdx];
      document.getElementById('gun-name').innerText = w.name;
      document.getElementById('gun-type').innerText = w.typeText;
      document.getElementById('ammo-clip').innerText = w.currentClip;
      document.getElementById('ammo-reserve').innerText = w.reserve;
    }

    /* =========================================================================
       7. RESTORED CYBORG OUTBREAK ZOMBIES (DETAILED CYBORG RIG & HP BARS)
       ========================================================================= */
    const zombies = [];

    function spawnRealisticZombie(pos, type = 'vanguard') {
      const zGrp = new THREE.Group();
      zGrp.position.copy(pos);

      let hp = 100;
      let speed = 3.8;
      let scale = 1.0;
      let skinColor = 0x2e5c38;
      let glowColor = 0xff0033;

      if (type === 'scout') {
        hp = 70;
        speed = 5.2;
        scale = 0.85;
        skinColor = 0x1e3a8a;
        glowColor = 0x38bdf8;
      } else if (type === 'boss') {
        hp = 260;
        speed = 2.8;
        scale = 1.4;
        skinColor = 0x581c87;
        glowColor = 0xa855f7;
      }

      const zSkin = new THREE.MeshStandardMaterial({ color: skinColor, roughness: 0.8 });
      const zArmor = new THREE.MeshStandardMaterial({ color: 0x1e293b, metalness: 0.7 });
      const zGlow = new THREE.MeshBasicMaterial({ color: glowColor });

      const torso = new THREE.Mesh(new THREE.BoxGeometry(0.55 * scale, 0.65 * scale, 0.35 * scale), zArmor);
      torso.position.y = 1.15 * scale; torso.castShadow = true; zGrp.add(torso);

      const chestGlow = new THREE.Mesh(new THREE.BoxGeometry(0.18 * scale, 0.18 * scale, 0.04 * scale), zGlow);
      chestGlow.position.set(0, 1.25 * scale, 0.18 * scale); zGrp.add(chestGlow);

      const head = new THREE.Mesh(new THREE.BoxGeometry(0.34 * scale, 0.34 * scale, 0.34 * scale), zSkin);
      head.position.y = 1.65 * scale; head.castShadow = true; zGrp.add(head);

      const eyeL = new THREE.Mesh(new THREE.SphereGeometry(0.045 * scale, 8, 8), zGlow);
      eyeL.position.set(-0.09 * scale, 1.68 * scale, 0.18 * scale); zGrp.add(eyeL);

      const eyeR = new THREE.Mesh(new THREE.SphereGeometry(0.045 * scale, 8, 8), zGlow);
      eyeR.position.set(0.09 * scale, 1.68 * scale, 0.18 * scale); zGrp.add(eyeR);

      const armL = new THREE.Group(); armL.position.set(-0.38 * scale, 1.35 * scale, 0); zGrp.add(armL);
      const armMeshL = new THREE.Mesh(new THREE.BoxGeometry(0.18 * scale, 0.6 * scale, 0.18 * scale), zSkin);
      armMeshL.position.y = -0.25 * scale; armL.add(armMeshL);

      const armR = new THREE.Group(); armR.position.set(0.38 * scale, 1.35 * scale, 0); zGrp.add(armR);
      const armMeshR = new THREE.Mesh(new THREE.BoxGeometry(0.18 * scale, 0.6 * scale, 0.18 * scale), zSkin);
      armMeshR.position.y = -0.25 * scale; armR.add(armMeshR);

      const legL = new THREE.Group(); legL.position.set(-0.18 * scale, 0.85 * scale, 0); zGrp.add(legL);
      const legMeshL = new THREE.Mesh(new THREE.BoxGeometry(0.2 * scale, 0.8 * scale, 0.2 * scale), zArmor);
      legMeshL.position.y = -0.4 * scale; legL.add(legMeshL);

      const legR = new THREE.Group(); legR.position.set(0.18 * scale, 0.85 * scale, 0); zGrp.add(legR);
      const legMeshR = new THREE.Mesh(new THREE.BoxGeometry(0.2 * scale, 0.8 * scale, 0.2 * scale), zArmor);
      legMeshR.position.y = -0.4 * scale; legR.add(legMeshR);

      // Overhead HP Bar
      const hpBarBg = new THREE.Mesh(new THREE.PlaneGeometry(0.9 * scale, 0.14 * scale), new THREE.MeshBasicMaterial({ color: 0x0f172a }));
      hpBarBg.position.set(0, 2.2 * scale, 0); zGrp.add(hpBarBg);

      const hpBarFill = new THREE.Mesh(new THREE.PlaneGeometry(0.86 * scale, 0.1 * scale), new THREE.MeshBasicMaterial({ color: 0xef4444 }));
      hpBarFill.position.set(0, 2.2 * scale, 0.01 * scale); zGrp.add(hpBarFill);

      scene.add(zGrp);

      const zObj = {
        mesh: zGrp,
        head: head,
        armL: armL,
        armR: armR,
        legL: legL,
        legR: legR,
        hpBarFill: hpBarFill,
        type: type,
        hp: hp,
        maxHp: hp,
        speed: speed,
        scale: scale,
        isDead: false,
        nextAtk: 0,
        anim: Math.random() * 10
      };
      zombies.push(zObj);
      return zObj;
    }

    /* =========================================================================
       8. BULLETS, TRACERS & SHELLS
       ========================================================================= */
    const bullets = [];

    function spawnBullet(origin, direction, dmg, isPellet = false) {
      const geo = new THREE.SphereGeometry(isPellet ? 0.08 : 0.12, 8, 8);
      const mat = new THREE.MeshBasicMaterial({ color: 0xfbbf24 });
      const mesh = new THREE.Mesh(geo, mat);
      mesh.position.copy(origin);
      scene.add(mesh);

      bullets.push({
        mesh: mesh,
        dir: direction.clone().normalize(),
        speed: 150,
        life: 1.4,
        damage: dmg
      });
    }

    /* =========================================================================
       9. CONTROLS, POINTER LOCK & 360° SMOOTH AIM
       ========================================================================= */
    const keys = {};
    let isPlaying = false;
    let isLocked = false;
    let isMapOpen = false;

    window.addEventListener('keydown', (e) => {
      keys[e.code] = true;
      if (!isPlaying) return;

      if (e.code === 'KeyR') triggerUltraFastRefill();
      if (e.code === 'KeyM') toggleTacticalMap();
      if (e.code === 'KeyV') {
        isThirdPerson = !isThirdPerson;
        document.getElementById('cam-mode').innerText = isThirdPerson ? 'TPS' : 'FPS';
      }
      if (e.code === 'Digit1') switchWeapon(0);
      if (e.code === 'Digit2') switchWeapon(1);
      if (e.code === 'Digit3') switchWeapon(2);
      if (e.code === 'Space') {
        if (player.isGrounded) {
          player.vy = 8.8;
          player.isGrounded = false;
        }
      }
    });

    window.addEventListener('keyup', (e) => {
      keys[e.code] = false;
    });

    function requestPointerLockSafe() {
      const elem = renderer.domElement;
      if (elem && elem.requestPointerLock) {
        elem.requestPointerLock();
      }
    }

    document.addEventListener('pointerlockchange', () => {
      isLocked = !!document.pointerLockElement;
      const resumeOverlay = document.getElementById('resume-overlay');
      if (isPlaying && !isLocked && !isMapOpen) {
        resumeOverlay.style.display = 'flex';
      } else {
        resumeOverlay.style.display = 'none';
      }
    });

    function requestLockAndResume() {
      if (isMapOpen) toggleTacticalMap(false);
      requestPointerLockSafe();
    }

    window.addEventListener('mousemove', (e) => {
      if (!isLocked || !isPlaying || isMapOpen) return;
      const sens = player.isADS ? 0.0012 : 0.0022;
      yaw -= e.movementX * sens;
      pitch -= e.movementY * sens;
      pitch = Math.max(-Math.PI / 2.3, Math.min(Math.PI / 2.3, pitch));
    });

    window.addEventListener('mousedown', (e) => {
      if (!isPlaying || isMapOpen) return;
      if (!isLocked) {
        requestPointerLockSafe();
        return;
      }
      if (e.button === 0) {
        player.isFiring = true;
        shoot();
      } else if (e.button === 2) {
        player.isADS = true;
      }
    });

    window.addEventListener('mouseup', (e) => {
      if (e.button === 0) player.isFiring = false;
      if (e.button === 2) player.isADS = false;
    });

    window.addEventListener('contextmenu', e => e.preventDefault());

    function shoot() {
      const now = performance.now() / 1000;
      const w = WEAPONS[currentWeaponIdx];
      if (now - lastShotTime < w.fireRate || player.isReloading) return;

      // Auto-reload immediately if clip reaches 0
      if (w.currentClip <= 0) {
        triggerUltraFastRefill();
        return;
      }

      w.currentClip--;
      lastShotTime = now;
      lastFireActionTime = now;
      audio.playShootSound(w.typeId);

      muzzleLight.intensity = 3.5;
      setTimeout(() => muzzleLight.intensity = 0, 45);

      pitch += w.recoil * 0.35;

      const aimDir = new THREE.Vector3();
      camera.getWorldDirection(aimDir);

      const spawnOrigin = player.pos.clone().add(new THREE.Vector3(0, 1.4, 0));

      if (w.typeId === 'shotgun') {
        for (let i = 0; i < w.pellets; i++) {
          const pDir = aimDir.clone().add(new THREE.Vector3(
            (Math.random() - 0.5) * w.spread,
            (Math.random() - 0.5) * w.spread,
            (Math.random() - 0.5) * w.spread
          )).normalize();
          spawnBullet(spawnOrigin, pDir, w.damage, true);
        }
      } else {
        spawnBullet(spawnOrigin, aimDir, w.damage, false);
      }

      // Auto-refill instantly if last bullet was fired
      if (w.currentClip <= 0) {
        triggerUltraFastRefill();
      }

      updateWeaponHUD();
    }

    function showHitmarker() {
      const hm = document.getElementById('hitmarker');
      hm.style.display = 'block';
      setTimeout(() => hm.style.display = 'none', 90);
    }

    function addKillFeed(victimName) {
      const kf = document.getElementById('kill-feed');
      const item = document.createElement('div');
      item.className = 'kill-feed-item';
      item.innerHTML = `<span class="kf-killer">SURVIVOR</span> <span class="kf-weapon">[${WEAPONS[currentWeaponIdx].name}]</span> <span class="kf-victim">${victimName}</span>`;
      kf.appendChild(item);
      setTimeout(() => {
        if (item.parentNode) item.parentNode.removeChild(item);
      }, 3500);
    }

    /* =========================================================================
       10. RADAR MINIMAP & FULL-SCREEN SATELLITE MAP
       ========================================================================= */
    const radarCvs = document.getElementById('radar-canvas');
    const radarCtx = radarCvs.getContext('2d');
    radarCvs.width = 130;
    radarCvs.height = 130;

    function renderRadar() {
      radarCtx.clearRect(0, 0, 130, 130);
      const cx = 65, cy = 65;

      radarCtx.strokeStyle = 'rgba(56, 189, 248, 0.4)';
      radarCtx.lineWidth = 1.5;
      radarCtx.beginPath();
      radarCtx.arc(cx, cy, 58, 0, Math.PI * 2);
      radarCtx.arc(cx, cy, 38, 0, Math.PI * 2);
      radarCtx.stroke();

      radarCtx.strokeStyle = 'rgba(56, 189, 248, 0.2)';
      radarCtx.beginPath();
      radarCtx.moveTo(cx, 0); radarCtx.lineTo(cx, 130);
      radarCtx.moveTo(0, cy); radarCtx.lineTo(130, cy);
      radarCtx.stroke();

      const radarScale = 0.5;
      zombies.forEach(z => {
        if (z.isDead) return;
        const dx = z.mesh.position.x - player.pos.x;
        const dz = z.mesh.position.z - player.pos.z;

        const cosY = Math.cos(yaw);
        const sinY = Math.sin(yaw);
        const rx = (dx * cosY - dz * sinY) * radarScale;
        const ry = (dx * sinY + dz * cosY) * radarScale;

        const dist = Math.sqrt(rx * rx + ry * ry);
        if (dist < 56) {
          radarCtx.fillStyle = z.type === 'boss' ? '#c084fc' : '#ef4444';
          radarCtx.beginPath();
          radarCtx.arc(cx + rx, cy + ry, z.type === 'boss' ? 4.5 : 3, 0, Math.PI * 2);
          radarCtx.fill();
        }
      });

      radarCtx.fillStyle = '#38bdf8';
      radarCtx.beginPath();
      radarCtx.moveTo(cx, cy - 6);
      radarCtx.lineTo(cx - 4, cy + 5);
      radarCtx.lineTo(cx + 4, cy + 5);
      radarCtx.closePath();
      radarCtx.fill();
    }

    const bigMapCvs = document.getElementById('big-map-canvas');
    const bigMapCtx = bigMapCvs.getContext('2d');

    function toggleTacticalMap(forceState) {
      isMapOpen = forceState !== undefined ? forceState : !isMapOpen;
      const modal = document.getElementById('map-modal');
      if (isMapOpen) {
        modal.style.display = 'flex';
        try { document.exitPointerLock(); } catch(e) {}
        renderBigMap();
      } else {
        modal.style.display = 'none';
        requestPointerLockSafe();
      }
    }

    document.getElementById('btn-toggle-map').addEventListener('click', () => toggleTacticalMap(true));
    document.getElementById('map-close-btn').addEventListener('click', () => toggleTacticalMap(false));

    function renderBigMap() {
      bigMapCvs.width = bigMapCvs.clientWidth;
      bigMapCvs.height = bigMapCvs.clientHeight;
      const w = bigMapCvs.width, h = bigMapCvs.height;
      co
