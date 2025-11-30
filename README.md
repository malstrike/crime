<!doctype html>
<html lang="ru">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>malstrike — access</title>
  <meta name="description" content="malstrike — персональная хакер-страница / портфолио в стиле терминала" />
  <!-- Google fonts: Share Tech Mono (mono) and Inter (UI) -->
  <link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Inter:wght@300;600&display=swap" rel="stylesheet">
  <style>
    :root{
      --bg-color: #04110b;
      --panel: rgba(0,0,0,0.35);
      --accent: #00ff7f; /* hacker green */
      --muted: #6ef2b0;
      --glass: rgba(255,255,255,0.03);
      --mono: "Share Tech Mono", monospace;
      --ui: "Inter", system-ui, sans-serif;
      --noise-op: 0.06;
    }

    /* Page reset */
    *{box-sizing:border-box;margin:0;padding:0}
    html,body,#app{height:100%}
    body{
      background: radial-gradient(ellipse at 10% 10%, rgba(0,255,120,0.03) 0%, transparent 20%),
                  linear-gradient(180deg, #00110a 0%, #001815 60%, #00060a 100%);
      color: var(--accent);
      font-family: var(--mono);
      -webkit-font-smoothing:antialiased;
      -moz-osx-font-smoothing:grayscale;
      overflow:hidden;
    }

    /* Subtle noise overlay */
    body::after{
      content:"";
      position:fixed;inset:0;
      background-image: url('data:image/svg+xml;utf8,\
        <svg xmlns="http://www.w3.org/2000/svg" width="200" height="200">\
          <filter id="n"><feTurbulence baseFrequency="0.9" stitchTiles="stitch" numOctaves="1" seed="2"/></filter>\
          <rect width="100%" height="100%" filter="url(%23n)" opacity="0.06" fill="black"/>\
        </svg>');
      opacity:var(--noise-op);
      pointer-events:none;
      mix-blend-mode:overlay;
    }

    /* Matrix canvas (behind everything) */
    #matrix {
      position:fixed; inset:0; z-index:0; width:100%; height:100%; pointer-events:none;
    }

    /* Main container */
    .wrap{
      position:relative;
      z-index:2;
      height:100%;
      display:flex;
      align-items:center;
      justify-content:center;
      padding:32px;
    }

    .card{
      width:min(1100px, 95%);
      max-width:1200px;
      min-height:420px;
      background: linear-gradient(180deg, rgba(0,0,0,0.30), rgba(0,0,0,0.40));
      border-radius:10px;
      box-shadow: 0 10px 40px rgba(0,0,0,0.6), inset 0 1px 0 rgba(255,255,255,0.02);
      display:grid;
      grid-template-columns: 1fr 420px;
      gap:20px;
      overflow:hidden;
      border:1px solid rgba(0,255,127,0.06);
      backdrop-filter: blur(6px) saturate(.85);
    }

    /* Left area: big terminal / intro */
    .left {
      padding:30px;
      position:relative;
      display:flex;
      flex-direction:column;
      gap:18px;
      min-height:420px;
    }

    .brand {
      display:flex;
      gap:12px;
      align-items:center;
      user-select:none;
    }

    .logo {
      width:64px; height:64px;
      display:grid;place-items:center;
      border-radius:8px;
      background:linear-gradient(135deg, rgba(0,255,127,0.06), rgba(0,255,127,0.02));
      border:1px solid rgba(0,255,127,0.08);
      box-shadow: 0 4px 18px rgba(0,255,127,0.03), inset 0 -6px 18px rgba(0,0,0,0.35);
      font-weight:700;
      color:var(--accent);
      font-size:20px;
      font-family:var(--mono);
      transform:skewX(-6deg);
    }

    .title {
      font-size:22px;
      letter-spacing:1px;
      color:var(--accent);
      display:flex;
      flex-direction:column;
      line-height:1;
    }

    .title .nick {
      font-size:44px;
      font-family:var(--mono);
      color:var(--accent);
      text-shadow: 0 0 18px rgba(0,255,127,0.12), 0 0 2px rgba(0,255,127,0.18);
      display:inline-block;
      position:relative;
      transform-origin:center;
      margin-top:6px;
    }

    /* small "glitch" effect */
    .title .nick::after, .title .nick::before {
      content:attr(data-text);
      position:absolute;
      left:0; top:0;
      width:100%;
      overflow:hidden;
      clip:rect(0,9999px,0,0);
      opacity:.8;
    }
    .title .nick::before { color: rgba(0,255,120,0.6); transform:translate(2px,-1px); animation:glitch-1 2.5s infinite linear; }
    .title .nick::after { color: rgba(0,150,80,0.4); transform:translate(-2px,1px); animation:glitch-2 3s infinite linear; }
    @keyframes glitch-1 {
      0%{clip:rect(0,9999px,0,0)} 10%{clip:rect(10px,9999px,50px,0)} 20%{clip:rect(0,9999px,0,0)}
      30%{clip:rect(30px,9999px,80px,0)} 40%{clip:rect(0,9999px,0,0)} 100%{clip:rect(0,9999px,0,0)}
    }
    @keyframes glitch-2 {
      0%{clip:rect(0,9999px,0,0)} 15%{clip:rect(8px,9999px,28px,0)} 35%{clip:rect(0,9999px,0,0)}
      55%{clip:rect(50px,9999px,120px,0)} 75%{clip:rect(0,9999px,0,0)} 100%{clip:rect(0,9999px,0,0)}
    }

    .subtitle {
      font-size:13px;
      color:var(--muted);
      opacity:0.9;
      max-width:70%;
    }

    .terminal {
      margin-top:8px;
      background: rgba(0,0,0,0.35);
      border-radius:8px;
      padding:18px;
      min-height:140px;
      border:1px solid rgba(0,255,127,0.04);
      box-shadow: 0 8px 30px rgba(0,0,0,0.6), inset 0 1px 0 rgba(255,255,255,0.02);
      font-size:15px;
      line-height:1.45;
      color:var(--accent);
      position:relative;
      overflow:hidden;
    }

    .terminal .cursor {
      display:inline-block;
      width:8px;height:18px;background:var(--accent);
      margin-left:6px; vertical-align:middle;
      animation:blink 1s steps(2) infinite;
    }
    @keyframes blink { 0%,50%{opacity:1} 51%,100%{opacity:0} }

    .links {
      margin-top:auto;
      display:flex;
      gap:12px;
      flex-wrap:wrap;
    }
    .btn {
      font-family:var(--ui);
      background:transparent;
      border:1px solid rgba(0,255,127,0.08);
      padding:10px 14px;
      border-radius:8px;
      color:var(--muted);
      font-weight:600;
      cursor:pointer;
      transition:all .18s ease;
      letter-spacing:0.6px;
    }
    .btn:hover{ transform:translateY(-3px); box-shadow: 0 6px 18px rgba(0,255,127,0.03); color:var(--accent) }

    /* Right column: widgets */
    .right {
      padding:22px;
      background: linear-gradient(180deg, rgba(0,0,0,0.18) 0%, rgba(0,0,0,0.06) 100%);
      border-left:1px dashed rgba(0,255,127,0.03);
      display:flex;
      flex-direction:column;
      gap:14px;
      min-width:260px;
      max-width:420px;
    }

    .panel {
      background:var(--panel);
      border-radius:8px;
      padding:12px;
      border:1px solid rgba(255,255,255,0.02);
      box-shadow: inset 0 1px 0 rgba(255,255,255,0.02);
      font-size:13px;
      color:var(--muted);
    }

    .panel h3 {
      color:var(--accent);
      font-size:13px;
      margin-bottom:8px;
      font-family:var(--mono);
    }

    .stat {
      display:flex;
      justify-content:space-between;
      gap:12px;
      padding:8px 6px;
      border-radius:6px;
      align-items:center;
      font-weight:600;
      color:var(--muted);
    }

    .footer {
      margin-top:auto;
      padding-top:8px;
      border-top:1px dashed rgba(0,255,127,0.03);
      display:flex;
      justify-content:space-between;
      align-items:center;
      font-size:12px;
      color:rgba(180,255,200,0.6);
    }

    /* small responsive */
    @media (max-width:920px){
      .card{ grid-template-columns: 1fr; }
      .right{ order:2; }
      .left{ order:1; }
      .title .nick{ font-size:36px }
    }

    /* small terminal prompt area */
    .cmdline { display:flex; gap:10px; align-items:center; margin-top:12px; }
    .cmd-input {
      flex:1;
      background:transparent;
      border:1px dashed rgba(0,255,127,0.04);
      padding:10px 12px;
      border-radius:6px;
      color:var(--accent);
      font-family:var(--mono);
      font-size:13px;
      outline:none;
    }

    /* tiny animations */
    .pulse {
      animation:pulse 3s infinite;
    }
    @keyframes pulse { 0%{transform:scale(1)} 50%{transform:scale(1.01)} 100%{transform:scale(1)} }
  </style>
