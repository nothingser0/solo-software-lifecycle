# Design Specification & UI Wireflow

> Dokumen spesifikasi desain antarmuka, arsitektur informasi, token visual, dan inventaris layar untuk mengunci alur visual sebelum tahap arsitektur teknis dan koding.

---

## 1. Metadata Desain
- **Nama Proyek**: [Nama Sistem / Aplikasi]
- **Klien**: [Perusahaan / Organisasi Klien]
- **Solo Lead UI/UX & Engineer**: [Nama Anda]
- **Versi Desain**: 1.0.0
- **Status Desain**: [Draft / In Review / Frozen (Approved)]
- **Google Stitch Project ID**: `projects/[PROJECT_ID]`
- **Design System Asset ID**: `assets/[ASSET_ID]` (dari `DESIGN.md`)
- **Tautan Live Interactive Prototype**: `[https://staging-preview-url atau Stitch Viewer URL]`
- **Tanggal Persetujuan**: [YYYY-MM-DD]

---

## 2. Arsitektur Informasi & Peta Rute URL (Sitemap)

| Rute URL | Nama Halaman | Peran Pengguna yang Mengakses | Fungsi Utama & Interaksi |
| :--- | :--- | :--- | :--- |
| `/login` | Halaman Masuk | Publik / Seluruh Pengguna | Form input email, password/OTP, link reset |
| `/dashboard` | Dashboard Utama | Super Admin, Manager, Staf | Ringkasan statistik, daftar dokumen terbaru, CTA baru |
| `/documents` | Manajemen Dokumen | Seluruh Pengguna Terotentikasi | Tabel dokumen, filter status, fitur pencarian, ekspor |
| `/documents/new` | Pembuat Dokumen | Staf Operasional, Manager | Form dinamis input variabel template, preview PDF |
| `/documents/:id` | Detail Dokumen | Seluruh Pengguna Terkait | Tampilan status audit trail, tombol tanda tangan digital |
| `/sign/:token` | Halaman Tanda Tangan | Tamu Eksternal / Signer | Canvas tanda tangan digital tanpa perlu login akun |

---

## 3. Sistem Desain & Token Visual (Design Tokens)

### 3.1 Tipografi
- **Primary Font (UI & Body Text)**: `Inter` atau `Geist Sans` (Fallback: `system-ui, sans-serif`)
- **Monospace Font (Kode, Hash, Angka Finansial)**: `JetBrains Mono` atau `Geist Mono`
- **Skala Ukuran Teks**:
  - Heading 1: `32px` / `line-height: 40px` / `font-weight: 700`
  - Heading 2: `24px` / `line-height: 32px` / `font-weight: 600`
  - Body Text: `14px` / `line-height: 20px` / `font-weight: 400`
  - Caption / Helper: `12px` / `line-height: 16px` / `font-weight: 400`

### 3.2 Palet Warna & Verifikasi Kontras (WCAG 2.1 AA)

| Token Warna | Nilai HEX | Penggunaan UI | Rasio Kontras ke Background | Status Kelulusan |
| :--- | :---: | :--- | :---: | :---: |
| `background` | `#FFFFFF` | Latar belakang halaman utama | - | Base |
| `surface` | `#F4F4F5` | Kartu komponen, container form | - | Base |
| `text-primary` | `#09090B` | Teks judul dan body text utama | **19.8 : 1** | PASS (AAA) |
| `text-muted` | `#71717A` | Placeholder, label sekunder | **4.6 : 1** | PASS (AA) |
| `primary` | `#[Brand]` | Tombol utama, link aktif | **$\ge 4.5 : 1$** | PASS (AA) |
| `success` | `#16A34A` | Badge status sukses, konfirmasi | **$\ge 4.5 : 1$** | PASS (AA) |
| `destructive` | `#DC2626` | Tombol hapus, alert error | **$\ge 4.5 : 1$** | PASS (AA) |

---

## 4. Pilihan Library Komponen Dasar
- **Pustaka UI**: `Shadcn UI` (berbasis Radix UI primitives & Tailwind CSS).
- **Icon Set**: `Lucide Icons` (konsisten, stroke 1.5px atau 2px).
- **Komponen Form**: `React Hook Form` + validasi `Zod`.
- **Komponen Notifikasi**: `Sonner` (toast notification ringan di pojok kanan bawah).

---

## 5. Inventaris Lengkap Seluruh Layar (Exhaustive Screen Inventory - 100% Coverage)

> **ATURAN CAKUPAN MUTLAK**: Tabel ini WAJIB mencakup **100% seluruh halaman/layar** yang telah didefinisikan pada batasan lingkup (`SCOPE_STATEMENT.md` / `PRD.md`) dari awal hingga akhir tanpa terkecuali. Jika dalam lingkup terdapat 10, 20, atau 100 halaman, seluruhnya WAJIB didaftarkan dan di-generate di Google Stitch dengan Screen ID unik masing-masing. DILARANG memangkas atau hanya memilih sebagian sampel layar.

| Kode Layar | Nama Layar | Google Stitch Screen ID | Default State | Loading Skeleton | Empty State | Error State |
| :---: | :--- | :--- | :--- | :--- | :--- | :--- |
| **SCR-01** | Dashboard | `screens/[ID_01]` | Widget statistik & tabel | Skeleton bar abu-abu | Banner "Belum ada dokumen" + CTA buat | Banner server timeout |
| **SCR-02** | Form Dokumen | `screens/[ID_02]` | Input form dinamis terstruktur | Tombol submit disable + loader | - | Pesan teks merah inline |
| **SCR-03** | Detail & Sign | `screens/[ID_03]` | Preview PDF + kotak tanda tangan | Skeleton render dokumen | - | Alert gagal verifikasi hash |
| **SCR-..** | [Seluruh Layar Lain] | `screens/[ID_..]` | [Wajib isi 100% tanpa ada yang di-skip] | ... | ... | ... |

---

## 6. Standar Aksesibilitas & Responsif
- [x] Seluruh tombol interaktif memiliki `focus-visible` ring untuk navigasi keyboard (Tombol Tab).
- [x] Input form memiliki label teks eksplisit (`<label htmlFor="...">`) dan `aria-describedby` untuk pesan error.
- [x] Antarmuka responsif mendukung viewport layar: Mobile (`375px`), Tablet (`768px`), dan Desktop (`1280px`).
- [x] Ukuran tap target tombol di mobile minimal `44 x 44 px` agar mudah ditekan jari.

---

## 7. Lembar Persetujuan Pembekuan Desain (Design Freeze Sign-Off)

Dengan ditandatanganinya lembar ini, Pihak Klien menyatakan telah meninjau dan menyetujui seluruh tata letak visual, arsitektur navigasi, dan alur prototipe interaktif pada dokumen ini.

**Klausul Pembekuan Desain**:
1. Seluruh rancangan visual resmi berstatus **FROZEN (DIBEKUKAN)**.
2. Tahap pengerjaan berikutnya akan langsung masuk ke spesifikasi arsitektur teknis dan pengkodean.
3. Segala perubahan struktur tata letak, penambahan halaman baru, atau perombakan alur antarmuka setelah tanggal persetujuan ini akan dikenakan biaya dan waktu tambahan melalui prosedur *Change Request (CR)*.

| Disetujui oleh Single PIC Klien | Divalidasi oleh Solo Engineer |
| :--- | :--- |
| **Nama**: _________________________ | **Nama**: _________________________ |
| **Jabatan**: ______________________ | **Jabatan**: Independent Lead Engineer |
| **Tanggal**: ______________________ | **Tanggal**: ______________________ |
| **Tanda Tangan**: | **Tanda Tangan**: |
