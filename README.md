<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
</head>
<body>
 
<h1>Laporan Analisis Eksplorasi Data (EDA)<br>Retail Sales Dataset</h1>
 
<hr>
 
<h2>Daftar Isi</h2>
<ol>
  <li><a href="#pendahuluan">Pendahuluan</a></li>
  <li><a href="#struktur">Struktur dan Kualitas Data</a></li>
  <li><a href="#statistik">Statistik Deskriptif</a></li>
  <li><a href="#distribusi">Distribusi Variabel Numerik</a></li>
  <li><a href="#outlier">Deteksi Outlier</a></li>
  <li><a href="#gender">Analisis Berdasarkan Gender</a></li>
  <li><a href="#kategori">Analisis Berdasarkan Kategori Produk</a></li>
  <li><a href="#temporal">Analisis Temporal Bulanan</a></li>
  <li><a href="#usia">Analisis Berdasarkan Kelompok Usia</a></li>
  <li><a href="#gender-kategori">Hubungan Gender dan Kategori Produk</a></li>
  <li><a href="#korelasi">Korelasi Antar Variabel Numerik</a></li>
  <li><a href="#kesimpulan">Kesimpulan dan Rekomendasi</a></li>
</ol>
 
<hr>
 
<h2 id="pendahuluan">1. Pendahuluan</h2>
 
<p>Laporan ini menyajikan hasil analisis eksplorasi data (Exploratory Data Analysis) terhadap dataset transaksi retail yang mencakup 1.000 baris data dan 9 kolom. Dataset memuat informasi lengkap tentang setiap transaksi, mulai dari identitas pelanggan, tanggal pembelian, jenis produk yang dibeli, hingga nilai total transaksi.</p>
 
<p>Tujuan analisis ini adalah untuk memahami karakteristik pelanggan, mengidentifikasi pola pembelian berdasarkan gender dan kelompok usia, mengukur performa tiap kategori produk, serta mengevaluasi tren penjualan secara temporal. Seluruh proses analisis dikerjakan menggunakan Python dengan memanfaatkan library Pandas untuk manipulasi data, Matplotlib dan Seaborn untuk visualisasi.</p>
 
<hr>
 
<h2 id="struktur">2. Struktur dan Kualitas Data</h2>
 
<p>Dataset terdiri dari 1.000 baris transaksi dengan 9 kolom: Transaction ID, Date, Customer ID, Gender, Age, Product Category, Quantity, Price per Unit, dan Total Amount. Kolom Date disimpan dalam format teks dan dikonversi ke format datetime sebelum digunakan untuk analisis temporal. Dari proses konversi tersebut juga diturunkan tiga kolom tambahan, yaitu year, month, dan month_name, yang digunakan untuk agregasi bulanan.</p>
 
<p>Dari sisi kualitas data, hasil pemeriksaan menunjukkan bahwa dataset ini berada dalam kondisi yang sangat baik. Tidak ditemukan satu pun nilai yang kosong (missing values) pada seluruh 13 kolom, termasuk kolom-kolom turunan. Selain itu, tidak ditemukan baris yang duplikat, sehingga setiap baris benar-benar merepresentasikan satu transaksi yang unik.</p>
 
<p>Pemeriksaan terhadap kolom kategorik juga menunjukkan konsistensi yang baik. Kolom Gender hanya berisi dua nilai yaitu Male dan Female, sementara kolom Product Category hanya berisi tiga nilai yaitu Beauty, Clothing, dan Electronics. Tidak ada nilai yang salah ketik, tidak ada spasi tersembunyi, dan tidak ada variasi penulisan yang tidak konsisten. Dengan kondisi ini, tidak diperlukan tahap data cleaning, imputasi, atau penghapusan duplikat — analisis dapat langsung dilanjutkan ke tahap eksplorasi.</p>
 
<hr>
 
<h2 id="statistik">3. Statistik Deskriptif</h2>
 
<p>Ringkasan statistik untuk empat variabel numerik utama memberikan gambaran awal yang penting sebelum masuk ke analisis yang lebih mendalam.</p>
 
<p>Variabel Age mencatat usia pelanggan dengan nilai minimum 18 tahun dan maksimum 64 tahun. Rata-rata usia pelanggan adalah 41,39 tahun dengan standar deviasi 13,68 tahun, yang menunjukkan sebaran yang cukup lebar. Nilai median (persentil ke-50) berada di angka 42 tahun, yang sangat dekat dengan rata-rata, mengindikasikan distribusi yang mendekati simetris.</p>
 
