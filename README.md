<html lang="pt-BR">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>ViabiCalc — Viabilidade Imobiliária</title>
<link rel="preconnect" href="https://fonts.googleapis.com"/>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;500;600;700&family=DM+Sans:wght@300;400;500&family=DM+Mono:wght@400;500&display=swap" rel="stylesheet"/>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
*{box-sizing:border-box;margin:0;padding:0}
:root{
  --bg:#F2F0E8;--surface:#FFFFFF;--surface2:#ECEAE0;--surface3:#E4E1D4;
  --border:rgba(40,35,20,0.10);--border2:rgba(40,35,20,0.20);
  --text:#1C1A10;--text2:#6A6554;--text3:#A09C88;
  --green:#1B5E38;--green2:#E8F3EC;--green3:#2D7A50;--green4:#0F3D22;
  --warn:#B05A00;--warnbg:#FDF0DC;
  --danger:#B02020;--dangerbg:#FDEAEA;
  --gold:#7A5800;--goldbg:#FDF3D0;
  --radius:14px;--radius-sm:9px;
}
html{scroll-behavior:smooth}
body{
  font-family:'DM Sans',sans-serif;
  background:var(--bg);
  color:var(--text);
  font-size:14px;
  line-height:1.55;
  min-height:100vh;
}

/* ── LAYOUT ─────────────────────────── */
.shell{display:flex;min-height:100vh}
.sidebar{
  width:240px;min-width:240px;
  background:var(--green4);
  display:flex;flex-direction:column;
  padding:28px 0;
  position:fixed;top:0;left:0;bottom:0;
  z-index:100;
}
.main{margin-left:240px;flex:1;padding:36px 40px 60px;max-width:1100px}

