# Tugas 9 (Pekan 9) — Cloud Delivery & Deployment Model

**Materi terkait:** Karakteristik esensial cloud computing (NIST SP 800-145), Deployment Model (Public/Private/Hybrid/Community), Service Model (IaaS/PaaS/SaaS).

> Bagian 2 mata kuliah ini murni **analisis & desain di atas kertas** — tidak ada biaya cloud sama sekali, karena tugas berhenti di tahap rekomendasi, bukan implementasi nyata (implementasi baru mulai Tugas 11).

## Studi Kasus

Pemerintah ingin membangun **Sistem Data Kependudukan** (nama, NIK, alamat, data biometrik) yang:
- Sangat **rahasia** (data pribadi warga negara, diatur UU Perlindungan Data Pribadi).
- Diakses oleh **ribuan instansi** (dukcapil daerah, kepolisian, bank untuk verifikasi KYC) dengan pola akses yang **tidak rata** (lonjakan saat musim pemilu/pendaftaran sekolah).
- Butuh **skalabilitas** agar tidak downtime saat lonjakan akses, tapi **tidak boleh** datanya berada di infrastruktur yang tidak terkendali penuh oleh negara.

## Tugas Kelompok

1. **Rekomendasikan Deployment Model** (Public / Private / Hybrid / Community Cloud) yang paling tepat untuk sistem ini. Jelaskan alasannya berdasarkan **karakteristik NIST** (on-demand self-service, broad network access, resource pooling, rapid elasticity, measured service) dikaitkan dengan kebutuhan kerahasiaan vs skalabilitas.
2. **Rekomendasikan Service Model** (IaaS / PaaS / SaaS) yang tepat untuk komponen-komponen berbeda dari sistem ini (boleh berbeda service model untuk komponen berbeda, mis. database inti vs portal publik untuk pengecekan status).
3. Jelaskan **minimal 2 risiko spesifik** dari pilihan kalian dan mitigasinya (mis. jika Hybrid Cloud dipilih: risiko sinkronisasi data antara private & public segment).
4. Bandingkan singkat dengan **satu alternatif yang kalian tolak** — kenapa alternatif itu kurang tepat (mis. kenapa Public Cloud murni ditolak, atau kenapa Private Cloud murni dianggap kurang skalabel).

## Jawaban

### Soal No. 1

Menurut standar **NIST**, pilihan **Hybrid Cloud** sangat tepat untuk Sistem Data Kependudukan karena menggabungkan infrastruktur *Private* dan *Public Cloud* yang tetap berdiri sebagai entitas terpisah (*unique entities*), namun saling terikat melalui teknologi terstandar (seperti terowongan terenkripsi dan *cloud bursting*). Pendekatan ini memungkinkan negara menyimpan seluruh basis data induk (nama, NIK, alamat, dan data biometrik) secara eksklusif di **Private Cloud** milik negara sesuai mandat **UU Perlindungan Data Pribadi (UU PDP)**, sementara lapisan gerbang API yang bersifat *stateless* (tidak menyimpan data permanen) dijalankan di **Public Cloud** untuk menyerap lonjakan akses ribuan instansi.

### Perbandingan dengan Deployment Model Lain

* Public Cloud, melanggar syarat kedaulatan infrastruktur negara karena data pribadi dan biometrik warga disimpan di lingkungan *multi-tenant* pihak ketiga yang berada di luar kendali penuh pemerintah.
* Private Cloud, kapasitas perangkat keras terbatas pada infrastruktur fisik internal sehingga berisiko mengalami *downtime* saat terjadi lonjakan akses ekstrem (musim pemilu atau pendaftaran sekolah), atau memboroskan anggaran (*over-provisioning*) saat trafik normal.
* Community Cloud, kapasitas infrastruktur terbatas pada cakupan komunitas sehingga tidak seelastis *Public Cloud*, serta kurang fleksibel ketika sistem harus melayani mitra komersial eksternal seperti perbankan untuk verifikasi KYC.