<p>Variabel Quantity hanya mengambil nilai bilangan bulat antara 1 hingga 4 unit. Rata-ratanya adalah 2,51 unit dengan standar deviasi 1,13. Distribusi nilai Quantity yang terbatas ini mencerminkan karakteristik transaksi retail konsumen akhir, bukan pembelian dalam jumlah besar seperti pada konteks grosir.</p>
 
<p>Variabel Price per Unit menunjukkan rentang yang sangat lebar, dari nilai minimum 25 hingga maksimum 500. Rata-ratanya adalah 179,89 dengan standar deviasi 189,68 — nilai standar deviasi yang bahkan melebihi setengah dari rata-rata, menandakan tingkat variabilitas yang sangat tinggi. Perbedaan harga ini secara langsung mencerminkan perbedaan antara kategori produk yang ada: produk Beauty dan Clothing cenderung lebih murah, sementara Electronics jauh lebih mahal.</p>
 
<p>Variabel Total Amount memiliki rentang dari 25 hingga 2.000, dengan rata-rata 456 dan median 135. Perbedaan yang cukup besar antara rata-rata dan median mengindikasikan distribusi yang condong ke kanan (right-skewed), di mana sebagian besar transaksi bernilai relatif kecil namun ada sejumlah transaksi bernilai sangat besar yang menarik rata-rata ke atas.</p>
 
<hr>
 
<h2 id="distribusi">4. Distribusi Variabel Numerik</h2>
 
<p>Distribusi Total Amount divisualisasikan menggunakan histogram dengan 30 bin dan estimasi kepadatan kernel (KDE). Hasilnya memperlihatkan pola distribusi yang tidak membentuk satu puncak tunggal, melainkan beberapa puncak di posisi-posisi tertentu. Pola ini disebut distribusi multimodal dan terjadi karena variabel Price per Unit dalam dataset ini hanya mengambil beberapa nilai diskret yang terbatas, yaitu 25, 30, 50, 300, dan 500. Ketika nilai-nilai ini dikalikan dengan Quantity yang berkisar antara 1 hingga 4, total transaksi yang dihasilkan pun mengelompok di titik-titik tertentu, bukan tersebar secara mulus dan kontinu. Pola ini bukan indikasi masalah pada data, melainkan mencerminkan struktur pricing yang memang demikian adanya.</p>
 
<p>Distribusi usia pelanggan memperlihatkan sebaran yang relatif datar dan merata di seluruh rentang usia 18 hingga 64 tahun. Tidak ada satu kelompok usia pun yang terlihat jauh mendominasi dibanding yang lain. Ini adalah sinyal positif yang menunjukkan bahwa toko ini berhasil menjangkau pelanggan dari berbagai generasi secara proporsional.</p>
 
<p>Untuk variabel Quantity, karena hanya mengambil empat nilai bulat (1, 2, 3, 4), digunakan bar chart daripada histogram. Hasilnya menunjukkan frekuensi yang sangat seimbang: sebanyak 253 transaksi membeli 1 unit, 243 transaksi membeli 2 unit, 241 transaksi membeli 3 unit, dan 263 transaksi membeli 4 unit. Ketidakhadiran preferensi yang jelas terhadap jumlah unit tertentu menunjukkan bahwa keputusan berapa banyak yang dibeli tampaknya bersifat acak atau sangat situasional.</p>
 
<hr>
 
<h2 id="outlier">5. Deteksi Outlier</h2>
 
<p>Deteksi outlier dilakukan menggunakan boxplot pada tiga variabel numerik: Age, Price per Unit, dan Total Amount.</p>
 
<p>Boxplot untuk variabel Age menunjukkan distribusi yang bersih tanpa adanya nilai yang jauh terpencil di luar batas atas maupun bawah. Hal ini konsisten dengan fakta bahwa usia pelanggan terdistribusi merata di rentang 18 hingga 64 tahun.</p>
 
<p>Boxplot untuk Price per Unit dan Total Amount memperlihatkan kotak (box) yang relatif sempit di bagian bawah namun dengan distribusi yang sangat lebar ke atas. Kondisi ini dapat terlihat seperti adanya outlier, namun sebenarnya bukan. Pola ini terjadi karena adanya perbedaan harga yang sangat signifikan antar kategori produk: produk Beauty memiliki harga satuan yang rendah (25–50), sedangkan Electronics bisa mencapai 300–500. Ketika kedua kategori ini digabungkan dalam satu distribusi, produk Electronics secara statistik terlihat seperti "outlier" padahal mereka adalah nilai yang valid dan merupakan bagian penting dari bisnis.</p>
 
