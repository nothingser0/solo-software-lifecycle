---
name: solo-software-lifecycle
description: End-to-end software development lifecycle (SDLC) orchestrator for solo developers and technical consultants executing projects from Small (MVP) to Enterprise scale. Covers the full 12-stage pipeline from raw idea triage, feasibility evaluation, discovery, contract gating, UI/UX, architecture/FSD, development, QA/SIT, data migration, UAT sign-off, production deployment, to BAST handover and maintenance. Trigger whenever proposing a new app idea, scoping a project, qualifying clients, drafting PRD/FSD, planning architectures, or closing projects.
---

# Solo Software Lifecycle Orchestrator

Framework operasional perangkat lunak untuk solo developer dan konsultan teknis dalam mengeksekusi proyek dari skala Kecil (MVP) hingga Enterprise dengan proteksi batas kerja, otomasi AI, dan gerbang kualitas berjenjang.

> 🚀 **PANDUAN INISIASI CEPAT (ANTI-CONFLICT PROTOCOL)**:
> Sebelum memulai koding atau membuat folder proyek, baca panduan Quickstart di **`README.md`**.
> Jangan pernah menyalin berkas harness (`AGENTS.md`, `TODO.md`, dll.) ke folder kosong **sebelum** menjalankan scaffolding framework bahasa Anda (`create-next-app`, `poetry init`, `composer`, `dotnet new`, `flutter create`, dll.) agar tidak terkena penolakan CLI (*directory conflict*).

---

## 1. Peta 12 Rantai Alur Kerja (The 12-Stage Pipeline)

```text
FASE INISIASI & DISCOVERY:
  01. Idea & Feasibility (Saringan 3 Lapis & Skor Kelayakan) ──► modules/01-idea-feasibility.md
  02. Discovery & Scope Definition (Elisitasi Kebutuhan Bisnis)
  03. [GATE KOMERSIAL] Legal SOW, DP, & Single PIC Agreement

FASE PERANCANGAN & SPESIFIKASI:
  04. UI/UX Design & Prototyping (Design System & User Flow)
  05. Arsitektur & Spesifikasi Teknis (PRD, FSD, & Skema DB)

FASE EKSEKUSI & VALIDASI:
  06. Development (Backend, Frontend, Integrasi API)
  07. Quality Assurance (Unit Test, SIT, & Security Audit)
  08. Data Migration & Seeding (Pembersihan & Transformasi Data)
  09. [GATE VALIDASI] UAT & Sign-Off Klien di Staging

FASE RILIS & PENUTUPAN:
  10. Deployment & Production Go-Live (CI/CD, DNS, SSL)
  11. [GATE PENYERAHAN] Pelunasan 100%, Training, BAST, & Handover Repositori
  12. Masa Garansi (Bug Fix) ──► Transisi ke Monthly Retainer / SLA
```

---

## 2. Prinsip Pertahanan Solo Developer (Core Solo Rules)

1. **Defensif terhadap Lingkup (Scope Protection)**: Solo dev tidak memiliki tim pengganti. Setiap penambahan fitur tanpa dokumen resmi adalah beban cuma-cuma (*unpaid work*).
2. **Aturan Single PIC**: Pada proyek Menengah ke atas, Klien wajib menetapkan satu penanggung jawab mutlak untuk mencegah konflik internal klien membebani developer.
3. **Ketergantungan Klien Terkunci (Client Dependency SLA)**: Jadwal rilis terikat pada kecepatan klien menyediakan data, akses, dan approval. Keterlambatan klien otomatis menggeser timeline.
4. **Gerbang Tanpa Kompromi (Gated Delivery)**:
   - Tidak ada koding tanpa DP & kesepakatan tertulis.
   - Tidak ada pointing domain produksi tanpa UAT Pass.
   - Tidak ada serah terima source code/kredensial root tanpa pelunasan 100% dan penandatanganan BAST.

---

## 3. Matriks Skala Proyek & Fast-Track Mode

