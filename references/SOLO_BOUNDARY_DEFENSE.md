# Panduan Pertahanan Batas Kerja Solo Developer (Solo Boundary Defense)

Dokumen ini adalah buku saku taktis bagi solo developer dan konsultan perangkat lunak untuk menjaga batasan lingkup, melindungi arus kas termin pembayaran, dan menangkis tekanan birokrasi klien tanpa merusak hubungan profesional.

---

## 1. Taktik Menolak Scope Creep (Penambahan Fitur Liar)

Klien sering melontarkan permintaan fitur tambahan secara kasual saat rapat atau obrolan chat. Solo dev dilarang keras langsung mengiyakan tanpa proses administrasi.

### Pola Komunikasi "Ya, Tapi Lewat Change Request"
Gunakan teknik **Validate, Position, Option**:

1. **Validasi**: Akui ide mereka bermanfaat (*"Ide filter pencarian otomatis berdasarkan geolokasi ini sangat bagus untuk kenyamanan pengguna..."*).
2. **Posisikan Batasan**: Tunjukkan dokumen acuan (*"...Namun sesuai dokumen Scope Statement v1.0 yang kita sepakati bersama, fitur pencarian di rilis ini dibatasi pada input teks nama kota..."*).
3. **Beri Opsi Keputusan**:
   > *"Agar jadwal go-live kita tanggal [Target Tanggal] tidak terganggu, ada 2 opsi terbaik:*
   > - *Opsi A: Kita masukkan ke dalam daftar prioritas pertama untuk **Fase 2 Pengembangan** setelah sistem resmi rilis.*
   > - *Opsi B: Kita kerjakan sekarang melalui **Change Request (CR)** resmi, dengan penyesuaian biaya sebesar Rp [Nominal] dan penambahan waktu kerja [X] hari.*
   >
   > *Opsi mana yang Bapak/Ibu prioritaskan untuk operasional saat ini?"*

---

## 2. Formula Standar Biaya & Jadwal Change Request (CR)

Jika klien memilih Opsi B, hitung penyesuaian secara terukur menggunakan rumus baku:

$$\text{Biaya CR} = (\text{Estimasi Jam Kerja Teknis} \times \text{Tarif Jam Anda}) + \text{Biaya Integrasi API/Infra Baru}$$

$$\text{Penambahan Jadwal} = \text{Estimasi Hari Kerja} + \text{Buffer QA (2 Hari Kerja)}$$

### Lembar Ringkas Change Request (CR Sheet)
Setiap perubahan wajib mencantumkan:
- **Nomor CR**: CR-[ID-Proyek]-001
- **Deskripsi Fitur Baru**: Penjelasan spesifik fungsionalitas yang diminta.
- **Dampak Teknis**: Modul yang terdampak dan pengujian ulang yang diperlukan.
- **Biaya Tambahan**: Nominal bersih.
- **Perubahan Tanggal Go-Live**: Dari tanggal lama $\to$ tanggal baru.
- **Persetujuan Tertulis**: Tanda tangan / konfirmasi email resmi dari Single PIC Klien.

---

## 3. Protokol Penegakan Aturan Single PIC

### Kasus: Staf Klien Lain Memberikan Instruksi Mendadak
Sering kali staf lapangan atau manajer lain di kantor klien meminta: *"Tolong tambahin kolom ini di laporan ya, penting banget."*

### Respon Standar Solo Developer:
> *"Terima kasih atas informasinya, Mas/Mbak [Nama Staf]. Sesuai Project Charter yang ditandatangani manajemen [Nama Perusahaan Klien], seluruh arahan perubahan fungsionalitas wajib divalidasi satu pintu melalui **[Nama Single PIC]**. Mohon disampaikan ke beliau agar dapat diinstruksikan secara tertulis ke saya. Hal ini untuk memastikan seluruh sistem tetap sinkron dan jadwal peluncuran tetap aman."*

---

## 4. Disiplin Termin Pembayaran (Payment Gating & Work Pause)

### Aturan Emas Arus Kas Solo Dev:
1. **No DP, No Work**: Jangan membuat sketsa UI mendalam atau menulis baris kode pertama sebelum DP Termin 1 masuk ke rekening.
2. **Staging is Yours, Production is Gated**:
   - Selama proses pengembangan hingga UAT, deploy hanya dilakukan di server staging milik akun developer sendiri.
   - Jangan pernah melakukan pointing domain klien atau instalasi di server produksi klien sebelum pembayaran termin UAT diterima.
3. **Protokol Hentikan Kerja Sementara (Work Pause Protocol)**:
   - Jika pembayaran termin terlambat lebih dari **7 hari kerja** sejak invoice dikirim:
     1. Kirim surat pemberitahuan penghentian sementara pekerjaan (*Notice of Work Suspension*).
     2. Hentikan seluruh aktivitas koding dan pengujian.
     3. Jadwal rilis resmi ditunda sampai pembayaran diselesaikan.

---

## 5. Batasan Pertahanan Hukum (Legal Shield)

Untuk menghindari tuntutan hukum tidak masuk akal dari klien skala korporat:

1. **Liability Cap (Batas Maksimal Ganti Rugi)**:
   - Pastikan di dalam SOW selalu ada klausul: *"Total liabilitas finansial maksimum Developer dibatasi maksimal sebesar total nilai uang yang telah diterima Developer dari Klien."*
2. **Klausul Disclaimer Teknologi**:
   - Untuk aplikasi di ranah sensitif (legal-tech, fintech, edutech): Nyatakan bahwa developer menyediakan perangkat lunak, bukan pemberi fatwa hukum, penasihat keuangan berlisensi, atau institusi penjamin simpanan.
3. **Penahanan Hak Cipta (IP Retention)**:
   - Hak kekayaan intelektual atas kode sumber baru beralih kepada klien pada saat **Pelunasan 100% dan penandatanganan Berita Acara Serah Terima (BAST)**.