<p>Dengan demikian, tidak ada satu pun nilai yang perlu dihapus atau diperlakukan sebagai anomali. Nilai-nilai ekstrem tersebut merupakan representasi yang sah dari produk-produk dengan harga tinggi, dan membuang mereka justru akan mengakibatkan distorsi pada analisis performa kategori produk.</p>
 
<hr>
 
<h2 id="gender">6. Analisis Berdasarkan Gender</h2>
 
<p>Dari total 1.000 transaksi, sebanyak 510 transaksi (51%) dilakukan oleh pelanggan Female dan 490 transaksi (49%) oleh pelanggan Male. Proporsi ini hampir seimbang, dengan selisih yang sangat kecil.</p>
 
<p>Pola serupa terlihat pada total nilai penjualan. Pelanggan Female menyumbang total penjualan sebesar 232.840, sedangkan pelanggan Male sebesar 223.160. Perbedaan sekitar 4,3% ini sejalan dengan perbedaan jumlah transaksi dan tidak mengindikasikan perbedaan yang signifikan dalam daya beli.</p>
 
<p>Konfirmasi lebih lanjut datang dari perbandingan rata-rata nilai transaksi per gender. Rata-rata Total Amount untuk pelanggan Female adalah 456,55, sementara untuk pelanggan Male adalah 455,43. Perbedaan yang hanya sebesar 1,12 ini secara praktis tidak berarti apa-apa. Artinya, dalam satu kali belanja, pelanggan pria dan wanita mengeluarkan jumlah uang yang hampir identik.</p>
 
<p>Kesimpulannya, tidak ada disparitas berarti antara pelanggan pria dan wanita dari segi volume maupun nilai transaksi. Namun, perbedaan yang lebih nuansir akan muncul ketika kita melihat jenis produk yang mereka pilih, yang akan dibahas pada bagian tersendiri.</p>
 
<hr>
 
<h2 id="kategori">7. Analisis Berdasarkan Kategori Produk</h2>
 
<p>Ketiga kategori produk — Electronics, Clothing, dan Beauty — dianalisis dari dua dimensi yang berbeda: frekuensi transaksi dan total revenue yang dihasilkan.</p>
 
<p>Dari sisi frekuensi transaksi, Clothing menempati posisi teratas dengan 351 transaksi, diikuti Electronics dengan 342 transaksi, dan Beauty di posisi terakhir dengan 307 transaksi. Selisih antara Clothing dan Beauty adalah 44 transaksi atau sekitar 14%.</p>
 
<p>Namun gambaran yang berbeda muncul ketika kita melihat total revenue. Electronics menghasilkan total penjualan sebesar 156.905, Clothing sebesar 155.580, dan Beauty sebesar 143.515. Meskipun Electronics bukan kategori dengan transaksi terbanyak, ia mampu menghasilkan revenue tertinggi karena harga satuan produknya jauh lebih tinggi dibanding dua kategori lainnya.</p>
 
<p>Ketika dilihat dari rata-rata nilai transaksi per kategori, Beauty justru mencatat rata-rata tertinggi sebesar 467,48, diikuti Electronics sebesar 458,79, dan Clothing di posisi terakhir dengan 443,25. Ini adalah temuan yang cukup mengejutkan karena secara keseluruhan Beauty menghasilkan revenue terendah. Penjelasannya adalah bahwa meskipun rata-rata transaksi individual Beauty relatif tinggi, jumlah transaksinya yang paling sedikit membuat akumulasi total revenue-nya tetap menjadi yang terkecil.</p>
 
<p>Pola ini membawa implikasi penting: sebuah kategori dengan rata-rata nilai transaksi tinggi sekalipun bisa menghasilkan total revenue yang rendah jika frekuensi pembeliannya rendah. Untuk meningkatkan revenue Beauty, strategi yang paling efektif adalah meningkatkan frekuensi pembelian, bukan hanya mempertahankan nilai rata-rata transaksinya.</p>
 
<hr>
 
<h2 id="temporal">8. Analisis Temporal Bulanan</h2>
 
<p>Analisis temporal dilakukan dengan mengagregasi data berdasarkan nama bulan menggunakan kolom month_name yang diekstrak dari kolom Date. Seluruh data berasal dari tahun 2023.</p>
 
<p>Distribusi jumlah transaksi per bulan menunjukkan pola yang relatif stabil sepanjang tahun. Tidak terdapat lonjakan atau penurunan yang ekstrem di bulan-bulan tertentu. Penjualan berlangsung cukup merata dari Januari hingga Desember tanpa ada satu bulan pun yang secara dramatis melampaui atau jauh tertinggal dari bulan lainnya.</p>
 
