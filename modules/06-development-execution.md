# Modul 06: Development (Backend, Frontend, Integrasi API, & 3 Pilar Rekayasa)

Modul ini adalah tahap keenam dalam siklus hidup proyek perangkat lunak untuk solo developer. Tujuannya adalah mengeksekusi penulisan kode nyata (*coding*) secara terarah menggunakan bantuan **AI Coding Agents (OpenChamber + OpenCode + OhMyOpenCode / Cursor / Claude Code)** melalui penyediaan **Agent Harness (berkas pemandu AI)**, mengintegrasikan antarmuka dari **Google Stitch**, mengelola percabangan **Git**, serta menegakkan 3 pilar rekayasa non-negosiasi: **Keamanan (Security)**, **Performa (Performance)**, dan **Efisiensi Sumber Daya (Resource Efficiency)**.

---

## 1. Siklus Eksekusi Modul 06 (The Agentic Vibe Coding Loop)

```text
[ INPUT: FSD.md, PRD.md, & Komponen UI Google Stitch dari Modul 04-05 ]
                                    │
                                    ▼
[ LANGKAH 1: Inisiasi Repositori & Pemasangan 7 Berkas Harness AI ]
  • Scaffold Clean Project (pnpm create next-app)
  • Pasang: AGENTS.md, CONTEXT.md, ARCHITECTURE.md, DESIGN.md, CONVENTIONS.md, .env.example, TODO.md
  • Git Init & Strategi Percabangan (main ──► staging ──► feat/*)
                                    │
                                    ▼
[ LANGKAH 2: Penerapan Komponen UI Google Stitch via MCP ]
  • OpenCode Panggil stitch_get_screen ──► Ekstrak ke src/components/
  • Pasang Rute Halaman (Dashboard, Forms, Detail, Sign Page)
  • Pastikan UI Visual 100% Identik dengan Prototipe Terkunci
                                    │
                                    ▼
[ LANGKAH 3: Pembangunan Basis Data & Migrasi (Dituntun AI) ]
  • AI Membaca ARCHITECTURE.md ──► Skema Prisma/Drizzle & Migrasi DDL
  • Pasang Database Pooling & Indexing pada Kolom Foreign Key
  • Eksekusi Migrasi Lokal & Penyuntikan Seed Data Riil
                                    │
                                    ▼
[ LANGKAH 4: Backend API, Layanan Vault, & 3 Pilar Rekayasa ]
  • AI Membaca TODO.md Sekuensial ──► Buat Handler API Ber-Zod
  • Pilar Security: Enkripsi Stream AES-256-GCM, HttpOnly Cookies, Parameterized Queries
  • Pilar Performance: N+1 Prevention, In-Memory Caching (Redis), Query Indexing
  • Pilar Resource: Zero Memory Leak, Stream Processing, Connection Pool Capping
  • Sambungkan Tombol UI Stitch ke Endpoint API (5 State Wajib Aktif)
                                    │
                                    ▼
[ LANGKAH 5: Uji Asersi Mandiri, Git Commit, & Pencapaian Termin ]
  • Jalankan Skrip Asersi `npm run test:smoke` (100% PASS)
  • Audit Kemanan Dependensi (`pnpm audit`) & TypeScript Check (`tsc --noEmit`)
  • Merge Fitur ke Branch `staging` & Tagih Milestone Termin 2 (Alpha)
                                    │
                                    ▼
[ OUTPUT: Repositori Kode Siap Pakai & RUNBOOK_LOCAL.md ] ──► Siap Masuk ke Modul 07: QA & SIT
```

---

## 2. Tujuh Berkas Kendali AI di Root Repo (The 7 Root Harness Files)

Sebelum memicu agen AI untuk menulis kode, letakkan 7 berkas kendali ini di root folder proyek:

| No | Nama Berkas | Sumber Rujukan | Fungsi untuk AI Coding Agent |
| :---: | :--- | :--- | :--- |
| **1** | **`AGENTS.md`** | `templates/dev/AGENTS_TEMPLATE.md` | Aturan main mutlak: larangan tipe `any`, perintah build/test, dan format commit. |
| **2** | **`CONTEXT.md`** | `templates/dev/CONTEXT_TEMPLATE.md` | Konteks bisnis, peran user (RBAC), dan daftar batas tegas *Out-of-Scope* agar AI tidak halusinasi. |
| **3** | **`ARCHITECTURE.md`** | `templates/dev/ARCHITECTURE_TEMPLATE.md` | Rangkuman FSD: struktur folder, skema tabel, dan kontrak rute API JSON. |
| **4** | **`DESIGN.md`** | `templates/design/DESIGN_MD_TEMPLATE.md` | Token visual dari Modul 04: palet Zinc, 1 aksen brand, font Inter, border 1px flat. |
| **5** | **`CONVENTIONS.md`** | `templates/dev/CONVENTIONS_TEMPLATE.md` | Aturan gaya koding: penamaan `kebab-case`, Server Components default, larangan barrel files. |
| **6** | **`.env.example`** | `templates/dev/ENV_EXAMPLE_TEMPLATE.md` | Kamus variabel lingkungan baku agar AI tidak mengarang nama key database/rahasia. |
| **7** | **`TODO.md`** | `templates/dev/TODO_TEMPLATE.md` | Daftar tugas atomik sekuensial yang dicentang `[x]` satu per satu oleh AI. |

