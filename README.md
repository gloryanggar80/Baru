<html style="margin:0;padding:0;">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@500;700;900&family=Rajdhani:wght@400;500;600;700&display=swap" rel="stylesheet">
<script src="https://cdn.tailwindcss.com"></script>
<script src="https://cdn.jsdelivr.net/npm/echarts@5/dist/echarts.min.js"></script>
<script>
(function(){
  var s=document.createElement('style');
  s.textContent=`
  :root{--bg:#03050a;--card:#070c1c;--text:#e8f1ff;--muted:#7b8bbf;--accent:#00f0ff;--accent2:#c24bff;--accent3:#00ffa8;--accent4:#ff3d8a;--border:rgba(0,240,255,.22)}
  *{box-sizing:border-box;-webkit-tap-highlight-color:transparent}
  html,body{margin:0;padding:0;background:var(--bg);color:var(--text);font-family:'Rajdhani',sans-serif;overflow-x:hidden}
  body{background:
    radial-gradient(1200px 600px at 10% -10%, rgba(0,240,255,.18), transparent 60%),
    radial-gradient(900px 500px at 110% 10%, rgba(194,75,255,.18), transparent 60%),
    radial-gradient(700px 700px at 50% 120%, rgba(0,255,168,.12), transparent 60%),
    var(--bg);
    background-attachment:fixed;
  }
  .font-tech{font-family:'Orbitron',sans-serif;letter-spacing:.08em}
  .num{font-variant-numeric:tabular-nums}

  /* === PS4 XMB TOP NAV === */
  .xmb{
    position:sticky;top:0;z-index:100;
    padding:10px 10px 12px;
    background:linear-gradient(180deg, rgba(3,5,10,.96) 60%, rgba(3,5,10,0));
    backdrop-filter:blur(18px);
    border-bottom:1px solid var(--border);
  }
  .xmb-track{
    display:flex;gap:10px;overflow-x:auto;scroll-snap-type:x mandatory;
    padding:6px 4px 10px;
    -webkit-overflow-scrolling:touch;
    scrollbar-width:none;
  }
  .xmb-track::-webkit-scrollbar{display:none}
  .xmb-item{
    scroll-snap-align:center;flex:0 0 auto;
    min-width:110px;padding:12px 10px;border-radius:16px;
    border:1px solid transparent;
    background:linear-gradient(180deg, rgba(255,255,255,.03), rgba(255,255,255,.01));
    text-align:center;cursor:pointer;
    transition:all .45s cubic-bezier(.2,.9,.2,1.3);
    transform-origin:center bottom;
  }
  .xmb-item .ic{font-size:26px;line-height:1;filter:drop-shadow(0 0 6px rgba(0,240,255,.4));transition:all .4s}
  .xmb-item .lb{font-family:'Orbitron';font-size:10px;letter-spacing:.12em;margin-top:6px;color:var(--muted);text-transform:uppercase;font-weight:700;transition:all .4s}
  .xmb-item.active{
    transform:translateY(-4px) scale(1.18);
    border-color:var(--accent);
    background:linear-gradient(180deg, rgba(0,240,255,.18), rgba(194,75,255,.12));
    box-shadow:
      0 12px 40px rgba(0,240,255,.35),
      0 0 0 1px rgba(0,240,255,.5),
      inset 0 0 24px rgba(0,240,255,.25);
  }
  .xmb-item.active .ic{filter:drop-shadow(0 0 14px var(--accent))}
  .xmb-item.active .lb{color:#fff;text-shadow:0 0 10px var(--accent)}
  .xmb-indicator{height:3px;background:linear-gradient(90deg,var(--accent),var(--accent2),var(--accent3));border-radius:3px;margin:0 4px;transition:all .5s;box-shadow:0 0 14px var(--accent)}

  /* === INTRO === */
  @keyframes introFade{0%{opacity:0;transform:scale(.88)}25%{opacity:1;transform:scale(1)}85%{opacity:1}100%{opacity:0;transform:scale(1.12)}}
  @keyframes blink{0%,49%{opacity:1}50%,100%{opacity:0}}
  @keyframes scan{0%{transform:translateY(-120%)}100%{transform:translateY(260%)}}
  @keyframes floatY{0%,100%{transform:translateY(0)}50%{transform:translateY(-10px)}}
  @keyframes neonPulse{0%,100%{box-shadow:0 0 24px rgba(0,240,255,.35),inset 0 0 24px rgba(194,75,255,.15)}50%{box-shadow:0 0 60px rgba(0,240,255,.7),inset 0 0 34px rgba(194,75,255,.3)}}
  .intro{position:fixed;inset:0;z-index:9999;display:flex;align-items:center;justify-content:center;background:radial-gradient(ellipse at center,#06102b 0%,#000 75%);animation:introFade 4.2s ease forwards}
  .intro .scanline{position:absolute;inset:auto 0 auto 0;height:140px;background:linear-gradient(180deg,transparent,rgba(0,240,255,.18),transparent);animation:scan 2.4s linear infinite;pointer-events:none}
  .typer{font-size:48px;font-weight:900;background:linear-gradient(90deg,var(--accent),var(--accent2),var(--accent3),var(--accent4));background-size:300% 100%;-webkit-background-clip:text;background-clip:text;color:transparent;animation:hueShift 6s linear infinite;text-shadow:0 0 40px rgba(0,240,255,.4)}
  @keyframes hueShift{0%{background-position:0% 50%}100%{background-position:300% 50%}}
  .caret{display:inline-block;width:3px;height:46px;background:var(--accent);margin-left:4px;vertical-align:middle;animation:blink .9s steps(1) infinite;box-shadow:0 0 12px var(--accent)}

  /* === CARDS === */
  .card4d{
    position:relative;border-radius:20px;overflow:hidden;
    border:1px solid var(--border);
    background:linear-gradient(160deg, rgba(10,16,38,.92), rgba(5,8,20,.72));
    backdrop-filter:blur(14px);
    box-shadow:
      0 20px 60px rgba(0,0,0,.55),
      0 2px 0 rgba(255,255,255,.04) inset,
      0 -2px 20px rgba(0,240,255,.06) inset;
    transition:transform .5s cubic-bezier(.2,.9,.2,1.3), box-shadow .5s;
    transform-style:preserve-3d;
  }
  .card4d::before{
    content:"";position:absolute;inset:-2px;border-radius:22px;padding:2px;
    background:linear-gradient(135deg,transparent 30%,rgba(0,240,255,.55) 50%,transparent 70%);
    -webkit-mask:linear-gradient(#000 0 0) content-box, linear-gradient(#000 0 0);
    -webkit-mask-composite:xor;mask-composite:exclude;
    transform:translateX(-120%);transition:transform 1.2s;pointer-events:none;
  }
  .card4d:hover::before,.card4d:active::before{transform:translateX(120%)}
  .card4d:hover{transform:translateY(-6px) rotateX(1deg);box-shadow:0 30px 80px rgba(0,240,255,.25)}
  .kpi{animation:neonPulse 3.6s ease-in-out infinite}
  .chip{display:inline-block;padding:3px 10px;border-radius:999px;font-size:10px;font-weight:800;letter-spacing:.14em;text-transform:uppercase}
  .inp{width:100%;background:rgba(255,255,255,.04);border:1px solid var(--border);border-radius:12px;padding:11px 13px;color:var(--text);font-size:15px;outline:none;transition:all .25s}
  .inp:focus{border-color:var(--accent);background:rgba(0,240,255,.08);box-shadow:0 0 0 3px rgba(0,240,255,.18),0 0 24px rgba(0,240,255,.25)}
  .btn{padding:11px 14px;border-radius:14px;font-weight:800;letter-spacing:.06em;border:none;cursor:pointer;transition:all .25s;font-family:'Rajdhani'}
  .btn-primary{background:linear-gradient(135deg,var(--accent),var(--accent2));color:#03101a;box-shadow:0 10px 30px rgba(0,240,255,.35)}
  .btn-primary:active{transform:scale(.96)}
  .btn-ghost{background:transparent;color:var(--text);border:1px solid var(--border)}
  .btn-ghost:active{background:rgba(0,240,255,.08)}
  .btn-lg{padding:16px;font-size:16px;border-radius:18px}
  .row{display:flex;align-items:center;gap:10px}
  .divider{height:1px;background:linear-gradient(90deg,transparent,var(--border),transparent);margin:10px 0}
  .hide{display:none !important}
  .branch-row,.trader-row{padding:13px;border-bottom:1px solid var(--border);cursor:pointer;transition:background .2s;display:flex;align-items:center;gap:12px}
  .branch-row:active,.trader-row:active{background:rgba(0,240,255,.1)}
  .avatar{width:42px;height:42px;border-radius:50%;background:linear-gradient(135deg,var(--accent2),var(--accent));display:flex;align-items:center;justify-content:center;font-weight:900;color:#fff;flex-shrink:0;box-shadow:0 0 16px rgba(194,75,255,.4)}
  ::-webkit-scrollbar{width:6px;height:6px}
  ::-webkit-scrollbar-thumb{background:rgba(0,240,255,.35);border-radius:3px}

  /* === AI LEVEL === */
  .lv-bar{height:10px;border-radius:999px;background:rgba(255,255,255,.06);overflow:hidden;border:1px solid var(--border)}
  .lv-fill{height:100%;background:linear-gradient(90deg,var(--accent3),var(--accent),var(--accent2),var(--accent4));background-size:300% 100%;animation:hueShift 4s linear infinite;box-shadow:0 0 16px var(--accent);transition:width .8s cubic-bezier(.2,.9,.2,1.3)}
  .lv-ring{width:110px;height:110px;border-radius:50%;background:conic-gradient(var(--accent) calc(var(--p)*1%), rgba(255,255,255,.06) 0);display:flex;align-items:center;justify-content:center;box-shadow:0 0 40px rgba(0,240,255,.4), inset 0 0 24px rgba(0,0,0,.6)}
  .lv-ring::before{content:"";position:absolute;width:90px;height:90px;border-radius:50%;background:var(--bg)}

  /* === 4D CHART BASE === */
  .chart4d{position:relative;border-radius:18px;overflow:hidden}
  .chart4d::after{
    content:"";position:absolute;inset:0;pointer-events:none;
    background:
      radial-gradient(600px 200px at 20% 0%, rgba(0,240,255,.18), transparent 60%),
      radial-gradient(500px 200px at 80% 100%, rgba(194,75,255,.18), transparent 60%);
    mix-blend-mode:screen;
  }
  .particles{position:absolute;inset:0;pointer-events:none;opacity:.6}
  `;
  document.head.appendChild(s);
})();
</script>
<div>

