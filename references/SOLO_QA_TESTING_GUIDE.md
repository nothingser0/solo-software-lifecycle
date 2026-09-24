# Panduan Quality Assurance & SIT Solo Developer

Dokumen ini adalah buku pedoman praktis bagi solo developer dalam menjalankan pengujian mutu perangkat lunak, verifikasi integrasi sistem (SIT), audit celah keamanan, dan uji beban konkurensi tanpa membuang waktu untuk pengujian manual yang berulang-ulang.

---

## 1. Filosofi Pengujian Solo Developer: "The Pragmatic Test Pyramid"

Kesalahan terbesar solo developer adalah mencoba meniru tim QA korporat dengan menulis ratusan skrip otomasi browser (Selenium / Cypress) yang lambat dan rapuh (*flaky*). Setiap kali ada perubahan class Tailwind atau tata letak HTML, pengujian browser tersebut gagal.

### Alokasi Energi Pengujian yang Efisien:
1. **70% Energi: Unit & Contract Tests**:
   - Uji logika murni (*pure functions*): validasi Zod, kalkulasi matematika, transformasi data, dan enkripsi/dekripsi AES-256.
   - Gunakan test runner cepat seperti **Vitest** (kecepatan eksekusi dalam hitungan milidetik).
2. **25% Energi: API Integration & System Integration Testing (SIT)**:
   - Uji endpoint API terhadap database uji lokal dan sandbox pihak ketiga (Payment Gateway, Cloud Storage, Email SMTP).
3. **5% Energi: 1 Critical Path E2E Smoke Test**:
   - Cukup satu pengujian alur kritis (*Core User Journey*) dari login hingga dokumen berstatus `SIGNED`.

---

## 2. Playbook Pengujian Integrasi Pihak Ketiga (SIT Playbook)

### 2.1 Pengujian Payment Gateway Sandbox (Midtrans / Xendit)
Saat menguji transaksi pembayaran, fokus utama adalah **keamanan webhook**:
- **Verifikasi Tanda Tangan Webhook (Signature Key)**:
  Pastikan backend Anda memverifikasi SHA-512 signature yang dikirim payment gateway untuk mencegah serangan webhook palsu (*spoofing*).
  ```typescript
  // Contoh verifikasi signature Midtrans
  const signature = crypto.createHash("sha512")
    .update(`${order_id}${status_code}${gross_amount}${server_key}`)
    .digest("hex");
  if (signature !== req.headers["x-callback-signature"]) {
    return Response.json({ error: "Invalid signature" }, { status: 403 });
  }
  ```
- **Simulasi Transaksi Sukses vs Gagal**:
  Kirim payload simulasi pembayaran sukses (`settlement`) dan pembayaran kedaluwarsa (`expire`) dari dashboard sandbox untuk memastikan mesin keadaan (*state machine*) status order berpindah dengan benar.

### 2.2 Pengujian Penyimpanan Dokumen (Cloudflare R2 / AWS S3)
- **Uji Enkripsi Biner**: Unduh file langsung via S3 CLI atau dashboard cloud. Buka file tersebut menggunakan PDF reader. File harus **rusak / gagal dibuka** (membuktikan enkripsi at-rest bekerja).
- **Uji Masa Kedaluwarsa Presigned URL**:
  Terbitkan presigned URL dengan durasi 5 detik untuk pengujian. Tunggu 6 detik, lalu buka link tersebut di browser. Browser wajib menerima respon `403 Forbidden` atau `Request has expired`.

### 2.3 Pengujian Email Transaksional (Zero Spamming)
- Gunakan layanan penangkap email lokal seperti **Mailpit** (via Docker) atau **Ethereal Email** saat pengujian lokal agar Anda tidak membuang kuota email API berbayar atau memicu deteksi spam filter.

---

## 3. Playbook Uji Beban & Konkurensi (Load Testing)

Solo developer wajib membuktikan sistem tidak akan tumbang saat puluhan orang mengakses aplikasi secara bersamaan.

### Skrip Uji Beban Cepat Menggunakan `k6`:
Buat berkas `scripts/load-test.js`:
```javascript
import http from "k6/http";
import { check, sleep } from "k6";

export const options = {
  vus: 50, // 50 Virtual Users bersamaan
  duration: "30s", // Selama 30 detik
  thresholds: {
    http_req_duration: ["p(95)<200"], // 95% request harus di bawah 200ms
    http_req_failed: ["rate<0.01"],   // Error rate harus di bawah 1%
  },
};

export default function () {
  const res = http.get("https://staging.domainklien.com/api/health");
  check(res, {
    "status is 200": (r) => r.status === 200,
  });
  sleep(1);
}
```

Jalankan perintah:
```bash
k6 run scripts/load-test.js
```

---

## 4. Checklist Audit Keamanan Mandiri (Security Gate)

Sebelum menyatakan lingkungan Staging siap untuk diuji klien, verifikasi 5 poin ini:
1. [x] **`pnpm audit`**: 0 kerentanan kritis atau tinggi.
2. [x] **Zero Secret Leak**: Tidak ada token AWS/S3 atau connection string database yang tertinggal di riwayat git commit.
3. [x] **IDOR Protection**: Pengguna A tidak dapat mengunduh dokumen Pengguna B dengan hanya mengganti ID di URL (`/api/v1/documents/UUID_B`).
4. [x] **Rate Limiting Aktif**: Percobaan login gagal lebih dari 5 kali berturut-turut otomatis memicu blokir sementara (*HTTP 429 Too Many Requests*).
5. [x] **Header Keamanan HTTP**: Header `Content-Security-Policy`, `X-Frame-Options`, dan `X-Content-Type-Options` terpasang rapi.