</head>
<body>
  <canvas id="matrix"></canvas>

  <div id="app" class="wrap">
    <div class="card" role="main" aria-label="malstrike hacker page">
      <section class="left">
        <div class="brand">
          <div class="logo">M</div>
          <div class="title">
            <div style="font-size:12px;color:var(--muted)">> user</div>
            <div class="nick" id="nick" data-text="malstrike">malstrike</div>
          </div>
        </div>

        <div class="subtitle">
          Добро пожаловать в личный узел malstrike. Здесь живут проекты, заметки и небольшие демонстрации.
        </div>

        <div class="terminal" aria-live="polite" id="terminal">
          <div id="lines"></div>
          <div style="height:4px"></div>
          <div style="display:flex;align-items:center;gap:8px">
            <span style="color:var(--muted);font-family:var(--mono);font-size:13px">≫</span>
            <div class="cursor" id="terminalCursor" aria-hidden="true"></div>
          </div>
        </div>

        <div class="cmdline">
          <input class="cmd-input" id="cmd" placeholder="type 'help' — then press Enter" aria-label="command input" />
          <button class="btn" id="copyBtn" title="Copy nickname">Copy nick</button>
        </div>

        <div class="links">
          <button class="btn" onclick="openLink('#projects')">Projects</button>
          <button class="btn" onclick="openLink('#about')">About</button>
          <button class="btn" onclick="openLink('https://example.com')">Contact</button>
        </div>
      </section>

      <aside class="right" aria-label="sidebar">
        <div class="panel">
          <h3>system</h3>
          <div class="stat"><span>status</span><span style="color:var(--accent)">ONLINE</span></div>
          <div class="stat"><span>uptime</span><span id="uptime">calculating...</span></div>
          <div class="stat"><span>session</span><span id="sess">0x0</span></div>
        </div>

        <div class="panel">
          <h3>quick</h3>
          <div style="display:flex;flex-direction:column;gap:8px">
            <button class="btn" onclick="runDemo()">Run demo</button>
            <button class="btn" onclick="toggleMatrix()">Toggle rain</button>
            <button class="btn" onclick="document.getElementById('cmd').focus()">Focus CMD</button>
          </div>
        </div>

        <div class="panel">
          <h3>info</h3>
          <div style="font-size:12px;color:var(--muted);line-height:1.4">
            Эта страница — статический шаблон. Можешь менять цвета в корне CSS, подключать свои скрипты и размещать здесь проекты.
          </div>
        </div>

        <div class="footer">
          <div>© malstrike</div>
          <div id="timeBox"></div>
        </div>
      </aside>
    </div>
  </div>

  <script>
    /***** Utilities *****/
    const $ = sel => document.querySelector(sel);
    const el = id => document.getElementById(id);

    /* Terminal lines */
    const linesEl = el('lines');
    const terminalCursor = el('terminalCursor');

    function pushLine(text, delay = 0, cls = '') {
      const p = document.createElement('div');
      p.textContent = text;
      if (cls) p.className = cls;
      p.style.opacity = '0';
      linesEl.appendChild(p);
      setTimeout(()=>{ p.style.transition = 'opacity .24s'; p.style.opacity='1'; linesEl.scrollTop = linesEl.scrollHeight; }, delay);
    }

    /* Typewriter effect */
    async function typeToTerminal(text, speed=18){
      const node = document.createElement('div');
      linesEl.appendChild(node);
      let s = '';
      for (let i=0;i<text.length;i++){
        s += text[i];
        node.textContent = s;
        linesEl.scrollTop = linesEl.scrollHeight;
        await new Promise(r => setTimeout(r, speed + (Math.random()*20)));
      }
      return node;
    }

    /* Initial demo sequence */
    async function introSequence(){
      pushLine('> booting malstrike node...', 120);
      await sleep(700);
      await typeToTerminal('loading modules: [core.crypto, ui.terminal, net.scan]', 12);
      pushLine('> initializing interface', 90);
      await sleep(400);
      await typeToTerminal('> welcome, malstrike — connection established', 10);
      pushLine('> type "help" for commands', 80);
    }

    function sleep(ms){ return new Promise(r => setTimeout(r, ms)); }

    /* Commands */
    document.getElementById('cmd').addEventListener('keydown', async (e)=>{
      if(e.key === 'Enter'){
        const v = e.target.value.trim();
        if(!v) return;
        pushLine('> ' + v, 10);
        e.target.value = '';
        await handleCommand(v.toLowerCase());
      }
    });

    async function handleCommand(cmd){
      if(cmd === 'help'){
        await typeToTerminal('help: help, whoami, nick, uptime, clear, demo', 12);
      } else if(cmd === 'whoami'){
        await typeToTerminal('user: malstrike (owner)', 12);
      } else if(cmd === 'nick'){
        await typeToTerminal('nick: malstrike', 12);
      } else if(cmd === 'uptime'){
        await typeToTerminal('uptime: ' + document.getElementById('uptime').textContent, 12);
      } else if(cmd === 'clear'){
        linesEl.innerHTML = '';
      } else if(cmd === 'demo'){
        runDemo();
      } else {
        await typeToTerminal('unknown command: ' + cmd, 10);
      }
    }

    /* Copy nick button */
    document.getElementById('copyBtn').addEventListener('click', async ()=>{
      try{
        await navigator.clipboard.writeText('malstrike');
        pushLine('> nick copied to clipboard ✔', 40);
      }catch(e){
        pushLine('> clipboard failed — ' + e.message, 40);
      }
    });

    /* Small helpers */
    function openLink(u){ if(u.startsWith('#')) location.hash = u; else window.open(u, '_blank'); }

    /* Uptime & session id */
    const started = Date.now();
    function updateUptime(){
      const s = Math.floor((Date.now()-started)/1000);
      const h = Math.floor(s/3600); const m = Math.floor((s%3600)/60); const sec = s%60;
      document.getElementById('uptime').textContent = `${h}h ${m}m ${sec}s`;
      document.getElementById('sess').textContent = '0x' + Math.floor(Math.random()*0xFFFFFF).toString(16);
      document.getElementById('timeBox').textContent = new Date().toLocaleString();
    }
    setInterval(updateUptime, 1000); updateUptime();

    /* Demo: visual pulses & terminal messages */
    async function runDemo(){
      pushLine('> demo sequence start', 20);
      for(let i=0;i<3;i++){
        document.querySelector('.card').classList.add('pulse');
        await sleep(280);
        document.querySelector('.card').classList.remove('pulse');
        await sleep(120);
      }
      await typeToTerminal('> scanning ports: 22 80 443 ... 1337', 10);
      pushLine('> demo complete', 50);
    }

    /* Matrix rain effect */
    const canvas = document.getElementById('matrix');
    let ctx = canvas.getContext('2d');
    let width, height;
    let columns = [];
    let showMatrix = true;

    function resize() {
      width = canvas.width = innerWidth;
      height = canvas.height = innerHeight;
      const fontSize = 14;
      ctx.font = fontSize + "px " + getComputedStyle(document.documentElement).getPropertyValue('--mono');
      const cols = Math.floor(width / fontSize) + 1;
      columns = new Array(cols).fill(0).map(()=>Math.floor(Math.random()*height));
    }
    addEventListener('resize', resize);
    resize();

    const chars = "ﾊﾐﾐﾑﾒﾕﾖﾗﾘﾛﾜｦﾝabcdefghijklmnopqrstuvwxyz0123456789@#$%&*-+=<>";

    function draw(){
      if(!showMatrix) { ctx.clearRect(0,0,width,height); requestAnimationFrame(draw); return; }
      ctx.fillStyle = "rgba(0,0,0,0.08)";
      ctx.fillRect(0,0,width,height);
      const fontSize = 14;
      ctx.fillStyle = "rgba(0,255,127,0.15)";
      for(let i=0;i<columns.length;i++){
        const x = i*fontSize;
        const y = columns[i]*fontSize;
        const text = chars.charAt(Math.floor(Math.random()*chars.length));
        ctx.fillStyle = (Math.random() > 0.97) ? "rgba(255,255,255,0.8)" : "rgba(0,255,127,0.18)";
        ctx.fillText(text, x, y);
        if(columns[i]*fontSize > height && Math.random() > 0.975) columns[i]=0;
        columns[i]++;
      }
      requestAnimationFrame(draw);
    }
    draw();

    function toggleMatrix(){
      showMatrix = !showMatrix;
    }

    /* small accessibility: focus outlines */
    document.addEventListener('keydown', (e)=>{
      if(e.key === 'Tab') document.documentElement.style.outline = 'none';
    });

    /* start */
    introSequence();

    /* small easter egg: console.log styling */
    console.log('%c malstrike active ', 'background: #00110a; color: #00ff7f; padding:6px 10px; border-radius:6px; font-weight:700;');

    // prevent accidental text selection on drag
    document.addEventListener('selectstart', e => {
      if (e.target.closest('.card')) {
        // allow selection only in input areas
        if (!e.target.closest('input')) e.preventDefault();
      }
    });

  </script>
</body>
</html>
