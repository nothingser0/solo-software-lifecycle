# Modul 11: [GATE PENYERAHAN] Pelunasan, Training, BAST, & Handover Repositori

Modul ini adalah **GERBANG PENYERAHAN & PENUTUPAN KOMERSIAL (CLOSURE & HANDOVER GATE)** dalam siklus hidup proyek solo developer. Aturan mutlak: **DILARANG MENYERAHKAN KEPEMILIKAN REPOSITORI GIT (TRANSFER OWNERSHIP), KREDENSIAL ROOT SERVER, DAN MENANDATANGANI BAST SEBELUM SISA PEMBAYARAN PELUNASAN (100%) MASUK DAN TERKONFIRMASI DI REKENING BANK DEVELOPER.**

Tujuannya adalah menyelenggarakan pelatihan operasional (*training*) bagi staf klien dengan jatah terukur, menyerahkan dokumentasi panduan pengguna (*User Manual*), memindahkan kepemilikan repositori secara aman, mengamankan pelunasan pembayaran akhir, dan menandatangani **Berita Acara Serah Terima (BAST)** berkekuatan hukum yang menjadi penanda dimulainya Masa Garansi (Modul 12).

---

## 1. Siklus Eksekusi Modul 11

```text
[ INPUT: Sistem Live di Produksi & GO_LIVE_REPORT.md dari Modul 10 ]
                                    │
                                    ▼
[ LANGKAH 1: Penerbitan Invoice Pelunasan (Termin 4: 10% – 20%) ]
  • Terbitkan Invoice Final Berdasarkan Bukti Go-Live Report
  • Berikan Tenggat Pembayaran Sesuai SOW (Maksimal 7 Hari Kerja)
                                    │
                                    ▼
[ LANGKAH 2: Pelatihan Pengguna (Training & Onboarding Klien) ]
  • Jatah Sesi Terbatas: 1x Sesi Staf Operasional & 1x Sesi Super Admin
  • Rekam Video Tutorial Ringkas & Serahkan Dokumen USER_MANUAL.md
  • Sesi Tambahan di Luar Jatah Wajib Dikenakan Biaya Training Tambahan
                                    │
                                    ▼
[ LANGKAH 3: Gerbang Penguncian Arus Kas (Payment-Gated Verification) ]
  • Cek Mutasi Rekening Bank Developer
  • DANA PELUNASAN SUDAH DITERIMA PENUH ──► Lanjut ke Langkah 4
  • DANA BELUM DITERIMA ──► TAHAN TRANSFER REPO & AKUN ROOT SERVER
                                    │
                                    ▼
[ LANGKAH 4: Transfer Kepemilikan Repositori & Kredensial Terenkripsi ]
  • Transfer Kepemilikan Repositori GitHub/GitLab ke Organisasi Klien
  • Serahkan Kredensial Produksi Melalui Jalur Terenkripsi (Bitwarden/1Password)
                                    │
                                    ▼
[ LANGKAH 5: Penandatanganan Berita Acara Serah Terima (BAST Sah) ]
  • Cetak Dokumen BAST Bermeterai Cukup (Rp 10.000,-) atau e-Meterai Peruri
  • Ditandatangani Bersama oleh Single PIC Klien & Developer
  • Pemicu Resmi: Masa Garansi Perbaikan Bug (Modul 12) Mulai Berjalan
                                    │
                                    ▼
[ OUTPUT: BAST Bertandatangan, Repositori Terpindah, & Pembayaran Lunas 100% ]
```

---

## 2. Aturan Emas Penyerahan Solo Developer: "No Pay, No Root"

### Mengapa Disiplin Ini Mutlak?
Di banyak perusahaan klien (terutama skala Menengah dan Korporat), divisi keuangan (*finance*) sering menunda pembayaran termin terakhir selama berminggu-minggu jika tim teknis mereka sudah memegang kendali penuh atas repositori kode dan server. 

Satu-satunya posisi tawar (*leverage*) solo developer untuk memastikan tagihan terbayar lunas adalah **kendali atas akun root infrastruktur dan kepemilikan repositori**.

### Protokol Penyerahan Bertahap yang Aman:
1. **Saat Go-Live (Modul 10)**: Berikan akses ke klien hanya sebagai **User Biasa / Admin Operasional** di aplikasi web (`app.klien.com`), bukan akun root cloud provider atau admin GitHub.
2. **Saat Menunggu Pembayaran**: Sistem sudah bisa dipakai oleh staf klien untuk bekerja, namun Developer masih memegang hak akses infrastruktur.
3. **Setelah Pembayaran 100% Cair**: Baru lakukan transfer kepemilikan repositori Git dan serahkan password database/cloud root.

---

## 3. Langkah demi Langkah Eksekusi

