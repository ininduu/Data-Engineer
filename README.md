---

# 📊 HUBUNGAN ANTARA CURAH HUJAN DENGAN PRODUKSI PADI

Sebagai seorang mahasiswa TRPL yang mengambil peran sebagai *Data Engineer*, kami tidak hanya melihat proyek ini sebagai kegiatan pengumpulan data semata, melainkan sebagai sebuah tantangan dalam membangun sistem **data pipeline** dan **arsitektur data** yang andal. Tugas utama kami adalah merancang serta mengimplementasikan infrastruktur yang mampu **mengumpulkan, memproses, dan menyajikan data secara efisien**, agar dapat dimanfaatkan oleh *Data Scientist* maupun *Business Analyst* untuk keperluan analisis lanjutan.

---

### 🔹 1. Ingesti Data (*Data Ingestion*)

Langkah awal dalam proyek ini adalah proses pengumpulan data dari berbagai sumber yang memiliki format beragam. Tantangan utama dalam tahap ini adalah bagaimana menstandarkan data dari sumber-sumber tersebut agar dapat digunakan dalam sistem terintegrasi.

* **Data Curah Hujan:**
  Diperoleh dari stasiun cuaca seperti BMKG dalam format CSV/API, atau dari data satelit seperti GPM dan TRMM yang biasanya dalam format NetCDF/HDF5. Data ini berbentuk *time-series* dan *spasial*, berbasis koordinat lintang dan bujur.

* **Data Produksi Padi:**
  Umumnya bersumber dari instansi pemerintah seperti BPS atau Kementerian Pertanian dalam format tabular (CSV atau Excel). Data ini bersifat agregat berdasarkan wilayah (provinsi/kabupaten) dan dibagi per musim tanam atau per tahun.

---

### 🔹 2. Penyimpanan Data (*Data Storage*)

Setelah data terkumpul, langkah selanjutnya adalah menyimpannya menggunakan arsitektur penyimpanan yang efisien dan dapat diskalakan.

* **Data Lake:**
  Semua data mentah disimpan terlebih dahulu di data lake (contoh: Google Cloud Storage atau AWS S3). Tujuannya agar tidak ada informasi yang hilang dan memungkinkan untuk dilakukan pemrosesan ulang apabila diperlukan.

* **Data Warehouse:**
  Data yang telah dibersihkan dan ditransformasikan kemudian dimasukkan ke dalam data warehouse (misalnya BigQuery atau Redshift) dengan rancangan skema seperti berikut:

  * **Fakta:** `fakta_produksi` → Menyimpan metrik utama seperti total hasil panen (ton) dan luas tanam (hektare).
  * **Dimensi:**

    * `dim_waktu` → Menyimpan atribut waktu (hari, bulan, tahun, musim tanam).
    * `dim_lokasi` → Menyimpan informasi lokasi (provinsi, kabupaten, koordinat).
    * `dim_cuaca` → Menyimpan data iklim yang telah diagregasi (total curah hujan, jumlah hari kering, dsb).

---

### 🔹 3. Proses ETL (*Extract, Transform, Load*)

Bagian inilah yang menjadi inti dari peran kami sebagai *Data Engineer*, yaitu membangun sistem *pipeline* yang berjalan secara otomatis dan terjadwal:

* **Extract (Ekstraksi):**
  Menjadwalkan proses pengambilan data dari sumber eksternal secara otomatis menggunakan skrip atau pipeline berbasis waktu.

* **Transform (Transformasi):**

  * *Pembersihan Data:* Mengatasi data kosong (*missing values*), outlier, dan format yang tidak konsisten.
  * *Integrasi Data:* Menggabungkan data curah hujan dan produksi padi berdasarkan dimensi waktu dan lokasi. Tantangan utama pada tahap ini adalah menyelaraskan granularitas data (harian pada cuaca vs musiman pada produksi), sehingga diperlukan agregasi seperti menghitung total curah hujan selama fase vegetatif atau jumlah hari kering berturut-turut.
  * *Enrichment:* Jika memungkinkan, kami juga menambahkan variabel lain seperti jenis tanah, ketersediaan irigasi, dan penggunaan pupuk untuk memperkaya konteks data.

* **Load (Pemuatan):**
  Data yang telah final akan dimuat ke dalam data warehouse agar dapat diakses dengan mudah oleh tim analis dan ilmuwan data.

---

### 🔹 4. Penyajian Data (*Data Serving*)

Output dari keseluruhan proses ETL ini adalah sebuah dataset yang terstruktur rapi dan dapat berfungsi sebagai **single source of truth**. Data ini siap digunakan oleh:

* **Data Scientist:** Untuk membangun model prediktif, seperti memprediksi hasil panen berdasarkan pola curah hujan.
* **Business Analyst / Pemerintah:** Untuk membuat dashboard interaktif dan mendukung proses pengambilan keputusan strategis di sektor pertanian.

---

### 🔸 Kesimpulan

Sebagai seorang *Data Engineer*, kami memandang hubungan antara curah hujan dan produksi padi tidak hanya dari sisi data teknis, melainkan sebagai misi untuk membangun **jembatan informasi** antara dunia meteorologi dan pertanian. Tantangan utamanya bukan hanya pada aspek teknis, melainkan juga bagaimana mengubah data mentah yang tidak terstruktur menjadi **aset informasi berharga** yang siap digunakan untuk meningkatkan ketahanan pangan dan kemajuan pertanian Indonesia.

---
