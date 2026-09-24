# Modul 02: Discovery & Scope Definition (Elisitasi Kebutuhan & Penguncian Lingkup)

Modul ini adalah tahap kedua dalam siklus pengembangan perangkat lunak untuk solo developer. Tujuannya adalah mengekstrak kebutuhan bisnis riil dari pemangku kepentingan (stakeholder/klien), mendefinisikan batasan teknis, dan mengunci batasan **In-Scope vs Out-of-Scope** ke dalam dokumen **`SCOPE_STATEMENT.md`** sebelum masuk ke komitmen kontrak atau perancangan detail.

---

## 1. Siklus Eksekusi Modul 02

```text
[ INPUT: Dokumen IDEA_BRIEF.md dari Modul 01 ]
                      │
                      ▼
[ LANGKAH 1: Wawancara Discovery Terarah (The 5 Pillars) ]
  • Tujuan Bisnis Riil & Metrik Sukses
  • Pemetaan Persona Pengguna & Matriks Hak Akses
                      │
                      ▼
[ LANGKAH 2: Breakdown Fitur & Prioritas MoSCoW ]
  • Must-Have (Fitur Vital Rilis)
  • Should-Have / Could-Have (Fitur Sekunder)
  • Won't-Have (Fitur Ditolak / Ditunda)
                      │
                      ▼
[ LANGKAH 3: Penguncian Batasan Lingkup (Scope Defense) ]
  • Daftar Eksplisit: APA YANG DIBUAT vs APA YANG TIDAK DIBUAT
  • Asumsi Teknis & Batasan Arsitektur Awal
                      │
                      ▼
[ LANGKAH 4: Pendaftaran Ketergantungan Klien (Client Dependencies) ]
  • Data Master, Akses Server, Kredensial API Pihak Ketiga
  • Batas Waktu Penyerahan (Dependency SLA)
                      │
                      ▼
[ OUTPUT: Dokumen SCOPE_STATEMENT.md ] ──► Siap Lanjut ke Modul 03: Legal SOW & DP
```

---

## 2. Langkah demi Langkah Eksekusi

### Langkah 1: Wawancara Discovery Terarah
Jalankan wawancara menggunakan panduan di `references/REQUIREMENT_ELICITATION_GUIDE.md`:
1. **Identifikasi Pengambil Keputusan**: Pastikan orang yang diwawancarai memiliki wewenang final menyetujui fitur.
2. **Bedah Kebutuhan vs Keinginan**: Bedakan kebutuhan inti bisnis (*needs*) dengan fitur pemanis (*nice-to-have wishes*).
3. **Petakan Peran Pengguna (User Roles)**: Tentukan siapa saja yang login ke sistem dan hak akses spesifiknya (misal: Super Admin, Kasir, Pelanggan).

### Langkah 2: Breakdown Fitur & Prioritisasi MoSCoW
Setiap modul dipecah menjadi fitur spesifik dengan label prioritas:
- **Must Have (P0)**: Sistem gagal berfungsi tanpa fitur ini (contoh: checkout order, otentikasi login).
- **Should Have (P1)**: Fitur penting tapi ada cara manual alternatif sementara (contoh: export laporan ke Excel).
- **Could Have (P2)**: Fitur tambahan jika waktu dan kapasitas solo dev tersisa (contoh: notifikasi WhatsApp).
- **Won't Have (P3)**: Fitur yang secara resmi disepakati tidak dibuat di fase ini (contoh: AI chatbot rekomendasi).

### Langkah 3: Penguncian Batasan Lingkup (In-Scope vs Out-of-Scope)
Solo developer wajib menuliskan bagian **Out-of-Scope** dengan detail agresif. Prinsip hukum perdata: *"Semua yang tidak tertulis secara eksplisit sebagai In-Scope adalah di luar tanggung jawab developer."*

Contoh Out-of-Scope standar yang wajib dicantumkan:
- Migrasi data manual dari buku/kertas fisik atau format database yang rusak.
- Pembelian lisensi font, aset gambar stok berbayar, atau biaya langganan API pihak ketiga.
- Penanganan kendala jaringan lokal, hardware scanner rusak, atau komputer kantor klien yang terinfeksi malware.

### Langkah 4: Identifikasi Ketergantungan Klien (Client Dependency SLA)
Daftar seluruh hal yang wajib disediakan klien agar pengerjaan tidak terhambat:
- Akun sandbox dan API secret key (payment gateway, email sender, cloud hosting).
- Master data awal dalam format digital terstruktur (CSV/JSON/Excel).
- Ketersediaan PIC untuk sesi klarifikasi mingguan.

Tentukan klausul: *Setiap keterlambatan penyerahan dependensi oleh klien $\ge 3$ hari kerja otomatis menggeser target rilis sistem tanpa denda bagi developer.*

---

## 3. Adaptasi Berdasarkan Skala Proyek

| Aspek | Skala Kecil (MVP / Freelance) | Skala Menengah (B2B SaaS / Agensi) | Skala Besar & Enterprise |
| :--- | :--- | :--- | :--- |
| **Durasi Wawancara** | 1 sesi chat/call (30–60 menit) | 2–3 sesi discovery (1–2 minggu) | Workshop berjenjang per divisi (2–4 minggu) |
| **Kedalaman Persona** | 1–2 user role sederhana | 3–5 role dengan matriks RBAC | Multi-divisi, hierarki departemen, SSO Okta/AD |
| **Dokumen Scope** | 1-page Scope Checklist | Formal Scope Statement & API outline | Scope Statement lengkap, RTM draft, Compliance scope |
| **Ketergantungan** | Akses hosting & payment key dasar | Integrasi 2–4 layanan cloud | Integrasi legacy system/ERP, izin firewall internal |

---

## 4. Artefak Keluaran (Deliverable)

Hasil akhir dari Modul 02 adalah berkas **`SCOPE_STATEMENT.md`** yang dibuat menggunakan template di `templates/discovery/SCOPE_STATEMENT_TEMPLATE.md`.

Dokumen ini menjadi dasar mutlak untuk penyusunan **Kontrak SOW, Penentuan Harga, dan Pembayaran DP pada Modul 03**.