### Langkah 1: Sesi Pelatihan Pengguna (Training Quota Management)
Untuk mencegah solo dev dijadikan staf bantuan operasional (*helpdesk*) cuma-cuma selamanya:
- **Jatah Standar**: Maksimal **2 kali sesi pertemuan daring** (@ 60 menit via Zoom/Google Meet):
  - *Sesi 1*: Penggunaan alur harian staf (Input data, pembuatan dokumen, alur tanda tangan).
  - *Sesi 2*: Manajemen sistem Super Admin (Manajemen user, hak akses, audit trail, export data).
- **Aset Pelatihan**: Rekam sesi pelatihan tersebut dan serahkan rekamannya bersama berkas **`USER_MANUAL.md`** agar staf baru klien di masa depan bisa belajar mandiri tanpa menghubungi developer lagi.

### Langkah 2: Protokol Penyerahan Kredensial (Zero Plaintext Sharing)
Dilarang keras mengirimkan password server, master key database, atau API secret via chat WhatsApp atau email teks terbuka.
- Gunakan jalur aman sekali pakai seperti **Bitwarden Send**, **1Password**, atau **Yopass** (tautan terenkripsi yang otomatis hancur setelah dibuka sekali).
- Rangkum daftar akun yang diserahterimakan pada berkas **`HANDOVER_PROTOCOL.md`**.

### Langkah 3: Transfer Kepemilikan Repositori Git
1. Masuk ke GitHub / GitLab project settings:
   - Pilih menu **"Transfer Ownership"** $\to$ Masukkan nama akun organisasi Klien.
2. Pastikan Klien mengangkat PIC teknis mereka menjadi Owner baru.
3. Hapus akses token pribadi (*Personal Access Token*) Anda dari pengaturan CI/CD klien dan gantikan dengan token milik organisasi klien.

### Langkah 4: Penandatanganan BAST Resmi
1. Siapkan dokumen **`BAST_TEMPLATE.md`** yang memuat:
   - Rincian deliverable yang diserahkan (Source code Git, URL live, User manual).
   - Pengesahan bahwa sistem telah diterima dengan baik.
   - Pernyataan tanggal mulai dan tanggal berakhirnya **Masa Garansi (Modul 12)**.
2. Bubuhkan meterai fisik Rp 10.000,- (atau e-Meterai resmi) dan tanda tangani bersama Single PIC Klien.

---

## 4. Adaptasi Berdasarkan Skala Proyek

| Parameter Handover | Skala Kecil (MVP / Freelance) | Skala Menengah (B2B SaaS / Agensi) | Skala Besar & Enterprise |
| :--- | :--- | :--- | :--- |
| **Sesi Training** | 1 sesi singkat via Google Meet | 2 sesi terstruktur (Staf & Admin) | Multi-sesi per divisi + Rekaman LMS |
| **Panduan Pengguna** | 1-page Quickstart Guide Markdown | Dokumen formal `USER_MANUAL.md` | User Manual PDF lengkap + API Docs OpenAPI |
| **Transfer Akun** | Invite admin via dashboard Vercel/DB | Transfer repo Git + Kredensial Vault | Transfer Cloud Tenant (AWS Org) + BAA/DPA |
| **Dokumen BAST** | BAST format sederhana via email | BAST resmi bermeterai Rp 10.000 | BAST formal legal korporasi + Berita Acara UAT |

---

## 5. Artefak Keluaran (Deliverables)

Modul ini menghasilkan 3 dokumen penutupan:
1. **`USER_MANUAL.md`**: Buku panduan operasional bagi admin dan staf pengguna sistem (menggunakan `templates/handover/USER_MANUAL_TEMPLATE.md`).
2. **`HANDOVER_PROTOCOL.md`**: Berita acara teknis transfer kepemilikan repositori Git, daftar akun yang diserahterimakan, dan checklist pelepasan wewenang (menggunakan `templates/handover/HANDOVER_PROTOCOL_TEMPLATE.md`).
3. **`BAST_REPORT.md`**: Berita Acara Serah Terima sah bermeterai yang mengalihkan hak lisensi/kepemilikan perangkat lunak dan mengaktifkan masa garansi (menggunakan `templates/handover/BAST_TEMPLATE.md`).
4. **Bukti Pembayaran Lunas 100%**: Konfirmasi mutasi bank penerimaan termin final.

---

## 6. Kriteria Kelulusan Gerbang (Gate Exit Criteria)

Gerbang Modul 11 dinyatakan **LOLOS (PASS)** jika:
- [x] **Dana sisa pelunasan (100%) telah masuk dan terkonfirmasi di rekening bank Developer.**
- [x] Sesi pelatihan staf dan admin telah dilaksanakan sesuai jatah yang disepakati.
- [x] Repositori Git dan kredensial produksi telah dialihkan ke organisasi Klien.
- [x] Dokumen `USER_MANUAL.md` telah diserahkan ke klien.
- [x] **Dokumen BAST resmi bermeterai telah ditandatangani oleh kedua belah pihak.**

*Dengan lolosnya gerbang ini, fase pengembangan proyek resmi DITUTUP (CLOSED), dan hubungan beralih ke masa pasca-proyek: **Modul 12: Masa Garansi (Bug Fix) & Transisi ke Monthly Retainer / SLA**.*