<!-- ============ INTRO ============ -->
<div id="intro" class="intro">
  <div class="scanline"></div>
  <div style="text-align:center">
    <div class="font-tech" style="color:var(--muted);font-size:11px;letter-spacing:.45em;margin-bottom:16px">⚡ ODYSSEUS AI CORE v4.2 — BOOTING</div>
    <div class="typer font-tech" id="typer"></div><span class="caret"></span>
    <div class="font-tech" style="color:var(--accent3);font-size:11px;letter-spacing:.4em;margin-top:20px;opacity:.85">NEURAL LINK · ESTABLISHED · LV <span id="lvIntro">01</span></div>
  </div>
</div>

<!-- ============ APP ============ -->
<div id="app" class="hide pb-28">

  <!-- PS4 XMB NAV (ATAS, BISA DIGESER) -->
  <nav class="xmb">
    <div class="xmb-track" id="xmb">
      <div class="xmb-item active" data-p="dash"><div class="ic">💻</div><div class="lb">DASH</div></div>
      <div class="xmb-item" data-p="gudang"><div class="ic">🏭</div><div class="lb">GUDANG</div></div>
      <div class="xmb-item" data-p="pedagang"><div class="ic">🗿</div><div class="lb">PEDAGANG</div></div>
      <div class="xmb-item" data-p="distribusi"><div class="ic">🛫</div><div class="lb">KIRIM</div></div>
      <div class="xmb-item" data-p="keuangan"><div class="ic">🪙</div><div class="lb">KEUANGAN</div></div>
      <div class="xmb-item" data-p="pembukuan"><div class="ic">📒</div><div class="lb">BUKU</div></div>
      <div class="xmb-item" data-p="setting"><div class="ic">⚙️</div><div class="lb">SETTING</div></div>
      <div class="xmb-item" data-p="ai"><div class="ic">🤖</div><div class="lb">AI LV</div></div>
    </div>
    <div class="xmb-indicator" id="xmbBar" style="width:90px;margin-left:14px"></div>
  </nav>

  <!-- CLOCK + AI LV STRIP -->
  <div class="px-4 pt-3 pb-1 row justify-between">
    <div class="chip" style="background:rgba(0,255,168,.12);color:var(--accent3)">🤖 AI LV <b id="aiLvTop">01</b></div>
    <div id="clock" class="font-tech num" style="color:var(--accent);font-size:13px;letter-spacing:.15em"></div>
    <div class="chip" style="background:rgba(194,75,255,.12);color:var(--accent2)">EXP <b id="aiExpTop">0</b>/100</div>
  </div>

  <!-- ============ DASHBOARD 4D ============ -->
  <section id="page-dash" class="page px-4 pt-3 space-y-4">
    <div class="grid grid-cols-2 gap-3">
      <div class="card4d kpi p-4"><div class="chip" style="background:rgba(0,240,255,.12);color:var(--accent)">OMSET</div><div id="kpiOmset" class="font-tech num" style="font-size:26px;font-weight:900;margin-top:6px">0</div></div>
      <div class="card4d kpi p-4"><div class="chip" style="background:rgba(194,75,255,.12);color:var(--accent2)">DISTRIBUSI</div><div id="kpiDist" class="font-tech num" style="font-size:26px;font-weight:900;margin-top:6px">0</div></div>
      <div class="card4d kpi p-4"><div class="chip" style="background:rgba(0,255,168,.12);color:var(--accent3)">STOK SIAP</div><div id="kpiStok" class="font-tech num" style="font-size:26px;font-weight:900;margin-top:6px">0</div></div>
      <div class="card4d kpi p-4"><div class="chip" style="background:rgba(255,61,138,.12);color:var(--accent4)">UNTUNG</div><div id="kpiUntung" class="font-tech num" style="font-size:26px;font-weight:900;margin-top:6px">0</div></div>
    </div>
    <div class="card4d p-4"><div class="font-tech mb-2" style="color:var(--accent);font-size:12px;letter-spacing:.18em">📈 TREND OMSET 7 HARI · 4D LIVE</div><div class="chart4d"><canvas class="particles" id="pLine"></canvas><div id="cLine" style="height:280px"></div></div></div>
    <div class="card4d p-4"><div class="font-tech mb-2" style="color:var(--accent2);font-size:12px;letter-spacing:.18em">🕸️ KINERJA CABANG · RADAR 4D</div><div class="chart4d"><div id="cRadar" style="height:300px"></div></div></div>
    <div class="card4d p-4"><div class="font-tech mb-2" style="color:var(--accent3);font-size:12px;letter-spacing:.18em">🏆 PENJUALAN PER CABANG · 3D BAR</div><div class="chart4d"><div id="cBar" style="height:300px"></div></div></div>
    <div class="card4d p-4"><div class="font-tech mb-2" style="color:var(--accent4);font-size:12px;letter-spacing:.18em">🍩 DISTRIBUSI PRODUK · DONAT 4D</div><div class="chart4d"><div id="cDonut" style="height:280px"></div></div></div>
    <div class="card4d p-4"><div class="font-tech mb-3" style="color:var(--accent);font-size:12px;letter-spacing:.18em">🏅 RANKING CABANG</div><div id="rankCabang"></div></div>
    <div class="card4d p-4"><div class="font-tech mb-3" style="color:var(--accent2);font-size:12px;letter-spacing:.18em">⭐ RANKING PEDAGANG TOP 8</div><div id="rankPedagang"></div></div>
    <div class="card4d p-4"><div class="font-tech mb-3" style="color:var(--accent3);font-size:12px;letter-spacing:.18em">🥟 PRODUK TERJUAL</div><div id="rankProduk"></div></div>
    <div class="card4d p-4"><div class="font-tech mb-3" style="color:var(--accent4);font-size:12px;letter-spacing:.18em">📊 TABEL DATA PEDAGANG</div><div style="overflow-x:auto"><table class="w-full text-sm"><thead><tr style="color:var(--muted)"><th class="text-left py-2">NAMA</th><th class="text-right">TAHU</th><th class="text-right">SOTONG</th><th class="text-right">SETORAN</th></tr></thead><tbody id="tData"></tbody></table></div></div>
  </section>

  <!-- ============ GUDANG ============ -->
  <section id="page-gudang" class="page hide px-4 pt-3 space-y-4">
    <div class="card4d p-4">
      <div class="font-tech mb-3" style="color:var(--accent);font-size:13px;letter-spacing:.18em">📦 STOK BARU HARI INI</div>
      <div class="grid grid-cols-2 gap-3"><div><label class="chip" style="background:rgba(0,240,255,.1);color:var(--accent)">TAHU</label><input class="inp mt-2 num" id="sbTahu" type="number" oninput="saveGudang();addExp(2)"></div>
      <div><label class="chip" style="background:rgba(194,75,255,.1);color:var(--accent2)">SOTONG</label><input class="inp mt-2 num" id="sbSotong" type="number" oninput="saveGudang();addExp(2)"></div></div>
    </div>
    <div class="card4d p-4">
      <div class="font-tech mb-3" style="color:#ff9a3c;font-size:13px;letter-spacing:.18em">🔄 STOK RIJEK · AUTO PINDAH 00:00</div>
      <div class="grid grid-cols-2 gap-3"><div><label class="chip" style="background:rgba(255,154,60,.1);color:#ff9a3c">TAHU</label><input class="inp mt-2 num" id="srTahu" type="number" oninput="saveGudang();addExp(1)"></div>
      <div><label class="chip" style="background:rgba(255,61,138,.1);color:var(--accent4)">SOTONG</label><input class="inp mt-2 num" id="srSotong" type="number" oninput="saveGudang();addExp(1)"></div></div>
    </div>
    <div class="card4d kpi p-4">
      <div class="font-tech mb-3" style="color:var(--accent3);font-size:13px;letter-spacing:.18em">✅ TOTAL SIAP DISTRIBUSI</div>
      <div class="grid grid-cols-2 gap-3">
        <div class="p-3 rounded-xl" style="background:rgba(0,255,168,.08);border:1px solid rgba(0,255,168,.25)"><div style="color:var(--muted);font-size:12px">TAHU</div><div id="totTahu" class="font-tech num" style="font-size:28px;font-weight:900;color:var(--accent3)">0</div></div>
        <div class="p-3 rounded-xl" style="background:rgba(0,240,255,.08);border:1px solid rgba(0,240,255,.25)"><div style="color:var(--muted);font-size:12px">SOTONG</div><div id="totSotong" class="font-tech num" style="font-size:28px;font-weight:900;color:var(--accent)">0</div></div>
      </div>
    </div>
    <div class="card4d p-4">
      <div class="font-tech mb-3" style="color:var(--accent4);font-size:13px;letter-spacing:.18em">🌶️ STOK BUMBU GUDANG</div>
      <div class="grid grid-cols-2 gap-3"><div><label class="chip" style="background:rgba(0,240,255,.1);color:var(--accent)">ATOM</label><input class="inp mt-2 num" id="bAtom" type="number" oninput="saveGudang();addExp(1)"></div>
      <div><label class="chip" style="background:rgba(255,61,138,.1);color:var(--accent4)">BALADO</label><input class="inp mt-2 num" id="bBalado" type="number" oninput="saveGudang();addExp(1)"></div></div>
    </div>
    <div class="card4d p-4">
      <div class="font-tech mb-3" style="color:var(--accent2);font-size:13px;letter-spacing:.18em">📤 BUMBU TERDISTRIBUSI · AUTO KURANG GUDANG</div>
      <div class="grid grid-cols-2 gap-3"><div><label class="chip" style="background:rgba(0,240,255,.1);color:var(--accent)">ATOM</label><input class="inp mt-2 num" id="dAtom" type="number" oninput="saveGudang();calcGudang();addExp(2)"></div>
      <div><label class="chip" style="background:rgba(255,61,138,.1);color:var(--accent4)">BALADO</label><input class="inp mt-2 num" id="dBalado" type="number" oninput="saveGudang();calcGudang();addExp(2)"></div></div>
    </div>
    <div class="card4d p-4">
      <div class="font-tech mb-3" style="color:var(--accent3);font-size:13px;letter-spacing:.18em">💰 NOMINAL BUMBU × 12.500</div>
      <div class="grid grid-cols-2 gap-3">
        <div class="p-3 rounded-xl" style="background:rgba(0,240,255,.06)"><div style="color:var(--muted);font-size:12px">ATOM</div><div id="nomAtom" class="font-tech num" style="font-size:20px;font-weight:900;color:var(--accent)">Rp 0</div></div>
        <div class="p-3 rounded-xl" style="background:rgba(255,61,138,.06)"><div style="color:var(--muted);font-size:12px">BALADO</div><div id="nomBalado" class="font-tech num" style="font-size:20px;font-weight:900;color:var(--accent4)">Rp 0</div></div>
      </div>
    </div>
  </section>

  <!-- ============ PEDAGANG ============ -->
  <section id="page-pedagang" class="page hide px-4 pt-3 space-y-4">
    <div class="grid grid-cols-2 gap-2"><button class="btn btn-primary" onclick="tambahPedagang()">＋ TAMBAH</button><button class="btn btn-ghost" onclick="hapusPedagang()">🗑 HAPUS</button></div>
    <div id="listCabang"></div>
    <div id="detailCabang" class="hide card4d p-4"></div>
    <div id="detailPedagang" class="hide card4d p-4 space-y-3"></div>
  </section>

  <!-- ============ DISTRIBUSI ============ -->
  <section id="page-distribusi" class="page hide px-4 pt-3 space-y-4"><div id="distList"></div></section>

  <!-- ============ KEUANGAN ============ -->
  <section id="page-keuangan" class="page hide px-4 pt-3 space-y-4">
    <div class="grid grid-cols-4 gap-2">
      <button class="btn btn-primary" onclick="keuTab('asuransi')">🛡️</button>
      <button class="btn btn-ghost" onclick="keuTab('pengeluaran')">🆘</button>
      <button class="btn btn-ghost" onclick="keuTab('kasbon')">💢</button>
      <button class="btn btn-ghost" onclick="keuTab('hutang')">💀</button>
    </div>
    <div id="keuPanel"></div>
  </section>

  <!-- ============ PEMBUKUAN ============ -->
  <section id="page-pembukuan" class="page hide px-4 pt-3 space-y-4">
    <div class="card4d kpi p-5 text-center">
      <div class="font-tech" style="color:var(--accent3);font-size:11px;letter-spacing:.25em">HASIL PEMBUKUAN × 230</div>
      <div id="hasilBuku" class="font-tech num" style="font-size:34px;font-weight:900;margin-top:6px;background:linear-gradient(90deg,var(--accent),var(--accent3),var(--accent2));-webkit-background-clip:text;background-clip:text;color:transparent">Rp 0</div>
    </div>
    <div class="card4d p-4"><div class="font-tech mb-2" style="color:var(--accent);font-size:12px;letter-spacing:.18em">📦 STOK BARU (KAMIS - RABU)</div><div id="bStokBaru"></div></div>
    <div class="card4d p-4"><div class="font-tech mb-2" style="color:var(--accent2);font-size:12px;letter-spacing:.18em">💥 TOTAL BS (SABTU - JUMAT)</div><div id="bBS"></div></div>
    <div class="card4d p-4 space-y-2">
      <div class="font-tech" style="color:#ff9a3c;font-size:12px;letter-spacing:.18em">⚙️ VARIABEL SIKLUS</div>
      <div class="row justify-between"><span style="color:var(--muted)">Sisa Kemarin</span><input class="inp num" id="sisaKemarin" style="width:150px;text-align:right" type="number" oninput="saveBuku();addExp(2)"></div>
      <div class="row justify-between"><span style="color:var(--muted)">Sisa Sekarang</span><input class="inp num" id="sisaSekarang" style="width:150px;text-align:right" type="number" oninput="saveBuku();addExp(2)"></div>
      <div class="row justify-between"><span style="color:var(--muted)">Total Asuransi</span><div id="totAsuransi" class="font-tech num">0</div></div>
      <div class="row justify-between"><span style="color:var(--muted)">Total Pengeluaran</span><div id="totPengeluaran" class="font-tech num">0</div></div>
    </div>
    <div class="card4d p-4"><div class="font-tech mb-2" style="color:var(--accent4);font-size:12px;letter-spacing:.18em">📋 DAFTAR PENGELUARAN</div><div id="listPengeluaran"></div></div>
    <div class="card4d p-4"><div class="font-tech mb-2" style="color:var(--accent3);font-size:12px;letter-spacing:.18em">📈 GRAFIK PEMBUKUAN 4D</div><div class="chart4d"><div id="cBuku" style="height:300px"></div></div></div>
    <button class="btn btn-primary btn-lg w-full" onclick="unduhJpeg();addExp(10)">📸 UNDUH JPEG PEMBUKUAN</button>
  </section>

  <!-- ============ SETTING ============ -->
  <section id="page-setting" class="page hide px-4 pt-3 space-y-4">
    <div class="card4d p-4">
      <div class="font-tech mb-3" style="color:var(--accent);font-size:12px;letter-spacing:.18em">🔆 KECERAHAN LAYAR</div>
      <input id="bright" type="range" min="30" max="100" value="100" oninput="setBright(this.value)" style="width:100%;accent-color:var(--accent)">
      <div id="brightVal" class="text-center font-tech num mt-1">100%</div>
    </div>
    <div class="card4d p-4 space-y-2">
      <div class="font-tech" style="color:var(--accent2);font-size:12px;letter-spacing:.18em">➕ TAMBAH MASTER</div>
      <button class="btn btn-ghost w-full" onclick="addMaster('cabang');addExp(3)">🏠 CABANG BARU</button>
      <button class="btn btn-ghost w-full" onclick="addMaster('barang');addExp(3)">🥟 BARANG + HARGA</button>
      <button class="btn btn-ghost w-full" onclick="addMaster('bumbu');addExp(3)">🌶️ BUMBU + HARGA</button>
    </div>
    <div class="card4d p-4"><div class="font-tech mb-2" style="color:var(--accent3);font-size:12px;letter-spacing:.18em">📍 DAFTAR CABANG</div><div id="mCabang"></div></div>
    <div class="card4d p-4"><div class="font-tech mb-2" style="color:#ff9a3c;font-size:12px;letter-spacing:.18em">💰 DAFTAR HARGA</div><div id="mHarga"></div></div>
  </section>

  <!-- ============ AI ODYSSEUS LV UP ============ -->
  <section id="page-ai" class="page hide px-4 pt-3 space-y-4">
    <div class="card4d p-5 text-center" style="background:
      radial-gradient(500px 200px at 50% 0%, rgba(0,240,255,.25), transparent 60%),
      radial-gradient(500px 200px at 50% 100%, rgba(194,75,255,.25), transparent 60%),
      linear-gradient(160deg, rgba(10,16,38,.92), rgba(5,8,20,.72))">
      <div style="font-size:64px;animation:floatY 3s ease-in-out infinite;filter:drop-shadow(0 0 24px var(--accent))">🤖</div>
      <div class="font-tech" style="font-size:22px;font-weight:900;background:linear-gradient(90deg,var(--accent),var(--accent2),var(--accent3),var(--accent4));-webkit-background-clip:text;background-clip:text;color:transparent">ODYSSEUS AI</div>
      <div class="row justify-center gap-4 mt-3">
        <div class="font-tech num" style="color:var(--accent3)">⚡ LV <b id="aiLv">01</b></div>
        <div class="font-tech num" style="color:var(--accent2)">🧠 EVO <b id="aiEvo">0</b></div>
      </div>
      <div class="lv-bar mt-3"><div class="lv-fill" id="lvFill" style="width:0%"></div></div>
      <div class="text-xs mt-1 num" style="color:var(--muted)">EXP <span id="aiExp">0</span> / 100 · Naik LV tiap 100 EXP</div>
    </div>

    <div class="card4d p-4">
      <div class="font-tech mb-2" style="color:var(--accent3);font-size:12px;letter-spacing:.18em">💡 SARAN CERDAS AI</div>
      <div id="aiSaran" class="space-y-2 text-sm"></div>
    </div>

    <div class="card4d p-4">
      <div class="font-tech mb-2" style="color:var(--accent);font-size:12px;letter-spacing:.18em">📊 RINGKASAN DATA REAL-TIME</div>
      <div id="aiData" class="space-y-1 text-sm num"></div>
    </div>

    <div class="card4d p-4" style="border-color:rgba(194,75,255,.4);box-shadow:0 0 40px rgba(194,75,255,.25)">
      <div class="font-tech mb-1" style="color:var(--accent2);font-size:12px;letter-spacing:.18em">⚡ INSTALL ODYSSEUS AI · LV MAX</div>
      <div style="color:var(--muted);font-size:12px;margin-bottom:10px">1 baris install otomatis full Termux + Ubuntu PRoot + Python + Rust + repo odysseus resmi</div>
      <div class="p-3 rounded-xl" style="background:#000;border:1px solid var(--border);font-family:monospace;font-size:11px;color:var(--accent3);word-break:break-all" id="installOne">curl -sSL https://raw.githubusercontent.com/AbuZar-Ansarii/Odysseus-Android/main/install.sh | bash</div>
      <div class="grid grid-cols-2 gap-2 mt-3">
        <button class="btn btn-primary" onclick="copyOne();addExp(10)">📋 SALIN 1 BARIS</button>
        <button class="btn btn-ghost" onclick="toggleFull()">📜 LANGKAH FULL</button>
      </div>
      <div id="installFull" class="hide mt-3 space-y-2 text-xs" style="font-family:monospace;color:var(--accent3)"></div>
      <button class="btn btn-primary btn-lg w-full mt-3" onclick="upgradeAI();">🚀 UPGRADE AI LV +10 SEGERA</button>
      <div class="text-xs mt-2 text-center" style="color:var(--muted)">User : <b class="text-white">admin</b> · Pass : <b class="text-white">71807180</b> · Port 7000</div>
    </div>
  </section>

