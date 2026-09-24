# Modul 10: Deployment & Production Go-Live (Peluncuran Resmi ke Lingkungan Produksi)

Modul ini adalah tahap kesepuluh dalam siklus hidup proyek perangkat lunak untuk solo developer. Tujuannya adalah memindahkan kode yang telah lolos UAT dari branch `staging` ke branch `main`, mengonfigurasi infrastruktur produksi resmi (Domain DNS, SSL TLS 1.3, Cloudflare, Basis Data Produksi), mengeksekusi migrasi basis data tanpa henti (*zero-downtime*), mengaktifkan pemantauan observabilitas, dan melakukan pengujian pasca-rilis (*Post-Deployment Smoke Test*) hingga sistem resmi **LIVE ON PRODUCTION**.

---

## 1. Siklus Eksekusi Modul 10

```text
[ INPUT: Berita Acara UAT Sah (Modul 09) & Kode Stabil di Branch staging ]
                                    │
                                    ▼
[ LANGKAH 1: Gerbang Pemeriksaan Pra-Rilis (Go / No-Go Gate) ]
  • Verifikasi Berita Acara UAT Bertandatangan (Prasyarat Mutlak)
  • Waktu Rilis Aman: Dilarang rilis Jumat sore atau menjelang hari libur
  • Snapshot Backup Basis Data Sebelum Eksekusi Migrasi
                                    │
                                    ▼
[ LANGKAH 2: Penggabungan Branch Git & Pelabelan Versi Resmi (SemVer Tag) ]
  • Merge staging ──► main (Clean Production Codebase)
  • Beri Label Rilis: git tag -a v1.0.0 -m "Release Production v1.0.0"
  • Push ke Remote Repository untuk Memicu Pipeline CI/CD Produksi
                                    │
                                    ▼
[ LANGKAH 3: Penyediaan Infrastruktur & Kunci Rahasia Produksi ]
  • Konfigurasi DNS Domain Resmi (A / CNAME Record) & Sertifikat SSL TLS 1.3
  • Konfigurasi Variabel Lingkungan Produksi (.env.production - Kunci Asli)
  • Pengalihan Kredensial Payment Gateway dari Sandbox ──► Production Mode
                                    │
                                    ▼
[ LANGKAH 4: Eksekusi Migrasi Basis Data Produksi (Zero-Downtime) ]
  • Eksekusi Migrasi Skema SQL DDL pada Database Produksi
  • Impor Data Riil Klien Terverifikasi (Hasil Modul 08)
                                    │
                                    ▼
[ LANGKAH 5: Pengaktifan Pemantauan & Bot Alert (Observability Live) ]
  • Hubungkan Sentry / Error Tracker Otomatis
  • Setup Uptime Healthcheck Monitor (Ping setiap 60 detik ke /api/health)
  • Verifikasi Jadwal Otomatisasi Daily Backup Database
                                    │
                                    ▼
[ LANGKAH 6: Uji Verifikasi Pasca-Deployment (Production Smoke Test) ]
  • Uji Coba Transaksi Nyata di Domain Publik (https://app.klien.com)
  • Penyusunan Dokumen GO_LIVE_VERIFICATION_REPORT.md
                                    │
                                    ▼
[ OUTPUT: Sistem LIVE di Produksi & GO_LIVE_REPORT.md ] ──► Siap Masuk ke Modul 11: Handover & BAST
```

---

## 2. Tiga Aturan Emas Deployment Solo Developer

1. **Aturan "No Friday Deployment"**:
   - DILARANG melakukan peluncuran sistem baru ke lingkungan produksi pada hari **Jumat sore, akhir pekan, atau malam sebelum hari libur nasional**.
   - Jika terjadi kendala tak terduga, solo developer akan terjebak lembur darurat di akhir pekan tanpa dukungan tim teknis klien atau customer support vendor cloud.
   - Waktu rilis ideal: **Selasa atau Rabu pukul 09.00–11.00 pagi** (seluruh pihak siaga penuh).
2. **Kunci Kredensial Asli (Zero Sandbox Keys in Prod)**:
   - Pastikan variabel lingkungan di server produksi telah diganti dengan akun asli (Live API Key Payment Gateway, Live SMTP, Live Cloudflare R2), bukan akun pengujian sandbox staging.
3. **Wajib Memiliki Rencana Mundur Darurat (Rollback Plan)**:
   - Sebelum menyentuh tombol deploy, solo dev harus tahu persis cara mengembalikan sistem ke kondisi semula dalam waktu $< 15\text{ menit}$ jika terjadi kegagalan fatal.

---

## 3. Langkah demi Langkah Eksekusi