### Analisis Bedasarkan  NIST

| Karakteristik NIST | Analisis Kebutuhan Studi Kasus |
| :--- | :--- |
| **On-Demand Self-Service** | Memungkinkan pemerintah menyediakan instans *API Gateway* dan *worker* verifikasi tambahan di sisi *Public Cloud* secara otomatis menjelang musim pemilu atau pendaftaran sekolah, tanpa bergantung pada pengadaan perangkat keras fisik yang memakan waktu lama. |
| **Broad Network Access** | Menyediakan konektivitas *endpoint* API yang merata dan terstandar bagi ribuan instansi dengan infrastruktur beragam (Dukcapil daerah, kepolisian, perbankan) tanpa membuka akses langsung ke jaringan inti pusat data negara. |
| **Resource Pooling** | Mencegah risiko kebocoran data lintas penyewa (*multi-tenancy*) dengan memisahkan kolam sumber daya: *Private Cloud* digunakan secara eksklusif (*dedicated*) untuk penyimpanan data sensitif, sedangkan *Public Cloud* hanya untuk pemrosesan trafik *stateless*. |
| **Rapid Elasticity** | Mendukung mekanisme *cloud bursting* untuk menambah kapasitas pemrosesan secara otomatis saat terjadi lonjakan akses tidak merata (pemilu/PPDB) agar terhindar dari *downtime*, lalu menyusutkannya kembali saat trafik normal demi efisiensi anggaran. |
| **Measured Service** | Mencatat setiap riwayat pengaksesan data oleh masing-masing instansi secara *real-time* untuk mengukur kuota penggunaan API sekaligus menyediakan jejak audit keamanan (*audit trail*) sesuai ketentuan UU PDP. |

### Soal No.2

Berikut adalah rekomendasi service model untuk implementasi studi kasus
| Komponen Sistem | Zona Deployment | Service Model | Tingkat Kendali Negara | Alasan Utama (Kerahasiaan vs. Skalabilitas) |
| :--- | :--- | :--- | :--- | :--- |
| **Basis Data Induk & Gudang Biometrik** | *Private Cloud* | **IaaS** | **Penuh** (OS, Storage, Enkripsi HSM, DB Engine) | Memenuhi mandat UU PDP dan kedaulatan penuh atas data pribadi & biometrik warga. |
| **Mesin Pencocokan Biometrik & Verifikasi Inti** | *Private Cloud* | **IaaS** | **Penuh** (Runtime, Akselerasi Hardware, Jaringan) | Memastikan pemrosesan data sensitif terjadi di zona terisolasi dengan latensi rendah. |
| **Gerbang API Elastis & Antrean (*Stateless*)** | *Public Cloud* | **PaaS** | **Menengah** (Konfigurasi API & Kode Routing) | Mendukung *cloud bursting* otomatis untuk jutaan *request* instansi tanpa beban kelola OS. |
| **Portal Publik Pengecekan Status** | *Public Cloud* | **PaaS** | **Menengah** (Kode Aplikasi Web & Antarmuka) | Menahan lonjakan akses masyarakat saat pemilu/PPDB dengan *auto-scaling* dan CDN. |
| **Helpdesk Mitra & Pemantauan Operasional** | *Public Cloud* | **SaaS** | **Terbatas** (Konfigurasi Pengguna & Hak Akses) | Efisiensi biaya dan kecepatan operasional untuk layanan non-sensitif lintas instansi. |

### Soal No. 3

Pilihan *Hybrid Cloud* membawa risiko yang spesifik pada konteks data kependudukan. Berikut empat risiko utama beserta mitigasinya.

