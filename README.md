<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>EDA Retail Sales Dataset - Laporan Analisis</title>
</head>
<body>
 
<h1>📊 Laporan EDA — Retail Sales Dataset</h1>
 
<blockquote>
<p><strong>Dataset:</strong> 1.000 transaksi retail | <strong>Kolom:</strong> 9 | <strong>Tools:</strong> Python, Pandas, Matplotlib, Seaborn</p>
</blockquote>
 
<hr>
 
<h2>Daftar Isi</h2>
 
<ol>
  <li><a href="#pendahuluan">Pendahuluan</a></li>
  <li><a href="#struktur-data">Struktur Data</a></li>
  <li><a href="#kualitas-data">Kualitas Data</a></li>
  <li><a href="#statistik-deskriptif">Statistik Deskriptif</a></li>
  <li><a href="#distribusi-variabel">Distribusi Variabel Numerik</a></li>
  <li><a href="#outliers">Deteksi Outliers</a></li>
  <li><a href="#analisis-gender">Analisis Berdasarkan Gender</a></li>
  <li><a href="#analisis-kategori">Analisis Berdasarkan Kategori Produk</a></li>
  <li><a href="#analisis-temporal">Analisis Temporal</a></li>
  <li><a href="#analisis-usia">Analisis Berdasarkan Kelompok Usia</a></li>
  <li><a href="#gender-kategori">Hubungan Gender dan Kategori Produk</a></li>
  <li><a href="#korelasi">Korelasi Fitur Numerik</a></li>
  <li><a href="#ringkasan">Ringkasan &amp; Kesimpulan</a></li>
</ol>
 
<hr>
 
<h2 id="pendahuluan">1. Pendahuluan</h2>
 
<p>Notebook ini melakukan <strong>Exploratory Data Analysis (EDA)</strong> pada dataset transaksi retail yang berisi informasi pelanggan, kategori produk, kuantitas, harga, dan total pembelian. Fokus analisis meliputi:</p>
 
<ul>
  <li>Memahami pola distribusi data numerik</li>
  <li>Segmentasi pelanggan berdasarkan gender dan usia</li>
  <li>Tren temporal penjualan bulanan</li>
  <li>Performa setiap kategori produk</li>
</ul>
 
<hr>
 
<h2 id="struktur-data">2. Struktur Data</h2>
 
<p>Dataset memiliki <strong>1.000 baris</strong> dan <strong>9 kolom</strong> sebagai berikut:</p>
 
<table>
  <thead>
    <tr>
      <th>Kolom</th>
      <th>Tipe Data</th>
      <th>Deskripsi</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>Transaction ID</code></td>
      <td>object</td>
      <td>ID unik setiap transaksi</td>
    </tr>
    <tr>
      <td><code>Date</code></td>
      <td>datetime</td>
      <td>Tanggal transaksi (dikonversi ke datetime)</td>
    </tr>
    <tr>
      <td><code>Customer ID</code></td>
      <td>object</td>
      <td>ID unik pelanggan</td>
    </tr>
    <tr>
      <td><code>Gender</code></td>
      <td>object</td>
      <td>Jenis kelamin pelanggan (Male / Female)</td>
    </tr>
    <tr>
      <td><code>Age</code></td>
      <td>int64</td>
      <td>Usia pelanggan (18–64 tahun)</td>
    </tr>
    <tr>
      <td><code>Product Category</code></td>
      <td>object</td>
      <td>Kategori produk: Beauty, Clothing, Electronics</td>
    </tr>
    <tr>
      <td><code>Quantity</code></td>
      <td>int64</td>
      <td>Jumlah produk yang dibeli (1–4 unit)</td>
    </tr>
    <tr>
      <td><code>Price per Unit</code></td>
      <td>float64</td>
      <td>Harga satuan produk (25–500)</td>
    </tr>
    <tr>
      <td><code>Total Amount</code></td>
      <td>float64</td>
      <td>Total nilai transaksi (25–2.000)</td>
    </tr>
  </tbody>
</table>
 
<h3>Kode: Import dan Load Data</h3>
 
<pre><code>import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
 
sns.set(style="whitegrid")
 
