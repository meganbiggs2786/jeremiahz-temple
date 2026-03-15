<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
<title>JEREMIAHZ TEMPLE</title>
<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Share+Tech+Mono&display=swap" rel="stylesheet"/>
<style>
:root{--cyan:#00e5ff;--pink:#ff1060;--purple:#8b2fff;--gold:#ffcc00;--bg:#060608;--bg2:#0c0c10;--bg3:#121218;--border:#1e1e2a;--text:#d0d0e0;--dim:#444455;}
*{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent;user-select:none;-webkit-user-select:none;}
input,textarea{user-select:text;-webkit-user-select:text;}
html,body{height:100%;overflow:hidden;background:var(--bg);color:var(--text);font-family:'Share Tech Mono',monospace;}
#app{display:flex;flex-direction:column;height:100%;}
#auth-screen{position:fixed;inset:0;background:var(--bg);z-index:1000;display:flex;flex-direction:column;align-items:center;justify-content:center;padding:24px;}
.auth-logo{font-family:'Orbitron',sans-serif;font-size:28px;font-weight:900;color:#fff;letter-spacing:6px;margin-bottom:6px;text-shadow:0 0 20px var(--purple);}
.auth-sub{font-size:10px;color:var(--dim);letter-spacing:4px;margin-bottom:30px;}
.auth-card{width:100%;max-width:340px;background:var(--bg2);border:1px solid var(--purple);border-radius:14px;padding:24px;box-shadow:0 0 30px rgba(139,47,255,0.2);}
.auth-title{font-family:'Orbitron',sans-serif;font-size:11px;color:var(--cyan);letter-spacing:3px;margin-bottom:16px;text-align:center;}
.auth-input{width:100%;background:var(--bg3);border:1px solid var(--border);border-radius:8px;padding:14px;color:#fff;font-size:14px;margin-bottom:10px;outline:none;display:block;-webkit-appearance:none;}
.auth-input:focus{border-color:var(--cyan);}
.auth-error{color:var(--pink);font-size:11px;text-align:center;margin-bottom:8px;min-height:18px;}
.auth-btn{width:100%;padding:16px;border-radius:8px;font-family:'Orbitron',sans-serif;font-size:12px;font-weight:700;letter-spacing:2px;text-align:center;cursor:pointer;display:block;-webkit-appearance:none;appearance:none;border:none;margin-bottom:12px;touch-action:manipulation;}
#header{flex-shrink:0;display:flex;align-items:center;justify-content:space-between;padding:10px 14px;background:var(--bg);border-bottom:1px solid var(--border);}
.logo-text{font-family:'Orbitron',sans-serif;font-size:10px;font-weight:900;color:#fff;letter-spacing:2px;}
.bpm-val{font-family:'Orbitron',sans-serif;font-size:20px;font-weight:900;color:#fff;}
.user-btn{padding:8px 14px;background:transparent;border:1px solid var(--border);border-radius:6px;color:var(--dim);font-size:9px;cursor:pointer;-webkit-appearance:none;touch-action:manipulation;}
#content{flex:1;overflow-y:auto;overflow-x:hidden;-webkit-overflow-scrolling:touch;padding-bottom:70px;}
#nav{position:fixed;bottom:0;left:0;right:0;height:60px;background:var(--bg);border-top:1px solid var(--border);display:flex;z-index:100;}
.nav-btn{flex:1;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:3px;background:transparent;border:none;cursor:pointer;color:var(--dim);font-size:7px;touch-action:manipulation;-webkit-appearance:none;letter-spacing:1px;}
.nav-btn.active{color:var(--cyan);}
.nav-icon{font-size:16px;}
.panel{padding:12px;display:none;}
.panel.active{display:block;}
.card{background:var(--bg2);border:1px solid var(--border);border-radius:10px;padding:14px;margin-bottom:12px;}
.deck{border-radius:10px;padding:12px;margin-bottom:12px;}
.deck-a{background:linear-gradient(135deg,#0a0a12,#0c0c18);border:1px solid rgba(0,229,255,0.3);box-shadow:0 0 20px rgba(0,229,255,0.05);}
.deck-b{background:linear-gradient(135deg,#120a0a,#180c0c);border:1px solid rgba(255,16,96,0.3);box-shadow:0 0 20px rgba(255,16,96,0.05);}
.waveform-wrap{height:50px;background:#040408;border-radius:6px;overflow:hidden;margin-bottom:10px;border:1px solid var(--border);}
.waveform-canvas{width:100%;height:100%;display:block;}

/* VINYL JOG WHEEL */
.jog-wrap{display:flex;justify-content:center;margin:12px 0;}
.jog-outer{width:130px;height:130px;border-radius:50%;position:relative;cursor:pointer;touch-action:manipulation;}
.vinyl{width:100%;height:100%;border-radius:50%;background:
  radial-gradient(circle at 50% 50%, #1a1a1a 0%, #1a1a1a 18%,
  transparent 18%, transparent 20%,
  #111 20%, #111 22%, transparent 22%, transparent 24%,
  #111 24%, #111 26%, transparent 26%, transparent 28%,
  #111 28%, #111 30%, transparent 30%, transparent 32%,
  #111 32%, #111 34%, transparent 34%, transparent 36%,
  #111 36%, #111 38%, transparent 38%, transparent 40%,
  #111 40%, #111 42%, transparent 42%, transparent 44%,
  #111 44%, #111 46%, transparent 46%, transparent 100%),
  conic-gradient(from 0deg, #0d0d0d, #1a1a1a, #0d0d0d, #1a1a1a, #0d0d0d);
position:relative;transition:box-shadow 0.3s;}
.vinyl::after{content:'';position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);width:24%;height:24%;border-radius:50%;background:radial-gradient(circle,var(--purple),#3a0080);box-shadow:0 0 10px var(--purple);}
.vinyl-a{box-shadow:0 0 0 2px rgba(0,229,255,0.3);}
.vinyl-b{box-shadow:0 0 0 2px rgba(255,16,96,0.3);}
.vinyl-a.spinning{animation:spin-vinyl 2s linear infinite;box-shadow:0 0 15px rgba(0,229,255,0.5),0 0 0 2px var(--cyan);}
.vinyl-b.spinning{animation:spin-vinyl 2s linear infinite;box-shadow:0 0 15px rgba(255,16,96,0.5),0 0 0 2px var(--pink);}

.ctrl-row{display:flex;gap:6px;margin-bottom:10px;}
.ctrl-btn{flex:1;padding:10px 4px;background:#0c0c14;border:1px solid var(--border);border-radius:6px;color:var(--dim);font-size:8px;cursor:pointer;text-align:center;touch-action:manipulation;-webkit-appearance:none;letter-spacing:1px;}
.ctrl-btn.on{border-color:var(--cyan);color:var(--cyan);background:rgba(0,229,255,0.08);}
.ctrl-btn-b.on{border-color:var(--pink);color:var(--pink);background:rgba(255,16,96,0.08);}
.fader-label{display:flex;justify-content:space-between;font-size:8px;color:var(--dim);margin-bottom:4px;letter-spacing:1px;}
input[type=range]{-webkit-appearance:none;width:100%;height:6px;border-radius:3px;background:#1e1e2a;outline:none;cursor:pointer;}
input[type=range]::-webkit-slider-thumb{-webkit-appearance:none;width:22px;height:22px;border-radius:50%;background:var(--cyan);cursor:pointer;box-shadow:0 0 8px rgba(0,229,255,0.5);}
.range-pink::-webkit-slider-thumb{background:var(--pink);box-shadow:0 0 8px rgba(255,16,96,0.5);}
.range-purple::-webkit-slider-thumb{background:var(--purple);box-shadow:0 0 8px rgba(139,47,255,0.5);}
.range-white::-webkit-slider-thumb{background:#fff;}
.vu-row{display:flex;gap:2px;height:50px;justify-content:center;margin-bottom:10px;}
.vu-bar{width:8px;height:100%;display:flex;flex-direction:column;justify-content:flex-end;gap:1px;}
.vu-seg{flex:1;border-radius:1px;}
.set-row{display:flex;justify-content:space-between;align-items:center;padding:10px 0;border-bottom:1px solid #0f0f18;}
.load-set-btn{padding:7px 14px;background:rgba(0,229,255,0.08);border:1px solid rgba(0,229,255,0.3);border-radius:4px;color:var(--cyan);font-size:8px;cursor:pointer;touch-action:manipulation;}
.seq-grid{display:flex;flex-direction:column;gap:5px;}
.seq-row-el{display:flex;align-items:center;gap:4px;}
.seq-label{font-size:8px;color:var(--dim);width:36px;flex-shrink:0;}
.seq-steps{display:flex;gap:2px;flex:1;}
.step-btn{flex:1;height:26px;border-radius:3px;border:none;cursor:pointer;touch-action:manipulation;}
.step-btn.off{background:#141420;}
.step-btn.on-kick{background:var(--cyan);box-shadow:0 0 4px rgba(0,229,255,0.5);}
.step-btn.on-snare{background:var(--pink);box-shadow:0 0 4px rgba(255,16,96,0.5);}
.step-btn.on-hh{background:var(--purple);box-shadow:0 0 4px rgba(139,47,255,0.5);}
.step-btn.on-808{background:var(--gold);box-shadow:0 0 4px rgba(255,204,0,0.5);}
.lib-track{display:flex;justify-content:space-between;align-items:center;padding:10px 0;border-bottom:1px solid #0f0f18;}
.toast{position:fixed;bottom:80px;left:50%;transform:translateX(-50%);background:var(--bg2);border:1px solid var(--cyan);border-radius:8px;padding:10px 20px;font-size:11px;color:var(--cyan);z-index:999;opacity:0;transition:opacity 0.3s;pointer-events:none;white-space:nowrap;box-shadow:0 0 20px rgba(0,229,255,0.3);}
.toast.show{opacity:1;}
.modal-bg{position:fixed;inset:0;background:rgba(0,0,0,0.85);z-index:500;display:flex;align-items:center;justify-content:center;padding:20px;}
.modal{background:var(--bg2);border:1px solid var(--purple);border-radius:14px;padding:20px;width:100%;max-width:320px;box-shadow:0 0 30px rgba(139,47,255,0.3);}
.modal-input{width:100%;background:var(--bg3);border:1px solid var(--border);border-radius:8px;padding:12px;color:#fff;font-size:14px;margin-bottom:10px;outline:none;-webkit-appearance:none;}
.platform-row{display:flex;justify-content:space-between;align-items:center;padding:12px;background:var(--bg3);border-radius:8px;border:1px solid var(--border);margin-bottom:8px;}
.live-btn{padding:8px 16px;border-radius:5px;border:1px solid var(--border);color:var(--dim);font-family:'Share Tech Mono',monospace;font-size:9px;letter-spacing:2px;cursor:pointer;background:transparent;touch-action:manipulation;}
.live-btn.active{border-color:var(--pink);color:var(--pink);background:rgba(255,16,96,0.1);box-shadow:0 0 10px rgba(255,16,96,0.3);}
.ai-tool-card{background:var(--bg2);border-radius:10px;overflow:hidden;margin-bottom:10px;}
.ai-tool-header{padding:12px;display:flex;justify-content:space-between;align-items:center;}
.ai-open-btn{padding:10px 18px;border:none;border-radius:8px;font-family:'Orbitron',sans-serif;font-size:10px;cursor:pointer;font-weight:700;touch-action:manipulation;}
.section-title{font-family:'Orbitron',sans-serif;font-size:9px;letter-spacing:3px;margin-bottom:10px;}
@keyframes spin-vinyl{to{transform:rotate(360deg)}}
@keyframes blink{0%,100%{opacity:1}50%{opacity:0.3}}
@keyframes glow-pulse{0%,100%{box-shadow:0 0 10px rgba(0,229,255,0.3)}50%{box-shadow:0 0 25px rgba(0,229,255,0.7)}}
</style>
</head>
<body>
<div class="toast" id="toast"></div>
<div id="auth-screen">
  <div class="auth-logo">JEREMIAHZ</div>
  <div class="auth-sub">TEMPLE</div>
  <div class="auth-card">
    <div class="auth-title" id="auth-title">LOGIN</div>
    <div class="auth-error" id="auth-error"></div>
    <input class="auth-input" id="auth-username" placeholder="Username" style="display:none" type="text" autocomplete="off"/>
    <input class="auth-input" id="auth-email" type="email" placeholder="Email address" autocomplete="email"/>
    <input class="auth-input" id="auth-password" type="password" placeholder="Password (min 6 chars)" autocomplete="current-password"/>
    <button class="auth-btn" id="auth-submit" style="background:linear-gradient(135deg,var(--cyan),var(--purple));color:#000;" onclick="submitAuth()">LOGIN</button>
    <button class="auth-btn" id="auth-toggle-btn" style="background:transparent;border:2px solid var(--cyan);color:var(--cyan);margin-bottom:0;" onclick="toggleAuthMode()">CREATE FREE ACCOUNT</button>
  </div>
</div>
<div class="modal-bg" id="save-modal" style="display:none;">
  <div class="modal">
    <div class="section-title" style="color:var(--cyan);">💾 SAVE SET</div>
    <input class="modal-input" id="save-title" placeholder="Set name e.g. Friday Night Mix" type="text"/>
    <button class="auth-btn" onclick="confirmSave()" style="background:linear-gradient(135deg,var(--cyan),var(--purple));color:#000;">SAVE TO CLOUD</button>
    <button class="auth-btn" onclick="closeSaveModal()" style="background:transparent;border:1px solid var(--border);color:var(--dim);margin-bottom:0;">Cancel</button>
  </div>
</div>
<div id="app" style="display:none;flex-direction:column;height:100%;">
  <div id="header">
    <div>
      <div class="logo-text">JEREMIAHZ TEMPLE</div>
      <div style="font-size:7px;color:var(--purple);letter-spacing:2px;">DJ STUDIO PRO</div>
    </div>
    <div style="text-align:center;">
      <div class="bpm-val" id="bpm-display">130</div>
      <div style="font-size:7px;color:var(--dim);letter-spacing:3px;">BPM</div>
    </div>
    <button class="user-btn" id="user-btn" onclick="signOut()">SIGN OUT</button>
  </div>
  <div id="content">

    <!-- MIXER -->
    <div class="panel active" id="panel-mixer">
      <div class="deck deck-a" style="margin-top:4px;">
        <div style="display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:8px;">
          <div>
            <div style="font-size:9px;color:var(--cyan);letter-spacing:2px;margin-bottom:3px;">◈ DECK A</div>
            <div style="font-size:13px;font-weight:700;color:#fff;margin-bottom:2px;" id="da-title">NO TRACK LOADED</div>
            <div style="font-size:10px;color:var(--dim);" id="da-artist">Load from library</div>
          </div>
          <div style="text-align:right;">
            <div style="font-size:7px;color:var(--dim);">KEY</div>
            <div style="font-family:'Orbitron',sans-serif;font-size:14px;color:var(--cyan);" id="da-key">—</div>
          </div>
        </div>
        <div class="waveform-wrap"><canvas class="waveform-canvas" id="wf-a" height="50"></canvas></div>
        <div class="jog-wrap">
          <div class="jog-outer" onclick="toggleDeck('a')">
            <div class="vinyl vinyl-a" id="vinyl-a"></div>
          </div>
        </div>
        <div class="ctrl-row">
          <button class="ctrl-btn" id="cue-a" onclick="tapCue('a')">⟨ CUE</button>
          <button class="ctrl-btn" id="loop-a" onclick="toggleLoop('a')">◉ LOOP</button>
          <button class="ctrl-btn" id="sync-a" onclick="syncDeck('a')">⇄ SYNC</button>
          <button class="ctrl-btn" onclick="showSaveModal()">💾 SAVE</button>
        </div>
        <div class="fader-label"><span>PITCH A</span><span id="pitch-a-val">0.0%</span></div>
        <input type="range" min="-12" max="12" step="0.1" value="0" oninput="updatePitch('a',this.value)"/>
        <div style="margin-top:10px;display:flex;gap:8px;">
          <div style="flex:1;">
            <div class="fader-label"><span>HI</span></div>
            <input type="range" min="0" max="100" value="75"/>
          </div>
          <div style="flex:1;">
            <div class="fader-label"><span>MID</span></div>
            <input type="range" min="0" max="100" value="70"/>
          </div>
          <div style="flex:1;">
            <div class="fader-label"><span>LO</span></div>
            <input type="range" min="0" max="100" value="72"/>
          </div>
        </div>
      </div>

      <!-- MASTER -->
      <div class="card">
        <div class="section-title" style="color:var(--cyan);">MASTER</div>
        <div class="vu-row" id="vu-meters"></div>
        <div class="fader-label"><span style="color:var(--cyan);">A</span><span style="color:var(--dim);">CROSSFADER</span><span style="color:var(--pink);">B</span></div>
        <input type="range" min="0" max="100" value="50" class="range-white"/>
        <div style="margin-top:10px;">
          <div class="fader-label"><span>MASTER VOL</span><span id="master-val">80</span></div>
          <input type="range" min="0" max="100" value="80" oninput="document.getElementById('master-val').textContent=this.value"/>
        </div>
      </div>

      <!-- DECK B -->
      <div class="deck deck-b">
        <div style="display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:8px;">
          <div>
            <div style="font-size:9px;color:var(--pink);letter-spacing:2px;margin-bottom:3px;">◈ DECK B</div>
            <div style="font-size:13px;font-weight:700;color:#fff;margin-bottom:2px;" id="db-title">NO TRACK LOADED</div>
            <div style="font-size:10px;color:var(--dim);" id="db-artist">Load from library</div>
          </div>
          <div style="text-align:right;">
            <div style="font-size:7px;color:var(--dim);">KEY</div>
            <div style="font-family:'Orbitron',sans-serif;font-size:14px;color:var(--pink);" id="db-key">—</div>
          </div>
        </div>
        <div class="waveform-wrap"><canvas class="waveform-canvas" id="wf-b" height="50"></canvas></div>
        <div class="jog-wrap">
          <div class="jog-outer" onclick="toggleDeck('b')">
            <div class="vinyl vinyl-b" id="vinyl-b"></div>
          </div>
        </div>
        <div class="ctrl-row">
          <button class="ctrl-btn ctrl-btn-b" id="cue-b" onclick="tapCue('b')">⟨ CUE</button>
          <button class="ctrl-btn ctrl-btn-b" id="loop-b" onclick="toggleLoop('b')">◉ LOOP</button>
          <button class="ctrl-btn ctrl-btn-b" id="sync-b" onclick="syncDeck('b')">⇄ SYNC</button>
          <button class="ctrl-btn ctrl-btn-b" onclick="showSaveModal()">💾 SAVE</button>
        </div>
        <div class="fader-label"><span>PITCH B</span><span id="pitch-b-val">0.0%</span></div>
        <input type="range" min="-12" max="12" step="0.1" value="0" class="range-pink" oninput="updatePitch('b',this.value)"/>
        <div style="margin-top:10px;display:flex;gap:8px;">
          <div style="flex:1;">
            <div class="fader-label"><span>HI</span></div>
            <input type="range" min="0" max="100" value="68" class="range-pink"/>
          </div>
          <div style="flex:1;">
            <div class="fader-label"><span>MID</span></div>
            <input type="range" min="0" max="100" value="74" class="range-pink"/>
          </div>
          <div style="flex:1;">
            <div class="fader-label"><span>LO</span></div>
            <input type="range" min="0" max="100" value="70" class="range-pink"/>
          </div>
        </div>
      </div>
    </div>
    <!-- BEATS -->
    <div class="panel" id="panel-beats">
      <div class="card">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;">
          <div class="section-title" style="color:var(--purple);margin:0;">STEP SEQUENCER</div>
          <div style="display:flex;gap:6px;">
            <button id="seq-play-btn" onclick="toggleSeq()" style="padding:8px 14px;background:rgba(139,47,255,0.1);border:1px solid var(--purple);border-radius:6px;color:var(--purple);font-size:10px;letter-spacing:2px;cursor:pointer;touch-action:manipulation;">▶ PLAY</button>
            <button onclick="savePattern()" style="padding:8px 10px;background:rgba(0,229,255,0.08);border:1px solid rgba(0,229,255,0.3);border-radius:6px;color:var(--cyan);font-size:9px;cursor:pointer;touch-action:manipulation;">💾</button>
          </div>
        </div>
        <div class="fader-label"><span>TEMPO</span><span id="seq-bpm-val">130 BPM</span></div>
        <input type="range" min="60" max="200" value="130" class="range-purple" oninput="updateBpm(this.value)" style="margin-bottom:12px;"/>
        <div class="seq-grid" id="seq-grid"></div>
        <div style="display:flex;gap:6px;margin-top:10px;overflow-x:auto;padding-bottom:4px;">
          <button onclick="loadPreset('trap')" style="padding:8px 12px;background:transparent;border:1px solid var(--border);border-radius:4px;color:var(--dim);font-size:8px;cursor:pointer;white-space:nowrap;touch-action:manipulation;">TRAP</button>
          <button onclick="loadPreset('boom')" style="padding:8px 12px;background:transparent;border:1px solid var(--border);border-radius:4px;color:var(--dim);font-size:8px;cursor:pointer;white-space:nowrap;touch-action:manipulation;">BOOM BAP</button>
          <button onclick="loadPreset('drill')" style="padding:8px 12px;background:transparent;border:1px solid var(--border);border-radius:4px;color:var(--dim);font-size:8px;cursor:pointer;white-space:nowrap;touch-action:manipulation;">DRILL</button>
          <button onclick="loadPreset('jersey')" style="padding:8px 12px;background:transparent;border:1px solid var(--border);border-radius:4px;color:var(--dim);font-size:8px;cursor:pointer;white-space:nowrap;touch-action:manipulation;">JERSEY</button>
        </div>
      </div>
    </div>

    <!-- SETS -->
    <div class="panel" id="panel-sets">
      <div class="card">
        <div class="section-title" style="color:var(--cyan);">☁ MY SAVED SETS</div>
        <div id="sets-list"><div style="color:var(--dim);font-size:11px;text-align:center;padding:20px;">Loading your sets...</div></div>
      </div>
      <div class="card">
        <div class="section-title" style="color:var(--purple);">📋 MY PATTERNS</div>
        <div id="patterns-list"><div style="color:var(--dim);font-size:11px;text-align:center;padding:20px;">Loading patterns...</div></div>
      </div>
    </div>

    <!-- LIBRARY -->
    <div class="panel" id="panel-library">
      <div class="card">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;">
          <div class="section-title" style="color:var(--cyan);margin:0;">🎵 MY LIBRARY</div>
          <button onclick="refreshLibrary()" style="padding:7px 10px;background:transparent;border:1px solid var(--border);border-radius:4px;color:var(--dim);font-size:8px;cursor:pointer;touch-action:manipulation;">↻ REFRESH</button>
        </div>
        <div id="library-list"><div style="color:var(--dim);font-size:11px;text-align:center;padding:20px;">Loading library...</div></div>
      </div>
    </div>

    <!-- AI STUDIO -->
    <div class="panel" id="panel-ai">
      <div style="font-family:'Orbitron',sans-serif;font-size:9px;color:var(--cyan);letter-spacing:3px;margin-bottom:12px;">⚡ AI STUDIO</div>

      <!-- SUNO -->
      <div class="ai-tool-card" style="border:2px solid rgba(139,47,255,0.6);">
        <div class="ai-tool-header" style="background:rgba(139,47,255,0.2);">
          <div>
            <div style="font-family:'Orbitron',sans-serif;font-size:12px;color:var(--purple);letter-spacing:2px;">⭐ SUNO AI</div>
            <div style="font-size:8px;color:#aaa;margin-top:3px;">Best beats · Free · Sign in with Google</div>
          </div>
          <button class="ai-open-btn" style="background:linear-gradient(135deg,var(--purple),#4400bb);color:#fff;" onclick="window.open('https://suno.com','_blank')">OPEN ↗</button>
        </div>
        <div style="padding:12px;">
          <div style="font-size:8px;color:var(--dim);letter-spacing:1px;margin-bottom:8px;">📋 TAP PROMPT TO COPY → PASTE IN SUNO:</div>
          <div id="beat-chips"></div>
        </div>
      </div>

      <!-- UDIO -->
      <div class="ai-tool-card" style="border:1px solid rgba(0,229,255,0.3);">
        <div class="ai-tool-header" style="background:rgba(0,229,255,0.08);">
          <div>
            <div style="font-family:'Orbitron',sans-serif;font-size:11px;color:var(--cyan);letter-spacing:2px;">UDIO</div>
            <div style="font-size:8px;color:#aaa;margin-top:2px;">Rap & trap beats · Free tier</div>
          </div>
          <button class="ai-open-btn" style="background:linear-gradient(135deg,var(--cyan),#007799);color:#000;" onclick="window.open('https://udio.com','_blank')">OPEN ↗</button>
        </div>
      </div>

      <!-- STEM SEPARATOR -->
      <div class="ai-tool-card" style="border:1px solid rgba(255,204,0,0.3);">
        <div class="ai-tool-header" style="background:rgba(255,204,0,0.06);">
          <div>
            <div style="font-family:'Orbitron',sans-serif;font-size:11px;color:var(--gold);letter-spacing:2px;">STEM SEPARATOR</div>
            <div style="font-size:8px;color:#aaa;margin-top:2px;">Split vocals · drums · bass · Free</div>
          </div>
          <button class="ai-open-btn" style="background:linear-gradient(135deg,var(--gold),#886600);color:#000;" onclick="window.open('https://splitter.ai','_blank')">OPEN ↗</button>
        </div>
      </div>

      <!-- VOCAL REMOVER -->
      <div class="ai-tool-card" style="border:1px solid rgba(255,16,96,0.3);">
        <div class="ai-tool-header" style="background:rgba(255,16,96,0.06);">
          <div>
            <div style="font-family:'Orbitron',sans-serif;font-size:11px;color:var(--pink);letter-spacing:2px;">VOCAL REMOVER</div>
            <div style="font-size:8px;color:#aaa;margin-top:2px;">Remove vocals · get instrumentals · Free</div>
          </div>
          <button class="ai-open-btn" style="background:linear-gradient(135deg,var(--pink),#880033);color:#fff;" onclick="window.open('https://vocalremover.org','_blank')">OPEN ↗</button>
        </div>
      </div>
    </div>

    <!-- LIVE STREAM -->
    <div class="panel" id="panel-live">
      <div class="card">
        <div class="section-title" style="color:var(--pink);">📡 GO LIVE</div>
        <div id="platforms-list"></div>
        <div style="margin-top:8px;padding:10px;background:var(--bg3);border-radius:6px;border:1px solid var(--border);">
          <div style="font-size:8px;color:var(--dim);letter-spacing:2px;margin-bottom:6px;">STREAM KEY</div>
          <input type="text" placeholder="Paste your stream key here" style="width:100%;background:var(--bg);border:1px solid var(--border);border-radius:4px;padding:8px 10px;color:var(--dim);font-family:'Share Tech Mono',monospace;font-size:10px;outline:none;"/>
        </div>
      </div>
      <div class="card">
        <div class="section-title" style="color:var(--pink);">⬇ FREE DOWNLOADS</div>
        <div style="font-size:10px;color:var(--dim);margin-bottom:12px;line-height:1.8;">Get free beats, samples and loops from these trusted sites:</div>
        <div id="download-sites"></div>
      </div>
    </div>

  </div>

  <!-- NAV -->
  <nav id="nav">
    <button class="nav-btn active" id="nav-mixer" onclick="switchTab('mixer')"><div class="nav-icon">🎚</div><span>MIXER</span></button>
    <button class="nav-btn" id="nav-beats" onclick="switchTab('beats')"><div class="nav-icon">🥁</div><span>BEATS</span></button>
    <button class="nav-btn" id="nav-ai" onclick="switchTab('ai');buildBeatChips();"><div class="nav-icon">🤖</div><span>AI</span></button>
    <button class="nav-btn" id="nav-live" onclick="switchTab('live');"><div class="nav-icon">📡</div><span>LIVE</span></button>
    <button class="nav-btn" id="nav-sets" onclick="switchTab('sets');loadSets();"><div class="nav-icon">☁</div><span>SETS</span></button>
    <button class="nav-btn" id="nav-library" onclick="switchTab('library');refreshLibrary();"><div class="nav-icon">🎵</div><span>LIBRARY</span></button>
  </nav>
</div>
<script>
const{createClient}=supabase;
const db=createClient('https://ucqokaspslumaixtkanf.supabase.co','eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InVjcW9rYXNwc2x1bWFpeHRrYW5mIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NzI5OTE5OTQsImV4cCI6MjA4ODU2Nzk5NH0.TzsdYRI4WN8HcKWU7AD0XsAdlh2D8cVkpTyEUVQKPn0');
let currentUser=null,authMode='login';
const state={bpm:130,deckA:{playing:false,progress:0.2,loop:false,title:'NO TRACK LOADED',artist:'',key:'—'},deckB:{playing:false,progress:0.4,loop:false,title:'NO TRACK LOADED',artist:'',key:'—'},seqPlaying:false,currentStep:-1,steps:[[1,0,0,0,1,0,0,0,1,0,0,0,1,0,0,0],[0,0,0,0,1,0,0,0,0,0,0,0,1,0,0,1],[1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1],[0,0,0,1,0,0,0,0,0,0,1,0,0,0,0,0]],vuL:0,vuR:0};
const PRESETS={trap:[[1,0,0,0,1,0,0,0,1,0,0,0,1,0,0,0],[0,0,0,0,1,0,0,0,0,0,0,0,1,0,0,1],[1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1],[1,0,0,1,0,0,1,0,0,1,0,0,0,1,0,0]],boom:[[1,0,0,0,0,0,0,0,1,0,0,0,0,0,0,0],[0,0,0,0,1,0,0,0,0,0,0,0,1,0,0,0],[0,1,0,1,0,1,0,1,0,1,0,1,0,1,0,1],[0,0,0,0,0,0,0,1,0,0,0,0,0,0,1,0]],drill:[[1,0,0,0,0,0,1,0,1,0,0,0,0,0,1,0],[0,0,0,0,1,0,0,0,0,0,0,0,1,0,0,0],[1,0,1,0,1,0,1,1,1,0,1,0,1,0,1,1],[1,0,0,0,1,0,0,1,1,0,0,0,1,0,1,0]],jersey:[[1,0,1,0,1,0,1,0,1,0,1,0,1,0,1,0],[0,0,1,0,0,0,1,0,0,0,1,0,0,0,1,0],[1,1,0,1,1,1,0,1,1,1,0,1,1,1,0,1],[0,1,0,0,0,1,0,0,0,1,0,0,0,1,0,0]]};
const PLATFORMS=[{name:'TWITCH',icon:'📡',col:'#9146ff',url:'https://twitch.tv'},{name:'YOUTUBE',icon:'▶',col:'#ff0000',url:'https://youtube.com'},{name:'TIKTOK',icon:'♪',col:'#00e5ff',url:'https://tiktok.com'},{name:'INSTAGRAM',icon:'◎',col:'#e040fb',url:'https://instagram.com'}];
const DOWNLOAD_SITES=[{name:'SPLICE',desc:'Free samples & loops',url:'https://splice.com'},{name:'LOOPERMAN',desc:'Free loops & acapellas',url:'https://looperman.com'},{name:'FREESOUND',desc:'Free sound effects & samples',url:'https://freesound.org'},{name:'CYMATICS',desc:'Free trap & EDM samples',url:'https://cymatics.fm/pages/free-download'},{name:'LANDR SAMPLES',desc:'Free sample packs',url:'https://samples.landr.com'}];
const BEAT_CHIPS=['dark trap beat heavy 808s 140bpm rap no melody','boom bap hip hop 90bpm hard drums classic rap','melodic trap beat emotional 140bpm piano sad','drill beat aggressive 145bpm dark uk drill bass','jersey club beat 140bpm bouncy fast percussion','hard rap beat 130bpm heavy kick snare no melody'];

function toggleAuthMode(){
  authMode=authMode==='login'?'signup':'login';
  document.getElementById('auth-title').textContent=authMode==='login'?'LOGIN':'SIGN UP';
  document.getElementById('auth-submit').textContent=authMode==='login'?'LOGIN':'CREATE ACCOUNT';
  document.getElementById('auth-toggle-btn').textContent=authMode==='login'?'CREATE FREE ACCOUNT':'BACK TO LOGIN';
  document.getElementById('auth-username').style.display=authMode==='signup'?'block':'none';
  document.getElementById('auth-error').textContent='';
}
async function submitAuth(){
  const email=document.getElementById('auth-email').value.trim();
  const password=document.getElementById('auth-password').value.trim();
  const username=document.getElementById('auth-username').value.trim();
  const errEl=document.getElementById('auth-error');
  errEl.textContent='';
  if(!email){errEl.textContent='Please enter your email';return;}
  if(!password||password.length<6){errEl.textContent='Password must be at least 6 characters';return;}
  if(authMode==='signup'&&!username){errEl.textContent='Please enter a username';return;}
  document.getElementById('auth-submit').textContent='LOADING...';
  if(authMode==='signup'){
    const{data,error}=await db.auth.signUp({email,password});
    if(error){errEl.textContent=error.message;document.getElementById('auth-submit').textContent='CREATE ACCOUNT';return;}
    await db.from('profiles').insert({id:data.user.id,username,display_name:username});
    const{data:d2,error:e2}=await db.auth.signInWithPassword({email,password});
    if(e2){errEl.textContent='Account created! Please login.';document.getElementById('auth-submit').textContent='LOGIN';toggleAuthMode();return;}
    onLogin(d2.user);
  }else{
    const{data,error}=await db.auth.signInWithPassword({email,password});
    if(error){errEl.textContent=error.message;document.getElementById('auth-submit').textContent='LOGIN';return;}
    onLogin(data.user);
  }
}
async function onLogin(user){
  currentUser=user;
  document.getElementById('auth-screen').style.display='none';
  const app=document.getElementById('app');
  app.style.display='flex';app.style.flexDirection='column';app.style.height='100%';
  document.getElementById('user-btn').textContent=user.email.split('@')[0].toUpperCase();
  buildVU();buildSeq();buildPlatforms();buildDownloadSites();
  drawWaveform('a');drawWaveform('b');startLoops();
  showToast('Welcome to Jeremiahz Temple! 🎛');
}
async function signOut(){await db.auth.signOut();currentUser=null;document.getElementById('auth-screen').style.display='flex';document.getElementById('app').style.display='none';}
window.onload=async()=>{const{data:{session}}=await db.auth.getSession();if(session)onLogin(session.user);};
function showSaveModal(){document.getElementById('save-modal').style.display='flex';}
function closeSaveModal(){document.getElementById('save-modal').style.display='none';}
async function confirmSave(){
  const title=document.getElementById('save-title').value.trim();
  if(!title){showToast('Enter a set name');return;}
  const{data:set,error}=await db.from('dj_sets').insert({user_id:currentUser.id,title,bpm:state.bpm}).select().single();
  if(error){showToast('Save failed: '+error.message);return;}
  await db.from('set_tracks').insert([
    {set_id:set.id,deck:'A',track_title:state.deckA.title,artist:state.deckA.artist,bpm:state.bpm,key_signature:state.deckA.key,position:0},
    {set_id:set.id,deck:'B',track_title:state.deckB.title,artist:state.deckB.artist,bpm:state.bpm,key_signature:state.deckB.key,position:1}
  ]);
  closeSaveModal();document.getElementById('save-title').value='';showToast('Set saved to cloud! ☁');
}
async function loadSets(){
  if(!currentUser)return;
  const{data:sets}=await db.from('dj_sets').select('*').eq('user_id',currentUser.id).order('created_at',{ascending:false});
  const wrap=document.getElementById('sets-list');
  if(!sets||!sets.length){wrap.innerHTML='<div style="color:var(--dim);font-size:11px;text-align:center;padding:20px;">No sets yet. Mix and hit Save!</div>';return;}
  wrap.innerHTML='';
  sets.forEach(s=>{
    const row=document.createElement('div');row.className='set-row';
    row.innerHTML=`<div><div style="font-size:12px;color:#ddd;">${s.title}</div><div style="font-size:9px;color:var(--dim);">${s.bpm} BPM · ${new Date(s.created_at).toLocaleDateString()}</div></div><button class="load-set-btn" onclick="loadSetToDecks('${s.id}')">LOAD</button>`;
    wrap.appendChild(row);
  });
  const{data:patterns}=await db.from('patterns').select('*').eq('user_id',currentUser.id).order('created_at',{ascending:false});
  const pWrap=document.getElementById('patterns-list');
  if(!patterns||!patterns.length){pWrap.innerHTML='<div style="color:var(--dim);font-size:11px;text-align:center;padding:20px;">No patterns yet.</div>';return;}
  pWrap.innerHTML='';
  patterns.forEach(p=>{
    const row=document.createElement('div');row.className='set-row';
    row.innerHTML=`<div><div style="font-size:12px;color:#ddd;">${p.name}</div><div style="font-size:9px;color:var(--dim);">${p.bpm} BPM</div></div><button class="load-set-btn" onclick='loadPatternData(${JSON.stringify(p.steps)})'>LOAD</button>`;
    pWrap.appendChild(row);
  });
}
async function loadSetToDecks(setId){
  const{data:tracks}=await db.from('set_tracks').select('*').eq('set_id',setId);
  tracks.forEach(t=>{
    if(t.deck==='A'){state.deckA.title=t.track_title;state.deckA.artist=t.artist||'';state.deckA.key=t.key_signature||'—';document.getElementById('da-title').textContent=t.track_title;document.getElementById('da-artist').textContent=t.artist||'';document.getElementById('da-key').textContent=t.key_signature||'—';}
    else{state.deckB.title=t.track_title;state.deckB.artist=t.artist||'';state.deckB.key=t.key_signature||'—';document.getElementById('db-title').textContent=t.track_title;document.getElementById('db-artist').textContent=t.artist||'';document.getElementById('db-key').textContent=t.key_signature||'—';}
  });
  switchTab('mixer');showToast('Set loaded!');
}
function loadPatternData(steps){state.steps=steps;buildSeq();switchTab('beats');showToast('Pattern loaded!');}
async function refreshLibrary(){
  if(!currentUser)return;
  const{data:tracks}=await db.from('library').select('*').eq('user_id',currentUser.id).order('uploaded_at',{ascending:false});
  const wrap=document.getElementById('library-list');
  if(!tracks||!tracks.length){wrap.innerHTML='<div style="color:var(--dim);font-size:11px;text-align:center;padding:20px;">Library empty. Add tracks via Supabase.</div>';return;}
  wrap.innerHTML='';
  tracks.forEach(t=>{
    const row=document.createElement('div');row.className='lib-track';
    row.innerHTML=`<div><div style="font-size:12px;color:#ddd;">${t.title}</div><div style="font-size:9px;color:var(--dim);">${t.artist||'Unknown'} · ${t.genre||''}</div></div><div style="display:flex;gap:4px;"><button style="padding:6px 10px;background:rgba(0,229,255,0.08);border:1px solid rgba(0,229,255,0.3);border-radius:4px;color:var(--cyan);font-size:8px;cursor:pointer;touch-action:manipulation;" onclick='loadLibTrack("a",${JSON.stringify(t)})'>→A</button><button style="padding:6px 10px;background:rgba(255,16,96,0.08);border:1px solid rgba(255,16,96,0.3);border-radius:4px;color:var(--pink);font-size:8px;cursor:pointer;touch-action:manipulation;" onclick='loadLibTrack("b",${JSON.stringify(t)})'>→B</button></div>`;
    wrap.appendChild(row);
  });
}
function loadLibTrack(deck,t){
  if(deck==='a'){state.deckA.title=t.title;state.deckA.artist=t.artist||'';state.deckA.key=t.key_signature||'—';document.getElementById('da-title').textContent=t.title;document.getElementById('da-artist').textContent=t.artist||'';document.getElementById('da-key').textContent=t.key_signature||'—';}
  else{state.deckB.title=t.title;state.deckB.artist=t.artist||'';state.deckB.key=t.key_signature||'—';document.getElementById('db-title').textContent=t.title;document.getElementById('db-artist').textContent=t.artist||'';document.getElementById('db-key').textContent=t.key_signature||'—';}
  switchTab('mixer');showToast('Loaded to Deck '+deck.toUpperCase());
}
async function savePattern(){
  if(!currentUser){showToast('Not logged in');return;}
  const name='Pattern '+new Date().toLocaleTimeString();
  const{error}=await db.from('patterns').insert({user_id:currentUser.id,name,bpm:state.bpm,steps:state.steps});
  if(error){showToast('Save failed');return;}
  showToast('Pattern saved! 🥁');
}
function buildPlatforms(){
  const wrap=document.getElementById('platforms-list');
  if(!wrap)return;
  wrap.innerHTML='';
  PLATFORMS.forEach(p=>{
    const row=document.createElement('div');row.className='platform-row';
    row.innerHTML=`<div style="display:flex;align-items:center;gap:10px;"><span style="font-size:20px;">${p.icon}</span><div><div style="font-family:'Orbitron',sans-serif;font-size:11px;color:#fff;letter-spacing:2px;">${p.name}</div><div style="font-size:8px;color:var(--dim);">Tap GO LIVE to open</div></div></div><button class="live-btn" onclick="goLive('${p.name}','${p.url}',this)" style="border-color:${p.col}44;color:${p.col}88;">GO LIVE</button>`;
    wrap.appendChild(row);
  });
}
function goLive(name,url,btn){
  btn.classList.toggle('active');
  btn.textContent=btn.classList.contains('active')?'● LIVE':'GO LIVE';
  if(btn.classList.contains('active')) window.open(url,'_blank');
}
function buildDownloadSites(){
  const wrap=document.getElementById('download-sites');
  if(!wrap)return;
  wrap.innerHTML='';
  DOWNLOAD_SITES.forEach(s=>{
    const row=document.createElement('div');
    row.style.cssText='display:flex;justify-content:space-between;align-items:center;padding:10px 0;border-bottom:1px solid #0f0f18;';
    row.innerHTML=`<div><div style="font-size:11px;color:#ddd;">${s.name}</div><div style="font-size:9px;color:var(--dim);">${s.desc}</div></div><button style="padding:6px 12px;background:transparent;border:1px solid var(--cyan);border-radius:4px;color:var(--cyan);font-family:'Share Tech Mono',monospace;font-size:8px;cursor:pointer;touch-action:manipulation;" onclick="window.open('${s.url}','_blank')">OPEN ↗</button>`;
    wrap.appendChild(row);
  });
}
function buildBeatChips(){
  const wrap=document.getElementById('beat-chips');
  if(!wrap||wrap.children.length)return;
  BEAT_CHIPS.forEach(p=>{
    const btn=document.createElement('button');
    btn.style.cssText='display:block;width:100%;margin-bottom:6px;padding:9px 12px;background:var(--bg3);border:1px solid var(--border);border-radius:6px;color:#bbb;font-family:\'Share Tech Mono\',monospace;font-size:10px;cursor:pointer;text-align:left;touch-action:manipulation;';
    btn.textContent=p;
    btn.onclick=()=>{
      try{navigator.clipboard.writeText(p);}catch(e){}
      const orig=btn.textContent;
      btn.textContent='✓ COPIED!';btn.style.borderColor='var(--purple)';btn.style.color='var(--purple)';
      setTimeout(()=>{btn.textContent=orig;btn.style.borderColor='var(--border)';btn.style.color='#bbb';},2000);
    };
    wrap.appendChild(btn);
  });
}
// ─── JOURNAL & COLLAB ────────────────────────────────────────────────────────
async function loadJournal(){
  if(!currentUser)return;
  const{data:entries}=await db.from('journal_entries').select('*').eq('user_id',currentUser.id).order('created_at',{ascending:false}).limit(20);
  const wrap=document.getElementById('journal-entries');
  if(!entries||!entries.length){wrap.innerHTML='<div style="color:var(--dim);font-size:11px;text-align:center;padding:20px;">No entries yet. Write something! ✍️</div>';return;}
  wrap.innerHTML='';
  entries.forEach(e=>{
    const d=new Date(e.created_at).toLocaleDateString();
    const row=document.createElement('div');
    row.style.cssText='padding:12px;background:var(--bg3);border-radius:8px;border:1px solid var(--border);margin-bottom:8px;';
    row.innerHTML=`<div style="font-size:8px;color:var(--purple);letter-spacing:2px;margin-bottom:6px;">${d}</div><div style="font-size:12px;color:#ddd;line-height:1.6;">${e.content}</div>`;
    wrap.appendChild(row);
  });
}

async function saveJournalEntry(){
  if(!currentUser)return;
  const content=document.getElementById('journal-input').value.trim();
  if(!content){showToast('Write something first!');return;}
  const{error}=await db.from('journal_entries').insert({user_id:currentUser.id,content});
  if(error){showToast('Save failed');return;}
  document.getElementById('journal-input').value='';
  showToast('Entry saved! ✍️');
  loadJournal();
}

async function loadCollab(){
  const{data:notes}=await db.from('collab_board').select('*,profiles(username)').order('created_at',{ascending:false}).limit(30);
  const wrap=document.getElementById('collab-notes');
  if(!notes||!notes.length){wrap.innerHTML='<div style="color:var(--dim);font-size:11px;text-align:center;padding:20px;">No collab notes yet!</div>';return;}
  wrap.innerHTML='';
  notes.forEach(n=>{
    const d=new Date(n.created_at).toLocaleTimeString();
    const isMe=n.user_id===currentUser?.id;
    const row=document.createElement('div');
    row.style.cssText=`padding:10px 12px;background:${isMe?'rgba(139,47,255,0.1)':'rgba(0,229,255,0.05)'};border-radius:8px;border:1px solid ${isMe?'rgba(139,47,255,0.3)':'rgba(0,229,255,0.15)'};margin-bottom:8px;`;
    row.innerHTML=`<div style="display:flex;justify-content:space-between;margin-bottom:4px;"><span style="font-size:8px;color:${isMe?'var(--purple)':'var(--cyan)'};">${n.profiles?.username||'Unknown'}</span><span style="font-size:8px;color:var(--dim);">${d}</span></div><div style="font-size:12px;color:#ddd;line-height:1.6;">${n.content}</div>`;
    wrap.appendChild(row);
  });
}

async function postCollabNote(){
  if(!currentUser)return;
  const content=document.getElementById('collab-input').value.trim();
  if(!content){showToast('Write something first!');return;}
  const{error}=await db.from('collab_board').insert({user_id:currentUser.id,content});
  if(error){showToast('Post failed');return;}
  document.getElementById('collab-input').value='';
  loadCollab();
}

async function inviteCollab(){
  const email=document.getElementById('invite-email').value.trim();
  if(!email){showToast('Enter an email');return;}
  showToast('Invite sent to '+email+'! 📧');
  document.getElementById('invite-email').value='';
}

function setupCollabRealtime(){
  db.channel('collab_board').on('postgres_changes',{event:'INSERT',schema:'public',table:'collab_board'},()=>{
    loadCollab();
  }).subscribe();
}
function toggleDeck(deck){const d=deck==='a'?state.deckA:state.deckB;d.playing=!d.playing;const vinyl=document.getElementById('vinyl-'+deck);if(d.playing)vinyl.classList.add('spinning');else vinyl.classList.remove('spinning');}
function toggleLoop(deck){const d=deck==='a'?state.deckA:state.deckB;d.loop=!d.loop;document.getElementById('loop-'+deck).classList.toggle('on',d.loop);}
function tapCue(deck){const btn=document.getElementById('cue-'+deck);btn.classList.add('on');setTimeout(()=>btn.classList.remove('on'),300);}
function syncDeck(deck){const btn=document.getElementById('sync-'+deck);btn.classList.add('on');setTimeout(()=>btn.classList.remove('on'),600);}
function updatePitch(deck,val){document.getElementById('pitch-'+deck+'-val').textContent=(val>0?'+':'')+parseFloat(val).toFixed(1)+'%';}
function updateBpm(val){state.bpm=parseInt(val);document.getElementById('bpm-display').textContent=val;document.getElementById('seq-bpm-val').textContent=val+' BPM';}
const wfData={a:[],b:[]};
function drawWaveform(deck){const c=document.getElementById('wf-'+deck),ctx=c.getContext('2d');c.width=c.offsetWidth*devicePixelRatio;c.height=50*devicePixelRatio;ctx.scale(devicePixelRatio,devicePixelRatio);const W=c.offsetWidth,H=50,prog=deck==='a'?state.deckA.progress:state.deckB.progress,col=deck==='a'?'#00e5ff':'#ff1060';if(!wfData[deck].length)for(let i=0;i<W;i++)wfData[deck].push(Math.random()*0.7+0.15);wfData[deck].forEach((v,i)=>{const h=v*(H/2);ctx.fillStyle=i/W<prog?col:'#1e1e2a';ctx.fillRect(i,H/2-h,1,h*2);});ctx.fillStyle='#fff';ctx.fillRect(Math.floor(prog*W)-1,0,2,H);}
function buildVU(){const wrap=document.getElementById('vu-meters');wrap.innerHTML='';[0,1].forEach(ch=>{const bar=document.createElement('div');bar.className='vu-bar';bar.id='vu-'+ch;for(let i=0;i<14;i++){const seg=document.createElement('div');seg.className='vu-seg';seg.id=`vu-${ch}-${i}`;seg.style.background='#141420';bar.appendChild(seg);}wrap.appendChild(bar);});}
function updateVU(){const playing=state.deckA.playing||state.deckB.playing||state.seqPlaying;if(playing){state.vuL=55+Math.random()*40;state.vuR=50+Math.random()*45;}else{state.vuL=Math.max(0,state.vuL-10);state.vuR=Math.max(0,state.vuR-10);}[0,1].forEach(ch=>{const v=ch===0?state.vuL:state.vuR;for(let i=0;i<14;i++){const seg=document.getElementById(`vu-${ch}-${i}`);if(!seg)return;const t=((13-i)/13)*100,col=i<2?'#ff0040':i<4?'#ffcc00':'#00e5ff';seg.style.background=v>=t?col:'#141420';}});}
const SEQ_LABELS=['KICK','SNARE','HH','808'],SEQ_CLASSES=['on-kick','on-snare','on-hh','on-808'];
function buildSeq(){const grid=document.getElementById('seq-grid');grid.innerHTML='';SEQ_LABELS.forEach((lbl,ri)=>{const row=document.createElement('div');row.className='seq-row-el';row.innerHTML=`<div class="seq-label">${lbl}</div><div class="seq-steps" id="seq-row-${ri}"></div>`;grid.appendChild(row);const stepsEl=document.getElementById('seq-row-'+ri);for(let si=0;si<16;si++){const btn=document.createElement('button');btn.className=`step-btn ${state.steps[ri][si]?SEQ_CLASSES[ri]:'off'}`;btn.id=`step-${ri}-${si}`;btn.onclick=()=>{state.steps[ri][si]=!state.steps[ri][si];btn.className=`step-btn ${state.steps[ri][si]?SEQ_CLASSES[ri]:'off'}`;};stepsEl.appendChild(btn);}});}
function loadPreset(name){state.steps=PRESETS[name].map(r=>[...r]);buildSeq();}
let seqInterval;
function toggleSeq(){state.seqPlaying=!state.seqPlaying;const btn=document.getElementById('seq-play-btn');if(state.seqPlaying){btn.textContent='■ STOP';btn.style.background='rgba(139,47,255,0.25)';let s=0;seqInterval=setInterval(()=>{if(state.currentStep>=0)SEQ_LABELS.forEach((_,ri)=>{const el=document.getElementById(`step-${ri}-${state.currentStep}`);if(el)el.style.outline='none';});state.currentStep=s;SEQ_LABELS.forEach((_,ri)=>{const el=document.getElementById(`step-${ri}-${s}`);if(el)el.style.outline='1px solid #666';});s=(s+1)%16;},(60000/state.bpm)/4);}else{clearInterval(seqInterval);btn.textContent='▶ PLAY';btn.style.background='rgba(139,47,255,0.1)';state.currentStep=-1;}}
function switchTab(tab){document.querySelectorAll('.panel').forEach(p=>p.classList.remove('active'));document.querySelectorAll('.nav-btn').forEach(b=>b.classList.remove('active'));document.getElementById('panel-'+tab).classList.add('active');document.getElementById('nav-'+tab).classList.add('active');if(tab==='journal'){loadJournal();loadCollab();setupCollabRealtime();}}
function showToast(msg){const t=document.getElementById('toast');t.textContent=msg;t.classList.add('show');setTimeout(()=>t.classList.remove('show'),2500);}
function startLoops(){setInterval(()=>{updateVU();if(state.deckA.playing){state.deckA.progress=state.deckA.progress>=0.99?0:state.deckA.progress+0.0025;drawWaveform('a');}if(state.deckB.playing){state.deckB.progress=state.deckB.progress>=0.99?0:state.deckB.progress+0.002;drawWaveform('b');}},120);}
window.addEventListener('resize',()=>{drawWaveform('a');drawWaveform('b');});
</script>

<!-- JOURNAL PANEL -->
<div class="panel" id="panel-journal" style="display:none;">
  <div class="card">
    <div class="section-title" style="color:var(--purple);">✍️ MY JOURNAL</div>
    <textarea id="journal-input" placeholder="Write your thoughts, lyrics, ideas..." style="width:100%;background:var(--bg3);border:1px solid var(--border);border-radius:8px;padding:12px;color:#fff;font-family:'Share Tech Mono',monospace;font-size:12px;outline:none;min-height:100px;resize:none;margin-bottom:8px;display:block;"></textarea>
    <button class="auth-btn" onclick="saveJournalEntry()" style="background:linear-gradient(135deg,var(--purple),var(--cyan));color:#000;">SAVE ENTRY</button>
    <div id="journal-entries" style="margin-top:12px;"></div>
  </div>
  <div class="card">
    <div class="section-title" style="color:var(--cyan);">🎨 COLLAB BOARD</div>
    <div style="margin-bottom:10px;padding:10px;background:var(--bg3);border-radius:8px;border:1px solid var(--border);">
      <div style="font-size:8px;color:var(--dim);letter-spacing:2px;margin-bottom:6px;">📧 INVITE COLLABORATOR</div>
      <div style="display:flex;gap:8px;">
        <input id="invite-email" type="email" placeholder="Friend's email" style="flex:1;background:var(--bg);border:1px solid var(--border);border-radius:6px;padding:10px;color:#fff;font-family:'Share Tech Mono',monospace;font-size:11px;outline:none;"/>
        <button onclick="inviteCollab()" style="padding:10px 14px;background:rgba(0,229,255,0.08);border:1px solid var(--cyan);border-radius:6px;color:var(--cyan);font-size:9px;cursor:pointer;touch-action:manipulation;white-space:nowrap;">INVITE ↗</button>
      </div>
    </div>
    <textarea id="collab-input" placeholder="Drop a note, idea or lyric for your collab..." style="width:100%;background:var(--bg3);border:1px solid var(--border);border-radius:8px;padding:12px;color:#fff;font-family:'Share Tech Mono',monospace;font-size:12px;outline:none;min-height:80px;resize:none;margin-bottom:8px;display:block;"></textarea>
    <button class="auth-btn" onclick="postCollabNote()" style="background:linear-gradient(135deg,var(--cyan),var(--purple));color:#000;">POST TO BOARD</button>
    <div id="collab-notes" style="margin-top:12px;"></div>
  </div>
</div>

</body>
</html>