</div>

<script>
/* ============================================================
   DATA LAYER + AI LEVEL SYSTEM
============================================================ */
const LS='odysseus_v42';
const DEF_CABANG=['KREO','PONDOK LABU','GANDARIA','PONDOK PINANG','KBJ/MTY'];
const DEF_TRADER={
  'KREO':['AGUS','Iwan','FAJAR','AZO'],
  'PONDOK LABU':['AKIM','Iwan','Bubun','AOS','Yayan','Wanda','Budi','UJANG'],
  'GANDARIA':['ISA','Yudi','YAYAT','UGUN','UCU','UJANG','Unang'],
  'PONDOK PINANG':['OGI','Ucup','ADE'],
  'KBJ/MTY':['AEP','Hasan','Aas','Deddy','YAMAN','DUDE','Fikri','AJI']
};
const HRG_BUTIR=250, HRG_BUMBU=12500, HRG_BUKU=230;

var DB={
  gudang:{sbTahu:0,sbSotong:0,srTahu:0,srSotong:0,bAtom:0,bBalado:0,dAtom:0,dBalado:0},
  cabang:JSON.parse(JSON.stringify(DEF_CABANG)),
  traders:{},
  keu:{asuransi:[],pengeluaran:[],kasbon:{},hutang:{}},
  buku:{sisaKemarin:0,sisaSekarang:0},
  harga:{tahu:HRG_BUTIR,sotong:HRG_BUTIR,atom:HRG_BUMBU,balado:HRG_BUMBU},
  trend:[],
  ai:{lv:1,exp:0,evo:0},
  lastDate:null,
  bright:100
};

