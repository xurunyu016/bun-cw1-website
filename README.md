[index.html](https://github.com/user-attachments/files/35328689/index.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Bun · All-in-One JavaScript Runtime — COMP10020 CW1</title>
<style>
  :root{
    --bg:#0b0c0f;
    --panel:#13151b;
    --panel-2:#1a1d25;
    --panel-3:#21252e;
    --line:#262a33;
    --text:#ece7dc;
    --muted:#9aa0ac;
    --cream:#f3ead8;
    --orange:#ff6a3d;
    --mint:#7ee2b8;
    --red:#f2708a;
    --code:#0a0b0e;
    --radius:14px;
    --shadow:0 14px 34px rgba(0,0,0,.35);
  }
  *{box-sizing:border-box;margin:0;padding:0}
  html{scroll-behavior:smooth}
  body{
    font-family:"Inter",-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,sans-serif;
    background:var(--bg);
    color:var(--text);
    line-height:1.65;
    -webkit-font-smoothing:antialiased;
    min-height:100vh;
  }
  /* ===== Layered animated background ===== */
  .bg-layer{position:fixed;inset:0;z-index:-2;overflow:hidden}
  .blob{position:absolute;border-radius:50%;filter:blur(90px);opacity:.35;animation:drift 22s ease-in-out infinite alternate}
  .blob.b1{width:560px;height:560px;left:-160px;top:-140px;background:radial-gradient(circle,#ff6a3d,transparent 65%)}
  .blob.b2{width:520px;height:520px;right:-180px;bottom:-160px;background:radial-gradient(circle,#3d6bff,transparent 65%);animation-delay:-8s}
  .blob.b3{width:380px;height:380px;left:38%;top:45%;background:radial-gradient(circle,#7ee2b8,transparent 65%);opacity:.16;animation-delay:-14s}
  @keyframes drift{
    from{transform:translate(0,0) scale(1)}
    to{transform:translate(60px,40px) scale(1.12)}
  }
  .bg-grid{
    position:fixed;inset:0;z-index:-1;
    background-image:radial-gradient(rgba(236,231,220,.07) 1px,transparent 1px);
    background-size:26px 26px;
    mask-image:linear-gradient(to bottom,transparent,black 18%,black 82%,transparent);
  }
  /* ===== Scroll progress ===== */
  #progress{position:fixed;top:0;left:0;height:2.5px;width:0;background:linear-gradient(90deg,var(--orange),var(--mint));z-index:200}

  /* ===== Layout shell ===== */
  .shell{display:grid;grid-template-columns:250px 1fr;max-width:1280px;margin:0 auto;gap:0}
  aside.side{
    position:sticky;top:0;height:100vh;overflow-y:auto;
    padding:28px 18px;border-right:1px solid var(--line);
  }
  .brand{display:flex;align-items:center;gap:10px;margin-bottom:28px;cursor:pointer}
  .brand-logo{
    width:38px;height:38px;border-radius:11px;flex:none;
    background:linear-gradient(135deg,var(--cream),#d9c9a8);
    display:grid;place-items:center;font-weight:800;color:#141413;font-size:19px;
  }
  .brand-text{font-weight:700;letter-spacing:-.02em}
  .brand-text small{display:block;color:var(--muted);font-weight:400;font-size:11px;letter-spacing:.08em;text-transform:uppercase}
  .nav-list{list-style:none;display:flex;flex-direction:column;gap:3px}
  .nav-list button{
    width:100%;text-align:left;background:transparent;border:none;color:var(--muted);
    padding:9px 12px;border-radius:9px;font-size:.89rem;cursor:pointer;transition:.16s;
    display:flex;gap:10px;align-items:center;
  }
  .nav-list button .idx{font-family:ui-monospace,Menlo,monospace;font-size:.72rem;color:#5c6270;width:22px}
  .nav-list button:hover{color:var(--text);background:var(--panel-2)}
  .nav-list button.active{background:var(--panel-3);color:var(--cream)}
  .nav-list button.active .idx{color:var(--orange)}
  .side-foot{margin-top:28px;padding-top:18px;border-top:1px solid var(--line);font-size:.76rem;color:var(--muted)}

  main{padding:36px 34px 90px;min-width:0}
  .view{display:none;animation:fade .28s ease}
  .view.active{display:block}
  @keyframes fade{from{opacity:0;transform:translateY(8px)}to{opacity:1;transform:none}}

  /* ===== Hero ===== */
  .hero{margin-bottom:42px}
  .eyebrow{
    display:inline-flex;align-items:center;gap:8px;color:var(--orange);
    font-size:.76rem;font-weight:700;letter-spacing:.16em;text-transform:uppercase;margin-bottom:18px;
  }
  .eyebrow::before{content:"";width:26px;height:1px;background:var(--orange)}
  h1{font-size:clamp(2rem,4.4vw,3.2rem);line-height:1.12;letter-spacing:-.03em;margin-bottom:16px}
  h1 .cream{color:var(--cream)}
  .hero p.lead{color:var(--muted);max-width:660px;font-size:1.05rem}
  .members{display:flex;flex-wrap:wrap;gap:10px;margin-top:24px}
  .chip{
    border:1px solid var(--line);background:var(--panel);border-radius:999px;
    padding:7px 14px;font-size:.83rem;display:flex;gap:8px;align-items:center;
  }
  .chip b{color:var(--cream)}
  .chip span{color:var(--muted);font-family:ui-monospace,Menlo,monospace;font-size:.74rem}

  /* ===== Terminal card ===== */
  .terminal{
    background:var(--code);border:1px solid var(--line);border-radius:var(--radius);
    margin-top:28px;overflow:hidden;box-shadow:var(--shadow);max-width:680px;
  }
  .term-bar{display:flex;gap:7px;padding:12px 15px;border-bottom:1px solid var(--line)}
  .dot{width:11px;height:11px;border-radius:50%}
  .dot.r{background:#ff5f57}.dot.y{background:#febc2e}.dot.g{background:#28c840}
  .term-body{padding:18px 20px;font-family:ui-monospace,Menlo,Consolas,monospace;font-size:.86rem;line-height:1.8}
  .term-body .prompt{color:var(--mint)}
  .term-body .out{color:var(--muted)}
  #typed{color:var(--cream)}
  .caret{display:inline-block;width:8px;height:15px;background:var(--orange);vertical-align:middle;animation:blink 1s steps(1) infinite}
  @keyframes blink{50%{opacity:0}}

  /* ===== Generic blocks ===== */
  h2{font-size:1.65rem;letter-spacing:-.02em;margin-bottom:8px}
  .page-sub{color:var(--muted);margin-bottom:26px;max-width:680px}
  h3{font-size:1.08rem;margin:0 0 10px;color:var(--cream)}
  .grid{display:grid;gap:16px}
  .g2{grid-template-columns:repeat(auto-fit,minmax(290px,1fr))}
  .g3{grid-template-columns:repeat(auto-fit,minmax(240px,1fr))}
  .card{
    background:var(--panel);border:1px solid var(--line);border-radius:var(--radius);
    padding:22px;transition:border-color .18s,transform .18s;
  }
  .card:hover{border-color:#343946;transform:translateY(-2px)}
  .card p,.card li{color:var(--muted);font-size:.93rem}
  .card ul{padding-left:1.15rem;margin-top:6px}
  .card li{margin-bottom:6px}
  .kicker{font-size:.7rem;font-weight:700;letter-spacing:.12em;text-transform:uppercase;color:var(--orange);margin-bottom:10px}
  .tag{display:inline-block;font-family:ui-monospace,Menlo,monospace;font-size:.74rem;font-weight:700;color:var(--cream);background:var(--panel-3);border:1px solid var(--line);padding:2px 9px;border-radius:6px;margin-bottom:10px}
  section.block{margin-bottom:38px}

  /* ===== Tiles (home) ===== */
  .tiles{display:grid;grid-template-columns:repeat(auto-fit,minmax(250px,1fr));gap:16px}
  .tile{cursor:pointer}
  .tile .num{font-family:ui-monospace,Menlo,monospace;color:#5c6270;font-size:.8rem;margin-bottom:12px}
  .tile h3{margin-bottom:8px}
  .tile:hover .num{color:var(--orange)}

  /* ===== Pain → merge ===== */
  .pain-row{display:flex;flex-wrap:wrap;gap:10px;margin:14px 0}
  .tool-chip{
    border:1px dashed var(--line);background:var(--panel-2);border-radius:10px;
    padding:10px 14px;font-family:ui-monospace,Menlo,monospace;font-size:.85rem;color:var(--text);
  }
  .arrow{align-self:center;color:var(--muted)}
  .merge-btn{
    background:var(--orange);color:#141413;border:none;font-weight:700;font-size:.88rem;
    padding:10px 18px;border-radius:10px;cursor:pointer;margin:14px 0;transition:.16s;
  }
  .merge-btn:hover{filter:brightness(1.1)}
  .bun-chip{
    display:inline-flex;align-items:center;gap:10px;border:1px solid var(--orange);
    background:linear-gradient(135deg,rgba(255,106,61,.16),rgba(255,106,61,.04));
    border-radius:14px;padding:16px 22px;font-weight:800;font-size:1.05rem;letter-spacing:-.01em;
  }
  .bun-chip small{display:block;font-weight:400;color:var(--muted);font-size:.78rem}
  .merged .tool-chip{opacity:.18;transition:.3s}

  /* ===== Timeline ===== */
  .timeline{display:grid;grid-template-columns:repeat(auto-fit,minmax(170px,1fr));gap:0;margin-top:18px}
  .tl-item{position:relative;padding:18px 16px;border-left:2px solid var(--line)}
  .tl-item::before{content:"";position:absolute;left:-7px;top:22px;width:12px;height:12px;border-radius:50%;background:var(--orange);border:2px solid var(--bg)}
  .tl-year{font-family:ui-monospace,Menlo,monospace;color:var(--orange);font-weight:700;font-size:.85rem;margin-bottom:6px}
  .tl-item p{font-size:.85rem;color:var(--muted)}

  /* ===== Tables ===== */
  table{width:100%;border-collapse:collapse;background:var(--panel);border:1px solid var(--line);border-radius:var(--radius);overflow:hidden;font-size:.9rem}
  th,td{padding:12px 15px;text-align:left;border-bottom:1px solid var(--line)}
  th{background:var(--panel-2);color:var(--cream);font-weight:600}
  td{color:var(--muted)}
  td:first-child{color:var(--text)}
  tr:last-child td{border-bottom:none}
  code{font-family:ui-monospace,Menlo,Consolas,monospace;color:var(--cream)}
  p code,li code,td code{background:var(--panel-3);padding:2px 6px;border-radius:5px;font-size:.83em}

  pre{
    background:var(--code);border:1px solid var(--line);border-radius:10px;
    padding:16px;overflow-x:auto;font-family:ui-monospace,Menlo,Consolas,monospace;
    font-size:.84rem;line-height:1.65;margin:10px 0;color:#e8e4d8;
  }
  pre .cm{color:#6b7280}
  pre .kw{color:var(--orange)}

  /* ===== Benchmark bars ===== */
  .bench-tabs{display:flex;flex-wrap:wrap;gap:8px;margin-bottom:18px}
  .bench-tabs button{
    background:var(--panel-2);border:1px solid var(--line);color:var(--muted);
    padding:8px 14px;border-radius:9px;font-size:.84rem;cursor:pointer;transition:.16s;
  }
  .bench-tabs button.active{background:var(--orange);color:#141413;font-weight:700;border-color:var(--orange)}
  .bar-row{margin:16px 0}
  .bar-label{display:flex;justify-content:space-between;font-size:.87rem;margin-bottom:6px}
  .bar-label .v{font-family:ui-monospace,Menlo,monospace;color:var(--cream)}
  .bar-track{height:13px;background:var(--panel-2);border-radius:7px;overflow:hidden}
  .bar-fill{height:100%;border-radius:7px;width:0;transition:width .7s cubic-bezier(.2,.7,.2,1)}
  .bar-fill.hot{background:linear-gradient(90deg,var(--orange),#ff9a6d)}
  .bar-fill.cool{background:#5c6270}
  .note{border-left:3px solid var(--mint);background:rgba(126,226,184,.07);padding:13px 16px;border-radius:0 10px 10px 0;font-size:.87rem;color:var(--muted);margin:14px 0}
  .note.warn{border-left-color:var(--orange);background:rgba(255,106,61,.07)}

  /* ===== Verdict columns ===== */
  .verdict{display:grid;grid-template-columns:1fr 1fr;gap:16px}
  @media(max-width:760px){.verdict{grid-template-columns:1fr}}
  .v-card{border-radius:var(--radius);padding:22px;border:1px solid var(--line)}
  .v-card.go{background:rgba(126,226,184,.06);border-color:rgba(126,226,184,.3)}
  .v-card.stay{background:rgba(242,112,138,.06);border-color:rgba(242,112,138,.3)}
  .v-card h3{margin-bottom:12px}
  .v-card.go h3{color:var(--mint)}
  .v-card.stay h3{color:var(--red)}
  .v-card ul{padding-left:1.15rem}
  .v-card li{color:var(--muted);margin-bottom:8px;font-size:.92rem}

  .refs li{color:var(--muted);font-size:.88rem;margin-bottom:10px}
  .refs li strong{color:var(--text)}

  footer{border-top:1px solid var(--line);margin-top:50px;padding:26px 34px;color:var(--muted);font-size:.82rem;text-align:center}

  /* ===== Mobile nav ===== */
  .mobile-nav{display:none}
  @media(max-width:900px){
    .shell{grid-template-columns:1fr}
    aside.side{position:static;height:auto;border-right:none;border-bottom:1px solid var(--line);padding:16px}
    .nav-list{flex-direction:row;flex-wrap:wrap}
    main{padding:24px 18px 70px}
  }
</style>
</head>
<body>

<div class="bg-layer">
  <div class="blob b1"></div><div class="blob b2"></div><div class="blob b3"></div>
</div>
<div class="bg-grid"></div>
<div id="progress"></div>

<div class="shell">

  <!-- ===== Sidebar ===== -->
  <aside class="side">
    <div class="brand" onclick="go('home')">
      <div class="brand-logo">b</div>
      <div class="brand-text">Bun <small>CW1 Study Site</small></div>
    </div>
    <ul class="nav-list" id="navList">
      <li><button data-view="home" class="active"><span class="idx">00</span> Home</button></li>
      <li><button data-view="why"><span class="idx">01</span> Why &amp; Architecture</button></li>
      <li><button data-view="features"><span class="idx">02</span> Features &amp; Speed</button></li>
      <li><button data-view="compare"><span class="idx">03</span> Comparison</button></li>
      <li><button data-view="cheatsheet"><span class="idx">04</span> Cheat Sheet</button></li>
      <li><button data-view="tutorial"><span class="idx">05</span> Tutorial</button></li>
      <li><button data-view="verdict"><span class="idx">06</span> Verdict</button></li>
      <li><button data-view="refs"><span class="idx">07</span> References</button></li>
    </ul>
    <div class="side-foot">COMP10020 Internet Technologies<br />UWS · Web Technology · 2026</div>
  </aside>

  <main>

    <!-- ========== 00 HOME ========== -->
    <section id="view-home" class="view active">
      <div class="hero">
        <div class="eyebrow">COMP10020 · Internet Technologies · CW1</div>
        <h1>One binary. <span class="cream">Five jobs.</span><br />The Bun runtime, unbundled.</h1>
        <p class="lead">
          Bun is a fast JavaScript and TypeScript runtime, package manager, bundler and test runner
          written in Zig on JavaScriptCore — an alternative to Node.js that consolidates the whole
          modern JS toolchain into a single executable.
        </p>
        <div class="members">
          <div class="chip"><b>Runyu Xu</b><span>B01857658</span></div>
          <div class="chip"><b>Yijie Shen</b><span>B01857661</span></div>
          <div class="chip"><b>Jiating Zhang</b><span>B01857710</span></div>
          <div class="chip"><b>Tianle Xi</b><span>B01857681</span></div>
        </div>

        <div class="terminal">
          <div class="term-bar"><span class="dot r"></span><span class="dot y"></span><span class="dot g"></span></div>
          <div class="term-body">
            <div><span class="prompt">$</span> <span id="typed"></span><span class="caret"></span></div>
            <div class="out" id="termOut"></div>
          </div>
        </div>
      </div>

      <div class="tiles">
        <div class="card tile" onclick="go('why')">
          <div class="num">01</div>
          <h3>Why Bun &amp; Architecture</h3>
          <p>The five-tool fragmentation problem, Zig + JavaScriptCore internals, and the project timeline.</p>
        </div>
        <div class="card tile" onclick="go('features')">
          <div class="num">02</div>
          <h3>Features &amp; Speed</h3>
          <p>One binary, five jobs; native APIs, package manager, benchmarks and the real-world gap.</p>
        </div>
        <div class="card tile" onclick="go('compare')">
          <div class="num">03</div>
          <h3>Comparison &amp; Compatibility</h3>
          <p>Bun vs Node.js vs Deno, what works today, where you hit walls, and who already adopts it.</p>
        </div>
        <div class="card tile" onclick="go('cheatsheet')">
          <div class="num">04</div>
          <h3>One-page Cheat Sheet</h3>
          <p>Essential CLI commands and built-in APIs, each mapped to the tool it replaces.</p>
        </div>
        <div class="card tile" onclick="go('tutorial')">
          <div class="num">05</div>
          <h3>Hands-on Tutorial</h3>
          <p>Install Bun, build a TypeScript HTTP + SQLite API, add tests — about 30 minutes.</p>
        </div>
        <div class="card tile" onclick="go('verdict')">
          <div class="num">06</div>
          <h3>The Verdict</h3>
          <p>Limitations, production caveats, when to choose Bun, and when to stay with Node.js.</p>
        </div>
      </div>
    </section>

    <!-- ========== 01 WHY ========== -->
    <section id="view-why" class="view">
      <div class="page-header">
        <h2>Why Bun? The problem it solves</h2>
        <p class="page-sub">Speaker 1 — what Bun is, the pain of the Node.js toolchain, and its unusual architecture.</p>
      </div>

      <div class="grid g2 block">
        <div class="card">
          <span class="tag">What is Bun?</span>
          <h3>A runtime and toolkit, not just a faster Node</h3>
          <p>
            Bun bundles a <strong>runtime</strong>, <strong>package manager</strong>,
            <strong>test runner</strong>, <strong>bundler</strong> and <strong>TypeScript
            transpiler</strong> into one self-contained executable.
          </p>
          <ul>
            <li><strong>Creator:</strong> Jarred Sumner; open-sourced 2022</li>
            <li><strong>Written in:</strong> Zig — a low-level, hand-tuned systems language</li>
            <li><strong>Engine:</strong> JavaScriptCore (WebKit/Safari), not V8</li>
            <li><strong>Release:</strong> v1.0 shipped September 2023; website bun.sh</li>
          </ul>
        </div>
        <div class="card" id="mergeCard">
          <span class="tag">Fragmentation pain</span>
          <h3>With Node.js you stitch five tools together</h3>
          <div class="pain-row" id="painRow">
            <span class="tool-chip">node</span>
            <span class="tool-chip">npm / yarn / pnpm</span>
            <span class="tool-chip">vite / webpack</span>
            <span class="tool-chip">jest / vitest</span>
            <span class="tool-chip">tsc</span>
          </div>
          <button class="merge-btn" onclick="mergeTools()">Merge into one binary →</button>
          <div class="bun-chip" id="bunChip" style="display:none">
            <div>bun<small>runtime · install · test · build · transpile</small></div>
          </div>
          <p style="margin-top:12px">What hurts: slow installs, slow cold starts for serverless, five config files, and version drift between tools.</p>
        </div>
      </div>

      <div class="grid g2 block">
        <div class="card">
          <h3>Ingredient 1 — Zig</h3>
          <p>
            A low-level systems language, like C but modern. Bun's networking, file I/O and
            transpiling are hand-tuned in Zig: no heavyweight GC, minimal startup overhead.
            <em>Compare: Node is C++, Deno is Rust.</em>
          </p>
        </div>
        <div class="card">
          <h3>Ingredient 2 — JavaScriptCore</h3>
          <p>
            Apple's JavaScript engine, the one inside Safari. It gives faster cold start and lower
            memory on short scripts than V8, which is tuned for long-running applications.
            <em>Compare: Node and Deno both use V8.</em>
          </p>
        </div>
      </div>

      <div class="card block">
        <span class="kicker">Timeline</span>
        <h3>From side project to Anthropic-backed</h3>
        <div class="timeline">
          <div class="tl-item"><div class="tl-year">2022</div><p>First release — Jarred Sumner open-sources the runtime.</p></div>
          <div class="tl-item"><div class="tl-year">Sep 2023</div><p>Bun 1.0 — production-ready runtime shipped.</p></div>
          <div class="tl-item"><div class="tl-year">2024</div><p>Windows support matures; <code>Bun.serve</code> and <code>bun install</code> stabilise.</p></div>
          <div class="tl-item"><div class="tl-year">2025</div><p>Joins Anthropic; adopted by Vercel, Cloudflare and startups.</p></div>
          <div class="tl-item"><div class="tl-year">Now</div><p>Rapid Node API parity; a default tool in many new JS projects.</p></div>
        </div>
      </div>
    </section>

    <!-- ========== 02 FEATURES ========== -->
    <section id="view-features" class="view">
      <div class="page-header">
        <h2>Features &amp; speed</h2>
        <p class="page-sub">Speaker 2 — the all-in-one toolchain, native APIs, package manager, tests, and what the benchmarks really show.</p>
      </div>

      <div class="grid g3 block">
        <div class="card"><span class="kicker">bun run</span><h3>Replaces node</h3><p>Runs JavaScript, TypeScript and JSX directly.</p></div>
        <div class="card"><span class="kicker">bun install</span><h3>Replaces npm install</h3><p>Pulls dependencies from npm and writes <code>node_modules</code>.</p></div>
        <div class="card"><span class="kicker">bun test</span><h3>Replaces jest / vitest</h3><p>Test runner with a Jest-compatible API.</p></div>
        <div class="card"><span class="kicker">bun build</span><h3>Replaces esbuild / webpack</h3><p>Bundles, minifies and tree-shakes to a single file.</p></div>
        <div class="card"><span class="kicker">bunx</span><h3>Replaces npx</h3><p>Runs a CLI package without installing it globally.</p></div>
        <div class="card"><span class="kicker">Same binary</span><h3>No version drift</h3><p>Same cache and same config across every tool.</p></div>
      </div>

      <div class="grid g2 block">
        <div class="card">
          <span class="kicker">Batteries included</span>
          <h3>Native APIs in the runtime</h3>
          <ul>
            <li><code>Bun.serve()</code> — a fast native HTTP server tuned in Zig, drop-in for <code>node:http</code>.</li>
            <li><code>bun:sqlite</code> — SQLite built in; no native compilation or <code>better-sqlite3</code> install pain.</li>
            <li><code>bun:websocket</code> — fast WebSocket client and server for realtime apps and dashboards.</li>
            <li><code>Bun.file()</code> — async file I/O backed by fast syscalls: read, write, stream out of the box.</li>
            <li>TypeScript, JSX and ESM run without a build step; top-level await works everywhere.</li>
          </ul>
        </div>
        <div class="card">
          <span class="kicker">Package manager</span>
          <h3><code>bun install</code> — the fastest tool in the box</h3>
          <p>~25× faster than <code>npm install</code> on typical projects, on the same hardware.</p>
          <ul>
            <li>Global content-addressable cache — download each package once across all projects.</li>
            <li>Parallel downloads and extraction, not one-at-a-time.</li>
            <li>Hardlinks into <code>node_modules</code> instead of copying files.</li>
            <li>Drop-in: reads <code>package.json</code>, writes a lockfile, works with npm scripts.</li>
          </ul>
          <div class="note warn">Speedup depends on network, project size and warm-cache state; figures come from bun.sh benchmarks.</div>
        </div>
      </div>

      <div class="grid g2 block">
        <div class="card">
          <h3><code>bun test</code> — run your tests</h3>
          <ul>
            <li>Jest-compatible: <code>describe</code>, <code>it</code>, <code>expect</code> — almost no rewrite.</li>
            <li>Watch mode, coverage reports and snapshots built in.</li>
            <li>TypeScript and JSX tests run directly, no precompile.</li>
            <li>Test files run in isolated parallel processes.</li>
          </ul>
        </div>
        <div class="card">
          <h3><code>bun build</code> — ship your code</h3>
          <ul>
            <li>Bundles to a single <code>.js</code>, <code>.css</code> or HTML file.</li>
            <li>Minification and tree-shaking built in.</li>
            <li>Generates sourcemaps; supports ESM and CommonJS output.</li>
            <li>Same performance class as esbuild, no separate install.</li>
          </ul>
        </div>
      </div>

      <div class="card block">
        <span class="kicker">Benchmarks</span>
        <h3>The three numbers people quote about Bun</h3>
        <div class="bench-tabs" id="benchTabs">
          <button data-b="cold" class="active">Cold start</button>
          <button data-b="install">Install speed</button>
          <button data-b="http">HTTP throughput</button>
        </div>
        <div id="benchContainer"></div>
        <div class="note">Synthetic benchmarks on controlled hardware. Treat them as order-of-magnitude, not guaranteed numbers.</div>
      </div>

      <div class="card block">
        <span class="kicker">Real world</span>
        <h3>In real apps the gap shrinks — but doesn't disappear</h3>
        <ul>
          <li><strong>Vercel, 2025:</strong> running Next.js on Bun cut latency by 28% — a solid win, not 10×.</li>
          <li><strong>JCSI peer-reviewed study, 2025:</strong> I/O-bound throughput was essentially tied — Bun 27.5k req/s vs Node 28.2k.</li>
        </ul>
        <p style="margin-top:8px"><strong>Why the gap shrinks:</strong> framework and application code dominate runtime; database and network I/O are the real bottlenecks; JS-level GC overhead cancels out some engine wins.</p>
        <p><strong>Where Bun still wins:</strong> developer experience (install, test, build speed), cold start for serverless / edge / CLI, and a smaller toolchain footprint.</p>
      </div>
    </section>

    <!-- ========== 03 COMPARISON ========== -->
    <section id="view-compare" class="view">
      <div class="page-header">
        <h2>Comparison &amp; compatibility</h2>
        <p class="page-sub">Speaker 3 — how Bun stacks up against Node.js and Deno, what works today, and who is adopting it.</p>
      </div>

      <div class="card block">
        <h3>Bun vs Node.js vs Deno — at a glance</h3>
        <table>
          <thead><tr><th>Aspect</th><th>Node.js</th><th>Deno</th><th>Bun</th></tr></thead>
          <tbody>
            <tr><td>Language</td><td>C++</td><td>Rust</td><td>Zig</td></tr>
            <tr><td>JS engine</td><td>V8</td><td>V8</td><td>JavaScriptCore</td></tr>
            <tr><td>Built-in package manager</td><td>npm</td><td>built-in</td><td>built-in, fast</td></tr>
            <tr><td>Test runner</td><td>external</td><td>built-in</td><td>built-in, Jest-like</td></tr>
            <tr><td>TypeScript</td><td>via tsc</td><td>native</td><td>native</td></tr>
            <tr><td>npm ecosystem compat</td><td>native</td><td>high</td><td>very high</td></tr>
          </tbody>
        </table>
      </div>

      <div class="grid g2 block">
        <div class="card">
          <h3>What works</h3>
          <ul>
            <li>Most <code>node:*</code> built-ins: <code>fs</code>, <code>path</code>, <code>http</code>, <code>crypto</code>, <code>stream</code>, <code>events</code>.</li>
            <li>Roughly 95%+ of npm packages install and run.</li>
            <li>ESM and CommonJS modules, import maps.</li>
            <li>Drop <code>bun --bun</code> in front of any npm script to force the Bun runtime.</li>
          </ul>
        </div>
        <div class="card">
          <h3>Where you might hit walls</h3>
          <ul>
            <li>Native C++ addons built against Node-specific headers.</li>
            <li>Very rare edge APIs that Node added recently.</li>
            <li>Tools that shell out to <code>node</code> internally.</li>
            <li>Workarounds: <code>bunfig.toml</code>, <code>--external</code>, or fall back to node for that script.</li>
          </ul>
        </div>
      </div>

      <div class="card block">
        <span class="kicker">Ecosystem</span>
        <h3>Who's actually using it?</h3>
        <ul>
          <li><strong>Vercel</strong> offers Bun as a runtime option on its Functions platform, alongside Node.</li>
          <li><strong>Cloudflare</strong> documents Bun compatibility for Workers and recommends it for local development.</li>
        </ul>
        <p style="margin-top:8px"><strong>Adoption pattern:</strong> most teams adopt Bun first as a development tool — <code>bun install</code>, <code>bun test</code>, local dev server — before switching their production runtime. Joining Anthropic in 2025 signals long-term maintenance and a stable roadmap.</p>
      </div>
    </section>

    <!-- ========== 04 CHEAT SHEET ========== -->
    <section id="view-cheatsheet" class="view">
      <div class="page-header">
        <h2>One-page Bun cheat sheet</h2>
        <p class="page-sub">Essential CLI commands and built-in APIs, mapped to the Node ecosystem tool they replace.</p>
      </div>
      <div class="card">
        <table>
          <thead><tr><th>Command / API</th><th>Replaces</th><th>Purpose &amp; notes</th></tr></thead>
          <tbody>
            <tr><td><code>bun run index.ts</code></td><td><code>node</code> + <code>ts-node</code></td><td>Executes TypeScript or JavaScript directly, zero config.</td></tr>
            <tr><td><code>bun --hot server.ts</code></td><td><code>nodemon</code></td><td>Hot-reloads in place without dropping active HTTP/WS connections.</td></tr>
            <tr><td><code>bun install</code> / <code>bun add pkg</code></td><td><code>npm install</code></td><td>Installs dependencies via global cache and system-level file cloning.</td></tr>
            <tr><td><code>bunx vite</code></td><td><code>npx vite</code></td><td>Executes a local or remote CLI package with fast startup.</td></tr>
            <tr><td><code>bun test</code></td><td><code>jest</code> / <code>vitest</code></td><td>Runs <code>expect()</code> unit tests natively with snapshot support.</td></tr>
            <tr><td><code>Bun.serve({ fetch })</code></td><td><code>express</code> / <code>node:http</code></td><td>High-throughput Web Standard <code>Request</code>/<code>Response</code> server.</td></tr>
            <tr><td><code>import { Database } from "bun:sqlite"</code></td><td><code>better-sqlite3</code></td><td>Synchronous, zero-dependency embedded SQLite in the binary.</td></tr>
          </tbody>
        </table>
      </div>
    </section>

    <!-- ========== 05 TUTORIAL ========== -->
    <section id="view-tutorial" class="view">
      <div class="page-header">
        <h2>Hands-on tutorial: zero-config TypeScript API (~30 min)</h2>
        <p class="page-sub">Four steps to install Bun, build an HTTP + SQLite API, and run an automated test.</p>
      </div>

      <div class="grid g2 block">
        <div class="card">
          <span class="tag">Step 01 · Install &amp; init</span>
          <h3>Scaffold a new project</h3>
<pre><span class="cm"># Install Bun (macOS / Linux / WSL)</span>
curl -fsSL https://bun.sh/install | bash

<span class="cm"># Windows PowerShell</span>
<span class="cm"># irm bun.sh/install.ps1 | iex</span>

mkdir bun-api && cd bun-api
bun init -y</pre>
        </div>
        <div class="card">
          <span class="tag">Step 02 · HTTP + SQLite</span>
          <h3>Create <code>index.ts</code></h3>
<pre><span class="kw">import</span> { Database } <span class="kw">from</span> "bun:sqlite";

<span class="kw">const</span> db = <span class="kw">new</span> Database(":memory:");
db.run("CREATE TABLE notes (id INTEGER PRIMARY KEY, text TEXT)");
db.run("INSERT INTO notes (text) VALUES ('Hello from Bun!')");

Bun.serve({
  port: 3000,
  fetch(req) {
    const rows = db.query("SELECT * FROM notes").all();
    <span class="kw">return</span> Response.json({ runtime: "Bun", data: rows });
  },
});</pre>
        </div>
        <div class="card">
          <span class="tag">Step 03 · Run with hot reload</span>
          <h3>Start the server</h3>
<pre><span class="cm"># Run TypeScript directly, no compile step</span>
bun --hot index.ts

<span class="cm"># Test the endpoint</span>
curl http://localhost:3000</pre>
          <div class="note">Expected: <code>{"runtime":"Bun","data":[{"id":1,"text":"Hello from Bun!"}]}</code></div>
        </div>
        <div class="card">
          <span class="tag">Step 04 · Built-in test suite</span>
          <h3>Write <code>math.test.ts</code></h3>
<pre><span class="kw">import</span> { expect, test } <span class="kw">from</span> "bun:test";

test("Bun native test runner", () => {
  expect(2 + 2).toBe(4);
});

<span class="cm"># Run from the terminal: bun test</span></pre>
        </div>
      </div>

      <div class="card">
        <h3>Troubleshooting</h3>
        <ul>
          <li><strong>Command not found after install:</strong> reopen the terminal, or add the Bun binary path to your shell profile (<code>~/.bashrc</code> / <code>~/.zshrc</code>).</li>
          <li><strong>Native module fails on Windows:</strong> use WSL; some C/C++ addons do not support native Windows under Bun.</li>
          <li><strong>Port 3000 in use:</strong> change the <code>port</code> in <code>Bun.serve()</code> or stop the other process.</li>
        </ul>
      </div>
    </section>

    <!-- ========== 06 VERDICT ========== -->
    <section id="view-verdict" class="view">
      <div class="page-header">
        <h2>The verdict</h2>
        <p class="page-sub">Speaker 4 — it's not a silver bullet. Know the rough edges, migrate incrementally, and benchmark your own workload.</p>
      </div>

      <div class="card block">
        <span class="kicker">Limitations</span>
        <h3>It's not a silver bullet — know the rough edges</h3>
        <ul>
          <li><strong>Young project</strong> — v1.0 only shipped in September 2023; the API surface still moves.</li>
          <li><strong>Not 100% Node-API compatible</strong> — niche native modules and very new Node APIs may break.</li>
          <li><strong>Smaller team</strong> — even with Anthropic, the core team is tiny compared to Node's company and ecosystem.</li>
          <li><strong>Performance gap shrinks in real apps</strong> — framework and database time dominate.</li>
          <li><strong>Debugging story is younger</strong> — DevTools, profiling and observability are catching up to Node.</li>
        </ul>
      </div>

      <div class="grid g2 block">
        <div class="card">
          <h3>Two things people underestimate</h3>
          <ul>
            <li><strong>Windows support matured late:</strong> Bun ran on macOS and Linux first. If your team develops on Windows, test early — some scripts and native deps still assume a POSIX environment.</li>
            <li><strong>Migrate incrementally, don't rewrite:</strong> start with <code>bun install</code> and <code>bun test</code> locally, move one service at a time, and keep Node as a fallback in CI.</li>
          </ul>
        </div>
        <div class="card">
          <span class="kicker">Takeaways</span>
          <h3>Three things to remember</h3>
          <ul>
            <li><strong>1.</strong> Bun is one toolchain, not just a faster Node — runtime, package manager, test runner, bundler and TypeScript in a single Zig binary.</li>
            <li><strong>2.</strong> The biggest wins are developer experience and cold start — not raw request throughput in real apps.</li>
            <li><strong>3.</strong> It's production-ready for new projects; migrate incrementally, keep Node as fallback, and benchmark your own workload.</li>
          </ul>
        </div>
      </div>

      <div class="verdict block">
        <div class="v-card go">
          <h3>Choose Bun when…</h3>
          <ul>
            <li>You're starting a <strong>new project</strong> and want a fast, modern default.</li>
            <li>You run <strong>APIs, WebSockets or realtime services</strong> where cold start matters.</li>
            <li>You deploy to <strong>serverless or edge functions</strong> — 5–10 ms startup is a real advantage.</li>
            <li>You want a <strong>faster local dev loop</strong> — installs, tests and builds feel snappier.</li>
            <li>You'd rather install one tool than configure five.</li>
          </ul>
        </div>
        <div class="v-card stay">
          <h3>…stick with Node.js when</h3>
          <ul>
            <li>Your product relies on <strong>native C++ addons</strong> that are hard to rebuild.</li>
            <li>You run <strong>mission-critical legacy systems</strong> where a runtime bug would cost real money.</li>
            <li>Your team lacks bandwidth to debug young-runtime edge cases.</li>
            <li>You need <strong>long-term LTS guarantees</strong> only the Node foundation can offer.</li>
            <li>Your app is I/O-bound and already fast — the runtime advantage won't move the needle.</li>
          </ul>
        </div>
      </div>
    </section>

    <!-- ========== 07 REFERENCES ========== -->
    <section id="view-refs" class="view">
      <div class="page-header">
        <h2>References &amp; questions</h2>
        <p class="page-sub">Further reading and sources used in this presentation.</p>
      </div>
      <div class="card">
        <ol class="refs">
          <li><strong>Bun Documentation.</strong> <em>Bun — a fast JavaScript runtime, package manager, bundler and test runner.</em> Available at: https://bun.sh (Accessed: [date]).</li>
          <li><strong>Bun GitHub Repository.</strong> Available at: https://github.com/oven-sh/bun (Accessed: [date]).</li>
          <li><strong>Vercel (2025).</strong> <em>Bun runtime on Vercel Functions.</em> Available at: https://vercel.com (Accessed: [date]).</li>
          <li><strong>DrCodes (2025).</strong> <em>Bun vs Node vs Deno 2025 Benchmark Guide.</em></li>
          <li><strong>Strapi (2026).</strong> <em>Bun vs Node.js: Performance &amp; Migration 2026.</em></li>
          <li><strong>Anthropic (2025).</strong> Announcement: Bun joins Anthropic.</li>
          <li><strong>JCSI (2025).</strong> Peer-reviewed study: I/O-bound throughput comparison of Bun and Node.js.</li>
        </ol>
        <div class="note">Questions on compatibility, migration, benchmarks, or anything else are welcome in the CW1 Q&amp;A session.</div>
      </div>
    </section>

  </main>
</div>

<footer>
  COMP10020 Internet Technologies · CW1 Group Resource Page · Built with semantic HTML5, vanilla CSS &amp; JS · No frameworks or build step required.
</footer>

<script>
  /* ===== View switching ===== */
  const views = document.querySelectorAll('.view');
  const navBtns = document.querySelectorAll('#navList button');
  function go(name){
    views.forEach(v => v.classList.remove('active'));
    navBtns.forEach(b => b.classList.remove('active'));
    const el = document.getElementById('view-' + name);
    const btn = document.querySelector('#navList button[data-view="' + name + '"]');
    if(el) el.classList.add('active');
    if(btn) btn.classList.add('active');
    window.scrollTo({top:0,behavior:'smooth'});
  }
  navBtns.forEach(b => b.addEventListener('click', () => go(b.dataset.view)));

  /* ===== Scroll progress ===== */
  const prog = document.getElementById('progress');
  addEventListener('scroll', () => {
    const h = document.documentElement;
    prog.style.width = (h.scrollTop / (h.scrollHeight - h.clientHeight) * 100) + '%';
  });

  /* ===== Hero typing effect ===== */
  const lines = [
    'bun --hot index.ts',
    '✓ listening on http://localhost:3000',
    'bun test',
    '✓ 1 passed · 0 failed'
  ];
  const typedEl = document.getElementById('typed');
  const outEl = document.getElementById('termOut');
  let li = 0, ci = 0, deleting = false;
  function typeTick(){
    const line = lines[li];
    if(!deleting){
      ci++;
      typedEl.textContent = line.slice(0, ci);
      if(ci === line.length){
        deleting = true;
        setTimeout(typeTick, 1300);
        return;
      }
      setTimeout(typeTick, 55);
    } else {
      ci--;
      typedEl.textContent = line.slice(0, ci);
      if(ci === 0){
        deleting = false;
        if(line.startsWith('bun')) outEl.textContent = '';
        li = (li + 1) % lines.length;
        setTimeout(typeTick, 300);
        return;
      }
      setTimeout(typeTick, 22);
    }
  }
  typeTick();

  /* ===== Toolchain merge animation ===== */
  function mergeTools(){
    const card = document.getElementById('mergeCard');
    const chip = document.getElementById('bunChip');
    card.classList.add('merged');
    chip.style.display = 'inline-flex';
  }

  /* ===== Benchmark bars ===== */
  const benchmarks = {
    cold:{
      title:'Cold start time (lower is better)',
      items:[
        {name:'Bun',val:'5–10 ms',pct:12,hot:true},
        {name:'Node.js',val:'50–100 ms',pct:100,hot:false}
      ]
    },
    install:{
      title:'Cold dependency install (lower is better)',
      items:[
        {name:'bun install',val:'~0.9s · ~25× faster',pct:12,hot:true},
        {name:'npm install',val:'~18.2s',pct:100,hot:false}
      ]
    },
    http:{
      title:'Hello-world throughput (requests/sec — higher is better)',
      items:[
        {name:'Bun (Bun.serve)',val:'~180k req/s',pct:100,hot:true},
        {name:'Node.js (node:http)',val:'~65k req/s',pct:36,hot:false}
      ]
    }
  };
  function renderBench(key){
    const data = benchmarks[key];
    const box = document.getElementById('benchContainer');
    let html = '<h3 style="margin-bottom:14px">' + data.title + '</h3>';
    data.items.forEach(it => {
      html += `<div class="bar-row">
        <div class="bar-label"><span>${it.name}</span><span class="v">${it.val}</span></div>
        <div class="bar-track"><div class="bar-fill ${it.hot?'hot':'cool'}" data-w="${it.pct}"></div></div>
      </div>`;
    });
    box.innerHTML = html;
    requestAnimationFrame(() => {
      box.querySelectorAll('.bar-fill').forEach(f => f.style.width = f.dataset.w + '%');
    });
  }
  document.querySelectorAll('#benchTabs button').forEach(btn => {
    btn.addEventListener('click', () => {
      document.querySelectorAll('#benchTabs button').forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      renderBench(btn.dataset.b);
    });
  });
  renderBench('cold');
</script>
</body>
</html>
<!-- （注：内容由AI生成） -->