df = pd.read_csv("retail_sales_dataset.csv")
print("Shape:", df.shape)  # Output: (1000, 9)
df.head()
</code></pre>
 
<p>Setelah load, kolom <code>Date</code> dikonversi ke format <code>datetime</code> dan diturunkan fitur temporal tambahan:</p>
 
<pre><code>df["Date"]       = pd.to_datetime(df["Date"])
df["year"]       = df["Date"].dt.year
df["month"]      = df["Date"].dt.month
df["day"]        = df["Date"].dt.day
df["month_name"] = df["Date"].dt.strftime("%b")
</code></pre>
 
<hr>
 
<h2 id="kualitas-data">3. Kualitas Data</h2>
 
<p>Pengecekan integritas data menunjukkan dataset dalam kondisi <strong>bersih</strong>:</p>
 
<table>
  <thead>
    <tr>
      <th>Pemeriksaan</th>
      <th>Hasil</th>
      <th>Keterangan</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Missing Values</td>
      <td>✅ 0</td>
      <td>Tidak ada nilai kosong di seluruh kolom</td>
    </tr>
    <tr>
      <td>Data Duplikat</td>
      <td>✅ 0</td>
      <td>Tidak ada baris duplikat</td>
    </tr>
    <tr>
      <td>Nilai unik Gender</td>
      <td>✅ 2</td>
      <td>Hanya <code>Male</code> dan <code>Female</code></td>
    </tr>
    <tr>
      <td>Nilai unik Kategori</td>
      <td>✅ 3</td>
      <td>Beauty, Clothing, Electronics</td>
    </tr>
  </tbody>
</table>
 
<blockquote>
<p>✅ <strong>Dataset siap dianalisis</strong> — tidak memerlukan proses data cleaning atau imputation.</p>
</blockquote>
 
<hr>
 
<h2 id="statistik-deskriptif">4. Statistik Deskriptif</h2>
 
<p>Ringkasan statistik untuk kolom numerik utama:</p>
 
<table>
  <thead>
    <tr>
      <th>Statistik</th>
      <th>Age</th>
      <th>Quantity</th>
      <th>Price per Unit</th>
      <th>Total Amount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Min</strong></td>
      <td>18</td>
      <td>1</td>
      <td>25</td>
      <td>25</td>
    </tr>
    <tr>
      <td><strong>Max</strong></td>
      <td>64</td>
      <td>4</td>
      <td>500</td>
      <td>2.000</td>
    </tr>
    <tr>
      <td><strong>Rentang</strong></td>
      <td>46 tahun</td>
      <td>3 unit</td>
      <td>475</td>
      <td>1.975</td>
    </tr>
  </tbody>
</table>
 
<ul>
  <li><strong>Age</strong>: Pelanggan berusia 18 hingga 64 tahun — mencakup segmen dewasa muda hingga lansia awal.</li>
  <li><strong>Quantity</strong>: Pembelian hanya 1–4 unit per transaksi, menandakan transaksi individual bukan grosir.</li>
  <li><strong>Price per Unit</strong>: Harga satuan bervariasi besar (25 hingga 500) — mencerminkan perbedaan kategori produk.</li>
  <li><strong>Total Amount</strong>: Nilai transaksi antara 25 hingga 2.000, sangat dipengaruhi harga satuan dan kuantitas.</li>
</ul>
 
<hr>
 
<h2 id="distribusi-variabel">5. Distribusi Variabel Numerik</h2>
 
<h3>5.1 Distribusi Total Amount</h3>
 
<pre><code>sns.histplot(df["Total Amount"], bins=30, kde=True, color="steelblue")
plt.title("Distribusi Total Amount Transaksi")
</code></pre>
 
<p><strong>Temuan:</strong> Distribusi <code>Total Amount</code> bersifat <em>multimodal</em> — tidak membentuk satu puncak tunggal melainkan beberapa kelompok. Hal ini terjadi karena harga satuan produk bersifat diskret (25, 30, 50, 300, 500), sehingga total transaksi juga mengelompok di nilai-nilai tertentu.</p>
 
<h3>5.2 Distribusi Usia Pelanggan</h3>
 
<pre><code>sns.histplot(df["Age"], bins=20, kde=True, color="coral")
plt.title("Distribusi Usia Pelanggan")
</code></pre>
 