function loadDB(){try{const x=JSON.parse(localStorage.getItem(LS));if(x)Object.assign(DB,x);}catch(e){}initTraders();saveDB();}
function saveDB(){localStorage.setItem(LS,JSON.stringify(DB));syncLvTop();}
function initTraders(){
  DB.cabang.forEach(c=>{
    if(!DB.traders[c])DB.traders[c]=[];
    (DEF_TRADER[c]||[]).forEach(n=>{if(!DB.traders[c].find(t=>t.nama.toUpperCase()===n.toUpperCase()))DB.traders[c].push(mkTrader(n));});
    DB.traders[c].forEach(t=>{if(!t.data)Object.assign(t,mkTrader(t.nama));});
  });
}
function mkTrader(nama){return{id:Date.now()+Math.random().toString(36).slice(2),nama,foto:'',data:{okT:0,okS:0,osT:0,osS:0,sisaT:0,sisaS:0,bsT:0,bsS:0,rijT:0,rijS:0,bAtom:0,bBalado:0}};}
DEF_CABANG.forEach(c=>{if(!DB.cabang.includes(c))DB.cabang.push(c);});

/* === AI EXP / LV === */
function addExp(x){
  if(!DB.ai)DB.ai={lv:1,exp:0,evo:0};
  DB.ai.exp=Math.max(0,(DB.ai.exp||0)+x);DB.ai.evo=(DB.ai.evo||0)+1;
  while(DB.ai.exp>=100){DB.ai.exp-=100;DB.ai.lv=(DB.ai.lv||1)+1;flashLv();}
  saveDB();syncLvTop();
}
function upgradeAI(){DB.ai.lv=(DB.ai.lv||1)+10;DB.ai.exp=0;DB.ai.evo=(DB.ai.evo||0)+25;saveDB();syncLvTop();flashLv();alert('🚀 ODYSSEUS AI NAIK 10 LV !\nLV Sekarang : '+DB.ai.lv+'\nEvolusi : '+DB.ai.evo+'x');}
function syncLvTop(){
  const lv=String(DB.ai?.lv||1).padStart(2,'0');
  document.getElementById('aiLvTop').textContent=lv;
  document.getElementById('aiExpTop').textContent=DB.ai?.exp||0;
  document.getElementById('lvIntro').textContent=lv;
  const a=document.getElementById('aiLv');if(a)a.textContent=lv;
  const b=document.getElementById('aiEvo');if(b)b.textContent=DB.ai?.evo||0;
  const c=document.getElementById('aiExp');if(c)c.textContent=DB.ai?.exp||0;
  const d=document.getElementById('lvFill');if(d)d.style.width=(DB.ai?.exp||0)+'%';
}
function flashLv(){
  const n=document.createElement('div');
  n.textContent='⬆️ LV UP !';
  Object.assign(n.style,{position:'fixed',top:'30%',left:'50%',transform:'translateX(-50%)',zIndex:99999,padding:'14px 26px',borderRadius:'16px',
    background:'linear-gradient(135deg,#00f0ff,#c24bff)',color:'#03101a',fontFamily:'Orbitron',fontWeight:900,fontSize:18,
    boxShadow:'0 20px 60px rgba(0,240,255,.6)',animation:'introFade 1.6s ease forwards'});
  document.body.appendChild(n);setTimeout(()=>n.remove(),1600);
}

/* === CLOCK + MIDNIGHT === */
function tick(){
  const d=new Date();
  document.getElementById('clock').textContent=d.toLocaleTimeString('id-ID',{hour:'2-digit',minute:'2-digit',second:'2-digit'});
  const today=d.toDateString();
  if(DB.lastDate && DB.lastDate!==today){midnightShift();addExp(25);}
  if(!DB.lastDate)DB.lastDate=today;
}
function midnightShift(){
  const sisaT=Math.max(0,n(DB.gudang.sbTahu)-sumAll('osT')+sumAll('rijT'));
  const sisaS=Math.max(0,n(DB.gudang.sbSotong)-sumAll('osS')+sumAll('rijS'));
  DB.gudang.srTahu=n(DB.gudang.srTahu)+sisaT;DB.gudang.srSotong=n(DB.gudang.srSotong)+sisaS;
  DB.gudang.sbTahu=0;DB.gudang.sbSotong=0;DB.gudang.dAtom=0;DB.gudang.dBalado=0;
  DB.cabang.forEach(c=>DB.traders[c].forEach(t=>{
    t.data.okT=t.data.osT;t.data.okS=t.data.osS;
    ['osT','osS','sisaT','sisaS','bsT','bsS','rijT','rijS','bAtom','bBalado'].forEach(k=>t.data[k]=0);
  }));
  DB.lastDate=new Date().toDateString();saveDB();renderAll();
}
setInterval(tick,1000);tick();

/* === HELPERS === */
const n=v=>Number(v||0);
const rp=v=>'Rp '+Number(v||0).toLocaleString('id-ID');
const sumAll=k=>DB.cabang.reduce((a,c)=>a+DB.traders[c].reduce((x,t)=>x+n(t.data[k]),0),0);
const sumCab=(c,k)=>DB.traders[c].reduce((a,t)=>a+n(t.data[k]),0);
function calcTrader(d){
  const distT=(n(d.osT)-n(d.rijT))-(n(d.sisaT)-n(d.bsT));
  const distS=(n(d.osS)-n(d.rijS))-(n(d.sisaS)-n(d.bsS));
  const jualT=(n(d.okT)-n(d.sisaT))*DB.harga.tahu;
  const jualS=(n(d.okS)-n(d.sisaS))*DB.harga.sotong;
  const bAtom=n(d.bAtom)*DB.harga.atom,bBalado=n(d.bBalado)*DB.harga.balado;
  return{distT,distS,jualT,jualS,bAtom,bBalado,total:jualT+jualS+bAtom+bBalado};
}

/* ============================================================
   PS4 XMB NAV
============================================================ */
const PAGES=['dash','gudang','pedagang','distribusi','keuangan','pembukuan','setting','ai'];
document.getElementById('xmb').addEventListener('click',e=>{
  const it=e.target.closest('.xmb-item');if(!it)return;
  document.querySelectorAll('.xmb-item').forEach(x=>x.classList.remove('active'));
  it.classList.add('active');
  const idx=PAGES.indexOf(it.dataset.p);
  const bar=document.getElementById('xmbBar');
  bar.style.width=(it.offsetWidth-8)+'px';
  bar.style.marginLeft=(it.offsetLeft+14)+'px';
  openPage(it.dataset.p);
});
function syncXmb(p){
  document.querySelectorAll('.xmb-item').forEach(x=>{
    x.classList.toggle('active',x.dataset.p===p);
    if(x.dataset.p===p){
      const bar=document.getElementById('xmbBar');
      bar.style.width=(x.offsetWidth-8)+'px';
      bar.style.marginLeft=(x.offsetLeft+14)+'px';
      x.scrollIntoView({behavior:'smooth',inline:'center',block:'nearest'});
    }
  });
}

/* === NAV === */
function openPage(p){
  document.querySelectorAll('.page').forEach(x=>x.classList.add('hide'));
  document.getElementById('page-'+p).classList.remove('hide');
  syncXmb(p);
  setTimeout(renderPage,60,p);
}
function renderPage(p){
  ({dash:renderDash,gudang:renderGudang,pedagang:renderPedagang,distribusi:renderDistribusi,
    keuangan:()=>keuTab('asuransi'),pembukuan:renderBuku,setting:renderSetting,ai:renderAI})[p]?.();
}
function renderAll(){PAGES.forEach(renderPage);setBright(DB.bright);syncLvTop();}

/* ============================================================
   4D CHART ENGINE
============================================================ */
const PAL=['#00f0ff','#c24bff','#00ffa8','#ff3d8a','#ffd24d','#ff9a3c'];
function baseOpt(){return{
  backgroundColor:'transparent',
  animationDuration:1800,
  animationEasing:'elasticOut',
  animationThreshold:1,
  tooltip:{trigger:'axis',backgroundColor:'rgba(5,8,20,.95)',borderColor:'#00f0ff',borderWidth:1,textStyle:{color:'#fff',fontFamily:'Rajdhani'},extraCssText:'box-shadow:0 10px 40px rgba(0,240,255,.4);border-radius:12px'},
  textStyle:{fontFamily:'Rajdhani',color:'#b8c8f0'}
};}
function axisStyle(){return{
  axisLabel:{color:'#7b8bbf',fontSize:11},
  axisLine:{lineStyle:{color:'rgba(0,240,255,.25)'}},
  splitLine:{lineStyle:{color:'rgba(0,240,255,.08)',type:'dashed'}},
  axisTick:{show:false}
};}
function grad4d(c1,c2){return{type:'linear',x:0,y:0,x2:0,y2:1,colorStops:[{offset:0,color:c1+'ee'},{offset:1,color:c2+'15'}]}};
function glowSerie(col){return{
  shadowBlur:24,shadowColor:col,shadowOffsetY:8,
  itemStyle:{borderRadius:[10,10,0,0]}
};}

