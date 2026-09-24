# Modul 08: Data Migration & Seeding (Migrasi Data Warisan & Penyemaian Data)

Modul ini adalah tahap kedelapan dalam siklus hidup proyek perangkat lunak untuk solo developer. Tujuannya adalah memindahkan data warisan (*legacy data*) milik klien (dari Excel, CSV, sistem lama, atau database usang) ke dalam skema database baru secara otomatis, terenkripsi, dan tervalidasi sebelum sesi pengujian pengguna (UAT) di Modul 09 dimulai.

---

## 1. Siklus Eksekusi Modul 08

```text
[ INPUT: Data Mentah Klien (CSV/Excel/SQL) & Skema Database FSD.md ]
                                    │
                                    ▼
[ LANGKAH 1: Audit Data Sumber & Penetapan Batas Tanggung Jawab ]
  • Aturan Clean-In / Clean-Out: Klien bertanggung jawab membersihkan data (Data Hygiene)
  • Developer HANYA menulis skrip transformasi otomatis (ETL)
                                    │
                                    ▼
[ LANGKAH 2: Penyusunan Matriks Pemetaan Kolom (Data Mapping) ]
  • Pemetaan Kolom Sumber ──► Kolom Database Baru (Tipe Data, Default Values)
  • Normalisasi Relasi Entitas & Transformasi Format (Tanggal, Rupiah, Enum)
                                    │
                                    ▼
[ LANGKAH 3: Masking & Sanitasi Data Sensitif Staging (UU PDP) ]
  • Masking Data PII (NIK, Nomor Rekening, Password Lama) di Server Staging
  • Pencegahan Kebocoran Data Pribadi Nyata ke Lingkungan Non-Produksi
                                    │
                                    ▼
[ LANGKAH 4: Eksekusi Skrip ETL Berbasis Transaksi Atomik (Batch Load) ]
  • Ekstraksi (Parse file) ──► Transformasi (Zod Validation) ──► Load (Batch Insert)
  • Pembungkusan dalam Blok Transaksi Database (Rollback Otomatis jika Error)
                                    │
                                    ▼
[ LANGKAH 5: Rekonsiliasi Data & Pengesahan Klien (Data Sign-Off) ]
  • Perhitungan Baris Sumber vs Baris Terimpor (100% Match)
  • Penyusunan Dokumen MIGRATION_RECONCILIATION_REPORT.md
  • Single PIC Klien Menandatangani Persetujuan Data
                                    │
                                    ▼
[ OUTPUT: Database Staging Terisi Data Nyata & RECONCILIATION_REPORT.md ] ──► Siap UAT di Modul 09
```

---

## 2. Prinsip Migrasi Solo Developer: "Clean-In, Clean-Out"

Salah satu jebakan terbesar yang menghabiskan waktu solo developer tanpa dibayar adalah **"Membersihkan Data Rusak Klien Secara Manual"**.

### Aturan Baku Perlindungan Solo Dev:
1. **Klien Pemilik Kebersihan Data (Client Owns Data Hygiene)**:
   - Klien wajib menyerahkan data yang sudah bersih dari baris ganda yang tidak valid, format sel yang rusak, atau data tanpa identitas.
   - Jika klien meminta developer merapikan ribuan baris spreadsheet manual, pekerjaan tersebut **WAJIB dimasukkan ke dalam Change Request (CR) jasa konsultasi data terpisah**.
2. **Validasi Skema Tanpa Pengecualian**:
   - Skrip migrasi wajib memvalidasi setiap baris menggunakan skema Zod/Pydantic.
   - Jika ada baris yang korup, skrip otomatis membuangnya ke file penampung `rejected-rows.csv` dengan keterangan alasan galat (*error reason*), tanpa mematikan seluruh proses.
3. **Kepatuhan Privasi Data (UU PDP No. 27/2022)**:
   - Di lingkungan **Staging**, data pribadi sensitif (NIK, nomor telepon pribadi, kata sandi lama) wajib disamarkan (*masked* / *faked*). Data asli hanya dimasukkan saat migrasi final di server Produksi (Modul 10).

---

## 3. Langkah demi Langkah Eksekusi

### Langkah 1: Audit Data Sumber
1. Minta klien menyerahkan data dalam format digital terstruktur (CSV, Excel `.xlsx`, atau SQL Dump).
2. Periksa konsistensi tipe data:
   - Format tanggal (apakah `YYYY-MM-DD`, `DD/MM/YYYY`, atau teks acak).
   - Format angka finansial (apakah mengandung karakter `Rp`, titik, atau koma).
   - Integritas ID relasional (apakah foreign key mengarah ke data yang benar-benar ada).

