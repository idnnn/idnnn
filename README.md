<!DOCTYPE html>
<html lang="id" data-theme="dawn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Rayyan | Rosé Pine Portfolio</title>
    <link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;700&family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        /* ══ Rosé Pine Dawn ══ */
        :root, [data-theme="dawn"] {
            --base: #faf4ed; --surface: #fffaf3; --overlay: #f2e9e1;
            --muted: #9893a5; --subtle: #797593; --text: #575279;
            --love: #b4637a; --gold: #ea9d34; --rose: #d7827e;
            --pine: #286983; --foam: #56949f; --iris: #907aa9;
            --highlight-low: #f4ede8; --highlight-med: #dfdad9; --highlight-high: #cecacd;
        }
        /* ══ Rosé Pine Moon ══ */
        [data-theme="moon"] {
            --base: #232136; --surface: #2a273f; --overlay: #393552;
            --muted: #6e6a86; --subtle: #908caa; --text: #e0def4;
            --love: #eb6f92; --gold: #f6c177; --rose: #ea9a97;
            --pine: #3e8fb0; --foam: #9ccfd8; --iris: #c4a7e7;
            --highlight-low: #2a283e; --highlight-med: #44415a; --highlight-high: #56526e;
        }

        @keyframes slideIn   { from{opacity:0;transform:translateY(18px)} to{opacity:1;transform:translateY(0)} }
        @keyframes floating  { 0%,100%{transform:translateY(0) rotate(0deg)} 50%{transform:translateY(-8px) rotate(1.5deg)} }
        @keyframes blink     { 0%,100%{opacity:1} 50%{opacity:0} }
        @keyframes ticker-scroll { 0%{transform:translateX(0)} 100%{transform:translateX(-50%)} }
        @keyframes floatY    { 0%,100%{transform:translateY(0px)} 50%{transform:translateY(-8px)} }
        @keyframes spinSlow  { from{transform:rotate(0deg)} to{transform:rotate(360deg)} }
        @keyframes spinRev   { from{transform:rotate(0deg)} to{transform:rotate(-360deg)} }
        @keyframes pulseOpacity { 0%,100%{opacity:.18} 50%{opacity:.38} }
        @keyframes typeIn    {
            0%   { opacity:0; transform:translateY(4px) }
            10%  { opacity:1; transform:translateY(0) }
            85%  { opacity:1 }
            100% { opacity:0 }
        }

        * { margin:0; padding:0; box-sizing:border-box; }

        body {
            background-color: var(--base);
            background-image: radial-gradient(var(--highlight-med) 1px, transparent 1px);
            background-size: 25px 25px;
            color: var(--text);
            font-family: 'Plus Jakarta Sans', sans-serif;
            display: flex;
            justify-content: center;
            min-height: 100vh;
            padding: 40px 20px;
            transition: background-color .4s, color .4s;
            overflow-x: hidden;
        }

        /* ══════════════════════════════════════════
           SIDE PANELS
        ══════════════════════════════════════════ */
        .side-panel {
            position: fixed;
            top: 0; bottom: 0;
            width: calc((100vw - 910px) / 2);
            min-width: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 24px;
            padding: 80px 16px;
            pointer-events: none;
            z-index: 0;
            overflow: hidden;
        }
        .side-panel.left  { left:0; }
        .side-panel.right { right:0; }
        @media(max-width:1180px) { .side-panel { display:none; } }

        /* spinning orbit thing */
        .orbit-wrap {
            position: relative;
            width: 64px; height: 64px;
            pointer-events: all;
        }
        .orbit-outer {
            position: absolute; inset:0;
            border-radius: 50%;
            border: 1.5px solid var(--iris);
            opacity: .2;
            animation: spinSlow 18s linear infinite;
        }
        .orbit-inner {
            position: absolute; inset:10px;
            border-radius: 50%;
            border: 1px dashed var(--foam);
            opacity: .15;
            animation: spinRev 10s linear infinite;
        }
        .orbit-dot {
            position: absolute; top:50%; left:-4px;
            width:8px; height:8px; margin-top:-4px;
            border-radius:50%; background:var(--iris); opacity:.35;
        }

        /* dot grid */
        .dot-grid {
            display: grid;
            grid-template-columns: repeat(4, 6px);
            gap: 6px;
            opacity: .15;
            animation: pulseOpacity 5s ease-in-out infinite;
        }
        .dot-grid span {
            width:6px; height:6px; border-radius:50%;
            display:block;
        }
        .dot-grid span:nth-child(4n+1)  { background:var(--iris); }
        .dot-grid span:nth-child(4n+2)  { background:var(--foam); }
        .dot-grid span:nth-child(4n+3)  { background:var(--rose); }
        .dot-grid span:nth-child(4n)    { background:var(--gold); }

        /* uptime clock */
        .side-clock {
            font-family: 'JetBrains Mono', monospace;
            font-size: 0.62rem;
            color: var(--subtle);
            opacity: .5;
            text-align: center;
            line-height: 1.9;
            pointer-events: all;
        }
        .side-clock .clk { color: var(--iris); opacity: .9; display:block; font-size:.7rem; }

        /* mini terminal — cycling commands */
        .mini-term {
            background: var(--overlay);
            border: 1.5px solid var(--highlight-med);
            border-radius: 8px;
            padding: 8px 12px;
            font-family: 'JetBrains Mono', monospace;
            font-size: 0.6rem;
            color: var(--subtle);
            width: 120px;
            opacity: .55;
            pointer-events: all;
            transition: opacity .3s;
            overflow: hidden;
            min-height: 36px;
        }
        .mini-term:hover { opacity: .9; }
        .mini-term .prompt { color: var(--pine); }
        .mini-term .cmd    { color: var(--text); }
        .mini-term .out    { color: var(--muted); margin-top:2px; display:block; }

        /* vertical label */
        .side-label {
            font-family: 'JetBrains Mono', monospace;
            font-size: 0.58rem;
            letter-spacing: 3px;
            color: var(--muted);
            opacity: .3;
            writing-mode: vertical-rl;
            text-transform: uppercase;
            animation: pulseOpacity 7s ease-in-out infinite;
        }

        /* ping indicator */
        .ping-badge {
            display: flex; align-items: center; gap:6px;
            font-family: 'JetBrains Mono', monospace;
            font-size: 0.6rem;
            color: var(--subtle);
            opacity: .45;
            pointer-events: all;
        }
        .ping-dot {
            width:7px; height:7px; border-radius:50%;
            background: var(--pine);
            box-shadow: 0 0 0 0 var(--pine);
            animation: ping 2s ease-out infinite;
        }
        @keyframes ping {
            0%   { box-shadow:0 0 0 0 rgba(86,148,159,.6); }
            70%  { box-shadow:0 0 0 6px rgba(86,148,159,0); }
            100% { box-shadow:0 0 0 0 rgba(86,148,159,0); }
        }

        /* ══════════════════════════════════════════
           THEME TOGGLE
        ══════════════════════════════════════════ */
        .theme-toggle {
            position: fixed;
            top: 20px; right: 20px;
            z-index: 999;
            background: var(--surface);
            border: 2px solid var(--overlay);
            border-radius: 12px;
            padding: 8px 14px;
            font-family: 'JetBrains Mono', monospace;
            font-size: 0.72rem; font-weight: 700;
            color: var(--text);
            cursor: pointer;
            box-shadow: 4px 4px 0px var(--highlight-med);
            transition: all .2s cubic-bezier(.175,.885,.32,1.275), background-color .4s, color .4s;
            display: flex; align-items: center; gap: 8px;
            user-select: none;
        }
        .theme-toggle:hover {
            transform: translate(-2px,-2px);
            box-shadow: 6px 6px 0px var(--iris);
            border-color: var(--iris);
            color: var(--iris);
        }
        .theme-icon { font-size:1rem; transition:transform .4s; }
        .theme-toggle:hover .theme-icon { transform:rotate(20deg); }

        /* ══════════════════════════════════════════
           LAYOUT
        ══════════════════════════════════════════ */
        .container { max-width:850px; width:100%; position:relative; z-index:1; }
        .container > * { animation:slideIn .6s cubic-bezier(.16,1,.3,1) both; }
        .container > *:nth-child(1){animation-delay:.00s}
        .container > *:nth-child(2){animation-delay:.07s}
        .container > *:nth-child(3){animation-delay:.13s}
        .container > *:nth-child(4){animation-delay:.19s}
        .container > *:nth-child(5){animation-delay:.25s}
        .container > *:nth-child(6){animation-delay:.31s}
        .container > *:nth-child(7){animation-delay:.37s}
        .container > *:nth-child(8){animation-delay:.43s}

        /* ══ Hover-lift ══ */
        .profile-section, .content-box, .terminal-box, .ticker-wrapper {
            transition: transform .25s cubic-bezier(.23,1,.32,1),
                        box-shadow .25s cubic-bezier(.23,1,.32,1),
                        background-color .4s, border-color .4s;
        }
        .profile-section:hover, .content-box:hover, .terminal-box:hover {
            transform: translate(-4px,-4px);
            box-shadow: 12px 12px 0px var(--highlight-high) !important;
        }

        /* ══ Profile ══ */
        .profile-section {
            display:flex; align-items:center; gap:30px;
            margin-bottom:20px;
            background:var(--surface);
            padding:30px; border-radius:20px;
            border:2px solid var(--overlay);
            box-shadow:8px 8px 0px var(--highlight-med);
        }
        .pfp-container { text-align:center; flex-shrink:0; }
        .profile-img {
            width:130px; height:130px;
            border-radius:18px; border:3px solid var(--iris);
            object-fit:cover;
            animation:floating 5s ease-in-out infinite;
            margin-bottom:10px;
            transition:box-shadow .3s, border-color .3s;
        }
        .profile-img:hover {
            border-color:var(--rose);
            box-shadow:0 0 0 4px var(--rose), 0 0 24px rgba(180,99,122,.3);
        }
        .os-badges { display:flex; justify-content:center; gap:10px; }
        .os-badge   { width:28px; height:28px; filter:grayscale(.3); transition:.3s; border-radius:4px; }
        .os-badge:hover { filter:grayscale(0); transform:scale(1.25) rotate(-5deg); }

        .profile-info h1 { font-weight:800; color:var(--love); font-size:1.9rem; margin-bottom:4px; }
        .profile-info .school { font-size:.9rem; color:var(--subtle); margin-bottom:10px; }
        .tags { margin-bottom:12px; }
        .tag {
            font-family:'JetBrains Mono',monospace;
            font-size:.72rem;
            background:var(--foam); color:var(--base);
            padding:2px 10px; border-radius:5px;
            margin-right:6px; margin-bottom:4px;
            display:inline-block;
            transition:background .2s, transform .2s;
        }
        .tag:hover { background:var(--pine); transform:translateY(-2px); }

        /* ══ Socials ══ */
        .socials-container { display:flex; gap:8px; flex-wrap:wrap; }
        .social-btn {
            text-decoration:none;
            font-family:'JetBrains Mono',monospace;
            font-size:.72rem; font-weight:bold;
            padding:6px 13px; border-radius:8px;
            border:2px solid var(--text);
            transition:all .22s cubic-bezier(.175,.885,.32,1.275);
            background:var(--surface); color:var(--text);
            display:inline-flex; align-items:center; gap:5px;
        }
        .social-btn:hover { transform:translateY(-4px) scale(1.06); box-shadow:5px 5px 0px var(--text); }
        .social-btn.ig:hover { background:var(--rose);  color:#fff; border-color:var(--rose); }
        .social-btn.dc:hover { background:var(--iris);  color:#fff; border-color:var(--iris); }
        .social-btn.gh:hover { background:var(--text);  color:var(--base); }
        .social-btn.rd:hover { background:#ff4500;      color:#fff; border-color:#ff4500; }

        /* ══ Content Box ══ */
        .content-box {
            background:var(--surface);
            border:2px solid var(--overlay);
            border-radius:15px; padding:25px;
            margin-bottom:20px;
            box-shadow:8px 8px 0px var(--highlight-med);
        }
        h2 {
            font-family:'JetBrains Mono',monospace;
            font-size:1.1rem; color:var(--rose);
            margin-bottom:15px;
            display:flex; align-items:center; gap:6px;
        }
        h2::before { content:"»"; color:var(--gold); }

        /* ══ Items ══ */
        .item { margin-bottom:16px; }
        .item:last-child { margin-bottom:0; }
        .item-title {
            font-weight:700; color:var(--pine);
            display:inline-block; position:relative;
        }
        .item-title::after {
            content:''; position:absolute; left:0; bottom:-2px;
            height:2px; width:0; background:var(--pine);
            transition:width .3s ease;
        }
        .item:hover .item-title::after { width:100%; }
        .item-sub  { font-size:.82rem; color:var(--subtle); font-family:'JetBrains Mono'; }
        .item-desc { font-size:.88rem; margin-top:5px; color:var(--text); line-height:1.55; }

        /* ══ Skills ══ */
        .skill-badges { display:flex; flex-wrap:wrap; gap:8px; }
        .skill-badge {
            font-family:'JetBrains Mono',monospace;
            font-size:.75rem; padding:4px 12px;
            border-radius:20px; border:2px solid; cursor:default;
            transition:transform .2s cubic-bezier(.23,1,.32,1), color .2s;
            background-image:linear-gradient(currentColor,currentColor);
            background-repeat:no-repeat; background-position:left center;
            background-size:0% 100%;
        }
        .skill-badge:hover { transform:translateY(-3px) scale(1.08); background-size:100% 100%; color:var(--base) !important; }
        .skill-badge.linux  { border-color:var(--pine); color:var(--pine); }
        .skill-badge.net    { border-color:var(--foam); color:var(--foam); }
        .skill-badge.code   { border-color:var(--iris); color:var(--iris); }
        .skill-badge.design { border-color:var(--rose); color:var(--rose); }

        /* ══ Terminal ══ */
        .terminal-box {
            background:#1f1d2e;
            border:2px solid #403d52;
            border-radius:15px; overflow:hidden;
            margin-bottom:20px;
            box-shadow:8px 8px 0px var(--highlight-med);
        }
        .fetch-header {
            background:#2a2739; padding:10px 16px;
            display:flex; align-items:center; gap:8px;
            border-bottom:2px solid #403d52;
        }
        .dot { width:12px; height:12px; border-radius:50%; cursor:pointer; transition:transform .2s; }
        .dot:hover { transform:scale(1.3); }
        .d-love{background:var(--love)} .d-gold{background:var(--gold)} .d-pine{background:var(--pine)}
        .terminal-title { margin-left:auto; font-size:11px; font-family:'JetBrains Mono'; color:#6e6a86; }
        .cursor {
            display:inline-block; width:7px; height:12px;
            background:#6e6a86; border-radius:1px;
            animation:blink 1s step-end infinite;
            vertical-align:middle; margin-left:2px;
        }
        .fetch-body {
            display:flex; flex-wrap:wrap; gap:30px; align-items:flex-start;
            font-family:'JetBrains Mono',monospace; font-size:.82rem;
            padding:24px 28px; background:#1f1d2e;
        }
        .ascii-art { color:#c4a7e7; white-space:pre; line-height:1.15; font-weight:bold; font-size:.75rem; flex-shrink:0; }
        .fetch-info { flex:1; min-width:200px; }
        .fetch-row  { margin-bottom:7px; display:flex; gap:10px; }
        .key { color:#eb6f92; font-weight:bold; min-width:80px; flex-shrink:0; }
        .val { color:#e0def4; }
        .sep { color:#6e6a86; }
        .fetch-uptime { margin-top:16px; display:flex; gap:6px; flex-wrap:wrap; }
        .uptime-tag { font-size:.68rem; background:#403d52; color:#e0def4; padding:2px 8px; border-radius:4px; border:1px solid #524f67; }
        .color-dots { margin-top:14px; display:flex; gap:6px; }
        .color-dot { width:16px; height:16px; border-radius:4px; transition:transform .2s; cursor:default; }
        .color-dot:hover { transform:scale(1.4); }

        /* ══ Ticker ══ */
        .ticker-wrapper {
            overflow:hidden; background:var(--text);
            border-radius:12px; border:2px solid var(--text);
            margin-bottom:20px;
            display:flex; align-items:stretch;
            box-shadow:8px 8px 0px var(--highlight-med);
        }
        .ticker-wrapper:hover { transform:translate(-4px,-4px); box-shadow:12px 12px 0px var(--highlight-high); }
        .ticker-label {
            background:var(--love); color:var(--base);
            font-family:'JetBrains Mono',monospace;
            font-size:.7rem; font-weight:700;
            padding:0 14px; display:flex; align-items:center;
            white-space:nowrap; letter-spacing:.05em; flex-shrink:0; gap:6px;
        }
        .ticker-track { overflow:hidden; flex:1; padding:10px 0; }
        .ticker-inner { display:flex; width:max-content; animation:ticker-scroll 55s linear infinite; }
        .ticker-inner:hover { animation-play-state:paused; }
        .ticker-item { font-family:'JetBrains Mono',monospace; font-size:.78rem; color:var(--base); white-space:nowrap; padding:0 28px; opacity:.9; transition:opacity .2s; }
        .ticker-item:hover { opacity:1; }
        .ticker-sep { color:var(--gold); padding:0 4px; }

        /* ══ Footer ══ */
        .footer { text-align:center; padding:30px 0 40px; }
        .konata { width:65px; mix-blend-mode:multiply; transition:transform .3s; }
        [data-theme="moon"] .konata { mix-blend-mode:screen; }
        .konata:hover { transform:scale(1.2) rotate(5deg); }
        .footer-note { font-size:.68rem; color:var(--muted); margin-top:10px; font-family:'JetBrains Mono'; }

        @media(max-width:600px){
            .profile-section { flex-direction:column; text-align:center; }
            .fetch-body { flex-direction:column; align-items:flex-start; }
            .socials-container { justify-content:center; }
            .tags { text-align:center; }
            .theme-toggle { top:12px; right:12px; }
        }
    </style>
</head>
<body>

<button class="theme-toggle" id="themeBtn" aria-label="Toggle dark mode">
    <span class="theme-icon" id="themeIcon">🌙</span>
    <span id="themeLabel">Moon</span>
</button>

<div class="side-panel left" aria-hidden="true">
    <div class="orbit-wrap">
        <div class="orbit-outer"></div>
        <div class="orbit-inner"></div>
        <div class="orbit-dot"></div>
    </div>

    <div class="side-clock">
        <span class="clk" id="clock-left">--:--:--</span>
        uptime<br>∞ hrs
    </div>

    <div class="dot-grid" id="left-grid"></div>

    <div class="mini-term" id="left-term">
        <span class="prompt">$ </span><span class="cmd" id="lt-cmd"></span><br>
        <span class="out" id="lt-out"></span>
    </div>

    <div class="side-label">rayyan sys</div>

    <div class="ping-badge">
        <div class="ping-dot"></div>
        <span>online</span>
    </div>

    <div class="orbit-wrap">
        <div class="orbit-outer" style="animation-duration:25s"></div>
        <div class="orbit-inner" style="animation-duration:14s"></div>
        <div class="orbit-dot" style="background:var(--rose)"></div>
    </div>
</div>

<div class="side-panel right" aria-hidden="true">
    <div class="dot-grid" id="right-grid"></div>

    <div class="mini-term" id="right-term">
        <span class="prompt">$ </span><span class="cmd" id="rt-cmd"></span><br>
        <span class="out" id="rt-out"></span>
    </div>

    <div class="side-label">portfolio v3</div>

    <div class="orbit-wrap">
        <div class="orbit-outer" style="animation-duration:22s;border-color:var(--foam)"></div>
        <div class="orbit-inner" style="animation-duration:12s"></div>
        <div class="orbit-dot" style="background:var(--foam)"></div>
    </div>

    <div class="side-clock">
        <span class="clk" id="clock-right">--/--/----</span>
        jakarta<br>wib
    </div>

    <div class="dot-grid"></div>
</div>

<div class="container">

    <div class="profile-section">
        <div class="pfp-container">
            <img src="https://cdn.discordapp.com/attachments/945191414768218122/1496756321960333413/image.png?ex=69eb0a73&is=69e9b8f3&hm=c6d22741e946a69fbe061100c2ec0e0f5147ad3c5519ebb990811616f70fbaeb&"
                 class="profile-img" alt="Rayyan">
            <div class="os-badges">
                <img src="https://upload.wikimedia.org/wikipedia/commons/3/35/Tux.svg"
                     class="os-badge" title="Linux">
                <img src="https://www.vectorlogo.zone/logos/freebsd/freebsd-icon.svg"
                     class="os-badge" title="FreeBSD">
            </div>
        </div>
        <div class="profile-info">
            <h1>Rayyan Ali Rizki H.</h1>
            <p class="school">Student @ SMK Bina Informatika</p>
            <div class="tags">
                <span class="tag">#linux_enthusiast</span>
                <span class="tag">#system_engineering</span>
            </div>
            <div class="socials-container">
                <a href="https://www.instagram.com/rayy.ism/" class="social-btn ig" target="_blank">IG Instagram</a>
                <a href="https://discord.com/users/lunaticrayy7" class="social-btn dc" target="_blank">⌨ Discord</a>
                <a href="https://github.com/idnnn" class="social-btn gh" target="_blank">⚡ GitHub</a>
                <a href="https://www.reddit.com/user/IndonesianRedditor8/" class="social-btn rd" target="_blank">🟠 Reddit</a>
            </div>
        </div>
    </div>

    <div class="content-box">
        <h2>Tentang Saya</h2>
        <p style="line-height:1.65;">
            Saya seorang siswa di <strong>SMK Bina Informatika</strong> yang berfokus pada
            <em>computer engineering</em> dan sistem operasi. Saya mendalami ekosistem Linux dan Unix
            untuk menciptakan lingkungan komputasi yang optimal dan terpersonalisasi. Fokus saya adalah
            pada stabilitas sistem dan pemahaman mendalam terhadap alur kerja di balik layar sebuah OS.
        </p>
    </div>

    <div class="terminal-box">
        <div class="fetch-header">
            <div class="dot d-love" title="Close"></div>
            <div class="dot d-gold" title="Minimize"></div>
            <div class="dot d-pine" title="Maximize"></div>
            <span class="terminal-title">rayyan@rose-pine ~ <span class="cursor"></span></span>
        </div>
        <div class="fetch-body">
<pre class="ascii-art">
 ██████╗  █████╗ ██╗   ██╗██╗   ██╗ █████╗ ███╗  ██╗
 ██╔══██╗██╔══██╗╚██╗ ██╔╝╚██╗ ██╔╝██╔══██╗████╗ ██║
 ██████╔╝███████║ ╚████╔╝  ╚████╔╝ ███████║██╔██╗██║
 ██╔══██╗██╔══██║  ╚██╔╝    ╚██╔╝  ██╔══██║██║╚████║
 ██║  ██║██║  ██║   ██║      ██║   ██║  ██║██║ ╚███║
 ╚═╝  ╚═╝╚═╝  ╚═╝   ╚═╝      ╚═╝   ╚═╝  ╚═╝╚═╝  ╚═╝

 rayyan@porto
 ────────────────</pre>
            <div class="fetch-info">
                <div class="fetch-row"><span class="key">OS</span><span class="sep">~</span><span class="val">Debian, Arch-based-distros, Freebsd, Nixos, Etc</span></div>
                <div class="fetch-row"><span class="key">School</span><span class="sep">~</span><span class="val">Bina-Informatika-Bintaro</span></div>
                <div class="fetch-row"><span class="key">Terminal</span><span class="sep">~</span><span class="val">kitty</span></div>
                <div class="fetch-row"><span class="key">Editor</span><span class="sep">~</span><span class="val">nano/vscode/vim</span></div>
                <div class="fetch-row"><span class="key">Font</span><span class="sep">~</span><span class="val">JetBrains Mono</span></div>
                <div class="fetch-row"><span class="key">Age</span><span class="sep">~</span><span class="val">16 years old</span></div>
                <div class="fetch-uptime">
                    <span class="uptime-tag">linux</span>
                    <span class="uptime-tag">unix</span>
                    <span class="uptime-tag">networking</span>
                </div>
                <div class="color-dots" style="margin-top:14px;">
                    <div class="color-dot" style="background:#eb6f92" title="Love"></div>
                    <div class="color-dot" style="background:#f6c177" title="Gold"></div>
                    <div class="color-dot" style="background:#ea9a97" title="Rose"></div>
                    <div class="color-dot" style="background:#3e8fb0" title="Pine"></div>
                    <div class="color-dot" style="background:#9ccfd8" title="Foam"></div>
                    <div class="color-dot" style="background:#c4a7e7" title="Iris"></div>
                    <div class="color-dot" style="background:#e0def4" title="Text"></div>
                    <div class="color-dot" style="background:#6e6a86" title="Subtle"></div>
                </div>
            </div>
        </div>
    </div>

    <div class="content-box">
        <h2>Skills & Tools</h2>
        <div class="skill-badges">
            <span class="skill-badge linux">Linux Admin</span>
            <span class="skill-badge linux">FreeBSD</span>
            <span class="skill-badge linux">Arch Linux</span>
            <span class="skill-badge net">Networking</span>
            <span class="skill-badge net">Cisco Packet Tracer</span>
            <span class="skill-badge net">TCP/IP</span>
            <span class="skill-badge code">Git</span>
            <span class="skill-badge code">HTML / CSS</span>
            <span class="skill-badge code">Roblox Studio</span>
            <span class="skill-badge design">UI Design</span>
            <span class="skill-badge design">Rosé Pine</span>
        </div>
    </div>

    <div class="content-box">
        <h2>Pendidikan</h2>
        <div class="item">
            <p class="item-title">SMK Bina Informatika Bintaro</p>
            <p class="item-sub">Teknik Komputer dan Jaringan · 2025 – Sekarang</p>
        </div>
        <div class="item">
            <p class="item-title">SMP Negeri 178 Jakarta</p>
            <p class="item-sub">Lulus Tahun 2025</p>
        </div>
    </div>

    <div class="content-box">
        <h2>Pengalaman & Projek</h2>
        <div class="item">
            <p class="item-title">Network Infrastructure Project</p>
            <p class="item-sub">School Project · 2025</p>
            <p class="item-desc">Membangun dan mengonfigurasi topologi jaringan lokal yang stabil untuk kebutuhan sekolah, mencakup routing, subnetting, dan troubleshooting.</p>
        </div>
        <div class="item">
            <p class="item-title">System Customization</p>
            <p class="item-sub">Personal Project · Ongoing</p>
            <p class="item-desc">Eksperimen desain antarmuka desktop dan web menggunakan palet warna Rosé Pine — termasuk konfigurasi WM, bar, dan terminal dari nol.</p>
        </div>
        <div class="item">
            <p class="item-title">Linux Distribution</p>
            <p class="item-sub">Personal · Ongoing</p>
            <p class="item-desc">Migrasi admin tool dari sudo ke doas, kustomisasi environment di sistem Linux/Unix, dan otomasi tugas harian menggunakan shell scripting.</p>
        </div>
    </div>

    <footer class="footer">
        <img src="https://64.media.tumblr.com/17ea99e3f67170ccce48bcaa4f2853d2/a99ee2e357a0bd1d-b1/s400x600/5c07ec5f31e087e1ff3ecc93ffef3005ca5c0fa7.gif" class="konata" alt="Konata">
        <p class="footer-note" id="footer-note">rayy</p>
    </footer>
</div>

<script>
/* ── dot grids ── */
['left-grid','right-grid'].concat([...document.querySelectorAll('.dot-grid:not(#left-grid):not(#right-grid)')]).forEach(el => {
    const g = typeof el === 'string' ? document.getElementById(el) : el;
    if (!g) return;
    for (let i = 0; i < 16; i++) { const s = document.createElement('span'); g.appendChild(s); }
});

/* ── live clock ── */
function tick() {
    const now = new Date();
    const t = now.toLocaleTimeString('id-ID', { hour:'2-digit', minute:'2-digit', second:'2-digit' });
    const d = now.toLocaleDateString('id-ID', { day:'2-digit', month:'2-digit', year:'numeric' });
    if(document.getElementById('clock-left')) document.getElementById('clock-left').textContent = t;
    if(document.getElementById('clock-right')) document.getElementById('clock-right').textContent = d;
}
tick(); setInterval(tick, 1000);

/* ── terminal commands ── */
const cmds = [
    { cmd: "ls projects/", out: "network_infra  sys_custom  scripts" },
    { cmd: "whoami", out: "rayyan" },
    { cmd: "uname -a", out: "Linux rose-pine 6.x-arch1-1" },
    { cmd: "neofetch", out: "loading system info..." },
    { cmd: "cat about.txt", out: "Student at SMK Bina Informatika." },
    { cmd: "ping google.com", out: "64 bytes from ... time=12ms" }
];

let ci = 0;
function cycleTerm(cmdEl, outEl, offset) {
    if(!cmdEl || !outEl) return;
    const entry = cmds[(ci + offset) % cmds.length];
    cmdEl.textContent = '';
    outEl.textContent = '';
    let i = 0;
    const type = setInterval(() => {
        cmdEl.textContent += entry.cmd[i++] || '';
        if (i >= entry.cmd.length) {
            clearInterval(type);
            setTimeout(() => { outEl.textContent = entry.out; }, 300);
        }
    }, 60);
}

const ltCmd = document.getElementById('lt-cmd');
const ltOut = document.getElementById('lt-out');
const rtCmd = document.getElementById('rt-cmd');
const rtOut = document.getElementById('rt-out');

function nextCycle() {
    cycleTerm(ltCmd, ltOut, 0);
    cycleTerm(rtCmd, rtOut, 3);
    ci++;
}
nextCycle();
setInterval(nextCycle, 5000);

/* ── theme toggle ── */
const btn   = document.getElementById('themeBtn');
const icon  = document.getElementById('themeIcon');
const label = document.getElementById('themeLabel');
const note  = document.getElementById('footer-note');
let dark = false;

btn.addEventListener('click', () => {
    dark = !dark;
    document.documentElement.setAttribute('data-theme', dark ? 'moon' : 'dawn');
    icon.textContent  = dark ? '☀️' : '🌙';
    label.textContent = dark ? 'Light' : 'Dark';
    note.textContent  = dark
        ? '-- rayy --'
        : '-- rayy --';
});
</script>
</body>
</html>