| Skala | Batasan Karakteristik | Modul 01: Ideation & Feasibility | Modul 02–05: Specs & Design | Modul 06–09: QA & UAT | Modul 10–12: Rilis & BAST |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Kecil (MVP / Fast-Track)** | 1–4 minggu, 1–3 fitur, solo user | **Fast-Track Protocol**: Gunakan 1 berkas `PROJECT_LITE.md` (Gabungan Modul 01, 02, 03, 05). **Modul 04 (Google Stitch UI/UX) TETAP WAJIB** untuk Web/Mobile agar tidak AI-slop | Unit test logika inti + Smoke test lokal, UAT langsung ke pemilik bisnis | Deploy PaaS/Store langsung, BAST format ringkas via email |
| **Menengah** | 1–3 bulan, Auth, DB, Payment/API | Feasibility 4 Dimensi, Validasi Pasar | PRD modular, Google Stitch Design System, FSD, API Contract | Automated test API, SIT, UAT PIC bertanda tangan | CI/CD pipeline, BAST bermeterai, garansi 30–60 hari |
| **Besar** | 3–6 bulan, integrasi multi-sistem | Audit Arsitektur Awal, Risk Analysis | PRD formal, FSD mendalam, Context Map, WBS level 3 | Full test pyramid, Pentest dasar, UAT formal bertahap | Zero-downtime deploy, BAST fisik/digital, garansi 90 hari |
| **Enterprise** | > 6 bulan, kepatuhan hukum, bank/BUMN | Audit UU PDP, Compliance, Security Gate | Business Case, Formal Charter, PRD, FSD, RTM, DPA | Third-party Pentest, Disaster recovery drill, Formal UAT | CAB Approval, scheduled maintenance window, BAST hukum, SLA |

---

## 4. Status Modul Eksekusi

- [x] **Modul 01: Idea & Feasibility**: `modules/01-idea-feasibility.md` — Saringan ide 3 lapis, uji kelayakan 4 dimensi, pemotongan fitur ekstrem, penentuan skala awal.
- [x] **Modul 02: Discovery & Scope Definition**: `modules/02-discovery-scope.md` — Elisitasi kebutuhan stakeholder, pemetaan peran pengguna, breakdown MoSCoW, penguncian In-Scope vs Out-of-Scope, dan pendaftaran dependensi klien.
- [x] **Modul 03: [GATE KOMERSIAL] Legal SOW, DP, & Single PIC Agreement**: `modules/03-legal-sow-charter.md` — Penentuan model kontrak, termin pembayaran milestone, pengikatan mutlak Single PIC, protokol Change Request, dan pengamanan Down Payment.
- [x] **Modul 04: UI/UX Design & Prototyping**: `modules/04-uiux-prototyping.md` — Wajib Google Stitch (DILARANG di-skip/diganti text wireframe bahkan untuk solo dev product), arsitektur informasi (IA), sitemap & rute, adopsi design system (Shadcn/Tailwind), matriks 5 state antarmuka, pencatatan Screen ID untuk Modul 06, dan pembekuan desain (*Design Freeze*).
- [x] **Modul 05: Arsitektur & Spesifikasi Teknis (PRD & FSD)**: `modules/05-architecture-specs.md` — Pemilihan tech stack (Boring Tech ladder), skema basis data SQL DDL, kontrak API & matriks error, arsitektur keamanan (UU PDP/AES-256), dan pengesahan FSD.
- [x] **Modul 06: Development (Backend, Frontend, Integrasi API)**: `modules/06-development-execution.md` — Setup repo & tooling, migrasi DB & seeding lokal, implementasi API Zod-gated, perakitan UI Stitch, enkripsi streaming AES-256, dan self-smoke test.
- [x] **Modul 07: Quality Assurance (Unit Test, SIT, & Security Audit)**: `modules/07-quality-assurance-sit.md` — Piramida pengujian solo dev, SIT sandbox pihak ketiga (Payment/Storage/Email), audit keamanan OWASP/UU PDP, uji beban k6, dan rilis staging.
- [x] **Modul 08: Data Migration & Seeding**: `modules/08-data-migration-seeding.md` — Protokol data hygiene (Clean-In/Clean-Out), pemetaan kolom sumber-ke-target, sanitasi masking PII Staging, skrip batch ETL atomik, dan rekonsiliasi data sign-off.
- [x] **Modul 09: [GATE VALIDASI] UAT & Sign-Off Klien di Staging**: `modules/09-uat-client-signoff.md` — Pengujian pengguna di Staging, matriks triase cacat (Severity 1/2/3/CR), penangkisan scope creep, klausul deemed acceptance, dan Berita Acara UAT bertandatangan.
- [x] **Modul 10: Deployment & Production Go-Live**: `modules/10-deployment-production.md` — Checklist pra-rilis (No Friday Deploy), git merge tagging SemVer, konfigurasi DNS/SSL TLS 1.3, rilis mobile Android Keystore & iOS TestFlight, migrasi DB zero-downtime, dan PVT.
- [x] **Modul 11: [GATE PENYERAHAN] Pelunasan, Training, BAST, & Handover Repositori**: `modules/11-handover-bast.md` — Penagihan invoice final, jatah kuota training (1–2 sesi), transfer repo Git & kredensial terenkripsi (Bitwarden Send), dan penandatanganan BAST sah bermeterai.
- [x] **Modul 12: Masa Garansi & Transisi ke Monthly Retainer / SLA**: `modules/12-warranty-sla-retainer.md` — Penegakan batas masa garansi bug-fix murni, matriks SLA respon/resolusi, penanganan darurat insiden post-mortem, dan konversi ke kontrak retainer bulanan berulang.

