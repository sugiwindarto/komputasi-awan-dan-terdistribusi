# Jurnal Proses — Tugas 9

## Diskusi deployment model
- Opsi yang dipertimbangkan dan kenapa akhirnya memilih salah satu: ...

## Diskusi service model per komponen
- ...

## Log Penggunaan AI (Level 2)

> Wajib diisi sesuai kebijakan Level 2 di [`../RUBRIK-UMUM.md`](../RUBRIK-UMUM.md). Tulis "Tidak memakai AI" pada baris pertama jika memang tidak dipakai. Hanya untuk brainstorming ide/outline — bukan untuk kode/analisis/teks akhir.


| Tanggal | Tool AI | Prompt yang diberikan | Ringkasan saran/ide AI | Bagaimana diolah jadi tulisan/kode sendiri |
| :--- | :--- | :--- | :--- | :--- |
| 20 November 2026 | Gemini | Memverifikasi jawaban latihan soal dasar *cloud computing*, lalu menanyakan alasan pemilihan *Hybrid Cloud* dibanding *Community Cloud* dan kaitannya dengan karakteristik NIST. | Mengoreksi beberapa konsep dasar (contoh SaaS, pelopor Salesforce 1999, definisi NIST), menjelaskan perbedaan kepemilikan tunggal vs. konsorsium pada *Community Cloud*, serta memetakan 5 karakteristik NIST terhadap *trade-off* kerahasiaan dan skalabilitas. | Menggunakan pemahaman mengenai *unique entities*, isolasi data, dan *rapid elasticity* sebagai kerangka berpikir awal untuk menyusun argumen analisis studi kasus secara mandiri. |
| 20 November 2026 | Gemini | Meminta analisis kelayakan *Hybrid Cloud* pada studi kasus Sistem Data Kependudukan (NIK, biometrik, UU PDP, lonjakan akses pemilu/PPDB), perbandingan dengan model lain, serta evaluasi terhadap draf jawaban pribadi. | Menjelaskan pemisahan *data plane* (*Private Cloud* negara dengan *zero data at rest* di publik) dan *compute plane* (*Public Cloud* untuk *cloud bursting*), membandingkan 4 *deployment model*, serta mengoreksi istilah teknis dan ejaan pada draf. | Menulis sendiri draf perbandingan antar-model dan tabel pemetaan 5 karakteristik NIST berdasarkan kasus, lalu menyuntingnya kembali dengan memasukkan istilah standar (*NIST SP 800-145*, *stateless*, *cloud bursting*) dan konteks UU PDP. |
| 20 November 2026 | Gemini | Meminta rekomendasi *Service Model* (IaaS, PaaS, SaaS) untuk setiap komponen Sistem Data Kependudukan (database inti vs. portal publik) | Merekomendasikan pendekatan multi-model: **IaaS** (*Private Cloud*) untuk basis data induk dan pencocokan biometrik, **PaaS** (*Public Cloud*) untuk gerbang API *stateless* dan portal publik, serta **SaaS** untuk sistem *helpdesk* operasional. | Menyelaraskan pemilihan *service model* tiap komponen dengan tingkat sensitivitas data dan tanggung jawab keamanan negara |
