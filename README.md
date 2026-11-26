# Proyek Data Mining — Kelompok 5
## Pengelompokan Pola Konsentrasi Polutan Udara pada 38 Ibu Kota Provinsi di Indonesia Menggunakan Algoritma K-Means

**Mata Kuliah:** Data Mining (Semester 5)  
**Anggota Kelompok 5:**
1. **Samuel** — Koordinator Repositori & Pemodelan K-Means
2. **Andreas** — Visualisasi, Analisis Wilayah/Waktu & Metodologi Teknis
3. **Elpita** — Dokumentasi, Penyusunan Laporan Akhir & Slide Presentasi

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

## 4. Pembagian Tugas Kelompok (3 Anggota)

Pembagian dirancang agar **setiap anggota punya area folder/file kerja masing-masing** sehingga tidak sering bentrok (*merge conflict*) saat bekerja bersamaan di Git, namun **ketiganya wajib memahami alur keseluruhan** saat presentasi.

### 1) Samuel — Koordinator Repositori & Lead Pemodelan K-Means
**Area Kerja Utama:** `03_GOOGLE_COLAB/`, `02_DATASET/`, dan manajemen repo GitHub.

**Daftar Tugas:**
1. Mengelola repositori GitHub, mengundang Andreas & Elpita sebagai *Collaborator*, dan memastikan sinkronisasi repo rapi.
2. Menyimpan dokumen `catatan_cleaning.txt` sebagai pelengkap audit pembersihan data.
3. Membuat dan menjalankan notebook **`03_Pemodelan_dan_Evaluasi_KMeans.ipynb`** di Google Colab menggunakan `data_terstandardisasi.csv` (6 fitur, tanpa `id_pengamatan`).
4. Menjalankan evaluasi kandidat jumlah cluster **$K = 2$ sampai $K = 8$** menggunakan **Elbow Method (SSE/Inertia)**, **Silhouette Score**, **Davies-Bouldin Index (DBI)**, serta mengecek jumlah anggota tiap cluster.
5. Memimpin diskusi singkat kelompok untuk menetapkan nilai $K$ final yang paling tepat secara metrik dan substantif.
6. Melatih model K-Means final dan menghasilkan file output:
   - `04_HASIL_ANALISIS/tabel_evaluasi_k.csv`
   - `04_HASIL_ANALISIS/hasil_clustering.csv` (gabungan data harian + label cluster)
   - `04_HASIL_ANALISIS/centroid_cluster_asli.csv` (hasil `inverse_transform` ke µg/m³)
   - `04_HASIL_ANALISIS/model_kmeans_final.pkl`
7. Membantu mengecek konsistensi angka antara notebook dan Bab IV laporan.

---

### 2) Andreas — Visualisasi Hasil, Analisis Wilayah/Waktu & Metodologi Teknis
**Area Kerja Utama:** `04_HASIL_ANALISIS/visualisasi/` dan draf teknis `05_LAPORAN/` (Bab III & bagian analisis Bab IV).

**Daftar Tugas:**
1. Menganalisis file `hasil_clustering.csv` dan `centroid_cluster_asli.csv` (setelah selesai dari Samuel) untuk menyusun tabel:
   - Profil karakteristik 6 polutan pada tiap cluster (rata-rata, median, jumlah & persentase hari).
   - Proporsi cluster per kota untuk ke-38 ibu kota (`04_HASIL_ANALISIS/proporsi_cluster_per_kota.csv`).
   - Pola perubahan cluster berdasarkan waktu (Agustus–November 2026) dan sorotan kasus ekstrem (seperti Palangka Raya vs Jakarta/Bandung vs Indonesia Timur).
