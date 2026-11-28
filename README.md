# Proyek Data Mining — Kelompok 5
## Pengelompokan Pola Konsentrasi Polutan Udara pada 38 Ibu Kota Provinsi di Indonesia Menggunakan Algoritma K-Means

**Mata Kuliah:** Data Mining (Semester 5)  
**Anggota Kelompok 5:**
1. **Samuel** — Pemodelan & Evaluasi K-Means + Laporan Bab III & IV (Pemodelan)
2. **Andreas** — Analisis & Visualisasi Cluster + Laporan Bab IV (Interpretasi)
3. **Elpita** — Laporan Akhir (Bab I, II, IV-EDA, V) & Slide Presentasi (PPT)

---

## 1. Gambaran Singkat Penelitian

Penelitian ini bertujuan mengelompokkan pola konsentrasi **6 polutan udara** pada **38 titik representatif ibu kota provinsi di Indonesia** selama **90 hari pengamatan** (20 Agustus 2026 – 17 November 2026) menggunakan algoritma **K-Means Clustering** (*Unsupervised Learning*).

### Spesifikasi Data & Metodologi Utama
- **Sumber Data:** Open-Meteo Air Quality API — model atmosfer **CAMS Global** (Copernicus Atmosphere Monitoring Service).
- **Batasan Penting:** Data merupakan **estimasi model atmosfer global** (resolusi spasial ~45 km), **bukan** pembacaan langsung stasiun sensor BMKG/KLHK. Label cluster hasil K-Means adalah **pengelompokan pola relatif dari dataset**, bukan kategori baku kesehatan/ISPU resmi.
- **6 Fitur Polutan (satuan µg/m³):** `pm2_5`, `pm10`, `carbon_monoxide`, `nitrogen_dioxide`, `sulphur_dioxide`, `ozone`.
- **Unit Analisis Baseline:** Rata-rata harian per kota (*city-day*) = **38 lokasi × 90 hari = 3.420 baris pengamatan**.
- **Standardisasi Baseline:** `StandardScaler` pada 6 fitur polutan (`id_pengamatan`, `provinsi`, `kota`, dan `tanggal` dipisahkan sebagai metadata untuk interpretasi).
- **Aturan Anomali:** 1 pengamatan di Palangka Raya (21 September 2026) dengan `pm2_5 > pm10` diberi penanda `flag_pm25_gt_pm10` dan **dipertahankan** (nilai ekstrem tidak dihapus otomatis).

> **Aturan Konsistensi Kelompok:** Jangan mengubah keputusan baseline (6 fitur, StandardScaler, 3.420 baris harian) atau menimpa dataset mentah tanpa kesepakatan seluruh anggota kelompok.

---

## 2. Status Progres Saat Ini

| Tahap Pengerjaan | Status | Keterangan |
|---|---|---|
| Persiapan 38 Lokasi & Rancangan Awal | **Selesai** | Excel koordinat, rancangan penelitian, referensi tersedia di `01_PERSIAPAN/`. |
| Uji API & Pengumpulan Data 38 Lokasi | **Selesai** | 82.080 baris per jam & 27.360 baris interval 3-jam UTC (`02_DATASET/`). |
| Audit Kualitas & Cleaning Data | **Selesai** | 0 missing value, 0 duplikasi, 0 nilai negatif, 38 grid unik, 1 anomali ditandai. |
| Agregasi Harian, EDA & Standardisasi | **Selesai** | `dataset_harian.csv` (3.420 baris), `data_terstandardisasi.csv`, `standard_scaler.pkl`, 5 grafik & 3 tabel EDA. |
| **Pemodelan & Evaluasi K-Means** | **Berikutnya** | Evaluasi $K=2..8$ (Elbow, Silhouette, DBI), pemilihan $K$ final, `hasil_clustering.csv`. |
| **Interpretasi & Visualisasi Hasil** | **Belum** | Profil centroid asli (µg/m³), sebaran per kota, grafik & peta di `04_HASIL_ANALISIS/visualisasi/`. |
| **Laporan Akhir & Presentasi PPT** | **Belum** | Dokumen DOCX di `05_LAPORAN/` dan PPTX di `06_PRESENTASI/`. |

---

## 3. Struktur Folder Proyek