/* ── SIDEBAR ─────────────────────────── */
.sb-logo{
  display:flex;align-items:center;gap:10px;
  padding:0 22px 28px;
  border-bottom:1px solid rgba(255,255,255,0.10);
  margin-bottom:12px;
}
.sb-logo-icon{
  width:34px;height:34px;background:var(--green3);
  border-radius:10px;display:flex;align-items:center;justify-content:center;
  flex-shrink:0;
}
.sb-logo-icon svg{width:18px;height:18px;fill:#fff}
.sb-logo-name{font-family:'Syne',sans-serif;font-size:16px;font-weight:700;color:#fff;letter-spacing:.02em}
.sb-logo-sub{font-size:10px;color:rgba(255,255,255,0.45);margin-top:1px}

.sb-section{font-size:10px;font-weight:500;color:rgba(255,255,255,0.35);
  letter-spacing:.1em;text-transform:uppercase;padding:0 22px 8px;margin-top:8px}

.sb-item{
  display:flex;align-items:center;gap:10px;
  padding:10px 22px;font-size:13px;color:rgba(255,255,255,0.65);
  cursor:pointer;transition:all .15s;border-left:3px solid transparent;
}
.sb-item:hover{color:#fff;background:rgba(255,255,255,0.06)}
.sb-item.active{color:#fff;background:rgba(255,255,255,0.10);border-left-color:var(--green3)}
.sb-item svg{width:16px;height:16px;fill:currentColor;flex-shrink:0}

.sb-bottom{margin-top:auto;padding:16px 22px 0;border-top:1px solid rgba(255,255,255,0.10)}
.sb-user{display:flex;align-items:center;gap:10px}
.sb-avatar{
  width:32px;height:32px;background:var(--green3);border-radius:50%;
  display:flex;align-items:center;justify-content:center;
  font-size:12px;font-weight:600;color:#fff;font-family:'Syne',sans-serif;
}
.sb-user-info{flex:1;min-width:0}
.sb-user-name{font-size:12px;color:#fff;font-weight:500;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.sb-user-role{font-size:10px;color:rgba(255,255,255,0.4)}

/* ── SCREENS ─────────────────────────── */
.screen{display:none;animation:fadeUp .25s ease}
.screen.active{display:block}
@keyframes fadeUp{from{opacity:0;transform:translateY(10px)}to{opacity:1;transform:translateY(0)}}

/* ── TOPBAR ─────────────────────────── */
.topbar{
  display:flex;justify-content:space-between;align-items:center;
  margin-bottom:32px;
}
.page-title{font-family:'Syne',sans-serif;font-size:24px;font-weight:700;color:var(--text)}
.page-sub{font-size:13px;color:var(--text3);margin-top:2px}
.btn-new{
  display:flex;align-items:center;gap:6px;
  padding:10px 20px;background:var(--green);color:#fff;
  border:none;border-radius:var(--radius-sm);font-family:inherit;
  font-size:13px;font-weight:500;cursor:pointer;transition:background .15s;
}
.btn-new:hover{background:var(--green3)}
.btn-new svg{width:14px;height:14px;fill:#fff}

/* ── CARDS ─────────────────────────── */
.card{
  background:var(--surface);border:0.5px solid var(--border);
  border-radius:var(--radius);padding:22px 24px;margin-bottom:16px;
}
.card-header{
  display:flex;align-items:center;justify-content:space-between;
  margin-bottom:18px;
}
.card-label{
  font-size:11px;font-weight:500;color:var(--text3);
  text-transform:uppercase;letter-spacing:.08em;
  display:flex;align-items:center;gap:6px;
}
.card-label::before{content:'';width:5px;height:5px;border-radius:50%;background:var(--green3);display:block}

/* ── PROJECT CARDS ─────────────────── */
.proj-grid{display:flex;flex-direction:column;gap:12px}
.proj-card{
  background:var(--surface);border:0.5px solid var(--border);
  border-radius:var(--radius);padding:20px 22px;
  cursor:pointer;transition:all .18s;
}
.proj-card:hover{border-color:var(--border2);box-shadow:0 4px 20px rgba(0,0,0,0.06);transform:translateY(-1px)}
.proj-card-top{display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:14px}
.proj-name{font-family:'Syne',sans-serif;font-size:16px;font-weight:600;color:var(--text);margin-bottom:3px}
.proj-meta{font-size:12px;color:var(--text3)}
.badge{
  padding:4px 12px;border-radius:20px;font-size:11px;font-weight:500;
  white-space:nowrap;flex-shrink:0;
}
.badge-green{background:var(--green2);color:var(--green3)}
.badge-warn{background:var(--warnbg);color:var(--warn)}
.badge-red{background:var(--dangerbg);color:var(--danger)}
.badge-gray{background:var(--surface2);color:var(--text3)}

.kpi-row{
  display:grid;grid-template-columns:repeat(4,1fr);gap:10px;
  background:var(--bg);border-radius:10px;padding:12px;margin-bottom:14px;
}
.kpi-item{display:flex;flex-direction:column;gap:2px}
.kpi-label{font-size:10px;color:var(--text3);text-transform:uppercase;letter-spacing:.06em}
.kpi-val{font-family:'DM Mono',monospace;font-size:13px;font-weight:500;color:var(--text)}
.kpi-val.pos{color:var(--green3)}
.kpi-val.neg{color:var(--danger)}
.kpi-val.warn{color:var(--warn)}

.proj-actions{display:flex;gap:6px}
.act-btn{
  padding:5px 13px;border:0.5px solid var(--border2);border-radius:7px;
  background:none;font-family:inherit;font-size:12px;color:var(--text2);
  cursor:pointer;transition:all .13s;
}
.act-btn:hover{background:var(--surface2);color:var(--text)}
.act-btn.danger:hover{color:var(--danger);border-color:var(--danger)}

/* ── FORM ─────────────────────────── */
.form-grid{display:grid;grid-template-columns:1fr 1fr;gap:14px}
.form-grid-3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:14px}
.field{display:flex;flex-direction:column;gap:5px}
.field label{font-size:12px;color:var(--text2);font-weight:400}
.field input,.field select{
  background:var(--surface2);border:0.5px solid var(--border2);
  border-radius:var(--radius-sm);padding:9px 12px;
  font-family:inherit;font-size:13px;color:var(--text);
  outline:none;transition:border-color .15s;width:100%;
}
.field input:focus,.field select:focus{border-color:var(--green3)}
.field .hint{font-size:11px;color:var(--text3)}
.field input[readonly]{opacity:.55;cursor:default}
.field input[readonly]:focus{border-color:var(--border2)}

.form-section{margin-bottom:6px}
.form-section-title{
  font-size:11px;font-weight:500;color:var(--text3);
  text-transform:uppercase;letter-spacing:.08em;
  margin:24px 0 12px;display:flex;align-items:center;gap:8px;
}
.form-section-title::after{content:'';flex:1;height:0.5px;background:var(--border)}

/* ── STEP INDICATOR ─────────────────── */
.steps{display:flex;margin-bottom:28px;background:var(--surface2);border-radius:10px;padding:4px;gap:3px}
.step-btn{
  flex:1;padding:8px;border-radius:8px;border:none;background:none;
  font-family:inherit;font-size:12px;color:var(--text3);
  cursor:pointer;transition:all .15s;font-weight:400;
}
.step-btn.active{background:var(--surface);color:var(--text);font-weight:500;
  box-shadow:0 1px 4px rgba(0,0,0,0.08)}
.form-step{display:none}
.form-step.active{display:block}

/* ── RESULT ─────────────────────────── */
.result-hero{
  background:var(--green4);border-radius:var(--radius);
  padding:28px 32px;margin-bottom:20px;position:relative;overflow:hidden;
}
.result-hero::before{
  content:'';position:absolute;right:-40px;top:-40px;
  width:200px;height:200px;border-radius:50%;
  background:rgba(255,255,255,0.04);
}
.result-hero-name{font-family:'Syne',sans-serif;font-size:22px;font-weight:700;color:#fff;margin-bottom:3px}
.result-hero-meta{font-size:13px;color:rgba(255,255,255,0.55)}
.result-hero-kpis{display:grid;grid-template-columns:repeat(4,1fr);gap:20px;margin-top:22px}
.hero-kpi-label{font-size:10px;color:rgba(255,255,255,0.45);text-transform:uppercase;letter-spacing:.08em;margin-bottom:4px}
.hero-kpi-val{font-family:'DM Mono',monospace;font-size:20px;font-weight:500;color:#fff}
.hero-kpi-sub{font-size:11px;color:rgba(255,255,255,0.4);margin-top:2px}

.kpi-cards{display:grid;grid-template-columns:repeat(4,1fr);gap:12px;margin-bottom:20px}
.kpi-card{background:var(--surface);border:0.5px solid var(--border);border-radius:12px;padding:16px 18px}
.kpi-card-label{font-size:11px;color:var(--text3);margin-bottom:6px}
.kpi-card-val{font-family:'DM Mono',monospace;font-size:22px;font-weight:500;color:var(--text);line-height:1.1}
.kpi-card-sub{font-size:11px;color:var(--text3);margin-top:4px}
.kpi-card.good .kpi-card-val{color:var(--green3)}
.kpi-card.warn .kpi-card-val{color:var(--warn)}
.kpi-card.bad  .kpi-card-val{color:var(--danger)}

/* ── TABLE ─────────────────────────── */
.table-wrap{overflow-x:auto}
table{width:100%;border-collapse:collapse;font-size:13px}
th{text-align:left;font-size:10px;font-weight:500;color:var(--text3);
  text-transform:uppercase;letter-spacing:.07em;padding:8px 10px;
  border-bottom:0.5px solid var(--border)}
td{padding:9px 10px;border-bottom:0.5px solid var(--border);color:var(--text)}
tr:last-child td{border-bottom:none}
.td-mono{font-family:'DM Mono',monospace;font-size:12px}
.td-label{font-size:13px;color:var(--text2)}
.td-pos{color:var(--green3)}
.td-neg{color:var(--danger)}
.td-total{font-weight:500}
tr.separator td{border-top:2px solid var(--border2);padding-top:11px}

/* ── CHART ─────────────────────────── */
.chart-wrap{position:relative;width:100%}

/* ── BUTTONS ─────────────────────────── */
.btn-row{display:flex;gap:8px;margin-top:22px;flex-wrap:wrap;align-items:center}
.btn{
  display:inline-flex;align-items:center;justify-content:center;gap:6px;
  padding:10px 20px;border-radius:var(--radius-sm);border:0.5px solid var(--border2);
  background:var(--surface);color:var(--text);font-family:inherit;
  font-size:13px;font-weight:500;cursor:pointer;transition:all .15s;
}
.btn:hover{background:var(--surface2)}
.btn-primary{background:var(--green);color:#fff;border-color:var(--green)}
.btn-primary:hover{background:var(--green3);border-color:var(--green3)}
.btn svg{width:13px;height:13px;fill:currentColor}

/* ── LOGIN ─────────────────────────── */
.login-page{
  min-height:100vh;display:flex;align-items:center;justify-content:center;
  background:var(--bg);padding:24px;
}
.login-card{
  background:var(--surface);border-radius:20px;padding:44px 40px;
  width:100%;max-width:420px;border:0.5px solid var(--border);
}
.login-logo{display:flex;align-items:center;gap:12px;margin-bottom:32px}
.login-logo-icon{
  width:40px;height:40px;background:var(--green4);border-radius:12px;
  display:flex;align-items:center;justify-content:center;
}
.login-logo-icon svg{width:20px;height:20px;fill:#fff}
.login-logo-name{font-family:'Syne',sans-serif;font-size:18px;font-weight:700;color:var(--text)}
.login-logo-sub{font-size:12px;color:var(--text3)}
.login-title{font-family:'Syne',sans-serif;font-size:22px;font-weight:700;color:var(--text);margin-bottom:6px}
.login-sub{font-size:13px;color:var(--text3);margin-bottom:28px}
.google-btn{
  width:100%;padding:12px;border:0.5px solid var(--border2);border-radius:10px;
  background:#fff;cursor:pointer;display:flex;align-items:center;justify-content:center;
  gap:10px;font-family:inherit;font-size:14px;font-weight:500;color:var(--text);
  margin-bottom:22px;transition:background .13s;
}
.google-btn:hover{background:var(--surface2)}
.divider{
  display:flex;align-items:center;gap:12px;margin-bottom:22px;
  font-size:12px;color:var(--text3);
}
.divider::before,.divider::after{content:'';flex:1;height:0.5px;background:var(--border2)}
.submit-btn{
  width:100%;padding:12px;background:var(--green4);color:#fff;border:none;
  border-radius:10px;font-family:inherit;font-size:14px;font-weight:500;
  cursor:pointer;margin-top:6px;transition:background .13s;
}
.submit-btn:hover{background:var(--green)}
.toggle-link{
  text-align:center;font-size:13px;color:var(--text3);margin-top:20px;
}
.toggle-link a{color:var(--green3);cursor:pointer;font-weight:500;text-decoration:none}
.toggle-link a:hover{text-decoration:underline}

/* ── VIABILITY BAR ─────────────────── */
.viab-bar-wrap{margin:16px 0 6px}
.viab-bar{display:flex;height:8px;border-radius:4px;overflow:hidden;gap:2px}
.vb-cost{background:#D4751A;border-radius:4px 0 0 4px}
.vb-profit{background:#2D7A50;border-radius:0 4px 4px 0}
.vb-legend{display:flex;gap:16px;font-size:11px;color:var(--text3);margin-top:5px}
.vb-legend span{display:flex;align-items:center;gap:4px}
.vb-dot{width:8px;height:8px;border-radius:2px;display:inline-block}

/* ── EMPTY ─────────────────────────── */
.empty-state{
  background:var(--surface);border:0.5px dashed var(--border2);
  border-radius:var(--radius);padding:48px;text-align:center;
}
.empty-icon{font-size:36px;margin-bottom:12px}
.empty-title{font-family:'Syne',sans-serif;font-size:17px;font-weight:600;color:var(--text);margin-bottom:6px}
.empty-sub{font-size:13px;color:var(--text3);margin-bottom:20px}

/* ── RESPONSIVE ─────────────────────── */
@media(max-width:800px){
  .sidebar{display:none}
  .main{margin-left:0;padding:24px 16px 48px}
  .form-grid,.form-grid-3,.kpi-cards,.result-hero-kpis,.kpi-row{grid-template-columns:1fr 1fr}
  .proj-card-top{flex-direction:column;gap:8px}
}
@media(max-width:480px){
  .form-grid,.form-grid-3,.kpi-cards,.result-hero-kpis,.kpi-row{grid-template-columns:1fr}
}
</style>
</head>
<body>

<!-- ════════════════════════════════════════
     LOGIN
════════════════════════════════════════ -->
<div id="page-login" class="login-page">
  <div class="login-card">
    <div class="login-logo">
      <div class="login-logo-icon">
        <svg viewBox="0 0 24 24"><path d="M3 21h18v-2H3v2zm0-4h18v-2H3v2zm0-4h18v-2H3v2zm0-4h18V7H3v2zm0-6v2h18V3H3z"/></svg>
      </div>
      <div>
        <div class="login-logo-name">ViabiCalc</div>
        <div class="login-logo-sub">Viabilidade Imobiliária</div>
      </div>
    </div>
    <div class="login-title">Bem-vindo de volta</div>
    <div class="login-sub">Acesse sua conta para gerenciar seus projetos</div>

    <button class="google-btn" onclick="mockLogin()">
      <svg width="18" height="18" viewBox="0 0 24 24">
        <path fill="#4285F4" d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z"/>
        <path fill="#34A853" d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z"/>
        <path fill="#FBBC05" d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l2.85-2.22.81-.62z"/>
        <path fill="#EA4335" d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z"/>
      </svg>
      Continuar com Google
    </button>

    <div class="divider">ou entre com e-mail</div>

    <div class="field" style="margin-bottom:12px">
      <label>E-mail</label>
      <input type="email" placeholder="joao@exemplo.com" value="joao.incorporador@email.com"/>
    </div>
    <div class="field" style="margin-bottom:4px">
      <label>Senha</label>
      <input type="password" placeholder="••••••••" value="senha123"/>
    </div>
    <button class="submit-btn" onclick="mockLogin()">Entrar na conta</button>
    <div class="toggle-link">Não tem conta? <a onclick="mockLogin()">Cadastre-se grátis</a></div>
  </div>
</div>

<!-- ════════════════════════════════════════
     APP SHELL (após login)
════════════════════════════════════════ -->
<div id="app-shell" class="shell" style="display:none">

  <!-- SIDEBAR -->
  <nav class="sidebar">
    <div class="sb-logo">
      <div class="sb-logo-icon">
        <svg viewBox="0 0 24 24"><path d="M3 21h18v-2H3v2zm0-4h18v-2H3v2zm0-4h18v-2H3v2zm0-4h18V7H3v2zm0-6v2h18V3H3z"/></svg>
      </div>
      <div>
        <div class="sb-logo-name">ViabiCalc</div>
        <div class="sb-logo-sub">Viabilidade Imobiliária</div>
      </div>
    </div>

    <div class="sb-section">Menu</div>
    <div class="sb-item active" onclick="showScreen('dashboard')">
      <svg viewBox="0 0 24 24"><path d="M3 13h8V3H3v10zm0 8h8v-6H3v6zm10 0h8V11h-8v10zm0-18v6h8V3h-8z"/></svg>
      Dashboard
    </div>
    <div class="sb-item" onclick="showScreen('novo-projeto')">
      <svg viewBox="0 0 24 24"><path d="M19 13h-6v6h-2v-6H5v-2h6V5h2v6h6v2z"/></svg>
      Novo Projeto
    </div>
    <div class="sb-item" onclick="showScreen('resultados')">
      <svg viewBox="0 0 24 24"><path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/></svg>
      Resultados
    </div>

    <div class="sb-section" style="margin-top:16px">Conta</div>
    <div class="sb-item" onclick="backToLogin()">
      <svg viewBox="0 0 24 24"><path d="M17 7l-1.41 1.41L18.17 11H8v2h10.17l-2.58 2.58L17 17l5-5-5-5zM4 5h8V3H4c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h8v-2H4V5z"/></svg>
      Sair
    </div>

    <div class="sb-bottom">
      <div class="sb-user">
        <div class="sb-avatar">JI</div>
        <div class="sb-user-info">
          <div class="sb-user-name">João Incorporador</div>
          <div class="sb-user-role">Plano gratuito</div>
        </div>
      </div>
    </div>
  </nav>

  <!-- MAIN AREA -->
  <main class="main">

    <!-- ── DASHBOARD ── -->
    <div class="screen active" id="screen-dashboard">
      <div class="topbar">
        <div>
          <div class="page-title">Meus Projetos</div>
          <div class="page-sub">3 empreendimentos cadastrados</div>
        </div>
        <button class="btn-new" onclick="showScreen('novo-projeto')">
          <svg viewBox="0 0 24 24"><path d="M19 13h-6v6h-2v-6H5v-2h6V5h2v6h6v2z"/></svg>
          Novo projeto
        </button>
      </div>

      <div class="proj-grid">

        <!-- Projeto 1 -->
        <div class="proj-card" onclick="showScreen('resultados')">
          <div class="proj-card-top">
            <div>
              <div class="proj-name">Residencial Parque Verde</div>
              <div class="proj-meta">Residencial · São Paulo/SP · 80 unid · 4.800 m²</div>
            </div>
            <span class="badge badge-green">Viável</span>
          </div>
          <div class="kpi-row">
            <div class="kpi-item">
              <span class="kpi-label">VGV</span>
              <span class="kpi-val">R$ 45,6M</span>
            </div>
            <div class="kpi-item">
              <span class="kpi-label">Margem liq.</span>
              <span class="kpi-val pos">18,4%</span>
            </div>
            <div class="kpi-item">
              <span class="kpi-label">TIR</span>
              <span class="kpi-val pos">23,1%</span>
            </div>
            <div class="kpi-item">
              <span class="kpi-label">VPL</span>
              <span class="kpi-val">R$ 4,2M</span>
            </div>
          </div>
          <div class="proj-actions">
            <button class="act-btn" onclick="event.stopPropagation();showScreen('resultados')">Ver resultados</button>
            <button class="act-btn" onclick="event.stopPropagation();showScreen('novo-projeto')">Editar</button>
            <button class="act-btn" onclick="event.stopPropagation()">Duplicar</button>
            <button class="act-btn danger" onclick="event.stopPropagation()">Excluir</button>
          </div>
        </div>

        <!-- Projeto 2 -->
        <div class="proj-card">
          <div class="proj-card-top">
            <div>
              <div class="proj-name">Edifício Comercial Centro</div>
              <div class="proj-meta">Comercial · Campinas/SP · 12 salas · 1.440 m²</div>
            </div>
            <span class="badge badge-warn">Atenção</span>
          </div>
          <div class="kpi-row">
            <div class="kpi-item">
              <span class="kpi-label">VGV</span>
              <span class="kpi-val">R$ 12,2M</span>
            </div>
            <div class="kpi-item">
              <span class="kpi-label">Margem liq.</span>
              <span class="kpi-val warn">7,1%</span>
            </div>
            <div class="kpi-item">
              <span class="kpi-label">TIR</span>
              <span class="kpi-val warn">14,8%</span>
            </div>
            <div class="kpi-item">
              <span class="kpi-label">VPL</span>
              <span class="kpi-val">R$ 860k</span>
            </div>
          </div>
          <div class="proj-actions">
            <button class="act-btn">Ver resultados</button>
            <button class="act-btn">Editar</button>
            <button class="act-btn">Duplicar</button>
            <button class="act-btn danger">Excluir</button>
          </div>
        </div>

        <!-- Projeto 3 -->
        <div class="proj-card">
          <div class="proj-card-top">
            <div>
              <div class="proj-name">Loteamento Horizonte</div>
              <div class="proj-meta">Misto · Ribeirão Preto/SP · 200 lotes · 30.000 m²</div>
            </div>
            <span class="badge badge-gray">Sem cálculo</span>
          </div>
          <div style="font-size:13px;color:var(--text3);padding:8px 0 14px">
            Parâmetros financeiros ainda não preenchidos.
          </div>
          <div class="proj-actions">
            <button class="act-btn" onclick="showScreen('novo-projeto')">Preencher parâmetros</button>
            <button class="act-btn">Duplicar</button>
            <button class="act-btn danger">Excluir</button>
          </div>
        </div>

      </div>
    </div>

    <!-- ── NOVO PROJETO ── -->
    <div class="screen" id="screen-novo-projeto">
      <div class="topbar">
        <div>
          <div class="page-title">Novo projeto</div>
          <div class="page-sub">Preencha os dados do empreendimento</div>
        </div>
        <button class="btn" onclick="showScreen('dashboard')">← Voltar</button>
      </div>

      <div class="steps">
        <button class="step-btn active" id="step-btn-1" onclick="goStep(1)">1. Identificação</button>
        <button class="step-btn" id="step-btn-2" onclick="goStep(2)">2. Custos</button>
        <button class="step-btn" id="step-btn-3" onclick="goStep(3)">3. Receitas</button>
        <button class="step-btn" id="step-btn-4" onclick="goStep(4)">4. Financiamento</button>
      </div>

      <!-- STEP 1 -->
      <div class="form-step active" id="step-1">
        <div class="card">
          <div class="card-header"><span class="card-label">Identificação do empreendimento</span></div>
          <div class="form-grid">
            <div class="field">
              <label>Nome do empreendimento *</label>
              <input type="text" value="Residencial Parque Verde" placeholder="Ex: Residencial Aurora"/>
            </div>
            <div class="field">
              <label>Tipo *</label>
              <select>
                <option selected>Residencial</option>
                <option>Comercial</option>
                <option>Misto</option>
                <option>Laje Corporativa</option>
                <option>Galpão / Industrial</option>
              </select>
            </div>
            <div class="field">
              <label>Cidade *</label>
              <input type="text" value="São Paulo"/>
            </div>
            <div class="field">
              <label>UF *</label>
              <select><option selected>SP</option><option>RJ</option><option>MG</option><option>RS</option><option>PR</option></select>
            </div>
          </div>
          <div class="form-section-title">Dimensionamento físico</div>
          <div class="form-grid-3">
            <div class="field">
              <label>Nº de unidades *</label>
              <input type="number" value="80"/>
            </div>
            <div class="field">
              <label>Área privativa total (m²) *</label>
              <input type="number" value="4800"/>
            </div>
            <div class="field">
              <label>Área média / unidade (m²)</label>
              <input type="number" value="60" readonly/>
              <span class="hint">Calculado automaticamente</span>
            </div>
          </div>
          <div class="form-section-title">Cronograma</div>
          <div class="form-grid-3">
            <div class="field"><label>Início das obras</label><input type="month" value="2025-03"/></div>
            <div class="field"><label>Lançamento</label><input type="month" value="2025-06"/></div>
            <div class="field"><label>Conclusão / entrega</label><input type="month" value="2027-09"/></div>
          </div>
        </div>
        <div class="btn-row">
          <button class="btn btn-primary" onclick="goStep(2)">Próximo: Custos →</button>
        </div>
      </div>

      <!-- STEP 2 -->
      <div class="form-step" id="step-2">
        <div class="card">
          <div class="card-header"><span class="card-label">Terreno</span></div>
          <div class="form-grid">
            <div class="field"><label>Valor do terreno (R$)</label><input type="number" value="2400000"/></div>
            <div class="field"><label>Área do terreno (m²)</label><input type="number" value="1200"/></div>
          </div>
        </div>
        <div class="card">
          <div class="card-header"><span class="card-label">Construção</span></div>
          <div class="form-grid">
            <div class="field"><label>Custo / m² construído (R$) — BDI incluso</label><input type="number" value="2800"/><span class="hint">CUB SP/2024 ≈ R$ 2.200–3.500/m²</span></div>
            <div class="field"><label>Área construída total (m²)</label><input type="number" value="6500"/><span class="hint">Inclui áreas comuns</span></div>
            <div class="field"><label>Infra / fundações (R$)</label><input type="number" value="320000"/></div>
            <div class="field"><label>Elétrica / hidráulica (R$)</label><input type="number" value="480000"/></div>
          </div>
        </div>
        <div class="card">
          <div class="card-header"><span class="card-label">Custos legais e incorporação</span></div>
          <div class="form-grid">
            <div class="field"><label>ITBI (% sobre terreno)</label><input type="number" value="3"/></div>
            <div class="field"><label>Registro, cartório, alvarás (R$)</label><input type="number" value="85000"/></div>
            <div class="field"><label>Projetos e aprovações (R$)</label><input type="number" value="150000"/></div>
            <div class="field"><label>Seguros (R$)</label><input type="number" value="60000"/></div>
          </div>
        </div>
        <div class="card">
          <div class="card-header"><span class="card-label">Marketing e vendas</span></div>
          <div class="form-grid">
            <div class="field"><label>Comissão de corretagem (% do VGV)</label><input type="number" value="5"/></div>
            <div class="field"><label>Marketing / publicidade (R$)</label><input type="number" value="200000"/></div>
          </div>
        </div>
        <div class="btn-row">
          <button class="btn" onclick="goStep(1)">← Voltar</button>
          <button class="btn btn-primary" onclick="goStep(3)">Próximo: Receitas →</button>
        </div>
      </div>

      <!-- STEP 3 -->
      <div class="form-step" id="step-3">
        <div class="card">
          <div class="card-header"><span class="card-label">Parâmetros de venda</span></div>
          <div class="form-grid">
            <div class="field"><label>Preço médio de venda (R$/m²)</label><input type="number" value="9500"/></div>
            <div class="field"><label>VGV estimado (R$) — calculado</label><input type="number" value="45600000" readonly/></div>
            <div class="field"><label>% vendas pré-lançamento</label><input type="number" value="20"/></div>
            <div class="field"><label>% vendas no lançamento</label><input type="number" value="50"/></div>
            <div class="field"><label>% vendas pós-lançamento</label><input type="number" value="30"/></div>
            <div class="field"><label>Inadimplência estimada (% VGV)</label><input type="number" value="3"/></div>
          </div>
        </div>
        <div class="btn-row">
          <button class="btn" onclick="goStep(2)">← Voltar</button>
          <button class="btn btn-primary" onclick="goStep(4)">Próximo: Financiamento →</button>
        </div>
      </div>

      <!-- STEP 4 -->
      <div class="form-step" id="step-4">
        <div class="card">
          <div class="card-header"><span class="card-label">Estrutura de capital</span></div>
          <div class="form-grid">
            <div class="field"><label>Capital próprio (%)</label><input type="number" value="40"/></div>
            <div class="field"><label>Financiamento bancário (%)</label><input type="number" value="60"/></div>
            <div class="field"><label>Taxa de juros (% a.a.)</label><input type="number" value="12"/></div>
            <div class="field"><label>Prazo do financiamento (meses)</label><input type="number" value="36"/></div>
            <div class="field"><label>TMA — taxa mínima de atratividade (% a.a.)</label><input type="number" value="15"/><span class="hint">Para cálculo do VPL</span></div>
            <div class="field"><label>Imposto sobre lucro — IRPJ+CSLL (%)</label><input type="number" value="27"/></div>
          </div>
        </div>
        <div class="btn-row">
          <button class="btn" onclick="goStep(3)">← Voltar</button>
          <button class="btn btn-primary" onclick="showScreen('resultados')">
            <svg viewBox="0 0 24 24"><path d="M9 16.17L4.83 12l-1.42 1.41L9 19 21 7l-1.41-1.41z"/></svg>
            Calcular viabilidade
          </button>
        </div>
      </div>

    </div><!-- /novo-projeto -->

    <!-- ── RESULTADOS ── -->
    <div class="screen" id="screen-resultados">
      <div class="topbar">
        <div>
          <div class="page-title">Resultado da Viabilidade</div>
          <div class="page-sub">Residencial Parque Verde · calculado agora</div>
        </div>
        <div style="display:flex;gap:8px">
          <button class="btn" onclick="showScreen('novo-projeto')">Editar parâmetros</button>
          <button class="btn btn-primary">
            <svg viewBox="0 0 24 24"><path d="M19 9h-4V3H9v6H5l7 7 7-7zM5 18v2h14v-2H5z"/></svg>
            Baixar PDF
          </button>
        </div>
      </div>

      <!-- Hero -->
      <div class="result-hero">
        <div style="display:flex;justify-content:space-between;align-items:flex-start;flex-wrap:wrap;gap:12px">
          <div>
            <div class="result-hero-name">Residencial Parque Verde</div>
            <div class="result-hero-meta">Residencial · São Paulo/SP · 80 unidades · 4.800 m² · Entrega set/2027</div>
          </div>
          <span class="badge badge-green" style="font-size:12px;padding:5px 14px">✓ Projeto Viável</span>
        </div>
        <div class="result-hero-kpis">
          <div>
            <div class="hero-kpi-label">VGV Total</div>
            <div class="hero-kpi-val">R$ 45,6M</div>
            <div class="hero-kpi-sub">Bruto estimado</div>
          </div>
          <div>
            <div class="hero-kpi-label">Investimento total</div>
            <div class="hero-kpi-val">R$ 36,7M</div>
            <div class="hero-kpi-sub">Inclui juros</div>
          </div>
          <div>
            <div class="hero-kpi-label">Lucro líquido</div>
            <div class="hero-kpi-val">R$ 8,4M</div>
            <div class="hero-kpi-sub">Após impostos</div>
          </div>
          <div>
            <div class="hero-kpi-label">Prazo</div>
            <div class="hero-kpi-val">30 meses</div>
            <div class="hero-kpi-sub">Obra + vendas</div>
          </div>
        </div>
        <div class="viab-bar-wrap">
          <div class="viab-bar">
            <div class="vb-cost" style="width:80.5%"></div>
            <div class="vb-profit" style="width:19.5%"></div>
          </div>
          <div class="vb-legend">
            <span><span class="vb-dot" style="background:#D4751A"></span>Custo total (80,5%)</span>
            <span><span class="vb-dot" style="background:#2D7A50"></span>Margem bruta (19,5%)</span>
          </div>
        </div>
      </div>

      <!-- KPI Cards -->
      <div class="kpi-cards">
        <div class="kpi-card good">
          <div class="kpi-card-label">VPL (TMA 15%)</div>
          <div class="kpi-card-val">R$ 4,2M</div>
          <div class="kpi-card-sub">Cria valor acima da TMA</div>
        </div>
        <div class="kpi-card good">
          <div class="kpi-card-label">TIR (ao ano)</div>
          <div class="kpi-card-val">23,1%</div>
          <div class="kpi-card-sub">TMA = 15% · Spread +8,1pp</div>
        </div>
        <div class="kpi-card good">
          <div class="kpi-card-label">Margem líquida</div>
          <div class="kpi-card-val">18,4%</div>
          <div class="kpi-card-sub">Bruta: 19,5%</div>
        </div>
        <div class="kpi-card">
          <div class="kpi-card-label">Payback simples</div>
          <div class="kpi-card-val">3 anos</div>
          <div class="kpi-card-sub">Descontado: 3 anos</div>
        </div>
      </div>

      <!-- Gráficos -->
      <div style="display:grid;grid-template-columns:1fr 1fr;gap:16px;margin-bottom:16px">
        <div class="card">
          <div class="card-header"><span class="card-label">Fluxo de caixa projetado (R$)</span></div>
          <div class="chart-wrap" style="height:200px"><canvas id="chart-fc-res"></canvas></div>
        </div>
        <div class="card">
          <div class="card-header"><span class="card-label">Composição do custo total</span></div>
          <div class="chart-wrap" style="height:200px"><canvas id="chart-pizza-res"></canvas></div>
        </div>
      </div>

      <!-- DRE -->
      <div class="card">
        <div class="card-header"><span class="card-label">DRE simplificado</span></div>
        <div class="table-wrap">
          <table>
            <thead>
              <tr><th>Linha do resultado</th><th style="text-align:right">Valor (R$)</th><th style="text-align:right">% do VGV</th></tr>
            </thead>
            <tbody>
              <tr><td class="td-label">(+) VGV total estimado</td><td class="td-mono td-pos" style="text-align:right">45.600.000</td><td class="td-mono" style="text-align:right">100,0%</td></tr>
              <tr><td class="td-label">(–) Inadimplência (3%)</td><td class="td-mono td-neg" style="text-align:right">(1.368.000)</td><td class="td-mono" style="text-align:right">3,0%</td></tr>
              <tr class="separator"><td class="td-label td-total">= Receita líquida de vendas</td><td class="td-mono td-total" style="text-align:right">44.232.000</td><td class="td-mono td-total" style="text-align:right">97,0%</td></tr>
              <tr><td class="td-label">(–) Terreno</td><td class="td-mono td-neg" style="text-align:right">(2.400.000)</td><td class="td-mono" style="text-align:right">5,3%</td></tr>
              <tr><td class="td-label">(–) Obra civil + BDI</td><td class="td-mono td-neg" style="text-align:right">(18.200.000)</td><td class="td-mono" style="text-align:right">39,9%</td></tr>
              <tr><td class="td-label">(–) Infraestrutura + instalações</td><td class="td-mono td-neg" style="text-align:right">(800.000)</td><td class="td-mono" style="text-align:right">1,8%</td></tr>
              <tr><td class="td-label">(–) Custos legais e incorporação</td><td class="td-mono td-neg" style="text-align:right">(367.000)</td><td class="td-mono" style="text-align:right">0,8%</td></tr>
              <tr><td class="td-label">(–) Marketing e corretagem</td><td class="td-mono td-neg" style="text-align:right">(2.480.000)</td><td class="td-mono" style="text-align:right">5,4%</td></tr>
              <tr class="separator"><td class="td-label td-total">= Lucro bruto</td><td class="td-mono td-pos td-total" style="text-align:right">19.985.000</td><td class="td-mono td-total" style="text-align:right">19,5%</td></tr>
              <tr><td class="td-label">(–) Juros de financiamento</td><td class="td-mono td-neg" style="text-align:right">(6.210.000)</td><td class="td-mono" style="text-align:right">13,6%</td></tr>
              <tr><td class="td-label">(–) IRPJ + CSLL (27%)</td><td class="td-mono td-neg" style="text-align:right">(3.723.000)</td><td class="td-mono" style="text-align:right">8,2%</td></tr>
              <tr class="separator"><td class="td-label td-total" style="font-size:14px">= Lucro líquido</td><td class="td-mono td-pos td-total" style="text-align:right;font-size:14px">8.388.000</td><td class="td-mono td-total" style="text-align:right;font-size:14px">18,4%</td></tr>
            </tbody>
          </table>
        </div>
      </div>

      <!-- Resumo técnico -->
      <div class="card">
        <div class="card-header"><span class="card-label">Resumo técnico</span></div>
        <div style="display:grid;grid-template-columns:1fr 1fr 1fr;gap:0">
          <div style="padding:10px 0;border-bottom:0.5px solid var(--border);display:flex;justify-content:space-between"><span style="color:var(--text2);font-size:13px">Custo total / m²</span><span class="td-mono">R$ 7.654/m²</span></div>
          <div style="padding:10px 16px;border-bottom:0.5px solid var(--border);border-left:0.5px solid var(--border);display:flex;justify-content:space-between"><span style="color:var(--text2);font-size:13px">Capital próprio</span><span class="td-mono">R$ 14,7M</span></div>
          <div style="padding:10px 0 10px 16px;border-bottom:0.5px solid var(--border);border-left:0.5px solid var(--border);display:flex;justify-content:space-between"><span style="color:var(--text2);font-size:13px">Financiamento</span><span class="td-mono">R$ 22,0M</span></div>
          <div style="padding:10px 0;display:flex;justify-content:space-between"><span style="color:var(--text2);font-size:13px">Retorno s/ cap. próprio</span><span class="td-mono" style="color:var(--green3)">57,1%</span></div>
          <div style="padding:10px 16px;border-left:0.5px solid var(--border);display:flex;justify-content:space-between"><span style="color:var(--text2);font-size:13px">VGV / unidade</span><span class="td-mono">R$ 570k</span></div>
          <div style="padding:10px 0 10px 16px;border-left:0.5px solid var(--border);display:flex;justify-content:space-between"><span style="color:var(--text2);font-size:13px">Lucro liq. / unidade</span><span class="td-mono" style="color:var(--green3)">R$ 104,9k</span></div>
        </div>
      </div>

      <div class="btn-row">
        <button class="btn" onclick="showScreen('dashboard')">← Voltar ao dashboard</button>
        <button class="btn" onclick="showScreen('novo-projeto')">Ajustar parâmetros</button>
        <button class="btn btn-primary">
          <svg viewBox="0 0 24 24"><path d="M19 9h-4V3H9v6H5l7 7 7-7zM5 18v2h14v-2H5z"/></svg>
          Gerar relatório PDF
        </button>
      </div>
    </div><!-- /resultados -->

  </main>
</div><!-- /app-shell -->

<script>
// ── NAVEGAÇÃO ───────────────────────────────────────────
function mockLogin(){
  document.getElementById('page-login').style.display='none';
  document.getElementById('app-shell').style.display='flex';
  showScreen('dashboard');
  setTimeout(renderCharts,200);
}
function backToLogin(){
  document.getElementById('app-shell').style.display='none';
  document.getElementById('page-login').style.display='flex';
}
function showScreen(name){
  document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));
  document.getElementById('screen-'+name).classList.add('active');
  document.querySelectorAll('.sb-item').forEach(i=>i.classList.remove('active'));
  const map={dashboard:0,'novo-projeto':1,resultados:2};
  const items=document.querySelectorAll('.sb-item');
  if(items[map[name]])items[map[name]].classList.add('active');
  if(name==='resultados')setTimeout(renderCharts,150);
}
function goStep(n){
  document.querySelectorAll('.form-step').forEach(s=>s.classList.remove('active'));
  document.querySelectorAll('.step-btn').forEach(b=>b.classList.remove('active'));
  document.getElementById('step-'+n).classList.add('active');
  document.getElementById('step-btn-'+n).classList.add('active');
}

// ── GRÁFICOS ────────────────────────────────────────────
let charts={};
function renderCharts(){
  if(charts.fc){charts.fc.destroy();delete charts.fc}
  if(charts.pizza){charts.pizza.destroy();delete charts.pizza}
  const ctxFc=document.getElementById('chart-fc-res');
  if(ctxFc){
    charts.fc=new Chart(ctxFc,{
      type:'bar',
      data:{
        labels:['Ano 0','Ano 1','Ano 2','Ano 3','Ano 4'],
        datasets:[{
          label:'Fluxo de Caixa',
          data:[-5300000,9100000,11400000,10200000,8800000],
          backgroundColor:[-5300000,9100000,11400000,10200000,8800000].map(v=>v>=0?'rgba(45,122,80,0.75)':'rgba(176,32,32,0.70)'),
          borderRadius:6,borderSkipped:false
        }]
      },
      options:{
        responsive:true,maintainAspectRatio:false,
        plugins:{legend:{display:false}},
        scales:{
          y:{
            ticks:{callback:v=>{const a=Math.abs(v);return(v<0?'-':'')+'R$'+(a>=1e6?(a/1e6).toFixed(1)+'M':(a/1e3).toFixed(0)+'k')},color:'#A09C88',font:{size:10}},
            grid:{color:'rgba(160,156,136,0.15)'}
          },
          x:{ticks:{color:'#A09C88',font:{size:11}},grid:{display:false}}
        }
      }
    });
  }
  const ctxP=document.getElementById('chart-pizza-res');
  if(ctxP){
    charts.pizza=new Chart(ctxP,{
      type:'doughnut',
      data:{
        labels:['Terreno','Obra civil','Infra','Legal','Marketing'],
        datasets:[{
          data:[2400000,18200000,800000,367000,2480000],
          backgroundColor:['#1B5E38','#2D7A50','#8a6200','#B05A00','#6b4c8a'],
          borderWidth:0
        }]
      },
      options:{
        responsive:true,maintainAspectRatio:false,
        plugins:{
          legend:{position:'right',labels:{font:{size:11},color:'#6A6554',boxWidth:10,padding:8}}
        },
        cutout:'60%'
      }
    });
  }
}
</script>
</body>
</html>
