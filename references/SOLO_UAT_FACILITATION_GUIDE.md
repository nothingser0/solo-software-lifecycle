# Panduan Fasilitasi UAT & Negosiasi Solo Developer

Dokumen ini adalah buku panduan taktis bagi solo developer untuk memandu proses pengujian penerimaan pengguna (*User Acceptance Test* / UAT) bersama klien, mengelola temuan cacat, menangkis revisi liar, dan mengamankan penandatanganan Berita Acara tanpa konflik.

---

## 1. Psikologi Klien Saat Sesi UAT

Klien sering menunda-nunda sesi UAT atau mendadak merasa cemas menjelang peluncuran. Hal ini biasanya dipicu oleh dua alasan:
1. **Beban Kerja Harian**: Karyawan klien sibuk dengan tugas operasional kantor sehingga pengujian sistem dianggap sebagai beban tambahan.
2. **Kecemasan Tanggung Jawab**: Single PIC Klien takut disalahkan oleh direksinya jika sistem ada bug setelah rilis.

### Taktik Membuka UAT (The 30-Minute Kick-Off Session):
- **Jangan Beri Akses Begitu Saja**: Dilarang hanya mengirimkan email: *"Pak/Bu, ini link staging-nya silakan dites ya."* Klien akan bingung harus mulai dari mana dan akhirnya tidak melakukan pengujian.
- **Jadwalkan Sesi Walk-Through 30 Menit**:
  - Dampingi PIC Klien via video call (Zoom / Google Meet).
  - Tunjukkan berkas **`UAT_SCENARIOS.md`** di layar.
  - Bimbing PIC mencoba Skenario 1 (Login dan Buat Dokumen Pertama) secara langsung. Begitu mereka melihat alur pertama berhasil, kecemasan mereka akan hilang dan mereka siap melanjutkan mandiri.

---

## 2. Naskah Komunikasi Menangkis Scope Creep Berkedok Bug

Klien sering memanfaatkan sesi UAT untuk meminta fitur tambahan secara gratis dengan dalih "sistem belum lengkap":

### Kasus 1: "Mas, bisa sekalian ditambahin export ke PDF warna abu-abu dan kirim WhatsApp otomatis?"
**Pola Respon Solo Dev**:
> *"Usulan fitur integrasi WhatsApp ini sangat bagus untuk meningkatkan kecepatan notifikasi. Mari kita cek bersama dokumen spesifikasi PRD dan FSD v1.0 yang menjadi acuan kontrak kita. Di sana disepakati bahwa sistem menggunakan pengiriman Email Transaksional, sedangkan WhatsApp masuk ke dalam daftar rencana pengembangan lanjutan (Fase 2).*
>
> *Agar jadwal peluncuran sistem utama kita tidak tertunda, mari kita selesaikan pengesahan fitur yang ada saat ini terlebih dahulu. Setelah sistem live, kita bisa langsung lanjutkan implementasi WhatsApp melalui lembar Change Request (CR) terpisah."*

### Kasus 2: "Mas, tata letak form-nya kok begini ya, saya mau tombolnya dipindah ke kiri dan kolomnya dipecah jadi 3 tab."
**Pola Respon Solo Dev**:
> *"Tata letak saat ini disusun persis mengikuti berkas `DESIGN_SPEC.md` yang telah disahkan pada lembar **Design Freeze** tanggal [Tanggal Persetujuan Modul 04]. Karena struktur form ini sudah terikat dengan logika database di backend, perombakan susunan kolom saat ini akan membutuhkan rekonstruksi skema ulang.*
>
> *Saran terbaik saya, kita jalankan sistem dengan layout yang telah disepakati ini selama 30 hari masa operasional. Jika dari hasil penggunaan harian staf merasa butuh penyesuaian, kita lakukan optimalisasi pada jadwal rilis pembaruan berikutnya."*

---

## 3. Protokol Penegakan "Deemed Acceptance Clause" (Klien Mangkir)

Jika Klien tidak melakukan pengujian dan mengabaikan pesan pengembang selama masa testing window:

### Kronologi Penegakan Status Notice:
1. **Hari ke-3**: Kirim pesan pengingat ramah (*Gentle Reminder*):
   > *"Halo Pak/Bu [Nama PIC], menyambung pembukaan sesi UAT per tanggal [Tanggal], apakah ada kendala dalam mencoba Skenario UAT di staging? Kami siap mendampingi jika ada alur yang membutuhkan klarifikasi."*
2. **Hari ke-7**: Kirim surat pengingat resmi (*Formal Notice*):
   > *"Selamat siang Pak/Bu, kami mengingatkan bahwa periode pengujian UAT proyek [Nama Proyek] akan berakhir dalam 3 hari kerja (sesuai SOW pasal batas pengujian 7 hari kerja). Mohon catatan pengujian dapat diserahkan sebelum tanggal [Tenggat] agar jadwal rilis produksi tetap terjaga."*
3. **Hari ke-10**: Penerbitan Surat Penerimaan Otomatis (*Notice of Deemed Acceptance*):
   > *"Mengingat periode pengujian UAT telah melewati batas waktu 10 hari kerja tanpa adanya catatan cacat teknis kritis yang dilaporkan, maka sesuai ketentuan SOW Pasal [X] ayat [Y], sistem secara hukum dinyatakan telah diterima secara memuaskan (**Deemed Accepted**). Dengan ini kami akan melanjutkan persiapan deployment ke lingkungan produksi (Modul 10)."*
