!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Laporan EDA — Retail Sales Dataset</title>
</head>
<body style="font-family: Georgia, 'Times New Roman', serif; max-width: 900px; margin: 40px auto; padding: 0 24px; color: #1a1a1a; line-height: 1.8; background: #fff;">
 
  <!-- HEADER -->
  <div style="border-top: 4px solid #2c3e50; border-bottom: 1px solid #ccc; padding: 32px 0 24px; margin-bottom: 40px;">
    <p style="font-family: 'Courier New', monospace; font-size: 12px; color: #888; margin: 0 0 8px; letter-spacing: 2px; text-transform: uppercase;">Exploratory Data Analysis</p>
    <h1 style="font-size: 2.2em; font-weight: bold; margin: 0 0 12px; color: #2c3e50; line-height: 1.2;">Retail Sales Dataset</h1>
    <p style="font-size: 1em; color: #555; margin: 0;">
      Analisis mendalam terhadap 1.000 transaksi retail — meliputi pola pembelian, segmentasi pelanggan, performa kategori produk, dan tren penjualan bulanan.
    </p>
    <div style="margin-top: 16px; display: flex; gap: 16px; flex-wrap: wrap;">
      <span style="background: #ecf0f1; padding: 4px 12px; border-radius: 4px; font-size: 13px; font-family: monospace;">1.000 transaksi</span>
      <span style="background: #ecf0f1; padding: 4px 12px; border-radius: 4px; font-size: 13px; font-family: monospace;">9 kolom</span>
      <span style="background: #ecf0f1; padding: 4px 12px; border-radius: 4px; font-size: 13px; font-family: monospace;">Python · Pandas · Seaborn</span>
    </div>
  </div>
 
  <!-- DAFTAR ISI -->
  <nav style="background: #f8f9fa; border-left: 4px solid #2c3e50; padding: 20px 24px; margin-bottom: 48px; border-radius: 0 6px 6px 0;">
    <p style="font-weight: bold; margin: 0 0 12px; font-size: 14px; letter-spacing: 1px; text-transform: uppercase; color: #2c3e50;">Daftar Isi</p>
    <ol style="margin: 0; padding-left: 20px; font-size: 14px; line-height: 2;">
      <li><a href="#s1" style="color: #2980b9; text-decoration: none;">Gambaran Dataset</a></li>
      <li><a href="#s2" style="color: #2980b9; text-decoration: none;">Kualitas Data</a></li>
      <li><a href="#s3" style="color: #2980b9; text-decoration: none;">Statistik Deskriptif</a></li>
      <li><a href="#s4" style="color: #2980b9; text-decoration: none;">Distribusi Variabel Numerik</a></li>
      <li><a href="#s5" style="color: #2980b9; text-decoration: none;">Deteksi Outliers</a></li>
      <li><a href="#s6" style="color: #2980b9; text-decoration: none;">Analisis Berdasarkan Gender</a></li>
      <li><a href="#s7" style="color: #2980b9; text-decoration: none;">Analisis Berdasarkan Kategori Produk</a></li>
      <li><a href="#s8" style="color: #2980b9; text-decoration: none;">Tren Temporal (Bulanan)</a></li>
      <li><a href="#s9" style="color: #2980b9; text-decoration: none;">Segmentasi Kelompok Usia</a></li>
      <li><a href="#s10" style="color: #2980b9; text-decoration: none;">Hubungan Gender × Kategori Produk</a></li>
      <li><a href="#s11" style="color: #2980b9; text-decoration: none;">Korelasi Antar Variabel</a></li>
      <li><a href="#s12" style="color: #2980b9; text-decoration: none;">Kesimpulan &amp; Rekomendasi</a></li>
    </ol>
  </nav>
 
 
  <!-- SECTION 1 -->
  <section id="s1" style="margin-bottom: 48px;">
    <h2 style="font-size: 1.5em; color: #2c3e50; border-bottom: 2px solid #ecf0f1; padding-bottom: 8px;">1. Gambaran Dataset</h2>
 
    <p>Dataset ini merekam <strong>1.000 transaksi retail</strong> yang mencakup informasi pelanggan, produk yang dibeli, hingga nilai transaksi. Setiap baris merepresentasikan satu kejadian pembelian yang unik.</p>
 
    <p>Berikut adalah struktur lengkap dari 9 kolom yang tersedia:</p>
 
    <table style="width: 100%; border-collapse: collapse; font-size: 14px; margin: 16px 0;">
      <thead>
        <tr style="background: #2c3e50; color: white;">
          <th style="padding: 10px 14px; text-align: left;">Kolom</th>
          <th style="padding: 10px 14px; text-align: left;">Tipe</th>
          <th style="padding: 10px 14px; text-align: left;">Deskripsi</th>
          <th style="padding: 10px 14px; text-align: left;">Rentang / Nilai</th>
        </tr>
      </thead>
      <tbody>
        <tr style="background: #f9f9f9;">
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;"><code>Transaction ID</code></td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">object</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Identifikasi unik tiap transaksi</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">—</td>
        </tr>
        <tr>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;"><code>Date</code></td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">datetime</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Tanggal transaksi terjadi</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">—</td>
        </tr>
        <tr style="background: #f9f9f9;">
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;"><code>Customer ID</code></td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">object</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Identifikasi unik tiap pelanggan</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">—</td>
        </tr>
        <tr>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;"><code>Gender</code></td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">object</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Jenis kelamin pelanggan</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Male / Female</td>
        </tr>
        <tr style="background: #f9f9f9;">
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;"><code>Age</code></td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">int64</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Usia pelanggan</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">18 – 64 tahun</td>
        </tr>
        <tr>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;"><code>Product Category</code></td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">object</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Kategori produk yang dibeli</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Beauty / Clothing / Electronics</td>
        </tr>
        <tr style="background: #f9f9f9;">
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;"><code>Quantity</code></td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">int64</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Jumlah unit yang dibeli</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">1 – 4 unit</td>
        </tr>
        <tr>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;"><code>Price per Unit</code></td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">float64</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Harga satuan produk</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">25 – 500</td>
        </tr>
        <tr style="background: #f9f9f9;">
          <td style="padding: 9px 14px;"><code>Total Amount</code></td>
          <td style="padding: 9px 14px;">float64</td>
          <td style="padding: 9px 14px;">Total nilai transaksi</td>
          <td style="padding: 9px 14px;">25 – 2.000</td>
        </tr>
      </tbody>
    </table>
 
    <p>Setelah data dimuat, kolom <code>Date</code> dikonversi ke format <code>datetime</code> dan diekstrak menjadi fitur turunan: <code>year</code>, <code>month</code>, <code>day</code>, dan <code>month_name</code>. Fitur ini digunakan untuk analisis temporal di bagian selanjutnya.</p>
  </section>
 
 
  <!-- SECTION 2 -->
  <section id="s2" style="margin-bottom: 48px;">
    <h2 style="font-size: 1.5em; color: #2c3e50; border-bottom: 2px solid #ecf0f1; padding-bottom: 8px;">2. Kualitas Data</h2>
 
    <p>Sebelum melakukan analisis, integritas data diperiksa dari tiga aspek: nilai kosong, baris duplikat, dan konsistensi nilai kategorik.</p>
 
    <table style="width: 100%; border-collapse: collapse; font-size: 14px; margin: 16px 0;">
      <thead>
        <tr style="background: #2c3e50; color: white;">
          <th style="padding: 10px 14px; text-align: left;">Pemeriksaan</th>
          <th style="padding: 10px 14px; text-align: center;">Hasil</th>
          <th style="padding: 10px 14px; text-align: left;">Kesimpulan</th>
        </tr>
      </thead>
      <tbody>
        <tr style="background: #f9f9f9;">
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Missing values</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center; color: #27ae60; font-weight: bold;">0</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Tidak ada nilai kosong di seluruh kolom</td>
        </tr>
        <tr>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Baris duplikat</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center; color: #27ae60; font-weight: bold;">0</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Setiap baris adalah transaksi yang unik</td>
        </tr>
        <tr style="background: #f9f9f9;">
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Nilai unik <code>Gender</code></td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center; color: #27ae60; font-weight: bold;">2</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Hanya berisi <code>Male</code> dan <code>Female</code></td>
        </tr>
        <tr>
          <td style="padding: 9px 14px;">Nilai unik <code>Product Category</code></td>
          <td style="padding: 9px 14px; text-align: center; color: #27ae60; font-weight: bold;">3</td>
          <td style="padding: 9px 14px;">Hanya berisi Beauty, Clothing, Electronics</td>
        </tr>
      </tbody>
    </table>
 
    <blockquote style="border-left: 4px solid #27ae60; padding: 12px 20px; margin: 20px 0; background: #f0faf4; color: #1e8449; border-radius: 0 6px 6px 0;">
      <strong>✅ Dataset bersih dan siap dianalisis</strong> — tidak diperlukan langkah data cleaning, imputasi, atau penghapusan duplikat.
    </blockquote>
  </section>
 
 
  <!-- SECTION 3 -->
  <section id="s3" style="margin-bottom: 48px;">
    <h2 style="font-size: 1.5em; color: #2c3e50; border-bottom: 2px solid #ecf0f1; padding-bottom: 8px;">3. Statistik Deskriptif</h2>
 
    <p>Ringkasan statistik untuk keempat variabel numerik utama memberikan gambaran awal tentang skala dan sebaran data.</p>
 
    <table style="width: 100%; border-collapse: collapse; font-size: 14px; margin: 16px 0;">
      <thead>
        <tr style="background: #2c3e50; color: white;">
          <th style="padding: 10px 14px; text-align: left;">Statistik</th>
          <th style="padding: 10px 14px; text-align: center;">Age</th>
          <th style="padding: 10px 14px; text-align: center;">Quantity</th>
          <th style="padding: 10px 14px; text-align: center;">Price per Unit</th>
          <th style="padding: 10px 14px; text-align: center;">Total Amount</th>
        </tr>
      </thead>
      <tbody>
        <tr style="background: #f9f9f9;">
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; font-weight: bold;">Min</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center;">18</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center;">1</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center;">25</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center;">25</td>
        </tr>
        <tr>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; font-weight: bold;">Max</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center;">64</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center;">4</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center;">500</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center;">2.000</td>
        </tr>
        <tr style="background: #f9f9f9;">
          <td style="padding: 9px 14px; font-weight: bold;">Rentang</td>
          <td style="padding: 9px 14px; text-align: center;">46 tahun</td>
          <td style="padding: 9px 14px; text-align: center;">3 unit</td>
          <td style="padding: 9px 14px; text-align: center;">475</td>
          <td style="padding: 9px 14px; text-align: center;">1.975</td>
        </tr>
      </tbody>
    </table>
 
    <p>Ada beberapa hal yang langsung terlihat dari tabel ini:</p>
 
    <ul>
      <li><strong>Age (18–64 tahun):</strong> Pelanggan mencakup rentang usia yang sangat luas, dari dewasa muda hingga lansia awal. Ini mengindikasikan toko yang menjangkau berbagai generasi sekaligus.</li>
      <li><strong>Quantity (1–4 unit):</strong> Pembelian hanya berkisar 1 hingga 4 unit per transaksi — ini adalah pola belanja konsumen akhir (retail), bukan pembelian grosir atau bisnis.</li>
      <li><strong>Price per Unit (25–500):</strong> Rentang harga yang sangat lebar mencerminkan perbedaan kategori produk yang mencolok. Produk Beauty berada di sisi bawah, Electronics di sisi atas.</li>
      <li><strong>Total Amount (25–2.000):</strong> Nilai transaksi terbentuk dari perkalian Quantity × Price per Unit, sehingga variabilitasnya sangat besar dan dipengaruhi kategori produk.</li>
    </ul>
  </section>
 
 
  <!-- SECTION 4 -->
  <section id="s4" style="margin-bottom: 48px;">
    <h2 style="font-size: 1.5em; color: #2c3e50; border-bottom: 2px solid #ecf0f1; padding-bottom: 8px;">4. Distribusi Variabel Numerik</h2>
 
    <h3 style="font-size: 1.1em; color: #34495e; margin-top: 24px;">4.1 Total Amount — Distribusi Multimodal</h3>
    <p>Distribusi <code>Total Amount</code> divisualisasikan dengan histogram + KDE (Kernel Density Estimation). Hasilnya <strong>tidak membentuk satu puncak tunggal (unimodal)</strong>, melainkan beberapa puncak yang mencerminkan kelompok-kelompok harga diskret.</p>
    <p>Ini terjadi karena <code>Price per Unit</code> hanya mengambil beberapa nilai tetap (misalnya 25, 30, 50, 300, 500). Ketika dikalikan dengan <code>Quantity</code> (1–4), total transaksi akan mengelompok di titik-titik tertentu. Pola ini disebut <em>distribusi multimodal</em> dan merupakan artefak dari struktur pricing yang bersifat diskret, bukan indikasi masalah data.</p>
 
    <h3 style="font-size: 1.1em; color: #34495e; margin-top: 24px;">4.2 Usia Pelanggan — Distribusi Merata</h3>
    <p>Distribusi usia pelanggan menunjukkan pola yang relatif <strong>datar (uniform)</strong> dari usia 18 hingga 64 tahun. Tidak ada kelompok usia yang tampak mendominasi secara dramatis. Ini adalah tanda bahwa toko ini berhasil menarik pelanggan dari berbagai generasi secara seimbang — dari Gen Z, Milenial, Gen X, hingga Baby Boomer.</p>
 
    <h3 style="font-size: 1.1em; color: #34495e; margin-top: 24px;">4.3 Quantity — Preferensi Pembelian Merata</h3>
    <p>Karena <code>Quantity</code> hanya bernilai bilangan bulat 1, 2, 3, atau 4, visualisasi yang tepat adalah <strong>bar chart</strong> (bukan histogram). Hasilnya menunjukkan frekuensi yang hampir seimbang di antara keempat nilai tersebut. Artinya pelanggan tidak memiliki preferensi kuat terhadap jumlah unit tertentu — pembelian 1 unit sama umum dengan pembelian 4 unit.</p>
  </section>
 
 
  <!-- SECTION 5 -->
  <section id="s5" style="margin-bottom: 48px;">
    <h2 style="font-size: 1.5em; color: #2c3e50; border-bottom: 2px solid #ecf0f1; padding-bottom: 8px;">5. Deteksi Outliers</h2>
 
    <p>Boxplot digunakan untuk mendeteksi nilai-nilai ekstrem pada tiga variabel numerik. Berikut interpretasinya:</p>
 
    <table style="width: 100%; border-collapse: collapse; font-size: 14px; margin: 16px 0;">
      <thead>
        <tr style="background: #2c3e50; color: white;">
          <th style="padding: 10px 14px; text-align: left;">Variabel</th>
          <th style="padding: 10px 14px; text-align: center;">Outlier?</th>
          <th style="padding: 10px 14px; text-align: left;">Penjelasan</th>
        </tr>
      </thead>
      <tbody>
        <tr style="background: #f9f9f9;">
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;"><code>Age</code></td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center; color: #27ae60;">Tidak</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Distribusi merata tanpa nilai ekstrem</td>
        </tr>
        <tr>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;"><code>Price per Unit</code></td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center; color: #e67e22;">Rentang lebar</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Wajar — harga Electronics jauh lebih tinggi dari Beauty</td>
        </tr>
        <tr style="background: #f9f9f9;">
          <td style="padding: 9px 14px;"><code>Total Amount</code></td>
          <td style="padding: 9px 14px; text-align: center; color: #e67e22;">Rentang lebar</td>
          <td style="padding: 9px 14px;">Dampak langsung dari variasi harga antar kategori produk</td>
        </tr>
      </tbody>
    </table>
 
    <blockquote style="border-left: 4px solid #e67e22; padding: 12px 20px; margin: 20px 0; background: #fef9f0; color: #784212; border-radius: 0 6px 6px 0;">
      <strong>⚠️ Catatan penting:</strong> Nilai-nilai yang terlihat "ekstrem" pada <code>Price per Unit</code> dan <code>Total Amount</code> bukanlah anomali data yang perlu dihapus. Mereka mencerminkan perbedaan harga yang nyata dan valid antara kategori produk (Electronics vs Beauty). Menghapusnya justru akan menghilangkan informasi berharga tentang produk mahal.
    </blockquote>
  </section>
 
 
  <!-- SECTION 6 -->
  <section id="s6" style="margin-bottom: 48px;">
    <h2 style="font-size: 1.5em; color: #2c3e50; border-bottom: 2px solid #ecf0f1; padding-bottom: 8px;">6. Analisis Berdasarkan Gender</h2>
 
    <p>Analisis ini membandingkan perilaku belanja antara pelanggan pria dan wanita dari dua sudut pandang: volume transaksi dan total nilai penjualan.</p>
 
    <table style="width: 100%; border-collapse: collapse; font-size: 14px; margin: 16px 0;">
      <thead>
        <tr style="background: #2c3e50; color: white;">
          <th style="padding: 10px 14px; text-align: left;">Metrik</th>
          <th style="padding: 10px 14px; text-align: center;">Female</th>
          <th style="padding: 10px 14px; text-align: center;">Male</th>
        </tr>
      </thead>
      <tbody>
        <tr style="background: #f9f9f9;">
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Proporsi Transaksi</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center;">~51%</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center;">~49%</td>
        </tr>
        <tr>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Total Penjualan</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center;">Sedikit lebih tinggi</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center;">Sedikit lebih rendah</td>
        </tr>
        <tr style="background: #f9f9f9;">
          <td style="padding: 9px 14px;">Rata-rata Nilai Transaksi</td>
          <td style="padding: 9px 14px; text-align: center;">Hampir setara</td>
          <td style="padding: 9px 14px; text-align: center;">Hampir setara</td>
        </tr>
      </tbody>
    </table>
 
    <p><strong>Interpretasi:</strong> Meskipun pelanggan Female sedikit lebih banyak bertransaksi (~51%), perbedaan ini tidak signifikan secara praktis. Yang lebih penting, <strong>rata-rata nilai transaksi per gender hampir identik</strong> — artinya daya beli dan pola pengeluaran antara pria dan wanita sangat mirip dalam dataset ini. Tidak ada gender yang secara konsisten "berbelanja lebih mahal" dari yang lain.</p>
 
    <p>Temuan ini mengindikasikan bahwa strategi harga dan promosi umum kemungkinan efektif untuk kedua gender. Namun, perbedaan baru akan muncul ketika kita melihat <em>jenis produk</em> yang dibeli — dibahas di Bagian 10.</p>
  </section>
 
 
  <!-- SECTION 7 -->
  <section id="s7" style="margin-bottom: 48px;">
    <h2 style="font-size: 1.5em; color: #2c3e50; border-bottom: 2px solid #ecf0f1; padding-bottom: 8px;">7. Analisis Berdasarkan Kategori Produk</h2>
 
    <p>Tiga kategori produk — <strong>Beauty</strong>, <strong>Clothing</strong>, dan <strong>Electronics</strong> — dianalisis dari dua dimensi: frekuensi transaksi dan kontribusi revenue.</p>
 
    <table style="width: 100%; border-collapse: collapse; font-size: 14px; margin: 16px 0;">
      <thead>
        <tr style="background: #2c3e50; color: white;">
          <th style="padding: 10px 14px; text-align: left;">Kategori</th>
          <th style="padding: 10px 14px; text-align: center;">Transaksi</th>
          <th style="padding: 10px 14px; text-align: center;">Total Revenue</th>
          <th style="padding: 10px 14px; text-align: center;">Rata-rata/Transaksi</th>
        </tr>
      </thead>
      <tbody>
        <tr style="background: #eaf4fb;">
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; font-weight: bold;">Electronics</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center;">~342</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center;"><strong>🥇 Tertinggi</strong></td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center;">Tertinggi</td>
        </tr>
        <tr>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; font-weight: bold;">Clothing</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center;"><strong>~351 🥇</strong></td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center;">Menengah</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center;">Menengah</td>
        </tr>
        <tr style="background: #f9f9f9;">
          <td style="padding: 9px 14px; font-weight: bold;">Beauty</td>
          <td style="padding: 9px 14px; text-align: center;">~307</td>
          <td style="padding: 9px 14px; text-align: center;">Terendah</td>
          <td style="padding: 9px 14px; text-align: center;">Terendah</td>
        </tr>
      </tbody>
    </table>
 
    <p>Ada temuan yang sangat menarik di sini: <strong>Clothing memiliki jumlah transaksi terbanyak, namun Electronics menghasilkan revenue tertinggi</strong>. Ini terjadi karena harga satuan Electronics jauh lebih tinggi, sehingga meskipun tidak sering dibeli, setiap transaksinya bernilai jauh lebih besar.</p>
 
    <p>Sebaliknya, Beauty memiliki transaksi paling sedikit <em>sekaligus</em> revenue terendah — kombinasi yang menempatkannya sebagai kategori dengan kinerja paling lemah dari kedua dimensi. Ini bisa menjadi sinyal untuk mengevaluasi strategi promosi untuk kategori Beauty.</p>
 
    <blockquote style="border-left: 4px solid #2980b9; padding: 12px 20px; margin: 20px 0; background: #eaf4fb; color: #1a5276; border-radius: 0 6px 6px 0;">
      <strong>💡 Insight kunci:</strong> Frekuensi transaksi ≠ revenue. Electronics adalah "sapi perah" utama meskipun tidak paling sering terjual. Strategi yang berbeda diperlukan untuk mengoptimalkan tiap kategori.
    </blockquote>
  </section>
 
 
  <!-- SECTION 8 -->
  <section id="s8" style="margin-bottom: 48px;">
    <h2 style="font-size: 1.5em; color: #2c3e50; border-bottom: 2px solid #ecf0f1; padding-bottom: 8px;">8. Tren Temporal (Bulanan)</h2>
 
    <p>Analisis temporal dilakukan dengan mengagregasi data berdasarkan bulan, menggunakan fitur <code>month_name</code> yang diekstrak dari kolom <code>Date</code>.</p>
 
    <h3 style="font-size: 1.1em; color: #34495e; margin-top: 24px;">8.1 Jumlah Transaksi per Bulan</h3>
    <p>Bar chart jumlah transaksi per bulan menunjukkan pola yang <strong>relatif stabil sepanjang tahun</strong>. Tidak ada lonjakan atau penurunan ekstrem yang mencolok di bulan tertentu. Distribusi yang merata ini mengindikasikan tidak adanya musim belanja yang sangat dominan — bisnis berjalan cukup konsisten setiap bulannya.</p>
 
    <h3 style="font-size: 1.1em; color: #34495e; margin-top: 24px;">8.2 Total Penjualan per Bulan</h3>
    <p>Line chart total penjualan bulanan memperlihatkan fluktuasi yang moderat namun tanpa tren kenaikan atau penurunan yang signifikan secara keseluruhan. Beberapa bulan menunjukkan sedikit peningkatan yang bisa dikaitkan dengan musim tertentu, namun perlu data lebih dari satu tahun untuk mengonfirmasi pola musiman yang nyata.</p>
 
    <h3 style="font-size: 1.1em; color: #34495e; margin-top: 24px;">8.3 Tren per Kategori per Bulan</h3>
    <p>Ketika tren penjualan dipecah per kategori, terlihat bahwa <strong>Electronics secara konsisten mendominasi total penjualan</strong> di hampir setiap bulan. Clothing dan Beauty berjalan berdekatan di kisaran nilai yang lebih rendah. Ketiga kategori mengalami fluktuasi di bulan yang berbeda, namun tidak saling berkorelasi kuat — artinya naik-turunnya satu kategori tidak selalu diikuti kategori lain.</p>
 
    <blockquote style="border-left: 4px solid #8e44ad; padding: 12px 20px; margin: 20px 0; background: #f5eef8; color: #4a235a; border-radius: 0 6px 6px 0;">
      <strong>📅 Rekomendasi:</strong> Untuk mendapatkan insight musiman yang lebih kuat (seperti lonjakan di akhir tahun atau musim liburan), diperlukan data historis minimal 2–3 tahun. Dataset saat ini hanya mencakup satu periode.
    </blockquote>
  </section>
 
 
  <!-- SECTION 9 -->
  <section id="s9" style="margin-bottom: 48px;">
    <h2 style="font-size: 1.5em; color: #2c3e50; border-bottom: 2px solid #ecf0f1; padding-bottom: 8px;">9. Segmentasi Kelompok Usia</h2>
 
    <p>Pelanggan dikelompokkan ke dalam 5 segmen usia menggunakan fungsi <code>pd.cut()</code>:</p>
 
    <table style="width: 100%; border-collapse: collapse; font-size: 14px; margin: 16px 0;">
      <thead>
        <tr style="background: #2c3e50; color: white;">
          <th style="padding: 10px 14px;">Kelompok Usia</th>
          <th style="padding: 10px 14px; text-align: center;">Jumlah Transaksi</th>
          <th style="padding: 10px 14px; text-align: left;">Karakteristik</th>
        </tr>
      </thead>
      <tbody>
        <tr style="background: #f9f9f9;">
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">18–25 tahun</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center;">~200</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Dewasa muda, Gen Z</td>
        </tr>
        <tr>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">26–35 tahun</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center;">~200</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Milenial muda</td>
        </tr>
        <tr style="background: #f9f9f9;">
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">36–45 tahun</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center;">~200</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Milenial senior</td>
        </tr>
        <tr>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">46–55 tahun</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center;">~200</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Gen X</td>
        </tr>
        <tr style="background: #f9f9f9;">
          <td style="padding: 9px 14px;">56–64 tahun</td>
          <td style="padding: 9px 14px; text-align: center;">~200</td>
          <td style="padding: 9px 14px;">Baby Boomer</td>
        </tr>
      </tbody>
    </table>
 
    <p>Distribusi yang <strong>sangat merata (~200 transaksi per kelompok)</strong> ini menunjukkan bahwa toko berhasil menjangkau semua segmen usia secara proporsional. Tidak ada kelompok usia yang mendominasi, yang merupakan tanda kesehatan bisnis dari sisi diversifikasi pelanggan.</p>
 
    <p>Namun, ketika dianalisis lebih dalam per kategori produk, perbedaan preferensi mulai terlihat. Electronics tetap dominan di semua kelompok usia, tetapi proporsi relatif antara Beauty dan Clothing bergeser antar segmen — memberikan peluang untuk kampanye yang lebih personal.</p>
  </section>
 
 
  <!-- SECTION 10 -->
  <section id="s10" style="margin-bottom: 48px;">
    <h2 style="font-size: 1.5em; color: #2c3e50; border-bottom: 2px solid #ecf0f1; padding-bottom: 8px;">10. Hubungan Gender × Kategori Produk</h2>
 
    <p>Analisis pivot table mengungkap pola yang lebih nuansir ketika gender dikombinasikan dengan kategori produk.</p>
 
    <table style="width: 100%; border-collapse: collapse; font-size: 14px; margin: 16px 0;">
      <thead>
        <tr style="background: #2c3e50; color: white;">
          <th style="padding: 10px 14px;">Kategori</th>
          <th style="padding: 10px 14px; text-align: center;">Female</th>
          <th style="padding: 10px 14px; text-align: center;">Male</th>
          <th style="padding: 10px 14px; text-align: left;">Pola</th>
        </tr>
      </thead>
      <tbody>
        <tr style="background: #f9f9f9;">
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; font-weight: bold;">Electronics</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center;">Tinggi</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center;"><strong>Lebih tinggi</strong></td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Male cenderung lebih dominan</td>
        </tr>
        <tr>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; font-weight: bold;">Clothing</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center;"><strong>Lebih tinggi</strong></td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center;">Menengah</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Female lebih aktif berbelanja pakaian</td>
        </tr>
        <tr style="background: #f9f9f9;">
          <td style="padding: 9px 14px; font-weight: bold;">Beauty</td>
          <td style="padding: 9px 14px; text-align: center;"><strong>Lebih tinggi</strong></td>
          <td style="padding: 9px 14px; text-align: center;">Rendah</td>
          <td style="padding: 9px 14px;">Female mendominasi pembelian produk kecantikan</td>
        </tr>
      </tbody>
    </table>
 
    <p>Pola yang muncul konsisten dengan perilaku belanja yang umum: <strong>Female lebih mendominasi pada Beauty dan Clothing</strong>, sementara <strong>Male lebih aktif pada Electronics</strong>. Meskipun total nilai transaksi per gender hampir setara (lihat Bagian 6), jenis produk yang dibeli berbeda secara signifikan.</p>
 
    <p>Insight ini memiliki implikasi langsung pada strategi pemasaran: promosi produk kecantikan dan fashion akan lebih efektif bila diarahkan ke segmen Female, sedangkan kampanye gadget dan elektronik lebih resonan untuk segmen Male.</p>
  </section>
 
 
  <!-- SECTION 11 -->
  <section id="s11" style="margin-bottom: 48px;">
    <h2 style="font-size: 1.5em; color: #2c3e50; border-bottom: 2px solid #ecf0f1; padding-bottom: 8px;">11. Korelasi Antar Variabel</h2>
 
    <p>Heatmap korelasi Pearson dihitung untuk keempat variabel numerik: <code>Age</code>, <code>Quantity</code>, <code>Price per Unit</code>, dan <code>Total Amount</code>.</p>
 
    <table style="width: 100%; border-collapse: collapse; font-size: 14px; margin: 16px 0;">
      <thead>
        <tr style="background: #2c3e50; color: white;">
          <th style="padding: 10px 14px;">Pasangan Variabel</th>
          <th style="padding: 10px 14px; text-align: center;">Korelasi</th>
          <th style="padding: 10px 14px; text-align: left;">Interpretasi</th>
        </tr>
      </thead>
      <tbody>
        <tr style="background: #f9f9f9;">
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;"><code>Price per Unit</code> ↔ <code>Total Amount</code></td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center; color: #27ae60; font-weight: bold;">Tinggi (+)</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Harga lebih mahal → total lebih besar</td>
        </tr>
        <tr>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;"><code>Quantity</code> ↔ <code>Total Amount</code></td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center; color: #27ae60; font-weight: bold;">Tinggi (+)</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Lebih banyak unit → total lebih besar</td>
        </tr>
        <tr style="background: #f9f9f9;">
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;"><code>Age</code> ↔ <code>Total Amount</code></td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; text-align: center; color: #7f8c8d;">Sangat rendah</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Usia tidak mempengaruhi nilai transaksi</td>
        </tr>
        <tr>
          <td style="padding: 9px 14px;"><code>Age</code> ↔ <code>Quantity</code></td>
          <td style="padding: 9px 14px; text-align: center; color: #7f8c8d;">Sangat rendah</td>
          <td style="padding: 9px 14px;">Usia tidak mempengaruhi jumlah unit yang dibeli</td>
        </tr>
      </tbody>
    </table>
 
    <p>Korelasi tinggi antara <code>Price per Unit</code>, <code>Quantity</code>, dan <code>Total Amount</code> adalah <strong>hubungan matematis</strong> yang sudah diharapkan — karena <code>Total Amount = Quantity × Price per Unit</code>. Ini bukan temuan yang mengejutkan.</p>
 
    <p>Yang lebih menarik adalah <strong>korelasi yang sangat rendah antara <code>Age</code> dan semua variabel lainnya</strong>. Ini mengonfirmasi temuan dari analisis kelompok usia: usia pelanggan tidak menentukan seberapa banyak atau seberapa mahal mereka berbelanja. Faktor penentu nilai transaksi lebih banyak bergantung pada <em>kategori produk</em> yang dipilih, bukan usia pembelinya.</p>
 
    <p>Scatter plot <code>Quantity</code> vs <code>Total Amount</code> yang dibagi per kategori produk memperlihatkan <strong>kluster-kluster yang terpisah jelas</strong>: Electronics dan Clothing menempati rentang atas, sementara Beauty berada di rentang bawah — perbedaan yang sepenuhnya driven oleh <code>Price per Unit</code>.</p>
  </section>
 
 
  <!-- SECTION 12 -->
  <section id="s12" style="margin-bottom: 48px;">
    <h2 style="font-size: 1.5em; color: #2c3e50; border-bottom: 2px solid #ecf0f1; padding-bottom: 8px;">12. Kesimpulan &amp; Rekomendasi</h2>
 
    <h3 style="font-size: 1.1em; color: #34495e; margin-top: 24px;">Ringkasan Temuan Utama</h3>
 
    <table style="width: 100%; border-collapse: collapse; font-size: 14px; margin: 16px 0;">
      <thead>
        <tr style="background: #2c3e50; color: white;">
          <th style="padding: 10px 14px;">Area</th>
          <th style="padding: 10px 14px;">Temuan Kunci</th>
        </tr>
      </thead>
      <tbody>
        <tr style="background: #f9f9f9;">
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; font-weight: bold;">Kualitas Data</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Bersih sempurna — 0 missing values, 0 duplikat, nilai kategorik konsisten</td>
        </tr>
        <tr>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; font-weight: bold;">Distribusi</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Total Amount multimodal akibat harga diskret; Age dan Quantity terdistribusi merata</td>
        </tr>
        <tr style="background: #f9f9f9;">
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; font-weight: bold;">Gender</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Proporsi hampir setara (~51%/49%); daya beli serupa; beda pada preferensi kategori</td>
        </tr>
        <tr>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; font-weight: bold;">Kategori Produk</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Electronics = revenue tertinggi; Clothing = transaksi terbanyak; Beauty = keduanya terendah</td>
        </tr>
        <tr style="background: #f9f9f9;">
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; font-weight: bold;">Tren Temporal</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Stabil sepanjang tahun; tidak ada pola musiman yang jelas dari satu periode data</td>
        </tr>
        <tr>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0; font-weight: bold;">Kelompok Usia</td>
          <td style="padding: 9px 14px; border-bottom: 1px solid #e0e0e0;">Merata di semua segmen; usia tidak berkorelasi dengan nilai transaksi</td>
        </tr>
        <tr style="background: #f9f9f9;">
          <td style="padding: 9px 14px; font-weight: bold;">Gender × Kategori</td>
          <td style="padding: 9px 14px;">Female dominan di Beauty &amp; Clothing; Male dominan di Electronics</td>
        </tr>
      </tbody>
    </table>
 
    <h3 style="font-size: 1.1em; color: #34495e; margin-top: 32px;">Rekomendasi Bisnis</h3>
 
    <ol style="line-height: 2.2; padding-left: 20px;">
      <li>
        <strong>Prioritaskan kategori Electronics</strong> — Kontribusi revenue-nya paling besar. Pastikan ketersediaan stok, pertimbangkan bundling promosi, dan alokasikan anggaran display yang lebih besar untuk kategori ini.
      </li>
      <li>
        <strong>Revitalisasi kategori Beauty</strong> — Kategori ini memiliki kinerja paling lemah dari sisi transaksi maupun revenue. Perlu dievaluasi: apakah karena kurangnya promosi, keterbatasan pilihan produk, atau harga yang kurang kompetitif.
      </li>
      <li>
        <strong>Targetkan kampanye berdasarkan gender</strong> — Promosi Beauty dan Clothing akan lebih efektif diarahkan ke segmen Female; kampanye Electronics lebih resonan untuk Male. Ini berlaku untuk iklan digital, email marketing, maupun tata letak toko.
      </li>
      <li>
        <strong>Manfaatkan distribusi usia yang merata</strong> — Karena semua kelompok usia terwakili secara seimbang, ada peluang untuk membuat kampanye yang tersegmentasi per generasi tanpa mengabaikan salah satu kelompok.
      </li>
      <li>
        <strong>Kumpulkan data multi-tahun</strong> — Tren temporal saat ini tidak menunjukkan pola musiman yang jelas. Dengan data 2–3 tahun, manajemen dapat mengidentifikasi bulan-bulan puncak untuk perencanaan stok dan promosi yang lebih akurat.
      </li>
      <li>
        <strong>Lanjutkan ke analisis RFM</strong> — Langkah analitik berikutnya yang direkomendasikan adalah analisis Recency-Frequency-Monetary untuk mengidentifikasi pelanggan loyal, pelanggan berisiko churn, dan prospek upsell.
      </li>
    </ol>
 
  </section>
 
 
  <!-- FOOTER -->
  <hr style="border: none; border-top: 1px solid #ccc; margin: 40px 0 20px;">
  <p style="font-size: 13px; color: #888; text-align: center; font-family: 'Courier New', monospace;">
    Laporan EDA · Retail Sales Dataset · Python (Pandas, Matplotlib, Seaborn)
  </p>
 
</body>
</html>
