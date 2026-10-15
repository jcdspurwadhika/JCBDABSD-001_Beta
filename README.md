# Analisis Pembatalan Booking City Hotel & Resort Hotel

Final Project Purwadhika (JCBDA01), Team Beta: Salsa, Arief, Rakha.

Notebook utama: `Team Beta_Final Project_JCBDA01.ipynb`

## Latar belakang

Dua hotel di Portugal, City Hotel (Lisbon) dan Resort Hotel (Algarve), mencatat 119.390 booking untuk kedatangan Juli 2015 sampai Agustus 2017. Di data mentah, 37% booking batal. Saat ini kebijakan deposit, pembatalan, dan overbooking berlaku sama untuk semua booking (98,7% booking tanpa deposit), padahal risiko batalnya berbeda jauh antarsegmen, channel, agent, dan waktu.

Pengguna utama analisis ini adalah Revenue Manager kedua hotel. Yang perlu diajak koordinasi: Reservation Manager, Sales & Distribution, Marketing, dan IT/Data.

Problem statement: segmen booking dan periode kedatangan mana yang perlu kebijakan deposit, pembatalan, dan overbooking yang berbeda, agar revenue yang hilang karena pembatalan bisa ditekan?

Ukuran utama di notebook adalah revenue loss rate, yaitu revenue hilang dibagi potensi revenue kelompok itu. Cancel rate (dari jumlah booking) ditampilkan sebagai pembanding.

## Dataset

- *Hotel Booking Demand* dari Antonio, Almeida & Nunes (2019), *Data in Brief* 22.
- 119.390 baris dan 32 kolom. Setelah duplikat dihapus tersisa 87.228 booking.
- File yang dibaca notebook: `Dataset/hotel_bookings.csv`.

## Isi notebook

| Bagian | Isi |
|---|---|
| Background Story | Konteks, stakeholder, problem statement, pertanyaan A-K, dan Ringkasan eksekutif |
| 1. Import Library & Data | Library, data mentah, warna grafik |
| 2. Data Understanding | Profil data, kamus data, nilai unik, kondisi awal target |
| 3. Data Cleaning | Duplikat, missing value, tipe data, inkonsistensi, outlier, feature engineering, verifikasi |
| 4. EDA | 4.A sampai 4.N, dibagi ke dalam enam babak (dijelaskan di bawah tabel pertanyaan) |
| 5. Insight & Recommendation | Jawaban problem statement, 12 insight, 7 rekomendasi, tabel pemilik, prioritas, dan KPI |
| Export Data | File `booking_bersih.csv` untuk dashboard Tableau atau Looker Studio |
| Batasan Analisis dan Referensi | Batasan data dan daftar sumber |

Pertanyaan analisis dan bagian yang menjawabnya:

| Kode | Pertanyaan | Bagian |
|---|---|---|
| A | Seberapa besar kerugian akibat pembatalan, kapan terjadinya, dan apakah makin parah? | 4.B |
| B | Booking seperti apa yang batal, setelah variabel perancu dikontrol? | 4.C |
| C | Kolom mana yang tidak valid untuk menjelaskan pembatalan? | 4.D |
| D | Kapan volume dan pembatalan naik atau turun, dan berapa kamar yang hilang menjelang kedatangan? | 4.E |
| E | Di agent dan segmen mana kerugian menumpuk? | 4.F |
| F | Segmen mana yang paling bernilai setelah dikurangi pembatalan? | 4.G |
| G | Apa yang perlu dicek sebelum kebijakan disusun? | 4.I |
| H | Kapan dan dari siapa pemasukan datang? | 4.J |
| I | Kelompok tamu mana yang besar, bernilai, dan tumbuh, dan kapan hotel paling ramai? | 4.L |
| J | Agent dan perusahaan mana yang paling bernilai dan menanggung revenue hilang? | 4.M |
| K | Apakah selisih yang dipakai di rekomendasi bisa terjadi karena kebetulan? | 4.N |

4.H (korelasi) dan 4.K (ringkasan sementara) tidak punya pertanyaan sendiri. Analisis dibaca dalam enam babak: konteks dan data (1-3, 4.A), masalah dan penyebab (4.B-4.D), waktu (4.E), sasaran dan nilai (4.F-4.I), pemasukan dan kelompok (4.J-4.M), lalu bukti dan keputusan (4.N, bagian 5, Batasan).