/* ============================================================
   DASHBOARD
============================================================ */
function renderDash(){
  let omset=0;DB.cabang.forEach(c=>DB.traders[c].forEach(t=>omset+=calcTrader(t.data).total));
  document.getElementById('kpiOmset').textContent=rp(omset);
  document.getElementById('kpiDist').textContent=(sumAll('osT')+sumAll('osS')-sumAll('rijT')-sumAll('rijS'))+' butir';
  document.getElementById('kpiStok').textContent=(Math.max(0,n(DB.gudang.sbTahu)+n(DB.gudang.srTahu)-sumAll('osT')+sumAll('rijT'))+Math.max(0,n(DB.gudang.sbSotong)+n(DB.gudang.srSotong)-sumAll('osS')+sumAll('rijS')))+' butir';
  document.getElementById('kpiUntung').textContent=rp(omset*0.24);
  DB.trend.push({t:new Date().toLocaleTimeString('id-ID',{hour:'2-digit',minute:'2-digit'}),v:omset});
  if(DB.trend.length>7)DB.trend.shift();
  const rc=DB.cabang.map(c=>({c,total:DB.traders[c].reduce((a,t)=>a+calcTrader(t.data).total,0)})).sort((a,b)=>b.total-a.total);
  const rt=[];DB.cabang.forEach(c=>DB.traders[c].forEach(t=>rt.push({nama:t.nama,cabang:c,total:calcTrader(t.data).total})));rt.sort((a,b)=>b.total-a.total);
  const maxR=Math.max(500000,...rc.map(x=>x.total));

  setTimeout(()=>{
    // LINE 4D
    (function(){const el=document.getElementById('cLine');el.innerHTML='';el.removeAttribute('_echarts_instance_');
    const c=echarts.init(el);const o=baseOpt();o.grid={left:40,right:20,top:20,bottom:30};
    o.xAxis={type:'category',data:DB.trend.map(x=>x.t),...axisStyle()};
    o.yAxis={type:'value',...axisStyle()};
    o.series=[{type:'line',smooth:true,symbol:'circle',symbolSize:10,data:DB.trend.map(x=>x.v),
      lineStyle:{width:5,color:PAL[0],shadowBlur:20,shadowColor:PAL[0]},
      itemStyle:{color:PAL[0],borderColor:'#fff',borderWidth:2,shadowBlur:16,shadowColor:PAL[0]},
      areaStyle:grad4d(PAL[0],PAL[0]),
      emphasis:{scale:1.4,itemStyle:{shadowBlur:30}}}];
    c.setOption(o);window.addEventListener('resize',()=>c.resize());particles('pLine');})();

    // RADAR 4D
    (function(){const el=document.getElementById('cRadar');el.innerHTML='';el.removeAttribute('_echarts_instance_');
    const c=echarts.init(el);const o=baseOpt();
    o.radar={indicator:DB.cabang.map(x=>({name:x.c,max:maxR})),
      axisName:{color:'#fff',fontFamily:'Orbitron',fontSize:10,textShadowColor:PAL[1],textShadowBlur:8},
      splitLine:{lineStyle:{color:'rgba(194,75,255,.25)'}},
      splitArea:{areaStyle:{color:['rgba(0,240,255,.04)','rgba(194,75,255,.06)']}},
      axisLine:{lineStyle:{color:'rgba(0,240,255,.25)'}}};
    o.series=[{type:'radar',data:[{value:rc.map(x=>x.total),name:'Kinerja',
      areaStyle:{color:{type:'radial',x:0.5,y:0.5,r:0.5,colorStops:[{offset:0,color:'rgba(194,75,255,.6)'},{offset:1,color:'rgba(0,240,255,.1)'}]}},
      lineStyle:{width:3,color:PAL[1],shadowBlur:20,shadowColor:PAL[1]},
      itemStyle:{color:PAL[1],shadowBlur:16}}]}];
    c.setOption(o);window.addEventListener('resize',()=>c.resize());})();

    // BAR 3D
    (function(){const el=document.getElementById('cBar');el.innerHTML='';el.removeAttribute('_echarts_instance_');
    const c=echarts.init(el);const o=baseOpt();o.grid={left:90,right:20,top:10,bottom:30};
    o.xAxis={type:'value',...axisStyle()};
    o.yAxis={type:'category',data:rc.map(x=>x.c).reverse(),...axisStyle(),axisLabel:{color:'#fff',fontFamily:'Orbitron',fontSize:10}};
    o.series=[{type:'bar',data:rc.map(x=>({value:x.total,itemStyle:{
      borderRadius:[0,10,10,0],
      color:{type:'linear',x:0,y:0,x2:1,y2:0,colorStops:[{offset:0,color:PAL[2]},{offset:1,color:PAL[0]}]},
      shadowBlur:22,shadowColor:PAL[0]
    }})),barWidth:22,emphasis:{itemStyle:{shadowBlur:40}}}];
    c.setOption(o);window.addEventListener('resize',()=>c.resize());})();

    // DONAT 4D
    (function(){const el=document.getElementById('cDonut');el.innerHTML='';el.removeAttribute('_echarts_instance_');
    const c=echarts.init(el);const o=baseOpt();o.tooltip.trigger='item';
    o.legend={bottom:0,textStyle:{color:'#cfd9ff'}};
    const donutData=[
      {value:Math.max(0,sumAll('osT')-sumAll('rijT')),name:'Tahu',itemStyle:{color:PAL[0],shadowBlur:24,shadowColor:PAL[0]}},
      {value:Math.max(0,sumAll('osS')-sumAll('rijS')),name:'Sotong',itemStyle:{color:PAL[1],shadowBlur:24,shadowColor:PAL[1]}},
      {value:n(DB.gudang.dAtom),name:'Atom',itemStyle:{color:PAL[2],shadowBlur:24,shadowColor:PAL[2]}},
      {value:n(DB.gudang.dBalado),name:'Balado',itemStyle:{color:PAL[3],shadowBlur:24,shadowColor:PAL[3]}}
    ];
    o.series=[{type:'pie',radius:['42%','74%'],center:['50%','46%'],roseType:'radius',
      itemStyle:{borderRadius:12,borderColor:'#03050a',borderWidth:3},
      label:{color:'#fff',fontFamily:'Orbitron',fontSize:11,textShadowBlur:6},
      labelLine:{lineStyle:{color:PAL[0],width:2}},
      data:donutData,emphasis:{scale:true,scaleSize:14,itemStyle:{shadowBlur:50}}}];
    c.setOption(o);window.addEventListener('resize',()=>c.resize());})();
  },50);

  document.getElementById('rankCabang').innerHTML=rc.map((r,i)=>`<div class="row py-2 border-b border-[var(--border)]"><span class="chip" style="background:rgba(0,240,255,.14);color:var(--accent)">#${i+1}</span><b style="flex:1;margin-left:8px">${r.c}</b><span class="num font-tech" style="color:var(--accent3)">${rp(r.total)}</span></div>`).join('');
  document.getElementById('rankPedagang').innerHTML=rt.slice(0,8).map((r,i)=>`<div class="row py-2 border-b border-[var(--border)]"><span class="chip" style="background:rgba(194,75,255,.14);color:var(--accent2)">#${i+1}</span><div style="flex:1;margin-left:8px"><b>${r.nama}</b><div style="color:var(--muted);font-size:11px">${r.cabang}</div></div><span class="num font-tech" style="color:var(--accent3)">${rp(r.total)}</span></div>`).join('');
  document.getElementById('rankProduk').innerHTML=`<div class="row justify-between py-2 border-b border-[var(--border)]"><span>🥟 Tahu Terjual</span><b class="num" style="color:var(--accent)">${sumAll('okT')-sumAll('sisaT')} butir</b></div>
    <div class="row justify-between py-2 border-b border-[var(--border)]"><span>🦑 Sotong Terjual</span><b class="num" style="color:var(--accent2)">${sumAll('okS')-sumAll('sisaS')} butir</b></div>
    <div class="row justify-between py-2 border-b border-[var(--border)]"><span>🌶️ Atom Tersalurkan</span><b class="num" style="color:var(--accent3)">${n(DB.gudang.dAtom)} bks</b></div>
    <div class="row justify-between py-2"><span>🔥 Balado Tersalurkan</span><b class="num" style="color:var(--accent4)">${n(DB.gudang.dBalado)} bks</b></div>`;
  document.getElementById('tData').innerHTML=rt.map(r=>`<tr style="border-top:1px solid var(--border)"><td class="py-1.5">${r.nama}</td><td class="text-right num">${r.total?Math.round(r.total/DB.harga.tahu/2):0}</td><td class="text-right num">${r.total?Math.round(r.total/DB.harga.tahu/4):0}</td><td class="text-right num font-tech" style="color:var(--accent3)">${rp(r.total)}</td></tr>`).join('');
}

/* Partikel 4D */
function particles(id){
  const c=document.getElementById(id);if(!c)return;
  const dpr=window.devicePixelRatio||1;c.width=c.clientWidth*dpr;c.height=c.clientHeight*dpr;
  const ctx=c.getContext('2d');ctx.scale(dpr,dpr);const W=c.clientWidth,H=c.clientHeight;
  const pts=Array.from({length:35},()=>({x:Math.random()*W,y:Math.random()*H,vx:(Math.random()-.5)*.4,vy:(Math.random()-.5)*.4,r:Math.random()*2+.4,c:PAL[Math.floor(Math.random()*PAL.length)]}));
  function frame(){ctx.clearRect(0,0,W,H);pts.forEach(p=>{
    p.x+=p.vx;p.y+=p.vy;if(p.x<0||p.x>W)p.vx*=-1;if(p.y<0||p.y>H)p.vy*=-1;
    ctx.beginPath();ctx.arc(p.x,p.y,p.r,0,7);ctx.fillStyle=p.c+'aa';ctx.shadowBlur=14;ctx.shadowColor=p.c;ctx.fill();
  });requestAnimationFrame(frame);}frame();
}

