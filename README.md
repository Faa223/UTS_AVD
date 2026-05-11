<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Laporan EDA — Retail Sales Dataset</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,700;0,900;1,700&family=IBM+Plex+Mono:wght@400;500&family=Plus+Jakarta+Sans:wght@400;500;600;700&display=swap" rel="stylesheet"/>
<style>
:root {
  --bg:        #faf8f4;
  --surface:   #ffffff;
  --card:      #f4f1eb;
  --border:    #e2ddd4;
  --ink:       #1a1714;
  --muted:     #7a7268;
  --accent:    #c84b2f;
  --accent2:   #2f6fc8;
  --accent3:   #2a9d6e;
  --accent4:   #d4a017;
  --beauty:    #c84b2f;
  --clothing:  #2f6fc8;
  --elec:      #2a9d6e;
}
 
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
 
body {
  background: var(--bg);
  color: var(--ink);
  font-family: 'Plus Jakarta Sans', sans-serif;
  line-height: 1.7;
  font-size: 15px;
}
 
/* ─── COVER ─── */
.cover {
  min-height: 100vh;
  display: grid;
  grid-template-rows: 1fr auto;
  padding: 0;
  background: var(--ink);
  color: #faf8f4;
  position: relative;
  overflow: hidden;
}
.cover-noise {
  position: absolute; inset: 0;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.04'/%3E%3C/svg%3E");
  pointer-events: none;
  opacity: .6;
}
.cover-accent-bar {
  position: absolute; top: 0; left: 0; right: 0; height: 4px;
  background: linear-gradient(90deg, var(--accent) 0%, var(--accent4) 50%, var(--accent2) 100%);
}
.cover-main {
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 80px 80px 60px;
  max-width: 960px;
}
.cover-tag {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 11px;
  letter-spacing: .22em;
  text-transform: uppercase;
  color: var(--accent4);
  margin-bottom: 32px;
  display: flex;
  align-items: center;
  gap: 12px;
}
.cover-tag::before {
  content: '';
  display: inline-block;
  width: 24px; height: 1px;
  background: var(--accent4);
}
.cover h1 {
  font-family: 'Playfair Display', serif;
  font-size: clamp(48px, 7vw, 88px);
  line-height: 1.0;
  font-weight: 900;
  letter-spacing: -.02em;
  margin-bottom: 32px;
}
.cover h1 .highlight {
  color: transparent;
  -webkit-text-stroke: 1.5px #faf8f4;
}
.cover h1 .red { color: var(--accent); -webkit-text-stroke: 0; }
.cover-desc {
  font-size: 16px;
  color: rgba(250,248,244,.6);
  max-width: 520px;
  line-height: 1.8;
  margin-bottom: 48px;
}
.cover-chips {
  display: flex; flex-wrap: wrap; gap: 10px;
}
.chip {
  border: 1px solid rgba(250,248,244,.2);
  border-radius: 4px;
  padding: 6px 14px;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 11px;
  letter-spacing: .08em;
  color: rgba(250,248,244,.65);
}
.chip b { color: #faf8f4; }
.cover-footer {
  border-top: 1px solid rgba(255,255,255,.08);
  padding: 28px 80px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 11px;
  color: rgba(250,248,244,.35);
  flex-wrap: wrap; gap: 8px;
}
.cover-grid-lines {
  position: absolute;
  top: 0; right: 0; bottom: 0;
  width: 45%;
  background-image:
    linear-gradient(rgba(255,255,255,.04) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255,255,255,.04) 1px, transparent 1px);
  background-size: 60px 60px;
  pointer-events: none;
}
.cover-circle {
  position: absolute;
  right: -120px; top: 50%;
  transform: translateY(-50%);
  width: 500px; height: 500px;
  border-radius: 50%;
  border: 1px solid rgba(255,255,255,.06);
  pointer-events: none;
}
.cover-circle::before {
  content: '';
  position: absolute;
  inset: 60px;
  border-radius: 50%;
  border: 1px solid rgba(255,255,255,.05);
}
 
/* ─── NAV (sticky TOC) ─── */
.toc-nav {
  position: sticky; top: 0; z-index: 100;
  background: rgba(250,248,244,.92);
  backdrop-filter: blur(12px);
  border-bottom: 1px solid var(--border);
  padding: 0 60px;
  display: flex;
  gap: 0;
  overflow-x: auto;
}
.toc-nav a {
  text-decoration: none;
  color: var(--muted);
  font-size: 12px;
  font-weight: 600;
  letter-spacing: .04em;
  padding: 16px 20px;
  border-bottom: 2px solid transparent;
  white-space: nowrap;
  transition: color .2s, border-color .2s;
}
.toc-nav a:hover { color: var(--accent); border-bottom-color: var(--accent); }
 
/* ─── MAIN LAYOUT ─── */
.page { max-width: 1000px; margin: 0 auto; padding: 0 60px; }
 
section {
  padding: 80px 0;
  border-bottom: 1px solid var(--border);
}
section:last-of-type { border-bottom: none; }
 
.sec-number {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 11px;
  letter-spacing: .18em;
  text-transform: uppercase;
  color: var(--accent);
  margin-bottom: 8px;
}
.sec-title {
  font-family: 'Playfair Display', serif;
  font-size: clamp(28px, 4vw, 44px);
  line-height: 1.1;
  font-weight: 700;
  margin-bottom: 40px;
}
.sec-title em { color: var(--accent); font-style: italic; }
 
/* ─── STEP BLOCKS (how analysis was done) ─── */
.steps { display: flex; flex-direction: column; gap: 0; }
.step {
  display: grid;
  grid-template-columns: 80px 1fr;
  gap: 0 32px;
  position: relative;
}
.step:not(:last-child)::after {
  content: '';
  position: absolute;
  left: 39px; top: 52px; bottom: 0;
  width: 1px;
  background: var(--border);
}
.step-num {
  width: 40px; height: 40px;
  border-radius: 50%;
  border: 1.5px solid var(--ink);
  display: flex; align-items: center; justify-content: center;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 12px;
  font-weight: 500;
  flex-shrink: 0;
  background: var(--bg);
  position: relative; z-index: 1;
}
.step-body { padding-bottom: 48px; }
.step-label {
  font-size: 13px;
  font-weight: 700;
  letter-spacing: .04em;
  margin-bottom: 6px;
  margin-top: 8px;
}
.step-text { font-size: 14px; color: var(--muted); line-height: 1.75; }
 
