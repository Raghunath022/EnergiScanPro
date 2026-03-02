<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>EnergiScan Enterprise — Multi-Machine Pro</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=IBM+Plex+Mono:wght@400;500;600&family=Outfit:wght@300;400;500;600;700&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.4.1/papaparse.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
<style>
*,*::before,*::after{margin:0;padding:0;box-sizing:border-box}
:root{
  --bg:#07101e;--bg2:#0b1628;--bg3:#0f1d34;--surf:#131f35;--surf2:#192640;
  --bdr:rgba(255,255,255,0.07);--bdr2:rgba(255,255,255,0.13);
  --cyan:#00d4ff;--cyan2:rgba(0,212,255,0.12);
  --grn:#00e676;--grn2:rgba(0,230,118,0.12);
  --amb:#ffab00;--amb2:rgba(255,171,0,0.12);
  --red:#ff4444;--red2:rgba(255,68,68,0.12);
  --pur:#b388ff;--pur2:rgba(179,136,255,0.1);
  --txt:#dde6f0;--txt2:#8fa3be;--txt3:#4a6080;
  --sidebar:260px;
}
html{scroll-behavior:smooth}
body{font-family:'Outfit',sans-serif;background:var(--bg);color:var(--txt);overflow-x:hidden;min-height:100vh}
body::before{content:'';position:fixed;inset:0;background-image:linear-gradient(rgba(0,212,255,.025) 1px,transparent 1px),linear-gradient(90deg,rgba(0,212,255,.025) 1px,transparent 1px);background-size:44px 44px;pointer-events:none;z-index:0}
.mono{font-family:'IBM Plex Mono',monospace}
.syne{font-family:'Syne',sans-serif}

/* ── SCROLLBAR ── */
::-webkit-scrollbar{width:4px;height:4px}
::-webkit-scrollbar-thumb{background:var(--bdr2);border-radius:4px}