---

## 3. Strategi Percabangan Git Solo Developer (Git Branching & Clean Production)

Solo developer tetap wajib menjaga kerapian branch agar rilis produksi bersih dari berkas draft/eksperimen:

```text
[ main ] ──────────────(Release Tag v1.0.0 - Production Clean)─────────────────►
   ▲
   │ (Merge setelah UAT Pass)
[ staging ] ───────────(Integration & Client Demo)─────────────────────────────►
   ▲
   │ (Merge setelah lolos tes lokal)
   ├── [ feat/auth-login ] ───────► (Selesai ──► Merge ke staging)
   ├── [ feat/document-vault ] ───► (Selesai ──► Merge ke staging)
   └── [ fix/pdf-render-bug ] ────► (Selesai ──► Merge ke staging)
```

### Aturan Baku Branching:
1. **Branch `main` (Production)**:
   - Kode produksi 100% stabil yang sudah lolos UAT klien.
   - Bersih dari berkas internal dev: berkas seperti `TODO.md` dan catatan draf internal tidak boleh mengotori branch produksi (diatur via `.gitignore` produksi atau build docker ignore).
   - Selalu diberi label SemVer: `git tag -a v1.0.0 -m "Release v1.0.0"`.
2. **Branch `staging` (Integration)**:
   - Wadah integrasi seluruh fitur yang siap diuji di server Staging. Klien menguji fitur di environment ini.
3. **Branch `feat/[nama-fitur]`**:
   - Cabang kerja solo dev untuk setiap item besar di `TODO.md`.
   - Setelah tugas selesai dan lulus `smoke-test` lokal, branch di-merge ke `staging`.
4. **Format Pesan Commit (Conventional Commits)**:
   - `feat(vault): implement streaming AES-256 encryption for PDF upload`
   - `fix(auth): correct Argon2id memory cost parameter`
   - `perf(db): add index on documents(creator_id, status)`

---

## 4. Enam Pilar Kualitas Rekayasa (The 6 Engineering Pillars)

Pengkodean bukan hanya tentang "fitur berjalan", melainkan wajib memenuhi 6 standar rekayasa:

### Pilar 1: Keamanan Defensif (Security by Design)
- **Zero Raw Queries**: 100% query SQL wajib melalui ORM/parameterized query untuk mencegah SQL Injection.
- **Validasi Ketat di Pintu Masuk**: Semua data request wajib melalui skema Zod.
- **Enkripsi Data Sensitif (UU PDP No. 27/2022)**: File dokumen dienkripsi AES-256-GCM sebelum masuk storage; password di-hash menggunakan Argon2id; token sesi disimpan di cookie `HttpOnly, Secure, SameSite=Strict`.
- **Audit Dependensi**: Jalankan `pnpm audit` secara berkala untuk memastikan tidak ada pustaka open-source yang memiliki celah keamanan (*vulnerability*).

### Pilar 2: Performa & Kecepatan (Performance Engineering)
- **Pencegahan N+1 Query**: Dilarang menjalankan query database di dalam perulangan loop. Gunakan `include`/`select` relasi atau batched query.
- **Database Indexing**: Pasang indeks pada setiap kolom Foreign Key dan kolom filter pencarian (`WHERE status = ...`).
- **Zero Layout Shift & Optimasi Aset**: Gunakan Next.js `<Image>` untuk kompresi WebP otomatis dan skeleton loader untuk mencegah pergeseran tampilan saat memuat data.
- **Caching**: Terapkan in-memory caching (Redis) untuk data master yang jarang berubah.

### Pilar 3: Efisiensi Sumber Daya & Biaya (Resource & Cost Efficiency)
- **Database Connection Pooling**: PostgreSQL memiliki batas koneksi terbatas. Selalu gunakan connection pooling (Prisma Accelerate, Supabase Pooler, atau PgBouncer) agar serverless functions tidak menenggelamkan database (*connection exhaustion*).
- **Streaming Files**: File PDF atau dokumen besar diproses menggunakan **Node.js Stream** (bukan `fs.readFileSync` ke dalam RAM) agar konsumsi memori server tetap rendah di bawah 256 MB.
- **Minimal Docker Footprint**: Jika menggunakan Docker, gunakan teknik *multi-stage build* berbasis Alpine Linux agar ukuran image kontainer kecil (< 150 MB) dan hemat biaya hosting.