/* ─── CODE BLOCK ─── */
.code-block {
  background: var(--ink);
  color: #e8e4dc;
  border-radius: 8px;
  padding: 20px 24px;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 12.5px;
  line-height: 1.7;
  margin: 16px 0;
  overflow-x: auto;
}
.code-block .kw  { color: #c47b5a; }
.code-block .fn  { color: #7dba8a; }
.code-block .st  { color: #e8c547; }
.code-block .cm  { color: #5a6478; font-style: italic; }
.code-block .var { color: #9ab4f0; }
 
/* ─── FINDING CARDS ─── */
.findings { display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 16px; margin: 32px 0; }
.finding {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 10px;
  padding: 24px;
  position: relative;
  overflow: hidden;
  transition: transform .2s, box-shadow .2s;
}
.finding:hover { transform: translateY(-3px); box-shadow: 0 8px 24px rgba(0,0,0,.08); }
.finding::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 3px;
  background: var(--find-color, var(--accent));
}
.finding-icon { font-size: 28px; margin-bottom: 12px; }
.finding-title { font-weight: 700; font-size: 14px; margin-bottom: 8px; }
.finding-text { font-size: 13px; color: var(--muted); line-height: 1.65; }
.finding-stat {
  margin-top: 14px;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 22px;
  font-weight: 500;
  color: var(--find-color, var(--accent));
}
 
/* ─── VISUAL CHART (pure CSS/SVG placeholders with real data labels) ─── */
.chart-container {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 28px 28px 20px;
  margin: 24px 0;
}
.chart-header { margin-bottom: 20px; }
.chart-title { font-weight: 700; font-size: 14px; }
.chart-subtitle { font-size: 12px; color: var(--muted); margin-top: 3px; }
 
/* Horizontal bar chart */
.hbar { display: flex; flex-direction: column; gap: 14px; }
.hbar-row { display: grid; grid-template-columns: 130px 1fr 80px; gap: 12px; align-items: center; font-size: 13px; }
.hbar-label { color: var(--muted); text-align: right; font-family: 'IBM Plex Mono', monospace; font-size: 12px; }
.hbar-track { height: 10px; background: var(--card); border-radius: 99px; overflow: hidden; }
.hbar-fill { height: 100%; border-radius: 99px; }
.hbar-val { font-family: 'IBM Plex Mono', monospace; font-size: 12px; font-weight: 500; }
 
/* Donut (CSS) */
.donut-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 32px; align-items: center; }
.donut-svg { width: 160px; height: 160px; }
.legend-list { display: flex; flex-direction: column; gap: 14px; }
.legend-row { display: flex; align-items: center; gap: 12px; font-size: 13px; }
.legend-swatch { width: 12px; height: 12px; border-radius: 3px; flex-shrink: 0; }
.legend-key { font-weight: 600; }
.legend-val { color: var(--muted); font-family: 'IBM Plex Mono', monospace; font-size: 12px; }
 
/* Grid of mini stats */
.stat-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 16px; margin: 24px 0; }
.stat-box {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 10px;
  padding: 20px;
  text-align: center;
}
.stat-box .num {
  font-family: 'Playfair Display', serif;
  font-size: 36px;
  font-weight: 700;
  line-height: 1;
  color: var(--sb-color, var(--accent));
  margin-bottom: 6px;
}
.stat-box .lbl { font-size: 12px; color: var(--muted); font-weight: 600; letter-spacing: .04em; }
.stat-box .sub { font-size: 11px; color: var(--border-color, var(--border)); margin-top: 4px; font-family: 'IBM Plex Mono', monospace; color: var(--muted); }
 
/* ─── TABLE ─── */
.data-table { width: 100%; border-collapse: collapse; font-size: 13px; }
.data-table th {
  text-align: left;
  padding: 10px 16px;
  background: var(--card);
  color: var(--muted);
  font-family: 'IBM Plex Mono', monospace;
  font-size: 10px;
  letter-spacing: .14em;
  text-transform: uppercase;
  border-bottom: 2px solid var(--border);
}
.data-table td {
  padding: 13px 16px;
  border-bottom: 1px solid var(--border);
  vertical-align: middle;
}
.data-table tr:last-child td { border-bottom: none; }
.data-table tr:hover td { background: var(--card); }
 
/* Badges */
.badge {
  display: inline-block;
  padding: 3px 10px;
  border-radius: 20px;
  font-size: 11px;
  font-weight: 600;
  font-family: 'IBM Plex Mono', monospace;
}
.b-beauty    { background: rgba(200,75,47,.1);  color: var(--beauty);   }
.b-clothing  { background: rgba(47,111,200,.1); color: var(--clothing); }
.b-elec      { background: rgba(42,157,110,.1); color: var(--elec);     }
.b-ok        { background: rgba(42,157,110,.1); color: var(--elec);     }
.b-warn      { background: rgba(212,160,23,.1); color: var(--accent4);  }
 
/* ─── INSIGHT PULL QUOTES ─── */
.pullquote {
  border-left: 3px solid var(--accent);
  padding: 16px 24px;
  margin: 24px 0;
  background: var(--card);
  border-radius: 0 8px 8px 0;
  font-size: 14px;
  color: var(--muted);
  line-height: 1.75;
}
.pullquote strong { color: var(--ink); }
 
/* ─── TWO-COL ─── */
.two-col { display: grid; grid-template-columns: 1fr 1fr; gap: 24px; }
@media(max-width: 700px) { .two-col { grid-template-columns: 1fr; } }
 
/* ─── CONCLUSION NUMBERED LIST ─── */
.conclusion-list { display: flex; flex-direction: column; gap: 20px; counter-reset: cl; }
.cl-item {
  display: grid;
  grid-template-columns: 48px 1fr;
  gap: 16px;
  align-items: start;
  padding: 20px;
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 10px;
  transition: border-color .2s;
}
.cl-item:hover { border-color: var(--accent); }
.cl-num {
  font-family: 'Playfair Display', serif;
  font-size: 28px;
  font-weight: 700;
  color: var(--border);
  line-height: 1;
  padding-top: 2px;
}
.cl-title { font-weight: 700; margin-bottom: 6px; }
.cl-body { font-size: 13px; color: var(--muted); line-height: 1.65; }
 
/* ─── FOOTER ─── */
.footer {
  background: var(--ink);
  color: rgba(250,248,244,.4);
  padding: 40px 60px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 12px;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 11px;
}
.footer a { color: rgba(250,248,244,.6); text-decoration: none; }
 
/* ─── ANIMATIONS ─── */
.reveal { opacity: 0; transform: translateY(28px); transition: opacity .6s ease, transform .6s ease; }
.reveal.in { opacity: 1; transform: translateY(0); }
 
/* ─── DIVIDER ─── */
.divider {
  height: 1px; background: var(--border);
  margin: 32px 0;
  position: relative;
}
.divider::before {
  content: '◆';
  position: absolute;
  left: 50%; top: 50%;
  transform: translate(-50%, -50%);
  background: var(--bg);
  padding: 0 12px;
  font-size: 8px;
  color: var(--border);
}
 
/* month bar chart */
.month-bars { display: flex; gap: 6px; align-items: flex-end; height: 120px; }
.mb-col { display: flex; flex-direction: column; align-items: center; gap: 6px; flex: 1; }
.mb-bar {
  width: 100%;
  border-radius: 4px 4px 0 0;
  min-height: 4px;
  transition: opacity .2s;
}
.mb-bar:hover { opacity: .75; }
.mb-label { font-family: 'IBM Plex Mono', monospace; font-size: 9px; color: var(--muted); }
</style>
</head>
<body>
 
<!-- ══ COVER ══ -->
<div class="cover">
  <div class="cover-noise"></div>
  <div class="cover-accent-bar"></div>
  <div class="cover-grid-lines"></div>
  <div class="cover-circle"></div>
 
  <div class="cover-main">
    <div class="cover-tag">Laporan Analisis Data · AVD 2026</div>
    <h1>
      Retail<br/>
      <span class="highlight">Sales</span><br/>
      <span class="red">Dataset</span>
    </h1>
    <p class="cover-desc">
      Laporan Exploratory Data Analysis (EDA) terhadap dataset transaksi retail dari Kaggle.
      Mencakup pemahaman struktur data, distribusi variabel, segmentasi pelanggan,
      tren temporal, dan rekomendasi bisnis berbasis data.
    </p>
    <div class="cover-chips">
      <div class="chip"><b>Sumber</b> · Kaggle — mohammadtalib786</div>
      <div class="chip"><b>Baris</b> · 1.000 transaksi</div>
      <div class="chip"><b>Kolom</b> · 9 fitur</div>
      <div class="chip"><b>Periode</b> · 2023</div>
      <div class="chip"><b>Tools</b> · Python · Pandas · Seaborn · Matplotlib</div>
    </div>
  </div>
 
  <div class="cover-footer">
    <span>EDA_RetailSalesDataset.ipynb</span>
    <span>Analisis Visual & Deskriptif</span>
    <span>2026</span>
  </div>
</div>
 
<!-- ══ STICKY NAV ══ -->
<nav class="toc-nav">
  <a href="#overview">Overview</a>
  <a href="#data-understanding">Data Understanding</a>
  <a href="#proses-eda">Proses EDA</a>
  <a href="#temuan">Temuan Utama</a>
  <a href="#temporal">Analisis Temporal</a>
  <a href="#segmentasi">Segmentasi</a>
  <a href="#kategori">Kategori Produk</a>
  <a href="#kesimpulan">Kesimpulan</a>
</nav>
 
<!-- ══ 1. OVERVIEW ══ -->
<section id="overview">
  <div class="page reveal">
    <div class="sec-number">01 — Overview</div>
    <div class="sec-title">Latar Belakang &amp; <em>Tujuan</em></div>
 
    <p style="color:var(--muted);max-width:680px;margin-bottom:32px;line-height:1.85;font-size:15px;">
      Dataset <strong>Retail Sales</strong> dari Kaggle merekam 1.000 transaksi toko retail sepanjang tahun 2023.
      Data ini mencakup informasi demografis pelanggan (gender, usia), kategori produk yang dibeli,
      kuantitas, harga satuan, dan total nilai transaksi. EDA ini bertujuan menghasilkan
      wawasan yang dapat mendukung pengambilan keputusan bisnis di bidang retail.
    </p>
 
    <div class="stat-grid">
      <div class="stat-box" style="--sb-color: var(--accent)">
        <div class="num">1K</div>
        <div class="lbl">Total Transaksi</div>
        <div class="sub">Sepanjang 2023</div>
      </div>
      <div class="stat-box" style="--sb-color: var(--accent3)">
        <div class="num">9</div>
        <div class="lbl">Kolom / Fitur</div>
        <div class="sub">Numerik &amp; kategorikal</div>
      </div>
      <div class="stat-box" style="--sb-color: var(--accent2)">
        <div class="num">3</div>
        <div class="lbl">Kategori Produk</div>
        <div class="sub">Beauty · Clothing · Electronics</div>
      </div>
      <div class="stat-box" style="--sb-color: var(--accent4)">
        <div class="num">$456K</div>
        <div class="lbl">Total Revenue</div>
        <div class="sub">Seluruh periode 2023</div>
      </div>
      <div class="stat-box" style="--sb-color: var(--accent)">
        <div class="num">0</div>
        <div class="lbl">Missing Values</div>
        <div class="sub">Dataset bersih sempurna</div>
      </div>
      <div class="stat-box" style="--sb-color: var(--accent3)">
        <div class="num">$456</div>
        <div class="lbl">Rata-rata Transaksi</div>
        <div class="sub">Mean Total Amount</div>
      </div>
    </div>
 
    <div class="pullquote">
      <strong>Tujuan Analisis:</strong> Memahami pola pembelian, mengidentifikasi segmen pelanggan utama,
      menemukan tren temporal penjualan, dan menganalisis kontribusi tiap kategori produk
      terhadap total revenue — guna mendukung strategi bisnis yang lebih terarah.
    </div>
  </div>
</section>
 
<!-- ══ 2. DATA UNDERSTANDING ══ -->
<section id="data-understanding">
  <div class="page reveal">
    <div class="sec-number">02 — Data Understanding</div>
    <div class="sec-title">Memahami <em>Struktur Data</em></div>
 
    <p style="color:var(--muted);margin-bottom:28px;font-size:14px;line-height:1.8;">
      Langkah pertama adalah memuat dataset dan memverifikasi struktur, tipe data, serta
      kualitasnya sebelum melakukan analisis lebih lanjut.
    </p>
 
    <div class="code-block">
<span class="cm"># Load dataset dan tampilkan info dasar</span>
<span class="kw">import</span> <span class="var">pandas</span> <span class="kw">as</span> <span class="var">pd</span>
<span class="kw">import</span> <span class="var">numpy</span> <span class="kw">as</span> <span class="var">np</span>
<span class="kw">import</span> <span class="var">matplotlib.pyplot</span> <span class="kw">as</span> <span class="var">plt</span>
<span class="kw">import</span> <span class="var">seaborn</span> <span class="kw">as</span> <span class="var">sns</span>
 
<span class="var">df</span> = <span class="var">pd</span>.<span class="fn">read_csv</span>(<span class="st">"retail_sales_dataset.csv"</span>)
<span class="fn">print</span>(<span class="st">"Shape:"</span>, <span class="var">df</span>.<span class="var">shape</span>)   <span class="cm"># (1000, 9)</span>
<span class="var">df</span>.<span class="fn">info</span>()
    </div>
 
    <div class="chart-container">
      <div class="chart-header">
        <div class="chart-title">Deskripsi Kolom Dataset</div>
        <div class="chart-subtitle">Nama kolom, tipe data, kelengkapan, dan keterangan</div>
      </div>
      <table class="data-table">
        <thead>
          <tr><th>#</th><th>Nama Kolom</th><th>Tipe Data</th><th>Non-Null</th><th>Keterangan</th></tr>
        </thead>
        <tbody>
          <tr><td>1</td><td><code>Transaction ID</code></td><td><span class="badge b-elec">int64</span></td><td>1000 ✓</td><td>ID unik setiap transaksi</td></tr>
          <tr><td>2</td><td><code>Date</code></td><td><span class="badge b-warn">object → datetime</span></td><td>1000 ✓</td><td>Tanggal transaksi (perlu konversi)</td></tr>
          <tr><td>3</td><td><code>Customer ID</code></td><td><span class="badge b-clothing">object</span></td><td>1000 ✓</td><td>ID unik pelanggan</td></tr>
          <tr><td>4</td><td><code>Gender</code></td><td><span class="badge b-clothing">object</span></td><td>1000 ✓</td><td>Male / Female</td></tr>
          <tr><td>5</td><td><code>Age</code></td><td><span class="badge b-elec">int64</span></td><td>1000 ✓</td><td>Usia pelanggan (18–64 tahun)</td></tr>
          <tr><td>6</td><td><code>Product Category</code></td><td><span class="badge b-clothing">object</span></td><td>1000 ✓</td><td>Beauty / Clothing / Electronics</td></tr>
          <tr><td>7</td><td><code>Quantity</code></td><td><span class="badge b-elec">int64</span></td><td>1000 ✓</td><td>Jumlah unit yang dibeli (1–4)</td></tr>
          <tr><td>8</td><td><code>Price per Unit</code></td><td><span class="badge b-elec">int64</span></td><td>1000 ✓</td><td>Harga satuan: $25 / $30 / $50 / $300 / $500</td></tr>
          <tr><td>9</td><td><code>Total Amount</code></td><td><span class="badge b-elec">int64</span></td><td>1000 ✓</td><td>Total transaksi = Quantity × Price per Unit</td></tr>
        </tbody>
      </table>
    </div>
 
    <div class="two-col" style="margin-top:20px;">
      <div class="chart-container">
        <div class="chart-header">
          <div class="chart-title">Verifikasi Kualitas Data</div>
        </div>
        <table class="data-table">
          <thead><tr><th>Pemeriksaan</th><th>Hasil</th><th>Status</th></tr></thead>
          <tbody>
            <tr><td>Missing Values</td><td>0 dari 9 kolom</td><td><span class="badge b-ok">✓ Bersih</span></td></tr>
            <tr><td>Duplikat</td><td>0 baris</td><td><span class="badge b-ok">✓ Bersih</span></td></tr>
            <tr><td>Inkonsistensi Gender</td><td>Hanya Male / Female</td><td><span class="badge b-ok">✓ Konsisten</span></td></tr>
            <tr><td>Inkonsistensi Category</td><td>3 nilai baku</td><td><span class="badge b-ok">✓ Konsisten</span></td></tr>
            <tr><td>Tipe Date</td><td>object → perlu konversi</td><td><span class="badge b-warn">⚠ Perlu Parsing</span></td></tr>
          </tbody>
        </table>
      </div>
 
      <div class="chart-container">
        <div class="chart-header">
          <div class="chart-title">Statistik Deskriptif</div>
        </div>
        <table class="data-table">
          <thead><tr><th>Stat</th><th>Age</th><th>Price/Unit</th><th>Total Amt</th></tr></thead>
          <tbody>
            <tr><td>Mean</td><td>41.39</td><td>$179.89</td><td>$456.00</td></tr>
            <tr><td>Std</td><td>13.68</td><td>$189.68</td><td>$560.00</td></tr>
            <tr><td>Min</td><td>18</td><td>$25</td><td>$25</td></tr>
            <tr><td>Median</td><td>42</td><td>$50</td><td>$135</td></tr>
            <tr><td>Max</td><td>64</td><td>$500</td><td>$2.000</td></tr>
          </tbody>
        </table>
        <div class="pullquote" style="margin-top:16px;font-size:13px;">
          <strong>Catatan:</strong> Mean Total Amount ($456) jauh di atas median ($135) — indikasi distribusi <em>right-skewed</em> akibat produk mahal seperti Electronics.
        </div>
      </div>
    </div>
  </div>
</section>
 
<!-- ══ 3. PROSES EDA ══ -->
<section id="proses-eda">
  <div class="page reveal">
    <div class="sec-number">03 — Proses EDA</div>
    <div class="sec-title">Tahapan <em>Analisis</em></div>
 
    <div class="steps">
 
      <div class="step">
        <div><div class="step-num">01</div></div>
        <div class="step-body">
          <div class="step-label">Parsing &amp; Preprocessing</div>
          <div class="step-text">Kolom <code>Date</code> dikonversi ke format datetime menggunakan <code>pd.to_datetime(df["Date"])</code>. Setelah itu diturunkan fitur turunan: <code>year</code>, <code>month</code>, <code>day</code>, dan <code>month_name</code> untuk keperluan analisis temporal.</div>
          <div class="code-block" style="margin-top:12px;">
<span class="var">df</span>[<span class="st">"Date"</span>] = <span class="var">pd</span>.<span class="fn">to_datetime</span>(<span class="var">df</span>[<span class="st">"Date"</span>])
<span class="var">df</span>[<span class="st">"month"</span>]      = <span class="var">df</span>[<span class="st">"Date"</span>].<span class="var">dt</span>.<span class="var">month</span>
<span class="var">df</span>[<span class="st">"month_name"</span>] = <span class="var">df</span>[<span class="st">"Date"</span>].<span class="var">dt</span>.<span class="fn">strftime</span>(<span class="st">"%b"</span>)
          </div>
        </div>
      </div>
 
      <div class="step">
        <div><div class="step-num">02</div></div>
        <div class="step-body">
          <div class="step-label">Analisis Distribusi</div>
          <div class="step-text">Histogram dan boxplot digunakan untuk memahami sebaran nilai <code>Total Amount</code>, <code>Age</code>, <code>Quantity</code>, dan <code>Price per Unit</code>. Ditemukan pola distribusi bimodal pada Total Amount akibat perbedaan harga antar kategori produk.</div>
          <div class="code-block" style="margin-top:12px;">
<span class="var">sns</span>.<span class="fn">histplot</span>(<span class="var">df</span>[<span class="st">"Total Amount"</span>], <span class="var">bins</span>=<span class="st">30</span>, <span class="var">kde</span>=<span class="kw">True</span>)
<span class="var">sns</span>.<span class="fn">boxplot</span>(<span class="var">y</span>=<span class="var">df</span>[<span class="st">"Price per Unit"</span>])   <span class="cm"># deteksi outlier</span>
          </div>
        </div>
      </div>
 
      <div class="step">
        <div><div class="step-num">03</div></div>
        <div class="step-body">
          <div class="step-label">Analisis Temporal</div>
          <div class="step-text">Transaksi dan total revenue diagregasi per bulan untuk mengidentifikasi pola musiman. Ditemukan bahwa Mei adalah bulan terbaik dan September adalah bulan terlemah sepanjang 2023.</div>
          <div class="code-block" style="margin-top:12px;">
<span class="var">sales_per_month</span> = <span class="var">df</span>.<span class="fn">groupby</span>(<span class="st">"month_name"</span>)[<span class="st">"Total Amount"</span>].<span class="fn">sum</span>()
<span class="var">sales_per_month</span>.<span class="fn">plot</span>(<span class="var">marker</span>=<span class="st">"o"</span>, <span class="var">color</span>=<span class="st">"darkorange"</span>)
          </div>
        </div>
      </div>
 
      <div class="step">
        <div><div class="step-num">04</div></div>
        <div class="step-body">
          <div class="step-label">Segmentasi Pelanggan</div>
          <div class="step-text">Pelanggan dianalisis berdasarkan Gender dan kelompok usia. Usia dibagi ke dalam 5 kelompok (18–25, 26–35, 36–45, 46–55, 56–64) menggunakan <code>pd.cut()</code> untuk melihat pola pembelian per segmen.</div>
          <div class="code-block" style="margin-top:12px;">
<span class="var">bins</span>   = [<span class="st">17</span>, <span class="st">25</span>, <span class="st">35</span>, <span class="st">45</span>, <span class="st">55</span>, <span class="st">65</span>]
<span class="var">labels</span> = [<span class="st">"18-25"</span>, <span class="st">"26-35"</span>, <span class="st">"36-45"</span>, <span class="st">"46-55"</span>, <span class="st">"56-64"</span>]
<span class="var">df</span>[<span class="st">"Age Group"</span>] = <span class="var">pd</span>.<span class="fn">cut</span>(<span class="var">df</span>[<span class="st">"Age"</span>], <span class="var">bins</span>=<span class="var">bins</span>, <span class="var">labels</span>=<span class="var">labels</span>)
          </div>
        </div>
      </div>
 
      <div class="step">
        <div><div class="step-num">05</div></div>
        <div class="step-body">
          <div class="step-label">Analisis Korelasi</div>
          <div class="step-text">Heatmap korelasi dibuat untuk fitur numerik: <code>Age</code>, <code>Quantity</code>, <code>Price per Unit</code>, dan <code>Total Amount</code>. Ditemukan korelasi kuat antara Price per Unit dan Total Amount (r = 0.89), sementara Age tidak berkorelasi signifikan dengan variabel lain.</div>
          <div class="code-block" style="margin-top:12px;">
<span class="var">corr</span> = <span class="var">df</span>[[<span class="st">"Age"</span>, <span class="st">"Quantity"</span>, <span class="st">"Price per Unit"</span>, <span class="st">"Total Amount"</span>]].<span class="fn">corr</span>()
<span class="var">sns</span>.<span class="fn">heatmap</span>(<span class="var">corr</span>, <span class="var">annot</span>=<span class="kw">True</span>, <span class="var">cmap</span>=<span class="st">"coolwarm"</span>, <span class="var">fmt</span>=<span class="st">".2f"</span>)
          </div>
        </div>
      </div>
 
    </div>
  </div>
</section>
 
<!-- ══ 4. TEMUAN UTAMA ══ -->
<section id="temuan">
  <div class="page reveal">
    <div class="sec-number">04 — Temuan Utama</div>
    <div class="sec-title">Apa yang <em>Ditemukan?</em></div>
 
    <div class="findings">
 
      <div class="finding" style="--find-color: var(--elec)">
        <div class="finding-icon">📊</div>
        <div class="finding-title">Dataset Bersih &amp; Lengkap</div>
        <div class="finding-text">Tidak ada missing values, duplikat, maupun inkonsistensi nilai pada seluruh 9 kolom dataset. Ini memungkinkan analisis langsung tanpa preprocessing besar.</div>
        <div class="finding-stat">0 Missing</div>
      </div>
 
      <div class="finding" style="--find-color: var(--accent)">
        <div class="finding-icon">📈</div>
        <div class="finding-title">Distribusi Right-Skewed</div>
        <div class="finding-text">Total Amount memiliki median $135 namun mean $456 — perbedaan besar ini disebabkan oleh transaksi Electronics bernilai tinggi yang menarik rata-rata ke atas.</div>
        <div class="finding-stat">$135 vs $456</div>
      </div>
 
      <div class="finding" style="--find-color: var(--accent4)">
        <div class="finding-icon">🗓️</div>
        <div class="finding-title">Puncak Penjualan di Mei</div>
        <div class="finding-text">Bulan Mei mencatat revenue tertinggi ($53.150) dengan 105 transaksi — 2,25× lebih besar dibanding September yang terendah ($23.620, 65 transaksi).</div>
        <div class="finding-stat">Mei = #1</div>
      </div>
 
      <div class="finding" style="--find-color: var(--clothing)">
        <div class="finding-icon">⚖️</div>
        <div class="finding-title">Gender Hampir Seimbang</div>
        <div class="finding-text">Female 51% (510 transaksi, $232.840) vs Male 49% (490 transaksi, $223.160). Selisih hanya 2,1% — tidak ada dominasi gender yang signifikan secara keseluruhan.</div>
        <div class="finding-stat">51% : 49%</div>
      </div>
 
      <div class="finding" style="--find-color: var(--elec)">
        <div class="finding-icon">⚡</div>
        <div class="finding-title">Electronics = Revenue Tertinggi</div>
        <div class="finding-text">Meski jumlah transaksinya paling sedikit (342), Electronics menghasilkan revenue tertinggi ($156.905) karena harga satuan premium $300–$500.</div>
        <div class="finding-stat">$156.905</div>
      </div>
 
      <div class="finding" style="--find-color: var(--accent)">
        <div class="finding-icon">👥</div>
        <div class="finding-title">Pelanggan Inti: 46–55 Tahun</div>
        <div class="finding-text">Kelompok usia 46–55 tahun paling aktif (229 transaksi) dan menghasilkan revenue tertinggi ($100.690) dibanding kelompok usia lainnya.</div>
        <div class="finding-stat">229 transaksi</div>
      </div>
 
    </div>
  </div>
</section>
 
<!-- ══ 5. TEMPORAL ══ -->
<section id="temporal">
  <div class="page reveal">
    <div class="sec-number">05 — Temporal Analysis</div>
    <div class="sec-title">Tren <em>Bulanan</em> 2023</div>
 
    <div class="chart-container">
      <div class="chart-header">
        <div class="chart-title">Total Penjualan per Bulan (Revenue)</div>
        <div class="chart-subtitle">Mei puncak tertinggi · September titik terendah · Q4 cenderung kuat</div>
      </div>
      <div class="month-bars">
        <!-- heights proportional to max 53150 -->
        <div class="mb-col"><div class="mb-bar" style="height:83.7%;background:var(--accent2);"></div><div class="mb-label">Jan</div></div>
        <div class="mb-col"><div class="mb-bar" style="height:82.9%;background:var(--accent2);"></div><div class="mb-label">Feb</div></div>
        <div class="mb-col"><div class="mb-bar" style="height:54.5%;background:var(--accent3);"></div><div class="mb-label">Mar</div></div>
        <div class="mb-col"><div class="mb-bar" style="height:63.7%;background:var(--accent2);"></div><div class="mb-label">Apr</div></div>
        <div class="mb-col"><div class="mb-bar" style="height:100%;background:var(--accent4);"></div><div class="mb-label">Mei</div></div>
        <div class="mb-col"><div class="mb-bar" style="height:69.1%;background:var(--accent2);"></div><div class="mb-label">Jun</div></div>
        <div class="mb-col"><div class="mb-bar" style="height:66.7%;background:var(--accent2);"></div><div class="mb-label">Jul</div></div>
        <div class="mb-col"><div class="mb-bar" style="height:69.5%;background:var(--accent2);"></div><div class="mb-label">Agu</div></div>
        <div class="mb-col"><div class="mb-bar" style="height:44.4%;background:var(--accent);"></div><div class="mb-label">Sep</div></div>
        <div class="mb-col"><div class="mb-bar" style="height:87.6%;background:var(--accent3);"></div><div class="mb-label">Okt</div></div>
        <div class="mb-col"><div class="mb-bar" style="height:65.7%;background:var(--accent2);"></div><div class="mb-label">Nov</div></div>
        <div class="mb-col"><div class="mb-bar" style="height:84.1%;background:var(--accent3);"></div><div class="mb-label">Des</div></div>
      </div>
    </div>
 
    <div class="two-col">
      <div class="chart-container">
        <div class="chart-header">
          <div class="chart-title">5 Bulan Revenue Tertinggi</div>
        </div>
        <div class="hbar">
          <div class="hbar-row"><div class="hbar-label">Mei</div><div class="hbar-track"><div class="hbar-fill" style="width:100%;background:var(--accent4);"></div></div><div class="hbar-val" style="color:var(--accent4)">$53.150</div></div>
          <div class="hbar-row"><div class="hbar-label">Oktober</div><div class="hbar-track"><div class="hbar-fill" style="width:87.6%;background:var(--accent3);"></div></div><div class="hbar-val">$46.580</div></div>
          <div class="hbar-row"><div class="hbar-label">Desember</div><div class="hbar-track"><div class="hbar-fill" style="width:84.1%;background:var(--accent2);"></div></div><div class="hbar-val">$44.690</div></div>
          <div class="hbar-row"><div class="hbar-label">Februari</div><div class="hbar-track"><div class="hbar-fill" style="width:82.9%;background:var(--accent2);"></div></div><div class="hbar-val">$44.060</div></div>
          <div class="hbar-row"><div class="hbar-label">Agustus</div><div class="hbar-track"><div class="hbar-fill" style="width:69.5%;background:var(--accent2);"></div></div><div class="hbar-val">$36.960</div></div>
        </div>
      </div>
      <div class="chart-container">
        <div class="chart-header">
          <div class="chart-title">Jumlah Transaksi per Bulan</div>
        </div>
        <div class="hbar">
          <div class="hbar-row"><div class="hbar-label">Mei</div><div class="hbar-track"><div class="hbar-fill" style="width:100%;background:var(--accent4);"></div></div><div class="hbar-val" style="color:var(--accent4)">105 tx</div></div>
          <div class="hbar-row"><div class="hbar-label">Oktober</div><div class="hbar-track"><div class="hbar-fill" style="width:91.4%;background:var(--accent3);"></div></div><div class="hbar-val">96 tx</div></div>
          <div class="hbar-row"><div class="hbar-label">April</div><div class="hbar-track"><div class="hbar-fill" style="width:81.9%;background:var(--accent2);"></div></div><div class="hbar-val">86 tx</div></div>
          <div class="hbar-row"><div class="hbar-label">Februari</div><div class="hbar-track"><div class="hbar-fill" style="width:81%;background:var(--accent2);"></div></div><div class="hbar-val">85 tx</div></div>
          <div class="hbar-row"><div class="hbar-label">September</div><div class="hbar-track"><div class="hbar-fill" style="width:61.9%;background:var(--accent);"></div></div><div class="hbar-val" style="color:var(--accent)">65 tx</div></div>
        </div>
      </div>
    </div>
 
    <div class="pullquote">
      <strong>Interpretasi:</strong> Tidak ada pola musiman yang sangat jelas, namun Q2 (Mei) dan Q4 (Oktober–Desember) cenderung lebih kuat. 
      September menjadi outlier negatif yang perlu mendapat perhatian strategi promosi khusus.
    </div>
  </div>
</section>
 
<!-- ══ 6. SEGMENTASI ══ -->
<section id="segmentasi">
  <div class="page reveal">
    <div class="sec-number">06 — Segmentasi Pelanggan</div>
    <div class="sec-title">Gender &amp; <em>Kelompok Usia</em></div>
 
    <div class="two-col">
      <div class="chart-container">
        <div class="chart-header">
          <div class="chart-title">Distribusi Gender</div>
          <div class="chart-subtitle">Proporsi transaksi &amp; revenue</div>
        </div>
        <div class="donut-grid">
          <svg class="donut-svg" viewBox="0 0 36 36">
            <circle cx="18" cy="18" r="15.9155" fill="none" stroke="#f0ebe3" stroke-width="3.8"/>
            <!-- female 51% = 183.6deg stroke-dasharray = 51 49 -->
            <circle cx="18" cy="18" r="15.9155" fill="none"
              stroke="#f5a3c7" stroke-width="3.8"
              stroke-dasharray="51 49"
              stroke-dashoffset="25"
              stroke-linecap="round"/>
            <circle cx="18" cy="18" r="15.9155" fill="none"
              stroke="var(--clothing)" stroke-width="3.8"
              stroke-dasharray="49 51"
              stroke-dashoffset="-26"
              stroke-linecap="round"/>
            <text x="18" y="20" text-anchor="middle" font-size="5" font-family="Playfair Display" fill="#1a1714" font-weight="700">51:49</text>
          </svg>
          <div class="legend-list">
            <div class="legend-row">
              <div class="legend-swatch" style="background:#f5a3c7;"></div>
              <div>
                <div class="legend-key">Female</div>
                <div class="legend-val">510 tx · $232.840</div>
              </div>
            </div>
            <div class="legend-row">
              <div class="legend-swatch" style="background:var(--clothing);"></div>
              <div>
                <div class="legend-key">Male</div>
                <div class="legend-val">490 tx · $223.160</div>
              </div>
            </div>
          </div>
        </div>
      </div>
 
      <div class="chart-container">
        <div class="chart-header">
          <div class="chart-title">Revenue per Gender × Kategori</div>
          <div class="chart-subtitle">Wanita dominan Beauty &amp; Clothing · Pria dominan Electronics</div>
        </div>
        <div class="hbar" style="margin-top:8px;">
          <div style="font-size:11px;color:var(--muted);font-family:'IBM Plex Mono',monospace;margin-bottom:8px;">— FEMALE —</div>
          <div class="hbar-row"><div class="hbar-label">Beauty</div><div class="hbar-track"><div class="hbar-fill" style="width:92.7%;background:#f5a3c7;"></div></div><div class="hbar-val">$74.830</div></div>
          <div class="hbar-row"><div class="hbar-label">Clothing</div><div class="hbar-track"><div class="hbar-fill" style="width:100%;background:#f5a3c7;"></div></div><div class="hbar-val">$81.275</div></div>
          <div class="hbar-row"><div class="hbar-label">Electronics</div><div class="hbar-track"><div class="hbar-fill" style="width:94.4%;background:#f5a3c7;"></div></div><div class="hbar-val">$76.735</div></div>
          <div style="font-size:11px;color:var(--muted);font-family:'IBM Plex Mono',monospace;margin:10px 0 8px;">— MALE —</div>
          <div class="hbar-row"><div class="hbar-label">Beauty</div><div class="hbar-track"><div class="hbar-fill" style="width:85.7%;background:var(--clothing);"></div></div><div class="hbar-val">$68.685</div></div>
          <div class="hbar-row"><div class="hbar-label">Clothing</div><div class="hbar-track"><div class="hbar-fill" style="width:92.7%;background:var(--clothing);"></div></div><div class="hbar-val">$74.305</div></div>
          <div class="hbar-row"><div class="hbar-label">Electronics</div><div class="hbar-track"><div class="hbar-fill" style="width:100%;background:var(--clothing);"></div></div><div class="hbar-val">$80.170</div></div>
        </div>
      </div>
    </div>
 
    <div class="divider"></div>
 
    <p style="font-weight:700;margin-bottom:20px;">Analisis Kelompok Usia</p>
    <div class="chart-container">
      <div class="chart-header">
        <div class="chart-title">Revenue &amp; Transaksi per Kelompok Usia</div>
        <div class="chart-subtitle">Kelompok 46–55 tahun mendominasi di kedua metrik</div>
      </div>
      <div class="hbar">
        <div class="hbar-row"><div class="hbar-label">18–25</div><div class="hbar-track"><div class="hbar-fill" style="width:83.9%;background:var(--accent);"></div></div><div class="hbar-val">$84.550 · 169 tx</div></div>
        <div class="hbar-row"><div class="hbar-label">26–35</div><div class="hbar-track"><div class="hbar-fill" style="width:97.8%;background:var(--accent3);"></div></div><div class="hbar-val">$98.480 · 205 tx</div></div>
        <div class="hbar-row"><div class="hbar-label">36–45</div><div class="hbar-track"><div class="hbar-fill" style="width:91.2%;background:var(--accent2);"></div></div><div class="hbar-val">$91.870 · 202 tx</div></div>
        <div class="hbar-row"><div class="hbar-label">46–55</div><div class="hbar-track"><div class="hbar-fill" style="width:100%;background:var(--accent4);"></div></div><div class="hbar-val" style="color:var(--accent4)"><b>$100.690 · 229 tx</b></div></div>
        <div class="hbar-row"><div class="hbar-label">56–64</div><div class="hbar-track"><div class="hbar-fill" style="width:79.8%;background:var(--muted);"></div></div><div class="hbar-val">$80.410 · 195 tx</div></div>
      </div>
    </div>
  </div>
</section>
 
<!-- ══ 7. KATEGORI ══ -->
<section id="kategori">
  <div class="page reveal">
    <div class="sec-number">07 — Kategori Produk</div>
    <div class="sec-title">Performa <em>Tiga Kategori</em></div>
 
    <div class="two-col">
      <div class="chart-container">
        <div class="chart-header">
          <div class="chart-title">Komposisi Revenue per Kategori</div>
        </div>
        <div class="donut-grid">
          <svg class="donut-svg" viewBox="0 0 36 36">
            <circle cx="18" cy="18" r="15.9155" fill="none" stroke="#f0ebe3" stroke-width="3.8"/>
            <circle cx="18" cy="18" r="15.9155" fill="none" stroke="var(--elec)" stroke-width="3.8"
              stroke-dasharray="34.4 65.6" stroke-dashoffset="25" stroke-linecap="round"/>
            <circle cx="18" cy="18" r="15.9155" fill="none" stroke="var(--clothing)" stroke-width="3.8"
              stroke-dasharray="34.1 65.9" stroke-dashoffset="-9.4" stroke-linecap="round"/>
            <circle cx="18" cy="18" r="15.9155" fill="none" stroke="var(--beauty)" stroke-width="3.8"
              stroke-dasharray="31.5 68.5" stroke-dashoffset="-43.5" stroke-linecap="round"/>
            <text x="18" y="19" text-anchor="middle" font-size="3.5" font-family="IBM Plex Mono" fill="var(--muted)">3 kategori</text>
          </svg>
          <div class="legend-list">
            <div class="legend-row">
              <div class="legend-swatch" style="background:var(--elec);"></div>
              <div><div class="legend-key">Electronics</div><div class="legend-val">$156.905 · 34.4%</div></div>
            </div>
            <div class="legend-row">
              <div class="legend-swatch" style="background:var(--clothing);"></div>
              <div><div class="legend-key">Clothing</div><div class="legend-val">$155.580 · 34.1%</div></div>
            </div>
            <div class="legend-row">
              <div class="legend-swatch" style="background:var(--beauty);"></div>
              <div><div class="legend-key">Beauty</div><div class="legend-val">$143.515 · 31.5%</div></div>
            </div>
          </div>
        </div>
      </div>
 
      <div class="chart-container">
        <div class="chart-header">
          <div class="chart-title">Jumlah Transaksi per Kategori</div>
          <div class="chart-subtitle">Clothing paling banyak transaksi, bukan Electronics</div>
        </div>
        <div class="hbar" style="margin-top:12px;">
          <div class="hbar-row"><div class="hbar-label">Clothing</div><div class="hbar-track"><div class="hbar-fill" style="width:100%;background:var(--clothing);"></div></div><div class="hbar-val" style="color:var(--clothing)">351 tx</div></div>
          <div class="hbar-row"><div class="hbar-label">Electronics</div><div class="hbar-track"><div class="hbar-fill" style="width:97.4%;background:var(--elec);"></div></div><div class="hbar-val">342 tx</div></div>
          <div class="hbar-row"><div class="hbar-label">Beauty</div><div class="hbar-track"><div class="hbar-fill" style="width:87.5%;background:var(--beauty);"></div></div><div class="hbar-val">307 tx</div></div>
        </div>
        <div class="pullquote" style="margin-top:16px;font-size:13px;">
          <strong>Insight Penting:</strong> Meski <em>Clothing</em> memiliki transaksi terbanyak (351), 
          <em>Electronics</em> justru menghasilkan revenue lebih tinggi — karena harga satuannya
          $300–$500 vs $25–$50 untuk Clothing.
        </div>
      </div>
    </div>
 
    <div class="chart-container" style="margin-top:20px;">
      <div class="chart-header">
        <div class="chart-title">Statistik Lengkap per Kategori Produk</div>
      </div>
      <table class="data-table">
        <thead>
          <tr><th>Kategori</th><th>Jml Transaksi</th><th>Total Revenue</th><th>Rata-rata</th><th>Median</th><th>Harga Satuan</th><th>Max Transaksi</th></tr>
        </thead>
        <tbody>
          <tr>
            <td><span class="badge b-elec">Electronics</span></td>
            <td>342</td><td><b>$156.905</b></td><td>$458.78</td><td>$300</td><td>$300 – $500</td><td>$2.000</td>
          </tr>
          <tr>
            <td><span class="badge b-clothing">Clothing</span></td>
            <td><b>351</b></td><td>$155.580</td><td>$443.25</td><td>$150</td><td>$25 – $500</td><td>$2.000</td>
          </tr>
          <tr>
            <td><span class="badge b-beauty">Beauty</span></td>
            <td>307</td><td>$143.515</td><td>$467.47</td><td>$100</td><td>$25 – $500</td><td>$2.000</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</section>
 
<!-- ══ 8. KESIMPULAN ══ -->
<section id="kesimpulan">
  <div class="page reveal">
    <div class="sec-number">08 — Kesimpulan &amp; Rekomendasi</div>
    <div class="sec-title">Apa yang <em>Perlu Dilakukan?</em></div>
 
    <div class="conclusion-list">
 
      <div class="cl-item">
        <div class="cl-num">01</div>
        <div>
          <div class="cl-title">Prioritaskan Electronics sebagai Lini Produk Premium</div>
          <div class="cl-body">Electronics menghasilkan revenue tertinggi ($156.905) meski transaksinya paling sedikit. Investasi lebih pada stok dan pemasaran Electronics — terutama ke segmen pria dan kelompok usia 46–55 tahun — dapat memaksimalkan profitabilitas.</div>
        </div>
      </div>
 
      <div class="cl-item">
        <div class="cl-num">02</div>
        <div>
          <div class="cl-title">Manfaatkan Momentum Mei dan Q4 untuk Promosi Besar</div>
          <div class="cl-body">Bulan Mei (Rp53.150), Oktober, dan Desember adalah periode penjualan terkuat. Rencanakan kampanye promosi, diskon bundling, atau program loyalitas terpusat di bulan-bulan ini untuk memaksimalkan konversi.</div>
        </div>
      </div>
 
      <div class="cl-item">
        <div class="cl-num">03</div>
        <div>
          <div class="cl-title">Intervensi Khusus di Bulan September</div>
          <div class="cl-body">September adalah titik terendah sepanjang tahun (65 transaksi, $23.620). Pertimbangkan flash sale, program referral, atau kampanye "back-to-work" untuk menggenjot penjualan di bulan yang biasanya sepi ini.</div>
        </div>
      </div>
 
      <div class="cl-item">
        <div class="cl-num">04</div>
        <div>
          <div class="cl-title">Targetkan Kelompok 46–55 Tahun sebagai Segmen Inti</div>
          <div class="cl-body">Kelompok ini paling aktif (229 transaksi) dan menghasilkan revenue tertinggi ($100.690). Buat program loyalitas, membership, atau penawaran eksklusif yang sesuai gaya hidup dan kebutuhan usia mid-life.</div>
        </div>
      </div>
 
      <div class="cl-item">
        <div class="cl-num">05</div>
        <div>
          <div class="cl-title">Kembangkan Strategi untuk Segmen 18–25 Tahun</div>
          <div class="cl-body">Kelompok termuda (18–25) memiliki revenue terendah ($84.550). Potensi pertumbuhannya besar — targetkan lewat media sosial, kolaborasi influencer, dan produk Beauty & Clothing yang sesuai tren dan budget mereka.</div>
        </div>
      </div>
 
      <div class="cl-item">
        <div class="cl-num">06</div>
        <div>
          <div class="cl-title">Personalisasi Pemasaran Berdasarkan Gender × Kategori</div>
          <div class="cl-body">Wanita dominan di Beauty ($74.830) dan Clothing ($81.275), sementara pria unggul di Electronics ($80.170). Segmentasi iklan digital yang tepat sasaran dapat meningkatkan efisiensi anggaran marketing secara signifikan.</div>
        </div>
      </div>
 
    </div>
 
    <div class="divider" style="margin-top:48px;"></div>
 
    <div style="display:flex;gap:16px;flex-wrap:wrap;margin-top:32px;">
      <div style="flex:1;min-width:240px;background:var(--ink);color:#faf8f4;border-radius:12px;padding:28px;">
        <div style="font-family:'IBM Plex Mono',monospace;font-size:10px;letter-spacing:.18em;color:rgba(250,248,244,.45);margin-bottom:12px;">TOOLS YANG DIGUNAKAN</div>
        <div style="display:flex;flex-wrap:wrap;gap:8px;">
          <span style="background:rgba(255,255,255,.08);border-radius:4px;padding:4px 10px;font-family:'IBM Plex Mono',monospace;font-size:11px;">Python 3</span>
          <span style="background:rgba(255,255,255,.08);border-radius:4px;padding:4px 10px;font-family:'IBM Plex Mono',monospace;font-size:11px;">Pandas</span>
          <span style="background:rgba(255,255,255,.08);border-radius:4px;padding:4px 10px;font-family:'IBM Plex Mono',monospace;font-size:11px;">NumPy</span>
          <span style="background:rgba(255,255,255,.08);border-radius:4px;padding:4px 10px;font-family:'IBM Plex Mono',monospace;font-size:11px;">Matplotlib</span>
          <span style="background:rgba(255,255,255,.08);border-radius:4px;padding:4px 10px;font-family:'IBM Plex Mono',monospace;font-size:11px;">Seaborn</span>
          <span style="background:rgba(255,255,255,.08);border-radius:4px;padding:4px 10px;font-family:'IBM Plex Mono',monospace;font-size:11px;">Jupyter Notebook</span>
        </div>
      </div>
      <div style="flex:1;min-width:240px;background:var(--card);border:1px solid var(--border);border-radius:12px;padding:28px;">
        <div style="font-family:'IBM Plex Mono',monospace;font-size:10px;letter-spacing:.18em;color:var(--muted);margin-bottom:12px;">DATASET</div>
        <div style="font-size:13px;color:var(--muted);line-height:1.8;">
          <b style="color:var(--ink);">Nama:</b> retail_sales_dataset.csv<br/>
          <b style="color:var(--ink);">Sumber:</b> Kaggle — mohammadtalib786<br/>
          <b style="color:var(--ink);">Ukuran:</b> 1.000 baris × 9 kolom<br/>
          <b style="color:var(--ink);">Periode:</b> 2023<br/>
          <b style="color:var(--ink);">Lisensi:</b> Public / Open Dataset
        </div>
      </div>
    </div>
  </div>
</section>
 
<!-- FOOTER -->
<div class="footer">
  <div>Analisis Visual &amp; Deskriptif — Retail Sales Dataset</div>
  <div>Mata Kuliah: Analisis dan Visualisasi Data · 2026</div>
  <div>Dibuat dengan Python · Pandas · Seaborn · Matplotlib</div>
</div>
 
<script>
const observer = new IntersectionObserver(entries => {
  entries.forEach(e => { if(e.isIntersecting) { e.target.classList.add('in'); observer.unobserve(e.target); } });
}, { threshold: 0.07 });
document.querySelectorAll('.reveal').forEach(el => observer.observe(el));
 
// highlight active nav
const sections = document.querySelectorAll('section[id]');
const navLinks = document.querySelectorAll('.toc-nav a');
window.addEventListener('scroll', () => {
  let current = '';
  sections.forEach(s => { if(window.scrollY >= s.offsetTop - 100) current = s.id; });
  navLinks.forEach(a => {
    a.style.color = a.getAttribute('href') === '#'+current ? 'var(--accent)' : '';
    a.style.borderBottomColor = a.getAttribute('href') === '#'+current ? 'var(--accent)' : 'transparent';
  });
});
</script>
</body>
</html>