<p><strong>Temuan:</strong> Sebaran usia pelanggan cukup merata di rentang 18–64 tahun, mengindikasikan bahwa toko ini melayani berbagai kelompok usia tanpa dominasi yang ekstrem pada satu segmen tertentu.</p>
 
<h3>5.3 Distribusi Quantity</h3>
 
<pre><code>quantity_count = df["Quantity"].value_counts().sort_index()
quantity_count.plot(kind="bar", color="mediumseagreen")
</code></pre>
 
<p><strong>Temuan:</strong> Karena <code>Quantity</code> hanya bernilai 1–4, bar chart digunakan. Distribusinya relatif seimbang di antara keempat nilai tersebut, artinya tidak ada preferensi kuat terhadap jumlah pembelian tertentu.</p>
 
<hr>
 
<h2 id="outliers">6. Deteksi Outliers</h2>
 
<p>Boxplot digunakan untuk mendeteksi outlier pada tiga variabel numerik:</p>
 
<pre><code>fig, axes = plt.subplots(1, 3, figsize=(12, 5))
for ax, col in zip(axes, ["Age", "Price per Unit", "Total Amount"]):
    sns.boxplot(y=df[col], ax=ax)
    ax.set_title(f"Boxplot {col}")
</code></pre>
 
<table>
  <thead>
    <tr>
      <th>Variabel</th>
      <th>Outlier?</th>
      <th>Penjelasan</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>Age</code></td>
      <td>Tidak</td>
      <td>Distribusi normal, tidak ada nilai ekstrem</td>
    </tr>
    <tr>
      <td><code>Price per Unit</code></td>
      <td>Terlihat lebar</td>
      <td>Wajar — produk Electronics jauh lebih mahal dari Beauty</td>
    </tr>
    <tr>
      <td><code>Total Amount</code></td>
      <td>Terlihat lebar</td>
      <td>Dampak langsung dari variasi harga satuan antar kategori</td>
    </tr>
  </tbody>
</table>
 
<blockquote>
<p>⚠️ Nilai-nilai ekstrem pada <code>Price per Unit</code> dan <code>Total Amount</code> <strong>bukan anomali</strong>, melainkan mencerminkan perbedaan harga yang nyata antar kategori produk.</p>
</blockquote>
 
<hr>
 
<h2 id="analisis-gender">7. Analisis Berdasarkan Gender</h2>
 
<pre><code>gender_count = df["Gender"].value_counts()
gender_sales = df.groupby("Gender")["Total Amount"].sum()
gender_avg   = df.groupby("Gender")["Total Amount"].mean()
</code></pre>
 
<h3>Jumlah Transaksi &amp; Total Penjualan per Gender</h3>
 
<table>
  <thead>
    <tr>
      <th>Metrik</th>
      <th>Female</th>
      <th>Male</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Jumlah Transaksi</td>
      <td>~510 (~51%)</td>
      <td>~490 (~49%)</td>
    </tr>
    <tr>
      <td>Total Penjualan</td>
      <td>Sedikit lebih tinggi</td>
      <td>Sedikit lebih rendah</td>
    </tr>
    <tr>
      <td>Rata-rata Transaksi</td>
      <td>Hampir setara</td>
      <td>Hampir setara</td>
    </tr>
  </tbody>
</table>
 
<p><strong>Temuan:</strong> Proporsi transaksi antara Female dan Male hampir seimbang (~51% vs ~49%). Tidak ada perbedaan signifikan pada rata-rata nilai transaksi per gender, menunjukkan daya beli yang relatif setara. Namun secara total volume, pelanggan Female sedikit mendominasi.</p>
 
<hr>
 
<h2 id="analisis-kategori">8. Analisis Berdasarkan Kategori Produk</h2>
 
<pre><code>category_sales = df.groupby("Product Category")["Total Amount"].sum().sort_values(ascending=False)
category_count = df["Product Category"].value_counts()
stats_category = df.groupby("Product Category")["Total Amount"].describe()
</code></pre>
 
<h3>Ringkasan Performa Kategori</h3>
 
