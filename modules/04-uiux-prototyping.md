# Modul 04: UI/UX Design & Prototyping (Google Stitch Universal Engine)

Modul ini adalah tahap keempat dalam siklus pengembangan perangkat lunak untuk solo developer. Tujuannya adalah menerjemahkan kebutuhan fungsional dari `SCOPE_STATEMENT.md` menjadi **Sistem Desain Anti-Slop**, **Layar UI Nyata Berbasis Kode**, dan **Prototipe Interaktif Layar Penuh** menggunakan **Google Stitch** sebagai alat tunggal untuk semua skala proyek (Kecil, Menengah, Besar, hingga Enterprise).

> ⛔ **ATURAN MUTLAK ANTI-SHORTCUT (NON-NEGOTIABLE FOR ALL PROJECTS)**:
> **DILARANG KERAS MENG-SKIP ATAU MENGGANTI GOOGLE STITCH DENGAN WIREFRAME TEKS/MARKDOWN**, baik pada proyek klien, produk mandiri (*solo dev product*), *internal tool*, maupun MVP.
>
> **Mengapa Ini Wajib dan Tidak Boleh Di-skip?**
> 1. **Koneksi Rantai Modul 06**: Google Stitch bukan sekadar untuk demo visual, melainkan menghasilkan **Screen ID dan kode markup/Tailwind nyata**. Pada Modul 06 (Development), agen AI (OpenCode) **WAJIB** memanggil tool MCP `stitch_get_screen` untuk menarik kode komponen tersebut ke dalam repositori.
> 2. **Pencegahan AI Slop**: Jika layar Stitch di-skip dan hanya diganti deskripsi teks / low-fi wireframe di markdown, pada Modul 06 agen AI akan mengarang styling CSS/HTML dari nol tanpa acuan baku, yang berujung pada tampilan berantakan (*AI Slop*) dan waktu pengerjaan 5x lebih lambat.
> 3. **Adaptasi untuk Solo Dev Product**: Jika proyek adalah produk mandiri tanpa klien eksternal, Anda sendiri yang bertindak sebagai approver pada lembar *Design Freeze*, namun **pembuatan layar di Google Stitch via `stitch_generate_screen_from_text` TETAP WAJIB DILAKUKAN**.

---

## 1. Siklus Eksekusi Modul 04

```text
[ INPUT: SCOPE_STATEMENT.md & Kontrak Sah + DP dari Modul 03 ]
                                │
                                ▼
[ LANGKAH 1: Penyusunan Guardrail DESIGN.md (Anti-Slop Token) ]
  • Palet Warna Netral (Zinc/Slate) + 1 Warna Brand Aksen
  • Tipografi Inter & JetBrains Mono, Border Flat 1px, Zero Gradient
                                │
                                ▼
[ LANGKAH 2: Inisiasi Project & Design System di Google Stitch ]
  • Buat Container Project (stitch_create_project)
  • Upload DESIGN.md (stitch_upload_design_md & stitch_create_design_system_from_design_md)
                                │
                                ▼
[ LANGKAH 3: Generasi Layar Berbasis Data Riil (stitch_generate_screen_from_text) ]
  • Generasi Setiap Halaman Utama (Login, Dashboard, Form, Detail)
  • Wajib Menyertakan 5 State: Default, Loading Skeleton, Empty, Error, Success
                                │
                                ▼
[ LANGKAH 4: Integrasi Navigasi & Live Staging Clickable Prototype ]
  • Hubungkan Tautan Antar-Layar (<a href="...">)
  • Deploy Instan ke Live Preview URL (Vercel / Cloudflare Pages / Stitch Viewer)
                                │
                                ▼
[ LANGKAH 5: Sesi Walk-Through & Pembekuan Desain (Design Freeze) ]
  • Demo Interaktif Bersama Single PIC Klien
  • Tanda Tangan Lembar Design Freeze Sign-Off
                                │
                                ▼
[ OUTPUT: 4 ARTEFAK LENGKAP ] ──► Siap Lanjut ke Modul 05: Arsitektur & FSD
```

---

## 2. Empat Artefak Keluaran (Deliverables) Modul 04

Modul ini menghasilkan 4 deliverable konkret:

| No | Nama Artefak | Format / Lokasi | Deskripsi & Fungsi |
| :---: | :--- | :--- | :--- |
| **1** | **`DESIGN.md`** | File Markdown di root proyek | Token desain dan aturan guardrail anti-slop yang diunggah ke Google Stitch sebagai acuan visual seluruh layar. |
| **2** | **`DESIGN_SPEC.md`** | File Markdown di root proyek | Ringkasan arsitektur informasi, sitemap rute URL, daftar Screen ID di Stitch, dan matriks 5 state layar. |
| **3** | **Interactive Prototype** | Tautan Live Staging Web / Stitch Viewer | Aplikasi antarmuka nyata yang bisa diklik tombolnya, diketik form-nya, dan diuji alur kerjanya oleh klien. |
| **4** | **Design Freeze Sign-Off** | Lembar bertandatangan di `DESIGN_SPEC.md` | Berita acara persetujuan tertulis dari Single PIC Klien yang mengunci struktur visual sebelum koding backend dimulai. |

