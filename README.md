<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
<title>JEREMIAHZ TEMPLE</title>
<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
<style>
  :root {
    --cyan:#00e5ff;--pink:#ff1060;--purple:#8b2fff;--gold:#ffcc00;
    --bg:#060608;--bg2:#0c0c10;--bg3:#121218;--border:#1e1e2a;
    --text:#d0d0e0;--dim:#444455;
  }
  *{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent;}
  html,body{height:100%;overflow:hidden;background:var(--bg);color:var(--text);font-family:'Share Tech Mono',monospace;}
  #app{display:flex;flex-direction:column;height:100%;}
  #auth-screen{position:fixed;inset:0;background:var(--bg);z-index:1000;display:flex;flex-direction:column;align-items:center;justify-content:center;padding:24px;}
  .auth-logo{font-family:'Orbitron',sans-serif;font-size:28px;font-weight:900;color:#fff;letter-spacing:6px;margin-bottom:6px;}
  .auth-sub{font-size:10px;color:var(--dim);letter-spacing:4px;margin-bottom:40px;}
  .auth-card{width:100%;max-width:340px;background:var(--bg2);border:1px solid var(--border);border-radius:14px;padding:24px;}
  .auth-title{font-family:'Orbitron',sans-serif;font-size:11px;color:var(--cyan);letter-spacing:3px;margin-bottom:20px;text-align:center;}
  .auth-input{width:100%;background:var(--bg3);border:1px solid var(--border);border-radius:8px;padding:12px;color:var(--text);font-size:13px;margin-bottom:10px;outline:none;}
  .auth-input:focus{border-color:var(--cyan);}
  .auth-btn{width:100%;padding:13px;background:linear-gradient(135deg,var(--cyan),var(--purple));border:none;border-radius:8px;color:#000;font-family:'Orbitron',sans-serif;font-size:11px;font-weight:700;letter-spacing:2px;cursor:pointer;margin-bottom:10px;}
  .auth-switch{text-align:center;font-size:10px;color:var(--dim);cursor:pointer;padding:8px;}
  .auth-switch span{color:var(--cyan);}
  .auth-error{color:var(--pink);font-size:10px;text-align:center;margin-bottom:8px;min-height:16px;}
  #header{flex-shrink:0;display:flex;align-items:center;justify-content:space-between;padding:10px 14px;background:var(--bg);border-bottom:1px solid var(--border);}
  .logo-text{font-family:'Orbitron',sans-serif;font-size:11px;font-weight:900;color:#fff;letter-spacing:2px;}
  .bpm-val{font-family:'Orbitron',sans-serif;font-size:20px;font-weight:900;color:#fff;}
  .user-btn{padding:6px 12px;background:transparent;border:1px solid var(--border);border-radius:6px;color:var(--dim);font-size:9px;letter-spacing:1px;cursor:pointer;}
  #content{flex:1;overflow-y:auto;overflow-x:hidden;-webkit-overflow-scrolling:touch;padding-bottom:70px;}
  #nav{position:fixed;bottom:0;left:0;right:0;height:60px;background:var(--bg);border-top:1px solid var(--border);display:flex;z-index:100;}
  .nav-btn{flex:1;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:3px;background:transparent;border:none;cursor:pointer;color:var(--dim);font-size:8px;letter-spacing:1px;}
  .nav-btn.active{color:var(--cyan);}
  .nav-icon{font-size:18px;}
  .panel{padding:12px;display:none;}
  .panel.active{display:block;}
  .card{background:var(--bg2);border:1px solid var(--border);border-radius:10px;padding:14px;margin-bottom:12px;}
  .section-title{font-family:'Orbitron',sans-serif;font-size:9px;letter-spacing:3px;color:var(--cyan);margin-bottom:10px;}
  .deck{border-radius:10px;padding:12px;margin-bottom:12px;}
  .deck-a{background:var(--bg2);border:1px solid rgba(0,229,255,0.2);}
  .deck-b{background:var(--bg2);border:1px solid rgba(255,16,96,0.2);}
  .track-title{font-size:13px;font-weight:700;color:#fff;margin-bottom:2px;}
  .track-artist{font-size:10px;color:var(--dim);}
  .waveform-wrap{height:44px;background:#080810;border-radius:6px;overflow:hidden;margin-bottom:10px;}
  .waveform-canvas{width:100%;height:100%;display:block;}
  .jog-wrap{display:flex;justify-content:center;margin:10px 0;}
  .jog{width:110px;height:110px;border-radius:50%;position:relative;cursor:pointer;}
  .jog-ring{position:absolute;inset:0;border-radius:50%;border:2px solid rgba(0,229,255,0.3);}
  .jog-inner{position:absolute;inset:8px;border-radius:50%;background:radial-gradient(circle at 40% 35%,#1a1a22,#08080f);border:1px solid #1e1e2a;display:flex;align-items:center;justify-content:center;}
  .jog-center{width:36px;height:36px;border-radius:50%;background:#0c0c14;border:1px solid #2a2a3a;display:flex;align-items:center;justify-content:center;font-size:16px;}
  .jog.playing .jog-ring{box-shadow:0 0 20px rgba(0,229,255,0.4);border-color:var(--cyan);animation:spin 2s linear infinite;}
  .jog-b .jog-ring{border-color:rgba(255,16,96,0.3);}
  .jog-b.playing .jog-ring{box-shadow:0 0 20px rgba(255,16,96,0.4);border-color:var(--pink);}
  .ctrl-row{display:flex;gap:6px;margin-bottom:10px;}
  .ctrl-btn{flex:1;padding:8px 4px;background:#0c0c14;border:1px solid var(--border);border-radius:6px;color:var(--dim);font-size:9px;letter-spacing:1px;cursor:pointer;text-align:center;}
  .ctrl-btn.on{border-color:var(--cyan);color:var(--cyan);background:rgba(0,229,255,0.08);}
  .ctrl-btn-b.on{border-color:var(--pink);color:var(--pink);background:rgba(255,16,96,0.08);}
  .fader-label{display:flex;justify-content:space-between;font-size:8px;color:var(--dim);letter-spacing:1px;margin-bottom:4px;}
  input[type=range]{-webkit-appearance:none;width:100%;height:4px;border-radius:2px;background:#1e1e2a;outline:none;cursor:pointer;}
  input[type=range]::-webkit-slider-thumb{-webkit-appearance:none;width:18px;height:18px;border-radius:50%;background:var(--cyan);cursor:pointer;}
  .range-pink::-webkit-slider-thumb{background:var(--pink);}
  .range-purple::-webkit-slider-thumb{background:var(--purple);}
  .vu-row{display:flex;gap:2px;height:50px;justify-content:center;margin-bottom:10px;}
  .vu-bar{width:8px;height:100%;display:flex;flex-direction:column;justify-content:flex-end;gap:1px;}
  .vu-seg{flex:1;border-radius:1px;}
  .modal-bg{position:fixed;inset:0;background:rgba(0,0,0,0.8);z-index:500;display:flex;align-items:center;justify-content:center;padding:20px;}
  .modal{background:var(--bg2);border:1px solid var(--border);border-radius:14px;padding:20px;width:100%;max-width:320px;}
  .modal-title{font-family:'Orbitron',sans-serif;font-size:11px;color:var(--cyan);letter-spacing:3px;margin-bottom:16px;}
  .modal-input{width:100%;background:var(--bg3);border:1px solid var(--border);border-radius:8px;padding:11px;color:var(--text);font-size:13px;margin-bottom:10px;outline:none;}
  .modal-btn{width:100%;padding:12px;background:linear-gradient(135deg,var(--cyan),var(--purple));border:none;border-radius:8px;color:#000;font-family:'Orbitron',sans-serif;font-size:10px;font-weight:700;letter-spacing:2px;cursor:pointer;margin-bottom:8px;}
  .modal-cancel{width:100%;padding:10px;background:transparent;border:1px solid var(--border);border-radius:8px;color:var(--dim);font-size:10px;cursor:pointer;}
  .set-row{display:flex;justify-content:space-between;align-items:center;padding:10px 0;border-bottom:1px solid #0f0f18;}
  .set-info-title{font-size:12px;color:#ddd;}
  .set-info-sub{font-size:9px;color:var(--dim);margin-top:2px;}
  .load-set-btn{padding:5px 12px;background:rgba(0,229,255,0.08);border:1px solid rgba(0,229,255,0.3);border-radius:4px;color:var(--cyan);font-size:8px;letter-spacing:1px;cursor:pointer;}
  .seq-grid{display:flex;flex-direction:column;gap:5px;}
  .seq-row{display:flex;align-items:center;gap:4px;}
  .seq-label{font-size:8px;color:var(--dim);width:36px;flex-shrink:0;}
  .seq-steps{display:flex;gap:2px;flex:1;}
  .step-btn{flex:1;height:22px;border-radius:3px;border:none;cursor:pointer;}
  .step-btn.off{background:#141420;}
  .step-btn.on-kick{background:var(--cyan);}
  .step-btn.on-snare{background:var(--pink);}
  .step-btn.on-hh{background:var(--purple);}
  .step-btn.on-808{background:var(--gold);}
  .lib-track{display:flex;justify-content:space-between;align-items:center;padding:10px 0;border-bottom:1px solid #0f0f18;}
  .toast{position:fixed;bottom:80px;left:50%;transform:translateX(-50%);background:var(--bg2);border:1px solid var(--cyan);border-radius:8px;padding:10px 20px;font-size:11px;color:var(--cyan);letter-spacing:1px;z-index:999;opacity:0;transition:opacity 0.3s;pointer-events:none;white-space:nowrap;}
  .toast.show{opacity:1;}
  @keyframes spin{to{transform:rotate(360deg)}}
</style>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Share+Tech+Mono&display=swap" rel="stylesheet"/>
</head>
<body>
<div class="toast" id="toast"></div>
<div id="auth-screen">
  <div class="auth-logo">JEREMIAHZ</div>
  <div class="auth-sub">TEMPLE</div>
  <div class="auth-card">
    <div class="auth-title" id="auth-title">LOGIN</div>
    <div class="auth-error" id="auth-error"></div>
    <input class="auth-input" id="auth-username" placeholder="Username (signup only)" style="display:none"/>
    <input class="auth-input" id="auth-email" type="email" placeholder="Email address"/>
    <input class="auth-input" id="auth-password" type="password" placeholder="Password"/>
        <div style="display:flex;flex-direction:column;gap:10px;margin-top:4px;">
      <div ontouchend="submitAuth()" onclick="submitAuth()" style="width:100%;padding:14px;background:linear-gradient(135deg,var(--cyan),var(--purple));border-radius:8px;color:#000;font-family:'Orbitron',sans-serif;font-size:11px;font-weight:700;letter-spacing:2px;text-align:center;cursor:pointer;user-select:none;" id="auth-submit">LOGIN</div>
      <div ontouchend="toggleAuthMode()" onclick="toggleAuthMode()" style="width:100%;padding:14px;background:transparent;border:2px solid var(--cyan);border-radius:8px;color:var(--cyan);font-family:'Orbitron',sans-serif;font-size:11px;font-weight:700;letter-spacing:2px;text-align:center;cursor:pointer;user-select:none;" id="auth-toggle-btn">CREATE FREE ACCOUNT</div>
    </div>
  </div>
</div>
<div class="modal-bg" id="save-modal" style="display:none;">
  <div class="modal">
    <div class="modal-title">💾 SAVE SET</div>
    <input class="modal-input" id="save-title" placeholder="Set name e.g. Friday Night Mix"/>
    <button class="modal-btn" onclick="confirmSave()">SAVE TO CLOUD</button>
    <button class="modal-cancel" onclick="closeSaveModal()">Cancel</button>
  </div>
</div>
<div id="app" style="display:none;">
  <div id="header">
    <div>
      <div class="logo-text">JEREMIAHZ TEMPLE</div>
      <div style="font-size:7px;color:var(--dim);letter-spacing:2px;">DJ STUDIO</div>
    </div>
    <div style="text-align:center;">
      <div class="bpm-val" id="bpm-display">130</div>
      <div style="font-size:7px;color:var(--dim);letter-spacing:3px;">BPM</div>
    </div>
    <button class="user-btn" id="user-btn" onclick="signOut()">SIGN OUT</button>
  </div>
  <div id="content">
    <div class="panel active" id="panel-mixer">
      <div class="deck deck-a" style="margin-top:4px;">
        <div style="display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:8px;">
          <div>
            <div style="font-size:9px;color:var(--cyan);letter-spacing:2px;margin-bottom:3px;">DECK A</div>
            <div class="track-title" id="da-title">NO TRACK LOADED</div>
            <div class="track-artist" id="da-artist">Load from library</div>
          </div>
          <div style="text-align:right;">
            <div style="font-size:7px;color:var(--dim);">KEY</div>
            <div style="font-family:'Orbitron',sans-serif;font-size:14px;color:var(--cyan);" id="da-key">—</div>
          </div>
        </div>
        <div class="waveform-wrap"><canvas class="waveform-canvas" id="wf-a" height="44"></canvas></div>
        <div class="jog-wrap">
          <div class="jog" id="jog-a" onclick="toggleDeck('a')">
            <div class="jog-ring" id="jog-a-ring"></div>
            <div class="jog-inner"><div class="jog-center" id="jog-a-icon">▶</div></div>
          </div>
        </div>
        <div class="ctrl-row">
          <button class="ctrl-btn" id="loop-a" onclick="toggleLoop('a')">◉ LOOP</button>
          <button class="ctrl-btn" id="sync-a" onclick="syncDeck('a')">⇄ SYNC</button>
          <button class="ctrl-btn" onclick="showSaveModal()">💾 SAVE</button>
        </div>
        <div class="fader-label"><span>PITCH A</span><span id="pitch-a-val">0.0%</span></div>
        <input type="range" min="-12" max="12" step="0.1" value="0" oninput="updatePitch('a',this.value)"/>
      </div>
      <div class="card">
        <div class="section-title">MASTER</div>
        <div class="vu-row" id="vu-meters"></div>
        <div class="fader-label"><span style="color:var(--cyan);">A</span><span style="color:var(--dim);">CROSSFADER</span><span style="color:var(--pink);">B</span></div>
        <input type="range" min="0" max="100" value="50" id="xfader"/>
        <div style="margin-top:10px;">
          <div class="fader-label"><span>MASTER VOL</span><span id="master-val">80</span></div>
          <input type="range" min="0" max="100" value="80" oninput="document.getElementById('master-val').textContent=this.value"/>
        </div>
      </div>
      <div class="deck deck-b">
        <div style="display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:8px;">
          <div>
            <div style="font-size:9px;color:var(--pink);letter-spacing:2px;margin-bottom:3px;">DECK B</div>
            <div class="track-title" id="db-title">NO TRACK LOADED</div>
            <div class="track-artist" id="db-artist">Load from library</div>
          </div>
          <div style="text-align:right;">
            <div style="font-size:7px;color:var(--dim);">KEY</div>
            <div style="font-family:'Orbitron',sans-serif;font-size:14px;color:var(--pink);" id="db-key">—</div>
          </div>
        </div>
        <div class="waveform-wrap"><canvas class="waveform-canvas" id="wf-b" height="44"></canvas></div>
        <div class="jog-wrap">
          <div class="jog jog-b" id="jog-b" onclick="toggleDeck('b')">
            <div class="jog-ring" id="jog-b-ring"></div>
            <div class="jog-inner"><div class="jog-center" id="jog-b-icon">▶</div></div>
          </div>
        </div>
        <div class="ctrl-row">
          <button class="ctrl-btn ctrl-btn-b" id="loop-b" onclick="toggleLoop('b')">◉ LOOP</button>
          <button class="ctrl-btn ctrl-btn-b" id="sync-b" onclick="syncDeck('b')">⇄ SYNC</button>
          <button class="ctrl-btn ctrl-btn-b" onclick="showSaveModal()">💾 SAVE</button>
        </div>
        <div class="fader-label"><span>PITCH B</span><span id="pitch-b-val">0.0%</span></div>
        <input type="range" min="-12" max="12" step="0.1" value="0" class="range-pink" oninput="updatePitch('b',this.value)"/>
      </div>
    </div>
    <div class="panel" id="panel-sets">
      <div class="card">
        <div class="section-title">☁ MY SAVED SETS</div>
        <div id="sets-list"><div style="color:var(--dim);font-size:11px;text-align:center;padding:20px;">Loading your sets...</div></div>
      </div>
      <div class="card">
        <div class="section-title">📋 MY PATTERNS</div>
        <div id="patterns-list"><div style="color:var(--dim);font-size:11px;text-align:center;padding:20px;">Loading patterns...</div></div>
      </div>
    </div>
    <div class="panel" id="panel-beats">
      <div class="card">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;">
          <div class="section-title" style="margin:0;color:var(--purple);">STEP SEQUENCER</div>
          <div style="display:flex;gap:6px;">
            <button id="seq-play-btn" onclick="toggleSeq()" style="padding:6px 14px;background:rgba(139,47,255,0.1);border:1px solid var(--purple);border-radius:6px;color:var(--purple);font-size:10px;letter-spacing:2px;cursor:pointer;">▶ PLAY</button>
            <button onclick="savePattern()" style="padding:6px 10px;background:rgba(0,229,255,0.08);border:1px solid rgba(0,229,255,0.3);border-radius:6px;color:var(--cyan);font-size:9px;cursor:pointer;">💾</button>
          </div>
        </div>
        <div class="fader-label"><span>TEMPO</span><span id="seq-bpm-val">130 BPM</span></div>
        <input type="range" min="60" max="200" value="130" class="range-purple" oninput="updateBpm(this.value)" style="margin-bottom:12px;"/>
        <div class="seq-grid" id="seq-grid"></div>
        <div style="display:flex;gap:6px;margin-top:10px;overflow-x:auto;">
          <button onclick="loadPreset('trap')" style="padding:4px 10px;background:transparent;border:1px solid var(--border);border-radius:4px;color:var(--dim);font-size:8px;cursor:pointer;white-space:nowrap;">TRAP</button>
          <button onclick="loadPreset('boom')" style="padding:4px 10px;background:transparent;border:1px solid var(--border);border-radius:4px;color:var(--dim);font-size:8px;cursor:pointer;white-space:nowrap;">BOOM BAP</button>
          <button onclick="loadPreset('drill')" style="padding:4px 10px;background:transparent;border:1px solid var(--border);border-radius:4px;color:var(--dim);font-size:8px;cursor:pointer;white-space:nowrap;">DRILL</button>
          <button onclick="loadPreset('jersey')" style="padding:4px 10px;background:transparent;border:1px solid var(--border);border-radius:4px;color:var(--dim);font-size:8px;cursor:pointer;white-space:nowrap;">JERSEY</button>
        </div>
      </div>
    </div>
    <div class="panel" id="panel-library">
      <div class="card">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;">
          <div class="section-title" style="margin:0;">🎵 MY LIBRARY</div>
          <button onclick="refreshLibrary()" style="padding:5px 10px;background:transparent;border:1px solid var(--border);border-radius:4px;color:var(--dim);font-size:8px;cursor:pointer;">↻ REFRESH</button>
        </div>
        <div id="library-list"><div style="color:var(--dim);font-size:11px;text-align:center;padding:20px;">Loading library...</div></div>
      </div>
    </div>
  </div>
  <nav id="nav">
    <button class="nav-btn active" id="nav-mixer" onclick="switchTab('mixer')"><div class="nav-icon">🎚</div><span>MIXER</span></button>
    <button class="nav-btn" id="nav-sets" onclick="switchTab('sets');loadSets();"><div class="nav-icon">☁</div><span>SETS</span></button>
    <button class="nav-btn" id="nav-beats" onclick="switchTab('beats')"><div class="nav-icon">🥁</div><span>BEATS</span></button>
    <button class="nav-btn" id="nav-library" onclick="switchTab('library');refreshLibrary();"><div class="nav-icon">🎵</div><span>LIBRARY</span></button>
  </nav>
</div>
<script>
const{createClient}=supabase;
const db=createClient('https://ucqokaspslumaixtkanf.supabase.co','eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InVjcW9rYXNwc2x1bWFpeHRrYW5mIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NzI5OTE5OTQsImV4cCI6MjA4ODU2Nzk5NH0.TzsdYRI4WN8HcKWU7AD0XsAdlh2D8cVkpTyEUVQKPn0');
let currentUser=null,authMode='login';
const state={bpm:130,deckA:{playing:false,progress:0.2,loop:false,title:'NO TRACK LOADED',artist:'',key:'—'},deckB:{playing:false,progress:0.4,loop:false,title:'NO TRACK LOADED',artist:'',key:'—'},seqPlaying:false,currentStep:-1,steps:[[1,0,0,0,1,0,0,0,1,0,0,0,1,0,0,0],[0,0,0,0,1,0,0,0,0,0,0,0,1,0,0,1],[1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1],[0,0,0,1,0,0,0,0,0,0,1,0,0,0,0,0]],vuL:0,vuR:0};
const PRESETS={trap:[[1,0,0,0,1,0,0,0,1,0,0,0,1,0,0,0],[0,0,0,0,1,0,0,0,0,0,0,0,1,0,0,1],[1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1],[1,0,0,1,0,0,1,0,0,1,0,0,0,1,0,0]],boom:[[1,0,0,0,0,0,0,0,1,0,0,0,0,0,0,0],[0,0,0,0,1,0,0,0,0,0,0,0,1,0,0,0],[0,1,0,1,0,1,0,1,0,1,0,1,0,1,0,1],[0,0,0,0,0,0,0,1,0,0,0,0,0,0,1,0]],drill:[[1,0,0,0,0,0,1,0,1,0,0,0,0,0,1,0],[0,0,0,0,1,0,0,0,0,0,0,0,1,0,0,0],[1,0,1,0,1,0,1,1,1,0,1,0,1,0,1,1],[1,0,0,0,1,0,0,1,1,0,0,0,1,0,1,0]],jersey:[[1,0,1,0,1,0,1,0,1,0,1,0,1,0,1,0],[0,0,1,0,0,0,1,0,0,0,1,0,0,0,1,0],[1,1,0,1,1,1,0,1,1,1,0,1,1,1,0,1],[0,1,0,0,0,1,0,0,0,1,0,0,0,1,0,0]]};
function toggleAuthMode(){authMode=authMode==='login'?'signup':'login';document.getElementById('auth-title').textContent=authMode==='login'?'LOGIN':'SIGN UP';document.getElementById('auth-submit').textContent=authMode==='login'?'LOGIN':'CREATE ACCOUNT';document.getElementById('auth-toggle-btn').textContent=authMode==='login'?'CREATE FREE ACCOUNT':'ALREADY HAVE AN ACCOUNT';document.getElementById('auth-username').style.display=authMode==='signup'?'block':'none';document.getElementById('auth-error').textContent='';}
async function submitAuth(){const email=document.getElementById('auth-email').value.trim(),password=document.getElementById('auth-password').value,username=document.getElementById('auth-username').value.trim(),errEl=document.getElementById('auth-error');errEl.textContent='';document.getElementById('auth-submit').textContent='...';if(authMode==='signup'){if(!username){errEl.textContent='Username required';document.getElementById('auth-submit').textContent='CREATE ACCOUNT';return;}const{data,error}=await db.auth.signUp({email,password});if(error){errEl.textContent=error.message;document.getElementById('auth-submit').textContent='CREATE ACCOUNT';return;}await db.from('profiles').insert({id:data.user.id,username,display_name:username});showToast('Account created!');setTimeout(()=>submitAuth(),1000);}else{const{data,error}=await db.auth.signInWithPassword({email,password});if(error){errEl.textContent=error.message;document.getElementById('auth-submit').textContent='LOGIN';return;}onLogin(data.user);}}
async function onLogin(user){currentUser=user;document.getElementById('auth-screen').style.display='none';document.getElementById('app').style.cssText='display:flex;flex-direction:column;height:100%;';document.getElementById('user-btn').textContent=user.email.split('@')[0].toUpperCase();buildVU();buildSeq();drawWaveform('a');drawWaveform('b');startLoops();showToast('Welcome to Jeremiahz Temple!');}
async function signOut(){await db.auth.signOut();currentUser=null;document.getElementById('auth-screen').style.display='flex';document.getElementById('app').style.display='none';}
window.onload=async()=>{const{data:{session}}=await db.auth.getSession();if(session)onLogin(session.user);};
function showSaveModal(){document.getElementById('save-modal').style.display='flex';}
function closeSaveModal(){document.getElementById('save-modal').style.display='none';}
async function confirmSave(){const title=document.getElementById('save-title').value.trim();if(!title){showToast('Enter a set name');return;}const{data:set,error}=await db.from('dj_sets').insert({user_id:currentUser.id,title,bpm:state.bpm}).select().single();if(error){showToast('Save failed');return;}await db.from('set_tracks').insert([{set_id:set.id,deck:'A',track_title:state.deckA.title,artist:state.deckA.artist,bpm:state.bpm,key_signature:state.deckA.key,position:0},{set_id:set.id,deck:'B',track_title:state.deckB.title,artist:state.deckB.artist,bpm:state.bpm,key_signature:state.deckB.key,position:1}]);closeSaveModal();document.getElementById('save-title').value='';showToast('Set saved to cloud!');}
async function loadSets(){if(!currentUser)return;const{data:sets}=await db.from('dj_sets').select('*').eq('user_id',currentUser.id).order('created_at',{ascending:false});const wrap=document.getElementById('sets-list');if(!sets||!sets.length){wrap.innerHTML='<div style="color:var(--dim);font-size:11px;text-align:center;padding:20px;">No sets yet. Mix and hit Save!</div>';return;}wrap.innerHTML='';sets.forEach(s=>{const d=new Date(s.created_at).toLocaleDateString(),row=document.createElement('div');row.className='set-row';row.innerHTML=`<div><div class="set-info-title">${s.title}</div><div class="set-info-sub">${s.bpm} BPM · ${d}</div></div><button class="load-set-btn" onclick="loadSetToDecks('${s.id}')">LOAD</button>`;wrap.appendChild(row);});const{data:patterns}=await db.from('patterns').select('*').eq('user_id',currentUser.id).order('created_at',{ascending:false});const pWrap=document.getElementById('patterns-list');if(!patterns||!patterns.length){pWrap.innerHTML='<div style="color:var(--dim);font-size:11px;text-align:center;padding:20px;">No patterns yet.</div>';return;}pWrap.innerHTML='';patterns.forEach(p=>{const row=document.createElement('div');row.className='set-row';row.innerHTML=`<div><div class="set-info-title">${p.name}</div><div class="set-info-sub">${p.bpm} BPM</div></div><button class="load-set-btn" onclick='loadPatternData(${JSON.stringify(p.steps)})'>LOAD</button>`;pWrap.appendChild(row);});}
async function loadSetToDecks(setId){const{data:tracks}=await db.from('set_tracks').select('*').eq('set_id',setId);tracks.forEach(t=>{if(t.deck==='A'){state.deckA.title=t.track_title;state.deckA.artist=t.artist||'';state.deckA.key=t.key_signature||'—';document.getElementById('da-title').textContent=t.track_title;document.getElementById('da-artist').textContent=t.artist||'';document.getElementById('da-key').textContent=t.key_signature||'—';}else{state.deckB.title=t.track_title;state.deckB.artist=t.artist||'';state.deckB.key=t.key_signature||'—';document.getElementById('db-title').textContent=t.track_title;document.getElementById('db-artist').textContent=t.artist||'';document.getElementById('db-key').textContent=t.key_signature||'—';}});switchTab('mixer');showToast('Set loaded!');}
function loadPatternData(steps){state.steps=steps;buildSeq();switchTab('beats');showToast('Pattern loaded!');}
async function refreshLibrary(){if(!currentUser)return;const{data:tracks}=await db.from('library').select('*').eq('user_id',currentUser.id).order('uploaded_at',{ascending:false});const wrap=document.getElementById('library-list');if(!tracks||!tracks.length){wrap.innerHTML='<div style="color:var(--dim);font-size:11px;text-align:center;padding:20px;">Library empty. Add tracks via Supabase.</div>';return;}wrap.innerHTML='';tracks.forEach((t)=>{const row=document.createElement('div');row.className='lib-track';row.innerHTML=`<div><div style="font-size:12px;color:#ddd;">${t.title}</div><div style="font-size:9px;color:var(--dim);">${t.artist||'Unknown'}</div></div><div style="display:flex;gap:4px;"><button style="padding:4px 8px;background:rgba(0,229,255,0.08);border:1px solid rgba(0,229,255,0.3);border-radius:4px;color:var(--cyan);font-size:8px;cursor:pointer;" onclick='loadLibTrack("a",${JSON.stringify(t)})'>→A</button><button style="padding:4px 8px;background:rgba(255,16,96,0.08);border:1px solid rgba(255,16,96,0.3);border-radius:4px;color:var(--pink);font-size:8px;cursor:pointer;" onclick='loadLibTrack("b",${JSON.stringify(t)})'>→B</button></div>`;wrap.appendChild(row);});}
function loadLibTrack(deck,t){if(deck==='a'){state.deckA.title=t.title;state.deckA.artist=t.artist||'';state.deckA.key=t.key_signature||'—';document.getElementById('da-title').textContent=t.title;document.getElementById('da-artist').textContent=t.artist||'';document.getElementById('da-key').textContent=t.key_signature||'—';}else{state.deckB.title=t.title;state.deckB.artist=t.artist||'';state.deckB.key=t.key_signature||'—';document.getElementById('db-title').textContent=t.title;document.getElementById('db-artist').textContent=t.artist||'';document.getElementById('db-key').textContent=t.key_signature||'—';}switchTab('mixer');showToast('Loaded to Deck '+deck.toUpperCase());}
async function savePattern(){if(!currentUser){showToast('Not logged in');return;}const name='Pattern '+new Date().toLocaleTimeString();const{error}=await db.from('patterns').insert({user_id:currentUser.id,name,bpm:state.bpm,steps:state.steps});if(error){showToast('Save failed');return;}showToast('Pattern saved!');}
function toggleDeck(deck){const d=deck==='a'?state.deckA:state.deckB;d.playing=!d.playing;const jog=document.getElementById('jog-'+deck);document.getElementById('jog-'+deck+'-icon').textContent=d.playing?'⏸':'▶';if(d.playing)jog.classList.add('playing');else jog.classList.remove('playing');}
function toggleLoop(deck){const d=deck==='a'?state.deckA:state.deckB;d.loop=!d.loop;document.getElementById('loop-'+deck).classList.toggle('on',d.loop);}
function syncDeck(deck){const btn=document.getElementById('sync-'+deck);btn.classList.add('on');setTimeout(()=>btn.classList.remove('on'),600);}
function updatePitch(deck,val){document.getElementById('pitch-'+deck+'-val').textContent=(val>0?'+':'')+parseFloat(val).toFixed(1)+'%';}
function updateBpm(val){state.bpm=parseInt(val);document.getElementById('bpm-display').textContent=val;document.getElementById('seq-bpm-val').textContent=val+' BPM';}
const wfData={a:[],b:[]};
function drawWaveform(deck){const c=document.getElementById('wf-'+deck),ctx=c.getContext('2d');c.width=c.offsetWidth*devicePixelRatio;c.height=44*devicePixelRatio;ctx.scale(devicePixelRatio,devicePixelRatio);const W=c.offsetWidth,H=44,prog=deck==='a'?state.deckA.progress:state.deckB.progress,col=deck==='a'?'#00e5ff':'#ff1060';if(!wfData[deck].length)for(let i=0;i<W;i++)wfData[deck].push(Math.random()*0.7+0.15);wfData[deck].forEach((v,i)=>{const h=v*(H/2);ctx.fillStyle=i/W<prog?col:'#1e1e2a';ctx.fillRect(i,H/2-h,1,h*2);});ctx.fillStyle='#fff';ctx.fillRect(Math.floor(prog*W)-1,0,2,H);}
function buildVU(){const wrap=document.getElementById('vu-meters');wrap.innerHTML='';[0,1].forEach(ch=>{const bar=document.createElement('div');bar.className='vu-bar';bar.id='vu-'+ch;for(let i=0;i<14;i++){const seg=document.createElement('div');seg.className='vu-seg';seg.id=`vu-${ch}-${i}`;seg.style.background='#141420';bar.appendChild(seg);}wrap.appendChild(bar);});}
function updateVU(){const playing=state.deckA.playing||state.deckB.playing||state.seqPlaying;if(playing){state.vuL=55+Math.random()*40;state.vuR=50+Math.random()*45;}else{state.vuL=Math.max(0,state.vuL-10);state.vuR=Math.max(0,state.vuR-10);}[0,1].forEach(ch=>{const v=ch===0?state.vuL:state.vuR;for(let i=0;i<14;i++){const seg=document.getElementById(`vu-${ch}-${i}`);if(!seg)return;const t=((13-i)/13)*100,col=i<2?'#ff0040':i<4?'#ffcc00':'#00e5ff';seg.style.background=v>=t?col:'#141420';}});}
const SEQ_LABELS=['KICK','SNARE','HH','808'],SEQ_CLASSES=['on-kick','on-snare','on-hh','on-808'];
function buildSeq(){const grid=document.getElementById('seq-grid');grid.innerHTML='';SEQ_LABELS.forEach((lbl,ri)=>{const row=document.createElement('div');row.className='seq-row';row.innerHTML=`<div class="seq-label">${lbl}</div><div class="seq-steps" id="seq-row-${ri}"></div>`;grid.appendChild(row);const stepsEl=document.getElementById('seq-row-'+ri);for(let si=0;si<16;si++){const btn=document.createElement('button');btn.className=`step-btn ${state.steps[ri][si]?SEQ_CLASSES[ri]:'off'}`;btn.id=`step-${ri}-${si}`;btn.onclick=()=>{state.steps[ri][si]=!state.steps[ri][si];btn.className=`step-btn ${state.steps[ri][si]?SEQ_CLASSES[ri]:'off'}`;};stepsEl.appendChild(btn);}});}
function loadPreset(name){state.steps=PRESETS[name].map(r=>[...r]);buildSeq();}
let seqInterval;
function toggleSeq(){state.seqPlaying=!state.seqPlaying;const btn=document.getElementById('seq-play-btn');if(state.seqPlaying){btn.textContent='■ STOP';btn.style.background='rgba(139,47,255,0.25)';let s=0;seqInterval=setInterval(()=>{if(state.currentStep>=0)SEQ_LABELS.forEach((_,ri)=>{const el=document.getElementById(`step-${ri}-${state.currentStep}`);if(el)el.style.outline='none';});state.currentStep=s;SEQ_LABELS.forEach((_,ri)=>{const el=document.getElementById(`step-${ri}-${s}`);if(el)el.style.outline='1px solid #666';});s=(s+1)%16;},(60000/state.bpm)/4);}else{clearInterval(seqInterval);btn.textContent='▶ PLAY';btn.style.background='rgba(139,47,255,0.1)';state.currentStep=-1;}}
function switchTab(tab){document.querySelectorAll('.panel').forEach(p=>p.classList.remove('active'));document.querySelectorAll('.nav-btn').forEach(b=>b.classList.remove('active'));document.getElementById('panel-'+tab).classList.add('active');document.getElementById('nav-'+tab).classList.add('active');}
function showToast(msg){const t=document.getElementById('toast');t.textContent=msg;t.classList.add('show');setTimeout(()=>t.classList.remove('show'),2500);}
function startLoops(){setInterval(()=>{updateVU();if(state.deckA.playing){state.deckA.progress=state.deckA.progress>=0.99?0:state.deckA.progress+0.0025;drawWaveform('a');}if(state.deckB.playing){state.deckB.progress=state.deckB.progress>=0.99?0:state.deckB.progress+0.002;drawWaveform('b');}},120);}
window.addEventListener('resize',()=>{drawWaveform('a');drawWaveform('b');});
</script>
</body>
</html>
