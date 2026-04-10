<!DOCTYPE html>
<html lang="pt">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<title>MozData · Auto MB</title>
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@300;400;500;600&family=Bebas+Neue&family=Outfit:wght@300;400;600;700;900&display=swap" rel="stylesheet">
<style>
:root {
  --bg: #060708; --s1: #0d1014; --s2: #131820; --border: #1a2230;
  --red: #e8000d; --red2: #ff3340; --green: #00d68f;
  --yellow: #ffc940; --blue: #3d9eff; --text: #dde4ee; --muted: #3d4f63;
  --mono: 'IBM Plex Mono', monospace;
  --display: 'Bebas Neue', sans-serif;
  --body: 'Outfit', sans-serif;
}
* { margin:0; padding:0; box-sizing:border-box; -webkit-tap-highlight-color:transparent; }
body { background:var(--bg); color:var(--text); font-family:var(--body); min-height:100vh; overflow-x:hidden; padding-bottom:100px; }
body::after { content:''; position:fixed; inset:0; background:repeating-linear-gradient(0deg,transparent,transparent 2px,rgba(0,0,0,0.08) 2px,rgba(0,0,0,0.08) 4px); pointer-events:none; z-index:999; }

.topbar { position:sticky; top:0; z-index:50; background:rgba(6,7,8,0.92); backdrop-filter:blur(16px); border-bottom:1px solid var(--border); padding:0 16px; height:56px; display:flex; align-items:center; justify-content:space-between; }
.brand { display:flex; align-items:center; gap:10px; }
.brand-logo { width:32px; height:32px; background:var(--blue); border-radius:6px; display:grid; place-items:center; font-size:18px; flex-shrink:0; }
.brand-name { font-family:var(--display); font-size:22px; letter-spacing:1px; color:#fff; }
.brand-name span { color:var(--blue); }
.live-badge { display:flex; align-items:center; gap:6px; padding:5px 10px; background:rgba(61,158,255,0.08); border:1px solid rgba(61,158,255,0.2); border-radius:20px; font-family:var(--mono); font-size:10px; color:var(--blue); }
.live-dot { width:6px; height:6px; background:var(--blue); border-radius:50%; animation:blink 1.4s infinite; }
@keyframes blink { 0%,100%{opacity:1;box-shadow:0 0 4px var(--blue);}50%{opacity:0.2;box-shadow:none;} }

.page { display:none; }
.page.active { display:block; }

.bottom-nav { position:fixed; bottom:0; left:0; right:0; z-index:50; background:rgba(13,16,20,0.97); border-top:1px solid var(--border); display:grid; grid-template-columns:repeat(4,1fr); height:64px; }
.nav-btn { display:flex; flex-direction:column; align-items:center; justify-content:center; gap:4px; background:none; border:none; color:var(--muted); font-family:var(--mono); font-size:9px; cursor:pointer; transition:color 0.2s; position:relative; }
.nav-btn .icon { font-size:20px; line-height:1; }
.nav-btn.active { color:var(--blue); }
.nav-btn.active::after { content:''; position:absolute; top:0; left:20%; right:20%; height:2px; background:var(--blue); border-radius:0 0 2px 2px; }
.nav-badge { position:absolute; top:8px; right:calc(50% - 14px); background:var(--red); color:#fff; font-size:9px; font-family:var(--mono); font-weight:600; width:16px; height:16px; border-radius:50%; display:grid; place-items:center; }

/* HOME */
#page-home { padding:16px 16px 0; }
.hero-status { background:var(--s1); border:1px solid var(--border); border-radius:16px; padding:20px; margin-bottom:16px; position:relative; overflow:hidden; }
.hero-status::before { content:'MB AUTO'; position:absolute; right:-10px; top:14px; font-family:var(--display); font-size:56px; color:rgba(61,158,255,0.05); letter-spacing:2px; white-space:nowrap; }
.status-row { display:flex; align-items:center; justify-content:space-between; margin-bottom:16px; }
.status-label { font-family:var(--mono); font-size:10px; color:var(--muted); text-transform:uppercase; letter-spacing:1.5px; }
.toggle-wrap { display:flex; align-items:center; gap:10px; }
.toggle { width:48px; height:26px; background:var(--s2); border:1px solid var(--border); border-radius:13px; cursor:pointer; position:relative; transition:background 0.3s; }
.toggle.on { background:var(--blue); border-color:var(--blue); }
.toggle-knob { position:absolute; top:3px; left:3px; width:18px; height:18px; background:#fff; border-radius:50%; transition:transform 0.3s cubic-bezier(0.34,1.56,0.64,1); box-shadow:0 1px 4px rgba(0,0,0,0.4); }
.toggle.on .toggle-knob { transform:translateX(22px); }
.toggle-state { font-family:var(--mono); font-size:12px; font-weight:600; }
.toggle-state.on{color:var(--blue);} .toggle-state.off{color:var(--muted);}
.stat-row { display:grid; grid-template-columns:1fr 1fr 1fr; gap:10px; }
.mini-stat { background:var(--s2); border-radius:10px; padding:12px 10px; text-align:center; }
.mini-stat-val { font-family:var(--display); font-size:26px; line-height:1; margin-bottom:4px; }
.mini-stat-val.blue{color:var(--blue);} .mini-stat-val.green{color:var(--green);} .mini-stat-val.red{color:var(--red2);} .mini-stat-val.yellow{color:var(--yellow);}
.mini-stat-label { font-family:var(--mono); font-size:9px; color:var(--muted); text-transform:uppercase; }

.op-tag { display:inline-flex; align-items:center; padding:2px 7px; border-radius:4px; font-family:var(--mono); font-size:9px; font-weight:600; text-transform:uppercase; }
.op-tag.VODACOM{background:rgba(230,0,0,0.12);color:#ff4444;border:1px solid rgba(230,0,0,0.2);}
.op-tag.MOVITEL{background:rgba(255,201,64,0.12);color:var(--yellow);border:1px solid rgba(255,201,64,0.2);}
.op-tag.TMCEL{background:rgba(0,214,143,0.12);color:var(--green);border:1px solid rgba(0,214,143,0.2);}

.section-head { display:flex; align-items:center; justify-content:space-between; padding:16px 16px 10px; }
.section-title { font-family:var(--mono); font-size:10px; color:var(--muted); text-transform:uppercase; letter-spacing:2px; }
.clear-btn { background:none; border:1px solid var(--border); border-radius:6px; padding:4px 10px; color:var(--muted); font-family:var(--mono); font-size:10px; cursor:pointer; }

.queue-list { padding:0 16px; display:flex; flex-direction:column; gap:10px; }
.order-card { background:var(--s1); border:1px solid var(--border); border-radius:14px; padding:16px; position:relative; overflow:hidden; animation:slideIn 0.3s ease both; }
@keyframes slideIn{from{opacity:0;transform:translateY(8px);}to{opacity:1;transform:translateY(0);}}
.order-card::before { content:''; position:absolute; left:0; top:0; bottom:0; width:3px; background:var(--card-color,var(--muted)); }
.order-card.pendente{--card-color:var(--yellow);} .order-card.processando{--card-color:var(--blue);} .order-card.concluido{--card-color:var(--green);} .order-card.erro{--card-color:var(--red);}
.order-top { display:flex; align-items:flex-start; justify-content:space-between; margin-bottom:8px; }
.order-phone { font-family:var(--mono); font-size:15px; font-weight:600; color:var(--text); }
.order-time { font-family:var(--mono); font-size:10px; color:var(--muted); margin-top:2px; }
.order-mb-val { font-family:var(--display); font-size:28px; color:var(--blue); line-height:1; text-align:right; }
.order-mb-price { font-family:var(--mono); font-size:11px; color:var(--muted); text-align:right; margin-top:2px; }
.order-bottom { display:flex; align-items:center; justify-content:space-between; margin-top:10px; }
.status-chip { display:inline-flex; align-items:center; gap:5px; padding:4px 10px; border-radius:6px; font-family:var(--mono); font-size:10px; font-weight:500; text-transform:uppercase; }
.status-chip.pendente{background:rgba(255,201,64,0.12);color:var(--yellow);}
.status-chip.processando{background:rgba(61,158,255,0.12);color:var(--blue);}
.status-chip.concluido{background:rgba(0,214,143,0.12);color:var(--green);}
.status-chip.erro{background:rgba(232,0,13,0.12);color:var(--red2);}
.order-actions { display:flex; gap:6px; }
.btn-sm { padding:6px 12px; border-radius:8px; border:none; font-family:var(--mono); font-size:11px; cursor:pointer; transition:opacity 0.2s,transform 0.1s; }
.btn-sm:active{transform:scale(0.95);} .btn-sm.primary{background:var(--blue);color:#fff;font-weight:600;}
.progress-bar { height:3px; background:var(--s2); border-radius:2px; margin-top:10px; overflow:hidden; }
.progress-fill { height:100%; background:var(--blue); border-radius:2px; width:0%; transition:width 0.4s ease; }

/* CONFIG */
#page-config { padding:16px 16px 0; }
.config-section { margin-bottom:20px; }
.config-title { font-family:var(--mono); font-size:10px; color:var(--muted); text-transform:uppercase; letter-spacing:2px; margin-bottom:10px; }
.config-card { background:var(--s1); border:1px solid var(--border); border-radius:14px; overflow:hidden; }
.config-row { display:flex; align-items:center; justify-content:space-between; padding:14px 16px; border-bottom:1px solid var(--border); }
.config-row:last-child{border-bottom:none;}
.config-row-label { font-size:14px; font-weight:600; color:var(--text); margin-bottom:2px; }
.config-row-sub { font-family:var(--mono); font-size:10px; color:var(--muted); }
.config-input { background:var(--s2); border:1px solid var(--border); border-radius:8px; padding:8px 12px; color:var(--text); font-family:var(--mono); font-size:13px; outline:none; text-align:right; max-width:140px; transition:border-color 0.2s; }
.config-input:focus{border-color:var(--blue);}
select.config-input{cursor:pointer;}

/* PACOTES MANAGER */
.add-pacote-form { background:var(--s1); border:1px solid var(--border); border-radius:14px; padding:16px; margin-bottom:12px; }
.form-label { font-family:var(--mono); font-size:10px; color:var(--muted); text-transform:uppercase; letter-spacing:1px; display:block; margin-bottom:6px; }
.form-grid { display:grid; grid-template-columns:1fr 1fr; gap:10px; margin-bottom:10px; }
.form-input { background:var(--s2); border:1px solid var(--border); border-radius:8px; padding:10px 12px; color:var(--text); font-family:var(--mono); font-size:13px; outline:none; width:100%; transition:border-color 0.2s; }
.form-input:focus{border-color:var(--blue);}

.pacotes-list { display:flex; flex-direction:column; gap:8px; }
.pacote-item { background:var(--s1); border:1px solid var(--border); border-radius:12px; padding:12px 14px; display:flex; align-items:center; gap:12px; }
.pacote-item-mb { font-family:var(--display); font-size:22px; color:var(--blue); line-height:1; }
.pacote-item-op { font-family:var(--mono); font-size:10px; color:var(--muted); margin-top:3px; }
.pacote-item-price { font-family:var(--mono); font-size:15px; color:var(--green); font-weight:600; margin-left:auto; margin-right:10px; }
.pacote-del { background:rgba(232,0,13,0.1); border:1px solid rgba(232,0,13,0.2); border-radius:8px; padding:6px 10px; color:var(--red2); font-size:14px; cursor:pointer; flex-shrink:0; }

.save-btn { width:100%; padding:16px; background:var(--blue); border:none; border-radius:14px; color:#fff; font-family:var(--display); font-size:20px; letter-spacing:2px; cursor:pointer; transition:opacity 0.2s; margin-top:8px; }
.save-btn:active{opacity:0.8;}
.add-btn { width:100%; padding:12px; background:rgba(61,158,255,0.1); border:1px solid rgba(61,158,255,0.3); border-radius:10px; color:var(--blue); font-family:var(--display); font-size:17px; letter-spacing:1px; cursor:pointer; transition:all 0.2s; }
.add-btn:active{opacity:0.8;}

/* HISTÓRICO */
#page-history { padding:16px 16px 0; }
.history-list { display:flex; flex-direction:column; gap:8px; }
.history-item { background:var(--s1); border:1px solid var(--border); border-radius:12px; padding:14px 16px; display:flex; align-items:center; gap:14px; }
.history-icon { width:38px; height:38px; border-radius:10px; display:grid; place-items:center; font-size:18px; flex-shrink:0; }
.history-icon.ok{background:rgba(0,214,143,0.1);} .history-icon.fail{background:rgba(232,0,13,0.1);}
.history-info { flex:1; min-width:0; }
.history-phone { font-family:var(--mono); font-size:13px; font-weight:500; color:var(--text); white-space:nowrap; overflow:hidden; text-overflow:ellipsis; }
.history-meta { font-family:var(--mono); font-size:10px; color:var(--muted); margin-top:2px; }
.history-right { text-align:right; flex-shrink:0; }
.history-mb { font-family:var(--display); font-size:22px; }
.history-mb.ok{color:var(--blue);} .history-mb.fail{color:var(--red2);}
.history-price { font-family:var(--mono); font-size:10px; color:var(--muted); margin-top:1px; }

/* LOGS */
#page-logs { padding:16px; }
.log-terminal { background:#050608; border:1px solid var(--border); border-radius:14px; padding:16px; font-family:var(--mono); font-size:11px; line-height:1.8; min-height:400px; max-height:60vh; overflow-y:auto; }
.log-line{margin-bottom:2px;} .log-line .ts{color:var(--muted);} .log-line .info{color:var(--blue);} .log-line .ok{color:var(--green);} .log-line .warn{color:var(--yellow);} .log-line .err{color:var(--red2);}

/* MODAL */
.modal-overlay { position:fixed; inset:0; background:rgba(0,0,0,0.7); backdrop-filter:blur(6px); z-index:200; display:none; place-items:center; padding:20px; }
.modal-overlay.open{display:grid;}
.modal { background:var(--s1); border:1px solid var(--border); border-radius:20px; padding:28px 24px; width:100%; max-width:380px; animation:popIn 0.3s cubic-bezier(0.34,1.56,0.64,1) both; }
@keyframes popIn{from{opacity:0;transform:scale(0.9) translateY(20px);}to{opacity:1;transform:scale(1) translateY(0);}}
.modal-title { font-family:var(--display); font-size:28px; letter-spacing:1px; margin-bottom:6px; }
.modal-sub { font-family:var(--mono); font-size:11px; color:var(--muted); margin-bottom:20px; }
.modal-detail { background:var(--s2); border-radius:12px; padding:16px; margin-bottom:20px; }
.modal-row { display:flex; justify-content:space-between; align-items:center; padding:6px 0; border-bottom:1px solid var(--border); }
.modal-row:last-child{border-bottom:none;}
.modal-row-label{font-family:var(--mono);font-size:11px;color:var(--muted);}
.modal-row-val{font-family:var(--mono);font-size:13px;font-weight:500;color:var(--text);}
.modal-btns{display:grid;grid-template-columns:1fr 1fr;gap:10px;}
.btn-modal{padding:14px;border-radius:12px;border:none;font-family:var(--display);font-size:18px;letter-spacing:1px;cursor:pointer;transition:opacity 0.2s;}
.btn-modal:active{opacity:0.8;} .btn-modal.confirm{background:var(--blue);color:#fff;} .btn-modal.cancel{background:var(--s2);color:var(--muted);border:1px solid var(--border);}

.op-selector{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin-bottom:16px;}
.op-btn{background:var(--s2);border:1px solid var(--border);border-radius:10px;padding:12px 6px;text-align:center;cursor:pointer;transition:all 0.15s;font-family:var(--mono);font-size:11px;font-weight:600;}
.op-btn:active{transform:scale(0.95);}
.op-btn.sel-VODACOM{border-color:#e60000;background:rgba(230,0,0,0.1);color:#ff4444;}
.op-btn.sel-MOVITEL{border-color:var(--yellow);background:rgba(255,201,64,0.1);color:var(--yellow);}
.op-btn.sel-TMCEL{border-color:var(--green);background:rgba(0,214,143,0.1);color:var(--green);}

.pacotes-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin-bottom:16px;max-height:200px;overflow-y:auto;}
.pacote-btn{background:var(--s2);border:1px solid var(--border);border-radius:10px;padding:10px 6px;text-align:center;cursor:pointer;transition:all 0.15s;}
.pacote-btn:active{transform:scale(0.95);}
.pacote-btn.selected{border-color:var(--blue);background:rgba(61,158,255,0.1);}
.pac-mb{font-family:var(--display);font-size:18px;color:var(--blue);}
.pac-price{font-family:var(--mono);font-size:10px;color:var(--muted);margin-top:2px;}

#toast{position:fixed;top:70px;left:50%;transform:translateX(-50%) translateY(-10px);background:var(--s2);border:1px solid var(--border);border-radius:30px;padding:10px 20px;font-family:var(--mono);font-size:12px;color:var(--text);z-index:300;opacity:0;transition:all 0.3s cubic-bezier(0.34,1.56,0.64,1);white-space:nowrap;pointer-events:none;}
#toast.show{opacity:1;transform:translateX(-50%) translateY(0);}
.empty-state{text-align:center;padding:48px 20px;color:var(--muted);font-family:var(--mono);font-size:12px;line-height:2;}
.empty-icon{font-size:36px;display:block;margin-bottom:10px;opacity:0.4;}
</style>
</head>
<body>

<div class="topbar">
  <div class="brand">
    <div class="brand-logo">📶</div>
    <div class="brand-name">MOZ<span>DATA</span></div>
  </div>
  <div class="live-badge"><div class="live-dot"></div>ACTIVO</div>
</div>

<!-- HOME -->
<div class="page active" id="page-home">
  <div style="padding:16px 16px 0">
    <div class="hero-status">
      <div class="status-row">
        <div><div class="status-label">Automação de MB</div></div>
        <div class="toggle-wrap">
          <span class="toggle-state off" id="toggleLabel">OFF</span>
          <div class="toggle" id="mainToggle" onclick="toggleAuto()"><div class="toggle-knob"></div></div>
        </div>
      </div>
      <div class="stat-row">
        <div class="mini-stat"><div class="mini-stat-val yellow" id="statPending">0</div><div class="mini-stat-label">Fila</div></div>
        <div class="mini-stat"><div class="mini-stat-val blue" id="statDone">0</div><div class="mini-stat-label">Enviados</div></div>
        <div class="mini-stat"><div class="mini-stat-val red" id="statErrors">0</div><div class="mini-stat-label">Erros</div></div>
      </div>
    </div>
  </div>
  <div class="section-head">
    <div class="section-title">Fila de MB</div>
    <button class="clear-btn" onclick="clearDone()">Limpar feitos</button>
  </div>
  <div class="queue-list" id="queueList">
    <div class="empty-state"><span class="empty-icon">📭</span>Nenhum pedido na fila.<br>Aguardando pedidos<br>no WhatsApp...</div>
  </div>
  <div style="padding:16px">
    <button class="save-btn" onclick="openAddModal()" style="background:var(--s2);color:var(--text);border:1px solid var(--border)">+ ADICIONAR MANUALMENTE</button>
  </div>
</div>

<!-- CONFIG -->
<div class="page" id="page-config">
  <div style="padding:16px 16px 0">

    <div class="config-section">
      <div class="config-title">Conta de envio</div>
      <div class="config-card">
        <div class="config-row">
          <div><div class="config-row-label">Operadora padrão</div><div class="config-row-sub">Usada no envio auto</div></div>
          <select class="config-input" id="cfgOperadora">
            <option value="VODACOM">VodaCom</option>
            <option value="MOVITEL">Movitel</option>
            <option value="TMCEL">Tmcel</option>
          </select>
        </div>
        <div class="config-row">
          <div><div class="config-row-label">Número de envio</div><div class="config-row-sub">Número que faz a recarga</div></div>
          <input class="config-input" type="tel" id="cfgPhone" placeholder="84XXXXXXX">
        </div>
        <div class="config-row">
          <div><div class="config-row-label">PIN / Senha</div><div class="config-row-sub">PIN da conta</div></div>
          <input class="config-input" type="password" id="cfgPin" placeholder="••••" maxlength="6">
        </div>
        <div class="config-row">
          <div><div class="config-row-label">Delay entre envios</div><div class="config-row-sub">Segundos de pausa</div></div>
          <input class="config-input" type="number" id="cfgDelay" placeholder="5" style="max-width:80px">
        </div>
        <div class="config-row">
          <div><div class="config-row-label">Confirmar no WhatsApp</div><div class="config-row-sub">Enviar msg ao cliente</div></div>
          <div class="toggle on" id="cfgConfirm" onclick="this.classList.toggle('on')"><div class="toggle-knob"></div></div>
        </div>
      </div>
    </div>

    <div class="config-section">
      <div class="config-title">Os meus pacotes</div>

      <div class="add-pacote-form">
        <div class="form-label" style="margin-bottom:12px">Criar novo pacote</div>
        <div class="form-grid">
          <div>
            <label class="form-label">Quantidade de MB</label>
            <input class="form-input" type="text" id="newMb" placeholder="ex: 1GB, 500MB">
          </div>
          