---

## 3. Langkah demi Langkah Eksekusi

### Langkah 1: Merumuskan Guardrail `DESIGN.md`
Gunakan template di `templates/design/DESIGN_MD_TEMPLATE.md`:
- Kunci warna background: `#FFFFFF` (Light) atau `#09090B` (Dark).
- Kunci warna teks berdaya kontras tinggi: `#09090B` (Rasio kontras $\ge 4.5:1$ WCAG AA).
- Batasi hanya ada **1 warna brand aksen** untuk tombol CTA dan link aktif.
- Larang dekorasi "AI Slop": gradien ungu, kartu mengambang (*floating cards*), dan drop-shadow tebal.

### Langkah 2: Registrasi ke Google Stitch via Tooling
1. Buat project container baru:
   `stitch_create_project(reason="Inisiasi UI prototype untuk [Nama Proyek]")`
2. Konversikan berkas `DESIGN.md` menjadi base64, lalu upload:
   `stitch_upload_design_md(projectId="...", designMdBase64="...")`
3. Terapkan design system tersebut ke project:
   `stitch_create_design_system_from_design_md(projectId="...", ...)`

### Langkah 3: Generasi Seluruh Layar Tanpa Terkecuali (100% Exhaustive Coverage)
Panggil `stitch_generate_screen_from_text` untuk **SETIAP halaman** yang tertera di `SCOPE_STATEMENT.md` dari awal hingga akhir:
- **DILARANG KERAS MEMANGKAS CAKUPAN LAYAR**: Jika proyek memiliki 10, 25, 50, atau 100 halaman dalam lingkupnya, **seluruh halaman tersebut WAJIB di-generate di Google Stitch**. Dilarang keras hanya men-generate 3–4 layar contoh sebagai sampel.
- Sertakan konteks data bisnis nyata Indonesia (format rupiah, istilah hukum/bisnis, nama kota).
- Wajib meminta state defensif: minta layar *Empty State* dan *Loading Skeleton*.
- Catat `screen_id` dari **setiap layar** yang berhasil di-generate ke dalam tabel inventaris di `DESIGN_SPEC.md`.

### Langkah 4: Merakit Clickable Demo Lengkap
1. Ambil kode HTML/CSS komponen dari Stitch untuk seluruh layar.
2. Pasang tag hyperlink routing standar untuk menghubungkan alur tombol:
   - Tombol "Login" $\to$ mengarahkan ke `/dashboard`
   - Tombol "Buat Dokumen Baru" $\to$ mengarahkan ke `/documents/new`
   - Tombol "Simpan Draf" $\to$ menampilkan modal/toast sukses dan mengarahkan ke `/documents/:id`
3. Deploy kode ke staging URL gratis (Vercel / Cloudflare Pages) agar dapat dibuka langsung oleh klien di HP maupun laptop untuk menguji alur utuh 100%.

### Langkah 5: Walk-Through & Pembekuan Desain (Design Freeze)
1. Jadwalkan demo bersama **Single PIC Klien** (atau self-review untuk solo product).
2. Biarkan klien mencoba mengklik dan mengetik form di live demo untuk seluruh layar.
3. Kunci persetujuan tertulis: *Tata letak visual dan alur navigasi resmi DIBEKUKAN (FROZEN). Perubahan layout di kemudian hari masuk skema Change Request (CR).*

---

## 4. Adaptasi Berdasarkan Skala Proyek

| Parameter | Skala Kecil (MVP / Freelance) | Skala Menengah (B2B SaaS / Agensi) | Skala Besar & Enterprise |
| :--- | :--- | :--- | :--- |
| **Cakupan Layar** | **100% seluruh halaman** dalam Scope (tanpa pengurangan) | **100% seluruh halaman** dalam Scope (tanpa pengurangan) | **100% seluruh halaman** dalam Scope (tanpa pengurangan) |
| **Media Demo** | Tautan Viewer Stitch / Live Preview | Live Staging Web (Vercel/Cloudflare) | Live Staging Web + Dokumen Audit Aksesibilitas |
| **Kepatuhan Desain** | Kontras visual standar $\ge 4.5:1$ | WCAG AA terverifikasi pada form | Full WCAG AA audit (Keyboard nav, Screen reader) |
| **Approval** | Konfirmasi tertulis email/chat | Tanda tangan lembar Design Freeze | Formal Design Sign-Off & Berita Acara Review UI |

---

## 5. Kriteria Kelulusan Gerbang (Gate Exit Criteria)

Gerbang Modul 04 dinyatakan **LOLOS (PASS)** jika:
- [x] Dokumen `DESIGN.md` telah diunggah dan aktif sebagai Design System di Stitch.
- [x] Seluruh layar utama telah di-generate dengan data riil dan 5 state lengkap.
- [x] Tautan demo interaktif (Clickable Prototype) dapat diklik tanpa dead-end.
- [x] **Single PIC Klien telah menandatangani persetujuan Design Freeze.**

*Jika seluruh kriteria terpenuhi, sistem resmi melangkah ke **Modul 05: Arsitektur & Spesifikasi Teknis (PRD & FSD)**.*