### Pilar 4: Observabilitas & Ketahanan (Observability & Reliability)
- **Structured JSON Logging**: Log menggunakan format JSON (Pino) dengan trace ID, actor ID, dan error stack untuk kemudahan filter log di cloud.
- **Healthcheck & Graceful Shutdown**: Sediakan rute `GET /api/health` dan tangani sinyal `SIGTERM` untuk menutup koneksi database secara tertib.

### Pilar 5: Kemudahan Perawatan & Kebersihan Tipe (Maintainability & Type Hygiene)
- **Single Source of Truth Tipe Data**: Seluruh tipe TypeScript diturunkan dari Zod (`z.infer<typeof Schema>`), dilarang duplikasi manual.
- **Haram Barrel Files (`index.ts`)**: Impor langsung dari file spesifik untuk mencegah circular dependencies dan mempercepat tree-shaking.
- **Early Returns (Guard Clauses)**: Tangani error di baris awal fungsi, hindari struktur if-else bersarang.

### Pilar 6: Ketahanan Data & Pemulihan (Data Durability & Disaster Recovery)
- **Soft-Delete Mutlak**: Dokumen transaksi legal DILARANG dihapus permanen (`DELETE FROM`). Gunakan kolom `deleted_at`.
- **Integritas Transaksi Atomik**: Mutasi multi-tabel wajib dibungkus dalam blok `db.$transaction` untuk mencegah korupsi data sebagian.

---

## 5. Langkah demi Langkah Eksekusi

### Langkah 1: Setup Repositori & Pemasangan Harness

> ⚠️ **PROTOKOL AMAN SCAFFOLDING JIKA FOLDER SUDAH BERISI `docs/`**:
> Sejak Modul 01–05 selesai, proyek sudah memiliki folder `docs/pm/` dan `docs/specs/`. Sebagian CLI framework (seperti `create-next-app`) akan menolak membuat proyek jika direktori tidak kosong.
> **Solusi Eksekusi Aman**:
> 1. Pindahkan sementara folder `docs/` ke direktori sementara luar (misal: `../_temp_docs`).
> 2. Jalankan perintah scaffolding framework di root proyek:
>    ```bash
>    pnpm create next-app@latest . --typescript --tailwind --app --no-src-dir=false --import-alias "@/*"
>    ```
> 3. Kembalikan folder `docs/` ke dalam proyek.

> ⛔ **PERINGATAN KERAS BENTURAN `AGENTS.md` BAWAAN FRAMEWORK**:
> Saat scaffolding Next.js 15 selesai, Next.js **secara otomatis men-generate file bawaan bernama `AGENTS.md`** (hanya berisi 9 baris peringatan Next.js).
> **DILARANG KERAS MENGANGGAP `AGENTS.md` SUDAH SELESAI LALU MELEWATKANNYA!**
> Agen WAJIB **MENIMPA** file tersebut menggunakan `templates/dev/AGENTS_TEMPLATE.md` (dan dapat menyertakan blok Next.js di bagian paling bawah). Seluruh aturan 6 pilar rekayasa, Zod boundary, dan larangan tipe `any` wajib terpasang aktif di `AGENTS.md`.

> 📁 **VERIFIKASI KEBERSIHAN ROOT DIREKTORI**:
> Pastikan di root direktori (`./`) **HANYA ADA 7 BERKAS HARNESS**:
> 1. `AGENTS.md`
> 2. `CONTEXT.md`
> 3. `ARCHITECTURE.md`
> 4. `DESIGN.md`
> 5. `CONVENTIONS.md`
> 6. `.env.example` (disalin menjadi `.env.local`)
> 7. `TODO.md`
>
> Seluruh dokumen spesifikasi (`PRD.md`, `FSD.md`, `DESIGN_SPEC.md`) WAJIB berada di dalam **`docs/specs/`**.
> Seluruh dokumen manajemen/hukum (`IDEA_BRIEF.md`, `SCOPE_STATEMENT.md`, `PROJECT_CHARTER.md`) WAJIB berada di dalam **`docs/pm/`**.
> **DILARANG menumpuk dokumen perencanaan di root folder!**

4. Buat branch staging: `git checkout -b staging`
5. Lakukan commit awal untuk mengunci harness dan scaffolding:
   ```bash
   git add .
   git commit -m "chore: initial project scaffolding with 7 AI harness files"
   ```