### Langkah 1: Pemeriksaan Pra-Rilis (Pre-Flight Checklist)
1. Periksa berkas `UAT_SIGNOFF_REPORT.md`: Pastikan tanda tangan Single PIC Klien sah.
2. Ambil snapshot backup manual database produksi (jika memperbarui sistem yang sudah ada):
   ```bash
   pg_dump -U postgres -d legal_vault_prod -F c -b -v -f "backup-pre-deploy-$(date +%Y%m%d).dump"
   ```

### Langkah 2: Merge Git & Pelabelan Versi Resmi
1. Pindah ke branch `main` dan gabungkan kode dari `staging`:
   ```bash
   git checkout main
   git merge --no-ff staging
   ```
2. Berikan label tag versi resmi:
   ```bash
   git tag -a v1.0.0 -m "Release Production v1.0.0 - Go-Live"
   git push origin main --tags
   ```

### Langkah 3: Konfigurasi DNS & SSL
1. Masuk ke dashboard DNS penyedia domain klien (Cloudflare, Niagahoster, Route53).
2. Arahkan DNS Record:
   - `Type A`: `@` $\to$ IP Server Produksi / Load Balancer.
   - `CNAME`: `app` atau `www` $\to$ domain hosting (Vercel / Cloud Run).
3. Verifikasi propagasi DNS menggunakan `dig` atau `nslookup`.
4. Pastikan sertifikat SSL terbit dan mendapatkan peringkat minimal **Grade A** di SSL Labs (TLS 1.3 aktif).

### Langkah 4: Eksekusi Migrasi Basis Data Produksi
Jalankan migrasi database produksi:
```bash
DATABASE_URL="postgresql://user:pass@prod-db:5432/db" pnpm db:migrate
```
*Catatan Solo Dev: Pastikan skrip migrasi bersifat aditif (hanya menambah kolom/tabel baru), dilarang menggunakan perintah destruktif (`DROP COLUMN` / `TRUNCATE`).*

### Langkah 5: Pengaktifan Observabilitas & Alerting
1. Pastikan DSN Sentry lingkungan produksi aktif (`environment: "production"`).
2. Daftarkan URL `https://app.klien.com/api/health` ke layanan uptime monitoring (Uptime Kuma, BetterStack, atau Cronitor).
3. Sambungkan bot alert ke grup Telegram atau nomor WhatsApp solo dev untuk notifikasi instan jika server down.

### Langkah 6: Production Verification Test (PVT)
Buka browser pada domain publik resmi:
1. Uji alur otentikasi login akun produksi.
2. Uji coba pembuatan 1 dokumen sampel dan pastikan PDF ter-generate serta tersimpan di bucket storage produksi.
3. Lakukan 1 transaksi pembayaran nominal kecil asli (misal Rp 10.000 via QRIS) untuk memvalidasi webhook payment gateway produksi.
4. Rangkum bukti hasil pengujian ke dalam dokumen **`GO_LIVE_VERIFICATION_REPORT.md`**.

---

## 4. Alur Khusus Deployment Aplikasi Mobile (Android & iOS)

Jika proyek mencakup aplikasi mobile (Flutter / React Native / Native), proses deployment memiliki karakteristik toko aplikasi (*App Store Ecosystem*) yang berbeda dari web:

```text
[ SOURCE CODE STAGING ]
           │
           ├──────────────────────────────────────┐
           ▼                                      ▼
   [ ANDROID RELEASE ]                     [ IOS RELEASE ]
   • Signing: Release Keystore (.jks)     • Signing: Distribution Cert & Provisioning Profile
   • Build: Android App Bundle (.aab)     • Build: iOS Archive (.ipa) via Xcode / Fastlane
   • Beta: Internal App Sharing           • Beta: Apple TestFlight Internal/External
           │                                      │
           ▼                                      ▼
[ GOOGLE PLAY CONSOLE ]                    [ APPLE APP STORE CONNECT ]
• Review: 24–72 Jam (Automated & Manual)  • Review: 24–48 Jam (Strict Apple Guidelines)
• Phased Rollout: 10% ──► 50% ──► 100%    • Phased Release: 7 Hari Bertahap
```

### 4.1 Manajemen Kunci & Signing (Keystore & Certificates)
- **Android**: Buat keystore produksi dan simpan berkas `.jks` serta password alias di brankas terenkripsi (Bitwarden). *Jika keystore hilang, aplikasi tidak akan pernah bisa di-update lagi di Google Play Store selamanya.*
- **iOS**: Daftarkan akun Apple Developer Program Klien ($99/tahun). Buat sertifikat distribusi dan App Store Provisioning Profile.

