# Solo Software Lifecycle Orchestrator

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Framework](https://img.shields.io/badge/Architecture-Stack--Agnostic-orange.svg)](#)
[![Design](https://img.shields.io/badge/Design-Anti--Slop_Stitch-purple.svg)](#)
[![Scale](https://img.shields.io/badge/Scale-Small_to_Enterprise-green.svg)](#)
[![AI Ready](https://img.shields.io/badge/AI_Agent-OpenCode_%7C_Cursor_%7C_Claude-brightgreen.svg)](#)

Framework siklus hidup rekayasa perangkat lunak (*Software Development Life Cycle* / SDLC) end-to-end yang dirancang khusus untuk **Solo Developer, Technical Consultant, dan AI Coding Agents** (OpenCode, OpenChamber, Cursor, Claude Code).

Membimbing eksekusi proyek dari ide mentah (*raw idea*), elisitasi kebutuhan, proteksi komersial, perancangan antarmuka anti-slop via Google Stitch, koding mandiri (*agentic vibe coding*), pengujian integrasi (SIT), audit data (UU PDP), validasi UAT, rilis produksi, serah terima BAST, hingga transisi ke pendapatan berulang (*Monthly Retainer SLA*).

---

## 🌟 Mengapa Framework Ini Dibuat?

Solo developer menghadapi tantangan yang sangat berbeda dibandingkan tim korporat:
- **Ketiadaan Bantalan Tim**: Tidak ada tim QA atau Account Manager terpisah. Setiap fitur yang tidak terdokumentasi adalah potensi kerja paksa tanpa bayaran (*unpaid scope creep*).
- **Sindrom AI Slop**: AI coding agent sering menghasilkan kode rapuh, gradien warna norak, dan halusinasi database jika tidak dipasangi berkas kendali (*Agent Harness*).
- **Risiko Pembayaran Macet**: Klien korporat sering menunda pelunasan jika akses root dan repositori diserahkan terlalu awal.

Framework ini menyelesaikan masalah di atas dengan **4 Gerbang Pemblokir Mutlak (The 4 Blocking Gates)**:
1. **Gate Komersial (Modul 03)**: Dilarang koding sebelum kontrak SOW ditandatangani dan Down Payment (DP 30%–50%) cair di rekening bank.
2. **Gate Desain (Modul 04)**: Dilarang koding sebelum klien menandatangani *Design Freeze Sign-Off* pada prototipe interaktif Google Stitch.
3. **Gate Validasi (Modul 09)**: Dilarang deploy produksi sebelum klien menandatangani Berita Acara UAT di server Staging.
4. **Gate Penyerahan (Modul 11)**: Dilarang mentransfer kepemilikan repositori GitHub dan password root sebelum sisa pelunasan 100% diterima dan BAST bermeterai ditandatangani (*No Pay, No Root*).

---

## 🗺️ Peta 12 Rantai Alur Kerja (The 12-Stage Pipeline)

```text
[ FASE INISIASI & DISCOVERY ]
  01. Idea & Feasibility           ──► Saringan 3 lapis, uji 4 kelayakan, penentuan skala proyek
  02. Discovery & Scope Definition ──► 5 pilar elisitasi, MoSCoW, batas tegas In vs Out-of-Scope
  03. [GATE KOMERSIAL] Legal & DP  ──► Kontrak SOW, Single PIC mutlak, pencairan DP 30%–50%

[ FASE PERANCANGAN & SPESIFIKASI ]
  04. UI/UX Prototyping (Stitch)   ──► Design.md anti-slop, Google Stitch MCP, 5-state, Design Freeze
  05. Arsitektur & Spesifikasi     ──► Stack-agnostic, skema SQL DDL, API contracts, FSD signed

[ FASE PRODUKSI KODING (VIBE CODING) ]
  06. Development Execution        ──► Scaffolding bersih, 7 berkas harness AI, 6 pilar rekayasa,
                                       Git branching (main, staging, feat/*), termin 2 (Alpha)

[ FASE PENGUJIAN & VALIDASI ]
  07. Quality Assurance & SIT      ──► Pragmatic test pyramid, sandbox SIT (payment/vault/email), k6 load test
  08. Data Migration & Seeding     ──► Clean-In/Clean-Out boundary, skrip batch ETL, masking PII Staging (UU PDP)
  09. [GATE VALIDASI] Client UAT   ──► Pengujian pengguna di Staging, triase bug, deemed acceptance, UAT signed

[ FASE RILIS & PENUTUPAN ]
  10. Deployment & Go-Live         ──► No Friday Deploy, DNS/SSL TLS 1.3, rilis mobile Keystore/TestFlight, PVT
  11. [GATE PENYERAHAN] BAST       ──► No Pay No Root (Pelunasan 100%), training (1–2 sesi), BAST bermeterai
  12. Masa Garansi & Retainer SLA  ──► Garansi bug murni (30–90 hari), SOP insiden, konversi ke Monthly Retainer
```

---

## ⚡ Panduan Memulai Proyek Baru (Quickstart & Scaffolding)

> **ATURAN MUTLAK INISIASI (ANTI-CONFLICT PROTOCOL)**:
> Jangan pernah menyalin berkas harness AI (`AGENTS.md`, `TODO.md`, dll.) ke folder tujuan **SEBELUM** Anda menjalankan scaffolding framework bahasa pemrograman Anda. Sebagian besar CLI framework akan menolak membuat proyek jika folder target sudah berisi berkas.

### Urutan Eksekusi yang Benar:

1. **Selesaikan Modul 01–05**: Dapatkan berkas spesifikasi (`PRD.md`, `FSD.md`, `DESIGN.md`, dan Screen ID Google Stitch).
2. **Inisiasi Proyek Bersih (Scaffolding First)**:
   Buka folder proyek baru yang masih **KOSONG**, jalankan perintah sesuai tech stack Anda:
   - **Next.js / Node.js**: `pnpm create next-app@latest . --typescript --tailwind --app`
   - **Python**: `poetry init` / `django-admin startproject config .` / `uv init`
   - **PHP (Laravel)**: `composer create-project laravel/laravel .`
   - **C# (.NET)**: `dotnet new webapi -o .`
   - **Java (Spring Boot)**: Inisiasi via Spring Initializr (Maven / Gradle)
   - **Go**: `go mod init <nama-modul>`
   - **Mobile Flutter**: `flutter create . --org com.klien`
   - **Mobile Expo**: `npx create-expo-app@latest .`
3. **Salin 7 Berkas Kendali AI (Inject Harness)**:
   Salin 7 berkas template dari folder `templates/dev/` repositori ini ke root proyek baru Anda:
   - `templates/dev/AGENTS_TEMPLATE.md`         $\to$ `AGENTS.md` *(Wajib menimpa AGENTS.md bawaan Next.js 15)*
   - `templates/dev/CONTEXT_TEMPLATE.md`        $\to$ `CONTEXT.md`
   - `templates/dev/ARCHITECTURE_TEMPLATE.md`   $\to$ `ARCHITECTURE.md`
   - `templates/design/DESIGN_MD_TEMPLATE.md`   $\to$ `DESIGN.md`
   - `templates/dev/CONVENTIONS_TEMPLATE.md`    $\to$ `CONVENTIONS.md`
   - `templates/dev/ENV_EXAMPLE_TEMPLATE.md`    $\to$ `.env.example`
   - `templates/dev/TODO_TEMPLATE.md`           $\to$ `TODO.md`
   - Simpan dokumen spesifikasi (`PRD.md`, `FSD.md`, `DESIGN_SPEC.md`) di dalam folder **`docs/specs/`**.
   - Simpan dokumen inisiasi & hukum (`IDEA_BRIEF.md`, `SCOPE_STATEMENT.md`, `PROJECT_CHARTER.md`) di dalam folder **`docs/pm/`**.
   - *(Dilarang menumpuk dokumen perencanaan di root folder!)*
4. **Mulai Koding Mandiri (Vibe Coding)**:
   Buka OpenChamber / OpenCode / Cursor di proyek baru Anda, picu AI:
   > *"Baca AGENTS.md dan TODO.md. Mulai kerjakan tugas Fase 1."*

---

## 🚀 Jalur Cepat (Fast-Track Mode untuk MVP 1–4 Minggu)

Untuk proyek skala kecil (MVP / Freelance cepat):
- Gunakan template terpadu **`templates/fast-track/PROJECT_LITE_TEMPLATE.md`** yang menggabungkan Modul 01, 02, 03, dan 05 menjadi satu berkas ramping **`PROJECT_LITE.md`**.
- **Prinsip UI/UX**: Untuk aplikasi Web dan Mobile, **Modul 04 (Google Stitch UI/UX) TETAP WAJIB DIJALANKAN** agar tampilan tidak menjadi "AI Slop" dan klien memiliki prototipe klik nyata. Modul 04 hanya boleh dilewati jika proyek bersifat murni backend/CLI/otomasi tanpa antarmuka visual.

---

## 📁 Struktur Repositori

```text
.
├── README.md                      # Dokumentasi utama repositori & panduan inisiasi
├── SKILL.md                       # Orchestrator utama & router otomatis AI
├── LICENSE                        # Lisensi open-source MIT
│
├── modules/                       # Panduan pelaksanaan langkah demi langkah per fase
│   ├── 01-idea-feasibility.md
│   ├── 02-discovery-scope.md
│   ├── 03-legal-sow-charter.md
│   ├── 04-uiux-prototyping.md
│   ├── 05-architecture-specs.md
│   ├── 06-development-execution.md
│   ├── 07-quality-assurance-sit.md
│   ├── 08-data-migration-seeding.md
│   ├── 09-uat-client-signoff.md
│   ├── 10-deployment-production.md
│   ├── 11-handover-bast.md
│   └── 12-warranty-sla-retainer.md
│
├── templates/                     # Berkas template siap pakai untuk proyek nyata
│   ├── fast-track/                # PROJECT_LITE_TEMPLATE.md
│   ├── ideation/                  # IDEA_BRIEF_TEMPLATE.md
│   ├── discovery/                 # SCOPE_STATEMENT_TEMPLATE.md
│   ├── commercial/                # PROJECT_CHARTER_TEMPLATE.md, SOW_CONTRACT_TEMPLATE.md
│   ├── design/                    # DESIGN_MD_TEMPLATE.md, DESIGN_SPEC_TEMPLATE.md
│   ├── specs/                     # PRD_FINAL_TEMPLATE.md, FSD_TECHNICAL_TEMPLATE.md
│   ├── dev/                       # AGENTS.md, CONTEXT.md, ARCHITECTURE.md, CONVENTIONS.md,
│   │                              # .env.example, TODO.md, RUNBOOK_LOCAL.md, VERIFY_LOCAL.md
│   ├── qa/                        # TEST_PLAN_SIT.md, SECURITY_AUDIT.md, SIT_REPORT.md
│   ├── migration/                 # DATA_MIGRATION_PLAN.md, RECONCILIATION_REPORT.md
│   ├── uat/                       # UAT_SCENARIOS.md, UAT_DEFECT_LOG.md, UAT_SIGNOFF.md
│   ├── deploy/                    # DEPLOYMENT_RUNBOOK.md, ROLLBACK_PLAN.md, GO_LIVE_REPORT.md
│   ├── handover/                  # USER_MANUAL.md, HANDOVER_PROTOCOL.md, BAST_TEMPLATE.md
│   └── maintenance/               # WARRANTY_POLICY.md, SLA_RETAINER_CONTRACT.md, INCIDENT_RESPONSE.md
│
└── references/                    # Knowledge base & pedoman taktis perlindungan solo developer
    ├── FEASIBILITY_CRITERIA.md            # Rubrik skor kelayakan & Kill Switch
    ├── REQUIREMENT_ELICITATION_GUIDE.md  # Bank pertanyaan 5 pilar elisitasi & deteksi red flags
    ├── SOLO_BOUNDARY_DEFENSE.md          # Taktik tolak scope creep & formula Change Request
    ├── SOLO_UIUX_GUIDE.md                # Prompting Google Stitch anti-slop & WCAG AA
    ├── SOLO_ARCHITECTURE_GUIDE.md        # Boring Tech ladder & kebersihan skema SQL DDL
    ├── SOLO_DEVELOPMENT_PATTERNS.md      # Validasi Zod, streaming AES-256, & pessimistic locking
    ├── SOLO_ENGINEERING_STANDARDS.md     # 6 Pilar Rekayasa, branching Git, & Docker multi-stage
    ├── SOLO_QA_TESTING_GUIDE.md          # Pragmatic test pyramid, sandbox SIT, & load test k6
    ├── SOLO_DATA_MIGRATION_GUIDE.md      # Penanganan spreadsheet rusak & masking PII UU PDP
    ├── SOLO_UAT_FACILITATION_GUIDE.md    # Naskah tolak revisi UAT & klausul Deemed Acceptance
    ├── SOLO_DEPLOYMENT_GUIDE.md          # Aturan No Friday Deploy, rilis mobile, auto-backup
    ├── SOLO_HANDOVER_GUIDE.md            # Aturan No Pay No Root, kuota training, & legalitas BAST
    └── SOLO_MAINTENANCE_RETAINER_GUIDE.md# Formula paket retainer bulanan, MRR rutin, & anti-burnout
```

---

## 📄 Lisensi

Didistribusikan di bawah [Lisensi MIT](LICENSE). Bebas digunakan untuk keperluan pribadi, komersial, freelance, agensi, dan proyek korporat.