<table>
  <thead>
    <tr>
      <th>Kategori</th>
      <th>Jumlah Transaksi</th>
      <th>Total Penjualan</th>
      <th>Rata-rata/Transaksi</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Electronics</strong></td>
      <td>~342</td>
      <td>Tertinggi</td>
      <td>Tertinggi (harga satuan besar)</td>
    </tr>
    <tr>
      <td><strong>Clothing</strong></td>
      <td>~351</td>
      <td>Menengah</td>
      <td>Menengah</td>
    </tr>
    <tr>
      <td><strong>Beauty</strong></td>
      <td>~307</td>
      <td>Terendah</td>
      <td>Terendah (harga satuan kecil)</td>
    </tr>
  </tbody>
</table>
 
<p><strong>Temuan penting:</strong></p>
<ul>
  <li><strong>Electronics</strong> menghasilkan total revenue tertinggi meskipun jumlah transaksinya tidak terbanyak — karena harga satuannya jauh lebih tinggi.</li>
  <li><strong>Clothing</strong> memiliki frekuensi transaksi terbanyak, namun revenue per transaksi lebih rendah.</li>
  <li><strong>Beauty</strong> adalah kategori dengan transaksi paling sedikit dan revenue terendah.</li>
  <li>Ini menunjukkan bahwa <em>volume transaksi ≠ revenue</em> — strategi pricing sangat berpengaruh.</li>
</ul>
 
<hr>
 
<h2 id="analisis-temporal">9. Analisis Temporal</h2>
 
<h3>9.1 Jumlah Transaksi per Bulan</h3>
 
<pre><code>orders_per_month = df.groupby("month_name")["Transaction ID"].count().reindex(month_order)
orders_per_month.plot(kind="bar", color="steelblue")
</code></pre>
 
<p><strong>Temuan:</strong> Distribusi transaksi sepanjang tahun relatif merata tanpa lonjakan ekstrem. Beberapa bulan menunjukkan sedikit peningkatan yang bisa mengindikasikan adanya musim belanja atau periode promosi tertentu.</p>
 
<h3>9.2 Total Penjualan per Bulan</h3>
 
<pre><code>sales_per_month = df.groupby("month_name")["Total Amount"].sum().reindex(month_order)
sales_per_month.plot(marker="o", color="darkorange", linewidth=2)
</code></pre>
 
<p><strong>Temuan:</strong> Tren penjualan bulanan cukup stabil. Line chart membantu mengidentifikasi bulan dengan revenue terbaik dan terlemah untuk perencanaan stok dan anggaran promosi.</p>
 
<h3>9.3 Tren per Bulan per Kategori</h3>
 
<pre><code>pivot_monthly = df.pivot_table(index="month_name", columns="Product Category",
                               values="Total Amount", aggfunc="sum").reindex(month_order)
pivot_monthly.plot(marker="o")
</code></pre>
 
<p><strong>Temuan:</strong> Electronics secara konsisten mendominasi total penjualan bulanan. Clothing dan Beauty memiliki tren yang lebih stabil dan saling berdekatan. Fluktuasi antar bulan terjadi pada semua kategori namun tidak berpola musiman yang jelas.</p>
 
<hr>
 
<h2 id="analisis-usia">10. Analisis Berdasarkan Kelompok Usia</h2>
 
<pre><code>bins   = [17, 25, 35, 45, 55, 65]
labels = ["18-25", "26-35", "36-45", "46-55", "56-64"]
df["Age Group"] = pd.cut(df["Age"], bins=bins, labels=labels)
 
age_sales = df.groupby("Age Group", observed=True)["Total Amount"].sum()
age_count = df.groupby("Age Group", observed=True)["Transaction ID"].count()
</code></pre>
 
<h3>Jumlah Transaksi &amp; Total Penjualan per Kelompok Usia</h3>
 
<table>
  <thead>
    <tr>
      <th>Kelompok Usia</th>
      <th>Jumlah Transaksi</th>
      <th>Kontribusi Penjualan</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>18–25</td>
      <td>~200</td>
      <td>Menengah</td>
    </tr>
    <tr>
      <td>26–35</td>
      <td>~200</td>
      <td>Menengah</td>
    </tr>
    <tr>
      <td>36–45</td>
      <td>~200</td>
      <td>Menengah</td>
    </tr>
    <tr>
      <td>46–55</td>
      <td>~200</td>
      <td>Menengah</td>
    </tr>
    <tr>
      <td>56–64</td>
      <td>~200</td>
      <td>Menengah</td>
    </tr>
  </tbody>
