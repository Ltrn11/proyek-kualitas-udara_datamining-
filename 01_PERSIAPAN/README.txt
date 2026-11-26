FOLDER 01_PERSIAPAN — PROYEK DATA MINING KUALITAS UDARA
Versi awal: 18 November 2026

ISI FOLDER
1. Daftar_38_Lokasi_Ibu_Kota_Provinsi.xlsx
   - Daftar 38 titik representatif ibu kota provinsi.
   - Memuat koordinat latitude/longitude, zona waktu, sumber koordinat, catatan verifikasi.
   - Sheet: Lokasi_38, Panduan_API, Referensi.

2. Rancangan_Penelitian_Awal_Kualitas_Udara.docx
   - Judul, latar belakang, rumusan masalah, tujuan, ruang lingkup, rancangan dataset,
     metodologi K-Means, risiko, target output, dan pekerjaan awal.

3. Referensi_Awal_Proyek_Kualitas_Udara.docx
   - Daftar sumber teknis, sumber koordinat/wilayah, regulasi, dan referensi ilmiah.

CATATAN PENTING SEBELUM PENGAMBILAN DATA
- Sumber data utama yang direncanakan: Open-Meteo Air Quality API, domain CAMS Global.
- Enam variabel: PM2.5, PM10, CO, NO2, SO2, dan O3.
- Periode target awal: 90 hari; kunci tanggal aktual setelah uji API.
- Data CAMS Global melalui Open-Meteo adalah keluaran model/arsip prakiraan, bukan pengukuran langsung
  dari sensor pemantau pada tiap kota.
- Koordinat dalam spreadsheet adalah titik awal representatif, bukan batas administrasi dan bukan
  representasi seluruh wilayah kota/provinsi. Periksa returned latitude/longitude dari API karena
  beberapa titik mungkin menggunakan grid CAMS yang sama.
- Koordinat Samarinda diberi catatan verifikasi ekstra karena tabel koordinat pembanding yang digunakan
  menunjukkan dugaan duplikasi dengan Palangka Raya; spreadsheet menggunakan koordinat pembanding
  dari portal Pemerintah Kota Samarinda.
- Banjarbaru digunakan untuk ibu kota Kalimantan Selatan sesuai UU No. 8 Tahun 2022.
- Tanjung Selor digunakan untuk ibu kota Kalimantan Utara sesuai UU No. 20 Tahun 2012.
- Wamena digunakan sebagai titik representatif ibu kota Papua Pegunungan yang berkedudukan di
  Kabupaten Jayawijaya sesuai UU No. 16 Tahun 2022.
- Jangan menyebut hasil cluster sebagai kategori resmi ISPU atau kategori kesehatan resmi.

LANGKAH PERTAMA
1. Buka dan periksa spreadsheet daftar 38 lokasi.
2. Buat notebook Google Colab pada folder 03_GOOGLE_COLAB.
3. Uji pengambilan data untuk Medan, Jakarta, dan Bandung.
4. Periksa enam variabel, satuan, timestamp, rentang tanggal, serta returned coordinates/grid.
5. Jangan mengambil semua 38 lokasi sebelum uji tiga kota konsisten.

Simpan tanggal akses dan tanggal pengambilan dataset ketika proses pengumpulan data dilakukan.
