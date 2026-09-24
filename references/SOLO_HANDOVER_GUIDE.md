# Panduan Serah Terima & BAST Solo Developer

Dokumen ini adalah pedoman taktis bagi solo developer dan konsultan teknis dalam mengeksekusi penutupan proyek, mengamankan pelunasan tagihan 100%, menetapkan batasan jatah pelatihan, memindahkan repositori, dan menandatangani Berita Acara Serah Terima (BAST) yang sah secara hukum.

---

## 1. Disiplin "Payment-Gated Handover": Menjaga Posisi Tawar Solo Dev

Banyak solo developer pemula melakukan kesalahan fatal: *menyerahkan akses admin root, mengalihkan repositori GitHub, dan menandatangani BAST sebelum pembayaran termin terakhir masuk ke rekening.*

### Realita di Perusahaan Klien:
Begitu tim teknis klien sudah memegang repositori dan password server, urgensi divisi keuangan (*finance*) mereka untuk mencairkan sisa tagihan Anda akan menurun drastis. Proses pembayaran sering diundur berminggu-minggu dengan berbagai alasan birokrasi internal.

### Urutan Mutlak Penyerahan Aset:
```text
1. Sistem Live di Produksi (Klien login sebagai User Biasa / Demo)
                    │
                    ▼
2. Terbitkan Invoice Pelunasan (Termin Final 10%–20%)
                    │
                    ▼
3. Tunggu Dana Masuk & Terverifikasi di Rekening Bank Anda
                    │
       ┌────────────┴────────────┐
       ▼                         ▼
 [ DANA BELUM CAIR ]       [ DANA SUDAH LUNAS 100% ]
 Tahan transfer repo       1. Laksanakan Sesi Training (1–2x)
 Tahan password root       2. Transfer Kepemilikan GitHub/Cloud
 Berikan akses tester      3. Tanda Tangani BAST Bermeterai
                           4. Masa Garansi Resmi Dimulai
```

---

## 2. Pengelolaan Jatah Pelatihan Pengguna (Training Quota)

Jangan biarkan diri Anda menjadi staf layanan pelanggan (*customer service*) atau tukang training gratisan selamanya.

### Aturan Baku Sesi Pelatihan:
1. **Batas Jatah Maksimal 2 Sesi**:
   - *Sesi 1 (60 Menit)*: Pelatihan alur operasional staf pengguna harian.
   - *Sesi 2 (60 Menit)*: Pelatihan konfigurasi dan manajemen untuk Super Admin.
2. **Wajib Merekam Video Sesi**:
   - Seluruh sesi pelatihan daring wajib direkam (format MP4).
   - Unggah rekaman tersebut ke Google Drive atau link privat dan serahkan bersama berkas `USER_MANUAL.md`.
3. **Klausul Pelatihan Tambahan**:
   - Jika di masa mendatang klien merekrut karyawan baru dan meminta developer melatih ulang secara tatap muka/online, cantumkan aturan: *"Pelatihan tambahan di luar 2 sesi yang disepakati dikenakan biaya jasa profesional sebesar Rp [X] per sesi."*

---

## 3. Protokol Penyerahan Kredensial Terenkripsi (Zero Plaintext)

Jangan pernah mengirimkan kredensial server produksi melalui pesan WhatsApp atau email teks terbuka karena rentan disadap atau tersimpan di backup cloud ponsel yang tidak aman.

### Gunakan Jalur Sekali Pakai (One-Time Secret Sharing):
- Gunakan layanan gratis seperti **Bitwarden Send** (`bitwarden.com/send`), **Yopass** (`yopass.se`), atau **1Password Share**.
- Atur parameter keamanan:
  - *Masa berlaku tautan*: Maksimal 24 jam.
  - *Batas pembukaan*: Otomatis hancur setelah dibuka 1 kali (*Delete after 1 view*).
  - *Kata sandi tambahan*: Berikan password pembuka tautan melalui media yang berbeda (misal: link dikirim via Email, password pembuka dikirim via SMS/Telepon).

---

## 4. Nilai Hukum BAST di Indonesia

Dokumen **Berita Acara Serah Terima (BAST)** adalah dokumen paling krusial bagi solo developer di mata hukum Indonesia (KUHPerdata Pasal 1320 & 1338):

1. **Bukti Pemenuhan Kewajiban**:
   - BAST adalah bukti mutlak bahwa developer telah menyelesaikan seluruh kewajibannya sesuai kontrak SOW. Klien tidak bisa lagi menggugat atau menuduh developer melakukan wanprestasi (*default/breach of contract*).
2. **Kunci Pembatas Scope Creep**:
   - Begitu BAST ditandatangani, klien tidak berhak meminta fitur baru secara gratis. Segala permintaan tambahan otomatis menjadi objek kontrak baru atau jasa Change Request berbayar.
3. **Penanda Resmi Mulai Garansi**:
   - Masa garansi (30/60/90 hari) **BARU MULAI DIHITUNG** sejak tanggal tanda tangan BAST. Tanpa BAST, klien sering menuntut garansi seumur hidup.
4. **Meterai Rp 10.000,-**:
   - Dokumen BAST wajib dibubuhi meterai fisik Rp 10.000,- yang ditandatangani menimpa meterai, atau menggunakan **e-Meterai Peruri** resmi untuk format digital.