```text
PROYEK_KUALITAS_UDARA/
├── 01_PERSIAPAN/        # Daftar 38 lokasi (.xlsx), rancangan awal & referensi (.docx)
├── 02_DATASET/          # Dataset mentah, 3-jam, bersih, harian, terstandardisasi & scaler (.pkl)
├── 03_GOOGLE_COLAB/     # Notebook pengujian API, pengolahan data, dan (nanti) pemodelan K-Means
├── 04_HASIL_ANALISIS/
│   ├── EDA/             # Grafik & tabel statistik deskriptif serta korelasi data harian
│   └── visualisasi/     # Output grafik evaluasi K-Means, profil cluster, & peta 38 kota
├── 05_LAPORAN/          # Draf dan laporan akhir penelitian (Bab I–V)
├── 06_PRESENTASI/       # Slide presentasi kelompok (.pptx)
├── MATERI/              # Slide materi perkuliahan Data Mining dari dosen (Pertemuan 3–10)
├── TAHAPAN PROYEK/      # PDF roadmap acuan pengerjaan proyek kelompok
├── .gitignore           # Daftar file sementara/cache yang tidak masuk ke Git
└── README.md            # Panduan proyek, pembagian tugas, dan alur Git
```

---

## 4. Pembagian Tugas Kelompok (3 Anggota — Fokus File Proyek di GitHub)

Pembagian dirancang **seimbang dan adil** untuk sisa pekerjaan di dalam repositori ini. Setiap anggota memegang area folder & file kerja yang jelas sehingga bisa dikerjakan paralel tanpa bentrok (*merge conflict*), namun ketiganya tetap wajib memahami alur keseluruhan.

### 1) Samuel — Pemodelan & Evaluasi K-Means + Bab III & IV (Pemodelan)
**Folder Kerja di GitHub:** `03_GOOGLE_COLAB/`, `04_HASIL_ANALISIS/`, dan draf di `05_LAPORAN/`.

**Daftar Tugas:**
1. Melengkapi `04_HASIL_ANALISIS/catatan_cleaning.txt` (ringkasan audit pembersihan data yang sudah ada).
2. Membuat & menjalankan notebook **`03_GOOGLE_COLAB/03_Pemodelan_KMeans.ipynb`** di Google Colab memakai `data_terstandardisasi.csv` (6 fitur polutan, tanpa `id_pengamatan`):
   - Menguji kandidat jumlah cluster **$K = 2$ sampai $K = 8$** dengan **Elbow Method (SSE/Inertia)**, **Silhouette Score**, **Davies-Bouldin Index (DBI)**, dan mengecek jumlah anggota tiap cluster.
   - Setelah sepakat memilih $K$ final bersama Andreas & Elpita, melatih model K-Means final dan menyimpan output ke `04_HASIL_ANALISIS/`:
     - `tabel_evaluasi_k.csv`
     - `hasil_clustering.csv` (gabungan `dataset_harian.csv` + label cluster)
     - `centroid_cluster_asli.csv` (hasil `inverse_transform` ke µg/m³)
     - `model_kmeans_final.pkl`
   - Membuat **2 grafik evaluasi & sebaran model** di `04_HASIL_ANALISIS/visualisasi/`:
     - `01_evaluasi_elbow_silhouette.png`
     - `02_sebaran_cluster_pca.png` *(PCA khusus untuk visualisasi 2D)*
3. Menulis draf **`05_LAPORAN/Bagian_Samuel_Bab3_dan_Modeling.docx`**:
   - **Bab III (Metodologi Penelitian):** alur kerja, 38 lokasi, API CAMS Global, audit/cleaning (termasuk anomali Palangka Raya), agregasi harian (3.420 baris), `StandardScaler`, dan skenario evaluasi K-Means.
   - **Bab IV (Sub-bab Pemodelan & Evaluasi K):** penjelasan hasil Elbow, Silhouette, DBI, dan alasan pemilihan $K$ final.

---

### 2) Andreas — Analisis & Visualisasi Cluster + Bab IV (Interpretasi)
**Folder Kerja di GitHub:** `03_GOOGLE_COLAB/`, `04_HASIL_ANALISIS/visualisasi/`, dan draf di `05_LAPORAN/`.

**Daftar Tugas:**
1. Membuat & menjalankan notebook **`03_GOOGLE_COLAB/04_Analisis_dan_Visualisasi_Cluster.ipynb`** menggunakan `hasil_clustering.csv` dan `centroid_cluster_asli.csv`:
   - Menyusun tabel profil tiap cluster (`04_HASIL_ANALISIS/profil_cluster.csv`: rata-rata, median, jumlah & persentase hari, polutan dominan).
   - Menghitung komposisi cluster tiap kota untuk ke-38 ibu kota (`04_HASIL_ANALISIS/proporsi_cluster_per_kota.csv`).
   - Menganalisis perubahan pola cluster per bulan (Agustus–November 2026) dan kasus menonjol (misal Palangka Raya vs Jakarta/Bandung vs Indonesia Timur).
2. Membuat **4 grafik interpretasi** (resolusi 300 DPI) di `04_HASIL_ANALISIS/visualisasi/`:
   - `03_profil_centroid_cluster.png`
   - `04_proporsi_cluster_38_kota.png`
   - `05_peta_cluster_dominan_indonesia.png`
   - `06_tren_cluster_bulanan.png`