</table>
 
<p><strong>Temuan:</strong> Distribusi transaksi antar kelompok usia sangat merata, menandakan bahwa toko ini berhasil menjangkau <em>semua segmen usia</em> secara setara. Tidak ada kelompok usia yang mendominasi secara signifikan.</p>
 
<h3>10.1 Preferensi Kategori per Kelompok Usia</h3>
 
<pre><code>pivot_age = df.pivot_table(index="Age Group", columns="Product Category",
                            values="Total Amount", aggfunc="sum", observed=True)
pivot_age.plot(kind="bar", colormap="Set2")
</code></pre>
 
<p><strong>Temuan:</strong> Electronics mendominasi pengeluaran di semua kelompok usia. Preferensi relatif terhadap Beauty dan Clothing berbeda-beda antar segmen usia, memberikan peluang untuk personalisasi kampanye pemasaran.</p>
 
<hr>
 
<h2 id="gender-kategori">11. Hubungan Gender dan Kategori Produk</h2>
 
<pre><code>pivot_gender_cat = df.pivot_table(index="Gender", columns="Product Category",
                               values="Total Amount", aggfunc="sum")
pivot_gender_cat.plot(kind="bar", colormap="Set1")
</code></pre>
 
<p><strong>Temuan:</strong></p>
<ul>
  <li><strong>Electronics</strong> adalah kategori dengan total penjualan tertinggi untuk <em>kedua gender</em>.</li>
  <li><strong>Female</strong> menunjukkan pengeluaran yang sedikit lebih tinggi pada kategori <strong>Beauty</strong> dan <strong>Clothing</strong>.</li>
  <li><strong>Male</strong> cenderung lebih banyak berkontribusi pada kategori <strong>Electronics</strong>.</li>
  <li>Pola ini konsisten dengan ekspektasi umum perilaku belanja berdasarkan gender.</li>
</ul>
 
<hr>
 
<h2 id="korelasi">12. Korelasi Fitur Numerik</h2>
 
<pre><code>num_cols = ["Age", "Quantity", "Price per Unit", "Total Amount"]
corr = df[num_cols].corr()
 
sns.heatmap(corr, annot=True, cmap="coolwarm", fmt=".2f", linewidths=0.5)
plt.title("Korelasi Fitur Numerik")
</code></pre>
 
<h3>Interpretasi Korelasi</h3>
 
<table>
  <thead>
    <tr>
      <th>Pasangan Variabel</th>
      <th>Korelasi</th>
      <th>Interpretasi</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Price per Unit ↔ Total Amount</td>
      <td>Tinggi positif</td>
      <td>Wajar: harga lebih tinggi → total lebih besar</td>
    </tr>
    <tr>
      <td>Quantity ↔ Total Amount</td>
      <td>Tinggi positif</td>
      <td>Wajar: lebih banyak unit → total lebih besar</td>
    </tr>
    <tr>
      <td>Age ↔ Total Amount</td>
      <td>Rendah</td>
      <td>Usia tidak berhubungan kuat dengan nilai transaksi</td>
    </tr>
    <tr>
      <td>Age ↔ Quantity</td>
      <td>Sangat rendah</td>
      <td>Usia tidak memengaruhi jumlah unit yang dibeli</td>
    </tr>
  </tbody>
</table>
 
<blockquote>
<p>💡 Korelasi tinggi antara <code>Price per Unit</code>, <code>Quantity</code>, dan <code>Total Amount</code> adalah <strong>matematis</strong> — karena <code>Total Amount = Quantity × Price per Unit</code>.</p>
</blockquote>
 
<h3>Scatter Plot: Quantity vs Total Amount</h3>
 
<pre><code>sns.scatterplot(data=df, x="Quantity", y="Total Amount",
                hue="Product Category", alpha=0.6, palette="Set2")
</code></pre>
 
<p><strong>Temuan:</strong> Scatter plot memperlihatkan <em>kelompok-kelompok harga</em> yang jelas. Electronics dan Clothing menempati rentang Total Amount yang jauh lebih tinggi dibandingkan Beauty, karena harga satuan yang berbeda signifikan.</p>
 