/* ── SIDEBAR ── */
.sidebar{position:fixed;left:0;top:0;bottom:0;width:var(--sidebar);background:var(--bg2);border-right:1px solid var(--bdr);z-index:200;display:flex;flex-direction:column;transition:transform .3s ease}
.sidebar-logo{padding:20px 18px;border-bottom:1px solid var(--bdr);display:flex;align-items:center;gap:10px}
.logo-icon{width:36px;height:36px;background:linear-gradient(135deg,var(--cyan),#0070ff);border-radius:10px;display:flex;align-items:center;justify-content:center;font-size:18px;flex-shrink:0;box-shadow:0 0 18px rgba(0,212,255,.3)}
.logo-name{font-family:'Syne',sans-serif;font-weight:800;font-size:16px;letter-spacing:-.3px}
.logo-tag{font-size:9px;font-family:'IBM Plex Mono',monospace;color:var(--cyan);letter-spacing:2px}

/* Machine list in sidebar */
.sb-machines{padding:12px;border-bottom:1px solid var(--bdr);overflow-y:auto;max-height:280px;flex-shrink:0}
.sb-label{font-size:9px;font-family:'IBM Plex Mono',monospace;color:var(--txt3);letter-spacing:2px;text-transform:uppercase;padding:4px 4px 8px}
.mc-btn{display:flex;align-items:center;gap:8px;width:100%;padding:8px 10px;border:1px solid transparent;border-radius:8px;background:transparent;color:var(--txt2);font-size:12.5px;font-family:'Outfit',sans-serif;cursor:pointer;transition:.2s;text-align:left;margin-bottom:2px}
.mc-btn:hover{background:var(--surf);color:var(--txt)}
.mc-btn.active{background:var(--cyan2);border-color:rgba(0,212,255,.25);color:var(--cyan)}
.mc-btn.all-btn.active{background:var(--pur2);border-color:rgba(179,136,255,.2);color:var(--pur)}
.mc-dot{width:7px;height:7px;border-radius:50%;flex-shrink:0}
.mc-dot.online{background:var(--grn);box-shadow:0 0 5px var(--grn)}
.mc-dot.warning{background:var(--amb);box-shadow:0 0 5px var(--amb)}
.mc-dot.offline{background:var(--red)}
.mc-dot.all{background:var(--pur)}
.mc-kw{margin-left:auto;font-family:'IBM Plex Mono',monospace;font-size:10px;color:var(--txt3)}

/* Nav */
.sb-nav{flex:1;overflow-y:auto;padding:8px 0}
.sb-section{font-size:9px;font-family:'IBM Plex Mono',monospace;color:var(--txt3);letter-spacing:2px;padding:10px 18px 6px;text-transform:uppercase}
.nav-item{display:flex;align-items:center;gap:9px;padding:9px 18px;color:var(--txt3);cursor:pointer;font-size:13.5px;font-weight:500;border-left:2px solid transparent;transition:.2s;user-select:none}
.nav-item:hover{color:var(--txt);background:rgba(255,255,255,.03)}
.nav-item.active{color:var(--cyan);border-left-color:var(--cyan);background:rgba(0,212,255,.05)}
.nav-icon{font-size:15px;width:18px;text-align:center}
.sb-footer{padding:12px;border-top:1px solid var(--bdr)}
.add-machine-btn{width:100%;padding:9px;background:var(--cyan2);border:1px solid rgba(0,212,255,.25);border-radius:8px;color:var(--cyan);font-size:12.5px;font-family:'Outfit',sans-serif;font-weight:600;cursor:pointer;transition:.2s;display:flex;align-items:center;justify-content:center;gap:6px}
.add-machine-btn:hover{background:rgba(0,212,255,.2)}

/* ── TOPBAR ── */
.main{margin-left:var(--sidebar);min-height:100vh;position:relative;z-index:1}
.topbar{background:var(--bg2);border-bottom:1px solid var(--bdr);padding:14px 24px;display:flex;justify-content:space-between;align-items:center;position:sticky;top:0;z-index:100}
.topbar-left h2{font-family:'Syne',sans-serif;font-size:20px;font-weight:700;letter-spacing:-.3px}
.topbar-left p{font-size:12px;color:var(--txt3);margin-top:1px}
.topbar-right{display:flex;gap:8px;align-items:center;flex-wrap:wrap}
.live-pill{display:flex;align-items:center;gap:6px;padding:5px 12px;background:var(--grn2);border:1px solid rgba(0,230,118,.2);border-radius:100px;font-size:11px;font-family:'IBM Plex Mono',monospace;color:var(--grn)}
.live-dot{width:6px;height:6px;border-radius:50%;background:var(--grn);animation:blink 1.2s infinite}
@keyframes blink{0%,100%{opacity:1}50%{opacity:.3}}
.hamburger{display:none;flex-direction:column;gap:4px;cursor:pointer;padding:4px;background:transparent;border:none}
.hamburger span{display:block;width:22px;height:2px;background:var(--txt2);border-radius:2px;transition:.3s}

/* ── CONTENT ── */
.content{padding:22px 24px}
.container{max-width:1600px;margin:0 auto}

/* ── VIEW ── */
.view{display:none}
.view.active{display:block}

/* ── GRID ── */
.grid{display:grid;gap:18px}
.g2{grid-template-columns:repeat(2,1fr)}
.g3{grid-template-columns:repeat(3,1fr)}
.g4{grid-template-columns:repeat(4,1fr)}
.g5{grid-template-columns:repeat(5,1fr)}
.mb{margin-bottom:20px}

/* ── CARD ── */
.card{background:var(--surf);border:1px solid var(--bdr);border-radius:12px;padding:18px}
.card-hd{display:flex;justify-content:space-between;align-items:center;margin-bottom:14px}
.card-title{font-family:'Syne',sans-serif;font-size:14px;font-weight:700}

/* ── STAT ── */
.stat{background:var(--surf);border:1px solid var(--bdr);border-radius:12px;padding:16px;position:relative;overflow:hidden}
.stat::after{content:'';position:absolute;top:0;left:0;right:0;height:2px}
.stat.c::after{background:linear-gradient(90deg,transparent,var(--cyan),transparent)}
.stat.g::after{background:linear-gradient(90deg,transparent,var(--grn),transparent)}
.stat.a::after{background:linear-gradient(90deg,transparent,var(--amb),transparent)}
.stat.p::after{background:linear-gradient(90deg,transparent,var(--pur),transparent)}
.stat.r::after{background:linear-gradient(90deg,transparent,var(--red),transparent)}
.stat-lbl{font-size:10px;font-family:'IBM Plex Mono',monospace;letter-spacing:1.5px;color:var(--txt3);text-transform:uppercase;margin-bottom:8px}
.stat-val{font-family:'IBM Plex Mono',monospace;font-size:26px;font-weight:600;line-height:1}
.stat-unit{font-size:12px;color:var(--txt2);margin-top:3px}
.stat-delta{font-size:11px;margin-top:8px;font-weight:600}
.up{color:var(--grn)}.dn{color:var(--red)}

/* ── MACHINE CARDS ── */
.mcard{background:var(--surf);border:1px solid var(--bdr);border-radius:12px;padding:16px;cursor:pointer;transition:.2s;position:relative;overflow:hidden}
.mcard:hover{border-color:var(--bdr2);transform:translateY(-2px);box-shadow:0 4px 20px rgba(0,0,0,.3)}
.mcard.selected{border-color:var(--cyan);box-shadow:0 0 16px rgba(0,212,255,.12)}
.mcard-glow{position:absolute;top:0;left:0;right:0;height:60px;opacity:0;transition:.2s;pointer-events:none}
.mcard:hover .mcard-glow{opacity:1}
.mcard-hd{display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:12px}
.mcard-name{font-family:'Syne',sans-serif;font-size:13px;font-weight:700}
.mcard-id{font-family:'IBM Plex Mono',monospace;font-size:9px;color:var(--txt3);margin-top:2px}
.status-pill{display:flex;align-items:center;gap:4px;font-size:10px;font-family:'IBM Plex Mono',monospace;font-weight:600;padding:3px 8px;border-radius:5px}
.status-pill.online{background:var(--grn2);color:var(--grn)}
.status-pill.warning{background:var(--amb2);color:var(--amb)}
.status-pill.offline{background:var(--red2);color:var(--red)}
.mcard-pwr{font-family:'IBM Plex Mono',monospace;font-size:22px;font-weight:600;margin-bottom:2px}
.mcard-unit{font-size:11px;color:var(--txt3)}
.bar-bg{height:4px;background:var(--bg3);border-radius:2px;margin-top:12px;overflow:hidden}
.bar-fill{height:100%;border-radius:2px;transition:width .6s ease}
.mcard-meta{display:flex;justify-content:space-between;margin-top:8px;font-size:10px;color:var(--txt3);font-family:'IBM Plex Mono',monospace}

/* ── BADGES ── */
.badge{display:inline-flex;align-items:center;gap:3px;padding:2px 8px;border-radius:4px;font-size:10px;font-family:'IBM Plex Mono',monospace;font-weight:600;letter-spacing:.3px}
.bc{background:var(--cyan2);color:var(--cyan)}
.bg{background:var(--grn2);color:var(--grn)}
.ba{background:var(--amb2);color:var(--amb)}
.br{background:var(--red2);color:var(--red)}
.bp{background:var(--pur2);color:var(--pur)}

/* ── BUTTONS ── */
.btn{display:inline-flex;align-items:center;gap:6px;padding:8px 16px;border-radius:8px;border:none;font-size:12.5px;font-weight:600;cursor:pointer;font-family:'Outfit',sans-serif;transition:.2s}
.btn-c{background:var(--cyan2);color:var(--cyan);border:1px solid rgba(0,212,255,.2)}
.btn-c:hover{background:rgba(0,212,255,.22)}
.btn-g{background:transparent;color:var(--txt2);border:1px solid var(--bdr)}
.btn-g:hover{background:var(--surf2);color:var(--txt)}
.btn-r{background:var(--red2);color:var(--red);border:1px solid rgba(255,68,68,.2)}
.btn-sm{padding:5px 12px;font-size:11.5px}

/* ── CHART ── */
.chart-box{position:relative;height:240px}
.chart-box.tall{height:320px}

/* ── TABLE ── */
.tbl{width:100%;border-collapse:collapse}
.tbl th{padding:9px 12px;text-align:left;font-size:10px;font-family:'IBM Plex Mono',monospace;letter-spacing:1.5px;color:var(--txt3);text-transform:uppercase;border-bottom:1px solid var(--bdr);background:var(--bg3)}
.tbl td{padding:12px;border-bottom:1px solid var(--bdr);font-size:12.5px}
.tbl tr:last-child td{border-bottom:none}
.tbl tr:hover td{background:rgba(255,255,255,.015)}

/* ── SENSOR ── */
.sensor-item{background:var(--bg3);border:1px solid var(--bdr);border-radius:9px;padding:12px}
.s-lbl{font-size:9px;font-family:'IBM Plex Mono',monospace;letter-spacing:1.5px;color:var(--txt3);text-transform:uppercase;margin-bottom:5px}
.s-val{font-family:'IBM Plex Mono',monospace;font-size:20px;font-weight:600}
.s-unit{font-size:10px;color:var(--txt3)}

/* ── FORM ── */
.f-lbl{display:block;font-size:10px;font-family:'IBM Plex Mono',monospace;letter-spacing:1px;color:var(--txt3);text-transform:uppercase;margin-bottom:6px}
.f-inp{width:100%;padding:9px 12px;background:var(--bg3);border:1px solid var(--bdr);border-radius:8px;color:var(--txt);font-size:13px;font-family:'Outfit',sans-serif;transition:.2s}
.f-inp:focus{outline:none;border-color:rgba(0,212,255,.4);box-shadow:0 0 0 3px rgba(0,212,255,.07)}
select.f-inp{cursor:pointer}
.fgrp{margin-bottom:14px}

/* ── UPLOAD ZONE ── */
.upload-zone{border:2px dashed var(--bdr2);border-radius:10px;padding:40px 20px;text-align:center;cursor:pointer;transition:.2s;background:var(--bg3)}
.upload-zone:hover,.upload-zone.drag{border-color:var(--cyan);background:rgba(0,212,255,.04)}

/* ── ALERT ── */
.alert{display:flex;gap:10px;padding:12px 14px;border-radius:8px;font-size:12.5px;margin-bottom:10px}
.alert-w{background:var(--amb2);border:1px solid rgba(255,171,0,.2);color:#ffd54f}
.alert-g{background:var(--grn2);border:1px solid rgba(0,230,118,.2);color:#69f0ae}
.alert-i{background:var(--cyan2);border:1px solid rgba(0,212,255,.2);color:var(--cyan)}
.alert-r{background:var(--red2);border:1px solid rgba(255,68,68,.2);color:#ff8a80}

/* ── MODAL ── */
.modal{display:none;position:fixed;inset:0;background:rgba(0,0,0,.7);z-index:1000;align-items:center;justify-content:center;padding:16px;backdrop-filter:blur(4px)}
.modal.open{display:flex}
.modal-box{background:var(--bg2);border:1px solid var(--bdr2);border-radius:14px;width:100%;max-width:520px;max-height:90vh;overflow-y:auto}
.modal-hd{padding:20px 22px;border-bottom:1px solid var(--bdr);display:flex;justify-content:space-between;align-items:center}
.modal-hd h3{font-family:'Syne',sans-serif;font-size:17px;font-weight:700}
.modal-bd{padding:22px}
.modal-ft{padding:14px 22px;border-top:1px solid var(--bdr);display:flex;gap:8px;justify-content:flex-end}

/* ── AI SUGGESTIONS PANEL ── */
.ai-panel{position:fixed;right:0;top:0;bottom:0;width:340px;background:var(--bg2);border-left:1px solid var(--bdr);z-index:150;display:flex;flex-direction:column;transform:translateX(100%);transition:transform .35s ease}
.ai-panel.open{transform:translateX(0)}
.ai-panel-hd{padding:18px 18px 14px;border-bottom:1px solid var(--bdr);display:flex;justify-content:space-between;align-items:center}
.ai-panel-title{font-family:'Syne',sans-serif;font-size:15px;font-weight:700;display:flex;align-items:center;gap:8px}
.ai-badge{background:linear-gradient(135deg,var(--cyan),var(--pur));-webkit-background-clip:text;-webkit-text-fill-color:transparent;font-size:12px;font-weight:800}
.ai-body{flex:1;overflow-y:auto;padding:14px}
.ai-input-area{padding:14px;border-top:1px solid var(--bdr);display:flex;flex-direction:column;gap:8px}
.ai-textarea{width:100%;padding:10px 12px;background:var(--bg3);border:1px solid var(--bdr);border-radius:8px;color:var(--txt);font-size:12.5px;font-family:'Outfit',sans-serif;resize:none;height:80px;transition:.2s}
.ai-textarea:focus{outline:none;border-color:rgba(0,212,255,.35)}
.ai-send-btn{padding:9px;background:linear-gradient(135deg,var(--cyan),#0070ff);border:none;border-radius:8px;color:#fff;font-size:12.5px;font-weight:700;cursor:pointer;font-family:'Outfit',sans-serif;transition:.2s}
.ai-send-btn:hover{opacity:.9}
.ai-send-btn:disabled{opacity:.5;cursor:not-allowed}

/* AI message bubbles */
.ai-msg{margin-bottom:12px;animation:fadeIn .3s ease}
@keyframes fadeIn{from{opacity:0;transform:translateY(4px)}to{opacity:1;transform:translateY(0)}}
.ai-msg-user{display:flex;justify-content:flex-end}
.ai-msg-user .bubble{background:rgba(0,212,255,.1);border:1px solid rgba(0,212,255,.2);color:var(--txt);border-radius:12px 12px 2px 12px;padding:9px 12px;font-size:12px;max-width:85%}
.ai-msg-bot .bubble{background:var(--surf2);border:1px solid var(--bdr2);color:var(--txt);border-radius:12px 12px 12px 2px;padding:11px 13px;font-size:12.5px;line-height:1.6;max-width:95%}
.ai-msg-bot .bubble strong{color:var(--cyan)}
.ai-msg-bot .bubble ul{margin:6px 0 6px 14px}
.ai-msg-bot .bubble li{margin-bottom:3px}
.typing-indicator{display:flex;gap:4px;padding:8px 12px}
.typing-dot{width:6px;height:6px;background:var(--txt3);border-radius:50%;animation:typing .8s infinite}
.typing-dot:nth-child(2){animation-delay:.2s}
.typing-dot:nth-child(3){animation-delay:.4s}
@keyframes typing{0%,80%,100%{transform:scale(1);opacity:.5}40%{transform:scale(1.2);opacity:1}}

/* Quick suggestion chips */
.suggestion-chips{display:flex;flex-wrap:wrap;gap:6px;margin-bottom:12px}
.chip{padding:5px 10px;background:var(--surf2);border:1px solid var(--bdr2);border-radius:100px;font-size:11px;color:var(--txt2);cursor:pointer;transition:.2s}
.chip:hover{border-color:rgba(0,212,255,.3);color:var(--cyan)}

/* AI trigger button */
.ai-trigger{display:flex;align-items:center;gap:6px;padding:7px 14px;background:linear-gradient(135deg,rgba(0,212,255,.15),rgba(179,136,255,.15));border:1px solid rgba(0,212,255,.25);border-radius:8px;color:var(--txt);font-size:12.5px;font-weight:600;cursor:pointer;font-family:'Outfit',sans-serif;transition:.2s}
.ai-trigger:hover{background:linear-gradient(135deg,rgba(0,212,255,.25),rgba(179,136,255,.22))}
.ai-sparkle{font-size:14px}

/* Overlay for AI panel on mobile */
.overlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,.5);z-index:140}
.overlay.show{display:block}

/* Compare bars */
.cmp-bar-wrap{display:flex;align-items:center;gap:8px}
.cmp-bar-bg{flex:1;height:5px;background:var(--bg3);border-radius:3px;overflow:hidden}
.cmp-bar-fill{height:100%;border-radius:3px}

/* Tabs */
.tabs{display:flex;gap:2px;background:var(--bg3);border-radius:8px;padding:3px;width:fit-content;margin-bottom:18px}
.tab-btn{padding:6px 14px;border-radius:6px;border:none;background:transparent;color:var(--txt3);font-size:12.5px;font-weight:600;cursor:pointer;font-family:'Outfit',sans-serif;transition:.2s}
.tab-btn.active{background:var(--surf2);color:var(--txt)}

/* ── REPORT TYPE CARDS ── */
.rpt-type-card{background:var(--bg3);border:1px solid var(--bdr);border-radius:10px;padding:14px 10px;text-align:center;cursor:pointer;transition:.2s}
.rpt-type-card:hover{border-color:var(--bdr2);background:var(--surf)}
.rpt-type-card.active{border-color:var(--cyan);background:rgba(0,212,255,.08)}
.rpt-type-icon{font-size:24px;margin-bottom:6px}
.rpt-type-name{font-size:12px;font-weight:700;margin-bottom:2px}
.rpt-type-desc{font-size:10px;color:var(--txt3)}

/* ── REPORT PREVIEW (light theme inside dark) ── */
#rptPreview{font-family:'Outfit',sans-serif;color:#111}
#rptPreview *{box-sizing:border-box}

/* ── RESPONSIVE ── */
@media(max-width:1100px){
  .g5{grid-template-columns:repeat(3,1fr)}
  .g4{grid-template-columns:repeat(2,1fr)}
}
@media(max-width:900px){
  :root{--sidebar:0px}
  .sidebar{transform:translateX(-260px);width:260px}
  .sidebar.open{transform:translateX(0)}
  .main{margin-left:0}
  .hamburger{display:flex}
  .g3,.g2{grid-template-columns:1fr}
  .g5{grid-template-columns:repeat(2,1fr)}
  .g4{grid-template-columns:repeat(2,1fr)}
  .content{padding:16px}
  .topbar{padding:12px 16px}
}
@media(max-width:600px){
  .g4,.g5{grid-template-columns:1fr 1fr}
  .topbar-left h2{font-size:17px}
  .ai-panel{width:100%;max-width:100%}
}
@media(max-width:400px){
  .g4,.g5{grid-template-columns:1fr}
}

/* Overlay for mobile sidebar */
.sb-overlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,.5);z-index:195}
.sb-overlay.show{display:block}
</style>
</head>
<body>

<!-- Mobile sidebar overlay -->
<div class="sb-overlay" id="sbOverlay" onclick="closeSidebar()"></div>

<!-- ── SIDEBAR ── -->
<aside class="sidebar" id="sidebar">
  <div class="sidebar-logo">
    <div class="logo-icon">⚡</div>
    <div>
      <div class="logo-name">EnergiScan</div>
      <div class="logo-tag">ENTERPRISE · MULTI-MACHINE</div>
    </div>
  </div>

  <div class="sb-machines">
    <div class="sb-label">Fleet Machines</div>
    <div id="sbMachineList"></div>
  </div>

  <nav class="sb-nav">
    <div class="sb-section">Navigation</div>
    <div class="nav-item active" data-view="fleet" onclick="nav('fleet',this)"><span class="nav-icon">🏭</span>Fleet Overview</div>
    <div class="nav-item" data-view="machine" onclick="nav('machine',this)"><span class="nav-icon">⚙️</span>Machine Detail</div>
    <div class="nav-item" data-view="compare" onclick="nav('compare',this)"><span class="nav-icon">⚖️</span>Compare Machines</div>
    <div class="nav-item" data-view="realtime" onclick="nav('realtime',this)"><span class="nav-icon">📡</span>Live Sensors</div>
    <div class="nav-item" data-view="upload" onclick="nav('upload',this)"><span class="nav-icon">📤</span>Data Upload</div>
    <div class="nav-item" data-view="analytics" onclick="nav('analytics',this)"><span class="nav-icon">📈</span>Analytics</div>
    <div class="nav-item" data-view="suggestions" onclick="nav('suggestions',this)"><span class="nav-icon">🤖</span>AI Suggestions</div>
    <div class="nav-item" data-view="reports" onclick="nav('reports',this)"><span class="nav-icon">📄</span>Reports</div>
    <div class="nav-item" data-view="settings" onclick="nav('settings',this)"><span class="nav-icon">⚙️</span>Settings</div>
  </nav>

  <div class="sb-footer">
    <button class="add-machine-btn" onclick="openAddModal()">＋ Add Machine</button>
  </div>
</aside>

<!-- ── MAIN ── -->
<div class="main" id="main">

<!-- TOPBAR -->
<div class="topbar">
  <div class="topbar-left">
    <h2 id="topTitle" class="syne">Fleet Overview</h2>
    <p id="topSub">Loading...</p>
  </div>
  <div class="topbar-right">
    <button class="hamburger" id="hamburger" onclick="toggleSidebar()">
      <span></span><span></span><span></span>
    </button>
    <div class="live-pill"><span class="live-dot"></span>LIVE</div>
    <button class="ai-trigger" onclick="toggleAIPanel()">
      <span class="ai-sparkle">✨</span>AI Suggestions
    </button>
    <button class="btn btn-g btn-sm" onclick="refreshAll()">↺ Refresh</button>
  </div>
</div>

<!-- ════ FLEET VIEW ════ -->
<div id="view-fleet" class="view active">
<div class="content"><div class="container">

  <!-- KPIs -->
  <div class="grid g5 mb">
    <div class="stat c"><div class="stat-lbl">Total Power</div><div class="stat-val" id="kpi-power" style="color:var(--cyan)">—</div><div class="stat-unit">kW Fleet</div><div class="stat-delta up" id="kpi-pdelta">↑ 6.2%</div></div>
    <div class="stat g"><div class="stat-lbl">Online</div><div class="stat-val" id="kpi-online" style="color:var(--grn)">—</div><div class="stat-unit" id="kpi-onlinesub">of — machines</div></div>
    <div class="stat a"><div class="stat-lbl">Monthly Cost</div><div class="stat-val" id="kpi-cost" style="color:var(--amb)">—</div><div class="stat-unit">₹ projected</div><div class="stat-delta" id="kpi-cdelta"></div></div>
    <div class="stat p"><div class="stat-lbl">Fleet Eff.</div><div class="stat-val" id="kpi-eff" style="color:var(--pur)">—</div><div class="stat-unit">%</div></div>
    <div class="stat g"><div class="stat-lbl">CO₂ Avoided</div><div class="stat-val" id="kpi-co2" style="color:var(--grn)">—</div><div class="stat-unit">kg/mo</div></div>
  </div>

  <!-- Machine Cards -->
  <div class="card-hd mb" style="margin-bottom:12px">
    <div class="card-title syne" style="font-size:16px">Machine Status</div>
    <div style="display:flex;gap:6px;flex-wrap:wrap">
      <button class="btn btn-g btn-sm" onclick="filterCards('all')">All</button>
      <button class="btn btn-g btn-sm" onclick="filterCards('online')">🟢 Online</button>
      <button class="btn btn-g btn-sm" onclick="filterCards('warning')">🟡 Warning</button>
      <button class="btn btn-g btn-sm" onclick="filterCards('offline')">🔴 Offline</button>
    </div>
  </div>
  <div class="grid g4 mb" id="fleetGrid"></div>

  <!-- Charts -->
  <div class="grid g2 mb">
    <div class="card">
      <div class="card-hd"><div class="card-title syne">Fleet Power 24h</div><span class="badge bc">kW</span></div>
      <div class="chart-box"><canvas id="fleetPowerChart"></canvas></div>
    </div>
    <div class="card">
      <div class="card-hd"><div class="card-title syne">Load Distribution</div><span class="badge bp">by machine</span></div>
      <div class="chart-box"><canvas id="fleetDistChart"></canvas></div>
    </div>
  </div>

  <!-- Alerts -->
  <div class="card">
    <div class="card-hd"><div class="card-title syne">Fleet Alerts</div></div>
    <div id="fleetAlerts"></div>
  </div>

</div></div>
</div>

<!-- ════ MACHINE DETAIL ════ -->
<div id="view-machine" class="view">
<div class="content"><div class="container">

  <div class="grid g4 mb">
    <div class="stat c"><div class="stat-lbl">Power</div><div class="stat-val" id="md-pwr" style="color:var(--cyan)">—</div><div class="stat-unit">kW</div></div>
    <div class="stat g"><div class="stat-lbl">Today Usage</div><div class="stat-val" id="md-kwh" style="color:var(--grn)">—</div><div class="stat-unit">kWh</div></div>
    <div class="stat a"><div class="stat-lbl">Op. Hours</div><div class="stat-val" id="md-hrs" style="color:var(--amb)">—</div><div class="stat-unit">h today</div></div>
    <div class="stat p"><div class="stat-lbl">Efficiency</div><div class="stat-val" id="md-eff" style="color:var(--pur)">—</div><div class="stat-unit">%</div></div>
  </div>

  <div class="grid g2 mb">
    <div class="card">
      <div class="card-hd"><div class="card-title syne">Live Sensors</div><span class="badge bg">Real-time</span></div>
      <div class="grid g2" id="mdSensors" style="gap:10px"></div>
    </div>
    <div class="card">
      <div class="card-hd"><div class="card-title syne">Machine Info</div>
        <div style="display:flex;gap:6px">
          <button class="btn btn-g btn-sm" onclick="openEditModal()">✏️ Edit</button>
          <button class="btn btn-r btn-sm" onclick="openRemoveModal()">🗑 Remove</button>
        </div>
      </div>
      <div id="mdInfo"></div>
    </div>
  </div>

  <div class="card mb">
    <div class="card-hd">
      <div class="card-title syne">Power Trend — 24h</div>
      <button class="btn btn-g btn-sm" onclick="exportMachineCSV()">↓ Export CSV</button>
    </div>
    <div class="chart-box tall"><canvas id="mdPowerChart"></canvas></div>
  </div>

  <div class="card">
    <div class="card-hd"><div class="card-title syne">Machine Alerts</div></div>
    <div id="mdAlerts"></div>
  </div>

</div></div>
</div>

<!-- ════ COMPARE ════ -->
<div id="view-compare" class="view">
<div class="content"><div class="container">

  <div style="display:flex;gap:8px;align-items:center;margin-bottom:18px;flex-wrap:wrap">
    <select class="f-inp" style="width:180px" id="compareMetric" onchange="buildCompare()">
      <option value="power">Power (kW)</option>
      <option value="efficiency">Efficiency (%)</option>
      <option value="cost">Daily Cost (₹)</option>
      <option value="uptime">Uptime (%)</option>
      <option value="load">Load %</option>
    </select>
    <button class="btn btn-c btn-sm" onclick="exportCompareCSV()">↓ Export</button>
  </div>

  <div class="card mb">
    <div class="card-hd"><div class="card-title syne">Side-by-Side Comparison</div></div>
    <div style="overflow-x:auto"><table class="tbl" id="compareTbl"></table></div>
  </div>

  <div class="grid g2">
    <div class="card">
      <div class="card-hd"><div class="card-title syne">Power vs Rated</div></div>
      <div class="chart-box"><canvas id="cmpPwrChart"></canvas></div>
    </div>
    <div class="card">
      <div class="card-hd"><div class="card-title syne">Efficiency Ranking</div></div>
      <div class="chart-box"><canvas id="cmpEffChart"></canvas></div>
    </div>
  </div>

</div></div>
</div>

<!-- ════ LIVE SENSORS ════ -->
<div id="view-realtime" class="view">
<div class="content"><div class="container">

  <div style="display:flex;gap:8px;align-items:center;margin-bottom:18px;flex-wrap:wrap">
    <select class="f-inp" style="width:200px" id="rtFilter" onchange="renderSensorGrid()">
      <option value="all">All Machines</option>
    </select>
    <button class="btn btn-c btn-sm" id="simBtn" onclick="toggleSim()">⏸ Pause</button>
  </div>

  <div class="grid g3 mb" id="rtSensorGrid"></div>

  <div class="card">
    <div class="card-hd"><div class="card-title syne">Live Fleet Power Flow</div><span class="badge bc">60s window</span></div>
    <div class="chart-box tall"><canvas id="liveFleetChart"></canvas></div>
  </div>

</div></div>
</div>

<!-- ════ UPLOAD ════ -->
<div id="view-upload" class="view">
<div class="content"><div class="container">

  <div class="grid g2 mb">
    <div class="card">
      <div class="card-hd"><div class="card-title syne">Upload Data</div></div>
      <div class="fgrp">
        <label class="f-lbl">Assign to Machine</label>
        <select class="f-inp" id="uploadMachSel"></select>
      </div>
      <div class="fgrp">
        <label class="f-lbl">Data Type</label>
        <select class="f-inp" id="uploadDataType">
          <option>Energy Consumption</option>
          <option>Sensor Readings</option>
          <option>Equipment Data</option>
        </select>
      </div>
      <div class="upload-zone" id="uploadZone" onclick="document.getElementById('fileInput').click()">
        <div style="font-size:36px;margin-bottom:10px">📁</div>
        <div style="font-weight:600;margin-bottom:4px">Drop file or click to browse</div>
        <div style="font-size:12px;color:var(--txt3)">CSV · XLSX · XLS · JSON — max 10MB</div>
      </div>
      <input type="file" id="fileInput" accept=".csv,.xlsx,.xls,.json" multiple style="display:none" onchange="handleUpload(event)">
      <div id="uploadFeedback" style="margin-top:12px"></div>
    </div>
    <div class="card">
      <div class="card-hd"><div class="card-title syne">Templates</div></div>
      <p style="font-size:12.5px;color:var(--txt3);margin-bottom:16px">Download correctly formatted templates for bulk data import.</p>
      <div style="display:flex;flex-direction:column;gap:8px;margin-bottom:18px">
        <button class="btn btn-g" onclick="dlTemplate('equipment')">📋 Equipment Data Template</button>
        <button class="btn btn-g" onclick="dlTemplate('consumption')">⚡ Energy Consumption Template</button>
        <button class="btn btn-g" onclick="dlTemplate('sensors')">📡 Sensor Readings Template</button>
      </div>
      <div class="alert alert-i">ℹ️ Include a <code style="background:var(--bg3);padding:1px 5px;border-radius:4px;font-family:monospace">machine_id</code> column to auto-route records to the correct machine.</div>
      <div style="margin-top:14px">
        <div class="card-title syne" style="margin-bottom:10px">Upload Stats</div>
        <div class="grid g2" style="gap:8px">
          <div style="background:var(--bg3);border-radius:8px;padding:12px"><div class="s-lbl">Total Files</div><div class="s-val" id="up-total">0</div></div>
          <div style="background:var(--bg3);border-radius:8px;padding:12px"><div class="s-lbl">Total Records</div><div class="s-val" id="up-records">0</div></div>
        </div>
      </div>
    </div>
  </div>

  <div class="card">
    <div class="card-hd"><div class="card-title syne">Upload History</div><button class="btn btn-g btn-sm" onclick="clearUploadHistory()">Clear</button></div>
    <div style="overflow-x:auto">
      <table class="tbl">
        <thead><tr><th>File</th><th>Machine</th><th>Type</th><th>Records</th><th>Date</th><th>Status</th></tr></thead>
        <tbody id="uploadTbody"><tr><td colspan="6" style="text-align:center;color:var(--txt3);padding:30px">No uploads yet</td></tr></tbody>
      </table>
    </div>
  </div>

</div></div>
</div>

<!-- ════ ANALYTICS ════ -->
<div id="view-analytics" class="view">
<div class="content"><div class="container">

  <div style="display:flex;gap:8px;align-items:center;margin-bottom:18px;flex-wrap:wrap">
    <select class="f-inp" style="width:180px" id="analyticsPeriod" onchange="buildAnalytics()">
      <option value="7">Last 7 Days</option>
      <option value="30" selected>Last 30 Days</option>
      <option value="90">Last 90 Days</option>
    </select>
    <select class="f-inp" style="width:200px" id="analyticsMach" onchange="buildAnalytics()">
      <option value="all">All Machines</option>
    </select>
  </div>

  <!-- Summary stats -->
  <div class="grid g4 mb">
    <div class="stat c"><div class="stat-lbl">Avg Power</div><div class="stat-val" id="an-avgpwr" style="color:var(--cyan)">—</div><div class="stat-unit">kW</div></div>
    <div class="stat g"><div class="stat-lbl">Total Energy</div><div class="stat-val" id="an-energy" style="color:var(--grn)">—</div><div class="stat-unit">kWh</div></div>
    <div class="stat a"><div class="stat-lbl">Total Cost</div><div class="stat-val" id="an-cost" style="color:var(--amb)">—</div><div class="stat-unit">₹</div></div>
    <div class="stat p"><div class="stat-lbl">Best Machine</div><div class="stat-val" id="an-best" style="color:var(--pur);font-size:14px">—</div><div class="stat-unit">highest eff.</div></div>
  </div>

  <div class="grid g2 mb">
    <div class="card">
      <div class="card-hd"><div class="card-title syne">Consumption by Machine</div></div>
      <div class="chart-box"><canvas id="anConsChart"></canvas></div>
    </div>
    <div class="card">
      <div class="card-hd"><div class="card-title syne">Cost Breakdown</div></div>
      <div class="chart-box"><canvas id="anCostChart"></canvas></div>
    </div>
    <div class="card">
      <div class="card-hd"><div class="card-title syne">Efficiency Trends</div></div>
      <div class="chart-box"><canvas id="anEffChart"></canvas></div>
    </div>
    <div class="card">
      <div class="card-hd"><div class="card-title syne">Peak Demand (24h avg)</div></div>
      <div class="chart-box"><canvas id="anPeakChart"></canvas></div>
    </div>
  </div>

  <!-- Predictive -->
  <div class="card">
    <div class="card-hd"><div class="card-title syne">🔮 Predictive Insights</div><button class="btn btn-c btn-sm" onclick="askAIPrediction()">✨ Ask AI</button></div>
    <div class="grid g3">
      <div style="background:var(--bg3);border-radius:10px;padding:16px">
        <div class="s-lbl">Next Month Forecast</div>
        <div id="pred-forecast" style="font-family:monospace;font-size:22px;font-weight:600;color:var(--cyan);margin:6px 0">₹1,24,500</div>
        <div style="font-size:11px;color:var(--grn)">↓ 7.3% expected savings</div>
      </div>
      <div style="background:var(--bg3);border-radius:10px;padding:16px">
        <div class="s-lbl">Maintenance Due</div>
        <div id="pred-maint" style="font-family:monospace;font-size:22px;font-weight:600;color:var(--amb);margin:6px 0">—</div>
        <div style="font-size:11px;color:var(--amb)">machines need attention</div>
      </div>
      <div style="background:var(--bg3);border-radius:10px;padding:16px">
        <div class="s-lbl">Top Performer</div>
        <div id="pred-top" style="font-family:monospace;font-size:16px;font-weight:600;color:var(--grn);margin:6px 0">—</div>
        <div style="font-size:11px;color:var(--txt3)">highest efficiency this period</div>
      </div>
    </div>
  </div>

</div></div>
</div>

<!-- ════ AI SUGGESTIONS (full page) ════ -->
<div id="view-suggestions" class="view">
<div class="content"><div class="container">

  <div class="grid g2" style="gap:18px">
    <!-- Chat -->
    <div class="card" style="display:flex;flex-direction:column;min-height:600px">
      <div class="card-hd"><div class="card-title syne">🤖 AI Energy Advisor <span class="ai-badge">Claude AI</span></div></div>
      
      <div style="margin-bottom:12px">
        <div style="font-size:11px;color:var(--txt3);margin-bottom:8px;font-family:monospace">QUICK PROMPTS</div>
        <div class="suggestion-chips" id="pageChips"></div>
      </div>

      <div id="pageAiBody" style="flex:1;overflow-y:auto;min-height:300px;background:var(--bg3);border-radius:10px;padding:12px;margin-bottom:12px">
        <div class="ai-msg ai-msg-bot"><div class="bubble">👋 Hello! I'm your AI Energy Advisor powered by Claude.<br><br>I can analyze your fleet data and provide <strong>actionable insights</strong> to reduce costs, improve efficiency, and prevent equipment failures.<br><br>Ask me anything about your machines!</div></div>
      </div>

      <div style="display:flex;flex-direction:column;gap:8px">
        <textarea class="ai-textarea" id="pageAiInput" placeholder="e.g. Which machine is consuming the most energy? How can I reduce costs?" onkeydown="if(event.ctrlKey&&event.key==='Enter')sendPageAI()"></textarea>
        <div style="display:flex;gap:8px;align-items:center">
          <button class="ai-send-btn" id="pageAiBtn" onclick="sendPageAI()" style="flex:1">✨ Send (Ctrl+Enter)</button>
          <button class="btn btn-g btn-sm" onclick="clearPageAI()">Clear</button>
        </div>
      </div>
    </div>

    <!-- Quick insights panel -->
    <div style="display:flex;flex-direction:column;gap:18px">
      <div class="card">
        <div class="card-hd"><div class="card-title syne">⚡ Quick Actions</div></div>
        <div style="display:flex;flex-direction:column;gap:8px">
          <button class="btn btn-g" onclick="quickAI('What is my highest energy consuming machine and how can I reduce its consumption?')">🔍 Find biggest energy waster</button>
          <button class="btn btn-g" onclick="quickAI('Which machines need maintenance based on efficiency trends?')">🔧 Check maintenance needs</button>
          <button class="btn btn-g" onclick="quickAI('Generate a cost optimization plan for my fleet')">💰 Cost optimization plan</button>
          <button class="btn btn-g" onclick="quickAI('Compare my machine efficiencies and rank them')">📊 Efficiency ranking</button>
          <button class="btn btn-g" onclick="quickAI('What peak demand management strategies should I implement?')">⏰ Peak demand strategy</button>
          <button class="btn btn-g" onclick="quickAI('Forecast my energy costs for next month based on current usage')">🔮 Next month forecast</button>
        </div>
      </div>

      <div class="card">
        <div class="card-hd"><div class="card-title syne">📊 Fleet Summary for AI</div></div>
        <div id="aiFleetSummary" style="font-size:12px;color:var(--txt2);line-height:1.7;background:var(--bg3);border-radius:8px;padding:12px;font-family:monospace"></div>
      </div>

      <div class="card">
        <div class="card-hd"><div class="card-title syne">💡 Auto-Generated Tips</div></div>
        <div id="autoTips" style="display:flex;flex-direction:column;gap:8px"></div>
      </div>
    </div>
  </div>

</div></div>
</div>

<!-- ════ REPORTS ════ -->
<div id="view-reports" class="view">
<div class="content"><div class="container">

  <!-- Builder Card -->
  <div class="card mb">
    <div class="card-hd" style="flex-wrap:wrap;gap:10px;margin-bottom:18px">
      <div class="card-title syne" style="font-size:16px">📋 Report Builder</div>
    </div>

    <!-- Controls row -->
    <div style="display:flex;gap:10px;flex-wrap:wrap;margin-bottom:16px;align-items:flex-end">
      <div style="flex:1;min-width:160px">
        <div class="f-lbl">Machine Scope</div>
        <select class="f-inp" id="rpt-machine"><option value="all">🏭 All Machines</option></select>
      </div>
      <div style="flex:1;min-width:140px">
        <div class="f-lbl">Period</div>
        <select class="f-inp" id="rpt-period">
          <option value="today">Today</option>
          <option value="week">This Week</option>
          <option value="month" selected>This Month</option>
          <option value="quarter">This Quarter</option>
          <option value="year">This Year</option>
        </select>
      </div>
      <div style="flex:1;min-width:150px">
        <div class="f-lbl">Report Format</div>
        <select class="f-inp" id="rpt-format">
          <option value="html">HTML (Rich)</option>
          <option value="csv">CSV (Data)</option>
        </select>
      </div>
    </div>

    <!-- Report type selector cards -->
    <div class="f-lbl" style="margin-bottom:8px">Report Type</div>
    <div class="grid g5 mb" id="rptTypeCards" style="margin-bottom:16px">
      <div class="rpt-type-card active" data-type="fleet" onclick="selectRptType(this,'fleet')">
        <div class="rpt-type-icon">🏭</div>
        <div class="rpt-type-name">Fleet Summary</div>
        <div class="rpt-type-desc">All machines overview</div>
      </div>
      <div class="rpt-type-card" data-type="machine" onclick="selectRptType(this,'machine')">
        <div class="rpt-type-icon">⚙️</div>
        <div class="rpt-type-name">Machine Detail</div>
        <div class="rpt-type-desc">Per-machine deep dive</div>
      </div>
      <div class="rpt-type-card" data-type="compare" onclick="selectRptType(this,'compare')">
        <div class="rpt-type-icon">⚖️</div>
        <div class="rpt-type-name">Comparison</div>
        <div class="rpt-type-desc">Side-by-side benchmarks</div>
      </div>
      <div class="rpt-type-card" data-type="energy" onclick="selectRptType(this,'energy')">
        <div class="rpt-type-icon">⚡</div>
        <div class="rpt-type-name">Energy Audit</div>
        <div class="rpt-type-desc">Consumption & costs</div>
      </div>
      <div class="rpt-type-card" data-type="maintenance" onclick="selectRptType(this,'maintenance')">
        <div class="rpt-type-icon">🔧</div>
        <div class="rpt-type-name">Maintenance</div>
        <div class="rpt-type-desc">Alerts & schedule</div>
      </div>
    </div>

    <!-- Action buttons -->
    <div style="display:flex;gap:8px;flex-wrap:wrap;align-items:center">
      <button class="btn btn-c" onclick="previewReport()">👁 Preview Report</button>
      <button class="btn" style="background:rgba(0,230,118,.12);color:var(--grn);border:1px solid rgba(0,230,118,.25)" onclick="downloadReport('csv')">↓ Download CSV</button>
      <button class="btn" style="background:rgba(179,136,255,.1);color:var(--pur);border:1px solid rgba(179,136,255,.2)" onclick="downloadReport('html')">↓ Download HTML</button>
      <button class="btn btn-g" onclick="printReport()">🖨 Print / PDF</button>
    </div>
  </div>

  <!-- Live Preview Panel -->
  <div class="card mb" id="rptPreviewCard" style="display:none">
    <div class="card-hd" style="flex-wrap:wrap;gap:8px;margin-bottom:14px">
      <div class="card-title syne">📄 Report Preview</div>
      <div style="display:flex;gap:8px;flex-wrap:wrap">
        <button class="btn btn-g btn-sm" onclick="closePreview()">✕ Close</button>
        <button class="btn" style="background:rgba(0,230,118,.12);color:var(--grn);border:1px solid rgba(0,230,118,.25);font-size:11.5px;padding:5px 12px" onclick="downloadReport('csv')">↓ CSV</button>
        <button class="btn" style="background:rgba(179,136,255,.1);color:var(--pur);border:1px solid rgba(179,136,255,.2);font-size:11.5px;padding:5px 12px" onclick="downloadReport('html')">↓ HTML</button>
        <button class="btn btn-g btn-sm" onclick="printReport()">🖨 Print</button>
      </div>
    </div>
    <!-- Render preview as real HTML in a scrollable div -->
    <div id="rptPreviewInner" style="background:#fff;border-radius:8px;overflow:auto;max-height:70vh;border:1px solid #e2e8f0"></div>
  </div>

  <!-- Report History -->
  <div class="card">
    <div class="card-hd" style="flex-wrap:wrap;gap:8px">
      <div class="card-title syne">📁 Report History</div>
      <button class="btn btn-g btn-sm" onclick="clearReportHistory()">🗑 Clear All</button>
    </div>
    <div style="overflow-x:auto">
      <table class="tbl">
        <thead><tr><th>Report Name</th><th>Type</th><th>Machine</th><th>Period</th><th>Generated</th><th>Actions</th></tr></thead>
        <tbody id="reportTbody">
          <tr><td colspan="6" style="text-align:center;color:var(--txt3);padding:32px;font-size:13px">No reports generated yet — use the builder above</td></tr>
        </tbody>
      </table>
    </div>
  </div>

</div></div>
</div>

<!-- ════ SETTINGS ════ -->
<div id="view-settings" class="view">
<div class="content"><div class="container">
  <div class="grid g2">
    <div class="card">
      <div class="card-hd"><div class="card-title syne">Facility Settings</div></div>
      <div class="fgrp"><label class="f-lbl">Facility Name</label><input class="f-inp" id="cfg-facility" value="Sharma Steel Works"></div>
      <div class="fgrp"><label class="f-lbl">Tariff Rate (₹/kWh)</label><input class="f-inp" type="number" id="cfg-tariff" value="8.5" step="0.1"></div>
      <div class="fgrp"><label class="f-lbl">Currency</label><select class="f-inp" id="cfg-currency"><option value="INR">INR (₹)</option><option value="USD">USD ($)</option></select></div>
      <div class="fgrp"><label class="f-lbl">CO₂ Factor (kg/kWh)</label><input class="f-inp" type="number" id="cfg-co2" value="0.82" step="0.01"></div>
      <button class="btn btn-c" onclick="saveSettings()">💾 Save Settings</button>
    </div>
    <div class="card">
      <div class="card-hd"><div class="card-title syne">Data & Alerts</div></div>
      <div class="fgrp"><label class="f-lbl">Simulation Interval</label><select class="f-inp" id="cfg-interval"><option value="1000">1 second</option><option value="1500" selected>1.5 seconds</option><option value="3000">3 seconds</option><option value="5000">5 seconds</option></select></div>
      <div class="fgrp"><label class="f-lbl">Alert Power Threshold (kW)</label><input class="f-inp" type="number" id="cfg-threshold" value="80"></div>
      <div class="fgrp"><label class="f-lbl">Min Efficiency Alert (%)</label><input class="f-inp" type="number" id="cfg-mineff" value="70"></div>
      <button class="btn btn-c" onclick="saveSettings()">💾 Save Settings</button>
      <div style="margin-top:14px">
        <div class="card-title syne" style="margin-bottom:10px;font-size:13px">Registered Machines</div>
        <div id="machineSettingsList"></div>
      </div>
    </div>
  </div>
</div></div>
</div>

</div><!-- /main -->

<!-- ── AI SIDE PANEL ── -->
<div class="overlay" id="aiOverlay" onclick="toggleAIPanel()"></div>
<div class="ai-panel" id="aiPanel">
  <div class="ai-panel-hd">
    <div class="ai-panel-title">✨ AI Advisor <span class="ai-badge">Claude</span></div>
    <button class="btn btn-g btn-sm" onclick="toggleAIPanel()">✕</button>
  </div>
  <div class="ai-body" id="aiBody">
    <div class="ai-msg ai-msg-bot"><div class="bubble">👋 Hi! I'm your AI Energy Advisor.<br><br>Ask me anything about your fleet — I can help reduce costs, identify efficiency gaps, prioritise maintenance, and forecast usage.<br><br><strong>Tip:</strong> Use the quick chips below or type your question.</div></div>
    <div class="suggestion-chips" id="aiChips"></div>
  </div>
  <div class="ai-input-area">
    <textarea class="ai-textarea" id="aiInput" placeholder="Ask about your energy data…" onkeydown="if(event.ctrlKey&&event.key==='Enter')sendAI()"></textarea>
    <button class="ai-send-btn" id="aiSendBtn" onclick="sendAI()">✨ Send (Ctrl+Enter)</button>
  </div>
</div>

<!-- ── ADD MACHINE MODAL ── -->
<div class="modal" id="addModal">
  <div class="modal-box">
    <div class="modal-hd"><h3>Add Machine</h3><button class="btn btn-g btn-sm" onclick="closeModal('addModal')">✕</button></div>
    <div class="modal-bd">
      <div class="grid g2">
        <div class="fgrp"><label class="f-lbl">Machine Name *</label><input class="f-inp" id="nm-name" placeholder="e.g. CNC Mill #3"></div>
        <div class="fgrp"><label class="f-lbl">Machine ID *</label><input class="f-inp" id="nm-id" placeholder="e.g. CNC-03"></div>
        <div class="fgrp"><label class="f-lbl">Type</label><select class="f-inp" id="nm-type"><option>CNC Mill</option><option>Lathe</option><option>Press</option><option>Compressor</option><option>HVAC</option><option>Motor</option><option>Furnace</option><option>Pump</option><option>Other</option></select></div>
        <div class="fgrp"><label class="f-lbl">Rated Power (kW) *</label><input class="f-inp" type="number" id="nm-rated" placeholder="e.g. 22.5"></div>
        <div class="fgrp"><label class="f-lbl">Location</label><input class="f-inp" id="nm-location" placeholder="e.g. Bay A"></div>
        <div class="fgrp"><label class="f-lbl">Shift Hours/Day</label><input class="f-inp" type="number" id="nm-shift" placeholder="e.g. 16" value="16"></div>
      </div>
      <div class="fgrp"><label class="f-lbl">Initial Status</label>
        <select class="f-inp" id="nm-status"><option value="online">Online</option><option value="warning">Warning</option><option value="offline">Offline</option></select>
      </div>
    </div>
    <div class="modal-ft">
      <button class="btn btn-g" onclick="closeModal('addModal')">Cancel</button>
      <button class="btn btn-c" onclick="addMachine()">Add Machine</button>
    </div>
  </div>
</div>

<!-- ── EDIT MODAL ── -->
<div class="modal" id="editModal">
  <div class="modal-box">
    <div class="modal-hd"><h3>Edit Machine</h3><button class="btn btn-g btn-sm" onclick="closeModal('editModal')">✕</button></div>
    <div class="modal-bd">
      <div class="grid g2">
        <div class="fgrp"><label class="f-lbl">Machine Name</label><input class="f-inp" id="em-name"></div>
        <div class="fgrp"><label class="f-lbl">Machine ID</label><input class="f-inp" id="em-id" disabled style="opacity:.5"></div>
        <div class="fgrp"><label class="f-lbl">Type</label><select class="f-inp" id="em-type"><option>CNC Mill</option><option>Lathe</option><option>Press</option><option>Compressor</option><option>HVAC</option><option>Motor</option><option>Furnace</option><option>Pump</option><option>Other</option></select></div>
        <div class="fgrp"><label class="f-lbl">Rated Power (kW)</label><input class="f-inp" type="number" id="em-rated"></div>
        <div class="fgrp"><label class="f-lbl">Location</label><input class="f-inp" id="em-location"></div>
        <div class="fgrp"><label class="f-lbl">Shift Hours/Day</label><input class="f-inp" type="number" id="em-shift"></div>
      </div>
      <div class="fgrp"><label class="f-lbl">Status</label>
        <select class="f-inp" id="em-status"><option value="online">Online</option><option value="warning">Warning</option><option value="offline">Offline</option></select>
      </div>
    </div>
    <div class="modal-ft">
      <button class="btn btn-g" onclick="closeModal('editModal')">Cancel</button>
      <button class="btn btn-c" onclick="saveMachineEdit()">Save Changes</button>
    </div>
  </div>
</div>

<!-- ── REMOVE MODAL ── -->
<div class="modal" id="removeModal">
  <div class="modal-box">
    <div class="modal-hd"><h3>Remove Machine</h3><button class="btn btn-g btn-sm" onclick="closeModal('removeModal')">✕</button></div>
    <div class="modal-bd">
      <div class="alert alert-r">⚠️ This will permanently remove the machine and all associated data.</div>
      <p id="removeText" style="font-size:13px;color:var(--txt2);margin-top:8px"></p>
    </div>
    <div class="modal-ft">
      <button class="btn btn-g" onclick="closeModal('removeModal')">Cancel</button>
      <button class="btn btn-r" onclick="removeMachine()">Remove</button>
    </div>
  </div>
</div>

<script>
// ══════════════════════════════════════════════
// DATA
// ══════════════════════════════════════════════
const TARIFF = 8.5, CO2F = 0.82;
const MC = ['#00d4ff','#b388ff','#00e676','#ffab00','#ff4444','#40c4ff','#ea80fc','#69f0ae','#ffd740','#ff6e40'];
const machineColor = i => MC[i % MC.length];

let machines = [
  {id:'CNC-01',name:'CNC Mill #1',    type:'CNC Mill',   location:'Bay A', rated:22.5,shift:16,status:'online',  added:'2026-01-10'},
  {id:'CNC-02',name:'CNC Mill #2',    type:'CNC Mill',   location:'Bay A', rated:18.5,shift:16,status:'online',  added:'2026-01-10'},
  {id:'LAT-01',name:'Lathe #1',       type:'Lathe',      location:'Bay B', rated:11.0,shift:12,status:'warning', added:'2026-01-15'},
  {id:'PRS-01',name:'Hydraulic Press',type:'Press',      location:'Bay C', rated:37.0,shift:8, status:'online',  added:'2026-01-20'},
  {id:'CMP-01',name:'Compressor A',   type:'Compressor', location:'Utility',rated:15.0,shift:24,status:'online', added:'2026-02-01'},
  {id:'HVAC-1',name:'HVAC Unit 1',    type:'HVAC',       location:'Roof',  rated:12.0,shift:24,status:'online',  added:'2026-02-10'},
  {id:'FRN-01',name:'Furnace #1',     type:'Furnace',    location:'Bay D', rated:55.0,shift:8, status:'offline', added:'2026-02-15'},
  {id:'MOT-01',name:'Conveyor Motor', type:'Motor',      location:'Floor', rated:7.5, shift:16,status:'online',  added:'2026-02-20'},
];

let live = {};
machines.forEach(m => {
  live[m.id] = {
    power: m.status==='offline' ? 0 : m.rated*(0.5+Math.random()*.4),
    eff: m.status==='warning' ? 58+Math.random()*15 : 80+Math.random()*15,
    temp: 38+Math.random()*40,
    volt: 410+Math.random()*10,
    pf: 0.85+Math.random()*.1,
    history: Array(24).fill(0).map(()=>m.status==='offline'?0:m.rated*(0.35+Math.random()*.55)),
    liveHistory: Array(60).fill(m.status==='offline'?0:m.rated*(0.45+Math.random()*.45)),
  };
  live[m.id].curr = ((live[m.id].power*1000)/(live[m.id].volt*1.732*live[m.id].pf)).toFixed(1);
});

let selectedId = machines[0].id;
let uploadHistory = [];
let simRunning = true, simTimer = null;
let charts = {}, liveChartInst = null;
let currentView = 'fleet';
let aiPanelOpen = false;
let settings = {facility:'Sharma Steel Works', tariff:8.5, co2f:0.82, interval:1500, threshold:80, mineff:70};

// ══════════════════════════════════════════════
// HELPERS
// ══════════════════════════════════════════════
const totalPwr = () => machines.reduce((s,m)=>s+live[m.id].power,0);
const onlineCnt = () => machines.filter(m=>m.status==='online').length;
const avgEff = () => (machines.reduce((s,m)=>s+live[m.id].eff,0)/machines.length).toFixed(1);
const fmt = n => parseFloat(n).toFixed(1);
const fmtINR = n => '₹'+Math.round(n).toLocaleString('en-IN');
const destroyChart = key => { if(charts[key]){charts[key].destroy();delete charts[key];} };

function chartDefaults(ctx, type, data, opts={}) {
  destroyChart(ctx.canvas.id);
  charts[ctx.canvas.id] = new Chart(ctx, {type, data, options:{
    responsive:true, maintainAspectRatio:false,
    interaction:{mode:'index',intersect:false},
    plugins:{legend:{labels:{color:'#8fa3be',boxWidth:10,font:{size:10}}}},
    scales:{
      x:{ticks:{color:'#4a6080',font:{size:10}},grid:{color:'rgba(255,255,255,0.03)'}},
      y:{ticks:{color:'#4a6080',font:{size:10}},grid:{color:'rgba(255,255,255,0.04)'}}
    },
    ...opts
  }});
}

// ══════════════════════════════════════════════
// NAVIGATION
// ══════════════════════════════════════════════
function nav(view, el) {
  document.querySelectorAll('.nav-item').forEach(n=>n.classList.remove('active'));
  if(el) el.classList.add('active');
  document.querySelectorAll('.view').forEach(v=>v.classList.remove('active'));
  document.getElementById('view-'+view).classList.add('active');
  currentView = view;
  closeSidebar();

  const titles = {fleet:'Fleet Overview',machine:'Machine Detail',compare:'Compare Machines',realtime:'Live Sensors',upload:'Data Upload',analytics:'Analytics',suggestions:'AI Suggestions',reports:'Reports',settings:'Settings'};
  document.getElementById('topTitle').textContent = titles[view]||view;

  if(view==='fleet') buildFleet();
  else if(view==='machine') buildMachineDetail(selectedId);
  else if(view==='compare') buildCompare();
  else if(view==='realtime') { buildSensorGrid(); buildLiveChart(); }
  else if(view==='upload') { populateUploadSelect(); }
  else if(view==='analytics') buildAnalytics();
  else if(view==='suggestions') buildSuggestionsPage();
  else if(view==='reports') { populateRptMachineSelect(); buildReportsTable(); }
  else if(view==='settings') buildSettingsPage();
}

// ══════════════════════════════════════════════
// SIDEBAR
// ══════════════════════════════════════════════
function buildSidebar() {
  const el = document.getElementById('sbMachineList');
  const tp = totalPwr();
  el.innerHTML = `
    <button class="mc-btn all-btn ${selectedId==='all'?'active':''}" onclick="selectMachine('all')">
      <span class="mc-dot all"></span><span>All Machines</span>
      <span class="mc-kw">${fmt(tp)}kW</span>
    </button>` +
    machines.map(m=>`
    <button class="mc-btn ${selectedId===m.id?'active':''}" onclick="selectMachine('${m.id}')">
      <span class="mc-dot ${m.status}"></span>
      <span style="flex:1;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;text-align:left">${m.name}</span>
      <span class="mc-kw">${m.status==='offline'?'OFF':fmt(live[m.id].power)+'kW'}</span>
    </button>`).join('');
}

function selectMachine(id) {
  selectedId = id;
  buildSidebar();
  if(id==='all') nav('fleet', document.querySelector('[data-view=fleet]'));
  else nav('machine', document.querySelector('[data-view=machine]'));
}

function toggleSidebar() {
  document.getElementById('sidebar').classList.toggle('open');
  document.getElementById('sbOverlay').classList.toggle('show');
}
function closeSidebar() {
  document.getElementById('sidebar').classList.remove('open');
  document.getElementById('sbOverlay').classList.remove('show');
}

// ══════════════════════════════════════════════
// FLEET VIEW
// ══════════════════════════════════════════════
function buildFleet() {
  const tp = totalPwr();
  const cost = tp*24*30*settings.tariff;
  document.getElementById('kpi-power').textContent = fmt(tp);
  document.getElementById('kpi-online').textContent = onlineCnt();
  document.getElementById('kpi-onlinesub').textContent = `of ${machines.length} machines`;
  document.getElementById('kpi-cost').textContent = fmtINR(cost);
  document.getElementById('kpi-cdelta').innerHTML = '<span class="dn">↑ 3.2% from last month</span>';
  document.getElementById('kpi-eff').textContent = avgEff();
  document.getElementById('kpi-co2').textContent = Math.round(tp*24*30*settings.co2f).toLocaleString('en-IN');
  document.getElementById('topSub').textContent = `${machines.length} machines · ${onlineCnt()} online · ${fmt(tp)} kW total`;

  buildFleetGrid('all');
  buildFleetPowerChart();
  buildFleetDistChart();
  buildFleetAlerts();
}

function buildFleetGrid(filter) {
  const list = filter==='all' ? machines : machines.filter(m=>m.status===filter);
  document.getElementById('fleetGrid').innerHTML = list.map((m,i)=>{
    const d=live[m.id], pct=Math.min(100,Math.round(d.power/m.rated*100));
    const col=machineColor(machines.indexOf(m));
    const daily = Math.round(d.power*m.shift*settings.tariff);
    return `<div class="mcard ${selectedId===m.id?'selected':''}" onclick="selectMachine('${m.id}')">
      <div class="mcard-glow" style="background:radial-gradient(circle at 50% 0%,${col}18,transparent 70%)"></div>
      <div class="mcard-hd">
        <div><div class="mcard-name">${m.name}</div><div class="mcard-id">${m.id} · ${m.type} · ${m.location}</div></div>
        <span class="status-pill ${m.status}">${m.status==='online'?'●':m.status==='warning'?'●':'✕'} ${m.status.toUpperCase()}</span>
      </div>
      <div class="mcard-pwr" style="color:${m.status==='offline'?'var(--txt3)':col}">${m.status==='offline'?'OFFLINE':fmt(d.power)}</div>
      <div class="mcard-unit">${m.status==='offline'?'Powered down':'kW · '+pct+'% of rated · '+fmtINR(daily)+'/day'}</div>
      <div class="bar-bg"><div class="bar-fill" style="width:${m.status==='offline'?0:pct}%;background:${m.status==='online'?col:m.status==='warning'?'var(--amb)':'var(--red)'}"></div></div>
      <div class="mcard-meta">
        <span>Eff ${fmt(d.eff)}%</span><span>Rated ${m.rated}kW</span><span>Shift ${m.shift}h</span><span>Temp ${fmt(d.temp)}°C</span>
      </div>
    </div>`;
  }).join('');
}

function filterCards(f) { buildFleetGrid(f); }

function buildFleetPowerChart() {
  const ctx = document.getElementById('fleetPowerChart').getContext('2d');
  const hrs = Array.from({length:24},(_,i)=>`${i}:00`);
  chartDefaults(ctx,'line',{
    labels:hrs,
    datasets:machines.slice(0,6).map((m,i)=>({
      label:m.name, data:live[m.id].history, borderColor:machineColor(i),
      backgroundColor:machineColor(i)+'15', tension:.4, fill:false, pointRadius:0, borderWidth:2
    }))
  });
}

function buildFleetDistChart() {
  const ctx = document.getElementById('fleetDistChart').getContext('2d');
  destroyChart('fleetDistChart');
  charts['fleetDistChart'] = new Chart(ctx,{type:'doughnut',data:{
    labels:machines.map(m=>m.name),
    datasets:[{data:machines.map(m=>live[m.id].power.toFixed(1)),backgroundColor:machines.map((_,i)=>machineColor(i)),borderWidth:0}]
  },options:{responsive:true,maintainAspectRatio:false,cutout:'60%',plugins:{legend:{position:'right',labels:{color:'#8fa3be',boxWidth:10,font:{size:10}}}}}});
}

function buildFleetAlerts() {
  const alerts = [];
  machines.forEach(m=>{
    const d=live[m.id];
    if(m.status==='offline') alerts.push({t:'r',msg:`<strong>${m.name}</strong> (${m.id}) is offline — check power supply and circuit breaker.`});
    if(m.status==='warning') alerts.push({t:'w',msg:`<strong>${m.name}</strong> efficiency degraded to ${fmt(d.eff)}% — maintenance recommended.`});
    if(d.temp>75) alerts.push({t:'w',msg:`<strong>${m.name}</strong> temperature high at ${fmt(d.temp)}°C — inspect cooling system.`});
  });
  if(!alerts.length) alerts.push({t:'g',msg:'✓ All systems operating within normal parameters.'});
  document.getElementById('fleetAlerts').innerHTML = alerts.map(a=>`<div class="alert alert-${a.t}${a.t==='r'?'':a.t==='w'?'w':a.t==='g'?'g':'i'}">${a.t==='r'?'🔴':a.t==='w'?'⚠️':'✓'} ${a.msg}</div>`).join('');
}

// ══════════════════════════════════════════════
// MACHINE DETAIL
// ══════════════════════════════════════════════
function buildMachineDetail(id) {
  const m = machines.find(x=>x.id===id);
  if(!m){nav('fleet',document.querySelector('[data-view=fleet]'));return;}
  const d = live[id];
  document.getElementById('topTitle').textContent = m.name;
  document.getElementById('topSub').textContent = `${m.id} · ${m.type} · ${m.location} · ${m.status.toUpperCase()}`;

  document.getElementById('md-pwr').textContent = m.status==='offline'?'0.0':fmt(d.power);
  document.getElementById('md-kwh').textContent = Math.round(d.power*m.shift*.92);
  document.getElementById('md-hrs').textContent = m.status==='offline'?0:m.shift;
  document.getElementById('md-eff').textContent = fmt(d.eff);

  const sensors = [
    {l:'POWER',v:m.status==='offline'?'OFF':fmt(d.power),u:'kW',c:'var(--cyan)'},
    {l:'VOLTAGE',v:m.status==='offline'?'—':fmt(d.volt),u:'V',c:'var(--pur)'},
    {l:'CURRENT',v:m.status==='offline'?'—':d.curr,u:'A',c:'var(--amb)'},
    {l:'POWER FACTOR',v:m.status==='offline'?'—':d.pf.toFixed(3),u:'',c:'#40c4ff'},
    {l:'TEMPERATURE',v:m.status==='offline'?'—':fmt(d.temp),u:'°C',c:d.temp>75?'var(--red)':'var(--grn)'},
    {l:'EFFICIENCY',v:fmt(d.eff),u:'%',c:d.eff<70?'var(--red)':d.eff<80?'var(--amb)':'var(--grn)'},
  ];
  document.getElementById('mdSensors').innerHTML = sensors.map(s=>`<div class="sensor-item"><div class="s-lbl">${s.l}</div><div class="s-val" style="color:${s.c}">${s.v}</div><div class="s-unit">${s.u}</div></div>`).join('');

  const mdaily = Math.round(d.power*m.shift*settings.tariff);
  document.getElementById('mdInfo').innerHTML = `<div style="display:grid;grid-template-columns:1fr 1fr;gap:0">
    ${[['Machine ID',m.id],['Type',m.type],['Location',m.location],['Rated Power',m.rated+' kW'],['Shift Hours',m.shift+' h/day'],['Status',m.status.toUpperCase()],['Added',m.added],['Daily Cost',fmtINR(mdaily)],['Monthly Cost',fmtINR(mdaily*30)]]
    .map(([k,v])=>`<div style="padding:8px 0;border-bottom:1px solid var(--bdr)"><div style="font-size:9px;font-family:monospace;color:var(--txt3);margin-bottom:2px">${k}</div><div style="font-size:12.5px;font-weight:600">${v}</div></div>`).join('')}
  </div>`;

  // Power chart
  const ctx = document.getElementById('mdPowerChart').getContext('2d');
  const idx = machines.indexOf(m), col=machineColor(idx);
  chartDefaults(ctx,'line',{
    labels:Array.from({length:24},(_,i)=>`${i}:00`),
    datasets:[{label:'Power (kW)',data:d.history,borderColor:col,backgroundColor:col+'18',tension:.4,fill:true,pointRadius:0,borderWidth:2}]
  },{plugins:{legend:{display:false}}});

  // Alerts
  const alts=[];
  if(m.status==='offline') alts.push({t:'r',msg:'Machine offline — check power and circuit breaker.'});
  if(m.status==='warning') alts.push({t:'w',msg:`Efficiency at ${fmt(d.eff)}% — below optimal. Schedule maintenance.`});
  if(d.temp>75) alts.push({t:'w',msg:`High temperature: ${fmt(d.temp)}°C — check cooling system.`});
  if(d.pf<0.88) alts.push({t:'w',msg:`Low power factor: ${d.pf.toFixed(3)} — consider capacitor bank.`});
  if(d.eff>=85) alts.push({t:'g',msg:'Operating efficiently within optimal parameters.'});
  document.getElementById('mdAlerts').innerHTML = alts.map(a=>`<div class="alert alert-${a.t}">${a.t==='r'?'🔴':a.t==='w'?'⚠️':'✓'} ${a.msg}</div>`).join('');
}

// ══════════════════════════════════════════════
// COMPARE
// ══════════════════════════════════════════════
function buildCompare() {
  const maxP = Math.max(...machines.map(m=>live[m.id].power));
  document.getElementById('compareTbl').innerHTML = `<thead><tr><th>Machine</th><th>ID</th><th>Status</th><th>Power (kW)</th><th>Efficiency</th><th>Daily Cost</th><th>Uptime</th><th>Load %</th></tr></thead>
  <tbody>${machines.map((m,i)=>{
    const d=live[m.id], pct=Math.min(100,Math.round(d.power/m.rated*100));
    const daily=Math.round(d.power*m.shift*settings.tariff);
    const uptime=m.status==='offline'?0:m.status==='warning'?65+Math.floor(Math.random()*15):90+Math.floor(Math.random()*9);
    const col=machineColor(i);
    return `<tr>
      <td><strong>${m.name}</strong></td>
      <td><span class="badge bc mono">${m.id}</span></td>
      <td><span class="status-pill ${m.status}" style="display:inline-flex">${m.status}</span></td>
      <td><div class="cmp-bar-wrap"><div class="cmp-bar-bg"><div class="cmp-bar-fill" style="width:${m.status==='offline'?0:Math.round(d.power/maxP*100)}%;background:${col};height:100%;border-radius:3px"></div></div><span class="mono" style="font-size:11px;min-width:40px">${fmt(d.power)}</span></div></td>
      <td><div class="cmp-bar-wrap"><div class="cmp-bar-bg"><div class="cmp-bar-fill" style="width:${d.eff}%;background:${d.eff>80?'var(--grn)':'var(--amb)'};height:100%;border-radius:3px"></div></div><span class="mono" style="font-size:11px;min-width:40px">${fmt(d.eff)}%</span></div></td>
      <td class="mono">${fmtINR(daily)}</td>
      <td><div class="cmp-bar-wrap"><div class="cmp-bar-bg"><div class="cmp-bar-fill" style="width:${uptime}%;background:${uptime>80?'var(--grn)':'var(--red)'};height:100%;border-radius:3px"></div></div><span class="mono" style="font-size:11px">${uptime}%</span></div></td>
      <td class="mono">${pct}%</td>
    </tr>`;
  }).join('')}</tbody>`;

  // Charts
  const ctx1=document.getElementById('cmpPwrChart').getContext('2d');
  chartDefaults(ctx1,'bar',{
    labels:machines.map(m=>m.id),
    datasets:[
      {label:'Current (kW)',data:machines.map(m=>live[m.id].power.toFixed(1)),backgroundColor:machines.map((_,i)=>machineColor(i)+'cc'),borderRadius:5},
      {label:'Rated (kW)',data:machines.map(m=>m.rated),backgroundColor:'rgba(255,255,255,0.05)',borderColor:'rgba(255,255,255,0.12)',borderWidth:1,borderRadius:5}
    ]
  },{plugins:{legend:{labels:{color:'#8fa3be',boxWidth:10,font:{size:10}}}}});

  const sorted=[...machines].sort((a,b)=>live[b.id].eff-live[a.id].eff);
  const ctx2=document.getElementById('cmpEffChart').getContext('2d');
  chartDefaults(ctx2,'bar',{
    labels:sorted.map(m=>m.id),
    datasets:[{label:'Efficiency (%)',data:sorted.map(m=>live[m.id].eff.toFixed(1)),
      backgroundColor:sorted.map(m=>live[m.id].eff>80?'rgba(0,230,118,.7)':'rgba(255,171,0,.7)'),borderRadius:5,indexAxis:'y'}]
  },{indexAxis:'y',plugins:{legend:{display:false}},scales:{
    x:{min:0,max:100,ticks:{color:'#4a6080'},grid:{color:'rgba(255,255,255,0.04)'}},
    y:{ticks:{color:'#4a6080'},grid:{display:false}}
  }});
}

function exportCompareCSV() {
  const rows=[['Machine','ID','Status','Power kW','Rated kW','Efficiency %','Daily Cost INR','Shift h']];
  machines.forEach(m=>{
    const d=live[m.id];
    rows.push([m.name,m.id,m.status,fmt(d.power),m.rated,fmt(d.eff),Math.round(d.power*m.shift*settings.tariff),m.shift]);
  });
  const csv=rows.map(r=>r.join(',')).join('\n');
  dlFile(csv,'energiscan_compare.csv','text/csv');
}

// ══════════════════════════════════════════════
// LIVE SENSORS
// ══════════════════════════════════════════════
function buildSensorGrid() {
  const sel=document.getElementById('rtFilter');
  if(sel.children.length<=1) machines.forEach(m=>{const o=document.createElement('option');o.value=m.id;o.textContent=m.name;sel.appendChild(o);});
  renderSensorGrid();
}

function renderSensorGrid() {
  const filter=document.getElementById('rtFilter')?.value||'all';
  const list=filter==='all'?machines:machines.filter(m=>m.id===filter);
  document.getElementById('rtSensorGrid').innerHTML=list.map((m)=>{
    const d=live[m.id], col=machineColor(machines.indexOf(m));
    return `<div class="card" style="border-color:${col}22">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:12px">
        <div><div style="font-weight:700;font-size:13px">${m.name}</div><div class="mono" style="font-size:9px;color:var(--txt3)">${m.id}</div></div>
        <span class="status-pill ${m.status}" style="display:inline-flex">${m.status}</span>
      </div>
      <div class="grid g2" style="gap:8px">
        <div class="sensor-item"><div class="s-lbl">POWER</div><div id="rt-${m.id}-p" class="s-val" style="color:${col}">${m.status==='offline'?'OFF':fmt(d.power)}</div><div class="s-unit">kW</div></div>
        <div class="sensor-item"><div class="s-lbl">TEMP</div><div id="rt-${m.id}-t" class="s-val" style="color:${d.temp>75?'var(--red)':'var(--grn)'}">${m.status==='offline'?'—':fmt(d.temp)}</div><div class="s-unit">°C</div></div>
        <div class="sensor-item"><div class="s-lbl">CURRENT</div><div id="rt-${m.id}-c" class="s-val" style="color:var(--amb)">${m.status==='offline'?'—':d.curr}</div><div class="s-unit">A</div></div>
        <div class="sensor-item"><div class="s-lbl">EFFICIENCY</div><div id="rt-${m.id}-e" class="s-val" style="color:var(--pur)">${fmt(d.eff)}</div><div class="s-unit">%</div></div>
      </div>
    </div>`;
  }).join('');
}

function buildLiveChart() {
  const ctx=document.getElementById('liveFleetChart').getContext('2d');
  if(liveChartInst){liveChartInst.destroy();}
  liveChartInst=new Chart(ctx,{type:'line',data:{
    labels:Array(60).fill(''),
    datasets:machines.slice(0,6).map((m,i)=>({
      label:m.name,data:[...live[m.id].liveHistory],borderColor:machineColor(i),tension:.4,fill:false,pointRadius:0,borderWidth:2
    }))
  },options:{responsive:true,maintainAspectRatio:false,animation:false,
    interaction:{mode:'index',intersect:false},
    plugins:{legend:{labels:{color:'#8fa3be',boxWidth:10,font:{size:10}}}},
    scales:{x:{display:false},y:{ticks:{color:'#4a6080'},grid:{color:'rgba(255,255,255,0.04)'},title:{display:true,text:'kW',color:'#4a6080'}}}
  }});
}

// ══════════════════════════════════════════════
// ANALYTICS
// ══════════════════════════════════════════════
function buildAnalytics() {
  const period=parseInt(document.getElementById('analyticsPeriod')?.value||30);
  const machSel=document.getElementById('analyticsMach');
  if(machSel&&machSel.children.length<=1) machines.forEach(m=>{const o=document.createElement('option');o.value=m.id;o.textContent=m.name;machSel.appendChild(o);});
  const selM=machSel?.value||'all';
  const mList=selM==='all'?machines:machines.filter(m=>m.id===selM);

  const avgP=(mList.reduce((s,m)=>s+live[m.id].power,0)/mList.length).toFixed(1);
  const totalEnergy=Math.round(mList.reduce((s,m)=>s+live[m.id].power*m.shift,0)*period/30*30);
  const totalCost=Math.round(totalEnergy*settings.tariff);
  const best=mList.reduce((a,b)=>live[a.id].eff>live[b.id].eff?a:b);

  document.getElementById('an-avgpwr').textContent=avgP;
  document.getElementById('an-energy').textContent=totalEnergy.toLocaleString();
  document.getElementById('an-cost').textContent=fmtINR(totalCost);
  document.getElementById('an-best').textContent=best.name;
  document.getElementById('pred-maint').textContent=machines.filter(m=>m.status==='warning'||m.status==='offline').length;
  document.getElementById('pred-top').textContent=best.name;

  const weeks=period<=7?['Mon','Tue','Wed','Thu','Fri','Sat','Sun']:['Wk 1','Wk 2','Wk 3','Wk 4'];
  const hrs=Array.from({length:24},(_,i)=>`${i}:00`);

  // Consumption stacked bar
  const ctx1=document.getElementById('anConsChart').getContext('2d');
  chartDefaults(ctx1,'bar',{labels:weeks,datasets:mList.slice(0,5).map((m,i)=>({
    label:m.name,data:weeks.map(()=>m.rated*(7+Math.random()*5)*m.shift/24),
    backgroundColor:machineColor(i)+'aa',borderRadius:4,stack:'s'
  }))},{scales:{x:{stacked:true,ticks:{color:'#4a6080'},grid:{display:false}},y:{stacked:true,ticks:{color:'#4a6080'},grid:{color:'rgba(255,255,255,0.04)'}}}});

  // Cost doughnut
  const ctx2=document.getElementById('anCostChart').getContext('2d');
  destroyChart('anCostChart');
  charts['anCostChart']=new Chart(ctx2,{type:'doughnut',data:{labels:mList.map(m=>m.name),datasets:[{data:mList.map(m=>Math.round(live[m.id].power*m.shift*30*settings.tariff)),backgroundColor:mList.map((_,i)=>machineColor(i)),borderWidth:0}]},options:{responsive:true,maintainAspectRatio:false,cutout:'60%',plugins:{legend:{position:'right',labels:{color:'#8fa3be',boxWidth:10,font:{size:10}}}}}});

  // Efficiency trend
  const ctx3=document.getElementById('anEffChart').getContext('2d');
  chartDefaults(ctx3,'line',{labels:weeks,datasets:mList.slice(0,5).map((m,i)=>({
    label:m.name,data:weeks.map(()=>live[m.id].eff*(0.9+Math.random()*.2)),
    borderColor:machineColor(i),tension:.4,pointRadius:3,borderWidth:2,fill:false
  }))},{scales:{x:{ticks:{color:'#4a6080'},grid:{color:'rgba(255,255,255,0.03)'}},y:{min:40,max:100,ticks:{color:'#4a6080'},grid:{color:'rgba(255,255,255,0.04)'}}}});

  // Peak demand bar
  const ctx4=document.getElementById('anPeakChart').getContext('2d');
  const tp2=totalPwr();
  chartDefaults(ctx4,'bar',{labels:hrs,datasets:[{label:'Fleet kW',data:hrs.map((_,i)=>i>=8&&i<=18?tp2*(0.65+Math.random()*.3):tp2*(0.25+Math.random()*.2)),
    backgroundColor:hrs.map((_,i)=>i>=8&&i<=18?'rgba(0,212,255,.65)':'rgba(0,212,255,.2)'),borderRadius:3}]},{plugins:{legend:{display:false}}});
}

// ══════════════════════════════════════════════
// AI SUGGESTIONS PAGE
// ══════════════════════════════════════════════
const quickChips = [
  'Reduce energy cost','Maintenance priorities','Efficiency improvements','Peak demand tips','ROI of upgrades','CO₂ reduction plan'
];

function buildSuggestionsPage() {
  // Chips
  document.getElementById('pageChips').innerHTML = quickChips.map(c=>`<span class="chip" onclick="quickAI('${c}')">${c}</span>`).join('');
  buildFleetSummary();
  buildAutoTips();
}

function buildFleetSummary() {
  const tp=totalPwr();
  const summary = machines.map(m=>`• ${m.name} (${m.id}): ${fmt(live[m.id].power)}kW / Eff:${fmt(live[m.id].eff)}% / Status:${m.status}`).join('\n');
  const el=document.getElementById('aiFleetSummary');
  if(el) el.textContent=`Facility: ${settings.facility}\nTotal Fleet Power: ${fmt(tp)} kW\nMachines: ${machines.length} (${onlineCnt()} online)\nFleet Efficiency: ${avgEff()}%\nMonthly Cost: ${fmtINR(tp*24*30*settings.tariff)}\n\n${summary}`;
}

function buildAutoTips() {
  const tips=[];
  const worstEff=machines.filter(m=>live[m.id].eff<75);
  const highTemp=machines.filter(m=>live[m.id].temp>75);
  const offline=machines.filter(m=>m.status==='offline');
  const lowPF=machines.filter(m=>live[m.id].pf<0.88&&m.status!=='offline');

  if(offline.length) tips.push({icon:'🔴',title:'Offline Machines',msg:`${offline.map(m=>m.name).join(', ')} are offline. Investigate immediately to prevent production loss.`});
  if(worstEff.length) tips.push({icon:'⚠️',title:'Low Efficiency Alert',msg:`${worstEff.map(m=>m.name).join(', ')} below 75% efficiency. Schedule predictive maintenance.`});
  if(highTemp.length) tips.push({icon:'🌡️',title:'Temperature Warning',msg:`${highTemp.map(m=>m.name).join(', ')} running hot. Check lubrication and cooling systems.`});
  if(lowPF.length) tips.push({icon:'⚡',title:'Power Factor Issue',msg:`${lowPF.map(m=>m.name).join(', ')} have low power factor (<0.88). Installing capacitor banks could reduce demand charges.`});
  tips.push({icon:'💡',title:'Peak Demand Tip',msg:'Consider staggering machine startup times to avoid simultaneous peaks (2–4 PM window). Could reduce demand charges by 10–15%.'});
  tips.push({icon:'💰',title:'Optimization Opportunity',msg:`Running at ${avgEff()}% fleet efficiency. Targeting 92% efficiency could save ${fmtINR(totalPwr()*24*30*settings.tariff*0.08)} monthly.`});

  const el=document.getElementById('autoTips');
  if(el) el.innerHTML=tips.map(t=>`<div class="alert alert-i" style="flex-direction:column;align-items:flex-start"><div style="font-weight:700;margin-bottom:4px">${t.icon} ${t.title}</div><div style="font-size:12px">${t.msg}</div></div>`).join('');
}

function getFleetContext() {
  const tp=totalPwr();
  return `You are an AI energy management advisor. Here is the current fleet data:
Facility: ${settings.facility}
Total Fleet Power: ${fmt(tp)} kW
Machines: ${machines.length} total, ${onlineCnt()} online
Fleet Efficiency: ${avgEff()}%
Monthly Cost Estimate: ${fmtINR(tp*24*30*settings.tariff)}
Tariff: ₹${settings.tariff}/kWh

Machine Details:
${machines.map(m=>`- ${m.name} (${m.id}): Type=${m.type}, Power=${fmt(live[m.id].power)}kW, Rated=${m.rated}kW, Eff=${fmt(live[m.id].eff)}%, Temp=${fmt(live[m.id].temp)}°C, PF=${live[m.id].pf.toFixed(2)}, Status=${m.status}, Shift=${m.shift}h/day, Location=${m.location}`).join('\n')}

Provide specific, actionable energy management recommendations based on this data. Be concise, use numbers and estimates where possible.`;
}

async function callClaude(prompt, systemContext) {
  if (!window.claude) throw new Error('AI unavailable — open in claude.ai');
  const system = systemContext || getFleetContext();
  let fullText = '';
  const stream = window.claude.streamText({ prompt, system });
  for await (const chunk of stream) { fullText += chunk; }
  return fullText;
}

// Side panel AI — streaming
async function sendAI() {
  const inp=document.getElementById('aiInput');
  const msg=inp.value.trim();
  if(!msg) return;
  const btn=document.getElementById('aiSendBtn');
  btn.disabled=true; inp.value='';
  appendAIMsg('aiBody',msg,'user');
  const bubble=appendStreamBubble('aiBody');
  try {
    if(!window.claude) throw new Error('Open in claude.ai to use AI features');
    const stream=window.claude.streamText({prompt:msg, system:getFleetContext()});
    let text='';
    for await(const chunk of stream){ text+=chunk; bubble.innerHTML=formatAIText(text); bubble.parentElement.parentElement.parentElement.scrollTop=99999; }
  } catch(e) {
    bubble.innerHTML='⚠️ '+e.message;
  }
  btn.disabled=false;
}

// Page AI — streaming
async function sendPageAI() {
  const inp=document.getElementById('pageAiInput');
  const msg=inp.value.trim();
  if(!msg) return;
  const btn=document.getElementById('pageAiBtn');
  btn.disabled=true; inp.value='';
  appendAIMsg('pageAiBody',msg,'user');
  const bubble=appendStreamBubble('pageAiBody');
  try {
    if(!window.claude) throw new Error('Open in claude.ai to use AI features');
    const stream=window.claude.streamText({prompt:msg, system:getFleetContext()});
    let text='';
    for await(const chunk of stream){ text+=chunk; bubble.innerHTML=formatAIText(text); bubble.parentElement.parentElement.parentElement.scrollTop=99999; }
  } catch(e) {
    bubble.innerHTML='⚠️ '+e.message;
  }
  btn.disabled=false;
}

function quickAI(prompt) {
  if(currentView==='suggestions') {
    document.getElementById('pageAiInput').value=prompt;
    sendPageAI();
  } else {
    if(!aiPanelOpen) toggleAIPanel();
    document.getElementById('aiInput').value=prompt;
    sendAI();
  }
}

function askAIPrediction() {
  quickAI('Based on current machine data, forecast my energy costs for next month and suggest 3 specific actions to reduce them.');
}

function clearPageAI() {
  document.getElementById('pageAiBody').innerHTML='<div class="ai-msg ai-msg-bot"><div class="bubble">👋 Chat cleared! Ask me anything about your energy fleet.</div></div>';
}

function appendAIMsg(containerId, text, role) {
  const el=document.getElementById(containerId);
  const div=document.createElement('div');
  div.className=`ai-msg ai-msg-${role}`;
  const formatted=role==='bot'?formatAIText(text):text;
  div.innerHTML=`<div class="bubble">${formatted}</div>`;
  el.appendChild(div);
  el.scrollTop=el.scrollHeight;
  return div;
}

function formatAIText(text) {
  return text
    .replace(/\*\*(.*?)\*\*/g,'<strong>$1</strong>')
    .replace(/^### (.*)/gm,'<strong>$1</strong>')
    .replace(/^## (.*)/gm,'<strong>$1</strong>')
    .replace(/^# (.*)/gm,'<strong>$1</strong>')
    .replace(/^- (.*)/gm,'• $1')
    .replace(/\n\n/g,'<br><br>')
    .replace(/\n/g,'<br>');
}

function appendTyping(containerId) {
  const el=document.getElementById(containerId);
  const div=document.createElement('div');
  div.className='ai-msg ai-msg-bot';
  div.innerHTML='<div class="bubble"><div class="typing-indicator"><span class="typing-dot"></span><span class="typing-dot"></span><span class="typing-dot"></span></div></div>';
  el.appendChild(div);
  el.scrollTop=el.scrollHeight;
  return div;
}

function appendStreamBubble(containerId) {
  const el=document.getElementById(containerId);
  const div=document.createElement('div');
  div.className='ai-msg ai-msg-bot';
  div.innerHTML='<div class="bubble"><div class="typing-indicator"><span class="typing-dot"></span><span class="typing-dot"></span><span class="typing-dot"></span></div></div>';
  el.appendChild(div);
  el.scrollTop=el.scrollHeight;
  const bubble=div.querySelector('.bubble');
  return bubble;
}

// AI Panel
function toggleAIPanel() {
  aiPanelOpen=!aiPanelOpen;
  document.getElementById('aiPanel').classList.toggle('open');
  document.getElementById('aiOverlay').classList.toggle('show');
  if(aiPanelOpen) {
    const chips=document.getElementById('aiChips');
    if(chips&&!chips.children.length) {
      chips.innerHTML=quickChips.map(c=>`<span class="chip" onclick="quickAI('${c}')">${c}</span>`).join('');
    }
  }
}

// ══════════════════════════════════════════════
// MACHINE MANAGEMENT
// ══════════════════════════════════════════════
function openAddModal(){document.getElementById('addModal').classList.add('open');}
function openEditModal(){
  const m=machines.find(x=>x.id===selectedId);if(!m)return;
  document.getElementById('em-name').value=m.name;
  document.getElementById('em-id').value=m.id;
  document.getElementById('em-type').value=m.type;
  document.getElementById('em-rated').value=m.rated;
  document.getElementById('em-location').value=m.location;
  document.getElementById('em-shift').value=m.shift;
  document.getElementById('em-status').value=m.status;
  document.getElementById('editModal').classList.add('open');
}
function openRemoveModal(){
  const m=machines.find(x=>x.id===selectedId);if(!m)return;
  document.getElementById('removeText').textContent=`Remove "${m.name}" (${m.id})? This cannot be undone.`;
  document.getElementById('removeModal').classList.add('open');
}
function closeModal(id){document.getElementById(id).classList.remove('open');}

function addMachine() {
  const name=document.getElementById('nm-name').value.trim();
  const id=document.getElementById('nm-id').value.trim().toUpperCase();
  const rated=parseFloat(document.getElementById('nm-rated').value);
  const type=document.getElementById('nm-type').value;
  const loc=document.getElementById('nm-location').value.trim()||'Unspecified';
  const shift=parseInt(document.getElementById('nm-shift').value)||16;
  const status=document.getElementById('nm-status').value;

  if(!name||!id||!rated){alert('Name, ID and Rated Power are required.');return;}
  if(machines.find(m=>m.id===id)){alert('Machine ID already exists.');return;}

  machines.push({id,name,type,rated,location:loc,shift,status,added:new Date().toISOString().slice(0,10)});
  live[id]={power:status==='offline'?0:rated*(0.5+Math.random()*.35),eff:80+Math.random()*15,temp:40+Math.random()*30,volt:410+Math.random()*10,pf:0.88,history:Array(24).fill(0).map(()=>rated*(0.4+Math.random()*.5)),liveHistory:Array(60).fill(rated*(0.45+Math.random()*.4))};
  live[id].curr=((live[id].power*1000)/(live[id].volt*1.732*live[id].pf)).toFixed(1);

  closeModal('addModal');
  ['nm-name','nm-id','nm-rated','nm-location'].forEach(i=>{document.getElementById(i).value='';});
  document.getElementById('nm-shift').value='16';
  buildSidebar();
  populateUploadSelect();
  alert(`✅ Machine "${name}" added!`);
}

function saveMachineEdit() {
  const m=machines.find(x=>x.id===selectedId);if(!m)return;
  m.name=document.getElementById('em-name').value.trim()||m.name;
  m.type=document.getElementById('em-type').value;
  m.rated=parseFloat(document.getElementById('em-rated').value)||m.rated;
  m.location=document.getElementById('em-location').value.trim()||m.location;
  m.shift=parseInt(document.getElementById('em-shift').value)||m.shift;
  m.status=document.getElementById('em-status').value;
  closeModal('editModal');
  buildSidebar();
  buildMachineDetail(selectedId);
}

function removeMachine() {
  machines=machines.filter(m=>m.id!==selectedId);
  delete live[selectedId];
  selectedId=machines[0]?.id||'';
  closeModal('removeModal');
  buildSidebar();
  nav('fleet',document.querySelector('[data-view=fleet]'));
}

// ══════════════════════════════════════════════
// UPLOAD
// ══════════════════════════════════════════════
function populateUploadSelect() {
  const sel=document.getElementById('uploadMachSel');
  if(!sel)return;
  sel.innerHTML=machines.map(m=>`<option value="${m.id}">${m.name} (${m.id})</option>`).join('');
}

const uzone=document.getElementById('uploadZone');
uzone.addEventListener('dragover',e=>{e.preventDefault();uzone.classList.add('drag');});
uzone.addEventListener('dragleave',()=>uzone.classList.remove('drag'));
uzone.addEventListener('drop',e=>{e.preventDefault();uzone.classList.remove('drag');handleUpload({target:{files:e.dataTransfer.files}});});

function handleUpload(event) {
  const files=event.target.files;
  const machId=document.getElementById('uploadMachSel').value;
  const mach=machines.find(m=>m.id===machId);
  const dtype=document.getElementById('uploadDataType').value;
  const fb=document.getElementById('uploadFeedback');
  fb.innerHTML='';

  Array.from(files).forEach(file=>{
    const reader=new FileReader();
    reader.onload=e=>{
      let records=0;
      try {
        if(file.name.endsWith('.csv')) records=Papa.parse(e.target.result,{header:true}).data.filter(r=>Object.values(r).some(v=>v)).length;
        else if(file.name.endsWith('.xlsx')||file.name.endsWith('.xls')) {const wb=XLSX.read(e.target.result,{type:'binary'});records=XLSX.utils.sheet_to_json(wb.Sheets[wb.SheetNames[0]]).length;}
        else if(file.name.endsWith('.json')) {const d=JSON.parse(e.target.result);records=Array.isArray(d)?d.length:1;}
        uploadHistory.unshift({file:file.name,machine:mach?.name||machId,type:file.name.split('.').pop().toUpperCase(),dtype,records,date:new Date().toLocaleString('en-IN'),status:'Processed'});
        updateUploadTable();
        fb.innerHTML+=`<div class="alert alert-g">✓ <strong>${file.name}</strong> — ${records} records → ${mach?.name}</div>`;
      } catch(err){
        fb.innerHTML+=`<div class="alert alert-w">⚠️ <strong>${file.name}</strong> — ${err.message}</div>`;
      }
    };
    file.name.endsWith('.csv')||file.name.endsWith('.json')?reader.readAsText(file):reader.readAsBinaryString(file);
  });
}

function updateUploadTable() {
  const total=uploadHistory.length, records=uploadHistory.reduce((s,r)=>s+r.records,0);
  document.getElementById('up-total').textContent=total;
  document.getElementById('up-records').textContent=records.toLocaleString();
  const tbody=document.getElementById('uploadTbody');
  if(!uploadHistory.length){tbody.innerHTML='<tr><td colspan="6" style="text-align:center;color:var(--txt3);padding:30px">No uploads yet</td></tr>';return;}
  tbody.innerHTML=uploadHistory.map(r=>`<tr>
    <td><strong>${r.file}</strong></td><td>${r.machine}</td>
    <td><span class="badge bc">${r.type}</span></td>
    <td class="mono">${r.records}</td>
    <td style="color:var(--txt3);font-size:11px">${r.date}</td>
    <td><span class="badge bg">Processed</span></td>
  </tr>`).join('');
}

function clearUploadHistory() {uploadHistory=[];updateUploadTable();}

function dlTemplate(type) {
  const csvs={
    equipment:'machine_id,Equipment Name,Power (W),Hours per Day,Quantity,Condition\nCNC-01,Spindle Motor,7460,12,1,Good',
    consumption:'machine_id,Timestamp,Energy (kWh),Cost (INR)\nCNC-01,2026-03-01 00:00,45.5,387',
    sensors:'machine_id,Sensor ID,Timestamp,Value,Unit\nCNC-01,CT-01,2026-03-02 10:00:00,42.5,kW'
  };
  dlFile(csvs[type],`energiscan_${type}_template.csv`,'text/csv');
}

function dlFile(content,filename,type) {
  const a=document.createElement('a');
  a.href=URL.createObjectURL(new Blob([content],{type}));
  a.download=filename;a.click();
}

// ══════════════════════════════════════════════
// SETTINGS
// ══════════════════════════════════════════════
function buildSettingsPage() {
  document.getElementById('cfg-facility').value=settings.facility;
  document.getElementById('cfg-tariff').value=settings.tariff;
  document.getElementById('cfg-co2').value=settings.co2f;
  document.getElementById('machineSettingsList').innerHTML=machines.map(m=>`
    <div style="display:flex;align-items:center;gap:8px;padding:8px 0;border-bottom:1px solid var(--bdr)">
      <span class="mc-dot ${m.status}"></span>
      <span style="flex:1;font-size:12.5px">${m.name}</span>
      <span class="mono" style="font-size:10px;color:var(--txt3)">${m.id}</span>
      <span class="badge ${m.status==='online'?'bg':m.status==='warning'?'ba':'br'}">${m.status}</span>
    </div>`).join('');
}

function saveSettings() {
  settings.facility=document.getElementById('cfg-facility').value||settings.facility;
  settings.tariff=parseFloat(document.getElementById('cfg-tariff').value)||settings.tariff;
  settings.co2f=parseFloat(document.getElementById('cfg-co2').value)||settings.co2f;
  settings.interval=parseInt(document.getElementById('cfg-interval').value)||1500;
  settings.threshold=parseFloat(document.getElementById('cfg-threshold').value)||80;
  settings.mineff=parseFloat(document.getElementById('cfg-mineff').value)||70;
  alert('✅ Settings saved!');
}

// ══════════════════════════════════════════════
// ══════════════════════════════════════════════
// REPORTS — FULLY FUNCTIONAL
// ══════════════════════════════════════════════
let currentRptType = 'fleet';
let lastReportHTML = '';
let lastReportCSV = '';
let reportHistory = [];

function selectRptType(el, type) {
  document.querySelectorAll('.rpt-type-card').forEach(c => c.classList.remove('active'));
  el.classList.add('active');
  currentRptType = type;
}

function getRptMachineList() {
  const sel = document.getElementById('rpt-machine')?.value || 'all';
  return sel === 'all' ? machines : machines.filter(m => m.id === sel);
}

function getPeriodLabel() {
  const p = document.getElementById('rpt-period')?.value || 'month';
  return {today:'Today', week:'This Week', month:'This Month', quarter:'This Quarter', year:'This Year'}[p] || 'This Month';
}

function getPeriodDays() {
  const p = document.getElementById('rpt-period')?.value || 'month';
  return {today:1, week:7, month:30, quarter:90, year:365}[p] || 30;
}

function populateRptMachineSelect() {
  const sel = document.getElementById('rpt-machine');
  if (!sel || sel.children.length > 1) return;
  machines.forEach(m => {
    const o = document.createElement('option');
    o.value = m.id; o.textContent = m.name + ' (' + m.id + ')';
    sel.appendChild(o);
  });
}

function buildReportData() {
  const type = currentRptType;
  const mList = getRptMachineList();
  const days = getPeriodDays();
  const period = getPeriodLabel();
  const genDate = new Date().toLocaleString('en-IN');
  const tp = mList.reduce((s,m) => s + live[m.id].power, 0);
  const totalEnergy = mList.reduce((s,m) => s + live[m.id].power * m.shift * days, 0);
  const totalCost = totalEnergy * settings.tariff;
  const avgEff = mList.reduce((s,m) => s + live[m.id].eff, 0) / mList.length;
  const onlineMachines = mList.filter(m => m.status === 'online').length;
  const warningMachines = mList.filter(m => m.status === 'warning');
  const offlineMachines = mList.filter(m => m.status === 'offline');
  const bestMachine = mList.reduce((a,b) => live[a.id].eff > live[b.id].eff ? a : b);
  const worstMachine = mList.reduce((a,b) => live[a.id].eff < live[b.id].eff ? a : b);
  return { type, mList, days, period, genDate, tp, totalEnergy, totalCost, avgEff,
    onlineMachines, warningMachines, offlineMachines, bestMachine, worstMachine };
}

function generateReportHTML(data) {
  const { type, mList, days, period, genDate, tp, totalEnergy, totalCost, avgEff,
    onlineMachines, warningMachines, offlineMachines, bestMachine } = data;
  const typeLabels = {fleet:'Fleet Summary', machine:'Machine Detail', compare:'Comparison', energy:'Energy Audit', maintenance:'Maintenance Report'};
  const title = typeLabels[type] + ' Report';
  const machineTitle = mList.length === machines.length ? 'All Machines (' + machines.length + ')' : mList.map(m=>m.name).join(', ');

  // Machine rows
  const machineRows = mList.map((m) => {
    const d = live[m.id];
    const daily = Math.round(d.power * m.shift * settings.tariff);
    const pct = Math.min(100, Math.round(d.power / m.rated * 100));
    const sc = m.status==='online'?'#16a34a':m.status==='warning'?'#d97706':'#dc2626';
    const ec = d.eff >= 85 ? '#16a34a' : d.eff >= 70 ? '#d97706' : '#dc2626';
    return '<tr style="border-bottom:1px solid #e5e7eb">' +
      '<td style="padding:10px 12px;font-weight:600">' + m.name + '</td>' +
      '<td style="padding:10px 12px;font-family:monospace;font-size:11px;color:#6b7280">' + m.id + '</td>' +
      '<td style="padding:10px 12px">' + m.type + '</td>' +
      '<td style="padding:10px 12px">' + m.location + '</td>' +
      '<td style="padding:10px 12px"><span style="background:' + sc + '18;color:' + sc + ';padding:2px 8px;border-radius:4px;font-size:10px;font-weight:700;letter-spacing:.5px">' + m.status.toUpperCase() + '</span></td>' +
      '<td style="padding:10px 12px;font-family:monospace;font-weight:700">' + d.power.toFixed(1) + ' kW</td>' +
      '<td style="padding:10px 12px;font-family:monospace;color:#6b7280">' + m.rated + ' kW</td>' +
      '<td style="padding:10px 12px"><div style="display:flex;align-items:center;gap:6px"><div style="flex:1;height:5px;background:#e5e7eb;border-radius:3px;min-width:40px"><div style="width:' + pct + '%;height:100%;background:' + (pct>75?'#16a34a':pct>50?'#d97706':'#dc2626') + ';border-radius:3px"></div></div><span style="font-size:10px;font-family:monospace">' + pct + '%</span></div></td>' +
      '<td style="padding:10px 12px;font-family:monospace;font-weight:600;color:' + ec + '">' + d.eff.toFixed(1) + '%</td>' +
      '<td style="padding:10px 12px;font-family:monospace">&#8377;' + daily.toLocaleString('en-IN') + '</td>' +
      '<td style="padding:10px 12px;font-family:monospace">&#8377;' + (daily*30).toLocaleString('en-IN') + '</td>' +
    '</tr>';
  }).join('');

  // Alert rows
  const alertRows = [];
  offlineMachines.forEach(m => alertRows.push('<tr style="background:#fff5f5"><td style="padding:9px 12px"><span style="background:#fee2e2;color:#dc2626;padding:2px 8px;border-radius:4px;font-size:10px;font-weight:700">CRITICAL</span></td><td style="padding:9px 12px;font-weight:600">' + m.name + '</td><td style="padding:9px 12px">Machine is offline — immediate attention required</td></tr>'));
  warningMachines.forEach(m => alertRows.push('<tr style="background:#fffbf0"><td style="padding:9px 12px"><span style="background:#fef3c7;color:#d97706;padding:2px 8px;border-radius:4px;font-size:10px;font-weight:700">WARNING</span></td><td style="padding:9px 12px;font-weight:600">' + m.name + '</td><td style="padding:9px 12px">Efficiency at ' + live[m.id].eff.toFixed(1) + '% — schedule maintenance</td></tr>'));
  mList.forEach(m => { if(live[m.id].temp > 75) alertRows.push('<tr style="background:#fffbf0"><td style="padding:9px 12px"><span style="background:#fef3c7;color:#d97706;padding:2px 8px;border-radius:4px;font-size:10px;font-weight:700">HEAT</span></td><td style="padding:9px 12px;font-weight:600">' + m.name + '</td><td style="padding:9px 12px">High temperature: ' + live[m.id].temp.toFixed(0) + '°C — check cooling system</td></tr>'); });
  mList.forEach(m => { if(live[m.id].pf < 0.88 && m.status !== 'offline') alertRows.push('<tr><td style="padding:9px 12px"><span style="background:#e0f2fe;color:#0284c7;padding:2px 8px;border-radius:4px;font-size:10px;font-weight:700">PF</span></td><td style="padding:9px 12px;font-weight:600">' + m.name + '</td><td style="padding:9px 12px">Low power factor: ' + live[m.id].pf.toFixed(3) + ' — capacitor bank recommended</td></tr>'); });
  if (!alertRows.length) alertRows.push('<tr><td colspan="3" style="padding:16px 12px;color:#6b7280;text-align:center">&#10003; No active alerts — all systems operating normally</td></tr>');

  // Efficiency ranking
  const rankRows = [...mList].sort((a,b) => live[b.id].eff - live[a.id].eff).map((m,i) => {
    const d = live[m.id];
    const medal = i===0?'🥇':i===1?'🥈':i===2?'🥉':'#'+(i+1);
    const sc = m.status==='online'?'#16a34a':m.status==='warning'?'#d97706':'#dc2626';
    return '<tr style="border-bottom:1px solid #e5e7eb"><td style="padding:9px 12px">' + medal + '</td><td style="padding:9px 12px;font-weight:600">' + m.name + '</td><td style="padding:9px 12px"><span style="background:' + sc + '18;color:' + sc + ';padding:2px 8px;border-radius:4px;font-size:10px;font-weight:700">' + m.status.toUpperCase() + '</span></td><td style="padding:9px 12px;font-family:monospace;font-weight:700">' + d.eff.toFixed(1) + '%</td><td style="padding:9px 12px;font-family:monospace">' + d.power.toFixed(1) + ' kW</td><td style="padding:9px 12px;font-family:monospace">&#8377;' + Math.round(d.power*m.shift*settings.tariff).toLocaleString('en-IN') + '/day</td></tr>';
  }).join('');

  // Maintenance rows (for maintenance report type)
  const maintRows = mList.map(m => {
    const d = live[m.id];
    const nextService = m.status==='offline'?'Immediate':m.status==='warning'?'Within 1 week':d.eff < 80 ? 'Within 1 month' : '3 months';
    const priority = m.status==='offline'?'CRITICAL':m.status==='warning'?'HIGH':d.eff < 80 ? 'MEDIUM' : 'LOW';
    const pc = {CRITICAL:'#dc2626',HIGH:'#d97706',MEDIUM:'#2563eb',LOW:'#16a34a'}[priority];
    return '<tr style="border-bottom:1px solid #e5e7eb"><td style="padding:9px 12px;font-weight:600">' + m.name + '</td><td style="padding:9px 12px;font-family:monospace;font-size:11px">' + m.id + '</td><td style="padding:9px 12px"><span style="background:' + pc + '18;color:' + pc + ';padding:2px 8px;border-radius:4px;font-size:10px;font-weight:700">' + priority + '</span></td><td style="padding:9px 12px">' + nextService + '</td><td style="padding:9px 12px;font-family:monospace">' + d.eff.toFixed(1) + '%</td><td style="padding:9px 12px;font-family:monospace">' + d.temp.toFixed(0) + '°C</td><td style="padding:9px 12px">Oil check, belt tension, filter' + (d.eff < 75 ? ', full service' : '') + '</td></tr>';
  }).join('');

  // Recommendations
  const recs = [];
  if (offlineMachines.length) recs.push({color:'#dc2626', text: '<strong>Immediate Action:</strong> Restore ' + offlineMachines.map(m=>m.name).join(', ') + '. Offline machines causing production loss estimated at ₹' + Math.round(offlineMachines.reduce((s,m)=>s+m.rated*m.shift*settings.tariff,0)).toLocaleString('en-IN') + '/day.'});
  if (warningMachines.length) recs.push({color:'#d97706', text: '<strong>This Week:</strong> Schedule maintenance for ' + warningMachines.map(m=>m.name).join(', ') + '. Efficiency degradation is costing an estimated extra 10-15% energy per shift.'});
  if (avgEff < 80) recs.push({color:'#2563eb', text: '<strong>Optimization:</strong> Fleet efficiency at ' + avgEff.toFixed(1) + '%. Targeting 88%+ through load balancing and predictive maintenance could save ₹' + Math.round(totalCost * 0.08).toLocaleString('en-IN') + ' over this ' + period.toLowerCase() + '.'});
  recs.push({color:'#7c3aed', text: '<strong>Peak Demand:</strong> Stagger machine start-up times (avoid simultaneous peaks between 2–4 PM). Estimated demand charge reduction: 10–15% of monthly bill.'});
  recs.push({color:'#0891b2', text: '<strong>Tariff Optimization:</strong> At ₹' + settings.tariff + '/kWh, shifting ' + Math.round(tp*0.2).toFixed(0) + ' kW of flexible load to off-peak hours could save ₹' + Math.round(tp*0.2*8*0.3*settings.tariff*30).toLocaleString('en-IN') + '/month.'});
  if (mList.some(m => live[m.id].pf < 0.88 && m.status !== 'offline')) recs.push({color:'#0891b2', text: '<strong>Power Factor Correction:</strong> Machines with PF below 0.88 detected. Installing capacitor banks can reduce reactive power charges and improve equipment lifespan.'});

  const recItems = recs.map((r,i) => '<li style="padding:11px 14px;background:#f8fafc;border-left:3px solid ' + r.color + ';border-radius:0 6px 6px 0;font-size:12.5px;line-height:1.6">' + (i+1) + '. ' + r.text + '</li>').join('');

  // Extra section by report type
  let extraSection = '';
  if (type === 'compare' || type === 'fleet') {
    extraSection = '<div class="section"><div class="section-title">&#127942; Efficiency Ranking</div><table><thead><tr><th>Rank</th><th>Machine</th><th>Status</th><th>Efficiency</th><th>Power</th><th>Cost/Day</th></tr></thead><tbody>' + rankRows + '</tbody></table></div>';
  }
  if (type === 'maintenance') {
    extraSection = '<div class="section"><div class="section-title">&#128295; Maintenance Schedule</div><table><thead><tr><th>Machine</th><th>ID</th><th>Priority</th><th>Next Service</th><th>Efficiency</th><th>Temp</th><th>Tasks</th></tr></thead><tbody>' + maintRows + '</tbody></table></div>';
  }

  return '<!DOCTYPE html>\n<html lang="en">\n<head>\n<meta charset="UTF-8">\n<meta name="viewport" content="width=device-width,initial-scale=1">\n<title>EnergiScan — ' + title + '</title>\n<style>\n*{box-sizing:border-box;margin:0;padding:0}\nbody{font-family:\'Segoe UI\',Arial,sans-serif;background:#f1f5f9;color:#1e293b;font-size:13px;line-height:1.5}\n@media print{body{background:#fff}.no-print{display:none!important}@page{margin:1.5cm;size:A4}section{break-inside:avoid}}\n.page{max-width:1100px;margin:0 auto;padding:20px}\n.header{background:linear-gradient(135deg,#0f172a 0%,#1e3a5f 60%,#0f2d4a 100%);color:#fff;padding:32px 36px;border-radius:12px 12px 0 0}\n.header-row{display:flex;justify-content:space-between;align-items:flex-start;flex-wrap:wrap;gap:12px}\n.brand{font-size:10px;letter-spacing:3px;color:#00d4ff;font-weight:800;margin-bottom:6px;font-family:monospace}\n.rpt-title{font-size:28px;font-weight:800;letter-spacing:-.5px;margin-bottom:3px}\n.rpt-sub{font-size:13px;color:#94a3b8}\n.meta-row{display:flex;gap:20px;margin-top:22px;flex-wrap:wrap;padding-top:20px;border-top:1px solid rgba(255,255,255,.1)}\n.meta-item{min-width:100px}\n.meta-lbl{font-size:9px;color:#64748b;letter-spacing:2px;text-transform:uppercase;font-family:monospace;font-weight:600}\n.meta-val{font-size:20px;font-weight:800;color:#fff;font-family:monospace;margin-top:3px}\n.no-print{display:inline-flex;align-items:center;gap:6px;padding:9px 18px;background:rgba(0,212,255,.15);border:1px solid rgba(0,212,255,.3);color:#00d4ff;border-radius:8px;font-size:12px;cursor:pointer;font-weight:700;text-decoration:none;transition:.2s}\n.no-print:hover{background:rgba(0,212,255,.25)}\n.section{background:#fff;border:1px solid #e2e8f0;border-top:none;padding:22px 28px}\n.section:first-of-type{border-top:1px solid #e2e8f0}\n.section-title{font-size:13px;font-weight:700;color:#0f172a;margin-bottom:14px;padding-bottom:10px;border-bottom:2px solid #f1f5f9;display:flex;align-items:center;gap:8px}\n.kpi-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(130px,1fr));gap:10px}\n.kpi{background:#f8fafc;border:1px solid #e2e8f0;border-radius:8px;padding:14px;text-align:center;position:relative;overflow:hidden}\n.kpi::before{content:\'\';position:absolute;top:0;left:0;right:0;height:2px}\n.kpi.c::before{background:#00d4ff}.kpi.g::before{background:#16a34a}.kpi.p::before{background:#7c3aed}.kpi.a::before{background:#d97706}.kpi.r::before{background:#dc2626}.kpi.b::before{background:#0891b2}\n.kpi-label{font-size:9px;font-weight:700;color:#64748b;letter-spacing:1.5px;text-transform:uppercase;margin-bottom:7px;font-family:monospace}\n.kpi-value{font-size:22px;font-weight:800;font-family:monospace;line-height:1}\n.kpi-unit{font-size:10px;color:#94a3b8;margin-top:3px}\ntable{width:100%;border-collapse:collapse;font-size:12px}\nthead tr{background:#f8fafc}\nth{padding:9px 12px;text-align:left;font-size:9px;font-weight:700;color:#475569;text-transform:uppercase;letter-spacing:.8px;border-bottom:2px solid #e2e8f0;font-family:monospace}\n.rec-list{list-style:none;display:flex;flex-direction:column;gap:8px}\n.footer{background:#0f172a;color:#64748b;padding:16px 28px;border-radius:0 0 12px 12px;display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:8px;font-size:11px;font-family:monospace}\n.footer span{color:#94a3b8}\n</style>\n</head>\n<body>\n<div class="page">\n  <div class="header">\n    <div class="header-row">\n      <div>\n        <div class="brand">&#9889; ENERGISCAN ENTERPRISE</div>\n        <div class="rpt-title">' + title + '</div>\n        <div class="rpt-sub">' + machineTitle + ' &nbsp;&middot;&nbsp; ' + period + ' &nbsp;&middot;&nbsp; Generated ' + genDate + '</div>\n      </div>\n      <button class="no-print" onclick="window.print()">&#128424; Print / Save PDF</button>\n    </div>\n    <div class="meta-row">\n      <div class="meta-item"><div class="meta-lbl">Fleet Power</div><div class="meta-val">' + tp.toFixed(1) + ' kW</div></div>\n      <div class="meta-item"><div class="meta-lbl">Machines</div><div class="meta-val">' + mList.length + ' (' + onlineMachines + ' online)</div></div>\n      <div class="meta-item"><div class="meta-lbl">Total Energy</div><div class="meta-val">' + Math.round(totalEnergy).toLocaleString() + ' kWh</div></div>\n      <div class="meta-item"><div class="meta-lbl">Period Cost</div><div class="meta-val">&#8377;' + Math.round(totalCost).toLocaleString('en-IN') + '</div></div>\n      <div class="meta-item"><div class="meta-lbl">Avg Efficiency</div><div class="meta-val">' + avgEff.toFixed(1) + '%</div></div>\n      <div class="meta-item"><div class="meta-lbl">CO&#8322; Avoided</div><div class="meta-val">' + Math.round(totalEnergy * settings.co2f) + ' kg</div></div>\n    </div>\n  </div>\n\n  <div class="section">\n    <div class="section-title">&#128202; Key Performance Indicators</div>\n    <div class="kpi-grid">\n      <div class="kpi c"><div class="kpi-label">Fleet Power</div><div class="kpi-value" style="color:#0070ff">' + tp.toFixed(1) + '</div><div class="kpi-unit">kW real-time</div></div>\n      <div class="kpi g"><div class="kpi-label">Machines Online</div><div class="kpi-value" style="color:#16a34a">' + onlineMachines + '/' + mList.length + '</div><div class="kpi-unit">operational</div></div>\n      <div class="kpi p"><div class="kpi-label">Fleet Efficiency</div><div class="kpi-value" style="color:#7c3aed">' + avgEff.toFixed(1) + '</div><div class="kpi-unit">%</div></div>\n      <div class="kpi a"><div class="kpi-label">Daily Cost</div><div class="kpi-value" style="color:#d97706">&#8377;' + Math.round(totalCost/days).toLocaleString('en-IN') + '</div><div class="kpi-unit">average</div></div>\n      <div class="kpi r"><div class="kpi-label">Period Cost</div><div class="kpi-value" style="color:#dc2626">&#8377;' + Math.round(totalCost).toLocaleString('en-IN') + '</div><div class="kpi-unit">' + period + '</div></div>\n      <div class="kpi b"><div class="kpi-label">Best Machine</div><div class="kpi-value" style="font-size:13px;color:#0891b2">' + bestMachine.name + '</div><div class="kpi-unit">' + live[bestMachine.id].eff.toFixed(1) + '% eff</div></div>\n    </div>\n  </div>\n\n  <div class="section">\n    <div class="section-title">&#9881;&#65039; Machine Performance Table</div>\n    <div style="overflow-x:auto">\n    <table>\n      <thead><tr><th>Machine</th><th>ID</th><th>Type</th><th>Location</th><th>Status</th><th>Power</th><th>Rated</th><th>Load</th><th>Efficiency</th><th>Daily Cost</th><th>Monthly Cost</th></tr></thead>\n      <tbody>' + machineRows + '</tbody>\n    </table>\n    </div>\n  </div>\n\n  ' + extraSection + '\n\n  <div class="section">\n    <div class="section-title">&#128680; Active Alerts &amp; Issues</div>\n    <table><thead><tr><th>Severity</th><th>Machine</th><th>Description</th></tr></thead><tbody>' + alertRows.join('') + '</tbody></table>\n  </div>\n\n  <div class="section">\n    <div class="section-title">&#128161; Recommendations &amp; Action Items</div>\n    <ul class="rec-list">' + recItems + '</ul>\n  </div>\n\n  <div class="footer">\n    <span>EnergiScan Enterprise &middot; ' + settings.facility + ' &middot; Tariff: &#8377;' + settings.tariff + '/kWh &middot; CO&#8322;: ' + settings.co2f + ' kg/kWh</span>\n    <span>Report ID: RPT-' + Date.now() + ' &middot; ' + genDate + '</span>\n  </div>\n</div>\n</body>\n</html>';
}

function generateReportCSV(data) {
  const { mList, days, period, genDate, tp, totalEnergy, totalCost, avgEff } = data;
  const rows = [];
  rows.push(['"EnergiScan Enterprise — ' + getPeriodLabel() + ' Report"']);
  rows.push(['"Facility:"', '"' + settings.facility + '"', '"Generated:"', '"' + genDate + '"']);
  rows.push(['"Period:"', '"' + period + '"', '"Tariff:"', '"₹' + settings.tariff + '/kWh"']);
  rows.push([]);
  rows.push(['"SUMMARY"']);
  rows.push(['"Total Fleet Power (kW):"', tp.toFixed(2), '"Total Energy (kWh):"', Math.round(totalEnergy), '"Total Cost (₹):"', Math.round(totalCost), '"Avg Efficiency (%):"', avgEff.toFixed(2)]);
  rows.push(['"Online Machines:"', data.onlineMachines, '"Warning:"', data.warningMachines.length, '"Offline:"', data.offlineMachines.length]);
  rows.push([]);
  rows.push(['"MACHINE PERFORMANCE"']);
  rows.push(['"Machine"', '"ID"', '"Type"', '"Location"', '"Status"', '"Power (kW)"', '"Rated (kW)"', '"Load %"', '"Efficiency %"', '"Temp °C"', '"Voltage V"', '"Current A"', '"Power Factor"', '"Daily Cost ₹"', '"Monthly Cost ₹"', '"Shift (h)"']);
  mList.forEach(m => {
    const d = live[m.id];
    const pct = Math.min(100, Math.round(d.power / m.rated * 100));
    const daily = Math.round(d.power * m.shift * settings.tariff);
    rows.push(['"' + m.name + '"', '"' + m.id + '"', '"' + m.type + '"', '"' + m.location + '"', '"' + m.status + '"',
      d.power.toFixed(2), m.rated, pct, d.eff.toFixed(2), d.temp.toFixed(1),
      d.volt.toFixed(1), d.curr, d.pf.toFixed(4), daily, daily * 30, m.shift]);
  });
  rows.push([]);
  rows.push(['"24-HOUR POWER HISTORY (kW)"']);
  rows.push(['"Machine"', '"ID"', ...Array.from({length:24}, (_,i) => '"' + i + ':00"')]);
  mList.forEach(m => rows.push(['"' + m.name + '"', '"' + m.id + '"', ...live[m.id].history.map(v => v.toFixed(3))]));
  rows.push([]);
  rows.push(['"ALERTS"']);
  rows.push(['"Severity"', '"Machine"', '"Issue"']);
  data.offlineMachines.forEach(m => rows.push(['"CRITICAL"', '"' + m.name + '"', '"Machine offline"']));
  data.warningMachines.forEach(m => rows.push(['"WARNING"', '"' + m.name + '"', '"Efficiency at ' + live[m.id].eff.toFixed(1) + '%"']));
  mList.forEach(m => { if(live[m.id].temp > 75) rows.push(['"HEAT"', '"' + m.name + '"', '"High temp: ' + live[m.id].temp.toFixed(0) + '°C"']); });
  return rows.map(r => r.join(',')).join('\n');
}

function previewReport() {
  const data = buildReportData();
  lastReportHTML = generateReportHTML(data);
  lastReportCSV = generateReportCSV(data);
  const inner = document.getElementById('rptPreviewInner');
  const card = document.getElementById('rptPreviewCard');
  // Render the HTML report directly
  inner.innerHTML = lastReportHTML;
  card.style.display = 'block';
  card.scrollIntoView({ behavior: 'smooth', block: 'start' });
  addToReportHistory(data);
}

function closePreview() {
  document.getElementById('rptPreviewCard').style.display = 'none';
}

function downloadReport(format) {
  const data = buildReportData();
  if (!lastReportHTML) lastReportHTML = generateReportHTML(data);
  if (!lastReportCSV) lastReportCSV = generateReportCSV(data);
  const typeLabels = {fleet:'fleet-summary', machine:'machine-detail', compare:'comparison', energy:'energy-audit', maintenance:'maintenance'};
  const fname = 'EnergiScan_' + typeLabels[data.type] + '_' + data.period.replace(/ /g,'_') + '_' + new Date().toISOString().slice(0,10);
  if (format === 'csv') {
    dlFile(lastReportCSV, fname + '.csv', 'text/csv;charset=utf-8;');
  } else {
    dlFile(lastReportHTML, fname + '.html', 'text/html;charset=utf-8;');
  }
  addToReportHistory(data);
}

function printReport() {
  const data = buildReportData();
  if (!lastReportHTML) lastReportHTML = generateReportHTML(data);
  const win = window.open('', '_blank', 'width=1100,height=800');
  if (!win) { alert('Please allow pop-ups to print reports.'); return; }
  win.document.write(lastReportHTML);
  win.document.close();
  win.focus();
  setTimeout(() => { win.print(); }, 600);
  addToReportHistory(data);
}

function addToReportHistory(data) {
  const typeLabels = {fleet:'Fleet Summary', machine:'Machine Detail', compare:'Comparison', energy:'Energy Audit', maintenance:'Maintenance'};
  const machName = data.mList.length === machines.length ? 'All Machines' : data.mList.map(m=>m.name).join(', ');
  // Update if same combo exists, else add
  const existing = reportHistory.findIndex(r => r.type === data.type && r.period === data.period && r.machine === machName);
  const entry = { name: typeLabels[data.type] + ' — ' + data.period, type: data.type, machine: machName, period: data.period, date: new Date().toLocaleString('en-IN'), html: lastReportHTML, csv: lastReportCSV };
  if (existing >= 0) reportHistory[existing] = entry;
  else reportHistory.unshift(entry);
  buildReportsTable();
}

function buildReportsTable() {
  const tbody = document.getElementById('reportTbody');
  if (!tbody) return;
  if (!reportHistory.length) {
    tbody.innerHTML = '<tr><td colspan="6" style="text-align:center;color:var(--txt3);padding:32px;font-size:13px">No reports generated yet — use the builder above to generate your first report</td></tr>';
    return;
  }
  const tc = {fleet:'bc', machine:'bc', compare:'bp', energy:'ba', maintenance:'br'};
  tbody.innerHTML = reportHistory.map((r, i) => '<tr>' +
    '<td><strong>' + r.name + '</strong></td>' +
    '<td><span class="badge ' + (tc[r.type]||'bc') + '">' + r.type + '</span></td>' +
    '<td style="font-size:12px;color:var(--txt2)">' + r.machine + '</td>' +
    '<td style="font-size:12px">' + r.period + '</td>' +
    '<td style="color:var(--txt3);font-size:11px">' + r.date + '</td>' +
    '<td><div style="display:flex;gap:5px;flex-wrap:wrap">' +
      '<button class="btn btn-g btn-sm" onclick="histPreview(' + i + ')">👁</button>' +
      '<button class="btn btn-sm" style="background:rgba(0,230,118,.1);color:var(--grn);border:1px solid rgba(0,230,118,.2)" onclick="histDl(' + i + ',\'csv\')">↓ CSV</button>' +
      '<button class="btn btn-sm" style="background:rgba(179,136,255,.08);color:var(--pur);border:1px solid rgba(179,136,255,.15)" onclick="histDl(' + i + ',\'html\')">↓ HTML</button>' +
      '<button class="btn btn-g btn-sm" onclick="histPrint(' + i + ')">🖨</button>' +
      '<button class="btn btn-sm" style="background:var(--red2);color:var(--red);border:1px solid rgba(255,68,68,.2)" onclick="histDelete(' + i + ')">✕</button>' +
    '</div></td>' +
  '</tr>').join('');
}

function histPreview(i) {
  const r = reportHistory[i];
  lastReportHTML = r.html; lastReportCSV = r.csv;
  document.getElementById('rptPreviewInner').innerHTML = r.html;
  const card = document.getElementById('rptPreviewCard');
  card.style.display = 'block';
  card.scrollIntoView({ behavior: 'smooth', block: 'start' });
}

function histDl(i, format) {
  const r = reportHistory[i];
  const fname = 'EnergiScan_' + r.type + '_' + r.period.replace(/ /g,'_');
  if (format === 'csv' && r.csv) dlFile(r.csv, fname + '.csv', 'text/csv;charset=utf-8;');
  else if (format === 'html' && r.html) dlFile(r.html, fname + '.html', 'text/html;charset=utf-8;');
}

function histPrint(i) {
  const r = reportHistory[i];
  const win = window.open('', '_blank', 'width=1100,height=800');
  if (!win) { alert('Please allow pop-ups to print.'); return; }
  win.document.write(r.html);
  win.document.close();
  setTimeout(() => win.print(), 600);
}

function histDelete(i) {
  reportHistory.splice(i, 1);
  buildReportsTable();
}

function clearReportHistory() {
  if (reportHistory.length && !confirm('Clear all ' + reportHistory.length + ' report(s)?')) return;
  reportHistory = [];
  lastReportHTML = ''; lastReportCSV = '';
  document.getElementById('rptPreviewCard').style.display = 'none';
  buildReportsTable();
}

// ══════════════════════════════════════════════
// SIMULATION
// ══════════════════════════════════════════════
function startSim() {
  simTimer=setInterval(()=>{
    machines.forEach(m=>{
      if(m.status==='offline') return;
      const d=live[m.id];
      d.power=Math.max(m.rated*.25,Math.min(m.rated*.99,d.power+(Math.random()-.47)*.9));
      d.temp=Math.max(28,Math.min(95,d.temp+(Math.random()-.5)*.5));
      d.eff=Math.max(50,Math.min(98,d.eff+(Math.random()-.5)*.25));
      d.volt=410+(Math.random()-.5)*8;
      d.curr=((d.power*1000)/(d.volt*1.732*d.pf)).toFixed(1);
      d.history.shift();d.history.push(d.power);
      d.liveHistory.shift();d.liveHistory.push(d.power);
    });

    buildSidebar();
    if(currentView==='fleet') updateFleetKPIs();
    else if(currentView==='machine') updateMachineLive();
    else if(currentView==='realtime') updateRTSensors();
  },settings.interval);
}

function toggleSim() {
  if(simRunning){clearInterval(simTimer);simRunning=false;document.getElementById('simBtn').textContent='▶ Resume';}
  else{startSim();simRunning=true;document.getElementById('simBtn').textContent='⏸ Pause';}
}

function updateFleetKPIs() {
  const tp=totalPwr();
  document.getElementById('kpi-power').textContent=fmt(tp);
  document.getElementById('kpi-online').textContent=onlineCnt();
  document.getElementById('kpi-cost').textContent=fmtINR(tp*24*30*settings.tariff);
  document.getElementById('kpi-eff').textContent=avgEff();
  document.getElementById('kpi-co2').textContent=Math.round(tp*24*30*settings.co2f).toLocaleString('en-IN');
  document.getElementById('topSub').textContent=`${machines.length} machines · ${onlineCnt()} online · ${fmt(tp)} kW total`;
}

function updateMachineLive() {
  const m=machines.find(x=>x.id===selectedId);if(!m||m.status==='offline')return;
  const d=live[selectedId];
  document.getElementById('md-pwr').textContent=fmt(d.power);
  document.getElementById('md-eff').textContent=fmt(d.eff);
}

function updateRTSensors() {
  machines.forEach(m=>{
    const d=live[m.id];
    const pe=document.getElementById(`rt-${m.id}-p`);
    const te=document.getElementById(`rt-${m.id}-t`);
    const ce=document.getElementById(`rt-${m.id}-c`);
    const ee=document.getElementById(`rt-${m.id}-e`);
    if(pe) pe.textContent=m.status==='offline'?'OFF':fmt(d.power);
    if(te) te.textContent=m.status==='offline'?'—':fmt(d.temp);
    if(ce) ce.textContent=m.status==='offline'?'—':d.curr;
    if(ee) ee.textContent=fmt(d.eff);
  });
  if(liveChartInst) {
    machines.slice(0,6).forEach((m,i)=>{liveChartInst.data.datasets[i].data=[...live[m.id].liveHistory];});
    liveChartInst.update('none');
  }
}

function refreshAll() {
  if(currentView==='fleet') buildFleet();
  else if(currentView==='machine') buildMachineDetail(selectedId);
  else if(currentView==='compare') buildCompare();
  else if(currentView==='realtime'){renderSensorGrid();}
  else if(currentView==='analytics') buildAnalytics();
}

function exportMachineCSV() {
  const m=machines.find(x=>x.id===selectedId);if(!m)return;
  const csv='Hour,Power (kW)\n'+live[selectedId].history.map((v,i)=>`${i}:00,${v.toFixed(2)}`).join('\n');
  dlFile(csv,`${m.id}_24h.csv`,'text/csv');
}

// ══════════════════════════════════════════════
// INIT
// ══════════════════════════════════════════════
function init() {
  buildSidebar();
  buildFleet();
  startSim();
}

init();
</script>
</body>
</html>