3. Menulis draf **`05_LAPORAN/Bagian_Andreas_Bab4_Interpretasi.docx`**:
   - **Bab IV (Sub-bab Interpretasi Profil Cluster, Sebaran 38 Kota, Tren Waktu & Keterbatasan):** memberi nama deskriptif relatif tiap cluster (bukan kategori ISPU resmi) dan menjelaskan pola dari 4 grafik di atas.

---

### 3) Elpita — Laporan Akhir (Bab I, II, IV-EDA, V) & Slide Presentasi (PPT)
**Folder Kerja di GitHub:** `05_LAPORAN/` dan `06_PRESENTASI/`.

**Daftar Tugas:**
1. Menulis bagian utama laporan di **`05_LAPORAN/Laporan_Penelitian_Kelompok5.docx`** (mengacu pada dokumen di `01_PERSIAPAN/`, `04_HASIL_ANALISIS/EDA/`, dan `MATERI/`):
   - **Bab I (Pendahuluan):** latar belakang, rumusan masalah, tujuan, manfaat, dan batasan penelitian (CAMS Global bukan sensor BMKG/KLHK; cluster bukan ISPU resmi).
   - **Bab II (Landasan Teori):** 6 polutan udara, Open-Meteo & CAMS Global, CRISP-DM, StandardScaler, K-Means, Euclidean Distance, Elbow & Silhouette.
   - **Bab IV (Sub-bab Hasil EDA):** merangkum 5 grafik & 3 tabel CSV yang sudah tersedia di `04_HASIL_ANALISIS/EDA/`.
   - **Bab V (Kesimpulan & Saran)**, Daftar Pustaka, dan Lampiran.
2. Menggabungkan draf `Bagian_Samuel_Bab3_dan_Modeling.docx` dan `Bagian_Andreas_Bab4_Interpretasi.docx` ke dalam file laporan final **`05_LAPORAN/Laporan_Penelitian_Kelompok5.docx`** serta merapikan format penulisan.
3. Menyusun slide presentasi final **`06_PRESENTASI/Presentasi_Kelompok5.pptx`** (12–14 slide, padat visual menggunakan grafik dari `04_HASIL_ANALISIS/EDA/` dan `04_HASIL_ANALISIS/visualisasi/`).

---

### Ringkasan Pembagian Beban Kerja

| Anggota | File Kode / Data | Grafik | Bagian Laporan (`05_LAPORAN/`) | Slide (`06_PRESENTASI/`) |
|---|---|---|---|---|
| **Samuel** | `03_Pemodelan_KMeans.ipynb`, `hasil_clustering.csv`, `centroid_cluster_asli.csv`, `catatan_cleaning.txt` | 2 grafik (Elbow/Silhouette & PCA) | Bab III & Bab IV (Pemodelan) | — |
| **Andreas** | `04_Analisis_dan_Visualisasi_Cluster.ipynb`, `profil_cluster.csv`, `proporsi_cluster_per_kota.csv` | 4 grafik (Profil, 38 Kota, Peta, Tren) | Bab IV (Interpretasi & Pembahasan) | — |
| **Elpita** | Rangkuman tabel `04_HASIL_ANALISIS/EDA/` | *(memakai grafik EDA & visualisasi)* | Bab I, II, IV (EDA), V, Daftar Pustaka & Gabung Final | `Presentasi_Kelompok5.pptx` (12–14 slide) |

### Alur Kerja Paralel (Tanpa Saling Tunggu di Awal)
- **Bisa langsung mulai sekarang secara bersamaan:**
  - **Samuel:** Buat `03_Pemodelan_KMeans.ipynb` (uji $K=2..8$) & mulai draf Bab III.
  - **Andreas:** Siapkan kerangka `04_Analisis_dan_Visualisasi_Cluster.ipynb` (kode template grafik & peta dari koordinat `02_DATASET/metadata_pengambilan_38_lokasi.csv`).
  - **Elpita:** Tulis Bab I, Bab II, Bab IV (bagian EDA dari folder `EDA/`), Daftar Pustaka, dan Slide 1–6 PPT.
- **Setelah Samuel `push` hasil evaluasi $K$:** Diskusi bertiga untuk menetapkan $K$ final $\rightarrow$ Samuel `push` `hasil_clustering.csv` + 2 grafik + draf Bab III/IV.
- **Setelah itu:** Andreas `pull` $\rightarrow$ jalankan Notebook 04 + 4 grafik + draf Bab IV interpretasi $\rightarrow$ `push` $\rightarrow$ Elpita `pull` $\rightarrow$ gabungkan laporan final + lengkapi Bab V & Slide 7–12 PPT $\rightarrow$ `push` final.

---