<hr>
 
<h2 id="ringkasan">13. Ringkasan &amp; Kesimpulan</h2>
 
<h3>Temuan Utama</h3>
 
<table>
  <thead>
    <tr>
      <th>Area Analisis</th>
      <th>Temuan Kunci</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Kualitas Data</strong></td>
      <td>Dataset bersih: 0 missing values, 0 duplikat, tipe data konsisten</td>
    </tr>
    <tr>
      <td><strong>Distribusi</strong></td>
      <td>Total Amount multimodal akibat harga diskret; Quantity merata di 1–4 unit</td>
    </tr>
    <tr>
      <td><strong>Gender</strong></td>
      <td>Proporsi transaksi hampir setara (~51% Female, ~49% Male); daya beli serupa</td>
    </tr>
    <tr>
      <td><strong>Kategori Produk</strong></td>
      <td>Electronics = revenue tertinggi; Clothing = transaksi terbanyak; Beauty = terendah keduanya</td>
    </tr>
    <tr>
      <td><strong>Tren Temporal</strong></td>
      <td>Penjualan stabil sepanjang tahun tanpa musim belanja yang jelas</td>
    </tr>
    <tr>
      <td><strong>Kelompok Usia</strong></td>
      <td>Semua segmen usia berkontribusi merata; Electronics dominan di semua kelompok</td>
    </tr>
    <tr>
      <td><strong>Gender × Kategori</strong></td>
      <td>Female lebih ke Beauty &amp; Clothing; Male lebih ke Electronics</td>
    </tr>
    <tr>
      <td><strong>Korelasi</strong></td>
      <td>Total Amount dipengaruhi kuat oleh Price per Unit dan Quantity; Age hampir tidak berkorelasi</td>
    </tr>
  </tbody>
</table>
 
<h3>Rekomendasi Bisnis</h3>
 
<ol>
  <li><strong>Fokus pada Electronics</strong> — Kategori ini menghasilkan revenue terbesar. Pastikan stok cukup dan pertimbangkan bundling promosi.</li>
  <li><strong>Strategi berbasis Gender</strong> — Kampanye Beauty dan Clothing lebih efektif diarahkan ke Female, sementara Electronics ke Male.</li>
  <li><strong>Personalisasi per Usia</strong> — Meski semua segmen berkontribusi merata, preferensi sub-kategori berbeda antar kelompok usia.</li>
  <li><strong>Investigasi Musim</strong> — Data perlu dikumpulkan lebih dari 1 tahun untuk mengidentifikasi pola musiman yang lebih kuat.</li>
  <li><strong>Analisis Lanjutan</strong> — Pertimbangkan analisis RFM (Recency, Frequency, Monetary) untuk segmentasi pelanggan yang lebih mendalam.</li>
</ol>
 
<hr>
 
<h3>Checklist Analisis yang Telah Dilakukan</h3>
 
<ul>
  <li>✅ Load dan inspeksi struktur dataset (1.000 baris, 9 kolom)</li>
  <li>✅ Konversi kolom <code>Date</code> ke datetime dan ekstraksi fitur temporal</li>
  <li>✅ Pengecekan missing values dan data duplikat</li>
  <li>✅ Analisis distribusi Total Amount, Age, dan Quantity</li>
  <li>✅ Deteksi outlier dengan boxplot</li>
  <li>✅ Analisis pola transaksi dan penjualan berdasarkan Gender</li>
  <li>✅ Analisis komposisi dan performa berdasarkan Kategori Produk</li>
  <li>✅ Tren temporal bulanan (transaksi dan penjualan)</li>
  <li>✅ Segmentasi berdasarkan Kelompok Usia dan preferensi kategori</li>
  <li>✅ Eksplorasi kombinasi Gender × Kategori Produk</li>
  <li>✅ Korelasi antar variabel numerik dan scatter plot</li>
</ul>
 
<hr>
 
<p><em>Laporan ini dibuat dari notebook EDA Retail Sales Dataset menggunakan Python (Pandas, Matplotlib, Seaborn).</em></p>
 
</body>
</html>