<p>Tren total penjualan bulanan memperlihatkan fluktuasi yang wajar namun tanpa arah tren yang jelas ke atas maupun ke bawah secara keseluruhan. Artinya bisnis ini tidak sedang mengalami pertumbuhan yang signifikan, namun juga tidak sedang dalam tren penurunan. Penjualan berjalan di kisaran yang stabil sepanjang tahun.</p>
 
<p>Ketika tren penjualan dipecah per kategori produk, terlihat bahwa ketiga kategori mengalami fluktuasi di bulan-bulan yang berbeda tanpa pola yang selaras. Naik-turunnya penjualan Electronics di suatu bulan tidak selalu diikuti atau diimbangi oleh pergerakan serupa pada Clothing maupun Beauty. Ini menunjukkan bahwa faktor yang memengaruhi pembelian tiap kategori bersifat independen satu sama lain.</p>
 
<p>Perlu dicatat bahwa analisis musiman yang lebih andal memerlukan data dari lebih dari satu tahun. Dengan hanya satu tahun data, sulit untuk membedakan antara fluktuasi acak dan pola musiman yang berulang setiap tahunnya.</p>
 
<hr>
 
<h2 id="usia">9. Analisis Berdasarkan Kelompok Usia</h2>
 
<p>Pelanggan dikelompokkan ke dalam lima segmen usia menggunakan fungsi pd.cut(): kelompok 18–25 tahun, 26–35 tahun, 36–45 tahun, 46–55 tahun, dan 56–64 tahun.</p>
 
<p>Hasil yang paling menonjol dari analisis ini adalah betapa meratanya distribusi transaksi di seluruh kelompok usia. Masing-masing kelompok menyumbang sekitar 200 transaksi, yang berarti setiap kelompok usia berkontribusi hampir tepat 20% dari total volume transaksi. Pola ini konsisten dengan temuan dari distribusi usia yang sudah dibahas sebelumnya, yaitu bahwa sebaran usia pelanggan memang cukup datar dan tidak ada satu generasi pun yang mendominasi.</p>
 
<p>Pola yang sama berlaku untuk total penjualan per kelompok usia — kontribusinya pun relatif seimbang antar segmen. Tidak ada satu kelompok usia yang secara signifikan menghasilkan revenue jauh lebih besar dibanding yang lain.</p>
 
<p>Ketika preferensi kategori produk dianalisis per kelompok usia, Electronics tetap menjadi kategori dengan total penjualan tertinggi di hampir semua segmen usia. Namun proporsi relatif antara Clothing dan Beauty bervariasi antar kelompok. Variasi ini, meskipun tidak dramatis, memberikan peluang untuk menyusun kampanye pemasaran yang sedikit lebih terpersonalisasi berdasarkan kelompok usia target.</p>
 
<hr>
 
<h2 id="gender-kategori">10. Hubungan Gender dan Kategori Produk</h2>
 
<p>Analisis pivot table yang menggabungkan dimensi Gender dan Product Category menghasilkan temuan yang lebih menarik dibanding analisis gender saja.</p>
 
<p>Untuk kategori Beauty, pelanggan Female menyumbang total penjualan sebesar 74.830, sedangkan pelanggan Male hanya 68.685. Selisih sekitar 8,9% ini menunjukkan bahwa produk kecantikan sedikit lebih diminati oleh pelanggan wanita, konsisten dengan ekspektasi umum.</p>
 
<p>Untuk kategori Clothing, pelanggan Female menyumbang 81.275 dan pelanggan Male 74.305. Perbedaan sebesar 9,4% kembali menempatkan pelanggan Female sebagai kelompok yang lebih dominan, sesuai dengan pola umum konsumsi fashion.</p>
 
<p>Yang menarik terjadi pada kategori Electronics. Di sini pola berbalik: pelanggan Male menyumbang total penjualan sebesar 80.170, sementara pelanggan Female hanya 76.735. Pelanggan pria lebih aktif membeli produk elektronik dibanding wanita, dengan selisih sekitar 4,5%.</p>
 
<p>Secara keseluruhan, temuan ini memperlihatkan bahwa meskipun total pengeluaran pria dan wanita hampir setara (seperti yang sudah dibahas di bagian sebelumnya), <em>komposisi produk</em> yang mereka beli berbeda secara bermakna. Perbedaan ini dapat dimanfaatkan untuk menargetkan komunikasi pemasaran yang lebih relevan: promosi kecantikan dan pakaian lebih efektif diarahkan ke segmen Female, sementara kampanye elektronik lebih tepat menyasar segmen Male.</p>
 
<hr>
 
<h2 id="korelasi">11. Korelasi Antar Variabel Numerik</h2>
 
<p>Analisis korelasi Pearson dilakukan terhadap empat variabel numerik: Age, Quantity, Price per Unit, dan Total Amount.</p>
 