| No | Risiko Spesifik | Dampak pada Sistem Kependudukan | Mitigasi |
| :--- | :--- | :--- | :--- |
| 1 | **Inkonsistensi & sinkronisasi data antara segmen *Private* dan *Public*** | *API Gateway* di *Public Cloud* dapat menampilkan status data yang usang (misalnya status KTP-el atau domisili yang baru diperbarui) sehingga hasil verifikasi KYC bank atau pengecekan kepolisian keliru. | Gateway di *Public Cloud* dibuat **benar-benar *stateless***: tidak menyimpan salinan data penduduk, hanya meneruskan (*proxy*) permintaan ke *Private Cloud* sebagai *single source of truth*. Cache jangka pendek hanya untuk hasil non-sensitif dengan **TTL singkat** dan invalidasi otomatis. |
| 2 | **Kebocoran data pada jalur penghubung (*data in transit*) dan pada gerbang publik** | Tautan *Private*–*Public* dan *endpoint* API yang terbuka ke ribuan instansi menjadi target serangan (*man-in-the-middle*, *API abuse*, DDoS). Satu kebocoran NIK dan biometrik bersifat permanen, karena biometrik tidak bisa "diganti" seperti kata sandi. | Terowongan terenkripsi (*IPsec VPN*/jalur privat khusus) dengan ***mutual TLS*** antara kedua segmen. Biometrik tidak pernah melewati *Public Cloud*: yang dikirim hanya hasil pencocokan (*match/no-match* atau skor). Dipasang *WAF*, *rate limiting*, dan otentikasi OAuth2 per instansi. Kunci enkripsi disimpan di ***HSM* milik negara**. |
| 3 | **Penyalahgunaan akses oleh instansi mitra (*over-access*/*purpose limitation*)** | Bank atau instansi lain dapat menarik data lebih dari yang diperlukan untuk KYC, atau menggunakannya di luar tujuan. Ini melanggar prinsip minimisasi data dalam UU PDP. | Kontrol akses berbasis peran (*RBAC*/*ABAC*) per instansi dengan ***scope* minimum** (misalnya bank hanya mendapat respons "valid/tidak valid" dan atribut terbatas). Setiap akses dicatat lewat *Measured Service* sebagai ***audit trail* yang tidak dapat diubah**, ditambah deteksi anomali dan peninjauan berkala. |
| 4 | **Ketergantungan pada satu penyedia *Public Cloud* (*vendor lock-in*) dan titik gagal tunggal pada tautan *Private*–*Public*** | Jika tautan putus atau penyedia mengalami gangguan saat musim pemilu, layanan ke seluruh instansi terhenti, padahal data induk di *Private Cloud* sebenarnya aman. | Tautan **redundan** (minimal dua jalur dari operator berbeda). Gateway dibuat portabel dengan **kontainer/Kubernetes** dan standar terbuka agar dapat dipindah antar penyedia (*multi-cloud*). Disiapkan ***failover* ke kapasitas cadangan di *Private Cloud*** dengan mode layanan terbatas (*graceful degradation*). |


### Soal No. 4

**Alternatif yang ditolak: *Public Cloud* murni.**

Model ini sekilas paling unggul dari sisi skalabilitas dan biaya, tetapi kurang tepat untuk Sistem Data Kependudukan karena alasan berikut.

| Aspek | *Public Cloud* Murni | *Hybrid Cloud* (Pilihan Kami) |
| :--- | :--- | :--- |
| **Kedaulatan & kendali data** | Data NIK dan biometrik berada di infrastruktur milik pihak ketiga yang tidak dikendalikan penuh oleh negara. Ini bertentangan dengan syarat studi kasus dan berisiko melanggar UU PDP serta ketentuan penyelenggaraan sistem elektronik untuk data strategis pemerintah. | Data induk tetap di *Private Cloud* milik negara. |
| ***Resource Pooling*** | Sumber daya dipakai bersama banyak penyewa (*multi-tenant*). Walau terisolasi secara logis, risiko kebocoran lintas penyewa dan akses oleh penyedia (termasuk yurisdiksi hukum negara lain) tidak dapat dihilangkan. | Kolam sumber daya sensitif bersifat *dedicated*, dan *Public Cloud* hanya memproses lalu lintas *stateless*. |
| **Pengelolaan kunci & audit** | Kendali atas lapisan fisik, hipervisor, dan jejak audit terbatas pada apa yang disediakan penyedia. | Kunci di *HSM* milik negara dan *audit trail* di bawah kendali penuh pemerintah. |
| ***Rapid Elasticity*** | Sangat baik. | Tetap terpenuhi lewat *cloud bursting* di segmen publik. |

