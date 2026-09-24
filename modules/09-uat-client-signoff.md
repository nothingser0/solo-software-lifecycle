# Modul 09: [GATE VALIDASI] UAT & Sign-Off Klien di Staging

Modul ini adalah **GERBANG VALIDASI PEMBLOKIR (BLOCKING VALIDATION GATE)** dalam siklus hidup proyek solo developer. Aturan mutlak: **DILARANG MELAKUKAN DEPLOYMENT KE SERVER PRODUKSI ATAU POINTING DOMAIN UTAMA SEBELUM GERBANG INI LOLOS.**

Tujuannya adalah memfasilitasi pengujian langsung oleh **Single PIC Klien** dan pengguna akhir (*key users*) di server Staging berdasarkan skenario di **`PRD.md`**, mengelola pelaporan perbaikan galat (*defect triage*), menangkis penambahan fitur berkedok bug, dan mengamankan penandatanganan **Berita Acara UAT (UAT Sign-Off Report)**.

---

## 1. Siklus Eksekusi Modul 09

```text
[ INPUT: Server Staging Lolos SIT (Modul 07) & Data Riil Terimpor (Modul 08) ]
                                    │
                                    ▼
[ LANGKAH 1: Penyiapan Skenario UAT & Kredensial Penguji Klien ]
  • Konversi Acceptance Criteria PRD.md menjadi Langkah Uji Bahasa Awam
  • Penerbitan Akun Penguji Klien di Staging (Super Admin, Manager, Staf)
                                    │
                                    ▼
[ LANGKAH 2: Pembukaan Sesi UAT & Penguncian Jendela Waktu (Testing Window) ]
  • Sesi Orientasi Singkat (30 Menit Demo Alur Skenario ke PIC Klien)
  • Penguncian Batas Waktu UAT (Maksimal 5–7 Hari Kerja)
  • Penerapan Klausul Deemed Acceptance (Penerimaan Otomatis jika Mangkir)
                                    │
                                    ▼
[ LANGKAH 3: Triase Temuan: Bug Nyata vs Penambahan Fitur (Scope Creep) ]
  • Severity 1 (Blocker): Wajib diperbaiki segera
  • Severity 2 (Major): Diperbaiki sebelum rilis produksi
  • Severity 3 (Minor/Cosmetic): Perbaikan wajar atau masuk masa garansi
  • Fitur Baru (Di luar PRD): Ditolak & dialihkan ke Change Request (CR)
                                    │
                                    ▼
[ LANGKAH 4: Perbaikan Bug di Branch fix/* & Verifikasi Ulang Staging ]
  • Solo Dev Memperbaiki Bug Valid di Branch Terisolasi ──► Merge ke staging
  • PIC Klien Melakukan Retest & Menandai Status RESOLVED di UAT_DEFECT_LOG.md
                                    │
                                    ▼
[ LANGKAH 5: Penandatanganan Berita Acara UAT (UAT Sign-Off) ]
  • Penyusunan Dokumen UAT_SIGNOFF_REPORT.md
  • Single PIC Klien Menandatangani Persetujuan Penerimaan Sistem
                                    │
                                    ▼
[ OUTPUT: Berita Acara UAT Bertandatangan ] ──► Buka Gerbang Modul 10: Production Deploy
```

---

## 2. Prinsip Perlindungan Solo Developer Saat UAT

### 1. Menangkis "Fitur Baru Berkedok Bug"
Klien sering kali mengatakan: *"Mas, ini kok tombolnya belum bisa kirim notifikasi ke Telegram ya? Ini error tolong diperbaiki."*

**Respon Baku Solo Dev**:
> *"Mari kita periksa bersama dokumen PRD dan FSD v1.0 yang telah kita sepakati, Pak/Bu. Pada Modul 4 rincian fitur notifikasi, sistem disepakati menggunakan pengiriman Email Transaksional, sedangkan integrasi Telegram tercatat sebagai Out-of-Scope (Fase 2). Karena sistem email di staging sudah berjalan sempurna, fungsionalitas ini berstatus Lolos UAT. Jika Bapak/Ibu ingin menambahkan modul Telegram sekarang, kami siap buatkan lembar Change Request (CR) terpisah."*