2. Membuat seluruh grafik final beresolusi tinggi (300 DPI) di folder `04_HASIL_ANALISIS/visualisasi/`:
   - `01_evaluasi_elbow_silhouette.png`
   - `02_profil_centroid_cluster.png`
   - `03_sebaran_cluster_pca.png` *(PCA hanya untuk visualisasi 2D)*
   - `04_proporsi_cluster_38_kota.png`
   - `05_peta_cluster_dominan_indonesia.png`
   - `06_tren_cluster_bulanan.png`
3. Menyusun draf **Bab III (Metodologi Penelitian)** (alur kerja, daftar lokasi, proses API, audit/cleaning, agregasi harian, standardisasi, rancangan evaluasi K-Means) serta bagian **pembahasan numerik & interpretasi grafik di Bab IV**.
4. Memastikan seluruh grafik memiliki judul, label sumbu, satuan (µg/m³), dan legenda yang jelas untuk dipakai Elpita di laporan & PPT.

---

### 3) Elpita — Dokumentasi, Integrasi Laporan Akhir & Slide Presentasi
**Area Kerja Utama:** `05_LAPORAN/` dan `06_PRESENTASI/`.

**Daftar Tugas:**
1. Menyusun draf **Bab I (Pendahuluan)** (latar belakang, rumusan masalah, tujuan, manfaat, batasan CAMS Global & non-ISPU) dan **Bab II (Landasan Teori)** (6 polutan udara, CAMS/Open-Meteo, CRISP-DM, StandardScaler, K-Means, Euclidean Distance, Elbow & Silhouette) dengan mengacu pada dokumen di `01_PERSIAPAN/` dan `MATERI/`.
2. Menggabungkan bagian Bab III & IV dari Andreas menjadi satu laporan utuh **`05_LAPORAN/Laporan_Penelitian_Kelompok5.docx`**, termasuk penulisan **Bab V (Kesimpulan & Saran)**, Daftar Pustaka, dan Lampiran.
3. Memeriksa konsistensi istilah di seluruh laporan: tidak menyebut data CAMS sebagai sensor BMKG/KLHK dan tidak menyebut cluster sebagai kategori resmi ISPU.
4. Merancang slide presentasi **`06_PRESENTASI/Presentasi_Kelompok5.pptx`** (12–14 slide, padat visual menggunakan grafik dari folder `EDA/` dan `visualisasi/`).
5. Menyusun daftar **tanya-jawab (Q&A)** singkat untuk latihan presentasi bersama Samuel dan Andreas.

---

### Alur Ketergantungan Kerja (Bisa Jalan Paralel!)
- **Hari ini (bisa langsung jalan bersamaan tanpa saling tunggu):**
  - **Samuel:** Mulai kerjakan `03_Pemodelan_dan_Evaluasi_KMeans.ipynb` (evaluasi $K=2..8$).
  - **Andreas:** Mulai tulis draf Bab III (Metodologi) berdasarkan pekerjaan pengumpulan data, cleaning, agregasi, dan EDA yang sudah ada.
  - **Elpita:** Mulai tulis draf Bab I (Pendahuluan), Bab II (Landasan Teori), Daftar Pustaka, dan kerangka awal PPT (Slide 1–6).
- **Setelah Samuel selesai evaluasi $K$:** Diskusi bertiga (15 menit) untuk sepakat memilih $K$ final $\rightarrow$ Samuel simpan `hasil_clustering.csv` dan `push` ke GitHub.
- **Setelah itu:** Andreas `pull` lalu buat grafik & analisis Bab IV $\rightarrow$ `push` ke GitHub $\rightarrow$ Elpita `pull` lalu integrasikan Bab IV–V, finalisasi DOCX dan PPT (Slide 7–12) $\rightarrow$ `push` final.

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
   - Saat menyusun laporan paralel, Andreas bisa menyimpan drafnya dulu di `05_LAPORAN/Draf_Bab3_Bab4_Andreas.docx` dan Elpita di `05_LAPORAN/Laporan_Penelitian_Kelompok5.docx`. Setelah selesai, Elpita menggabungkannya ke file laporan utama.
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