Keunggulan *Public Cloud* murni hanya pada elastisitas dan efisiensi biaya, dan keduanya sudah dapat dipenuhi *Hybrid Cloud* tanpa mengorbankan kedaulatan data. Karena kerahasiaan dan kendali penuh negara adalah **syarat mutlak** (bukan sekadar preferensi), *Public Cloud* murni gugur sejak awal.

*Private Cloud* murni juga dipertimbangkan, tetapi ditolak karena kapasitasnya terbatas pada perangkat keras fisik. Sistem berisiko *downtime* saat lonjakan pemilu/PPDB, atau memboroskan anggaran (*over-provisioning*) bila kapasitas disiapkan untuk beban puncak sepanjang tahun.


## Kesimpulan

*Hybrid Cloud* memberikan keseimbangan terbaik antara **kerahasiaan** dan **skalabilitas**: data induk dan biometrik tetap berada di bawah kendali penuh negara di *Private Cloud* (IaaS), sedangkan lapisan *stateless* (API Gateway dan portal publik) memanfaatkan elastisitas *Public Cloud* (PaaS/SaaS). Risiko utama (sinkronisasi data, kebocoran jalur penghubung, penyalahgunaan akses mitra, dan ketergantungan penyedia) dapat dimitigasi dengan desain *stateless*, enkripsi *mutual TLS* + HSM, kontrol akses *scope* minimum, serta tautan redundan.


## Struktur Submission

```
tugas-09-cloud-delivery-deployment/
├── README.md      # Analisis lengkap (isi dengan poin 1-4 di atas)
├── JURNAL.md
└── bukti/          # (opsional) diagram pendukung
```

## Rubrik Penilaian (Tugas 9)

| Komponen | Bobot | Kriteria |
|---|---|---|
| Ketepatan rekomendasi deployment model | 30% | Argumen berbasis karakteristik NIST, bukan sekadar "private cloud lebih aman" tanpa detail |
| Ketepatan rekomendasi service model | 25% | Pemetaan komponen sistem ke IaaS/PaaS/SaaS masuk akal |
| Analisis risiko & mitigasi | 25% | Risiko spesifik ke konteks data kependudukan (bukan risiko generik) |
| Perbandingan dengan alternatif yang ditolak | 20% | Argumen penolakan jelas dan berdasar |

## Batasan Penggunaan AI (Level 2)

Kebijakan **Level 2 (AI Assisted Idea Generation & Structuring)** berlaku — lihat [`../RUBRIK-UMUM.md`](../RUBRIK-UMUM.md). Boleh bertanya ke AI gambaran umum karakteristik NIST/deployment model untuk brainstorming; **tidak boleh** meminta AI menuliskan rekomendasi/analisis akhir yang tinggal ditempel ke `README.md`. Catat pemakaian AI di "Log Penggunaan AI" pada `JURNAL.md`.

- Jawaban yang terasa seperti "template" (bisa dipakai untuk sistem cloud apa saja tanpa menyebut spesifik kependudukan/data pribadi/regulasi) akan dinilai rendah pada komponen ketepatan rekomendasi.
- `README.md` wajib membahas minimal satu skenario tandingan yang kalian buat sendiri (mis. "bagaimana jika instansi daerah tidak punya infrastruktur private cloud sendiri?") beserta jawabannya.