### 4.2 Strategi Beta Testing Sebelum Publik (TestFlight & Internal Sharing)
- Dilarang langsung melempar build pertama ke produksi publik.
- **Android**: Upload ke track **Internal Testing** di Google Play Console $\to$ bagikan link ke Single PIC Klien untuk verifikasi di ponsel Android asli.
- **iOS**: Upload ke **TestFlight** $\to$ invite akun email Apple ID milik Single PIC Klien untuk uji coba di perangkat iPhone nyata.

### 4.3 Mengantisipasi Waktu Review Toko Aplikasi (The Review Buffer)
- Tidak seperti web yang bisa rilis instan dalam 2 menit, rilis mobile terikat jadwal review manusia:
  - Apple App Store: Membutuhkan waktu **24–48 jam kerja**.
  - Google Play Console: Membutuhkan waktu **24–72 jam kerja** (terutama untuk akun pengembang baru).
- **Strategi Tangkal Komplain Klien**: Cantumkan di jadwal bahwa tanggal go-live mobile terhitung sejak status aplikasi berubah menjadi *Ready for Sale / Published* oleh toko aplikasi.

### 4.4 Penanganan Rollback & Pembaruan Darurat Mobile (OTA & Force Update)
- Aplikasi mobile tidak bisa di-rollback secara instan jika ada bug kritis di tangan pengguna.
- **Mekanisme Wajib Force-Update**: Aplikasi mobile wajib memiliki pengecekan versi minimum di splash screen (`GET /api/v1/app/version-check`). Jika ada bug kritis, backend dapat memaksa pengguna meng-update aplikasi ke versi terbaru sebelum bisa membuka dashboard.
- **Over-The-Air (OTA) Updates**: Untuk React Native (Expo Updates) atau Flutter (Shorebird), pasang mekanisme OTA patch agar perbaikan kode JavaScript/Dart minor bisa terdistribusi seketika tanpa harus melewati proses review toko aplikasi ulang.

---

## 5. Adaptasi Berdasarkan Skala Proyek

| Aspek Deployment | Skala Kecil (MVP / Freelance) | Skala Menengah (B2B SaaS / Agensi) | Skala Besar & Enterprise |
| :--- | :--- | :--- | :--- |
| **Infrastruktur** | PaaS (Vercel / Railway / Render) | Managed Cloud Run / Docker VPS + Managed DB | Multi-region AWS/GCP, Kubernetes, Private VPC |
| **Strategi Rilis** | Rolling restart instan (< 1 menit) | Blue-Green Deployment / Container Swap | Canary Deployment bertahap (10% $\to$ 50% $\to$ 100%) |
| **Jadwal Rilis** | Jam kerja santai (Selasa pagi) | Scheduled maintenance (Selasa 10.00 WIB) | Scheduled Window malam hari dengan persetujuan CAB |
| **Monitoring** | Sentry gratis + Uptime Kuma bot | Sentry + BetterStack log aggregation | APM penuh (Datadog/New Relic) + PagerDuty SLA |

---

## 5. Artefak Keluaran (Deliverables)

Modul ini menghasilkan 3 dokumen eksekusi:
1. **`DEPLOYMENT_RUNBOOK.md`**: Panduan langkah demi langkah teknis proses rilis, konfigurasi server, dan variabel lingkungan produksi (menggunakan `templates/deploy/DEPLOYMENT_RUNBOOK_TEMPLATE.md`).
2. **`ROLLBACK_PLAN.md`**: Prosedur darurat pemulihan jika terjadi kegagalan fatal saat go-live (menggunakan `templates/deploy/ROLLBACK_PLAN_TEMPLATE.md`).
3. **`GO_LIVE_VERIFICATION_REPORT.md`**: Laporan bukti bahwa domain resmi aktif, SSL aman, database stabil, dan sistem siap dipakai operasional klien (menggunakan `templates/deploy/GO_LIVE_REPORT_TEMPLATE.md`).

---

## 6. Kriteria Kelulusan Gerbang (Gate Exit Criteria)

Gerbang Modul 10 dinyatakan **LOLOS (PASS)** jika:
- [x] Branch `main` telah diberi tag versi SemVer resmi (`v1.0.0`).
- [x] Domain resmi (`https://app.klien.com`) aktif dengan enkripsi SSL/TLS 1.3 valid.
- [x] Migrasi database produksi sukses dijalankan tanpa kehilangan data.
- [x] Seluruh variabel lingkungan menggunakan akun live produksi (bukan sandbox).
- [x] Uji transaksi nyata pasca-rilis (*PVT*) berhasil 100%.
- [x] Sistem monitoring uptime dan pelacak error Sentry aktif.

*Begitu sistem resmi Live di Produksi, sistem melangkah ke tahap penutupan komersial dan serah terima: **Modul 11: [GATE PENYERAHAN] Pelunasan, Training, BAST, & Handover Repositori**.*
