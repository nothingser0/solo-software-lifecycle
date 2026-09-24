# Modul 05: Arsitektur & Spesifikasi Teknis (PRD & FSD)

Modul ini adalah tahap kelima dalam siklus hidup proyek perangkat lunak untuk solo developer. Tujuannya adalah merancang seluruh "mesin, kabel data, basis data, dan sistem keamanan" di balik antarmuka yang telah dibekukan pada Modul 04, menghasilkan dua cetak biru utama: **`PRD.md`** (*Product Requirement Document*) dan **`FSD.md`** (*Functional Specification Document*).

---

## 1. Siklus Eksekusi Modul 05

```text
[ INPUT: SCOPE_STATEMENT.md dari Modul 02 & DESIGN_SPEC.md dari Modul 04 ]
                                    │
                                    ▼
[ LANGKAH 1: Pemilihan Tech Stack & Topologi Hosting Sesuai Skala ]
  • Tangga Boring Tech: Hindari over-engineering solo dev
  • Penetapan Bahasa, Framework, Database, dan Cloud Provider
                                    │
                                    ▼
[ LANGKAH 2: Perancangan Skema Basis Data (Database Schema & DDL) ]
  • Desain Tabel, Primary/Foreign Keys, Constraint CHECK, dan Indeks
  • Strategi Audit Trail (created_at, updated_at, actor_id, soft-delete)
                                    │
                                    ▼
[ LANGKAH 3: Pemetaan Kontrak API & Matriks Endpoint ]
  • HTTP Verbs, Path, Header Otentikasi & Idempotency
  • Skema Payload JSON Request & Respon (Sukses vs Error Matrix)
                                    │
                                    ▼
[ LANGKAH 4: Pondasi Keamanan & Kepatuhan Regulasi (Security Blueprint) ]
  • Enkripsi Data Vault (AES-256-GCM / Envelope Encryption)
  • Manajemen Sesi (HttpOnly Cookies, JWT Rotation) & Hashing Argon2id
  • Proteksi OWASP Top 10, Rate Limiting, & Kepatuhan UU PDP
                                    │
                                    ▼
[ LANGKAH 5: Finalisasi & Penandatanganan PRD & FSD ]
  • Review Teknis Bersama Single PIC Klien
  • Tanda Tangan Technical Sign-Off
                                    │
                                    ▼
[ OUTPUT: Dokumen PRD.md & FSD.md ] ──► Siap Masuk ke Modul 06: Development
```

---

## 2. Prinsip Arsitektur Solo Developer: "The Boring Tech Ladder"

Solo dev tidak boleh memilih teknologi karena tren sesaat. Pilihlah teknologi yang paling stabil, minim perawatan, dan mudah di-debug sendirian di malam hari:

| Skala Proyek | Rekomendasi Tech Stack | Database & Storage | Hosting / Infrastruktur |
| :--- | :--- | :--- | :--- |
| **Kecil (MVP)** | Monolith Modern: **Next.js (App Router)** atau **Laravel** | PostgreSQL tunggal (Supabase) / SQLite | PaaS: **Vercel** / **Railway** |
| **Menengah (SaaS/SMB)** | Decoupled / Modular Monolith: **Next.js + Node.js/Go API** | Managed PostgreSQL + Redis (Upstash) + Cloudflare R2 | Container: **Docker** di **Railway / GCP Cloud Run** |
| **Besar (Scale-Up)** | Modular Monorepo: **Go / Node.js** (Hexagonal Architecture) | PostgreSQL (Read/Write Replica) + Redis Cluster + S3 | Managed Cloud: **AWS ECS / EKS** + Terraform |
| **Enterprise** | Strict Microservices / Distributed Monolith: **Java Spring / .NET / Go** | Enterprise DB (Postgres/Oracle) + Kafka + HSM Vault | Hybrid / Private Cloud + On-Premise Data Center |

---

## 3. Langkah demi Langkah Eksekusi

### Langkah 1: Perancangan Skema Basis Data
1. Identifikasi seluruh entitas data dari formulir di `DESIGN_SPEC.md`.
2. Tuliskan skema relasional lengkap dalam format SQL DDL baku.
3. Kunci integritas data di level basis data:
   - Gunakan `UUIDv7` atau `BIGINT` untuk Primary Key.
   - Pasang relasi `FOREIGN KEY` dengan `ON DELETE RESTRICT` (jangan biarkan data transaksi terhapus otomatis secara liar).
   - Pasang constraint `CHECK` (misal: `CHECK (nominal >= 0)`).
   - Pasang indeks pada kolom yang sering dicari (`WHERE user_id = ... AND status = ...`).

### Langkah 2: Pemetaan Kontrak API (API Contract)
Setiap tombol aksi di antarmuka harus memiliki pasangan endpoint API yang terdefinisi dengan format baku:
- **Metode & Rute**: `POST /api/v1/documents`
- **Headers**:
  ```http
  Authorization: Bearer <TOKEN>
  Content-Type: application/json
  X-Idempotency-Key: <UUID>
  ```
- **Payload Request JSON**: Skema field input beserta tipe data dan aturan validasi.
- **Respon Sukses & Respon Error**: Format seragam (`status`, `data`, `error: { code, message }`).