### Langkah 2: Ekstraksi Komponen Google Stitch via MCP di OpenCode
1. Instruksikan OpenCode:
   > *"Gunakan tool `stitch_get_screen` untuk mengambil kode layar dengan Screen ID yang terdaftar di `DESIGN_SPEC.md`. Ekstrak kode HTML/Tailwind tersebut menjadi komponen React bersih di `src/components/ui/` dan pasang halamannya di `src/app/`."*
2. OpenCode memanggil API Stitch MCP secara langsung, mengambil markup/CSS, dan menuliskan file komponen lokal.
3. Jalankan `pnpm dev` untuk memastikan UI lokal sudah 100% identik dengan hasil desain di Modul 04.

### Langkah 3: Eksekusi Migrasi Basis Data & Seeding
1. Instruksikan agen AI (OpenCode):
   > *"Baca ARCHITECTURE.md bagian Database Models. Buat skema Prisma/Drizzle lengkap dengan konstrain CHECK, relasi foreign key, dan indeks performa. Jalankan migrasinya."*
2. Jalankan migrasi: `pnpm db:migrate`
3. Jalankan seeding data awal: `pnpm db:seed`

### Langkah 4: Koding Backend API, Layanan Vault, & Wiring UI
1. Instruksikan agen AI mengeksekusi item pada `TODO.md` satu per satu:
   - Terapkan validasi Zod pada handler API.
   - Buat layanan enkripsi stream AES-256-GCM ke S3/R2 dan presigned URL 15 menit.
   - Sambungkan form UI Stitch ke endpoint API via `fetch` atau Server Actions.
   - Pastikan kelima kondisi layar berfungsi: *Skeleton Loader*, *Empty State*, *Inline Error Message*, dan *Toast Notifikasi*.

### Langkah 5: Uji Asersi Mandiri (Smoke Test Lokal)
Jalankan skrip uji cepat:
```bash
pnpm run test:smoke
```
Pastikan kompilasi bersih (`pnpm run type-check`) dan audit dependensi aman (`pnpm audit`).

---

## 6. Pencapaian Milestone Pembayaran (Termin Gates)

1. **Milestone Alpha (Termin 2 - 25% s/d 30%)**:
   - *Kriteria Lolos*: Basis data termigrasi, otentikasi login aktif, alur pembuatan draf dokumen berjalan lokal, dan pilar keamanan/performa dasar terverifikasi di branch `staging`.
   - *Tindakan*: Terbitkan Invoice Termin 2 ke Klien.
2. **Milestone Beta (Termin 3 - 20% s/d 25%)**:
   - *Kriteria Lolos*: Seluruh modul backend, frontend, vault terenkripsi terhubung lengkap serta siap diuji coba di server Staging.
   - *Tindakan*: Lanjut ke Modul 07 (QA & SIT) sebelum UAT klien.

---

## 7. Artefak Keluaran (Deliverables)

1. **Source Code Repositori Git**: Basis kode bersih dengan branch `staging` aktif dan riwayat commit terstruktur.
2. **`RUNBOOK_LOCAL.md`**: Panduan lengkap setup environment, migrasi DB, dan menjalankan aplikasi di lokal (menggunakan `templates/dev/RUNBOOK_LOCAL_TEMPLATE.md`).
3. **`VERIFY_LOCAL.md`**: Lembar hasil verifikasi mandiri bahwa seluruh endpoint FSD, 3 pilar rekayasa, dan alur Stitch berfungsi 100% (menggunakan `templates/dev/VERIFY_LOCAL_TEMPLATE.md`).

---

## 8. Kriteria Kelulusan Gerbang (Gate Exit Criteria)

Gerbang Modul 06 dinyatakan **LOLOS (PASS)** jika:
- [x] Percabangan Git terstruktur (`main`, `staging`, `feat/*`) dengan commit rapi.
- [x] 7 berkas kendali AI (Agent Harness) terpasang di root proyek.
- [x] Komponen Google Stitch telah diekstrak via MCP dan terhubung ke backend API.
- [x] Kode sumber berhasil di-build tanpa error kompilasi TypeScript (`tsc --noEmit` exit 0).
- [x] Migrasi basis data berjalan mulus dengan indeks pencarian terpasang.
- [x] Enkripsi file vault AES-256-GCM berbasis stream berhasil menyimpan dan membaca dokumen via presigned URL.
- [x] Skrip uji mandiri (`smoke-test`) lulus 100% dan audit dependensi (`pnpm audit`) bebas celah kritis.

*Jika seluruh kriteria terpenuhi, sistem resmi melangkah ke **Modul 07: Quality Assurance (Unit Test, SIT, & Security Audit)**.*