---

## 5. Direktori Template

### Jalur Cepat (Fast-Track Mode)
- `templates/fast-track/PROJECT_LITE_TEMPLATE.md`: Template spesifikasi ramping terpadu (Ide + Scope + Komersial + Skema DB) untuk proyek MVP 1–4 minggu. *(Catatan: Modul 04 Google Stitch tetap wajib untuk Web/Mobile).*

### Modul 01 (Aktif)
- `templates/ideation/IDEA_BRIEF_TEMPLATE.md`: Template ringkasan ide, perumusan elevator pitch, 3-filter triage, dan kartu skor kelayakan solo dev.

### Modul 02 (Aktif)
- `templates/discovery/SCOPE_STATEMENT_TEMPLATE.md`: Template kesepakatan lingkup proyek, breakdown fitur MoSCoW, matriks RBAC, batas Out-of-Scope, dan dependensi SLA.

### Modul 03 (Aktif)
- `templates/commercial/PROJECT_CHARTER_TEMPLATE.md`: Piagam proyek pengunci wewenang Single PIC, objektif bisnis, dan milestone global.
- `templates/commercial/SOW_CONTRACT_TEMPLATE.md`: Perjanjian kerja sama legal, klausul termin pembayaran, liability cap, retensi IP, dan aturan Change Request.

### Modul 04 (Aktif)
- `templates/design/DESIGN_MD_TEMPLATE.md`: Template sistem desain anti-slop untuk diunggah ke Google Stitch via `stitch_upload_design_md`.
- `templates/design/DESIGN_SPEC_TEMPLATE.md`: Spesifikasi desain antarmuka, sitemap rute URL, katalog Screen ID Google Stitch, dan lembar pembekuan desain (*Design Freeze*).

### Modul 05 (Aktif)
- `templates/specs/PRD_FINAL_TEMPLATE.md`: Dokumen spesifikasi kebutuhan produk resmi (fungsional, matriks RBAC, metrik KPI, batasan NFR).
- `templates/specs/FSD_TECHNICAL_TEMPLATE.md`: Dokumen spesifikasi teknis arsitektur (ERD, SQL DDL baku, kontrak API JSON request/response, dan security blueprint).

