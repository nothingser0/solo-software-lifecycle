# Panduan Pemeliharaan Retainer Bulanan & SLA Solo Developer

Dokumen ini adalah buku pedoman taktis bagi solo developer untuk mengubah bisnis berbasis proyek satu kali (*one-off project*) menjadi arus kas bulanan berulang yang stabil (*Monthly Recurring Revenue* / MRR) melalui kontrak pemeliharaan (*Retainer SLA*), mengelola kepanikan klien, dan menegakkan batas jam kerja tanpa burnout.

---

## 1. Transformasi Arus Kas: Dari Proyek Lepas ke Retainer Bulanan

Kelemahan terbesar solo developer adalah siklus "Pesta dan Paceklik" (*Feast and Famine*): bulan ini mendapat uang besar dari proyek, bulan depan pendapatan nol karena harus mencari klien baru.

### Solusi: Mesin Retainer Pemeliharaan
Setiap proyek yang selesai di Modul 11 wajib ditawari kontrak pemeliharaan bulanan (Modul 12).
- Jika Anda memiliki **3–5 klien retainer** @ Rp 3.000.000 s/d Rp 7.000.000 per bulan:
  - Anda memiliki pendapatan dasar bulanan tetap sebesar **Rp 15.000.000 – Rp 35.000.000/bulan**.
  - Waktu yang dihabiskan untuk maintenance preventif per klien rata-rata hanya **3–5 jam per bulan**.

---

## 2. Struktur Formula Paket Retainer Solo Developer

| Nama Paket | Sasaran Klien | Fasilitas & Alokasi Waktu | Biaya Rekomendasi / Bulan |
| :--- | :--- | :--- | :---: |
| **Bronze (Essential)** | Skala Kecil (MVP / Startup tahap awal) | Pemantauan Uptime 24/7, Sentry monitoring, update patch keamanan dependensi bulanan, 1x uji restore backup DB. Alokasi: **5 Jam / Bulan**. | Rp 2.500.000 – Rp 4.000.000 |
| **Silver (Standard)** | Skala Menengah (B2B SaaS / Agensi) | Seluruh fasilitas Bronze + Alokasi **15 Jam / Bulan** untuk penambahan fitur minor / penyesuaian form. SLA respon $< 4\text{ jam}$ di hari kerja. | Rp 5.000.000 – Rp 8.000.000 |
| **Gold (Enterprise)** | Skala Besar / Korporasi | Seluruh fasilitas Silver + Alokasi **30 Jam / Bulan** + Dukungan On-Call akhir pekan untuk gangguan *Severity 1* (Sistem Down). | Rp 12.000.000 – Rp 20.000.000 |

---

## 3. Protokol Menangani Kepanikan Klien ("Klien Panik di WhatsApp")

Klien sering mengirim pesan histeris dengan tanda seru berderet: *"MAS APLIKASI ERROR SEMUA CEPAT DIBENERIN INI BISNIS BERHENTI!!!"*

### SOP 3 Langkah Menenangkan Klien (The Calming Protocol):
1. **Langkah 1: Jangan Ikut Panik, Akui Pesan Seketika ($< 15\text{ Menit}$)**:
   > *"Halo Pak/Bu [Nama PIC], terima kasih laporannya. Pesan sudah saya terima dan saat ini sedang saya cek di dashboard monitoring server."*
2. **Langkah 2: Mintakan Fakta Teknis Objektif (Isolasi Masalah)**:
   > *"Mohon bantu kirimkan tangkapan layar (screenshot) layar yang error dan apakah kendala ini dialami oleh seluruh staf kantor atau hanya pada 1 komputer tertentu?"*
3. **Langkah 3: Cek Dashboard Riil (Sentry / Uptime)**:
   - Sering kali kendala tersebut ternyata bukan sistem down, melainkan staf klien lupa password, koneksi WiFi kantor klien terputus, atau salah menginput format data.
   - Tunjukkan hasil investigasi berbasis fakta secara tenang dan solutif.

---

## 4. Disiplin Jam Kerja & Batas On-Call (Anti-Burnout)

Solo developer bukan robot. Jangan pernah menjanjikan dukungan siaga 24 jam sehari jika klien hanya membayar paket pemeliharaan murah:

1. **Kunci Jam Kerja di Kontrak**:
   - Jam operasional resmi adalah **Senin s/d Jumat pukul 09.00 – 17.00 WIB**.
   - Pesan yang masuk di hari Sabtu/Minggu atau malam hari akan ditangani pada hari kerja berikutnya pada pukul 09.00 WIB.
2. **Pengecualian On-Call Khusus**:
   - Respon di luar jam kerja (malam/akhir pekan) **HANYA BERLAKU** untuk insiden **Severity 1 (Sistem Mati Total)** pada Klien yang mengambil **Paket Gold Enterprise**.
   - Permintaan penambahan teks form atau revisi tampilan di hari Minggu wajib ditolak secara sopan dan dikerjakan hari Senin.