## Temuan utama

| Temuan | City Hotel | Resort Hotel |
|---|---|---|
| Revenue hilang karena batal (26 bulan) | €6,42 juta (34,2% potensi revenue) | €4,82 juta (30,7%) |
| Porsi revenue hilang dari segmen risiko tinggi (Online TA, lead time > 90 hari) | 40,0% | 48,0% |
| Cancel rate segmen risiko vs booking lain | 44,7% vs 25,7% | 49,8% vs 18,0% |
| Agent dengan revenue hilang terbesar | Agent 9: €5,13 juta | Agent 240: €3,25 juta |
| Kamar batal yang jatuh di malam yang masih kosong | €2,81 juta | €1,31 juta |
| Malam penuh (dasar buffer overbooking) | 33 malam, terutama Juli-Agustus | 121 malam, Mei-September |

Angka lengkap, termasuk confidence interval dan RevPAR, ada di Ringkasan eksekutif di bawah Background Story.

## Rekomendasi

1. Deposit berbasis risiko untuk segmen risiko tinggi dan booking lebih dari 7 malam.
2. Pantau *pickup* harian dan jual ulang kamar yang dilepas.
3. Syarat pembatalan khusus untuk agent 9 dan 240, bersama Sales & Distribution.
4. Buffer overbooking hanya untuk malam yang diperkirakan penuh.
5. Early-bird non-refundable dan tarif khusus libur.
6. Alasan bagi tamu untuk booking langsung, bersama Marketing.
7. Di musim sepi, jual upgrade dan isi kamar kosong.

Kalau 10% kamar yang dilepas berhasil dijual ulang, nilainya sekitar €190 ribu per tahun (rekomendasi 2). Pemilik, prioritas, KPI, dan urutan pelaksanaan tiap rekomendasi ada di bagian 5.

## Cara menjalankan

1. Susun folder seperti ini. Notebook membaca data dari `../Dataset/hotel_bookings.csv`, jadi folder `output` (tempat notebook) dan folder `Dataset` harus berdampingan.

   ```
   Finpro Purwa/
   ├── Dataset/
   │   └── hotel_bookings.csv
   └── output/
       ├── Team Beta_Final Project_JCBDA01.ipynb
       └── README.md
   ```

   Kalau letaknya berbeda, ubah path di sel `pd.read_csv` pada bagian 1 dan variabel `folder` di sel Export.

2. Pasang library yang dipakai. Notebook diuji dengan Python 3.13.9, pandas 2.3.3, numpy 2.3.5, matplotlib 3.10.6, seaborn 0.13.2, dan scipy 1.16.3.

   ```bash
   pip install pandas numpy matplotlib seaborn scipy
   ```

3. Buka notebook di Jupyter, lalu jalankan semua sel dari atas (Run All). Prosesnya sekitar satu menit.

Dengan pandas 3, urutan tanggal yang jumlahnya seri di tabel 4.L.7 bisa sedikit berbeda, tetapi angkanya tetap sama.

## File yang dihasilkan

Sel Export menyimpan `booking_bersih.csv` ke folder `output`, di samping notebook. Isinya satu baris per booking yang sudah bersih (87.228 baris, 39 kolom), termasuk kolom revenue, revenue_masuk_booking, revenue_hilang, dan forfeited_deposit_revenue. Total potensi revenue di file ini €34.446.255.

Untuk dashboard, revenue loss rate dihitung dari jumlah revenue_hilang dibagi jumlah revenue di kelompok yang sama.

## Batasan utama

- Revenue adalah estimasi (adr × total_nights). Dataset tidak memuat pembayaran aktual, komisi agent, atau profit.
- Tidak ada ID booking atau ID tamu, jadi duplikat tidak bisa dipastikan. Menghapus duplikat menurunkan cancel rate City dari 41,8% ke 30,1%.
- Tidak ada data kapasitas kamar. Okupansi dan RevPAR memakai proksi kamar terisi di malam paling penuh (City 216, Resort 184), jadi angkanya batas atas.
- Semua temuan adalah asosiasi. Efek kebijakan baru bisa dibuktikan lewat uji coba.
- Data berhenti di Agustus 2017.

Daftar lengkapnya ada di bagian Batasan Analisis di notebook.
