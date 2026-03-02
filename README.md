<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1.0"/>
<title>FarmTech Pro – KPP-Enabled Farmer Platform</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800;900&family=DM+Sans:ital,wght@0,300;0,400;0,500;0,600;0,700;1,400&family=JetBrains+Mono:wght@400;500;700&display=swap" rel="stylesheet"/>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"/>
<style>
:root{
  --bg:#050e08;--surf:#091508;--card:#0d1f10;--card2:#112415;
  --bdr:#1a3820;--bdr2:#234826;
  --acc:#22c55e;--acc2:#16a34a;--acc3:#14532d;
  --gold:#f59e0b;--gold2:#d97706;--blue:#38bdf8;--purple:#a78bfa;
  --red:#ef4444;--orange:#f97316;
  --txt:#f0fdf4;--txt2:#bbf7d0;--muted:#4d6b55;--muted2:#658a6e;
  --radius:16px;--radius-sm:10px;--radius-xs:8px;
}
*{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent}
html{scroll-behavior:smooth}
body{font-family:'DM Sans',sans-serif;background:var(--bg);color:var(--txt);min-height:100vh;overflow-x:hidden}
::-webkit-scrollbar{width:4px}::-webkit-scrollbar-thumb{background:var(--bdr2);border-radius:4px}

@keyframes fadeUp{from{opacity:0;transform:translateY(18px)}to{opacity:1;transform:none}}
@keyframes fadeIn{from{opacity:0}to{opacity:1}}
@keyframes glowPulse{0%,100%{box-shadow:0 0 20px rgba(34,197,94,.25)}50%{box-shadow:0 0 50px rgba(34,197,94,.6)}}
@keyframes pop{from{opacity:0;transform:translateY(-12px) scale(.9)}to{opacity:1;transform:none}}
@keyframes badgePop{0%{transform:scale(0)}60%{transform:scale(1.15)}100%{transform:scale(1)}}
@keyframes spin{to{transform:rotate(360deg)}}
@keyframes shimmer{0%{background-position:-200% 0}100%{background-position:200% 0}}
@keyframes float{0%,100%{transform:translateY(0)}50%{transform:translateY(-8px)}}

