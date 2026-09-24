# Panduan Deployment & Go-Live Produksi Solo Developer

Dokumen ini adalah buku pedoman praktis bagi solo developer dalam merencanakan, mengonfigurasi, dan mengeksekusi peluncuran perangkat lunak ke lingkungan produksi dengan aman, minim downtime, dan bebas kepanikan.

---

## 1. Waktu Rilis & Manajemen Risiko (The Timing Razor)

### Kapan Waktu Terbaik untuk Go-Live?
- **Hari Terbaik**: **Selasa atau Rabu**.
- **Jam Terbaik**: **Pukul 09.00 – 11.00 pagi** (WIB).
- **Alasannya**: Seluruh jam kerja masih tersisa seharian penuh jika ada kendala. Tim operasional klien sedang aktif di kantor untuk memvalidasi alur kerja, dan dukungan pelanggan (*customer support*) vendor cloud/payment gateway responsif penuh.

### Kapan Dilarang Keras Deploy?
- **Jumat Sore**: Jika terjadi galat tak terduga, Anda akan dipaksa lembur darurat di akhir pekan sendirian.
- **Malam Hari / Tengah Malam**: Kecuali pada proyek Enterprise yang mewajibkan *Scheduled Maintenance Window*, deploy tengah malam saat tubuh lelah meningkatkan risiko kesalahan ketik konfigurasi sebesar 300%.

---

## 2. Pola Migrasi Basis Data Tanpa Henti (Zero-Downtime Migration)

Jika memperbarui aplikasi yang sudah memiliki pengguna aktif, gunakan pola **Expand and Contract (Tiga Fase)** untuk menghindari downtime:

1. **Fase 1 (Expand)**:
   - Tambahkan tabel atau kolom baru dengan sifat *nullable* atau miliki nilai default:
     ```sql
     -- AMAN: Menambah kolom baru tanpa merusak kode lama
     ALTER TABLE users ADD COLUMN phone_number VARCHAR(20) NULL;
     ```
2. **Fase 2 (Transition)**:
   - Deploy kode baru yang mulai menulis data ke kolom baru dan kolom lama secara bersamaan.
3. **Fase 3 (Contract)**:
   - Setelah seluruh data lama terkonversi, barulah hapus kolom lama pada rilis minor berikutnya.
   - DILARANG menggunakan perintah destruktif seperti `DROP TABLE` atau `ALTER COLUMN ... TYPE` mendadak di jam sibuk.

---

## 3. Taktik DNS Propagation & SSL Hardening

1. **Turunkan Nilai TTL 24 Jam Sebelum Go-Live**:
   - Satu hari sebelum peluncuran, ubah nilai TTL (*Time-To-Live*) pada DNS record domain dari `86400` (24 jam) menjadi **`300` (5 menit)**.
   - Hal ini memastikan bahwa saat Anda mengubah IP server saat go-live, seluruh perangkat pengguna di internet akan mengenali server baru dalam waktu 5 menit, bukan menunggu seharian.
2. **Konfigurasi SSL Paling Aman**:
   - Jika menggunakan Cloudflare, gunakan mode **"Full (Strict)"** agar enkripsi berjalan penuh dari browser pengguna ke Cloudflare, dan dari Cloudflare ke server aplikasi Anda.

---

## 4. Setup Otomatisasi Backup Harian & Alerting

### 4.1 Skrip Otomatisasi Backup PostgreSQL ke Cloud Storage
Buat skrip `scripts/backup-db.sh` dan pasang di crontab server:
```bash
#!/bin/bash
BACKUP_DIR="/tmp/backups"
TIMESTAMP=$(date +"%Y%m%d_%H%M%S")
FILENAME="db_backup_$TIMESTAMP.sql.gz"

mkdir -p $BACKUP_DIR
# Dump dan kompresi basis data
pg_dump -U postgres -d legal_vault_prod | gzip > "$BACKUP_DIR/$FILENAME"

# Upload ke Cloudflare R2 / S3 via AWS CLI
aws s3 cp "$BACKUP_DIR/$FILENAME" "s3://legal-backup-bucket/daily/$FILENAME" --endpoint-url https://<account_id>.r2.cloudflarestorage.com

# Hapus backup lokal
rm -f "$BACKUP_DIR/$FILENAME"
echo "Backup $FILENAME berhasil diupload ke cloud storage."
```

Pasang di Cron Linux:
```text
# Berjalan setiap hari pukul 02.00 dini hari
0 2 * * * /bin/bash /app/scripts/backup-db.sh >> /var/log/db-backup.log 2>&1
```

### 4.2 Bot Alerting Down Telegram Gratis
Pasang pemantauan uptime gratis (Uptime Kuma atau BetterStack) yang mengecek endpoint `GET /api/health` setiap 60 detik. Hubungkan webhook ke bot Telegram solo dev agar Anda mendapat notifikasi seketika jika server mengalami kendala jaringan.

---

## 5. Panduan Khusus Rilis Aplikasi Mobile (Android & iOS)

### 5.1 Perlindungan Kunci Keystore Android
- Kunci `release-keystore.jks` adalah identitas unik aplikasi di Google Play Store. Jika kunci ini hilang atau terhapus:
  - Google Play Console **TIDAK MENGIZINKAN** pembaruan aplikasi selamanya (harus membuat nama paket baru dari nol dan kehilangan seluruh pengguna).
  - *SOP Solo Dev*: Wajib backup file `.jks` ke password vault terenkripsi (Bitwarden) dan simpan salinan cadangan di cold storage aman.

### 5.2 Strategi Menghadapi Review Apple App Store
- Apple memiliki tim peninjau manusia yang sangat ketat:
  - Sediakan akun demo penguji aktif (`tester-apple@domain.com` / password) di kolom *App Review Information* di App Store Connect.
  - Cantumkan tautan Kebijakan Privasi (*Privacy Policy URL*) dan Syarat Ketentuan (*Terms of Service*) yang valid di web.
  - Sediakan tombol *"Hapus Akun"* di dalam aplikasi jika aplikasi memiliki fitur registrasi (Pedoman Apple Guideline 5.1.1 wajib).

### 5.3 Pembaruan Tanpa Toko Aplikasi (Over-The-Air / OTA Updates)
- Untuk aplikasi React Native (Expo) atau Flutter (Shorebird):
  - Pasang modul OTA update agar Anda dapat merilis perbaikan bug darurat (*hotfix*) langsung ke ponsel pengguna dalam hitungan menit tanpa harus menunggu proses peninjauan toko aplikasi selama 2 hari.
