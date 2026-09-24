# Panduan Migrasi Data & Seeding Solo Developer

Dokumen ini adalah pedoman taktis bagi solo developer dalam mengeksekusi pemindahan data warisan (*legacy data*) milik klien dari format spreadsheet/CSV/SQL usang ke dalam basis data modern tanpa terjebak lembur merapikan data kotor secara manual.

---

## 1. Menghindari Perangkap "Spreadsheet Hell"

Klien sering menganggap data mereka "sudah rapi", padahal di dalamnya terdapat:
- Sel Excel yang di-*merge* (*merged cells*).
- Format tanggal acak (sebagian `12/04/2024`, sebagian `4 Desember 2024`, sebagian kosong).
- Nilai angka tercampur huruf (`"Rp 15.000.000 (belum diskon)"`).
- Data ganda dengan ejaan nama berbeda (*"PT Sinar Maju"* vs *"PT. Sinar Maju, Tbk"*).

### Aturan Komunikasi Pertahanan:
> *"Pak/Bu, skrip migrasi kami bekerja secara otomatis membaca data terstruktur. Kolom tanggal wajib berformat YYYY-MM-DD dan nominal wajib angka murni. Baris yang formatnya rusak akan otomatis dipisahkan ke berkas `rejected-rows.csv` agar dapat dilengkapi oleh staf operasional Bapak/Ibu."*

---

## 2. Pola Arsitektur Skrip ETL (Extract, Transform, Load)

Gunakan pola skrip mandiri TypeScript/Node.js dengan modul `csv-parse` dan `zod`:

```typescript
import fs from "node:fs";
import { parse } from "csv-parse/sync";
import { z } from "zod";
import { db } from "@/lib/db";

// 1. Skema Validasi Baris Data
const LegacyRowSchema = z.object({
  Nama: z.string().min(1),
  Email: z.string().email(),
  Nominal: z.string().transform((val) => Number(val.replace(/[^0-9]/g, ""))),
});

async function runMigration() {
  const fileContent = fs.readFileSync("legacy-data.csv", "utf-8");
  const rawRecords = parse(fileContent, { columns: true, skip_empty_lines: true });

  const validBatch: any[] = [];
  const rejectedRows: any[] = [];

  // 2. Tahap Transform & Karantina (Quarantine Pattern)
  for (const [index, raw] of rawRecords.entries()) {
    const parsed = LegacyRowSchema.safeParse(raw);
    if (!parsed.success) {
      rejectedRows.push({
        rowNumber: index + 2, // Baris ke-1 adalah header
        data: JSON.stringify(raw),
        error: parsed.error.issues.map((i) => i.message).join("; "),
      });
      continue;
    }

    validBatch.push({
      fullName: parsed.data.Nama.trim(),
      email: parsed.data.Email.toLowerCase().trim(),
      amount: parsed.data.Nominal,
    });
  }

  // 3. Simpan Baris Gagal ke CSV Karantina
  if (rejectedRows.length > 0) {
    fs.writeFileSync("rejected-rows.csv", JSON.stringify(rejectedRows, null, 2));
    console.log(`Karantina: ${rejectedRows.length} baris gagal divalidasi.`);
  }

  // 4. Tahap Load: Batch Insert dalam Transaksi Atomik
  const CHUNK_SIZE = 500;
  for (let i = 0; i < validBatch.length; i += CHUNK_SIZE) {
    const chunk = validBatch.slice(i, i + CHUNK_SIZE);
    await db.$transaction(async (tx) => {
      await tx.user.createMany({ data: chunk, skipDuplicates: true });
    });
  }

  console.log(`Migrasi Sukses: ${validBatch.length} baris berhasil terimpor.`);
}
```

---

## 3. Protokol Masking Data Pribadi Staging (UU PDP No. 27/2022)

Dilarang keras menyalin database produksi yang memuat data NIK KTP atau nomor rekening asli ke server Staging. Lingkungan Staging sering diakses oleh penguji eksternal atau developer lepas.

### Aturan Masking Otomatis:
- **NIK KTP**: Pertahankan 6 digit pertama (kode wilayah) dan 2 digit terakhir, samarkan sisanya: `357801********01`.
- **Email**: Ganti domain menjadi `@staging.local` atau gunakan alias generik: `user_001@staging.local`.
- **Kata Sandi**: Timpa seluruh kata sandi lama dengan hash satu arah default (misal: `StagingPassword123!`) agar tester dapat login saat pengujian UAT tanpa mengetahui kata sandi asli pengguna lama.

---

## 4. Taktik Pengesahan Data Bersama Klien (Data Sign-Off)

1. **Jangan Beri Data Mentah ke Klien**:
   Tunjukkan hasil migrasi langsung di antarmuka web Staging. Saat klien melihat dashboard dengan nama pelanggan dan dokumen asli mereka, tingkat kepercayaan klien akan melonjak drastis.
2. **Kunci Persetujuan Tertulis**:
   Kirimkan lembar **`MIGRATION_RECONCILIATION_REPORT.md`** yang memuat:
   - Jumlah baris sumber vs baris terimpor.
   - Lampiran berkas `rejected-rows.csv`.
   - Pernyataan bahwa klien menyetujui data hasil impor sudah akurat untuk digunakan pada sesi **UAT (Modul 09)**.