<p>Korelasi yang paling tinggi ditemukan antara Price per Unit dengan Total Amount, dan antara Quantity dengan Total Amount. Kedua hubungan ini adalah konsekuensi matematis langsung dari definisi variabel Total Amount, yang secara definitif merupakan hasil perkalian antara Quantity dan Price per Unit. Oleh karena itu, korelasi tinggi ini bukan temuan yang substantif secara analitis — ia hanya mengkonfirmasi konsistensi internal data.</p>
 
<p>Temuan yang lebih bermakna justru ada pada korelasi yang rendah. Variabel Age menunjukkan korelasi yang sangat lemah terhadap semua variabel lainnya. Korelasi Age dengan Total Amount sangat mendekati nol, demikian pula korelasi Age dengan Quantity. Artinya, usia seorang pelanggan tidak dapat digunakan untuk memprediksi seberapa banyak yang mereka beli atau seberapa besar nilai transaksi mereka. Faktor penentu nilai transaksi jauh lebih bergantung pada kategori produk yang dipilih — apakah pelanggan membeli Electronics atau Beauty — bukan pada usia pembelinya.</p>
 
<p>Scatter plot antara Quantity dan Total Amount yang diberi warna berdasarkan kategori produk memperlihatkan pola yang sangat jelas. Titik-titik data mengelompok ke dalam kluster-kluster yang terpisah sesuai kategori. Electronics dan sebagian Clothing berada di rentang Total Amount yang lebih tinggi, sementara titik-titik Beauty berkonsentrasi di bagian bawah grafik. Ini mengkonfirmasi secara visual bahwa kategori produk adalah faktor paling dominan yang membentuk nilai transaksi, jauh lebih dominan dibandingkan usia atau jumlah unit yang dibeli.</p>
 
<hr>
 
<h2 id="kesimpulan">12. Kesimpulan dan Rekomendasi</h2>
 
<p>Analisis eksplorasi data terhadap 1.000 transaksi retail ini menghasilkan beberapa temuan utama yang dapat dirangkum sebagai berikut.</p>
 
<p>Pertama, dataset berada dalam kondisi bersih sempurna tanpa missing values dan tanpa duplikat, sehingga seluruh analisis dapat dilakukan tanpa bias akibat data yang cacat.</p>
 
<p>Kedua, tidak ada perbedaan yang signifikan antara pelanggan pria dan wanita dari sisi volume maupun nilai transaksi. Keduanya berbelanja dengan frekuensi dan nilai yang hampir identik. Namun perbedaan yang bermakna muncul pada komposisi produk: wanita lebih banyak membeli Beauty dan Clothing, sedangkan pria lebih banyak membeli Electronics.</p>
 
<p>Ketiga, Electronics adalah kategori yang menghasilkan revenue tertinggi meskipun bukan yang paling sering dibeli. Ini didorong oleh harga satuan yang jauh lebih tinggi dibanding dua kategori lain. Beauty adalah kategori dengan kinerja paling lemah dari sisi total revenue, meskipun rata-rata nilai per transaksinya bukan yang terendah.</p>
 
<p>Keempat, distribusi transaksi antar kelompok usia sangat merata, yang berarti toko ini berhasil melayani semua generasi secara proporsional. Usia pelanggan juga tidak berkorelasi dengan nilai transaksi, sehingga tidak dapat dijadikan prediktor untuk menentukan siapa yang akan berbelanja lebih besar.</p>
 
<p>Kelima, pola penjualan bulanan cenderung stabil sepanjang tahun tanpa indikasi musim belanja yang kuat. Namun, konfirmasi pola musiman memerlukan data dari lebih dari satu tahun.</p>
 
<p>Berdasarkan temuan-temuan di atas, beberapa rekomendasi dapat dikemukakan. Dari sisi strategi produk, perlu dilakukan evaluasi mendalam terhadap kategori Beauty — apakah masalahnya ada pada jangkauan produk, harga, atau visibilitas di toko. Dari sisi pemasaran, kampanye yang tersegmentasi berdasarkan gender akan lebih efektif dibanding kampanye umum untuk kategori-kategori tertentu. Dari sisi pengembangan analitik, langkah berikutnya yang direkomendasikan adalah analisis RFM (Recency, Frequency, Monetary) untuk mengidentifikasi pelanggan loyal, pelanggan berisiko berhenti berbelanja, dan peluang upselling. Selain itu, pengumpulan data yang mencakup lebih dari satu tahun akan memungkinkan analisis tren dan musiman yang jauh lebih dapat diandalkan.</p>
 
</body>
</html>