/* ============================================================
   GUDANG
============================================================ */
function renderGudang(){
  Object.keys(DB.gudang).forEach(k=>{const el=document.getElementById(k);if(el)el.value=DB.gudang[k]||0;});
  calcGudang();
}
function saveGudang(){
  ['sbTahu','sbSotong','srTahu','srSotong','bAtom','bBalado','dAtom','dBalado'].forEach(k=>{const el=document.getElementById(k);if(el)DB.gudang[k]=n(el.value);});
  saveDB();
}
function calcGudang(){
  const tT=n(DB.gudang.sbTahu)+n(DB.gudang.srTahu)-sumAll('osT')+sumAll('rijT');
  const tS=n(DB.gudang.sbSotong)+n(DB.gudang.srSotong)-sumAll('osS')+sumAll('rijS');
  document.getElementById('totTahu').textContent=Math.max(0,tT);
  document.getElementById('totSotong').textContent=Math.max(0,tS);
  document.getElementById('nomAtom').textContent=rp(n(DB.gudang.dAtom)*DB.harga.atom);
  document.getElementById('nomBalado').textContent=rp(n(DB.gudang.dBalado)*DB.harga.balado);
}

/* ============================================================
   PEDAGANG
============================================================ */
function renderPedagang(){
  const w=document.getElementById('listCabang');w.innerHTML='';
  DB.cabang.forEach(c=>{
    const d=document.createElement('div');d.className='branch-row card4d';d.style.padding='12px';
    d.innerHTML=`<div><div class="font-tech" style="font-weight:900">${c}</div><div style="color:var(--muted);font-size:12px">${DB.traders[c].length} Pedagang</div></div><svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="var(--accent)" stroke-width="2.5"><path d="M9 18l6-6-6-6"/></svg>`;
    d.onclick=()=>openCabang(c);w.appendChild(d);
  });
}
function openCabang(c){
  const w=document.getElementById('detailCabang');w.classList.remove('hide');
  document.getElementById('detailPedagang').classList.add('hide');
  w.innerHTML=`<div class="font-tech mb-3" style="color:var(--accent);font-size:13px;letter-spacing:.18em">📍 ${c}</div>`;
  DB.traders[c].forEach(t=>{
    const r=document.createElement('div');r.className='trader-row';r.style.borderRadius='12px';
    r.innerHTML=`<img class="avatar" src="${t.foto||''}" onerror="this.style.display='none'"><div class="avatar" ${t.foto?'style="display:none"':''}>${t.nama[0]}</div><div style="flex:1"><div style="font-weight:800">${t.nama}</div><div class="num" style="color:var(--muted);font-size:12px">${rp(calcTrader(t.data).total)}</div></div><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="var(--accent2)" stroke-width="2.5"><path d="M9 18l6-6-6-6"/></svg>`;
    r.onclick=()=>openPedagang(c,t);w.appendChild(r);
  });
}
function openPedagang(c,t){
  const w=document.getElementById('detailPedagang');w.classList.remove('hide');
  const d=t.data;
  w.innerHTML=`
    <div class="row" style="gap:14px">
      <img class="avatar" style="width:60px;height:60px;font-size:24px" src="${t.foto||''}" onerror="this.style.display='none'">
      <div class="avatar" ${t.foto?'style="display:none;width:60px;height:60px;font-size:24px"':''}>${t.nama[0]}</div>
      <div style="flex:1"><div class="font-tech" style="font-size:20px;font-weight:900">${t.nama}</div><div style="color:var(--muted);font-size:12px">${c}</div></div>
      <label class="btn btn-ghost" style="padding:8px 10px">📸<input type="file" accept="image/*" class="hide" onchange="upFoto('${c}','${t.id}',this)"></label>
    </div>
    <div class="divider"></div>
    <div class="font-tech" style="color:var(--accent);font-size:11px;letter-spacing:.18em">🥟 TAHU · 🦑 SOTONG</div>
    ${field2('OLDER KEMARIN','okT','okS',d)}
    ${field2('OLDER SEKARANG','osT','osS',d)}
    ${field2('SISA','sisaT','sisaS',d)}
    ${field2('BS','bsT','bsS',d)}
    ${field2('RIJEK','rijT','rijS',d)}
    <div class="divider"></div>
    <div class="font-tech" style="color:var(--accent2);font-size:11px;letter-spacing:.18em">🌶️ BUMBU</div>
    ${field2('BUMBU','bAtom','bBalado',d,['ATOM','BALADO'])}
    <div class="divider"></div>
    <div class="card4d p-3 space-y-1 text-sm num" style="padding:12px">
      <div class="row justify-between"><span style="color:var(--muted)">Dist Tahu</span><b id="rDT" style="color:var(--accent)">0</b></div>
      <div class="row justify-between"><span style="color:var(--muted)">Dist Sotong</span><b id="rDS" style="color:var(--accent2)">0</b></div>
      <div class="row justify-between"><span style="color:var(--muted)">Terjual × ${DB.harga.tahu}</span><b id="rJT" style="color:var(--accent3)">0</b></div>
      <div class="row justify-between"><span style="color:var(--muted)">Bumbu</span><b id="rB" style="color:var(--accent4)">0</b></div>
      <div class="row justify-between pt-2" style="border-top:1px solid var(--border)"><span style="color:var(--accent)">TOTAL</span><b class="font-tech" id="rTot" style="color:var(--accent);font-size:20px">0</b></div>
    </div>
    <button class="btn btn-primary btn-lg w-full" onclick="saveTrader('${c}','${t.id}')">💾 SIMPAN & UPDATE GRAFIK</button>
  `;recalcTrader();
}
function field2(label,a,b,d,lab=['TAHU','SOTONG']){
  return `<div class="mb-2">
    <div class="chip mb-1" style="background:rgba(255,255,255,.04);color:var(--muted)">${label}</div>
    <div class="grid grid-cols-2 gap-2">
      <div><div class="text-xs" style="color:var(--accent)">${lab[0]}</div><input class="inp num" data-k="${a}" type="number" value="${n(d[a])}" oninput="recalcTrader()"></div>
      <div><div class="text-xs" style="color:var(--accent2)">${lab[1]}</div><input class="inp num" data-k="${b}" type="number" value="${n(d[b])}" oninput="recalcTrader()"></div>
    </div></div>`;
}
function recalcTrader(){
  const o={};document.getElementById('detailPedagang').querySelectorAll('[data-k]').forEach(i=>o[i.dataset.k]=n(i.value));
  const r=calcTrader(o);
  document.getElementById('rDT').textContent=r.distT;
  document.getElementById('rDS').textContent=r.distS;
  document.getElementById('rJT').textContent=rp(r.jualT+r.jualS);
  document.getElementById('rB').textContent=rp(r.bAtom+r.bBalado);
  document.getElementById('rTot').textContent=rp(r.total);
}
function saveTrader(c,id){
  const t=DB.traders[c].find(x=>x.id===id);if(!t)return;
  document.getElementById('detailPedagang').querySelectorAll('[data-k]').forEach(i=>t.data[i.dataset.k]=n(i.value));
  saveDB();renderAll();addExp(5);
  alert('✅ '+t.nama+' tersimpan · AI +5 EXP');
}
function upFoto(c,id,inp){
  const f=inp.files[0];if(!f)return;
  const r=new FileReader();r.onload=e=>{DB.traders[c].find(x=>x.id===id).foto=e.target.result;saveDB();renderPedagang();openCabang(c);addExp(2);};
  r.readAsDataURL(f);
}
function tambahPedagang(){
  const nama=prompt('✏️ NAMA PEDAGANG BARU:');if(!nama)return;
  const cb=prompt('🏠 PILIH CABANG (nomor):\n\n'+DB.cabang.map((c,i)=>i+1+'. '+c).join('\n'));
  if(!cb||isNaN(cb)||cb<1||cb>DB.cabang.length)return alert('❌ Cabang tidak valid');
  const c=DB.cabang[cb-1];DB.traders[c].push(mkTrader(nama.toUpperCase()));
  saveDB();renderPedagang();addExp(5);alert('✅ '+nama+' → '+c);
}
function hapusPedagang(){
  const all=[];DB.cabang.forEach(c=>DB.traders[c].forEach(t=>all.push({c,t})));
  const p=prompt('🗑️ HAPUS (nomor / nama):\n\n'+all.map((x,i)=>i+1+'. '+x.t.nama+' ('+x.c+')').join('\n'));
  if(!p)return;let f=null;
  if(!isNaN(p))f=all[n(p)-1];else f=all.find(x=>x.t.nama.toUpperCase()===p.toUpperCase());
  if(!f)return alert('❌ Tidak ditemukan');
  if(confirm('Hapus '+f.t.nama+'?')){DB.traders[f.c]=DB.traders[f.c].filter(x=>x.id!==f.t.id);saveDB();renderPedagang();addExp(2);alert('✅ Terhapus');}
}

/* ============================================================
   DISTRIBUSI
============================================================ */
function renderDistribusi(){
  const w=document.getElementById('distList');w.innerHTML='';
  DB.cabang.forEach(c=>{
    const k=['osT','osS','sisaT','sisaS','bsT','bsS','rijT','rijS','bAtom','bBalado'];
    const v={};k.forEach(x=>v[x]=sumCab(c,x));const r=calcTrader(v);
    const d=document.createElement('div');d.className='card4d p-4';
    d.innerHTML=`<details open><summary class="font-tech cursor-pointer" style="list-style:none;display:flex;justify-content:space-between;align-items:center">
      <span style="font-weight:900;color:var(--accent)">📍 ${c}</span><span class="num" style="color:var(--accent3)">${rp(r.total)}</span>
    </summary>
    <div class="mt-3 space-y-2 text-sm">
      ${mini2('OLDER SEKARANG',v.osT,v.osS)}${mini2('SISA',v.sisaT,v.sisaS)}${mini2('BS',v.bsT,v.bsS)}${mini2('RIJEK',v.rijT,v.rijS)}${mini2('BUMBU',v.bAtom,v.bBalado)}
      <div class="divider"></div>
      <div class="row justify-between num"><span style="color:var(--muted)">Dist Tahu</span><b style="color:var(--accent)">${r.distT}</b></div>
      <div class="row justify-between num"><span style="color:var(--muted)">Dist Sotong</span><b style="color:var(--accent2)">${r.distS}</b></div>
      <div class="row justify-between num pt-2" style="border-top:1px solid var(--border)"><span style="color:var(--accent)">TOTAL</span><b class="font-tech" style="color:var(--accent);font-size:18px">${rp(r.total)}</b></div>
    </div></details>`;
    w.appendChild(d);
  });
}
function mini2(l,a,b){return `<div class="row justify-between num"><span style="color:var(--muted)">${l}</span><b style="color:var(--accent)">${a}</b> · <b style="color:var(--accent2)">${b}</b></div>`;}

