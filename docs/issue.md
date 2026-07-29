# 🏗️ Master Plan & Panduan Implementasi: Toko Bangunan POS & ERP

> **PENTING UNTUK DEVELOPER / AI:** 
> Dokumen ini adalah spesifikasi teknis lengkap (SOP) untuk membangun sistem Kasir & ERP Toko Bangunan. Anda **DIWAJIBKAN** untuk mengikuti panduan ini secara berurutan (dari Fase 1 hingga selesai). Jangan melompat mengerjakan Frontend jika API Backend belum selesai dan belum dites.

---

## 🛠️ Stack Teknologi Utama
1. **Database:** PostgreSQL (Di-hosting di Supabase)
2. **Backend:** ExpressJS + TypeScript + Prisma ORM
3. **Frontend:** Next.js (App Router) + TypeScript + Tailwind CSS
4. **State Management:** Zustand (Client state) & React Query (Server state / Caching)
5. **Autentikasi:** JWT (Access Token & Refresh Token)

---

## 🚀 FASE 1: PANDUAN SETUP AWAL (INITIALIZATION)

Tahap ini bertujuan untuk menyiapkan pondasi proyek. Lakukan eksekusi secara berurutan.

### 1. Setup Backend (ExpressJS + Prisma)
**Direktori:** `/backend`
- Jalankan inisialisasi: `npm init -y`
- Install dependencies utama: `npm install express cors jsonwebtoken dotenv`
- Install dev dependencies: `npm install -D typescript @types/node @types/express @types/cors @types/jsonwebtoken ts-node nodemon`
- Setup TypeScript: `npx tsc --init`. (Pastikan `outDir` mengarah ke `./dist`).
- Inisialisasi Prisma ORM: `npx prisma init`
- **Tugas Supabase:** Buat project di Supabase. Dapatkan URL koneksi PostgreSQL. Masukkan URL tersebut ke file `/backend/.env` pada variabel `DATABASE_URL`.
- Buat struktur folder: `src/controllers`, `src/routes`, `src/middlewares`, `src/services`, `src/utils`.

### 2. Setup Frontend (Next.js PWA)
**Direktori:** `/frontend`
- Jalankan inisialisasi: `npx create-next-app@latest .` (Pilih: TypeScript = Yes, Tailwind CSS = Yes, App Router = Yes).
- Install dependencies tambahan: `npm install zustand @tanstack/react-query axios`
- Install library PWA (opsional untuk tahap awal, namun wajib nanti): `npm install next-pwa`
- Setup konfigurasi Axios untuk base URL backend (misal: `http://localhost:5000/api`).

---

## 🗺️ FASE 2: PANDUAN IMPLEMENTASI FITUR (MILESTONE)

*Setiap sub-poin di bawah ini bisa dijadikan acuan pembuatan 1 sesi prompt untuk AI atau 1 tiket task untuk Junior Developer.*

### 🔐 Milestone 1: Arsitektur Dasar & Autentikasi
Sistem ini menggunakan multi-akses (Kasir, Admin, Super User) dan dibatasi oleh Cabang.
1. **Skema Prisma (Backend):**
   - Buat model `Branch` (Cabang).
   - Buat model `User` (Karyawan) dengan atribut `role` (SUPER_USER, ADMIN, KASIR) dan berikan relasi `branch_id` ke model `Branch`.
2. **API Autentikasi (Backend):**
   - Buat fungsi *seeder* untuk meng-generate 1 Super User pertama.
   - Endpoint `POST /api/auth/login` yang mereturn JWT Access Token (limit singkat misal 1 jam) dan Refresh Token.
   - Buat Middleware `verifyToken` dan `verifyRole`.
3. **UI Autentikasi (Frontend):**
   - Buat Halaman Login. Simpan token yang didapat ke dalam Cookies / LocalStorage.
   - Konfigurasi *Axios Interceptors* agar setiap request ke Backend otomatis menyematkan token JWT di *Header Authorization*.

### 📦 Milestone 2: Master Data Multi-Cabang
Data produk, kategori, pelanggan, dan supplier. Perhatian: Kasir hanya boleh mengakses data di cabangnya sendiri.
1. **Skema Prisma (Backend):**
   - Buat model `Category`, `Supplier`, `Customer`.
   - Buat model `Product` (Nama, SKU/Barcode, Satuan [sak/batang/m3], Harga Jual).
