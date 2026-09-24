# Modul 01: Idea & Feasibility (Penyaringan Ide & Uji Kelayakan)

Modul ini adalah gerbang pertama dalam siklus pengembangan perangkat lunak untuk solo developer. Tujuannya adalah mengubah ide mentah yang abstrak menjadi **Ringkasan Ide Teruji (Validated Idea Brief)** dengan batasan skala yang jelas sebelum waktu terbuang untuk menulis dokumen panjang atau koding.

---

## 1. Siklus Eksekusi Modul 01

```text
[ IDE KASAR / MENTAH ]
          │
          ▼
[ LANGKAH 1: Saringan 3 Lapis (The 3-Filter Triage) ]
  • Masalah Riil & Nilai Unik
  • Core User Loop (Alur Utama Inti)
  • MVP Razor (Pemotongan Fitur Ekstrem)
          │
          ▼
[ LANGKAH 2: Uji 4 Dimensi Kelayakan (Feasibility Check) ]
  • Teknis (Tech Stack & Kesiapan API)
  • Bandwidth Solo Dev (Batas Waktu & Maintenance)
  • Regulasi & Legal (Izin Usaha, UU PDP, Liabilitas)
  • Ekonomi & Nilai Bisnis (Willingness to Pay / ROI)
          │
          ▼
[ LANGKAH 3: Determinasi Skala Proyek (Scale Classification) ]
  • Kecil (MVP / Freelance)
  • Menengah (B2B SaaS / Agensi)
  • Besar (Scale-Up / Multi-System)
  • Enterprise (Korporasi / Regulasi Ketat)
          │
          ▼
[ OUTPUT: Dokumen IDEA_BRIEF.md ] ──► Siap Lanjut ke Modul 02: Discovery & Scope
```

---

## 2. Langkah demi Langkah Eksekusi

### Langkah 1: Saringan 3 Lapis (The 3-Filter Triage)

Lakukan interogasi terarah terhadap ide mentah:

1. **Saringan Masalah (Problem Statement)**:
   - *Pertanyaan*: Siapa yang punya masalah ini, seberapa sering masalah ini terjadi, dan bagaimana mereka mengatasinya sekarang (manual, spreadsheet, jasa orang lain)?
   - *Prinsip*: Jangan membangun software untuk masalah yang cukup diselesaikan dengan Google Sheet atau form sederhana, kecuali ada kebutuhan otomasi/keamanan data khusus.
2. **Saringan Alur Utama (Core User Loop)**:
   - *Pertanyaan*: Apa alur 3 langkah dari interaksi pengguna?
   - *Format Baku*: `[User Input Data] ──► [Sistem Melakukan Proses/Transformasi] ──► [User Menerima Hasil/Value]`.
3. **Saringan Pemotongan Ekstrem (MVP Razor)**:
   - *Pertanyaan*: Jika aplikasi ini hanya boleh memiliki SATU fitur utama saat peluncuran pertama, fitur apa yang membuat pengguna tetap mau memakai aplikasi ini?
   - *Tindakan*: Singkirkan fitur sekunder (social login, dark mode, grafik analitik rumit, integrasi multi-gateway) ke daftar *Backlog Masa Depan*.

---

### Langkah 2: Uji 4 Dimensi Kelayakan (Feasibility Rubric)

Evaluasi kelayakan ide menggunakan skor 1–5 pada 4 dimensi:

| Dimensi Kelayakan | Pertanyaan Uji Kritis Solo Dev | Batas Minimum Lolos |
| :--- | :--- | :--- |
| **1. Kelayakan Teknis** | Apakah pustaka, SDK, dan API yang dibutuhkan sudah matang dan terdokumentasi? Apakah membutuhkan riset R&D komputasi berat? | Skor $\ge 3$ (Jika butuh R&D berat sendirian, simplifikasi ide) |
| **2. Kelayakan Bandwidth** | Apakah aplikasi bisa diselesaikan dalam rentang waktu solo dev (maks. 1–3 bulan untuk rilis pertama)? Apakah biaya operasional hariannya rendah? | Skor $\ge 4$ (Hindari arsitektur multi-service yang butuh on-call 24/7) |
| **3. Kelayakan Regulasi & Legal** | Apakah pengoperasian sistem melanggar hukum, membutuhkan izin khusus (OJK, Kominfo, Kemenkes), atau memegang data pribadi sensitif (UU PDP)? | Skor $\ge 4$ (Jika ada risiko pidana/denda tanpa modal hukum, pivot/scope down) |
| **4. Kelayakan Komersial** | Apakah ada pihak yang bersedia membayar untuk sistem ini (B2B/B2C)? Jika pesanan klien, apakah budget realistis terhadap effort? | Skor $\ge 3$ (Harus ada kejelasan sumber pendapatan atau margin yang layak) |

*Lihat panduan lengkap di: `references/FEASIBILITY_CRITERIA.md`.*

---

### Langkah 3: Klasifikasi Skala Proyek (Scale Triage)

Tentukan kategori proyek sejak awal untuk menentukan seberapa berat formalitas dokumen berikutnya:

1. **Skala Kecil (MVP / Freelance Tool)**:
   - *Indikator*: Pengguna tunggal/tim kecil, 1–2 entitas data, waktu kerja < 1 bulan, tanpa integrasi sistem perbankan/regulasi.
   - *Arah Lanjutan*: Langsung susun 1-page Brief & Scope Statement sederhana, lewati charter formal.
2. **Skala Menengah (B2B SaaS / Agensi)**:
   - *Indikator*: Multi-tenant, ada pembayaran berlangganan, autentikasi berbasis peran (RBAC), integrasi 1–3 API pihak ketiga, waktu kerja 1–3 bulan.
   - *Arah Lanjutan*: Wajib menyusun PRD ringan, kontrak SOW resmi, dan arsitektur database modular.
3. **Skala Besar (Scale-Up / Platform Terdistribusi)**:
   - *Indikator*: Volume transaksi tinggi, concurrency tinggi, integrasi multi-sistem perusahaan, waktu kerja 3–6 bulan.
   - *Arah Lanjutan*: Wajib menyusun Project Charter, PRD formal, FSD mendalam, dan WBS terperinci.
4. **Skala Enterprise / Industri (Korporasi, Perbankan, BUMN)**:
   - *Indikator*: Kepatuhan regulasi ketat (UU PDP, ISO 27001, SOC2), multi-stakeholder internal klien, audit trail permanen, SLA uptime 99.9%.
   - *Arah Lanjutan*: Wajib ada persetujuan formal legal, Project Charter bertandatangan, single PIC terikat, FSD lengkap, dan RTM.

---

## 3. Artefak Keluaran (Deliverable)

Hasil akhir dari Modul 01 adalah berkas **`IDEA_BRIEF.md`** yang dibuat menggunakan template di `templates/ideation/IDEA_BRIEF_TEMPLATE.md`.

Dokumen ini menjadi prasyarat sebelum melangkah ke **Modul 02: Discovery & Scope Definition**.