### Langkah 3: Arsitektur Keamanan Terpasang (Built-in Security)
Kunci protokol keamanan sebelum menulis kode:
1. **Penyimpanan Dokumen Sensitif (Vault)**:
   - File PDF dokumen wajib dienkripsi sebelum masuk cloud storage menggunakan AES-256-GCM. Kunci enkripsi dikelola terpisah (*Key Management Service*).
   - Tautan unduhan dokumen wajib menggunakan *Presigned URL* dengan masa kedaluwarsa maksimal 15 menit.
2. **Otentikasi & Password**:
   - Password wajib di-hash menggunakan **Argon2id** (atau bcrypt dengan cost factor $\ge 12$).
   - Token sesi disimpan di `HttpOnly, Secure, SameSite=Strict` cookie untuk mencegah pencurian token melalui serangan Cross-Site Scripting (XSS).
3. **Pembatasan Laju Request (Rate Limiting)**:
   - Endpoint sensitif (Login, Kirim OTP, Checkout) diproteksi pembatasan laju (contoh: maksimal 5 percobaan per IP dalam 15 menit).

### Langkah 4: Penyusunan Dokumen PRD & FSD
- **`PRD.md`**: Memuat ringkasan kebutuhan fungsional bisnis, matriks hak akses pengguna (RBAC), metrik keberhasilan (KPI), dan batasan non-fungsional (NFR: latency $< 200\text{ ms}$, uptime $99.9\%$).
- **`FSD.md`**: Memuat detail teknis mutlak (diagram ERD, script SQL DDL, tabel API contract, state machine transaksi, dan audit logging).

### Langkah 5: Technical Sign-Off Bersama Klien
- Solo dev memaparkan dokumen PRD & FSD ke **Single PIC Klien**.
- Klien menandatangani lembar persetujuan spesifikasi teknis (*Technical Sign-off*).
- Dengan ditandatanganinya FSD, lingkup dan logika teknis resmi terkunci.

---

## 4. Adaptasi Berdasarkan Skala Proyek

| Aspek | Skala Kecil (MVP / Freelance) | Skala Menengah (B2B SaaS / Agensi) | Skala Besar & Enterprise |
| :--- | :--- | :--- | :--- |
| **Dokumen PRD** | Ringkas (3–5 halaman) | Modular terstruktur (10–20 halaman) | Formal Enterprise PRD lengkap |
| **Dokumen FSD** | Skema tabel & rute API inti | FSD lengkap: ERD, SQL DDL, API contracts | FSD mendalam, RTM, Disaster Recovery SOP |
| **Skema Database** | 3–6 tabel relasional | 10–20 tabel dengan migrasi terversi | 30+ tabel, partisi data, sharding plan |
| **Keamanan** | HTTPS, password hashing, RLS | S3 AES-256, JWT rotation, rate limiter | Zero-knowledge vault, HSM, ISO 27001 audit |
| **Approval** | Persetujuan via email/chat tertulis | Tanda tangan lembar Technical Sign-off | Formal Sign-off CAB (Change Advisory Board) |

---

## 5. Artefak Keluaran (Deliverables)

> 📁 **ATURAN LOKASI BERKAS MUTLAK**:
> Seluruh dokumen spesifikasi Modul 05 WAJIB disimpan di dalam folder **`docs/specs/`** (bukan di root direktori).
> DILARANG menaruh `PRD.md` atau `FSD.md` di root proyek.

Modul ini menghasilkan 2 dokumen teknis utama:
1. **`docs/specs/PRD.md`**: Dokumen kebutuhan produk fungsional dan non-fungsional (menggunakan `templates/specs/PRD_FINAL_TEMPLATE.md`).
2. **`docs/specs/FSD.md`**: Dokumen spesifikasi teknis fungsional, skema database DDL, API contracts, dan arsitektur keamanan (menggunakan `templates/specs/FSD_TECHNICAL_TEMPLATE.md`).

---

## 6. Kriteria Kelulusan Gerbang (Gate Exit Criteria)

Gerbang Modul 05 dinyatakan **LOLOS (PASS)** jika:
- [x] Seluruh skema basis data telah ditulis dalam format SQL DDL baku beserta constraints.
- [x] Seluruh endpoint API telah memiliki kontrak payload JSON dan matriks penanganan error.
- [x] Pondasi keamanan (enkripsi, hashing, rate limiting) telah didefinisikan secara eksplisit.
- [x] **Single PIC Klien (atau solo developer) telah menandatangani lembar persetujuan PRD & FSD.**

---

## 🛑 PROTOKOL GERBANG KELUAR & WAJIB BERHENTI (MANDATORY STOP)

Setelah berkas `docs/specs/PRD.md` dan `docs/specs/FSD.md` selesai ditulis:
1. **DILARANG KERAS langsung melanjutkan, melakukan scaffolding, atau memanggil tool untuk Modul 06 dalam giliran (turn) yang sama!**
2. Tampilkan ringkasan cetak biru teknis kepada pengguna:
   - Tech stack & topologi infrastruktur terpilih
   - Tabel skema database utama (entitas & relasi)
   - Daftar endpoint API inti
3. **AKHIRI RESPON ANDA (END TURN)** dan ajukan konfirmasi kepada pengguna:
   > *"Dokumen spesifikasi teknis `docs/specs/PRD.md` dan `docs/specs/FSD.md` telah selesai disusun. Apakah arsitektur dan skema database ini disetujui (Technical Sign-Off) sebelum kita memulai scaffolding dan koding di Modul 06?"*
4. Tunggu respon persetujuan eksplisit dari pengguna sebelum memulai Modul 06.