### Langkah 2: Penyusunan Dokumen Rencana Migrasi (Mapping Matrix)
Susun tabel pemetaan dari format lama ke skema FSD baru:
- Contoh: Kolom Excel `"Nama Lengkap"` $\to$ Kolom SQL `users.full_name` (`VARCHAR(150)`).
- Contoh: Kolom Excel `"Tgl Lahir"` $\to$ Transformasi `new Date(row.tgl)` $\to$ `users.birth_date` (`DATE`).

### Langkah 3: Penulisan Skrip Otomasi ETL (Batch Loading)
Tuliskan skrip eksekusi mandiri (misal: `scripts/migrate-data.ts` atau script Python):
1. **Extract**: Baca berkas sumber menggunakan stream parser (`csv-parse` atau `exceljs`).
2. **Transform**: Validasi tiap baris dengan Zod. Generate UUIDv7 untuk primary key baru. Hash kata sandi sementara menggunakan Argon2id.
3. **Load**: Masukkan data ke database menggunakan operasi *Batch Insert* (`createMany` atau SQL `COPY`) per blok 500–1.000 baris di dalam transaksi atomik (`db.$transaction`).

### Langkah 4: Rekonsiliasi & Penanganan Baris Ditolak
1. Skrip menghitung:
   - Total baris di berkas sumber: $N_{\text{source}}$
   - Total baris berhasil diimpor: $N_{\text{imported}}$
   - Total baris gagal/korup: $N_{\text{rejected}}$
2. Seluruh baris yang gagal otomatis diekspor ke `rejected-rows.csv` lengkap dengan nomor baris dan pesan error validasinya.
3. Berikan berkas `rejected-rows.csv` kepada Klien untuk diperbaiki oleh tim operasional mereka.

### Langkah 5: Pengesahan Data Bersama Klien
1. Tampilkan dashboard Staging yang kini sudah menampilkan data riil milik klien (bukan data dummy "Lorem Ipsum").
2. Kirim berkas **`MIGRATION_RECONCILIATION_REPORT.md`** kepada Single PIC Klien.
3. Klien menandatangani lembar persetujuan data (*Data Sign-Off*).

---

## 4. Adaptasi Berdasarkan Skala Proyek

| Aspek Migrasi Data | Skala Kecil (MVP / Freelance) | Skala Menengah (B2B SaaS / Agensi) | Skala Besar & Enterprise |
| :--- | :--- | :--- | :--- |
| **Volume Data** | $< 1.000$ baris data | $1.000 – 100.000$ baris data | $> 100.000$ baris data / Multi-database |
| **Format Sumber** | File Excel tunggal / CSV | Beberapa file Excel + Database MySQL lama | Database Oracle/SAP, legacy API, data terdistribusi |
| **Metode Eksekusi** | Skrip TypeScript sederhana satu kali jalan | Skrip ETL terstruktur dengan batching & logging | ETL Pipeline modular, rollback plan bertingkat |
| **Sanitasi PII Staging** | Ganti nama & email generik | Skrip masking otomatis NIK & telepon | Data Anonymization Engine sesuai audit ISO/PDP |
| **Formalitas Sign-Off** | Konfirmasi via chat/email tertulis | Lembar `RECONCILIATION_REPORT.md` signed | Berita Acara Migrasi Data Resmi bermeterai |

---

## 5. Artefak Keluaran (Deliverables)

Modul ini menghasilkan 3 artefak utama:
1. **`DATA_MIGRATION_PLAN.md`**: Dokumen pemetaan kolom sumber ke target, aturan transformasi, dan batas kepemilikan data (menggunakan `templates/migration/DATA_MIGRATION_PLAN_TEMPLATE.md`).
2. **`MIGRATION_RECONCILIATION_REPORT.md`**: Laporan bukti perbandingan jumlah data sumber vs target, daftar baris ditolak, dan lembar persetujuan PIC Klien (menggunakan `templates/migration/RECONCILIATION_REPORT_TEMPLATE.md`).
3. **Database Staging Terisi Data Riil**: Basis data di server Staging yang telah siap digunakan untuk sesi pengujian UAT.

---

## 6. Kriteria Kelulusan Gerbang (Gate Exit Criteria)

Gerbang Modul 08 dinyatakan **LOLOS (PASS)** jika:
- [x] Dokumen pemetaan kolom (`DATA_MIGRATION_PLAN.md`) telah disepakati.
- [x] Skrip ETL berhasil mengimpor seluruh data valid tanpa memicu integritas foreign key error.
- [x] Data sensitif di server Staging telah disanitasi/disamarkan sesuai UU PDP.
- [x] Seluruh baris gagal telah diekspor ke `rejected-rows.csv` dan diserahkan ke klien.
- [x] **Single PIC Klien telah menandatangani lembar pengesahan rekonsiliasi data.**

*Jika seluruh kriteria terpenuhi, sistem resmi membuka gerbang pengujian penerimaan pengguna: **Modul 09: [GATE VALIDASI] UAT & Sign-Off Klien di Staging**.*
