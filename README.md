<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>EGASH BATTLE</title>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif; }
  
  :root {
    --bg: #0a0514;
    --panel: linear-gradient(145deg, #1a1030, #0f081c);
    --border: #3d2963;
    --accent: #f97316;
    --text: #f8fafc;
    --text-muted: #a78bfa;
    --gold: #fbbf24;
    --green: #10b981;
    --red: #ef4444;
  }

  body {
    background-color: var(--bg);
    color: var(--text);
    min-height: 100vh;
    overflow-x: hidden;
    background-image: 
      radial-gradient(circle at 50% 0%, #2e1065 0%, transparent 50%),
      radial-gradient(circle at 10% 90%, #4c1d95 0%, transparent 40%);
    background-attachment: fixed;
  }

  .topbar {
    display: flex; justify-content: space-between; align-items: center;
    padding: 16px 24px; background: rgba(10, 5, 20, 0.85); backdrop-filter: blur(15px);
    border-bottom: 1px solid rgba(255,255,255,0.05); position: sticky; top: 0; z-index: 50;
    flex-wrap: wrap; gap: 10px; box-shadow: 0 4px 30px rgba(0,0,0,0.5);
  }
  .logo { font-size: 24px; font-weight: 900; color: var(--accent); letter-spacing: 2px; text-shadow: 0 0 15px rgba(249, 115, 22, 0.6); display: flex; align-items: center; gap: 8px;}
  .logo span { font-size: 30px; }
  .nav-tabs { display: flex; gap: 8px; background: rgba(0,0,0,0.5); padding: 4px; border-radius: 12px; border: 1px solid var(--border); }
  .tab-btn { background: transparent; border: none; color: var(--text-muted); font-weight: bold; font-size: 13px; cursor: pointer; padding: 8px 16px; border-radius: 8px; transition: 0.3s; }
  .tab-btn:hover { color: white; }
  .tab-btn.active { color: white; background: var(--accent); box-shadow: 0 4px 10px rgba(249, 115, 22, 0.4); }
  
  .balance-container { display: flex; align-items: center; gap: 12px; }
  .balance-pill { background: #000; border: 1px solid var(--gold); padding: 8px 16px; border-radius: 20px; font-weight: bold; font-size: 15px; display: flex; align-items: center; gap: 8px; color: var(--gold); cursor: pointer; transition: 0.3s; box-shadow: 0 0 15px rgba(251, 191, 36, 0.1); }
  .balance-pill:hover { transform: scale(1.05); box-shadow: 0 0 20px rgba(251, 191, 36, 0.3); }
  .add-balance-btn { background: var(--accent); color: white; border: none; width: 36px; height: 36px; border-radius: 50%; font-size: 20px; font-weight: bold; display: flex; align-items: center; justify-content: center; cursor: pointer; transition: 0.3s; box-shadow: 0 4px 10px rgba(249, 115, 22, 0.4); }
  .add-balance-btn:hover { transform: scale(1.1); box-shadow: 0 0 20px rgba(249, 115, 22, 0.6); }
  .brain-icon { font-size: 16px; }

  .live-section {
    background: rgba(21, 13, 38, 0.7); backdrop-filter: blur(10px); border-bottom: 1px solid rgba(255,255,255,0.05);
    padding: 12px 24px; display: flex; gap: 16px; align-items: center; overflow: hidden; flex-wrap: wrap;
  }
  .best-drop-box {
    flex-shrink: 0; display: flex; align-items: center; gap: 8px;
    background: rgba(249, 115, 22, 0.1); border: 1px solid rgba(249, 115, 22, 0.3);
    padding: 6px 12px; border-radius: 8px; font-size: 12px; font-weight: bold;
  }
  .best-drop-box .bd-emoji { font-size: 20px; }
  .best-drop-box .bd-price { color: var(--accent); }

  .live-feed-wrap {
    flex-grow: 1; overflow: hidden; white-space: nowrap; position: relative;
    mask-image: linear-gradient(to right, transparent, black 5%, black 95%, transparent);
    -webkit-mask-image: linear-gradient(to right, transparent, black 5%, black 95%, transparent);
  }
  .live-feed { display: inline-flex; gap: 12px; will-change: transform; }
  .feed-item {
    display: inline-flex; align-items: center; gap: 6px;
    background: rgba(255,255,255,0.05); border: 1px solid var(--border);
    padding: 4px 10px; border-radius: 20px; font-size: 12px;
  }
  .feed-item .fi-emoji { font-size: 16px; }
  .feed-item .fi-name { font-weight: bold; }
  .feed-item .fi-price { color: var(--gold); }

  .btn {
    background: var(--panel); border: 1px solid var(--border); color: white;
    padding: 12px 20px; border-radius: 12px; font-weight: bold; font-size: 14px;
    cursor: pointer; display: inline-block; text-align: center; transition: 0.2s;
    box-shadow: 0 4px 6px rgba(0,0,0,0.3);
  }
  .btn:hover { filter: brightness(1.2); transform: translateY(-2px); }
  .btn-primary { background: linear-gradient(135deg, #f97316, #ea580c); border-color: #f97316; color: #fff; box-shadow: 0 4px 15px rgba(249, 115, 22, 0.4); }
  .btn-primary:hover { box-shadow: 0 6px 20px rgba(249, 115, 22, 0.6); }
  .btn-fast { background: linear-gradient(135deg, #fbbf24, #d97706); color: #000; border-color: #fbbf24; box-shadow: 0 4px 15px rgba(251, 191, 36, 0.4); }
  .btn:disabled { opacity: 0.5; cursor: not-allowed; transform: none; filter: grayscale(1); box-shadow: none; }

  .container { max-width: 900px; margin: 0 auto; padding: 24px 16px; }
  .page { display: none; animation: fadeIn 0.4s ease; }
  .page.active { display: block; }
  @keyframes fadeIn { from { opacity: 0; transform: translateY(15px); } to { opacity: 1; transform: translateY(0); } }

  .category-title {
    font-size: 18px; font-weight: 900; text-transform: uppercase; letter-spacing: 2px;
    margin: 24px 0 16px; text-align: center; color: var(--accent);
    text-shadow: 0 0 10px rgba(249, 115, 22, 0.5);
    border-bottom: 1px dashed var(--border); padding-bottom: 10px;
  }

  .cases-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(180px, 1fr)); gap: 16px; }
  .case-card {
    background: var(--panel); backdrop-filter: blur(10px);
    border: 1px solid var(--border); border-radius: 20px;
    padding: 20px; cursor: pointer; transition: 0.3s; position: relative; overflow: hidden;
    box-shadow: 0 10px 20px rgba(0,0,0,0.4);
  }
  .case-card:hover { transform: translateY(-8px); border-color: var(--accent); box-shadow: 0 15px 30px rgba(0,0,0,0.6), 0 0 20px rgba(249, 115, 22, 0.2); }
  .case-card.free-case { border-color: var(--gold); box-shadow: 0 0 20px rgba(251, 191, 36, 0.2); }
  .case-card.free-case:hover { border-color: var(--gold); box-shadow: 0 15px 30px rgba(0,0,0,0.6), 0 0 30px rgba(251, 191, 36, 0.4); }
  
  .case-visual { height: 100px; display: flex; align-items: center; justify-content: center; margin-bottom: 16px; }
  .case-box { width: 100px; height: 80px; border-radius: 12px; position: relative; box-shadow: 0 10px 20px rgba(0,0,0,0.8); transform: perspective(600px) rotateX(10deg) rotateY(-10deg); transition: 0.4s; }
  .case-card:hover .case-box { transform: perspective(600px) rotateX(0deg) rotateY(0deg) scale(1.1); }
  .case-box::after { content: ''; position: absolute; top: -8px; left: -8px; right: -8px; height: 22px; background: inherit; border-radius: 12px 12px 0 0; border-bottom: 2px solid rgba(0,0,0,0.5); filter: brightness(1.3); }
  .case-box-emoji { position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); font-size: 40px; z-index: 2; filter: drop-shadow(0 4px 6px rgba(0,0,0,0.5)); }

  .case-title { font-size: 15px; font-weight: 900; text-align: center; margin-bottom: 12px; text-transform: uppercase; color: #e2e8f0; }
  .case-footer { display: flex; justify-content: space-between; border-top: 1px solid rgba(255,255,255,0.1); padding-top: 12px; font-size: 12px; }
  .case-price { font-weight: 900; color: var(--gold); display: flex; align-items: center; gap: 4px; font-size: 14px; }

  .secret-admin-btn { display: inline-block; width: 20px; height: 20px; margin-top: 40px; text-align: center; color: transparent; cursor: pointer; }
  .secret-admin-btn:active { color: rgba(255,255,255,0.1); }

  .back-nav { margin-bottom: 20px; }
  .detail-header { background: var(--panel); backdrop-filter: blur(10px); border: 1px solid var(--border); border-radius: 20px; padding: 24px; margin-bottom: 24px; text-align: center; box-shadow: 0 10px 30px rgba(0,0,0,0.4); }.detail-header .case-box { width: 100px; height: 90px; transform: none; margin: 0 auto; }
  .detail-header h2 { font-size: 28px; margin: 16px 0; font-weight: 900; text-shadow: 0 2px 4px rgba(0,0,0,0.5); }
  
  .qty-selector { display: flex; gap: 8px; margin: 20px 0; max-width: 400px; margin-left: auto; margin-right: auto; background: rgba(0,0,0,0.3); padding: 6px; border-radius: 12px; }
  .qty-btn { flex: 1; padding: 12px; background: transparent; border: none; color: var(--text-muted); font-weight: bold; border-radius: 8px; cursor: pointer; transition: 0.2s; }
  .qty-btn.active { background: var(--accent); color: white; box-shadow: 0 2px 10px rgba(249, 115, 22, 0.4); }

  .chances-title { font-size: 14px; font-weight: bold; color: var(--text-muted); margin-bottom: 16px; text-transform: uppercase; text-align: center; letter-spacing: 1px; }
  .chances-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 12px; }
  .chance-card { background: rgba(13, 6, 23, 0.8); border: 1px solid rgba(255,255,255,0.05); border-radius: 16px; padding: 12px; text-align: center; position: relative; display: flex; flex-direction: column; align-items: center; transition: 0.2s; }
  .chance-card:hover { background: rgba(255,255,255,0.05); transform: translateY(-2px); }
  
  .model-container { width: 80px; height: 80px; margin-bottom: 8px; display: flex; align-items: center; justify-content: center; perspective: 600px; }
  .emoji-model { font-size: 50px; filter: drop-shadow(0px 10px 5px rgba(0,0,0,0.5)); transform: rotateY(-10deg) rotateX(10deg); transition: 0.3s; }
  .chance-card:hover .emoji-model { transform: rotateY(0deg) rotateX(0deg) scale(1.1); }
  
  .chance-name { font-size: 11px; font-weight: 800; margin: 4px 0; min-height: 28px; display: flex; align-items: center; text-align: center;}
  .chance-price { font-size: 11px; background: rgba(255,255,255,0.1); padding: 2px 8px; border-radius: 10px; color: var(--gold); font-weight: 900; margin-bottom: 4px; }
  .chance-percent-top { font-size: 10px; color: var(--green); font-weight: bold; margin-bottom: 4px; }
  
  .badge { position: absolute; top: 8px; right: 8px; font-size: 9px; font-weight: 900; padding: 2px 6px; border-radius: 4px; text-transform: uppercase; letter-spacing: 0.5px; }
  .badge-okup { background: var(--green); color: #000; box-shadow: 0 0 10px rgba(16, 185, 129, 0.4); }
  .badge-minus { background: var(--red); color: #fff; box-shadow: 0 0 10px rgba(239, 68, 68, 0.4); }

  .inv-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(130px, 1fr)); gap: 12px; }
  .inv-item { background: var(--panel); border: 1px solid var(--border); border-radius: 20px; padding: 16px; text-align: center; display: flex; flex-direction: column; align-items: center; box-shadow: 0 4px 10px rgba(0,0,0,0.3); }
  .inv-name { font-size: 12px; font-weight: 800; margin-bottom: 4px; }
  .inv-price { font-size: 11px; color: var(--gold); font-weight: 900; margin-bottom: 12px; }
  .sell-btn { background: rgba(239, 68, 68, 0.2); color: #fca5a5; border: 1px solid rgba(239, 68, 68, 0.5); padding: 8px 12px; border-radius: 8px; font-size: 11px; font-weight: bold; cursor: pointer; width: 100%; transition: 0.2s; }
  .sell-btn:hover { background: var(--red); color: white; }
  .empty-state { text-align: center; color: var(--text-muted); padding: 60px 0; font-size: 16px; }

  .games-menu { display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 16px; }
  .game-card { background: var(--panel); border: 1px solid var(--border); border-radius: 20px; padding: 30px; text-align: center; cursor: pointer; transition: 0.3s; box-shadow: 0 10px 20px rgba(0,0,0,0.3); }
  .game-card:hover { transform: translateY(-8px); border-color: var(--accent); box-shadow: 0 15px 30px rgba(0,0,0,0.5); }
  .game-icon { font-size: 56px; margin-bottom: 12px; filter: drop-shadow(0 5px 10px rgba(0,0,0,0.5)); }
  .game-title { font-size: 20px; font-weight: 900; margin-bottom: 8px; text-transform: uppercase; letter-spacing: 1px; }
  .game-desc { font-size: 13px; color: var(--text-muted); }

  .game-container { background: var(--panel); border: 1px solid var(--border); border-radius: 20px; padding: 24px; box-shadow: 0 10px 30px rgba(0,0,0,0.4); }
  .bet-input-wrapper { display: flex; align-items: center; gap: 8px; justify-content: center; margin-bottom: 16px; }
  .bet-input-wrapper input { width: 120px; padding: 10px; background: rgba(0,0,0,0.5); border: 1px solid var(--border); border-radius: 10px; color: white; font-size: 16px; font-weight: bold; text-align: center; outline: none; transition: 0.2s; }
  .bet-input-wrapper input:focus { border-color: var(--accent); box-shadow: 0 0 10px rgba(249, 115, 22, 0.3); }
  
  .rocket-window { height: 220px; background: #000; border-radius: 16px; position: relative; overflow: hidden; margin-bottom: 20px; display: flex; align-items: center; justify-content: center; border: 1px solid var(--border); box-shadow: inset 0 0 50px rgba(0,0,0,0.8); }
  .rocket-multiplier-display { font-size: 52px; font-weight: 900; color: var(--green); text-shadow: 0 0 20px rgba(16, 185, 129, 0.5); }
  .rocket-multiplier-display.crashed { color: var(--red); text-shadow: 0 0 20px rgba(239, 68, 68, 0.5); }
  .rocket-sprite { position: absolute; bottom: 20px; left: 50%; margin-left: -30px; font-size: 60px; will-change: transform, bottom; }

  .upgrade-container { display: flex; flex-direction: column; align-items: center; margin-bottom: 20px; }
  .upgrade-wheel-wrap { position: relative; width: 260px; height: 260px; margin-bottom: 24px; }
  .wheel-arrow { position: absolute; top: -15px; left: 50%; margin-left: -15px; font-size: 30px; color: var(--accent); z-index: 10; transform-origin: 15px 145px; will-change: transform; filter: drop-shadow(0 0 10px rgba(249, 115, 22, 0.8)); }
  .upgrade-wheel-canvas { width: 100%; height: 100%; border-radius: 50%; border: 6px solid #1a1030; position: relative; z-index: 2; box-shadow: 0 0 30px rgba(0,0,0,0.8); }
  .upgrade-slots { display: flex; justify-content: center; gap: 16px; margin-bottom: 20px; width: 100%; }
  .up-slot { background: rgba(0,0,0,0.4); border: 1px solid var(--border); border-radius: 16px; padding: 16px; text-align: center; flex: 1; max-width: 140px; box-shadow: inset 0 0 20px rgba(0,0,0,0.5); }
  .up-slot-icon { font-size: 36px; margin-bottom: 8px; filter: drop-shadow(0 5px 10px rgba(0,0,0,0.5)); }
  .up-slot-name { font-size: 12px; font-weight: bold; }
  .chance-selector { display: flex; gap: 8px; margin-bottom: 16px; width: 100%; max-width: 300px; }
  .chance-option { flex: 1; padding: 10px; background: rgba(0,0,0,0.3); border: 1px solid var(--border); color: white; font-weight: bold; border-radius: 10px; cursor: pointer; transition: 0.2s; }
  .chance-option.active { background: var(--accent); border-color: var(--accent); box-shadow: 0 0 10px rgba(249, 115, 22, 0.4); }

  .mines-grid { display: grid; grid-template-columns: repeat(5, 1fr); gap: 8px; max-width: 320px; margin: 0 auto 20px; }
  .mine-cell { aspect-ratio: 1; background: linear-gradient(145deg, #2a1a45, #150d26); border: 1px solid rgba(255,255,255,0.05); border-radius: 12px; display: flex; align-items: center; justify-content: center; font-size: 24px; cursor: pointer; transition: 0.1s; box-shadow: 0 4px 6px rgba(0,0,0,0.3); }
  .mine-cell:hover { filter: brightness(1.3); transform: scale(1.02); }
  .mine-cell.opened { background: #0a0514; cursor: default; box-shadow: inset 0 5px 10px rgba(0,0,0,0.8); transform: scale(0.95); }
  .mine-cell.opened.mine { background: var(--red); }
  .mine-cell.opened.gem { background: var(--green); }

  .coin-game-container { text-align: center; }
  .coin-img { font-size: 100px; margin: 20px 0; transition: transform 0.5s; display: inline-block; filter: drop-shadow(0 10px 20px rgba(0,0,0,0.5)); }
  .coin-img.flipping { animation: flip 1s infinite linear; }
  @keyframes flip { 0% { transform: rotateY(0); } 100% { transform: rotateY(360deg); } }.rules-box { background: rgba(0,0,0,0.3); border: 1px solid var(--border); border-radius: 16px; padding: 16px; text-align: left; font-size: 13px; color: #cbd5e1; margin-bottom: 20px; line-height: 1.6; }
  .rules-box b { color: var(--accent); }

  .roulette-overlay { position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(4, 2, 10, 0.95); backdrop-filter: blur(5px); z-index: 100; display: none; flex-direction: column; align-items: center; justify-content: center; padding: 16px; overflow-y: auto;}
  .roulette-overlay.active { display: flex; }
  
  .roulette-box { width: 100%; max-width: 800px; height: 130px; background: #110a1f; border: 1px solid var(--border); position: relative; overflow: hidden; border-radius: 16px; margin-bottom: 15px; box-shadow: inset 0 0 50px rgba(0,0,0,0.8); }
  .roulette-center-line { position: absolute; top: 0; bottom: 0; left: 50%; width: 2px; background: var(--accent); z-index: 10; margin-left: -1px; box-shadow: 0 0 15px var(--accent); }
  .roulette-track { display: flex; height: 100%; position: absolute; left: 0; will-change: transform; }
  .r-slot { flex: 0 0 110px; height: 100%; border-right: 1px solid rgba(255,255,255,0.05); display: flex; flex-direction: column; align-items: center; justify-content: center; background: rgba(255,255,255,0.02); padding: 5px 0;}
  .r-slot .r-name { font-size: 10px; font-weight: bold; text-align: center; padding: 0 4px; margin-bottom: 4px; line-height: 1.1;}
  .r-slot .r-price { font-size: 11px; color: var(--gold); font-weight: 900; background: rgba(0,0,0,0.6); padding: 1px 6px; border-radius: 4px;}

  .r-result { font-size: 24px; font-weight: bold; text-align: center; margin-bottom: 24px; display: none; }
  .r-result.show { display: block; animation: popIn 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275); }
  .r-result.win { color: var(--green); text-shadow: 0 0 15px rgba(16, 185, 129, 0.5); }
  .r-result.lose { color: var(--red); text-shadow: 0 0 15px rgba(239, 68, 68, 0.5); }
  @keyframes popIn { 0% { transform: scale(0.8); opacity: 0; } 100% { transform: scale(1); opacity: 1; } }

  .status-text { font-size: 15px; font-weight: bold; margin-bottom: 16px; min-height: 22px; text-align: center; }

  .modal-overlay { position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0,0,0,0.85); backdrop-filter: blur(5px); z-index: 200; display: none; align-items: center; justify-content: center; padding: 16px; }
  .modal-overlay.active { display: flex; }
  .modal { background: linear-gradient(145deg, #1a1030, #0f081c); border: 2px solid var(--border); border-radius: 24px; padding: 24px; width: 100%; max-width: 500px; text-align: left; max-height: 90vh; overflow-y: auto; box-shadow: 0 20px 50px rgba(0,0,0,0.8); }
  .modal h3 { margin-bottom: 20px; font-size: 24px; text-align: center; color: var(--text); font-weight: 900; text-shadow: 0 2px 4px rgba(0,0,0,0.5); }
  
  .deposit-tabs { display: flex; gap: 8px; margin-bottom: 20px; background: rgba(0,0,0,0.4); padding: 6px; border-radius: 14px; }
  .deposit-tab { flex: 1; padding: 12px; text-align: center; color: var(--text-muted); font-weight: bold; cursor: pointer; border-radius: 10px; font-size: 14px; transition: 0.2s; }
  .deposit-tab.active { background: var(--accent); color: white; box-shadow: 0 4px 10px rgba(249, 115, 22, 0.3); }
  
  .deposit-inputs { display: flex; gap: 8px; margin-bottom: 16px; }
  .deposit-inputs input { width: 100%; padding: 12px; background: rgba(0,0,0,0.5); border: 1px solid var(--border); border-radius: 10px; color: white; font-size: 14px; outline: none; }
  .deposit-inputs input:focus { border-color: var(--accent); }
  .deposit-inputs button { padding: 10px 16px; background: rgba(255,255,255,0.05); border: 1px solid var(--border); color: white; border-radius: 10px; font-weight: bold; cursor: pointer; transition: 0.2s; }
  .deposit-inputs button:hover { background: rgba(255,255,255,0.1); }
  
  .deposit-items-grid { display: grid; grid-template-co