/* ============================================================
   KEUANGAN
============================================================ */
function keuTab(t){
  const btns=document.querySelectorAll('#page-keuangan .btn');
  ['asuransi','pengeluaran','kasbon','hutang'].forEach((x,i)=>btns[i].className=i===({asuransi:0,pengeluaran:1,kasbon:2,hutang:3}[t])?'btn btn-primary':'btn btn-ghost');
  const w=document.getElementById('keuPanel');
  if(t==='asuransi')w.innerHTML=`<div class="card4d p-4 space-y-3"><div class="font-tech" style="color:var(--accent);font-size:12px;letter-spacing:.18em">🛡️ ASURANSI</div><input class="inp num" id="inAsNom" type="number" placeholder="Nominal"><button class="btn btn-primary w-full" onclick="addAsuransi()">💾 SIMPAN</button><div class="divider"></div><div id="listAs"></div></div>`;
  if(t==='pengeluaran')w.innerHTML=`<div class="card4d p-4 space-y-3"><div class="font-tech" style="color:#ff9a3c;font-size:12px;letter-spacing:.18em">🆘 PENGELUARAN</div><input class="inp" id="inKet" placeholder="Keterangan"><input class="inp num" id="inNom" type="number" placeholder="Nominal"><button class="btn btn-primary w-full" onclick="addPengeluaran()">💾 SIMPAN</button><div class="divider"></div><div id="listPen"></div></div>`;
  if(t==='kasbon')w.innerHTML=`<div class="card4d p-4 space-y-2"><div class="font-tech mb-2" style="color:var(--accent2);font-size:12px;letter-spacing:.18em">💢 KASBON</div>${allTraderBtns('kasbon')}</div>`;
  if(t==='hutang')w.innerHTML=`<div class="card4d p-4 space-y-2"><div class="font-tech mb-2" style="color:var(--accent4);font-size:12px;letter-spacing:.18em">💀 HUTANG</div>${allTraderBtns('hutang')}</div><div class="card4d p-4 mt-3"><div class="font-tech mb-2" style="color:var(--accent3);font-size:12px;letter-spacing:.18em">📋 DAFTAR HUTANG</div><div id="listHutang"></div></div>`;
  if(t==='asuransi')renderAs();if(t==='pengeluaran')renderPen();if(t==='hutang')setTimeout(renderHutang,20);
}
function allTraderBtns(m){
  let h='';DB.cabang.forEach(c=>{
    h+=`<div class="font-tech num mt-3 mb-1" style="color:var(--muted);font-size:10px;letter-spacing:.2em">${c}</div>`;
    DB.traders[c].forEach(t=>h+=`<button class="btn btn-ghost w-full mb-1" onclick="open${m==='kasbon'?'Kasbon':'Hutang'}Input('${c}','${t.id}','${t.nama}')">${t.nama}</button>`);
  });return h;
}
function addAsuransi(){const v=n(document.getElementById('inAsNom').value);if(!v)return;DB.keu.asuransi.push({tgl:stamp(),nom:v});saveDB();renderAs();addExp(3);alert('✅ Tersimpan');}
function renderAs(){const w=document.getElementById('listAs');if(!w)return;w.innerHTML=DB.keu.asuransi.map((a,i)=>`<div class="row justify-between py-2 border-b border-[var(--border)] num"><span>${a.tgl}</span><b>${rp(a.nom)}</b><button class="btn btn-ghost" style="padding:4px 10px;font-size:11px" onclick="DB.keu.asuransi.splice(${i},1);saveDB();renderAs()">✕</button></div>`).join('')||'<div style="color:var(--muted)">Belum ada</div>';}
function addPengeluaran(){const k=document.getElementById('inKet').value;const v=n(document.getElementById('inNom').value);if(!k||!v)return;DB.keu.pengeluaran.push({tgl:stamp(),ket:k,nom:v});saveDB();renderPen();addExp(3);alert('✅ Tersimpan');}
function renderPen(){const w=document.getElementById('listPen');if(!w)return;w.innerHTML=DB.keu.pengeluaran.map((a,i)=>`<div class="py-2 border-b border-[var(--border)]"><div class="row justify-between num"><b>${a.ket}</b><b style="color:#ff9a3c">${rp(a.nom)}</b></div><div class="row justify-between text-xs" style="color:var(--muted)"><span>${a.tgl}</span><button class="btn btn-ghost" style="padding:2px 8px;font-size:10px" onclick="DB.keu.pengeluaran.splice(${i},1);saveDB();renderPen()">✕</button></div></div>`).join('')||'<div style="color:var(--muted)">Belum ada</div>';}
function openKasbonInput(c,id,nama){const v=prompt('💢 '+nama+'\nNominal kasbon:');if(!v)return;if(!DB.keu.kasbon[id])DB.keu.kasbon[id]=[];DB.keu.kasbon[id].push({tgl:stamp(),nom:n(v),nama});saveDB();addExp(3);alert('✅ Kasbon tersimpan');}
function openHutangInput(c,id,nama){const v=prompt('💀 '+nama+'\nNominal hutang:');if(!v)return;if(!DB.keu.hutang[id])DB.keu.hutang[id]={nama,cabang:c,items:[]};DB.keu.hutang[id].items.push({tgl:stamp(),nom:n(v),bayar:0});saveDB();renderHutang();addExp(3);alert('✅ Hutang tersimpan');}
function renderHutang(){
  const w=document.getElementById('listHutang');if(!w)return;let h='';
  Object.entries(DB.keu.hutang).forEach(([id,hx])=>{
    const tot=hx.items.reduce((a,b)=>a+n(b.nom),0),bay=hx.items.reduce((a,b)=>a+n(b.bayar),0),sisa=tot-bay,lunas=sisa<=0;
    h+=`<details class="card4d p-3 mb-2" style="padding:12px"><summary class="cursor-pointer" style="list-style:none;display:flex;justify-content:space-between;align-items:center"><b>${hx.nama}</b><span class="chip ${lunas?'':'bg-red-500/15 text-red-300'}" style="background:${lunas?'rgba(0,255,168,.14);color:var(--accent3)':''}">${lunas?'✅ LUNAS':rp(sisa)}</span></summary>`;
    hx.items.forEach((it,i)=>{const ss=n(it.nom)-n(it.bayar);
      h+=`<div class="mt-2 p-2 rounded-xl" style="background:rgba(255,255,255,.03)"><div class="row justify-between text-xs num"><span>${it.tgl}</span><b>${rp(it.nom)}</b></div>
      <div class="row gap-2 mt-2"><input class="inp num" value="${it.bayar}" type="number" onchange="DB.keu.hutang['${id}'].items[${i}].bayar=n(this.value);saveDB();renderHutang()"><button class="btn btn-primary" style="padding:8px 12px" onclick="DB.keu.hutang['${id}'].items[${i}].bayar=${n(it.nom)};saveDB();renderHutang();addExp(2)">LUNAS</button></div>
      <div class="text-xs num mt-1" style="color:${ss<=0?'var(--accent3)':'#ff9a3c'}">Sisa : ${rp(ss)}</div></div>`;
    });h+=`</details>`;
  });w.innerHTML=h||'<div style="color:var(--muted)">Belum ada hutang</div>';
}
const stamp=()=>new Date().toLocaleDateString('id-ID')+' '+new Date().toLocaleTimeString('id-ID',{hour:'2-digit',minute:'2-digit'});

/* ============================================================
   PEMBUKUAN
============================================================ */
function renderBuku(){
  document.getElementById('sisaKemarin').value=DB.buku.sisaKemarin||0;
  document.getElementById('sisaSekarang').value=DB.buku.sisaSekarang||0;
  document.getElementById('bStokBaru').innerHTML=miniTbl(['Tgl','Tahu','Sotong'],[['Hari ini',DB.gudang.sbTahu,DB.gudang.sbSotong],['Rata / 7h',Math.round(n(DB.gudang.sbTahu)/7),Math.round(n(DB.gudang.sbSotong)/7)]]);
  const bsT=sumAll('bsT'),bsS=sumAll('bsS');
  document.getElementById('bBS').innerHTML=miniTbl(['Item','Tahu','Sotong'],[['Total BS (Sabtu-Jum)',bsT,bsS],['Rata / 7h',Math.round(bsT/7),Math.round(bsS/7)]]);
  const totAs=DB.keu.asuransi.reduce((a,b)=>a+n(b.nom),0);
  const totPen=DB.keu.pengeluaran.reduce((a,b)=>a+n(b.nom),0);
  document.getElementById('totAsuransi').textContent=rp(totAs);
  document.getElementById('totPengeluaran').textContent=rp(totPen);
  document.getElementById('listPengeluaran').innerHTML=DB.keu.pengeluaran.map(p=>`<div class="row justify-between py-2 border-b border-[var(--border)] num"><span><b>${p.ket}</b> <span style="color:var(--muted)">· ${p.tgl}</span></span><b style="color:#ff9a3c">${rp(p.nom)}</b></div>`).join('')||'<div style="color:var(--muted)">Belum ada</div>';
  const stokT=n(DB.gudang.sbTahu)+n(DB.gudang.srTahu),stokS=n(DB.gudang.sbSotong)+n(DB.gudang.srSotong);
  const a=(stokT-bsT)+(stokS-bsS)+n(DB.buku.sisaKemarin)-n(DB.buku.sisaSekarang);
  const hasil=a*HRG_BUKU+totAs-totPen;
  document.getElementById('hasilBuku').textContent=rp(hasil);
  setTimeout(()=>{
    const el=document.getElementById('cBuku');if(!el)return;el.innerHTML='';el.removeAttribute('_echarts_instance_');
    const c=echarts.init(el);const o=baseOpt();o.grid={left:40,right:20,top:20,bottom:40};
    o.xAxis={type:'category',data:['Stok T','Stok S','BS T','BS S','Asuransi','Pengeluaran','Hasil'],...axisStyle(),axisLabel:{rotate:25,color:'#cfd9ff',fontFamily:'Orbitron',fontSize:10}};
    o.yAxis={type:'value',...axisStyle()};
    o.series=[{type:'bar',data:[stokT,stokS,bsT,bsS,totAs,totPen,Math.max(0,hasil)].map((v,i)=>({value:v,itemStyle:{
      borderRadius:[12,12,0,0],
      color:{type:'linear',x:0,y:0,x2:0,y2:1,colorStops:[{offset:0,color:PAL[i%PAL.length]},{offset:1,color:PAL[(i+1)%PAL.length]+'22'}]},
      shadowBlur:22,shadowColor:PAL[i%PAL.length]
    }})),barWidth:22,emphasis:{scale:1.2}}];
    c.setOption(o);window.addEventListener('resize',()=>c.resize());
  },60);
}
function miniTbl(h,rows){let t='<table class="w-full text-sm num"><thead><tr style="color:var(--muted)">'+h.map(x=>`<th class="text-left py-1">${x}</th>`).join('')+'</tr></thead><tbody>';rows.forEach(r=>t+='<tr style="border-top:1px solid var(--border)">'+r.map(x=>`<td class="py-1.5">${x}</td>`).join('')+'</tr>');return t+'</tbody></table>';}
function saveBuku(){DB.buku.sisaKemarin=n(document.getElementById('sisaKemarin').value);DB.buku.sisaSekarang=n(document.getElementById('sisaSekarang').value);saveDB();renderBuku();}
function unduhJpeg(){
  const node=document.getElementById('page-pembukuan');
  if(!window.html2canvas){const s=document.createElement('script');s.src='https://cdn.jsdelivr.net/npm/html2canvas@1.4.1/dist/html2canvas.min.js';s.onload=unduhJpeg;document.head.appendChild(s);return;}
  html2canvas(node,{background:'#03050a',scale:2.2,useCORS:true}).then(c=>{const a=document.createElement('a');a.href=c.toDataURL('image/jpeg',.93);a.download='Pembukuan_Odysseus_'+Date.now()+'.jpg';a.click();});
}

