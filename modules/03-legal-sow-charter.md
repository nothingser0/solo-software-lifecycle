# Modul 03: [GATE KOMERSIAL] Legal SOW, DP, & Single PIC Agreement

Modul ini adalah **GERBANG KOMERSIAL PEMBLOKIR (BLOCKING GATE)** dalam siklus hidup proyek solo developer. Aturan fundamental: **TIDAK ADA SATU BARIS KODE ATAU DESAIN DETAIL YANG DIKERJAKAN SEBELUM GERBANG INI LOLOS.**

Tujuannya adalah mengikat dokumen `SCOPE_STATEMENT.md` ke dalam perjanjian legal berkekuatan hukum, mengamankan uang muka (Down Payment), mengunci Single PIC dari pihak klien, dan menetapkan protokol Change Request.

---

## 1. Siklus Eksekusi Modul 03

```text
[ INPUT: Dokumen SCOPE_STATEMENT.md dari Modul 02 ]
                         │
                         ▼
[ LANGKAH 1: Penentuan Model Kontrak & Estimasi Komersial ]
  • Fixed-Price Milestone (Skala Kecil & Menengah)
  • Time & Materials / Retainer Bulanan (Skala Besar & Fleksibel)
                         │
                         ▼
[ LANGKAH 2: Penguncian Struktur Termin Pembayaran (Payment Milestones) ]
  • Termin 1 (DP 30–50%): Prasyarat memulai riset teknis & UI/UX
  • Termin Antara (Alpha/Beta): Terikat pada verifikasi deliverable
  • Termin Akhir (Pelunasan 100%): Prasyarat penyerahan repo & BAST
                         │
                         ▼
[ LANGKAH 3: Pengikatan Klausul Single PIC & SLA Respon ]
  • 1 Pengambil Keputusan Mutlak dari Pihak Klien
  • SLA Review Klien Maksimal 3 Hari Kerja (Keterlambatan = Geser Jadwal)
                         │
                         ▼
[ LANGKAH 4: Penetapan Klausul Proteksi Hukum Solo Developer ]
  • Batasan Tanggung Jawab (Liability Cap = Maksimal Nilai Kontrak)
  • Kepemilikan Source Code (IP ditahan sampai lunas 100%)
  • Protokol Perubahan Fitur (Change Request / CR Resmi)
                         │
                         ▼
[ OUTPUT: Dokumen SOW_CONTRACT.md & PROJECT_CHARTER.md ]
                         │
          ┌──────────────┴──────────────┐
          ▼                             ▼
    [ DP BELUM DITERIMA ]         [ DP SUDAH DITERIMA & KONTRAK SAH ]
    • JANGAN MULAI KODING         • Lolos Gate Komersial
    • Status: On-Hold             • Lanjut ke Modul 04: UI/UX Design
```

---

## 2. Langkah demi Langkah Eksekusi

### Langkah 1: Memilih Model Kontrak yang Tepat
1. **Fixed-Price (Harga Tetap Berbasis Milestone)**:
   - *Kapan Digunakan*: Lingkup di `SCOPE_STATEMENT.md` sudah sangat jelas dan klien tidak fleksibel terhadap anggaran.
   - *Kunci Solo Dev*: Wajib tambahkan buffer biaya 20–30% untuk mitigasi revisi wajar.
2. **Time & Materials / Monthly Retainer**:
   - *Kapan Digunakan*: Klien memiliki roadmap dinamis (*"fitur dipikirkan sambil jalan"*) atau proyek berskala Besar/Enterprise yang membutuhkan riset berkelanjutan.
   - *Kunci Solo Dev*: Tagihan per bulan atau per blok 40 jam kerja dengan pembayaran di muka setiap awal periode.

---

### Langkah 2: Menetapkan Struktur Termin Pembayaran Bertahap
Sebagai solo developer, jangan pernah menerima pembayaran di akhir proyek (100% saat selesai). Skema termin baku:

| Termin | Milestone / Kondisi Pembayaran | Persentase | Prasyarat Deliverable |
| :---: | :--- | :---: | :--- |
| **Termin 1 (DP)** | Tanda Tangan Kontrak & Inisiasi Proyek | **30% – 50%** | Penyerahan SOW & Project Charter yang disepakati |
| **Termin 2 (Alpha)**| Core Engine & Integrasi Database Selesai | **25% – 30%** | Demo fungsionalitas backend & UI dasar di lokal/staging |
| **Termin 3 (Beta)** | Integrasi Lengkap & Lolos UAT Internal | **20% – 25%** | Aplikasi siap diuji klien di Staging (SIT Pass) |
| **Termin 4 (Final)**| Go-Live Production & Serah Terima Resmi | **10% – 15%** | UAT Sign-off Klien disetujui, siap penyerahan BAST |

---

### Langkah 3: Menegakkan Aturan Single PIC
Klien korporasi sering memiliki banyak kepala yang saling bertolak belakang arahannya.
- Wajib cantumkan nama, jabatan, email, dan nomor telepon **1 orang PIC Klien**.
- Seluruh instruksi, persetujuan desain, hasil uji UAT, dan penandatanganan dokumen hanya sah jika ditandatangani oleh PIC tersebut.
- Masukkan klausul: *"Instruksi lisan atau permintaan tertulis dari staf klien di luar PIC yang ditunjuk tidak memiliki kekuatan mengikat developer."*

---

### Langkah 4: Mengunci Klausul Proteksi Hukum Vital Solo Dev
1. **Hak Kekayaan Intelektual (Intellectual Property / IP)**:
   - Source code, kredensial server, dan lisensi software sepenuhnya tetap menjadi hak milik intelektual Developer sampai seluruh pembayaran termin (100%) lunas.
2. **Batasan Ganti Rugi (Liability Cap)**:
   - Developer tidak bertanggung jawab atas kerugian tidak langsung, hilangnya keuntungan bisnis, atau kebocoran data akibat kelalaian penyimpanan password oleh karyawan klien.
   - Total liabilitas finansial maksimum developer dalam kondisi apapun dibatasi maksimal sebesar total nilai kontrak yang telah dibayarkan oleh klien.
3. **Mekanisme Change Request (CR)**:
   - Setiap fitur tambahan di luar `SCOPE_STATEMENT.md` wajib dituangkan ke lembar CR dengan formula: `Biaya Tambahan = Jam Estimasi x Tarif Per Jam` dan `Jadwal Rilis Bertambah X Hari`.

---

## 3. Adaptasi Berdasarkan Skala Proyek

| Aspek | Skala Kecil (MVP / Freelance) | Skala Menengah (B2B SaaS / Agensi) | Skala Besar & Enterprise |
| :--- | :--- | :--- | :--- |
| **Format Kontrak** | Invoice DP 50% + Scope Statement via Email | Dokumen SOW & Perjanjian Kerja Sama (PKS) | Master Service Agreement (MSA) + SOW formal |
| **Legalitas** | Tanda tangan elektronik (PDF signature) | Tanda tangan basah bermeterai / e-Meterai | Legal review dari tim hukum korporasi klien |
| **Termin DP** | Wajib 50% di muka | Minimal 30–40% di muka | Minimal 20–30% di muka (sesuai SOP korporat) |
| **Klausul NDA** | Cukup klausul kerahasiaan di dalam SOW | Non-Disclosure Agreement (NDA) standar | Mutual NDA formal + klausul UU PDP ketat |

---

## 4. Artefak Keluaran (Deliverables)

1. **`PROJECT_CHARTER.md`**: Dokumen piagam proyek untuk mengunci objektif, penunjukan Single PIC, dan timeline global (menggunakan template `templates/commercial/PROJECT_CHARTER_TEMPLATE.md`).
2. **`SOW_CONTRACT.md`**: Dokumen kontrak kerja komersial yang mengikat lingkup, biaya, termin pembayaran, dan klausul hukum (menggunakan template `templates/commercial/SOW_CONTRACT_TEMPLATE.md`).

---

## 5. Kriteria Kelulusan Gerbang (Gate Exit Criteria)

Gerbang ini dinyatakan **LOLOS (PASS)** jika dan hanya jika:
- [x] Kontrak SOW telah ditandatangani oleh Klien dan Developer.
- [x] Single PIC Klien telah ditunjuk secara resmi.
- [x] **Dana Pembayaran DP (Termin 1) telah masuk dan terkonfirmasi di rekening bank Developer.**

*Jika ketiga syarat di atas terpenuhi, sistem resmi melangkah ke **Modul 04: UI/UX Design & Prototyping**.*