### Modul 06 (Aktif)
- `templates/dev/AGENTS_TEMPLATE.md`: Template aturan kendali mutlak AI coding agent (OpenCode / OpenChamber).
- `templates/dev/CONTEXT_TEMPLATE.md`: Template konteks bisnis, peran user, dan batasan Out-of-Scope untuk AI agent.
- `templates/dev/ARCHITECTURE_TEMPLATE.md`: Template cetak biru arsitektur, struktur folder, dan skema database untuk AI agent.
- `templates/dev/CONVENTIONS_TEMPLATE.md`: Template konvensi gaya kode (kebab-case, Server Components, no barrel files).
- `templates/dev/ENV_EXAMPLE_TEMPLATE.md`: Template kamus variabel lingkungan baku (.env.example).
- `templates/dev/TODO_TEMPLATE.md`: Template daftar tugas atomik koding mandiri untuk AI agent.
- `templates/dev/RUNBOOK_LOCAL_TEMPLATE.md`: Panduan lokal developer (setup environment variables, perintah migrasi, seed fixtures, dan smoke test).
- `templates/dev/VERIFY_LOCAL_TEMPLATE.md`: Checklist verifikasi mandiri sebelum push ke staging (build status, API endpoint check, UI 5-state test, dan security sanity).

### Modul 07 (Aktif)
- `templates/qa/TEST_PLAN_SIT_TEMPLATE.md`: Template rencana pengujian integrasi sistem (SIT) terhadap layanan pihak ketiga di Staging.
- `templates/qa/SECURITY_AUDIT_TEMPLATE.md`: Template laporan audit celah keamanan OWASP Top 10 dan kepatuhan data pribadi UU PDP.
- `templates/qa/SIT_REPORT_TEMPLATE.md`: Laporan resmi bukti kelulusan pengujian integrasi sistem (SIT Pass) prasyarat pembukaan sesi UAT Klien.

### Modul 08 (Aktif)
- `templates/migration/DATA_MIGRATION_PLAN_TEMPLATE.md`: Template pemetaan kolom sumber ke database SQL, batas tanggung jawab data hygiene, dan aturan transformasi.
- `templates/migration/RECONCILIATION_REPORT_TEMPLATE.md`: Template laporan kuantitatif rekonsiliasi baris data terimpor vs ditolak dan lembar Data Sign-Off Klien.

### Modul 09 (Aktif)
- `templates/uat/UAT_SCENARIOS_TEMPLATE.md`: Template panduan pengujian langkah demi langkah bagi pengguna awam di server Staging.
- `templates/uat/UAT_DEFECT_LOG_TEMPLATE.md`: Template lembar kerja pelacakan temuan kendala UAT, matriks triase severity, dan status resolusi.
- `templates/uat/UAT_SIGNOFF_TEMPLATE.md`: Dokumen resmi Berita Acara Hasil Uji Terima Pengguna (UAT Sign-Off Report) bertandatangan Single PIC Klien.

### Modul 10 (Aktif)
- `templates/deploy/DEPLOYMENT_RUNBOOK_TEMPLATE.md`: Template panduan teknis langkah rilis produksi, DNS/SSL check, dan kunci rahasia live.
- `templates/deploy/ROLLBACK_PLAN_TEMPLATE.md`: Template prosedur darurat 15 menit pemulihan rollback jika terjadi kegagalan fatal go-live.
- `templates/deploy/GO_LIVE_REPORT_TEMPLATE.md`: Dokumen resmi Laporan Verifikasi Peluncuran Sistem (Go-Live Report) dengan bukti operasional stabil.

### Modul 11 (Aktif)
- `templates/handover/USER_MANUAL_TEMPLATE.md`: Template panduan operasional pengguna bagi staf dan admin sistem.
- `templates/handover/HANDOVER_PROTOCOL_TEMPLATE.md`: Template berita acara pengalihan kepemilikan repositori Git dan penyerahan kredensial terenkripsi.
- `templates/handover/BAST_TEMPLATE.md`: Dokumen resmi Berita Acara Serah Terima Pekerjaan (BAST) bermeterai Rp 10.000,- pemicu resmi berjalannya masa garansi.

### Modul 12 (Aktif)
- `templates/maintenance/WARRANTY_POLICY_TEMPLATE.md`: Template kebijakan resmi batas garansi, jam kerja layanan, dan definisi galat yang dilindungi.
- `templates/maintenance/SLA_RETAINER_CONTRACT_TEMPLATE.md`: Perjanjian kerja sama pemeliharaan bulanan berulang (Monthly Retainer SLA) pemicu pendapatan rutin.
- `templates/maintenance/INCIDENT_RESPONSE_TEMPLATE.md`: Prosedur standar operasional (SOP) penanganan insiden darurat produksi dan analisis akar masalah (RCA).