### 2. Klausul Penerimaan Otomatis (The Deemed Acceptance Clause)
Untuk mencegah klien menunda-nunda sesi testing selama berminggu-minggu yang menyebabkan proyek mangkrak:
- Jendela waktu pengujian ditetapkan maksimal **5–7 hari kerja**.
- Berlaku klausul baku di kontrak SOW: *"Apabila Klien tidak melakukan pengujian atau tidak memberikan catatan perbaikan tertulis dalam kurun waktu 10 (sepuluh) hari kerja sejak tautan Staging diserahkan, maka perangkat lunak secara hukum dianggap telah diterima secara penuh (Deemed Accepted) dan developer berhak melanjutkan ke tahap deployment produksi serta penagihan pelunasan."*

---

## 3. Matriks Triase Cacat (Defect Severity Matrix)

Setiap laporan kendala dari klien wajib diklasifikasikan ke dalam 4 tingkatan:

| Tingkat Keparahan | Definisi & Dampak | Batas Waktu Respon Dev | Dampak Terhadap UAT Sign-Off |
| :--- | :--- | :---: | :--- |
| **Severity 1 (Blocker)** | Sistem crash, data korup, pembayaran gagal, alur inti terputus total. | $< 24$ Jam | **MEMBLOKIR** sign-off (Wajib beres). |
| **Severity 2 (Major)** | Fitur penting tidak berjalan sesuai FSD, namun ada cara alternatif sementara (*workaround*). | $< 48$ Jam | Wajib diperbaiki sebelum deploy produksi. |
| **Severity 3 (Minor)** | Salah ketik (*typo*), pergeseran margin teks 2px, warna badge kurang kontras. | $< 72$ Jam | **TIDAK MEMBLOKIR** sign-off (Bisa dibereskan saat jeda rilis / garansi). |
| **Out-of-Scope (CR)** | Permintaan alur baru atau penambahan kolom database di luar PRD. | Dijawab hari itu | **DITOLAK DARI UAT** $\to$ Masuk lembar Change Request. |

---

## 4. Adaptasi Berdasarkan Skala Proyek

| Aspek UAT | Skala Kecil (MVP / Freelance) | Skala Menengah (B2B SaaS / Agensi) | Skala Besar & Enterprise |
| :--- | :--- | :--- | :--- |
| **Durasi Testing** | 2–3 hari kerja | 5–7 hari kerja | 10–14 hari kerja multi-divisi |
| **Peserta UAT** | Pemilik bisnis langsung (1 orang) | Single PIC + 2 staf operasional | Tim QA Klien, Business Analyst, & End Users |
| **Media Pencatatan** | Spreadsheet / UAT Checklist Markdown | Dokumen `UAT_DEFECT_LOG.md` formal | Issue tracker resmi (Jira / Linear / Redmine) |
| **Pengesahan** | Konfirmasi email persetujuan resmi | Berita Acara UAT bertandatangan digital | Dokumen Berita Acara UAT fisik bermeterai |

---

## 5. Artefak Keluaran (Deliverables)

Modul ini menghasilkan 3 berkas pengesahan:
1. **`UAT_SCENARIOS.md`**: Panduan langkah demi langkah pengujian bagi pengguna awam (menggunakan `templates/uat/UAT_SCENARIOS_TEMPLATE.md`).
2. **`UAT_DEFECT_LOG.md`**: Tabel pencatatan seluruh temuan kendala selama UAT beserta status perbaikannya (menggunakan `templates/uat/UAT_DEFECT_LOG_TEMPLATE.md`).
3. **`UAT_SIGNOFF_REPORT.md`**: Berita Acara Hasil UAT resmi bertandatangan Single PIC Klien yang mengesahkan bahwa seluruh fungsi sistem telah diterima (menggunakan `templates/uat/UAT_SIGNOFF_TEMPLATE.md`).

---

## 6. Kriteria Kelulusan Gerbang (Gate Exit Criteria)

Gerbang Modul 09 dinyatakan **LOLOS (PASS)** jika dan hanya jika:
- [x] Seluruh skenario pengujian berstatus **PASS** atau seluruh temuan Severity 1 & 2 telah **RESOLVED**.
- [x] Permintaan penambahan fitur baru telah dipisahkan secara tertulis ke lembar Change Request.
- [x] **Single PIC Klien telah menandatangani dokumen `UAT_SIGNOFF_REPORT.md`.**

*Begitu dokumen UAT ditandatangani, branch `staging` diizinkan di-merge ke branch `main`, dan sistem resmi melangkah ke **Modul 10: Deployment & Production Go-Live**.*