## 5. Panduan Kolaborasi Git & GitHub (Wajib Dibaca Semua Anggota)

URL Repositori:
```text
https://github.com/Ltrn11/proyek-kualitas-udara_datamining-.git
```

### A. Persiapan Awal (Cukup 1 Kali di Laptop Andreas & Elpita)
1. Pastikan **Git** sudah terpasang di laptop (cek di terminal/PowerShell dengan `git --version`).
2. Terima undangan *Collaborator* dari Samuel (cek email GitHub atau notifikasi di website GitHub).
3. Atur nama & email Git (ganti sesuai akun masing-masing):
   ```bash
   git config --global user.name "Nama Kamu"
   git config --global user.email "email_github_kamu@gmail.com"
   ```
4. Unduh (*clone*) proyek ini ke laptop di folder yang kamu inginkan:
   ```bash
   git clone https://github.com/Ltrn11/proyek-kualitas-udara_datamining-.git
   cd proyek-kualitas-udara_datamining-
   ```

---

### B. Rutinitas Harian Setiap Kali Mau Mengerjakan Tugas

#### 1. SEBELUM Mulai Mengedit: Wajib `git pull`!
Selalu ambil update terbaru dari teman terlebih dahulu supaya tidak bekerja di versi lama:
```bash
git pull origin main
```

#### 2. Kerjakan Tugas di Folder / File Bagianmu
Simpan perubahan file seperti biasa di laptop.

#### 3. SETELAH Selesai Mengedit: Cek, Commit, dan `git push`
Jalankan perintah berikut secara berurutan di terminal (di dalam folder proyek):

```bash
# 1. Lihat file apa saja yang berubah atau baru ditambahkan
git status

# 2. Masukkan semua perubahan ke daftar siap simpan
git add .

# 3. Simpan perubahan dengan pesan yang jelas (tulis nama + apa yang dikerjakan)
git commit -m "[Samuel] Tambah notebook evaluasi K-Means K=2 sampai K=8"

# 4. Pastikan tidak ada update baru dari teman sebelum mengirim
git pull --rebase origin main

# 5. Kirim perubahan ke GitHub
git push origin main
```

Contoh format pesan commit yang baik:
- `git commit -m "[Samuel] Tambah hasil clustering dan centroid asli"`
- `git commit -m "[Andreas] Tambah 6 grafik visualisasi cluster dan draf Bab III"`
- `git commit -m "[Elpita] Update Bab I-II laporan dan kerangka slide PPT"`

---

### C. Aturan Penting Agar Tidak Terjadi Konflik (*Merge Conflict*)
File `.docx`, `.pptx`, dan `.ipynb` sulit digabung otomatis oleh Git jika 2 orang mengedit **file yang sama di waktu yang bersamaan**. Ikuti 4 aturan ini:

1. **Selalu `git pull` sebelum mulai kerja**, dan langsung `git push` begitu selesai satu bagian.
2. **Jangan edit file `.docx` atau `.pptx` yang sama secara bersamaan.**
   - Saat menyusun laporan paralel, simpan dulu di file draf masing-masing (`05_LAPORAN/Bagian_Samuel_Bab3_dan_Modeling.docx`, `05_LAPORAN/Bagian_Andreas_Bab4_Interpretasi.docx`, dan `05_LAPORAN/Laporan_Penelitian_Kelompok5.docx` untuk Elpita). Setelah selesai, Elpita menggabungkannya ke file laporan utama.
   - Jika memang perlu mengedit file yang sama, cukup kabari dulu di grup WA: *"Aku lagi edit laporan utama ya, tunggu aku push dulu."*
3. **Tutup dulu Microsoft Word / PowerPoint / Excel** sebelum menjalankan `git pull` atau `git commit` supaya file sementara (`~$...`) tidak mengunci file.
4. **Jangan pernah menimpa atau menghapus file di `02_DATASET/`** (`dataset_kualitas_udara_raw.csv`, `dataset_harian.csv`, `data_terstandardisasi.csv`, dll.).

---

### D. Solusi Cepat Jika Muncul Error Saat Push/Pull
- **Error: `rejected - fetch first` saat `git push`:**  
  Artinya ada teman yang baru saja melakukan `push`. Solusinya jalankan:
  ```bash
  git pull --rebase origin main
  git push origin main
  ```
- **Error: `Your local changes would be overwritten by merge` saat `git pull`:**  
  Artinya kamu punya perubahan lokal yang belum di-commit. Simpan dulu dengan `git add .` dan `git commit -m "simpan progres sementara"`, lalu jalankan `git pull --rebase origin main`.
- **Diminta login saat `git push` pertama kali:**  
  Pilih opsi **"Sign in with your browser"** pada jendela Git Credential Manager Windows, lalu login dengan akun GitHub masing-masing.
