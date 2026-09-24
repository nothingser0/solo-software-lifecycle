# Modul 04: UI/UX Design & Prototyping (Google Stitch Universal Engine)

Modul ini adalah tahap keempat dalam siklus pengembangan perangkat lunak untuk solo developer. Tujuannya adalah menerjemahkan kebutuhan fungsional dari `SCOPE_STATEMENT.md` menjadi **Sistem Desain Anti-Slop**, **Layar UI Nyata Berbasis Kode**, dan **Prototipe Interaktif Layar Penuh** menggunakan **Google Stitch** sebagai alat tunggal untuk semua skala proyek (Kecil, Menengah, Besar, hingga Enterprise).

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

### Langkah 3: Generasi Layar Berbasis Data Riil (No Lorem Ipsum)
Panggil `stitch_generate_screen_from_text` untuk setiap halaman yang tertera di `SCOPE_STATEMENT.md`:
- Sertakan konteks data bisnis nyata Indonesia (format rupiah, istilah hukum/bisnis, nama kota).
- Wajib meminta state defensif: minta layar *Empty State* dan *Loading Skeleton*.
- Catat `screen_id` dari setiap layar yang berhasil di-generate.

### Langkah 4: Merakit Clickable Demo
1. Ambil kode HTML/CSS komponen dari Stitch.
2. Pasang tag hyperlink routing standar untuk menghubungkan alur tombol:
   - Tombol "Login" $\to$ mengarahkan ke `/dashboard`
   - Tombol "Buat Dokumen Baru" $\to$ mengarahkan ke `/documents/new`
   - Tombol "Simpan Draf" $\to$ menampilkan modal/toast sukses dan mengarahkan ke `/documents/:id`
3. Deploy kode ke staging URL gratis (Vercel / Cloudflare Pages) agar dapat dibuka langsung oleh klien di HP maupun laptop.

### Langkah 5: Walk-Through & Pembekuan Desain (Design Freeze)
1. Jadwalkan demo 30 menit bersama **Single PIC Klien**.
2. Biarkan klien mencoba mengklik dan mengetik form di live demo.
3. Kunci persetujuan tertulis: *Tata letak visual dan alur navigasi resmi DIBEKUKAN (FROZEN). Perubahan layout di kemudian hari masuk skema Change Request (CR).*

---

## 4. Adaptasi Berdasarkan Skala Proyek

| Parameter | Skala Kecil (MVP / Freelance) | Skala Menengah (B2B SaaS / Agensi) | Skala Besar & Enterprise |
| :--- | :--- | :--- | :--- |
| **Jumlah Layar** | 3–5 layar inti (Happy path + Empty) | 8–15 layar mencakup seluruh alur | 20+ layar mencakup seluruh peran pengguna |
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
