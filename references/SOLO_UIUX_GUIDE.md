# Panduan UI/UX Solo Developer: Efisiensi, Aksesibilitas, & Pembekuan Desain

Dokumen ini adalah pedoman praktis bagi solo developer dalam merancang antarmuka pengguna yang profesional, ergonomis, dan aksesibel tanpa terjebak dalam perangkap *pixel-pushing* atau revisi visual tanpa batas dari klien.

---

## 1. Google Stitch Universal Engine: Efisiensi Maksimal Solo Developer

Solo developer dilarang membuang waktu membuat komponen primitif dari nol atau mendesain dua kali (desain di kanvas vektor lalu koding ulang). **Google Stitch adalah mesin tunggal standar untuk seluruh skala proyek (Kecil hingga Enterprise).**

### 1.1 Prosedur Tiga Langkah Menjalankan Stitch Bebas Slop
1. **Langkah 1: Kunci Design System (`DESIGN.md`)**:
   - Selalu upload template `DESIGN.md` terlebih dahulu ke project Stitch via `stitch_upload_design_md` lalu pasang via `stitch_create_design_system_from_design_md`.
   - Ini memastikan AI di Google Stitch tidak pernah menggunakan gradien ungu, kartu mengambang, atau font aneh.
2. **Langkah 2: Formula Prompting Layar Presisi**:
   - *Pola Prompt*:
     > *"Bangun antarmuka [Nama Layar] untuk pengguna [Role]. Tampilkan layout berbasis Tailwind yang bersih dan flat. Data yang ditampilkan: [Daftar Kolom/Field Riil]. Komponen: gunakan tabel data rapat, badge status warna semantik, dan tombol aksi bergaris batas 1px. Jangan gunakan drop-shadow tebal atau gradien warna. Terapkan design system yang telah terpasang."*
3. **Langkah 3: Menghubungkan Layar Menjadi Prototipe Nyata**:
   - Ambil kode komponen hasil Stitch.
   - Hubungkan tautan routing: `<a href="/target-halaman">`.
   - Deploy instan ke Vercel atau Cloudflare Pages sebagai live demo interaktif untuk klien.

### 1.2 Matriks Penggunaan Stitch Sesuai Skala:
| Skala | Cakupan di Google Stitch | Output Demo ke Klien |
| :--- | :--- | :--- |
| **Kecil (MVP)** | 3–5 layar (Happy path + Empty state) | Tautan Live Staging Vercel instan |
| **Menengah (B2B SaaS)** | 8–15 layar lengkap dengan varian 5 state | Live Staging Web interaktif penuh |
| **Besar & Enterprise** | 20+ layar mencakup seluruh user role & permission | Live Staging Web + Audit Kepatuhan Aksesibilitas WCAG AA |

---

## 2. Prinsip "Anti-Slop" Visual Solo Engineer

Desain perangkat lunak yang matang dicirikan oleh **keterbacaan dan kejelasan interaksi**, bukan ornamen grafis berlebihan:

1. **Aturan 60-30-10 untuk Warna**:
   - **60%**: Warna dasar netral (Putih `#FFFFFF` / Abu-abu terang `#F4F4F5` untuk background dan kontainer).
   - **30%**: Tipografi gelap berdaya kontras tinggi (Hitam `#09090B` / Slate `#334155`).
   - **10%**: Warna aksen utama brand klien (hanya untuk tombol tindakan utama, tautan aktif, dan penanda fokus).
2. **Kepatuhan Kontras Teks (WCAG 2.1 AA)**:
   - Jangan gunakan teks abu-abu pudar di atas background putih yang membuat mata lelah.
   - Rasio kontras teks biasa ke background wajib minimal **4.5 : 1**.
   - Rasio kontras teks besar (heading $> 18\text{px}$ bold) minimal **3.0 : 1**.
3. **Penyelamat Pengalaman: Empty State & Skeleton Loader**:
   - Jangan biarkan layar kosong melompong saat pengguna baru pertama kali mendaftar.
   - Selalu siapkan ilustrasi ringkas, teks panduan, dan tombol Call-to-Action (*"Belum ada dokumen yang dibuat. Klik tombol di bawah untuk membuat dokumen pertama Anda."*).
   - Gantikan spinner bulat berputar dengan *Skeleton Loader* yang menyerupai bentuk kartu/tabel agar layout halaman tidak bergeser (*zero layout shift*).

---

## 3. Protokol Walk-Through Prototipe Bersama Klien

Saat melakukan sesi demo prototipe dengan **Single PIC Klien**, arahkan percakapan pada alur fungsi, bukan debat selera artistik pribadi:

### Taktik Mengarahkan Feedback:
- **Jangan Tanya**: *"Gimana tampilannya, suka nggak dengan warnanya?"* (Pertanyaan ini memicu opini subjektif liar).
- **Pertanyaan yang Benar**:
  - *"Apakah urutan pengisian formulir ini sudah sesuai dengan SOP operasional staf Bapak/Ibu di kantor?"*
  - *"Apakah informasi status dokumen di halaman ini sudah cukup jelas bagi staf untuk mengambil tindakan berikutnya?"*

### Menghadapi Komentar Subjektif Klien:
- **Kasus**: *"Mas, warnanya kurang jreng ya, coba dibuat merah menyala dan logonya diperbesar."*
- **Respon Solo Dev**:
  > *"Warna saat ini dirancang mengikuti panduan identitas resmi perusahaan Bapak/Ibu dan telah lolos uji rasio kontras aksesibilitas standar WCAG 2.1 AA. Hal ini penting agar mata staf tidak cepat lelah saat bekerja berjam-jam di depan layar. Jika ingin warna lebih menonjol, kita bisa terapkan pada tombol aksi utama tanpa mengubah warna dasar halaman."*

---

## 4. Penegakan Protokol Pembekuan Desain (Design Freeze)

Setelah Single PIC Klien menyetujui alur prototipe pada dokumen `DESIGN_SPEC.md`:

1. **Kunci Seluruh Layout**:
   - Status desain resmi dinyatakan **FROZEN**.
   - Halaman Figma atau kode mockup dilabeli sebagai *Approved Baseline*.
2. **Batas Toleransi Perubahan Pasca-Freeze**:
   - *Boleh direvisi gratis*: Perubahan teks label (copywriting), penggantian warna tombol sedikit, atau penukaran ikon kecil.
   - *Wajib masuk Change Request (CR)*: Pemindahan posisi kolom di database yang mengubah struktur form, penambahan halaman baru, perombakan alur multi-step wizard, atau perubahan arsitektur navigasi.
