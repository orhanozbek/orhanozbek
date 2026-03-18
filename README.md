<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Orhan Arda Özbek</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Exo+2:wght@300;600;800&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
  :root {
    --blue: #34abeb; --blue-dim: #1a6fa0; --blue-glow: rgba(52,171,235,0.15);
    --green: #daf7dc; --spotify: #1DB954;
    --bg: #0D1117; --bg2: #111820; --bg3: #161d27;
    --border: rgba(52,171,235,0.25); --border-hot: rgba(52,171,235,0.6);
    --text: #c9d1d9; --text-dim: #6e7681;
    --mono: 'Share Tech Mono', monospace; --sans: 'Exo 2', sans-serif;
  }
  body { background: var(--bg); color: var(--text); font-family: var(--mono); min-height: 100vh; overflow-x: hidden; perspective: 1200px; }
  #matrix-canvas { position: fixed; top: 0; left: 0; width: 100%; height: 100%; opacity: 0.04; pointer-events: none; z-index: 0; }
  .scan-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 2; background: repeating-linear-gradient(0deg, transparent, transparent 2px, rgba(0,0,0,0.03) 2px, rgba(0,0,0,0.03) 4px); }
  .wrapper { position: relative; z-index: 1; max-width: 860px; margin: 0 auto; padding: 48px 24px 80px; transform-style: preserve-3d; transition: transform 0.1s ease-out; }

  /* HEADER */
  .header { text-align: center; margin-bottom: 40px; animation: fadeDown 0.8s ease both; }
  .header-tag { font-size: 11px; color: var(--blue); letter-spacing: 4px; margin-bottom: 12px; }
  .header h1 { font-family: var(--sans); font-size: clamp(36px, 7vw, 72px); font-weight: 800; line-height: 1; background: linear-gradient(135deg, #fff 0%, var(--blue) 60%, var(--blue-dim) 100%); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; letter-spacing: -1px; margin-bottom: 16px; }
  .header-sub { font-size: 13px; color: var(--text-dim); letter-spacing: 2px; }
  .header-line { width: 120px; height: 1px; background: linear-gradient(90deg, transparent, var(--blue), transparent); margin: 20px auto 0; }

  /* BADGES */
  .badges { display: flex; flex-wrap: wrap; gap: 8px; justify-content: center; margin-bottom: 32px; animation: fadeUp 0.9s 0.2s ease both; }
  .badge { font-size: 11px; letter-spacing: 1px; padding: 5px 14px; border: 1px solid var(--border); border-radius: 2px; color: var(--blue); background: var(--blue-glow); position: relative; overflow: hidden; transition: border-color 0.3s, background 0.3s; cursor: default; }
  .badge:hover { border-color: var(--border-hot); background: rgba(52,171,235,0.25); }
  .badge::before { content: ''; position: absolute; top: 0; left: -100%; width: 60%; height: 100%; background: linear-gradient(90deg, transparent, rgba(52,171,235,0.3), transparent); transition: left 0.5s; }
  .badge:hover::before { left: 140%; }

  /* SOCIAL BAR */
  .social-bar { display: flex; gap: 10px; justify-content: center; flex-wrap: wrap; margin-bottom: 40px; animation: fadeUp 0.9s 0.28s ease both; }
  .social-btn { display: flex; align-items: center; gap: 8px; padding: 9px 20px; font-size: 12px; font-family: var(--mono); border: 1px solid var(--border); border-radius: 3px; color: var(--text); text-decoration: none; background: var(--bg2); transition: border-color 0.3s, color 0.3s, background 0.3s, transform 0.2s; position: relative; overflow: hidden; }
  .social-btn:hover { transform: translateY(-3px); }
  .social-btn svg { width: 14px; height: 14px; fill: currentColor; flex-shrink: 0; }
  .social-btn.gh:hover { color: #fff; border-color: rgba(255,255,255,0.4); background: rgba(255,255,255,0.06); }
  .social-btn.li:hover { color: #0a66c2; border-color: rgba(10,102,194,0.5); background: rgba(10,102,194,0.1); }
  .social-btn.tw:hover { color: #1d9bf0; border-color: rgba(29,155,240,0.5); background: rgba(29,155,240,0.1); }

  /* GRID */
  .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; margin-bottom: 16px; }
  @media (max-width: 600px) { .grid { grid-template-columns: 1fr; } }
  .full { grid-column: 1 / -1; }

  /* CARD */
  .card { background: var(--bg2); border: 1px solid var(--border); border-radius: 4px; overflow: hidden; transition: border-color 0.3s, transform 0.3s, box-shadow 0.3s; animation: fadeUp 0.8s ease both; }
  .card:hover { border-color: var(--border-hot); transform: translateY(-4px) rotateX(1.5deg) rotateY(-1.5deg); box-shadow: 0 16px 48px rgba(52,171,235,0.13); }
  .card-titlebar { display: flex; align-items: center; gap: 6px; padding: 10px 14px; background: var(--bg3); border-bottom: 1px solid var(--border); }
  .dot { width: 10px; height: 10px; border-radius: 50%; }
  .dot-r { background: #ff5f57; } .dot-y { background: #ffbd2e; } .dot-g { background: #28ca41; }
  .card-title { font-size: 11px; color: var(--text-dim); margin-left: 6px; letter-spacing: 1px; }
  .card-body { padding: 18px 20px; }
  .terminal-line { display: flex; gap: 8px; margin-bottom: 8px; font-size: 12px; align-items: baseline; }
  .terminal-line:last-child { margin-bottom: 0; }
  .prompt { color: var(--blue); flex-shrink: 0; }
  .key { color: var(--text-dim); min-width: 90px; flex-shrink: 0; }
  .val { color: var(--green); }
  .val-blue { color: var(--blue); }

  /* STATS */
  .stats-row { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; margin-bottom: 16px; animation: fadeUp 0.8s 0.1s ease both; }
  @media (max-width: 600px) { .stats-row { grid-template-columns: 1fr; } }
  .stat-frame { background: var(--bg2); border: 1px solid var(--border); border-radius: 4px; overflow: hidden; transition: border-color 0.3s, transform 0.3s, box-shadow 0.3s; }
  .stat-frame:hover { border-color: var(--border-hot); transform: translateY(-4px) rotateX(1.5deg); box-shadow: 0 16px 48px rgba(52,171,235,0.13); }
  .stat-frame img { width: 100%; display: block; }

  /* STREAK */
  .streak-row { margin-bottom: 16px; animation: fadeUp 0.8s 0.15s ease both; }
  .streak-frame { background: var(--bg2); border: 1px solid var(--border); border-radius: 4px; overflow: hidden; transition: border-color 0.3s, transform 0.3s, box-shadow 0.3s; }
  .streak-frame:hover { border-color: var(--border-hot); transform: translateY(-4px); box-shadow: 0 16px 48px rgba(52,171,235,0.13); }
  .streak-frame img { width: 100%; display: block; }

  /* SPOTIFY */
  .spotify-card { background: var(--bg2); border: 1px solid var(--border); border-radius: 4px; overflow: hidden; transition: border-color 0.3s, transform 0.3s, box-shadow 0.3s; animation: fadeUp 0.8s 0.36s ease both; }
  .spotify-card:hover { border-color: rgba(29,185,84,0.5); transform: translateY(-4px); box-shadow: 0 16px 48px rgba(29,185,84,0.1); }
  .spotify-titlebar { display: flex; align-items: center; gap: 8px; padding: 10px 14px; background: var(--bg3); border-bottom: 1px solid rgba(29,185,84,0.2); }
  .spotify-dot { width: 10px; height: 10px; border-radius: 50%; background: var(--spotify); animation: pulse-green 2s ease-in-out infinite; }
  @keyframes pulse-green { 0%,100%{box-shadow:0 0 0 0 rgba(29,185,84,0.5);} 50%{box-shadow:0 0 0 6px rgba(29,185,84,0);} }
  .spotify-body { padding: 16px 20px; display: flex; align-items: center; gap: 16px; }
  .album-art { width: 58px; height: 58px; border-radius: 4px; background: var(--bg3); border: 1px solid rgba(29,185,84,0.3); display: flex; align-items: center; justify-content: center; flex-shrink: 0; overflow: hidden; }
  .album-inner { width: 100%; height: 100%; background: linear-gradient(135deg, #1a1a2e, #0f3460); display: flex; align-items: center; justify-content: center; }
  .vinyl { width: 36px; height: 36px; border-radius: 50%; border: 2px solid rgba(29,185,84,0.5); display: flex; align-items: center; justify-content: center; animation: spin 5s linear infinite; }
  .vinyl::before { content: ''; width: 8px; height: 8px; border-radius: 50%; background: var(--spotify); }
  @keyframes spin { from{transform:rotate(0deg)} to{transform:rotate(360deg)} }
  .track-info { flex: 1; min-width: 0; }
  .track-name { font-size: 13px; color: var(--text); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
  .track-artist { font-size: 11px; color: var(--text-dim); margin-top: 4px; }
  .track-status { font-size: 10px; color: var(--spotify); margin-top: 8px; letter-spacing: 1px; display: flex; align-items: center; gap: 6px; }
  .eq-bars { display: flex; gap: 2px; align-items: flex-end; height: 12px; }
  .eq-bar { width: 3px; background: var(--spotify); border-radius: 1px; animation: eq 0.8s ease-in-out infinite alternate; }
  .eq-bar:nth-child(1){height:4px;animation-delay:0s} .eq-bar:nth-child(2){height:10px;animation-delay:0.2s} .eq-bar:nth-child(3){height:6px;animation-delay:0.4s} .eq-bar:nth-child(4){height:12px;animation-delay:0.1s} .eq-bar:nth-child(5){height:5px;animation-delay:0.3s}
  @keyframes eq { from{transform:scaleY(0.3)} to{transform:scaleY(1)} }
  .progress-bar { width: 100%; height: 2px; background: var(--bg3); border-radius: 1px; margin-top: 10px; overflow: hidden; }
  .progress-fill { height: 100%; width: 40%; background: var(--spotify); border-radius: 1px; animation: progress 30s linear infinite; }
  @keyframes progress { from{width:20%} to{width:92%} }

  /* ACTIVITY */
  .activity-card { background: var(--bg2); border: 1px solid var(--border); border-radius: 4px; overflow: hidden; margin-bottom: 16px; animation: fadeUp 0.8s 0.3s ease both; transition: border-color 0.3s, transform 0.3s, box-shadow 0.3s; }
  .activity-card:hover { border-color: var(--border-hot); transform: translateY(-3px); box-shadow: 0 12px 40px rgba(52,171,235,0.1); }
  .activity-card img { width: 100%; display: block; }

  /* QUOTE */
  .quote { text-align: right; font-size: 12px; color: var(--text-dim); font-style: italic; padding: 16px 4px; border-top: 1px solid var(--border); animation: fadeUp 0.8s 0.5s ease both; }
  .quote span { color: var(--blue); }

  /* BLINK */
  .blink { display: inline-block; width: 7px; height: 12px; background: var(--blue); margin-left: 3px; animation: blink 1s step-end infinite; vertical-align: middle; }

  @keyframes fadeDown { from{opacity:0;transform:translateY(-20px)} to{opacity:1;transform:translateY(0)} }
  @keyframes fadeUp   { from{opacity:0;transform:translateY(16px)}  to{opacity:1;transform:translateY(0)} }
  @keyframes blink    { 0%,100%{opacity:1} 50%{opacity:0} }
  .grid .card:nth-child(1){animation-delay:0.0s} .grid .card:nth-child(2){animation-delay:0.12s} .grid .card:nth-child(3){animation-delay:0.24s}
</style>
</head>
<body>
<canvas id="matrix-canvas"></canvas>
<div class="scan-overlay"></div>
<div class="wrapper" id="pw">

  <div class="header">
    <div class="header-tag">// portfolio.init()</div>
    <h1>ORHAN ARDA ÖZBEK</h1>
    <div class="header-sub">BACKEND ENGINEER &nbsp;·&nbsp; C# / .NET / MSSQL &nbsp;·&nbsp; CLOUD</div>
    <div class="header-line"></div>
  </div>

  <div class="badges">
    <div class="badge">FOCUS: BACKEND DEV</div>
    <div class="badge">STACK: C# · .NET · SQL</div>
    <div class="badge">OPEN TO WORK</div>
    <div class="badge">EXPERTISE: .NET CORE</div>
  </div>

  <div class="social-bar">
    <a href="https://github.com/orhanozbek" class="social-btn gh" target="_blank">
      <svg viewBox="0 0 16 16"><path d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27.68 0 1.36.09 2 .27 1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.013 8.013 0 0016 8c0-4.42-3.58-8-8-8z"/></svg>
      GitHub
    </a>
    <a href="https://linkedin.com/in/orhanozbek" class="social-btn li" target="_blank">
      <svg viewBox="0 0 24 24"><path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433a2.062 2.062 0 01-2.063-2.065 2.064 2.064 0 112.063 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/></svg>
      LinkedIn
    </a>
    <a href="https://twitter.com/orhanozbek" class="social-btn tw" target="_blank">
      <svg viewBox="0 0 24 24"><path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-4.714-6.231-5.401 6.231H2.744l7.737-8.843L1.254 2.25H8.08l4.253 5.622 5.91-5.622zm-1.161 17.52h1.833L7.084 4.126H5.117z"/></svg>
      Twitter / X
    </a>
  </div>

  <div class="stats-row">
    <div class="stat-frame">
      <img src="https://github-readme-stats.vercel.app/api?username=orhanozbek&show_icons=true&theme=transparent&title_color=34abeb&icon_color=34abeb&text_color=daf7dc&hide_border=true&bg_color=111820" alt="GitHub Stats" />
    </div>
    <div class="stat-frame">
      <img src="https://github-readme-stats.vercel.app/api/top-langs/?username=orhanozbek&layout=compact&theme=transparent&title_color=34abeb&text_color=daf7dc&hide_border=true&bg_color=111820" alt="Top Languages" />
    </div>
  </div>

  <div class="streak-row">
    <div class="streak-frame">
      <img src="https://streak-stats.demolab.com/?user=orhanozbek&theme=dark&hide_border=true&background=111820&ring=34abeb&fire=34abeb&currStreakLabel=34abeb&sideLabels=daf7dc&dates=6e7681&stroke=1a2535" alt="GitHub Streak" />
    </div>
  </div>

  <div class="grid">
    <div class="card" style="animation-delay:0s">
      <div class="card-titlebar">
        <div class="dot dot-r"></div><div class="dot dot-y"></div><div class="dot dot-g"></div>
        <span class="card-title">system.status</span>
      </div>
      <div class="card-body">
        <div class="terminal-line"><span class="prompt">$</span><span class="key">[EXPERTISE]</span><span class="val">C#, .NET Core, MSSQL</span></div>
        <div class="terminal-line"><span class="prompt">$</span><span class="key">[FOCUS]</span><span class="val">Backend Engineering</span></div>
        <div class="terminal-line"><span class="prompt">$</span><span class="key">[STATUS]</span><span class="val">Open to opportunities</span></div>
        <div class="terminal-line" style="margin-top:12px"><span class="prompt">></span><span class="val-blue">ready_for_new_challenges<span class="blink"></span></span></div>
      </div>
    </div>

    <div class="card" style="animation-delay:0.12s">
      <div class="card-titlebar">
        <div class="dot dot-r"></div><div class="dot dot-y"></div><div class="dot dot-g"></div>
        <span class="card-title">contact.info</span>
      </div>
      <div class="card-body">
        <div class="terminal-line"><span class="prompt">$</span><span class="key">[GITHUB]</span><a href="https://github.com/orhanozbek" style="color:var(--green);text-decoration:none;font-size:12px;">github.com/orhanozbek</a></div>
        <div class="terminal-line"><span class="prompt">$</span><span class="key">[LINKEDIN]</span><a href="https://linkedin.com/in/orhanozbek" style="color:var(--green);text-decoration:none;font-size:12px;">in/orhanozbek</a></div>
        <div class="terminal-line"><span class="prompt">$</span><span class="key">[TWITTER]</span><a href="https://twitter.com/orhanozbek" style="color:var(--green);text-decoration:none;font-size:12px;">@orhanozbek</a></div>
        <div class="terminal-line" style="margin-top:12px"><span class="prompt">></span><span class="val-blue">ping_me_anytime<span class="blink"></span></span></div>
      </div>
    </div>

    <div class="card full" style="animation-delay:0.24s">
      <div class="card-titlebar">
        <div class="dot dot-r"></div><div class="dot dot-y"></div><div class="dot dot-g"></div>
        <span class="card-title">tech.stack — ls /skills/</span>
      </div>
      <div class="card-body" style="padding:20px;">
        <img src="https://skillicons.dev/icons?i=cs,dotnet,aspnet,sql,mysql,js,aws,docker,git,github,postman,linux&theme=dark" alt="Tech Stack" style="max-width:100%;display:block;" />
      </div>
    </div>

    <div class="spotify-card full">
      <div class="spotify-titlebar">
        <div class="spotify-dot"></div>
        <span class="card-title" style="color:var(--spotify);margin-left:6px;">spotify.now_playing</span>
      </div>
      <div class="spotify-body">
        <div class="album-art"><div class="album-inner"><div class="vinyl"></div></div></div>
        <div class="track-info">
          <div class="track-name">Lofi Hip Hop — Coding Session</div>
          <div class="track-artist">ChilledCow · lofi hip hop radio</div>
          <div class="track-status">
            <div class="eq-bars"><div class="eq-bar"></div><div class="eq-bar"></div><div class="eq-bar"></div><div class="eq-bar"></div><div class="eq-bar"></div></div>
            NOW PLAYING
          </div>
          <div class="progress-bar"><div class="progress-fill"></div></div>
        </div>
      </div>
    </div>
  </div>

  <div class="activity-card">
    <img src="https://github-readme-activity-graph.vercel.app/graph?username=orhanozbek&theme=react-dark&hide_border=true&area=true&color=34abeb&line=34abeb&point=ffffff&area_color=1a6fa0" alt="Activity Graph" />
  </div>

  <div class="quote"><span>"</span>Clean code always looks like it was written by someone who cares.<span>"</span></div>
</div>

<script>
const canvas = document.getElementById('matrix-canvas');
const ctx = canvas.getContext('2d');
let cols, drops;
function resize() {
  canvas.width = window.innerWidth; canvas.height = window.innerHeight;
  cols = Math.floor(canvas.width / 18); drops = Array(cols).fill(1);
}
resize(); window.addEventListener('resize', resize);
const chars = '01アイウエオカキクケコサシスセソ<>{}[]();./\\|#@';
function draw() {
  ctx.fillStyle = 'rgba(13,17,23,0.07)'; ctx.fillRect(0,0,canvas.width,canvas.height);
  ctx.fillStyle = '#34abeb'; ctx.font = '13px Share Tech Mono, monospace';
  drops.forEach((y,i) => {
    ctx.fillText(chars[Math.floor(Math.random()*chars.length)], i*18, y*18);
    if (y*18 > canvas.height && Math.random() > 0.975) drops[i] = 0;
    drops[i]++;
  });
}
setInterval(draw, 50);

/* 3D PARALLAX */
const pw = document.getElementById('pw');
let ticking = false;
document.addEventListener('mousemove', e => {
  if (ticking) return; ticking = true;
  requestAnimationFrame(() => {
    const dx = (e.clientX - window.innerWidth/2) / (window.innerWidth/2);
    const dy = (e.clientY - window.innerHeight/2) / (window.innerHeight/2);
    pw.style.transform = `rotateX(${dy * -4}deg) rotateY(${dx * 4}deg)`;
    ticking = false;
  });
});
document.addEventListener('mouseleave', () => {
  pw.style.transition = 'transform 0.8s ease';
  pw.style.transform = 'rotateX(0deg) rotateY(0deg)';
  setTimeout(() => { pw.style.transition = 'transform 0.1s ease-out'; }, 820);
});
</script>
</body>
</html>
