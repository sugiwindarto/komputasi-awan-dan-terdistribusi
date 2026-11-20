# Jurnal Proses — Tugas 9

## Diskusi deployment model

- **Pendapat Jingga:** Jingga mempertimbangkan keamanan dan kerahasiaan data kependudukan sebagai hal utama. Data seperti NIK, alamat, dan biometrik sebaiknya tetap berada di Private Cloud yang dikelola pemerintah agar kendali terhadap data lebih terjamin. Namun, penggunaan Private Cloud saja juga memiliki keterbatasan kapasitas ketika terjadi lonjakan akses.

- **Pendapat Sugi:** Sugi mempertimbangkan kebutuhan akses dari ribuan instansi dengan jumlah permintaan yang tidak selalu stabil. Menurut pertimbangannya, Public Cloud dapat dimanfaatkan untuk komponen yang membutuhkan fleksibilitas dan penambahan kapasitas secara cepat. Oleh karena itu, penggabungan Public Cloud dan Private Cloud melalui Hybrid Cloud dinilai lebih sesuai.

- **Pendapat Salman:** Salman mempertimbangkan kedua aspek tersebut, yaitu keamanan data dan skalabilitas sistem. Menurutnya, Public Cloud sepenuhnya kurang sesuai karena pemerintah tidak memiliki kendali penuh terhadap infrastruktur penyimpanan data sensitif. Sementara itu, Private Cloud sepenuhnya berpotensi mengalami keterbatasan kapasitas ketika terjadi lonjakan permintaan. Salman menyetujui Hybrid Cloud karena data utama tetap berada di Private Cloud, sedangkan layanan yang membutuhkan fleksibilitas dapat memanfaatkan Public Cloud.

- **Kesimpulan diskusi:** Kelompok menyepakati Hybrid Cloud sebagai deployment model karena dinilai paling sesuai untuk menyeimbangkan kebutuhan keamanan, kendali data, dan skalabilitas Sistem Data Kependudukan.


## Diskusi service model per komponen

- **Pendapat Jingga:** Jingga mempertimbangkan penggunaan IaaS pada komponen inti yang mengelola data sensitif, yaitu basis data induk, gudang biometrik, dan mesin pencocokan biometrik. Dengan IaaS, pemerintah memiliki kendali lebih besar terhadap sistem operasi, penyimpanan, runtime, dan konfigurasi keamanan di Private Cloud.

- **Pendapat Sugi:** Sugi mempertimbangkan penggunaan PaaS untuk Gerbang API, antrean permintaan, dan portal publik pengecekan status. Komponen tersebut membutuhkan fleksibilitas dalam pengelolaan aplikasi serta kemampuan menyesuaikan kapasitas ketika permintaan meningkat. Penggunaan PaaS juga mengurangi kebutuhan pengelolaan infrastruktur secara langsung.

- **Pendapat Salman:** Salman menyetujui penggunaan SaaS untuk layanan pendukung, yaitu Helpdesk Mitra dan Pemantauan Operasional. Menurutnya, layanan tersebut tidak perlu dibangun dan dikelola seluruh infrastrukturnya sendiri. Namun, penggunaannya tetap harus dibatasi agar data kependudukan yang sensitif tidak tersimpan atau terekspos pada layanan tersebut.

- **Kesimpulan diskusi:** Kelompok menyepakati penggunaan kombinasi IaaS, PaaS, dan SaaS sesuai kebutuhan setiap komponen. IaaS digunakan untuk komponen inti di Private Cloud, PaaS untuk layanan aplikasi di Public Cloud, dan SaaS untuk layanan pendukung operasional.

## Log Penggunaan AI (Level 2)

> Wajib diisi sesuai kebijakan Level 2 di [`../RUBRIK-UMUM.md`](../RUBRIK-UMUM.md). Tulis "Tidak memakai AI" pada baris pertama jika memang tidak dipakai. Hanya untuk brainstorming ide/outline — bukan untuk kode/analisis/teks akhir.


| Tanggal | Tool AI | Prompt yang diberikan | Ringkasan saran/ide AI | Bagaimana diolah jadi tulisan/kode sendiri |
| :--- | :--- | :--- | :--- | :--- |
| 20 November 2026 | Gemini | Memverifikasi jawaban latihan soal dasar *cloud computing*, lalu menanyakan alasan pemilihan *Hybrid Cloud* dibanding *Community Cloud* dan kaitannya dengan karakteristik NIST. | Mengoreksi beberapa konsep dasar (contoh SaaS, pelopor Salesforce 1999, definisi NIST), menjelaskan perbedaan kepemilikan tunggal vs. konsorsium pada *Community Cloud*, serta memetakan 5 karakteristik NIST terhadap *trade-off* kerahasiaan dan skalabilitas. | Menggunakan pemahaman mengenai *unique entities*, isolasi data, dan *rapid elasticity* sebagai kerangka berpikir awal untuk menyusun argumen analisis studi kasus secara mandiri. |
| 20 November 2026 | Gemini | Meminta analisis kelayakan *Hybrid Cloud* pada studi kasus Sistem Data Kependudukan (NIK, biometrik, UU PDP, lonjakan akses pemilu/PPDB), perbandingan dengan model lain, serta evaluasi terhadap draf jawaban pribadi. | Menjelaskan pemisahan *data plane* (*Private Cloud* negara dengan *zero data at rest* di publik) dan *compute plane* (*Public Cloud* untuk *cloud bursting*), membandingkan 4 *deployment model*, serta mengoreksi istilah teknis dan ejaan pada draf. | Menulis sendiri draf perbandingan antar-model dan tabel pemetaan 5 karakteristik NIST berdasarkan kasus, lalu menyuntingnya kembali dengan memasukkan istilah standar (*NIST SP 800-145*, *stateless*, *cloud bursting*) dan konteks UU PDP. |
| 20 November 2026 | Gemini | Meminta rekomendasi *Service Model* (IaaS, PaaS, SaaS) untuk setiap komponen Sistem Data Kependudukan (database inti vs. portal publik) | Merekomendasikan pendekatan multi-model: **IaaS** (*Private Cloud*) untuk basis data induk dan pencocokan biometrik, **PaaS** (*Public Cloud*) untuk gerbang API *stateless* dan portal publik, serta **SaaS** untuk sistem *helpdesk* operasional. | Menyelaraskan pemilihan *service model* tiap komponen dengan tingkat sensitivitas data dan tanggung jawab keamanan negara |
| 20 November 2026 | ChatGPT | Meminta masukan mengenai penyusunan diagram arsitektur Hybrid Cloud berdasarkan komponen yang sudah ditentukan kelompok, serta saran untuk menyusun poin-poin diskusi pada JURNAL.md. | Memberikan beberapa saran mengenai pembagian komponen Public Cloud dan Private Cloud, alur proses verifikasi data, serta pemilihan IaaS, PaaS, dan SaaS sesuai fungsi masing-masing komponen. | Menjadikan saran AI sebagai bahan pertimbangan untuk memahami susunan arsitektur dan alur sistem, kemudian menyesuaikannya dengan rancangan serta keputusan yang telah disepakati kelompok. |
