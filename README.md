html_content = r'''<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Animasi Bola Dunia 3D</title>
<script src="https://cdn.jsdelivr.net/npm/gif.js@0.2.0/dist/gif.min.js"></script>
<style>
  *{box-sizing:border-box;margin:0;padding:0}
  body{
    min-height:100vh;
    background: radial-gradient(ellipse at center, #0a1a3a 0%, #000010 70%, #000 100%);
    color:#eaf2ff;
    font-family: system-ui, -apple-system, Segoe UI, Roboto, sans-serif;
    display:flex; flex-direction:column; align-items:center; justify-content:center;
    padding:24px; gap:20px;
  }
  h1{font-size:22px; letter-spacing:1px; color:#9fd3ff; text-shadow:0 0 12px #3a8bff88}
  .wrap{
    background: rgba(255,255,255,0.04);
    border:1px solid rgba(120,180,255,0.18);
    border-radius:20px;
    padding:20px;
    backdrop-filter: blur(6px);
    box-shadow: 0 10px 40px rgba(0,80,200,0.25);
  }
  canvas{
    display:block;
    border-radius:50%;
    background:#000;
    box-shadow:
      0 0 60px rgba(80,160,255,0.45),
      inset -20px -20px 60px rgba(0,0,0,0.7),
      inset 10px 10px 30px rgba(150,210,255,0.15);
  }
  .controls{
    display:flex; flex-wrap:wrap; gap:10px; justify-content:center;
    margin-top:18px;
  }
  button{
    padding:10px 16px;
    border:none; border-radius:10px;
    background: linear-gradient(135deg,#2a7bff,#00c2ff);
    color:#fff; font-weight:600; cursor:pointer;
    box-shadow:0 4px 14px rgba(42,123,255,0.4);
    transition: transform .15s ease;
  }
  button:hover{transform:translateY(-2px)}
  button.secondary{
    background: rgba(255,255,255,0.08);
    border:1px solid rgba(150,200,255,0.3);
    box-shadow:none;
  }
  .speed{
    display:flex; align-items:center; gap:8px;
    padding:8px 12px;
    background: rgba(255,255,255,0.06);
    border-radius:10px;
    border:1px solid rgba(150,200,255,0.2);
  }
  input[type=range]{accent-color:#3aa0ff}
  .status{min-height:20px; font-size:13px; color:#8ec8ff; margin-top:6px; text-align:center}
  .hint{font-size:12px; color:#7aa8d8; opacity:.8; text-align:center; max-width:420px}
</style>
</head>
<body>
  <h1>🌍 Animasi Bola Dunia Berputar</h1>
  <div class="wrap">
    <canvas id="bumi" width="360" height="360"></canvas>
    <div class="controls">
      <button id="toggle">⏸ Jeda</button>
      <button id="reset">⟲ Reset</button>
      <div class="speed">
        <span>Kecepatan</span>
        <input id="spd" type="range" min="0.1" max="3" step="0.1" value="1"/>
        <span id="spdVal">1.0x</span>
      </div>
      <button id="gif" class="secondary">📥 Download GIF</button>
    </div>
    <div class="status" id="status"></div>
  </div>
  <p class="hint">Klik <b>Download GIF</b> untuk menyimpan animasi berputar sebagai file .gif</p>

<script>
(() => {
  const canvas = document.getElementById('bumi');
  const ctx = canvas.getContext('2d');
  const W = canvas.width, H = canvas.height, R = W/2 - 6;
  const CX = W/2, CY = H/2;

  // ---------- Peta benua (vector sederhana, dalam longitude [-180,180], latitude [-90,90]) ----------
  // Setiap benua = array poligon (titik-titik lon,lat). Disesuaikan agar Afrika + Eropa + Amerika tampak jelas.
  const continents = [
    // Afrika
    [[-17,15],[-15,28],[-5,35],[10,37],[25,32],[33,30],[35,10],[40,5],[50,12],[51,-15],[43,-25],[35,-34],[20,-34],[12,-5],[8,5],[-5,5],[-15,5]],
    // Eropa
    [[-10,36],[0,43],[5,50],[10,60],[20,70],[30,70],[40,65],[50,60],[55,55],[45,45],[35,40],[25,35],[15,37],[5,36]],
    // Timur Tengah + Arab
    [[35,30],[45,30],[55,25],[58,15],[50,12],[45,15],[40,20]],
    // Amerika Utara
    [[-168,66],[-140,70],[-100,74],[-60,55],[-55,45],[-65,25],[-80,25],[-85,30],[-90,20],[-105,22],[-118,32],[-125,40],[-130,55],[-155,58],[-165,60]],
    // Amerika Selatan
    [[-80,12],[-70,12],[-50,5],[-35,-5],[-38,-20],[-45,-35],[-55,-45],[-65,-55],[-72,-50],[-75,-35],[-78,-20],[-80,-5]],
    // Asia (sebagian kiri agar terlihat saat rotasi)
    [[55,70],[70,75],[90,70],[100,60],[110,50],[105,40],[95,30],[80,25],[70,30],[60,45],[55,55]],
    // Australia
    [[115,-20],[130,-12],[145,-15],[153,-25],[150,-35],[135,-38],[120,-34],[114,-28]],
    // Greenland
    [[-50,82],[-30,83],[-20,78],[-25,70],[-40,68],[-52,72]],
    // Antartika (strip bawah)
    [[-180,-65],[-90,-70],[0,-72],[90,-70],[180,-65],[180,-85],[-180,-85]]
  ];

  // ---------- Bintang latar ----------
  const stars = Array.from({length:140}, () => ({
    x: Math.random()*W, y: Math.random()*H,
    r: Math.random()*1.3 + 0.2,
    a: Math.random()*0.7 + 0.3
  }));

  let angle = 0;        // rotasi bujur (yaw)
  let tilt = 0.41;      // kemiringan sumbu ~23.5°
  let speed = 1;
  let running = true;
  let rafId = null;

  // ---------- Proyeksi 3D -> 2D ----------
  function project(lonDeg, latDeg, rotY){
    const lon = lonDeg * Math.PI/180 + rotY;
    const lat = latDeg * Math.PI/180;
    // koordinat bola satuan (rotasi Y)
    let x = Math.cos(lat) * Math.cos(lon);
    let y = Math.sin(lat);
    let z = Math.cos(lat) * Math.sin(lon);
    // kemiringan sumbu (rotasi X)
    const cosT = Math.cos(tilt), sinT = Math.sin(tilt);
    const y2 = y * cosT - z * sinT;
    const z2 = y * sinT + z * cosT;
    return { x, y:y2, z:z2 };
  }

  function lonLatToXY(lon, lat, rotY){
    const p = project(lon, lat, rotY);
    if (p.z < -0.02) return null; // titik di sisi belakang
    return {
      x: CX + p.x * R,
      y: CY - p.y * R,
      z: p.z
    };
  }

  // ---------- Gambar 1 frame ----------
  function drawFrame(){
    // 1. Latar hitam + bintang
    ctx.fillStyle = '#000010';
    ctx.fillRect(0,0,W,H);
    for(const s of stars){
      ctx.globalAlpha = s.a * (0.6 + 0.4*Math.sin(performance.now()/800 + s.x));
      ctx.fillStyle = '#cfe3ff';
      ctx.beginPath();
      ctx.arc(s.x, s.y, s.r, 0, Math.PI*2);
      ctx.fill();
    }
    ctx.globalAlpha = 1;

    // 2. Gradien samudra + atmosfer
    const grad = ctx.createRadialGradient(CX-R*0.35, CY-R*0.4, R*0.1, CX, CY, R);
    grad.addColorStop(0, '#4fb3ff');
    grad.addColorStop(0.55, '#1565c0');
    grad.addColorStop(0.9, '#062a66');
    grad.addColorStop(1, '#021030');
    ctx.fillStyle = grad;
    ctx.beginPath();
    ctx.arc(CX, CY, R, 0, Math.PI*2);
    ctx.fill();

    // 3. Awan tipis (garis-garis acak yang konsisten per sudut)
    ctx.save();
    ctx.globalAlpha = 0.18;
    ctx.fillStyle = '#ffffff';
    const cloudSeed = Math.floor(angle*10);
    for(let i=0;i<14;i++){
      const clon = ((i*47 + cloudSeed*3) % 360) - 180;
      const clat = Math.sin(i*1.9)*35;
      const p = lonLatToXY(clon, clat, angle);
      if(!p) continue;
      ctx.beginPath();
      ctx.ellipse(p.x, p.y, 14 + (i%3)*6, 4 + (i%2)*2, i*0.3, 0, Math.PI*2);
      ctx.fill();
    }
    ctx.restore();

    // 4. Benua
    for(const poly of continents){
      const pts = [];
      let frontCount = 0;
      for(const [lon,lat] of poly){
        const p = lonLatToXY(lon, lat, angle);
        pts.push(p);
        if(p) frontCount++;
      }
      if(frontCount < 2) continue; // terlalu banyak di belakang, skip

      ctx.beginPath();
      let moved = false;
      for(let i=0;i<pts.length;i++){
        const p = pts[i];
        if(!p){ moved = false; continue; }
        // jika lompat dari belakang ke depan -> pindah pena
        const prev = pts[i===0?pts.length-1:i-1];
        if(!prev || !moved){
          ctx.moveTo(p.x, p.y);
          moved = true;
        } else {
          // hindari garis memotong bola jika jarak terlalu jauh
          const dx = p.x - prev.x, dy = p.y - prev.y;
          if(dx*dx + dy*dy < R*R*0.6){
            ctx.lineTo(p.x, p.y);
          } else {
            ctx.moveTo(p.x, p.y);
          }
        }
      }
      ctx.closePath();

      // gradien hijau kecoklatan + shading sesuai z
      const centerZ = pts.filter(Boolean).reduce((a,b)=>a+b.z,0) / Math.max(1,pts.filter(Boolean).length);
      const shade = Math.max(0.25, Math.min(1, 0.55 + centerZ*0.7));
      const landGrad = ctx.createLinearGradient(CX-R, CY, CX+R, CY);
      landGrad.addColorStop(0, `rgba(60,130,70,${shade})`);
      landGrad.addColorStop(0.5, `rgba(90,160,75,${shade})`);
      landGrad.addColorStop(1, `rgba(120,150,90,${shade*0.9})`);
      ctx.fillStyle = landGrad;
      ctx.fill();
      ctx.strokeStyle = `rgba(30,70,40,${0.5*shade})`;
      ctx.lineWidth = 0.8;
      ctx.stroke();
    }

    // 5. Shading malam (bagian kanan gelap)
    const night = ctx.createRadialGradient(CX+R*0.55, CY, R*0.1, CX, CY, R);
    night.addColorStop(0, 'rgba(0,0,0,0)');
    night.addColorStop(0.55, 'rgba(0,0,10,0.35)');
    night.addColorStop(1, 'rgba(0,0,20,0.78)');
    ctx.fillStyle = night;
    ctx.beginPath();
    ctx.arc(CX, CY, R, 0, Math.PI*2);
    ctx.fill();

    // 6. Sorotan cahaya (kiri atas)
    const hl = ctx.createRadialGradient(CX-R*0.45, CY-R*0.5, 2, CX, CY, R);
    hl.addColorStop(0, 'rgba(255,255,255,0.35)');
    hl.addColorStop(0.3, 'rgba(180,230,255,0.12)');
    hl.addColorStop(1, 'rgba(0,0,0,0)');
    ctx.fillStyle = hl;
    ctx.beginPath();
    ctx.arc(CX, CY, R, 0, Math.PI*2);
    ctx.fill();

    // 7. Lingkar atmosfer luar
    ctx.strokeStyle = 'rgba(120,200,255,0.55)';
    ctx.lineWidth = 2;
    ctx.beginPath();
    ctx.arc(CX, CY, R+2, 0, Math.PI*2);
    ctx.stroke();
  }

  function loop(){
    if(running) angle += 0.008 * speed;
    drawFrame();
    rafId = requestAnimationFrame(loop);
  }
  loop();

  // ---------- Kontrol UI ----------
  const toggleBtn = document.getElementById('toggle');
  toggleBtn.onclick = () => {
    running = !running;
    toggleBtn.textContent = running ? '⏸ Jeda' : '▶ Putar';
  };
  document.getElementById('reset').onclick = () => { angle = 0; };
  const spd = document.getElementById('spd');
  const spdVal = document.getElementById('spdVal');
  spd.oninput = () => { speed = parseFloat(spd.value); spdVal.textContent = speed.toFixed(1)+'x'; };

  // ---------- Download GIF ----------
  const statusEl = document.getElementById('status');
  document.getElementById('gif').onclick = async () => {
    if(typeof GIF === 'undefined'){
      statusEl.textContent = '⚠ Gagal memuat library GIF, coba lagi.';
      return;
    }
    const frames = 48;      // 1 putaran penuh = 48 frame
    const delay  = 1000/24; // 24 FPS
    const gif = new GIF({
      workers: 2,
      quality: 8,
      width: W,
      height: H,
      workerScript: 'https://cdn.jsdelivr.net/npm/gif.js@0.2.0/dist/gif.worker.js'
    });

    statusEl.textContent = '🔄 Membuat GIF... 0%';
    const startAngle = angle;
    const wasRunning = running;
    running = false;

    for(let i=0;i<frames;i++){
      angle = startAngle + (i/frames) * Math.PI * 2;
      drawFrame();
      gif.addFrame(ctx, {copy:true, delay});
      statusEl.textContent = `🔄 Membuat GIF... ${Math.round((i+1)/frames*100)}%`;
      await new Promise(r => setTimeout(r, 10));
    }

    running = wasRunning;
    gif.on('finished', blob => {
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = 'bola-dunia-bergerak.gif';
      document.body.appendChild(a);
      a.click();
      a.remove();
      URL.revokeObjectURL(url);
      statusEl.textContent = '✅ GIF berhasil di-download!';
      setTimeout(()=>statusEl.textContent='', 3500);
    });
    gif.render();
  };
})();
</script>
</body>
</html>
'''

path = '/mnt/bola_dunia_animasi.html'
with open(path, 'w', encoding='utf-8') as f:
    f.write(html_content)
print('File tersimpan di:', path)
print('Ukuran:', len(html_content), 'karakter')