/* TOPBAR */
#topbar{position:sticky;top:0;z-index:100;display:flex;align-items:center;justify-content:space-between;padding:10px 14px;background:rgba(5,14,8,.96);backdrop-filter:blur(20px);border-bottom:1px solid var(--bdr)}
.logo{display:flex;align-items:center;gap:8px;cursor:pointer}
.logo-icon{width:34px;height:34px;border-radius:10px;background:linear-gradient(135deg,var(--acc),var(--acc3));display:flex;align-items:center;justify-content:center;font-size:16px;box-shadow:0 0 16px rgba(34,197,94,.45)}
.logo-txt{font-family:'Syne',sans-serif;font-weight:900;font-size:16px;background:linear-gradient(135deg,var(--acc),#86efac);-webkit-background-clip:text;-webkit-text-fill-color:transparent}
.topbar-right{display:flex;align-items:center;gap:6px;flex-wrap:wrap}
.uid-badge{font-family:'JetBrains Mono',monospace;font-size:10px;padding:4px 10px;border-radius:20px;background:rgba(34,197,94,.12);color:var(--acc);border:1px solid rgba(34,197,94,.3);font-weight:700;display:none}
.sub-badge{font-size:10px;padding:4px 10px;border-radius:20px;background:rgba(245,158,11,.12);color:var(--gold);border:1px solid rgba(245,158,11,.3);font-weight:800;display:none}
.lang-selector{padding:4px 10px;background:var(--surf);border:1px solid var(--bdr);border-radius:20px;color:var(--txt);font-size:10px;font-weight:700;cursor:pointer}

/* BUTTONS */
.btn{display:inline-flex;align-items:center;justify-content:center;gap:7px;padding:11px 22px;border-radius:var(--radius-sm);border:none;font-family:'DM Sans',sans-serif;font-weight:700;font-size:14px;cursor:pointer;transition:all .2s;position:relative;overflow:hidden}
.btn:active{transform:scale(.97)}
.btn-primary{background:linear-gradient(135deg,var(--acc),var(--acc2));color:#000;box-shadow:0 4px 15px rgba(34,197,94,.25)}
.btn-primary:hover{box-shadow:0 4px 25px rgba(34,197,94,.5)}
.btn-gold{background:linear-gradient(135deg,var(--gold),var(--gold2));color:#000;box-shadow:0 4px 15px rgba(245,158,11,.25)}
.btn-outline{background:transparent;color:var(--acc);border:1.5px solid var(--acc)}
.btn-outline:hover{background:rgba(34,197,94,.08)}
.btn-ghost{background:rgba(255,255,255,.04);color:var(--txt2);border:1px solid var(--bdr)}
.btn-ghost:hover{background:rgba(255,255,255,.08)}
.btn-danger{background:rgba(239,68,68,.1);color:var(--red);border:1.5px solid var(--red)}
.btn-sm{padding:7px 14px;font-size:12px;border-radius:var(--radius-xs)}
.btn-full{width:100%}
.btn-lg{padding:14px 28px;font-size:16px}

/* INPUTS */
.inp{width:100%;padding:11px 14px;border-radius:var(--radius-sm);border:1.5px solid var(--bdr);background:var(--surf);color:var(--txt);font-size:14px;font-family:'DM Sans',sans-serif;outline:none;transition:border .2s,box-shadow .2s}
.inp:focus{border-color:var(--acc);box-shadow:0 0 0 3px rgba(34,197,94,.1)}
.inp::placeholder{color:var(--muted)}
select.inp{appearance:none;cursor:pointer;background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='8'%3E%3Cpath d='M1 1l5 5 5-5' stroke='%234d6b55' stroke-width='2' fill='none' stroke-linecap='round'/%3E%3C/svg%3E");background-repeat:no-repeat;background-position:right 12px center}

/* CARDS */
.card{background:var(--card);border-radius:var(--radius);border:1px solid var(--bdr);padding:18px;margin-bottom:12px}
.card-gold{border-color:rgba(245,158,11,.35);background:linear-gradient(135deg,rgba(245,158,11,.05),var(--card))}
.card-green{border-color:rgba(34,197,94,.3);background:linear-gradient(135deg,rgba(34,197,94,.05),var(--card))}

/* TAGS */
.tag{display:inline-flex;align-items:center;gap:3px;padding:3px 9px;border-radius:20px;font-size:11px;font-weight:700}
.tag-green{background:rgba(34,197,94,.12);color:var(--acc);border:1px solid rgba(34,197,94,.25)}
.tag-gold{background:rgba(245,158,11,.12);color:var(--gold);border:1px solid rgba(245,158,11,.25)}
.tag-blue{background:rgba(56,189,248,.12);color:var(--blue);border:1px solid rgba(56,189,248,.25)}
.tag-red{background:rgba(239,68,68,.12);color:var(--red);border:1px solid rgba(239,68,68,.25)}
.tag-purple{background:rgba(167,139,250,.12);color:var(--purple);border:1px solid rgba(167,139,250,.25)}

/* SCREEN SYSTEM */
.screen{display:none;min-height:calc(100vh - 60px);width:100%}
.screen.active{display:block;animation:fadeIn .3s}

/* LANDING */
#landing-screen.active{display:flex;flex-direction:column;align-items:center;justify-content:center;padding:40px 20px;text-align:center}
.landing-bg{position:absolute;inset:0;background:radial-gradient(ellipse 70% 50% at 50% 0%,rgba(34,197,94,.08),transparent);pointer-events:none}
.hero-icon{width:84px;height:84px;border-radius:24px;background:linear-gradient(135deg,var(--acc),var(--acc3));display:flex;align-items:center;justify-content:center;font-size:40px;margin:0 auto 22px;box-shadow:0 0 60px rgba(34,197,94,.5);animation:glowPulse 3s infinite,float 4s ease-in-out infinite}
.hero-title{font-family:'Syne',sans-serif;font-size:clamp(38px,8vw,64px);font-weight:900;background:linear-gradient(135deg,var(--acc) 0%,#86efac 50%,var(--gold) 100%);-webkit-background-clip:text;-webkit-text-fill-color:transparent;letter-spacing:-2px;line-height:1;margin-bottom:16px}
.hero-sub{font-size:clamp(13px,2.8vw,16px);color:var(--muted2);max-width:520px;line-height:1.7;margin-bottom:24px}
.hero-btns{display:flex;gap:12px;flex-wrap:wrap;justify-content:center;margin-bottom:28px}

/* KPP LOGIN */
.kpp-container{max-width:540px;width:100%;margin:0 auto;padding:32px 20px}
.kpp-card{background:var(--card);border:1px solid var(--bdr);border-radius:var(--radius);padding:28px;box-shadow:0 8px 40px rgba(0,0,0,.4)}
.kpp-header{text-align:center;margin-bottom:24px}
.kpp-icon{width:72px;height:72px;background:linear-gradient(135deg,var(--acc),var(--acc2));border-radius:var(--radius-sm);display:flex;align-items:center;justify-content:center;font-size:36px;margin:0 auto 16px;box-shadow:0 0 30px rgba(34,197,94,.4);animation:pulse 2s infinite}
@keyframes pulse{0%,100%{transform:scale(1)}50%{transform:scale(1.05)}}
.kpp-title{font-family:'Syne',sans-serif;font-size:24px;font-weight:900;margin-bottom:8px}
.kpp-subtitle{font-size:13px;color:var(--muted2);line-height:1.6}
.field-group{margin-bottom:14px}
.field-label{font-size:12px;color:var(--muted2);margin-bottom:6px;display:block;font-weight:600}
.input-group{position:relative}
.input-icon{position:absolute;left:14px;top:50%;transform:translateY(-50%);color:var(--muted);font-size:16px}
.input-group .inp{padding-left:42px}
.alert{padding:11px 14px;border-radius:var(--radius-sm);font-size:12.5px;line-height:1.6;margin-bottom:14px;display:flex;gap:8px;align-items:flex-start}
.alert-info{background:rgba(56,189,248,.08);border:1px solid rgba(56,189,248,.25);color:#bae6fd}
.alert-warn{background:rgba(245,158,11,.08);border:1px solid rgba(245,158,11,.3);color:#fde68a}
.alert-success{background:rgba(34,197,94,.08);border:1px solid rgba(34,197,94,.25);color:#bbf7d0}

/* LOADING */
.loading{display:flex;flex-direction:column;align-items:center;justify-content:center;padding:60px 20px;gap:20px}
.spinner{width:48px;height:48px;border:4px solid rgba(34,197,94,.2);border-top-color:var(--acc);border-radius:50%;animation:spin 1s linear infinite}
.loading-text{font-size:15px;color:var(--txt2);font-weight:600}
.loading-steps{margin-top:20px;font-size:13px;color:var(--muted);text-align:center;max-width:400px}
.loading-steps div{margin:8px 0;padding:8px 16px;background:rgba(255,255,255,.02);border-radius:var(--radius-xs);border-left:3px solid transparent;transition:all .3s}
.loading-steps div.active{border-left-color:var(--acc);background:rgba(34,197,94,.08);color:var(--txt)}

/* DASHBOARD */
.dash-header{padding:14px 16px;background:var(--surf);border-bottom:1px solid var(--bdr);display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:10px}
.dash-name{font-family:'Syne',sans-serif;font-size:16px;font-weight:900}
.nav-tabs{display:flex;gap:2px;padding:7px 8px;background:var(--surf);border-bottom:1px solid var(--bdr);overflow-x:auto;scrollbar-width:none}
.nav-tabs::-webkit-scrollbar{display:none}
.nav-tab{display:flex;align-items:center;gap:5px;padding:8px 11px;border-radius:var(--radius-xs);border:none;background:transparent;color:var(--muted);font-weight:600;font-size:12.5px;cursor:pointer;white-space:nowrap;flex-shrink:0;transition:all .18s}
.nav-tab:hover{color:var(--txt2);background:rgba(255,255,255,.04)}
.nav-tab.active{background:rgba(34,197,94,.14);color:var(--acc);font-weight:800}
.tab-content{display:none;padding-bottom:50px}
.tab-content.active{display:block;animation:fadeIn .3s}

/* METRICS */
.metrics-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(140px,1fr));gap:10px;padding:14px 16px}
.metric-card{padding:16px;border-radius:var(--radius);background:var(--card);border:1px solid var(--bdr);text-align:center;transition:all .2s}
.metric-card:hover{transform:translateY(-2px);border-color:var(--bdr2)}
.metric-icon{font-size:24px;margin-bottom:8px}
.metric-val{font-family:'Syne',sans-serif;font-size:22px;font-weight:900;margin-bottom:4px}
.metric-lbl{font-size:11px;color:var(--muted);line-height:1.3}
.c-green{color:var(--acc)}.c-gold{color:var(--gold)}.c-blue{color:var(--blue)}.c-red{color:var(--red)}

/* SCHEMES */
.sec-hd{padding:14px 16px 8px;display:flex;align-items:center;justify-content:space-between}
.sec-hd-title{font-family:'Syne',sans-serif;font-size:15px;font-weight:800}
.scheme-card{margin:0 16px 12px;padding:18px;border-radius:var(--radius);background:var(--card);border:1px solid var(--bdr);cursor:pointer;transition:all .2s;position:relative}
.scheme-card:hover{border-color:var(--bdr2);transform:translateY(-2px)}
.scheme-header{display:flex;gap:12px;margin-bottom:10px}
.scheme-icon-box{width:48px;height:48px;border-radius:12px;display:flex;align-items:center;justify-content:center;font-size:24px;flex-shrink:0}
.scheme-name{font-weight:800;font-size:14px;margin-bottom:4px;line-height:1.3}
.scheme-ministry{font-size:11px;color:var(--muted2);margin-bottom:4px}
.scheme-amount{font-family:'Syne',sans-serif;font-size:16px;font-weight:900;color:var(--gold)}
.scheme-desc{font-size:12px;color:var(--muted2);line-height:1.65;margin-bottom:10px}
.scheme-footer{display:flex;gap:8px;flex-wrap:wrap;margin-top:12px}
.badge-new{position:absolute;top:12px;right:12px;background:var(--red);color:#fff;font-size:9px;font-weight:800;padding:3px 10px;border-radius:20px}

/* WEATHER */
.weather-card{background:linear-gradient(135deg,#0b2a14,#0a2410);border:2px solid rgba(34,197,94,.3);border-radius:var(--radius);padding:24px;text-align:center;margin:0 16px 14px}
.weather-icon{font-size:60px;margin-bottom:12px}
.weather-temp{font-family:'Syne',sans-serif;font-size:48px;font-weight:900;color:var(--acc);margin-bottom:8px}
.weather-condition{font-size:16px;font-weight:700;margin-bottom:20px}
.weather-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(100px,1fr));gap:12px;margin-top:20px}
.weather-item{padding:12px;background:rgba(34,197,94,.06);border:1px solid rgba(34,197,94,.15);border-radius:var(--radius-xs)}
.weather-item-label{font-size:11px;color:var(--muted2);margin-bottom:4px}
.weather-item-value{font-size:16px;font-weight:800}

/* TOAST */
#toast{position:fixed;bottom:24px;left:50%;transform:translateX(-50%) translateY(120px);background:var(--card2);border:1px solid var(--bdr2);border-radius:var(--radius-sm);padding:12px 20px;font-size:13px;font-weight:600;color:var(--txt);z-index:999;transition:transform .3s;box-shadow:0 8px 30px rgba(0,0,0,.6);max-width:90vw;text-align:center}
#toast.show{transform:translateX(-50%) translateY(0)}

/* PROFILE */
.profile-card{background:linear-gradient(135deg,#0b2a14,#0a2410);border:2px solid rgba(34,197,94,.3);border-radius:var(--radius);padding:24px;margin:0 16px 14px;position:relative}
.profile-header{display:flex;gap:16px;align-items:flex-start;margin-bottom:16px}
.profile-avatar{width:64px;height:64px;border-radius:var(--radius-sm);background:linear-gradient(135deg,var(--acc),var(--acc2));display:flex;align-items:center;justify-content:center;font-size:32px}
.profile-name{font-family:'Syne',sans-serif;font-size:20px;font-weight:900;margin-bottom:4px}
.profile-uid{font-family:'JetBrains Mono',monospace;font-size:14px;color:var(--acc);font-weight:700;letter-spacing:1px;margin-bottom:8px}
.profile-tags{display:flex;gap:6px;flex-wrap:wrap}

/* MODAL */
.modal-overlay{position:fixed;inset:0;background:rgba(0,0,0,.8);z-index:200;display:flex;align-items:flex-end;justify-content:center;backdrop-filter:blur(4px)}
.modal-overlay.hidden{display:none}
.modal-sheet{background:var(--surf);border-radius:20px 20px 0 0;border:1px solid var(--bdr2);width:100%;max-width:600px;max-height:85vh;overflow-y:auto;padding:24px 20px 40px;animation:fadeUp .3s}
.modal-title{font-family:'Syne',sans-serif;font-size:20px;font-weight:900;margin-bottom:8px}
.modal-sub{font-size:12px;color:var(--muted2);margin-bottom:20px}

/* CARBON */
.carbon-hero{margin:0 16px 14px;padding:24px;border-radius:var(--radius);background:linear-gradient(135deg,#071a0c,#0a2410);border:2px solid rgba(34,197,94,.4);box-shadow:0 0 40px rgba(34,197,94,.08)}
.carbon-title{font-family:'Syne',sans-serif;font-size:20px;font-weight:900;margin-bottom:8px;line-height:1.2}
.carbon-sub{font-size:12.5px;color:var(--muted2);line-height:1.65;margin-bottom:16px}
.carbon-stats{display:grid;grid-template-columns:1fr 1fr 1fr;gap:10px;margin-bottom:16px}
.carbon-stat{text-align:center;padding:12px;border-radius:var(--radius-xs);background:rgba(34,197,94,.07);border:1px solid rgba(34,197,94,.15)}
.carbon-stat-val{font-family:'Syne',sans-serif;font-size:18px;font-weight:900;color:var(--acc)}
.carbon-stat-lbl{font-size:10px;color:var(--muted2);margin-top:2px}

/* TX LIST */
.tx-list{margin:0 16px 14px;background:var(--card);border-radius:var(--radius);border:1px solid var(--bdr);overflow:hidden}
.tx-item{display:flex;align-items:center;gap:12px;padding:14px;border-bottom:1px solid rgba(26,56,32,.5)}
.tx-item:last-child{border-bottom:none}
.tx-icon{width:40px;height:40px;border-radius:10px;display:flex;align-items:center;justify-content:center;font-size:16px;flex-shrink:0}
.tx-icon.credit{background:rgba(34,197,94,.12)}
.tx-icon.debit{background:rgba(239,68,68,.12)}
.tx-info{flex:1;min-width:0}
.tx-desc{font-weight:700;font-size:13px;margin-bottom:3px}
.tx-meta{font-size:11px;color:var(--muted)}
.tx-amount{text-align:right}
.tx-val{font-weight:900;font-size:14px}
.tx-c{color:var(--acc)}.tx-d{color:var(--red)}

@media(max-width:480px){
  .metrics-grid{grid-template-columns:repeat(2,1fr)}
  .carbon-stats{grid-template-columns:1fr 1fr}
}
</style>
</head>
<body>

<!-- TOPBAR -->
<div id="topbar">
  <div class="logo" onclick="showScreen('landing')">
    <div class="logo-icon">🌾</div>
    <div class="logo-txt">FarmTech Pro</div>
  </div>
  <div class="topbar-right">
    <select class="lang-selector" id="langSelector" onchange="changeLanguage(this.value)">
      <option value="en">English</option>
      <option value="hi">हिंदी</option>
      <option value="pa">ਪੰਜਾਬੀ</option>
    </select>
    <div class="uid-badge" id="topbar-uid">KPP-000000</div>
    <div class="sub-badge" id="topbar-sub">⭐ PRO</div>
    <button class="btn btn-ghost btn-sm" id="topbar-login-btn" onclick="showScreen('kpp-login')">Login</button>
    <button class="btn btn-danger btn-sm" id="topbar-logout-btn" style="display:none" onclick="logoutUser()">Logout</button>
  </div>
</div>

<div id="screens-wrap">

<!-- LANDING SCREEN -->
<div class="screen active" id="landing-screen">
  <div class="landing-bg"></div>
  <div class="hero-icon">🌾</div>
  <h1 class="hero-title" data-translate="heroTitle">FarmTech Pro</h1>
  <p class="hero-sub" data-translate="heroSub">India's first KPP-enabled digital farming platform. Auto-fetch your data, apply for schemes, earn carbon credits.</p>
  <div class="hero-btns">
    <button class="btn btn-primary btn-lg" onclick="showScreen('kpp-login')">
      <i class="fas fa-id-card"></i> <span data-translate="loginKPP">Login with KPP</span>
    </button>
    <button class="btn btn-outline btn-lg" onclick="loadDemo()">
      <i class="fas fa-play"></i> <span data-translate="tryDemo">Try Demo</span>
    </button>
  </div>
  <div style="margin-top:28px;display:flex;gap:12px;flex-wrap:wrap;justify-content:center">
    <div class="tag tag-green">✅ KPP Auto-Fetch</div>
    <div class="tag tag-gold">⭐ ₹499/year Pro</div>
    <div class="tag tag-blue">🌿 Carbon Credits</div>
    <div class="tag tag-purple">📱 28+ Schemes</div>
  </div>
</div>

<!-- KPP LOGIN SCREEN -->
<div class="screen" id="kpp-login-screen">
  <div class="kpp-container">
    <div class="kpp-card">
      <div class="kpp-header">
        <div class="kpp-icon"><i class="fas fa-id-card"></i></div>
        <h2 class="kpp-title" data-translate="kppTitle">Kisan Pehchaan Patra Login</h2>
        <p class="kpp-subtitle" data-translate="kppSubtitle">Enter your 14-digit KPP number to auto-fetch all your farm data from government database</p>
      </div>

      <div class="alert alert-info">
        <i class="fas fa-info-circle"></i>
        <div data-translate="kppInfo">Soil data, NPK values, land records, and crop history will be automatically loaded</div>
      </div>

      <form onsubmit="handleKPPLogin(event)" id="kppForm">
        <div class="field-group">
          <label class="field-label" data-translate="kppLabel">Kisan Pehchaan Patra Number *</label>
          <div class="input-group">
            <i class="fas fa-id-card input-icon"></i>
            <input type="text" class="inp" id="kppNumber" placeholder="XX-XXXX-XXXX-XXX" maxlength="17" required oninput="formatKPP(this)">
          </div>
        </div>

        <div class="field-group">
          <label class="field-label" data-translate="mobileLabel">Mobile Number (Linked to KPP) *</label>
          <div class="input-group">
            <i class="fas fa-mobile-alt input-icon"></i>
            <input type="tel" class="inp" id="mobileNumber" placeholder="9876543210" maxlength="10" required>
          </div>
        </div>

        <button type="submit" class="btn btn-primary btn-full btn-lg">
          <i class="fas fa-sign-in-alt"></i> <span data-translate="fetchLogin">Fetch Data & Login</span>
        </button>
      </form>

      <div style="text-align:center;margin-top:16px">
        <button class="btn btn-ghost btn-sm" onclick="showScreen('landing')">
          <i class="fas fa-arrow-left"></i> <span data-translate="backHome">Back to Home</span>
        </button>
      </div>

      <div class="alert alert-warn" style="margin-top:16px">
        <i class="fas fa-exclamation-triangle"></i>
        <div data-translate="demoNotice">Demo Mode: Enter any 14-digit number to see simulated auto-fetched data</div>
      </div>
    </div>
  </div>
</div>

<!-- LOADING SCREEN -->
<div class="screen" id="loading-screen">
  <div class="loading">
    <div class="spinner"></div>
    <div class="loading-text" data-translate="loadingText">Fetching your data from KPP database...</div>
    <div class="loading-steps" id="loadingSteps">
      <div class="active" data-translate="step1">✓ Verifying KPP number...</div>
      <div data-translate="step2">⏳ Fetching land records...</div>
      <div data-translate="step3">⏳ Loading soil analysis data...</div>
      <div data-translate="step4">⏳ Retrieving crop history...</div>
      <div data-translate="step5">⏳ Preparing your dashboard...</div>
    </div>
  </div>
</div>

<!-- DASHBOARD SCREEN -->
<div class="screen" id="dashboard-screen">
  <div class="dash-header">
    <div>
      <div style="font-size:11px;color:var(--muted2)" data-translate="welcome">Welcome back 👋</div>
      <div class="dash-name" id="farmerName">Ramesh Kumar</div>
    </div>
    <div style="display:flex;gap:6px;align-items:center">
      <div class="uid-badge" id="dashKPP">KPP-123456</div>
      <div class="sub-badge" style="display:none" id="dashPro">⭐ PRO</div>
    </div>
  </div>

  <!-- NAV TABS -->
  <div class="nav-tabs">
    <button class="nav-tab active" onclick="switchTab('overview')"><i class="fas fa-home"></i> <span data-translate="tabOverview">Overview</span></button>
    <button class="nav-tab" onclick="switchTab('soil')"><i class="fas fa-vial"></i> <span data-translate="tabSoil">Soil Data</span></button>
    <button class="nav-tab" onclick="switchTab('crop')"><i class="fas fa-seedling"></i> <span data-translate="tabCrop">Crop Advisor</span></button>
    <button class="nav-tab" onclick="switchTab('yield')"><i class="fas fa-chart-line"></i> <span data-translate="tabYield">Yield</span></button>
    <button class="nav-tab" onclick="switchTab('weather')"><i class="fas fa-cloud-sun"></i> <span data-translate="tabWeather">Weather</span></button>
    <button class="nav-tab" onclick="switchTab('disease')"><i class="fas fa-bug"></i> <span data-translate="tabDisease">Disease AI</span></button>
    <button class="nav-tab" onclick="switchTab('schemes')"><i class="fas fa-landmark"></i> <span data-translate="tabSchemes">Schemes</span></button>
    <button class="nav-tab" onclick="switchTab('carbon')"><i class="fas fa-leaf"></i> <span data-translate="tabCarbon">Carbon</span></button>
    <button class="nav-tab" onclick="switchTab('market')"><i class="fas fa-store"></i> <span data-translate="tabMarket">Market</span></button>
    <button class="nav-tab" onclick="switchTab('profile')"><i class="fas fa-user"></i> <span data-translate="tabProfile">Profile</span></button>
  </div>

  <!-- TAB: OVERVIEW -->
  <div class="tab-content active" id="tab-overview">
    <div class="metrics-grid">
      <div class="metric-card">
        <div class="metric-icon">🌾</div>
        <div class="metric-val c-green" id="metricLand">3.5</div>
        <div class="metric-lbl" data-translate="metricLand">Acres (from KPP)</div>
      </div>
      <div class="metric-card">
        <div class="metric-icon">🧪</div>
        <div class="metric-val c-blue" id="metricSoil">Good</div>
        <div class="metric-lbl" data-translate="metricSoil">Soil Health</div>
      </div>
      <div class="metric-card">
        <div class="metric-icon">💰</div>
        <div class="metric-val c-gold">₹18,500</div>
        <div class="metric-lbl" data-translate="metricBenefits">Benefits Received</div>
      </div>
      <div class="metric-card">
        <div class="metric-icon">🌿</div>
        <div class="metric-val c-green">₹4,200</div>
        <div class="metric-lbl" data-translate="metricCarbon">Carbon Credits</div>
      </div>
    </div>

    <div class="sec-hd">
      <div class="sec-hd-title" data-translate="recentActivity">🕐 Recent Activity</div>
    </div>
    <div class="tx-list">
      <div class="tx-item">
        <div class="tx-icon credit">💰</div>
        <div class="tx-info">
          <div class="tx-desc" data-translate="tx1">PM-KISAN 18th Installment</div>
          <div class="tx-meta">14 Feb 2025</div>
        </div>
        <div class="tx-amount"><span class="tx-val tx-c">+₹2,000</span></div>
      </div>
      <div class="tx-item">
        <div class="tx-icon credit">🌿</div>
        <div class="tx-info">
          <div class="tx-desc" data-translate="tx2">Carbon Credit Payout Q3</div>
          <div class="tx-meta">8 Feb 2025 • Net after 10% fee</div>
        </div>
        <div class="tx-amount"><span class="tx-val tx-c">+₹3,780</span></div>
      </div>
      <div class="tx-item">
        <div class="tx-icon debit">💳</div>
        <div class="tx-info">
          <div class="tx-desc" data-translate="tx3">FarmTech Subscription</div>
          <div class="tx-meta">1 Jan 2025 • Annual Pro</div>
        </div>
        <div class="tx-amount"><span class="tx-val tx-d">-₹588</span></div>
      </div>
      <div class="tx-item">
        <div class="tx-icon credit">☔</div>
        <div class="tx-info">
          <div class="tx-desc" data-translate="tx4">PMFBY Crop Insurance Claim</div>
          <div class="tx-meta">20 Dec 2024</div>
        </div>
        <div class="tx-amount"><span class="tx-val tx-c">+₹12,720</span></div>
      </div>
    </div>
  </div>

  <!-- TAB: SOIL DATA -->
  <div class="tab-content" id="tab-soil">
    <div class="sec-hd">
      <div class="sec-hd-title" data-translate="soilTitle">🧪 Soil Analysis</div>
    </div>
    <div class="alert alert-success" style="margin:0 16px 14px">
      <i class="fas fa-check-circle"></i>
      <div data-translate="soilInfo">Data auto-fetched from Soil Health Card linked to your KPP</div>
    </div>

    <div class="card" style="margin:0 16px 14px">
      <div style="font-family:'Syne',sans-serif;font-weight:800;font-size:14px;margin-bottom:14px;color:var(--acc)" data-translate="npkTitle">NPK Values</div>
      <div style="display:grid;grid-template-columns:repeat(3,1fr);gap:12px">
        <div class="weather-item">
          <div class="weather-item-label" data-translate="nitrogen">Nitrogen (N)</div>
          <div class="weather-item-value c-green" id="soilN">245 ppm</div>
        </div>
        <div class="weather-item">
          <div class="weather-item-label" data-translate="phosphorus">Phosphorus (P)</div>
          <div class="weather-item-value c-gold" id="soilP">18 ppm</div>
        </div>
        <div class="weather-item">
          <div class="weather-item-label" data-translate="potassium">Potassium (K)</div>
          <div class="weather-item-value c-blue" id="soilK">180 ppm</div>
        </div>
      </div>
    </div>

    <div class="card" style="margin:0 16px 14px">
      <div style="font-family:'Syne',sans-serif;font-weight:800;font-size:14px;margin-bottom:14px;color:var(--acc)" data-translate="otherParams">Other Parameters</div>
      <div style="display:grid;grid-template-columns:repeat(2,1fr);gap:12px">
        <div class="weather-item">
          <div class="weather-item-label" data-translate="phLevel">pH Level</div>
          <div class="weather-item-value" id="soilPH">6.8</div>
        </div>
        <div class="weather-item">
          <div class="weather-item-label" data-translate="organicCarbon">Organic Carbon</div>
          <div class="weather-item-value" id="soilOC">0.52%</div>
        </div>
        <div class="weather-item">
          <div class="weather-item-label" data-translate="ec">EC (dS/m)</div>
          <div class="weather-item-value" id="soilEC">0.38</div>
        </div>
        <div class="weather-item">
          <div class="weather-item-label" data-translate="texture">Soil Texture</div>
          <div class="weather-item-value" id="soilTexture">Loamy</div>
        </div>
      </div>
    </div>
  </div>

  <!-- TAB: CROP ADVISOR -->
  <div class="tab-content" id="tab-crop">
    <div class="sec-hd">
      <div class="sec-hd-title" data-translate="cropTitle">🌱 AI Crop Advisor</div>
    </div>
    <div class="alert alert-success" style="margin:0 16px 14px">
      <i class="fas fa-robot"></i>
      <div data-translate="cropInfo">Recommendations based on your soil NPK data, weather, and regional crop performance</div>
    </div>

    <div style="margin:0 16px 14px">
      <button class="btn btn-primary btn-full" onclick="generateCropRecommendations()">
        <i class="fas fa-magic"></i> <span data-translate="generateRecs">Generate AI Recommendations</span>
      </button>
    </div>

    <div id="cropRecsDiv"></div>
  </div>

  <!-- TAB: YIELD -->
  <div class="tab-content" id="tab-yield">
    <div class="sec-hd">
      <div class="sec-hd-title" data-translate="yieldTitle">📊 AI Yield Prediction</div>
    </div>

    <div class="card" style="margin:0 16px 14px">
      <div class="field-group">
        <label class="field-label" data-translate="selectCrop">Select Crop</label>
        <select class="inp" id="yieldCrop">
          <option>Wheat</option>
          <option>Rice</option>
          <option>Corn</option>
          <option>Soybean</option>
          <option>Cotton</option>
        </select>
      </div>
      <div class="field-group">
        <label class="field-label" data-translate="irrigation">Irrigation Type</label>
        <select class="inp" id="yieldIrrigation">
          <option>Drip Irrigation</option>
          <option>Flood Irrigation</option>
          <option>Sprinkler</option>
          <option>Rainfed</option>
        </select>
      </div>
      <button class="btn btn-primary btn-full" onclick="predictYield()">
        <i class="fas fa-chart-line"></i> <span data-translate="predict">Predict Yield</span>
      </button>
    </div>

    <div id="yieldResultDiv"></div>
  </div>

  <!-- TAB: WEATHER -->
  <div class="tab-content" id="tab-weather">
    <div class="sec-hd">
      <div class="sec-hd-title" data-translate="weatherTitle">🌤️ Weather Forecast</div>
    </div>
    
    <div style="margin:0 16px 14px">
      <button class="btn btn-primary btn-full" onclick="loadWeather()">
        <i class="fas fa-sync"></i> <span data-translate="refreshWeather">Refresh Weather Data</span>
      </button>
    </div>

    <div id="weatherDataDiv"></div>
  </div>

  <!-- TAB: DISEASE -->
  <div class="tab-content" id="tab-disease">
    <div class="sec-hd">
      <div class="sec-hd-title" data-translate="diseaseTitle">🐛 AI Disease Detection</div>
    </div>

    <div class="card" style="margin:0 16px 14px">
      <div class="field-group">
        <label class="field-label" data-translate="uploadImage">Upload Crop Leaf Image</label>
        <input type="file" class="inp" id="diseaseImage" accept="image/*" onchange="previewDiseaseImage(event)" style="padding:10px">
      </div>
      <img id="diseasePreview" style="width:100%;max-height:300px;object-fit:contain;border-radius:var(--radius-xs);margin-bottom:14px;display:none;border:1px solid var(--bdr)" alt="Preview">
      <button class="btn btn-primary btn-full" id="analyzeBtn" disabled onclick="analyzeDisease()">
        <i class="fas fa-search"></i> <span data-translate="analyze">Analyze Image</span>
      </button>
    </div>

    <div id="diseaseResultDiv"></div>
  </div>

  <!-- TAB: SCHEMES -->
  <div class="tab-content" id="tab-schemes">
    <div class="sec-hd">
      <div class="sec-hd-title" data-translate="schemesTitle">📋 Government Schemes</div>
    </div>

    <div style="padding:0 16px 14px">
      <input class="inp" placeholder="🔍 Search schemes..." oninput="filterSchemes(this.value)">
    </div>

    <div id="schemesList">
      <!-- Carbon Credit -->
      <div class="scheme-card card-green scheme-item">
        <div class="badge-new">NEW 2025</div>
        <div class="scheme-header">
          <div class="scheme-icon-box" style="background:rgba(34,197,94,.12)">🌿</div>
          <div>
            <div class="scheme-name">Budget 2025-26 Carbon Credit Programme</div>
            <div class="scheme-ministry">Ministry of Agriculture & Environment</div>
            <div class="scheme-amount">₹500-₹2,500/acre/year</div>
          </div>
        </div>
        <div class="scheme-desc">India's landmark farmer carbon credit initiative. Earn verified carbon credits for sustainable farming practices.</div>
        <div class="scheme-footer">
          <div class="tag tag-green">Carbon</div>
          <div class="tag tag-gold">Income</div>
          <button class="btn btn-primary btn-sm" onclick="applyScheme('Carbon Credit')">Apply →</button>
        </div>
      </div>

      <!-- PM-KISAN -->
      <div class="scheme-card scheme-item">
        <div class="scheme-header">
          <div class="scheme-icon-box" style="background:rgba(245,158,11,.12)">💰</div>
          <div>
            <div class="scheme-name">PM-KISAN</div>
            <div class="scheme-ministry">Ministry of Agriculture</div>
            <div class="scheme-amount">₹6,000/year (3 installments)</div>
          </div>
        </div>
        <div class="scheme-desc">Direct income support to all landholding farmers. Amount credited directly to bank.</div>
        <div class="scheme-footer">
          <div class="tag tag-gold">Income</div>
          <div class="tag tag-green">DBT</div>
          <button class="btn btn-primary btn-sm" onclick="applyScheme('PM-KISAN')">Apply →</button>
        </div>
      </div>

      <!-- PMFBY -->
      <div class="scheme-card scheme-item">
        <div class="scheme-header">
          <div class="scheme-icon-box" style="background:rgba(56,189,248,.12)">☔</div>
          <div>
            <div class="scheme-name">PMFBY Crop Insurance</div>
            <div class="scheme-ministry">Ministry of Agriculture</div>
            <div class="scheme-amount">Up to ₹2 lakh coverage</div>
          </div>
        </div>
        <div class="scheme-desc">Comprehensive crop insurance for weather, pest & disease damage. Low premium 1.5-5%.</div>
        <div class="scheme-footer">
          <div class="tag tag-blue">Insurance</div>
          <button class="btn btn-primary btn-sm" onclick="applyScheme('PMFBY')">Apply →</button>
        </div>
      </div>

      <!-- KCC -->
      <div class="scheme-card scheme-item">
        <div class="scheme-header">
          <div class="scheme-icon-box" style="background:rgba(167,139,250,.12)">💳</div>
          <div>
            <div class="scheme-name">Kisan Credit Card</div>
            <div class="scheme-ministry">Department of Financial Services</div>
            <div class="scheme-amount">₹1.6L-₹3L @ 4% interest</div>
          </div>
        </div>
        <div class="scheme-desc">Low-interest credit for farm inputs, maintenance & post-harvest expenses.</div>
        <div class="scheme-footer">
          <div class="tag tag-purple">Credit</div>
          <button class="btn btn-primary btn-sm" onclick="applyScheme('KCC')">Apply →</button>
        </div>
      </div>

      <!-- PMKSY -->
      <div class="scheme-card scheme-item">
        <div class="scheme-header">
          <div class="scheme-icon-box" style="background:rgba(56,189,248,.12)">💧</div>
          <div>
            <div class="scheme-name">PM Krishi Sinchayee Yojana</div>
            <div class="scheme-ministry">Ministry of Jal Shakti</div>
            <div class="scheme-amount">55-65% subsidy on irrigation</div>
          </div>
        </div>
        <div class="scheme-desc">Subsidy on drip & sprinkler irrigation systems. Save water, increase yield.</div>
        <div class="scheme-footer">
          <div class="tag tag-blue">Water</div>
          <div class="tag tag-green">Subsidy</div>
          <button class="btn btn-primary btn-sm" onclick="applyScheme('PMKSY')">Apply →</button>
        </div>
      </div>

      <!-- Soil Health Card -->
      <div class="scheme-card scheme-item">
        <div class="scheme-header">
          <div class="scheme-icon-box" style="background:rgba(34,197,94,.12)">🔬</div>
          <div>
            <div class="scheme-name">Soil Health Card Scheme</div>
            <div class="scheme-ministry">Department of Agriculture</div>
            <div class="scheme-amount">Free soil testing</div>
          </div>
        </div>
        <div class="scheme-desc">Free soil health card every 2 years with fertilizer recommendations.</div>
        <div class="scheme-footer">
          <div class="tag tag-green">Free</div>
          <button class="btn btn-primary btn-sm" onclick="applyScheme('Soil Health')">Apply →</button>
        </div>
      </div>
    </div>
  </div>

  <!-- TAB: CARBON -->
  <div class="tab-content" id="tab-carbon">
    <div class="carbon-hero">
      <div class="tag tag-green" style="margin-bottom:12px">🌿 UNION BUDGET 2025-26 — LIVE</div>
      <div class="carbon-title">Carbon Credit Programme for Farmers</div>
      <div class="carbon-sub">Earn ₹500-₹2,500 per acre annually by practicing sustainable farming. Credits verified by NABARD-empanelled agencies.</div>
      <div class="carbon-stats">
        <div class="carbon-stat">
          <div class="carbon-stat-val">₹500</div>
          <div class="carbon-stat-lbl">Min/acre/yr</div>
        </div>
        <div class="carbon-stat">
          <div class="carbon-stat-val">₹2,500</div>
          <div class="carbon-stat-lbl">Max/acre/yr</div>
        </div>
        <div class="carbon-stat">
          <div class="carbon-stat-val">90%</div>
          <div class="carbon-stat-lbl">Your Share</div>
        </div>
      </div>
      <button class="btn btn-primary btn-full" onclick="enrollCarbon()">
        <i class="fas fa-leaf"></i> <span data-translate="enrollCarbon">Enroll in Carbon Programme</span>
      </button>
    </div>

    <div class="alert alert-warn" style="margin:0 16px 14px">
      <i class="fas fa-exclamation-triangle"></i>
      <div><strong>10% Processing Fee:</strong> When carbon credits are sold, 10% is deducted as FarmTech's certification & processing fee. You receive 90% net in your bank.</div>
    </div>

    <div class="card" style="margin:0 16px 14px">
      <div style="font-family:'Syne',sans-serif;font-weight:800;font-size:14px;margin-bottom:12px;color:var(--acc)" data-translate="carbonCalc">Carbon Credit Calculator</div>
      <div class="field-group">
        <label class="field-label" data-translate="yourLand">Your Farm Area (acres)</label>
        <input type="number" class="inp" id="ccAcres" placeholder="e.g. 5" oninput="calcCarbon()">
      </div>
      <div class="field-group">
        <label class="field-label" data-translate="practice">Farming Practice</label>
        <select class="inp" id="ccPractice" onchange="calcCarbon()">
          <option value="0">Select practice...</option>
          <option value="800">Zero Tillage — ₹800/acre/yr</option>
          <option value="1200">Organic Farming — ₹1,200/acre/yr</option>
          <option value="2000">Agroforestry — ₹2,000/acre/yr</option>
          <option value="2500">Multiple Practices — ₹2,500/acre/yr</option>
        </select>
      </div>
      <div id="ccResult" style="display:none;background:rgba(34,197,94,.06);border:1px solid rgba(34,197,94,.2);border-radius:var(--radius-xs);padding:14px;font-size:13px">
        <div style="display:flex;justify-content:space-between;margin-bottom:6px">
          <span>Gross Amount:</span>
          <span id="ccGross" style="font-weight:800;color:var(--gold)">₹0</span>
        </div>
        <div style="display:flex;justify-content:space-between;margin-bottom:6px">
          <span>10% FarmTech Fee:</span>
          <span id="ccFee" style="font-weight:800;color:var(--red)">₹0</span>
        </div>
        <div style="display:flex;justify-content:space-between;border-top:1px solid var(--bdr);padding-top:8px;margin-top:8px">
          <span style="font-weight:800">Your Net Payout:</span>
          <span id="ccNet" style="font-weight:900;color:var(--acc);font-size:16px">₹0</span>
        </div>
      </div>
    </div>
  </div>

  <!-- TAB: MARKET -->
  <div class="tab-content" id="tab-market">
    <div class="sec-hd">
      <div class="sec-hd-title" data-translate="marketTitle">📊 Live Mandi Prices</div>
    </div>

    <div class="alert alert-info" style="margin:0 16px 14px">
      <i class="fas fa-info-circle"></i>
      <div data-translate="marketDisclaimer">Indicative prices. Verify with your local APMC before selling.</div>
    </div>

    <div style="display:grid;grid-template-columns:repeat(auto-fill,minmax(145px,1fr));gap:10px;padding:0 16px 16px">
      <div class="card" style="padding:14px;margin:0;text-align:center">
        <div style="font-size:20px;margin-bottom:4px">🌾</div>
        <div style="font-size:12px;font-weight:700;margin-bottom:4px">Wheat</div>
        <div style="font-size:22px;font-weight:900;color:var(--acc);margin:4px 0">₹2,320</div>
        <div style="font-size:10px;color:var(--muted)">per quintal</div>
        <div style="font-size:12px;font-weight:700;color:var(--acc);margin-top:4px">▲ +1.2%</div>
      </div>
      <div class="card" style="padding:14px;margin:0;text-align:center">
        <div style="font-size:20px;margin-bottom:4px">🌾</div>
        <div style="font-size:12px;font-weight:700;margin-bottom:4px">Paddy</div>
        <div style="font-size:22px;font-weight:900;color:var(--acc);margin:4px 0">₹2,310</div>
        <div style="font-size:10px;color:var(--muted)">per quintal</div>
        <div style="font-size:12px;font-weight:700;color:var(--red);margin-top:4px">▼ -0.5%</div>
      </div>
      <div class="card" style="padding:14px;margin:0;text-align:center">
        <div style="font-size:20px;margin-bottom:4px">🟡</div>
        <div style="font-size:12px;font-weight:700;margin-bottom:4px">Chana Dal</div>
        <div style="font-size:22px;font-weight:900;color:var(--gold);margin:4px 0">₹5,850</div>
        <div style="font-size:10px;color:var(--muted)">per quintal</div>
        <div style="font-size:12px;font-weight:700;color:var(--acc);margin-top:4px">▲ +2.8%</div>
      </div>
      <div class="card" style="padding:14px;margin:0;text-align:center">
        <div style="font-size:20px;margin-bottom:4px">🌻</div>
        <div style="font-size:12px;font-weight:700;margin-bottom:4px">Soybean</div>
        <div style="font-size:22px;font-weight:900;color:var(--acc);margin:4px 0">₹4,320</div>
        <div style="font-size:10px;color:var(--muted)">per quintal</div>
        <div style="font-size:12px;font-weight:700;color:var(--acc);margin-top:4px">▲ +0.8%</div>
      </div>
    </div>
  </div>

  <!-- TAB: PROFILE -->
  <div class="tab-content" id="tab-profile">
    <div class="profile-card">
      <div class="profile-header">
        <div class="profile-avatar">👨‍🌾</div>
        <div style="flex:1">
          <div class="profile-name" id="profileName">Ramesh Kumar</div>
          <div style="font-size:12px;color:var(--muted2);margin-bottom:6px">Farmer • Punjab</div>
          <div class="profile-uid" id="profileKPP">KPP-123456</div>
          <div class="profile-tags">
            <div class="tag tag-gold">⭐ Pro</div>
            <div class="tag tag-green">🌿 Carbon Enrolled</div>
          </div>
        </div>
      </div>
    </div>

    <div class="card" style="margin:0 16px 14px">
      <div style="font-family:'Syne',sans-serif;font-weight:800;font-size:14px;margin-bottom:12px;color:var(--acc)" data-translate="subscription">Subscription Status</div>
      <div style="display:flex;flex-direction:column;gap:10px;font-size:13px">
        <div style="display:flex;justify-content:space-between">
          <span style="color:var(--muted2)">Plan</span>
          <span class="tag tag-gold">⭐ Pro — Full Access</span>
        </div>
        <div style="display:flex;justify-content:space-between">
          <span style="color:var(--muted2)">Valid Till</span>
          <span style="font-weight:700">31 Dec 2025</span>
        </div>
        <div style="display:flex;justify-content:space-between">
          <span style="color:var(--muted2)">Annual Cost</span>
          <span style="font-weight:700;color:var(--gold)">₹499 + GST</span>
        </div>
        <div style="display:flex;justify-content:space-between">
          <span style="color:var(--muted2)">Carbon Fee</span>
          <span style="font-weight:700;color:var(--orange)">10% of credits</span>
        </div>
      </div>
    </div>

    <div style="margin:0 16px 14px;display:flex;gap:10px">
      <button class="btn btn-outline btn-sm" onclick="showToast('Support: 1800-180-1551')">
        <i class="fas fa-phone"></i> Support
      </button>
      <button class="btn btn-danger btn-sm" onclick="logoutUser()">
        <i class="fas fa-sign-out-alt"></i> Logout
      </button>
    </div>
  </div>

</div><!-- end dashboard -->

</div><!-- end screens -->

<!-- APPLY MODAL -->
<div class="modal-overlay hidden" id="applyModal">
  <div class="modal-sheet">
    <div class="modal-title" id="modalTitle">Apply for Scheme</div>
    <div class="modal-sub" id="modalSub">Your profile data will be auto-filled</div>

    <div class="field-group">
      <label class="field-label">Purpose / Use</label>
      <select class="inp">
        <option>Regular annual benefit</option>
        <option>New season application</option>
        <option>Status check & update</option>
      </select>
    </div>

    <div class="field-group">
      <label class="field-label">Season / Year</label>
      <select class="inp">
        <option>Rabi 2024-25</option>
        <option>Kharif 2025</option>
        <option>Annual 2025-26</option>
      </select>
    </div>

    <div class="alert alert-info">
      <i class="fas fa-lock"></i>
      <div>Your Aadhaar, land records & bank details from KPP will be auto-attached</div>
    </div>

    <button class="btn btn-primary btn-full" onclick="submitApplication()">
      <i class="fas fa-paper-plane"></i> Submit Application
    </button>
    <button class="btn btn-ghost btn-full btn-sm" style="margin-top:10px" onclick="closeModal()">Cancel</button>
  </div>
</div>

<!-- TOAST -->
<div id="toast"></div>

<script>
// ===== TRANSLATIONS =====
const translations = {
  en: {
    heroTitle: "FarmTech Pro",
    heroSub: "India's first KPP-enabled digital farming platform. Auto-fetch your data, apply for schemes, earn carbon credits.",
    loginKPP: "Login with KPP",
    tryDemo: "Try Demo",
    kppTitle: "Kisan Pehchaan Patra Login",
    kppSubtitle: "Enter your 14-digit KPP number to auto-fetch all your farm data from government database",
    kppInfo: "Soil data, NPK values, land records, and crop history will be automatically loaded",
    kppLabel: "Kisan Pehchaan Patra Number *",
    mobileLabel: "Mobile Number (Linked to KPP) *",
    fetchLogin: "Fetch Data & Login",
    backHome: "Back to Home",
    demoNotice: "Demo Mode: Enter any 14-digit number to see simulated auto-fetched data",
    loadingText: "Fetching your data from KPP database...",
    step1: "✓ Verifying KPP number...",
    step2: "⏳ Fetching land records...",
    step3: "⏳ Loading soil analysis data...",
    step4: "⏳ Retrieving crop history...",
    step5: "⏳ Preparing your dashboard...",
    welcome: "Welcome back 👋",
    tabOverview: "Overview",
    tabSoil: "Soil Data",
    tabCrop: "Crop Advisor",
    tabYield: "Yield",
    tabWeather: "Weather",
    tabDisease: "Disease AI",
    tabSchemes: "Schemes",
    tabCarbon: "Carbon",
    tabMarket: "Market",
    tabProfile: "Profile",
    metricLand: "Acres (from KPP)",
    metricSoil: "Soil Health",
    metricBenefits: "Benefits Received",
    metricCarbon: "Carbon Credits",
    recentActivity: "🕐 Recent Activity",
    soilTitle: "🧪 Soil Analysis",
    soilInfo: "Data auto-fetched from Soil Health Card linked to your KPP",
    npkTitle: "NPK Values",
    nitrogen: "Nitrogen (N)",
    phosphorus: "Phosphorus (P)",
    potassium: "Potassium (K)",
    otherParams: "Other Parameters",
    phLevel: "pH Level",
    organicCarbon: "Organic Carbon",
    ec: "EC (dS/m)",
    texture: "Soil Texture",
    cropTitle: "🌱 AI Crop Advisor",
    cropInfo: "Recommendations based on your soil NPK data, weather, and regional crop performance",
    generateRecs: "Generate AI Recommendations",
    yieldTitle: "📊 AI Yield Prediction",
    selectCrop: "Select Crop",
    irrigation: "Irrigation Type",
    predict: "Predict Yield",
    weatherTitle: "🌤️ Weather Forecast",
    refreshWeather: "Refresh Weather Data",
    diseaseTitle: "🐛 AI Disease Detection",
    uploadImage: "Upload Crop Leaf Image",
    analyze: "Analyze Image",
    schemesTitle: "📋 Government Schemes",
    marketTitle: "📊 Live Mandi Prices",
    marketDisclaimer: "Indicative prices. Verify with your local APMC before selling.",
    subscription: "Subscription Status",
    enrollCarbon: "Enroll in Carbon Programme",
    carbonCalc: "Carbon Credit Calculator",
    yourLand: "Your Farm Area (acres)",
    practice: "Farming Practice",
    tx1: "PM-KISAN 18th Installment",
    tx2: "Carbon Credit Payout Q3",
    tx3: "FarmTech Subscription",
    tx4: "PMFBY Crop Insurance Claim"
  },
  hi: {
    heroTitle: "फार्मटेक प्रो",
    heroSub: "भारत का पहला KPP-सक्षम डिजिटल कृषि मंच। अपना डेटा स्वतः प्राप्त करें, योजनाओं के लिए आवेदन करें, कार्बन क्रेडिट अर्जित करें।",
    loginKPP: "KPP से लॉगिन करें",
    tryDemo: "डेमो आज़माएं",
    kppTitle: "किसान पहचान पत्र लॉगिन",
    kppSubtitle: "अपने सभी खेती डेटा को स्वचालित रूप से प्राप्त करने के लिए अपना 14 अंकों का KPP नंबर दर्ज करें",
    welcome: "वापसी पर स्वागत है 👋",
    tabOverview: "अवलोकन",
    tabSoil: "मिट्टी डेटा",
    tabCrop: "फसल सलाहकार",
    tabYield: "उपज",
    tabWeather: "मौसम",
    tabDisease: "रोग AI",
    tabSchemes: "योजनाएं",
    tabCarbon: "कार्बन",
    tabMarket: "बाजार",
    tabProfile: "प्रोफ़ाइल"
  },
  pa: {
    heroTitle: "ਫਾਰਮਟੈਕ ਪ੍ਰੋ",
    heroSub: "ਭਾਰਤ ਦਾ ਪਹਿਲਾ KPP-ਸਮਰੱਥ ਡਿਜੀਟਲ ਖੇਤੀਬਾੜੀ ਪਲੇਟਫਾਰਮ।",
    loginKPP: "KPP ਨਾਲ ਲੌਗਇਨ ਕਰੋ",
    tryDemo: "ਡੈਮੋ ਅਜ਼ਮਾਓ",
    welcome: "ਵਾਪਸੀ 'ਤੇ ਸੁਆਗਤ ਹੈ 👋",
    tabOverview: "ਸੰਖੇਪ",
    tabSoil: "ਮਿੱਟੀ ਡੇਟਾ",
    tabCrop: "ਫਸਲ ਸਲਾਹਕਾਰ",
    tabYield: "ਉਪਜ",
    tabWeather: "ਮੌਸਮ",
    tabDisease: "ਰੋਗ AI",
    tabSchemes: "ਯੋਜਨਾਵਾਂ",
    tabCarbon: "ਕਾਰਬਨ",
    tabMarket: "ਮਾਰਕਿਟ",
    tabProfile: "ਪ੍ਰੋਫਾਈਲ"
  }
};

// ===== STATE =====
let currentLang = 'en';
let userProfile = null;
let farmerData = null;

// ===== UTILITY =====
function formatKPP(input) {
  let value = input.value.replace(/\D/g, '');
  if (value.length > 2) value = value.slice(0, 2) + '-' + value.slice(2);
  if (value.length > 7) value = value.slice(0, 7) + '-' + value.slice(7);
  if (value.length > 12) value = value.slice(0, 12) + '-' + value.slice(12);
  input.value = value.slice(0, 17);
}

function changeLanguage(lang) {
  currentLang = lang;
  document.querySelectorAll('[data-translate]').forEach(el => {
    const key = el.getAttribute('data-translate');
    if (translations[lang] && translations[lang][key]) {
      if (el.tagName === 'INPUT') el.placeholder = translations[lang][key];
      else el.textContent = translations[lang][key];
    }
  });
  showToast(`Language changed to ${lang === 'en' ? 'English' : lang === 'hi' ? 'हिंदी' : 'ਪੰਜਾਬੀ'}`);
}

function showScreen(name) {
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  const screen = document.getElementById(name + '-screen');
  if (screen) screen.classList.add('active');
  window.scrollTo(0, 0);
}

function switchTab(name) {
  document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
  document.querySelectorAll('.nav-tab').forEach(t => t.classList.remove('active'));
  const tab = document.getElementById('tab-' + name);
  if (tab) tab.classList.add('active');
  document.querySelectorAll('.nav-tab').forEach(t => {
    if (t.textContent.toLowerCase().includes(name.substring(0, 4))) t.classList.add('active');
  });
}

let toastTimer;
function showToast(msg) {
  const toast = document.getElementById('toast');
  toast.textContent = msg;
  toast.classList.add('show');
  clearTimeout(toastTimer);
  toastTimer = setTimeout(() => toast.classList.remove('show'), 3500);
}

// ===== DEMO =====
function loadDemo() {
  document.getElementById('kppNumber').value = '12-3456-7890-123';
  document.getElementById('mobileNumber').value = '9876543210';
  showScreen('kpp-login');
  showToast('Demo data pre-filled');
}

// ===== KPP LOGIN =====
function handleKPPLogin(e) {
  e.preventDefault();
  const kppNumber = document.getElementById('kppNumber').value;
  const mobile = document.getElementById('mobileNumber').value;
  
  if (!kppNumber || !mobile) {
    showToast('Please fill all fields');
    return;
  }
  
  showScreen('loading');
  
  // Animate loading steps
  const steps = document.querySelectorAll('.loading-steps div');
  steps.forEach((step, i) => {
    setTimeout(() => {
      steps.forEach(s => s.classList.remove('active'));
      step.classList.add('active');
    }, (i + 1) * 800);
  });
  
  // Generate mock data
  setTimeout(() => {
    const names = ['Ramesh Kumar', 'Suresh Singh', 'Raj Patel', 'Mahesh Verma'];
    const districts = ['Ludhiana', 'Amritsar', 'Jalandhar', 'Patiala'];
    
    const randomIndex = Math.floor(Math.random() * names.length);
    
    farmerData = {
      kppNumber,
      name: names[randomIndex],
      district: districts[randomIndex],
      state: 'Punjab',
      landArea: (2 + Math.random() * 6).toFixed(1),
      mobile,
      soil: {
        n: Math.round(200 + Math.random() * 100),
        p: Math.round(15 + Math.random() * 15),
        k: Math.round(150 + Math.random() * 80),
        ph: (6 + Math.random() * 1.5).toFixed(1),
        oc: (0.3 + Math.random() * 0.4).toFixed(2),
        ec: (0.2 + Math.random() * 0.3).toFixed(2),
        texture: ['Loamy', 'Sandy', 'Clay'][Math.floor(Math.random() * 3)]
      },
      soilHealth: ['Excellent', 'Good', 'Fair'][Math.floor(Math.random() * 3)]
    };
    
    userProfile = farmerData;
    
    // Update UI
    document.getElementById('farmerName').textContent = farmerData.name;
    document.getElementById('profileName').textContent = farmerData.name;
    document.getElementById('topbar-uid').textContent = kppNumber;
    document.getElementById('topbar-uid').style.display = 'block';
    document.getElementById('dashKPP').textContent = kppNumber;
    document.getElementById('profileKPP').textContent = kppNumber;
    document.getElementById('topbar-sub').style.display = 'block';
    document.getElementById('dashPro').style.display = 'inline-flex';
    document.getElementById('topbar-login-btn').style.display = 'none';
    document.getElementById('topbar-logout-btn').style.display = 'flex';
    
    // Update metrics
    document.getElementById('metricLand').textContent = farmerData.landArea;
    document.getElementById('metricSoil').textContent = farmerData.soilHealth;
    
    // Update soil data
    document.getElementById('soilN').textContent = farmerData.soil.n + ' ppm';
    document.getElementById('soilP').textContent = farmerData.soil.p + ' ppm';
    document.getElementById('soilK').textContent = farmerData.soil.k + ' ppm';
    document.getElementById('soilPH').textContent = farmerData.soil.ph;
    document.getElementById('soilOC').textContent = farmerData.soil.oc + '%';
    document.getElementById('soilEC').textContent = farmerData.soil.ec;
    document.getElementById('soilTexture').textContent = farmerData.soil.texture;
    
    showScreen('dashboard');
    showToast(`Welcome, ${farmerData.name}!`);
  }, 5000);
}

// ===== LOGOUT =====
function logoutUser() {
  userProfile = null;
  farmerData = null;
  document.getElementById('topbar-uid').style.display = 'none';
  document.getElementById('topbar-sub').style.display = 'none';
  document.getElementById('topbar-login-btn').style.display = 'flex';
  document.getElementById('topbar-logout-btn').style.display = 'none';
  showScreen('landing');
  showToast('Logged out successfully');
}

// ===== CROP RECOMMENDATIONS =====
function generateCropRecommendations() {
  if (!farmerData) return;
  
  const container = document.getElementById('cropRecsDiv');
  container.innerHTML = '<div class="loading" style="padding:40px 20px"><div class="spinner"></div><div class="loading-text">Generating AI recommendations...</div></div>';
  
  setTimeout(() => {
    const N = farmerData.soil.n;
    const pH = parseFloat(farmerData.soil.ph);
    
    const crops = [
      { name: 'Wheat', confidence: Math.min(95, 70 + (N > 200 ? 15 : 0)), yield: `${(4.5 + Math.random()).toFixed(1)}-${(5.5 + Math.random()).toFixed(1)}`, reason: 'Excellent NPK balance and pH for wheat' },
      { name: 'Rice', confidence: Math.min(90, 65 + (pH > 6.5 ? 15 : 0)), yield: `${(5 + Math.random()).toFixed(1)}-${(6 + Math.random()).toFixed(1)}`, reason: 'Good water retention and suitable pH' },
      { name: 'Corn', confidence: Math.min(85, 60 + (N > 220 ? 15 : 0)), yield: `${(7 + Math.random()).toFixed(1)}-${(8.5 + Math.random()).toFixed(1)}`, reason: 'Adequate nitrogen for corn growth' }
    ];
    
    let html = '';
    crops.forEach((crop, i) => {
      const confClass = crop.confidence >= 85 ? 'card-green' : 'card';
      html += `
        <div class="${confClass}" style="margin:0 16px 12px">
          <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:10px">
            <div style="font-family:'Syne',sans-serif;font-size:16px;font-weight:900">${crop.name}</div>
            <div class="tag tag-${crop.confidence >= 85 ? 'green' : 'gold'}">${crop.confidence}% Match</div>
          </div>
          <div style="font-size:13px;color:var(--muted2);margin-bottom:8px">Expected Yield: <strong style="color:var(--acc)">${crop.yield} t/ha</strong></div>
          <div style="font-size:12px;color:var(--txt2)">${crop.reason}</div>
        </div>
      `;
    });
    
    container.innerHTML = html;
  }, 2000);
}

// ===== YIELD PREDICTION =====
function predictYield() {
  if (!farmerData) return;
  
  const crop = document.getElementById('yieldCrop').value;
  const irrigation = document.getElementById('yieldIrrigation').value;
  
  const container = document.getElementById('yieldResultDiv');
  container.innerHTML = '<div class="loading" style="padding:40px 20px"><div class="spinner"></div><div class="loading-text">Running AI models...</div></div>';
  
  setTimeout(() => {
    const baseYield = { 'Wheat': 4.5, 'Rice': 5.5, 'Corn': 8.0, 'Soybean': 3.0, 'Cotton': 2.8 };
    const irrigationMult = { 'Drip Irrigation': 1.25, 'Flood Irrigation': 1.0, 'Sprinkler': 1.15, 'Rainfed': 0.75 };
    
    const predicted = (baseYield[crop] * irrigationMult[irrigation]).toFixed(2);
    const total = (predicted * farmerData.landArea).toFixed(2);
    
    container.innerHTML = `
      <div class="card card-green" style="margin:0 16px 14px">
        <div style="font-family:'Syne',sans-serif;font-size:16px;font-weight:900;margin-bottom:14px;color:var(--acc)">Prediction Results</div>
        <div style="display:grid;grid-template-columns:repeat(2,1fr);gap:12px">
          <div class="weather-item">
            <div class="weather-item-label">Predicted Yield</div>
            <div class="weather-item-value c-green">${predicted} t/ha</div>
          </div>
          <div class="weather-item">
            <div class="weather-item-label">Total Production</div>
            <div class="weather-item-value c-gold">${total} tons</div>
          </div>
        </div>
      </div>
    `;
  }, 2000);
}

// ===== WEATHER =====
function loadWeather() {
  if (!farmerData) return;
  
  const container = document.getElementById('weatherDataDiv');
  container.innerHTML = '<div class="loading" style="padding:40px 20px"><div class="spinner"></div><div class="loading-text">Fetching weather...</div></div>';
  
  setTimeout(() => {
    const temp = Math.round(20 + Math.random() * 15);
    const humidity = Math.round(50 + Math.random() * 30);
    
    container.innerHTML = `
      <div class="weather-card">
        <div class="weather-icon">☀️</div>
        <div class="weather-temp">${temp}°C</div>
        <div class="weather-condition">Clear Sky</div>
        <div class="weather-grid">
          <div class="weather-item">
            <div class="weather-item-label">Humidity</div>
            <div class="weather-item-value c-blue">${humidity}%</div>
          </div>
          <div class="weather-item">
            <div class="weather-item-label">Wind Speed</div>
            <div class="weather-item-value">12 km/h</div>
          </div>
          <div class="weather-item">
            <div class="weather-item-label">Rainfall</div>
            <div class="weather-item-value c-green">0 mm</div>
          </div>
        </div>
      </div>
    `;
  }, 1500);
}

// ===== DISEASE DETECTION =====
function previewDiseaseImage(e) {
  const file = e.target.files[0];
  if (file) {
    const reader = new FileReader();
    reader.onload = function(event) {
      document.getElementById('diseasePreview').src = event.target.result;
      document.getElementById('diseasePreview').style.display = 'block';
      document.getElementById('analyzeBtn').disabled = false;
    };
    reader.readAsDataURL(file);
  }
}

function analyzeDisease() {
  const container = document.getElementById('diseaseResultDiv');
  container.innerHTML = '<div class="loading" style="padding:40px 20px"><div class="spinner"></div><div class="loading-text">Analyzing image...</div></div>';
  
  setTimeout(() => {
    const diseases = ['Healthy', 'Early Blight', 'Late Blight', 'Leaf Spot'];
    const disease = diseases[Math.floor(Math.random() * diseases.length)];
    const confidence = Math.round(75 + Math.random() * 20);
    
    container.innerHTML = `
      <div class="card ${disease === 'Healthy' ? 'card-green' : ''}" style="margin:0 16px 14px">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:10px">
          <div style="font-family:'Syne',sans-serif;font-size:16px;font-weight:900">${disease}</div>
          <div class="tag tag-${disease === 'Healthy' ? 'green' : 'gold'}">${confidence}% Confidence</div>
        </div>
        <div class="alert alert-${disease === 'Healthy' ? 'success' : 'warn'}">
          <i class="fas fa-${disease === 'Healthy' ? 'check-circle' : 'exclamation-triangle'}"></i>
          <div>${disease === 'Healthy' ? 'Your crop appears healthy!' : 'Disease detected. Treatment recommended.'}</div>
        </div>
      </div>
    `;
  }, 2500);
}

// ===== SCHEMES =====
function filterSchemes(query) {
  const items = document.querySelectorAll('.scheme-item');
  items.forEach(item => {
    const text = item.textContent.toLowerCase();
    item.style.display = text.includes(query.toLowerCase()) ? '' : 'none';
  });
}

function applyScheme(name) {
  if (!userProfile) {
    showToast('Please login first');
    return;
  }
  document.getElementById('modalTitle').textContent = `Apply for ${name}`;
  document.getElementById('applyModal').classList.remove('hidden');
}

function closeModal() {
  document.getElementById('applyModal').classList.add('hidden');
}

function submitApplication() {
  closeModal();
  showToast('✅ Application submitted! Reference: FT-' + Math.floor(10000 + Math.random() * 90000));
}

document.getElementById('applyModal').addEventListener('click', function(e) {
  if (e.target === this) closeModal();
});

// ===== CARBON =====
function enrollCarbon() {
  if (!userProfile) {
    showToast('Please login first');
    return;
  }
  showToast('✅ Enrolled in Carbon Credit Programme! Audit scheduled within 30 days.');
}

function calcCarbon() {
  const acres = parseFloat(document.getElementById('ccAcres').value) || 0;
  const rate = parseFloat(document.getElementById('ccPractice').value) || 0;
  const result = document.getElementById('ccResult');
  
  if (acres > 0 && rate > 0) {
    const gross = acres * rate;
    const fee = gross * 0.1;
    const net = gross - fee;
    
    document.getElementById('ccGross').textContent = '₹' + gross.toLocaleString('en-IN');
    document.getElementById('ccFee').textContent = '₹' + fee.toLocaleString('en-IN');
    document.getElementById('ccNet').textContent = '₹' + net.toLocaleString('en-IN');
    result.style.display = 'block';
  } else {
    result.style.display = 'none';
  }
}

// ===== INIT =====
showScreen('landing');
</script>
</body>
</html>