/* ============================================================
   SETTING
============================================================ */
function renderSetting(){
  document.getElementById('bright').value=DB.bright||100;document.getElementById('brightVal').textContent=(DB.bright||100)+'%';
  document.getElementById('mCabang').innerHTML=DB.cabang.map(c=>`<div class="row justify-between py-2 border-b border-[var(--border)]"><b>${c}</b><span style="color:var(--muted)">${DB.traders[c]?.length||0} pedagang</span></div>`).join('');
  document.getElementById('mHarga').innerHTML=Object.entries(DB.harga).map(([k,v])=>`<div class="row justify-between py-2 border-b border-[var(--border)]"><b class="uppercase">${k}</b><input class="inp num" style="width:150px;text-align:right" type="number" value="${v}" onchange="DB.harga['${k}']=n(this.value);saveDB();renderAll()"></div>`).join('');
}
function setBright(v){DB.bright=n(v);document.getElementById('app').style.filter='brightness('+(n(v)/100)+')';document.getElementById('brightVal').textContent=v+'%';saveDB();}
function addMaster(t){
  if(t==='cabang'){const nm=prompt('🏠 Nama cabang baru:');if(!nm)return;nm=nm.toUpperCase();if(!DB.cabang.includes(nm)){DB.cabang.push(nm);DB.traders[nm]=[];saveDB();renderSetting();renderPedagang();addExp(3);alert('✅ Cabang ditambah');}}
  if(t==='barang'){const nm=prompt('🥟 Nama barang:');if(!nm)return;const hr=prompt('💰 Harga / butir:');if(!hr)return;DB.harga[nm.toLowerCase()]=n(hr);saveDB();renderSetting();addExp(3);alert('✅ Barang ditambah');}
  if(t==='bumbu'){const nm=prompt('🌶️ Nama bumbu:');if(!nm)return;const hr=prompt('💰 Harga / bungkus:');if(!hr)return;DB.harga[nm.toLowerCase()]=n(hr);saveDB();renderSetting();addExp(3);alert('✅ Bumbu ditambah');}
}

/* ============================================================
   AI ODYSSEUS + INSTALL
============================================================ */
const INSTALL_FULL=[
  '# 1) Update + install pendukung Termux',
  'pkg update && pkg upgrade -y',
  'pkg install git curl proot-distro -y',
  '',
  '# 2) Install Ubuntu PRoot',
  'proot-distro install ubuntu',
  'proot-distro login ubuntu',
  '',
  '# 3) Update Ubuntu + toolchain Python + Rust (aarch64)',
  'apt update && apt upgrade -y',
  'apt install -y git python3 python3-pip python3-venv build-essential libssl-dev libffi-dev python3-dev rustc cargo curl',
  '',
  '# 4) Clone repo resmi Odysseus',
  'git clone https://github.com/pewdiepie-archdaemon/odysseus.git',
  'cd odysseus',
  '',
  '# 5) Virtual env + upgrade installer',
  'python3 -m venv venv',
  'source venv/bin/activate',
  'pip install --upgrade pip setuptools wheel',
  '',
  '# 6) Install dependencies AI',
  'pip install -r requirements.txt',
  '',
  '# 7) Init DB dengan password admin',
  'export ODYSSEUS_ADMIN_PASSWORD="71807180"',
  'python3 setup.py',
  '',
  '# 8) Jalankan server (port 7000)',
  'chmod +x run.sh',
  './run.sh',
  '',
  '# — Atau jalan langsung via uvicorn:',
  'proot-distro login ubuntu -- bash -c "cd odysseus && source venv/bin/activate && python3 -m uvicorn app:app --host 0.0.0.0 --port 7000"'
];
function renderAI(){
  syncLvTop();
  document.getElementById('installFull').innerHTML=INSTALL_FULL.map(l=>`<div class="p-2 rounded-lg" style="background:#000;border-left:3px solid var(--accent)">${l||'<br>'}</div>`).join('');
  const omset=DB.cabang.reduce((a,c)=>a+DB.traders[c].reduce((x,t)=>x+calcTrader(t.data).total,0),0);
  const jmlT=DB.cabang.reduce((a,c)=>a+DB.traders[c].length,0);
  const sT=Math.max(0,n(DB.gudang.sbTahu)+n(DB.gudang.srTahu)-sumAll('osT')+sumAll('rijT'));
  const sS=Math.max(0,n(DB.gudang.sbSotong)+n(DB.gudang.srSotong)-sumAll('osS')+sumAll('rijS'));
  const totHut=Object.values(DB.keu.hutang).reduce((a,h)=>a+h.items.reduce((x,i)=>x+n(i.nom)-n(i.bayar),0),0);
  document.getElementById('aiData').innerHTML=`<div class="row justify-between"><span style="color:var(--muted)">Cabang</span><b>${DB.cabang.length}</b></div>
    <div class="row justify-between"><span style="color:var(--muted)">Pedagang</span><b>${jmlT}</b></div>
    <div class="row justify-between"><span style="color:var(--muted)">Omset Hari Ini</span><b style="color:var(--accent3)">${rp(omset)}</b></div>
    <div class="row justify-between"><span style="color:var(--muted)">Stok Tahu Siap</span><b style="color:${sT<20?'var(--accent4)':'var(--accent)'}">${sT}</b></div>
    <div class="row justify-between"><span style="color:var(--muted)">Stok Sotong Siap</span><b style="color:var(--accent2)">${sS}</b></div>
    <div class="row justify-between"><span style="color:var(--muted)">Bumbu Atom</span><b>${n(DB.gudang.bAtom)} bks</b></div>
    <div class="row justify-between"><span style="color:var(--muted)">Bumbu Balado</span><b>${n(DB.gudang.bBalado)} bks</b></div>
    <div class="row justify-between"><span style="color:var(--muted)">Total Hutang Berjalan</span><b style="color:var(--accent4)">${rp(totHut)}</b></div>`;
  const s=[];
  if(omset<500000)s.push('⚠️ Omset di bawah 500rb — cek ranking cabang terbawah, tambah stok pagi');
  else s.push('✅ Omset SEHAT — pola distribusi baik, pertahankan');
  if(sT<20)s.push('🔴 STOK TAHUN KRITIS (< 20 butir) — segera produksi tambahan');
  else if(sT<60)s.push('🟡 Stok tahu menipis — siapkan stok tambahan besok');
  else s.push('🟢 Stok tahu aman untuk 1-2 hari');
  if(n(DB.gudang.bAtom)<5||n(DB.gudang.bBalado)<5)s.push('🌶️ Stok bumbu < 5 — segera restock dari distributor');
  if(Object.keys(DB.keu.hutang).length>3)s.push('💀 '+Object.keys(DB.keu.hutang).length+' pedagang punya hutang — jadwalkan penagihan rutin');
  const totPen=DB.keu.pengeluaran.reduce((a,b)=>a+n(b.nom),0);
  if(totPen>omset*0.3)s.push('🆘 PENGELUARAN > 30% OMSET — audit pos pengeluaran segera');
  s.push(`🧠 AI telah berevolusi ${DB.ai?.evo||0}x · terus belajar dari setiap input data`);
  s.push(`⚡ Naikkan level AI dengan rajin input data → prediksi stok makin akurat`);
  document.getElementById('aiSaran').innerHTML=s.map(x=>`<div class="p-3 rounded-xl" style="background:rgba(0,255,168,.05);border-left:3px solid var(--accent3)">${x}</div>`).join('');
}
function copyOne(){navigator.clipboard.writeText(document.getElementById('installOne').textContent).then(()=>{alert('✅ 1 baris install tersalin!\n\nBuka Termux lalu TEKAN LAMA → TEMPEL → ENTER');addExp(5);});}
function toggleFull(){document.getElementById('installFull').classList.toggle('hide');}

/* ============================================================
   INTRO TYPER + BOOT
============================================================ */
(function(){
  const el=document.getElementById('typer');const txt='AI ODYSSEUS';let i=0;
  const t=setInterval(()=>{el.textContent+=txt[i++];if(i>=txt.length){clearInterval(t);setTimeout(()=>{document.getElementById('intro').classList.add('hide');document.getElementById('app').classList.remove('hide');openPage('dash');},900);}},110);
})();
loadDB();
</script>
</div>
</html>