2. **CRUD API (Backend):**
   - Buat REST API untuk semua Master Data (GET, POST, PUT, DELETE).
   - **Aturan Multi-Cabang:** Filter query GET berdasarkan `user.branch_id` (diambil dari token JWT kasir yang sedang login). Super User bisa melihat semua.
3. **UI Master Data (Frontend):**
   - Buat halaman Admin berupa tabel (dilengkapi filter kategori & kolom search).
   - Form untuk menambah dan mengedit barang.

### 📊 Milestone 3: Sistem Stok Berbasis Ledger (SANGAT KRITIKAL)
**ATURAN KERAS:** JANGAN PERNAH membuat sistem stok dengan sekadar `stok = stok - 1`. Gunakan riwayat (Ledger).
1. **Skema Prisma (Backend):**
   - Buat model `StockMovement` dengan field: `product_id`, `branch_id`, `type` (IN, OUT, RETURN_GOOD, RETURN_BAD, VOID), `qty`, `avg_cost` (Harga Modal Rata-rata), `reference_id` (ID Transaksi).
2. **Logika Backend:**
   - Stok akhir sebuah barang = `SUM(qty IN) - SUM(qty OUT)`. Buatkan *Service Function* untuk menghitung ini secara dinamis.
   - Implementasikan *Average Cost* (HPP). Setiap ada pembelian barang dari Supplier, hitung ulang rata-rata harga modal barang tersebut.
3. **Fitur Pengembalian/Void:**
   - Jika ada pembatalan transaksi, insert row baru di `StockMovement` dengan tipe `VOID` untuk mengembalikan kuantitas stok tanpa menghapus riwayat sebelumnya.

### 🛒 Milestone 4: Modul Kasir (Point of Sale) & PWA
Ini adalah layar utama yang dipakai kasir sehari-hari.
1. **Fitur Offline-First (Frontend):**
   - Gunakan fitur PWA agar website bisa diakses tanpa internet.
   - Cache hasil API Master Data Produk ke dalam *IndexedDB* (bisa dibantu library localForage atau React Query cache).
   - Jika offline, tampung data transaksi di IndexedDB lokal. Jika kembali online, kirim ke server (Sync).
2. **UI Kasir (Frontend):**
   - Tampilan keranjang belanja (Cart).
   - Pilihan metode bayar (Cash, QRIS, Transfer, Kasbon).
   - Simpan data customer pembeli (jika diinput).
3. **Cetak Struk & Notif WA:**
   - Gunakan `navigator.bluetooth` API untuk connect ke printer Thermal Bluetooth lokal.
   - Buat tombol "Kirim Struk WA" yang membuka URL redirect: `https://wa.me/{NoHPCustomer}?text={StrukTeksFormat}`.

### 💳 Milestone 5: Piutang (Kasbon) & Hutang Supplier
Sistem harus bisa mencicil hutang secara bertahap dengan berbagai metode pembayaran.
1. **Skema Prisma (Backend):**
   - Model `Receivable` (Piutang Customer): Total Kasbon, Sisa Hutang, Status.
   - Model `Payable` (Hutang Supplier).
   - Model `PaymentHistory` (Log cicilan).
2. **Logika Transaksi:**
   - Saat kasir memilih metode "Kasbon", validasi apakah customer sudah didaftarkan, lalu catat di `Receivable`.
   - Buat UI & API khusus untuk layar "Pembayaran Cicilan".

### 📈 Milestone 6: Akuntansi & Laporan (Laba Rugi)
Fitur untuk Owner / Super User memantau kinerja toko.
1. **API Laporan (Backend):**
   - **Laporan Laba Rugi:** `Total Pendapatan (Penjualan) - Total HPP (Berdasarkan Average Cost di StockMovement)`.
   - **Cash Saat Ini:** Selisih uang masuk (Penjualan Tunai/Transfer/Cicilan Piutang) dikurangi uang keluar (Bayar Supplier).
   - Buat endpoint agregasi data per hari, minggu, bulan. Fitur filter per cabang wajib ada.
2. **Manajemen Investor:**
   - Model `Investor` (Nama, Modal Disetor, Persentase Bagi Hasil).
   - Kalkulasi pembagian otomatis berdasarkan Net Profit (Laba Bersih).
3. **UI Dashboard (Frontend):**
   - Buat chart menggunakan library `chart.js` atau `recharts` untuk visualisasi performa toko.