---

## 6. Referensi & Pengetahuan Taktis

### Modul 01 (Aktif)
- `references/FEASIBILITY_CRITERIA.md`: Rubrik uji 4 dimensi (teknis, bandwidth solo, kepatuhan UU PDP/ITE, ekonomi) dan daftar red-flag pemicu pembatalan proyek (*Kill Switch*).

### Modul 02 (Aktif)
- `references/REQUIREMENT_ELICITATION_GUIDE.md`: Bank pertanyaan 5 pilar elisitasi, taktik membongkar kebutuhan tersembunyi, dan deteksi red-flags klien saat wawancara.

### Modul 03 (Aktif)
- `references/SOLO_BOUNDARY_DEFENSE.md`: Panduan taktis menolak scope creep, formula hitungan biaya Change Request, penegakan Single PIC, dan protokol penghentian kerja sementara (*Work Pause*).

### Modul 04 (Aktif)
- `references/SOLO_UIUX_GUIDE.md`: Pedoman efisiensi desain solo dev, pemilihan pustaka komponen (Shadcn/Tailwind), rasio kontras WCAG 2.1 AA, dan taktik walk-through prototipe bersama klien.

### Modul 05 (Aktif)
- `references/SOLO_ARCHITECTURE_GUIDE.md`: Pedoman arsitektur Boring Tech, aturan integritas basis data SQL DDL, standar keamanan OWASP Top 10, enkripsi AES-256, dan kepatuhan UU PDP No. 27/2022.

### Modul 06 (Aktif)
- `references/SOLO_DEVELOPMENT_PATTERNS.md`: Pola koding produksi solo dev (validasi batas Zod, streaming enkripsi file AES-256-GCM, transaksi penguncian baris pessimistik, presigned URLs, dan skrip uji mandiri tanpa framework).
- `references/SOLO_ENGINEERING_STANDARDS.md`: Standar rekayasa mendalam (protokol percabangan Git, rilis produksi bersih dari dokumen internal dev, audit keamanan OWASP/UU PDP, pencegahan N+1 query, optimasi aset, dan connection pooling).

### Modul 07 (Aktif)
- `references/SOLO_QA_TESTING_GUIDE.md`: Pedoman efisiensi pengujian solo dev (The Pragmatic Test Pyramid), verifikasi sandbox pihak ketiga (Payment/Storage/Email), audit keamanan OWASP, dan pengujian beban k6.

### Modul 08 (Aktif)
- `references/SOLO_DATA_MIGRATION_GUIDE.md`: Pedoman pemindahan data warisan (Spreadsheet Hell avoidance), skrip otomasi ETL dengan Zod dan batching, masking data sensitif UU PDP di Staging, dan pengesahan Data Sign-Off.

### Modul 09 (Aktif)
- `references/SOLO_UAT_FACILITATION_GUIDE.md`: Panduan fasilitasi UAT bersama klien, naskah menangkis penambahan fitur berkedok bug, matriks triase tingkat keparahan cacat, dan penegakan surat klausul penerimaan otomatis (*Deemed Acceptance*).

### Modul 10 (Aktif)
- `references/SOLO_DEPLOYMENT_GUIDE.md`: Pedoman deployment produksi (aturan No Friday Deploy), migrasi database tanpa henti (Expand and Contract), penurunan TTL DNS, dan skrip backup database harian terenkripsi ke S3/R2.

### Modul 11 (Aktif)
- `references/SOLO_HANDOVER_GUIDE.md`: Pedoman penutupan proyek dan serah terima (aturan No Pay No Root), jatah batas sesi pelatihan (1–2 sesi), transmisi kredensial terenkripsi sekali pakai (Bitwarden Send), dan kekuatan hukum BAST di Indonesia.

### Modul 12 (Aktif)
- `references/SOLO_MAINTENANCE_RETAINER_GUIDE.md`: Pedoman pemeliharaan retainer bulanan (konversi proyek lepas ke MRR stabil), formula paket Bronze/Silver/Gold, penanganan kepanikan klien di WhatsApp, dan batas on-call anti-burnout.
