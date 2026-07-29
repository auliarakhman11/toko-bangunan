# 📋 Roadmap & Issue Planning - Toko Bangunan App

Dokumen ini digunakan sebagai acuan perencanaan (*planning*) pembuatan issue dan pengerjaan fitur pada GitHub Repository. Setiap poin dapat dijadikan **GitHub Issue** tersendiri.

---

## 🎯 Milestone 1: Inisialisasi Project & Arsitektur dasar
- [ ] **#1 Setup Project Backend**
  - Inisialisasi server backend (Environment, Dependencies, Port).
  - Setup struktur folder (Controllers, Routes, Models/Services, Middlewares).
  - Setup koneksi Database (MongoDB / PostgreSQL / MySQL).
- [ ] **#2 Setup Project Frontend**
  - Inisialisasi framework/starter Frontend (HTML/CSS/JS / React / Vite).
  - Setup layouting umum (Navbar, Footer, Sidebar Admin, Responsive Container).
  - Setup router & pemicu API Client (Axios/Fetch utility).
- [ ] **#3 Dokumentasi & CORS Setup**
  - Konfigurasi CORS pada backend agar membolehkan permintaan dari origin frontend.
  - Setup file `.env.example` di backend dan frontend.

---

## 🔐 Milestone 2: Autentikasi & Manajemen Pengguna
- [ ] **#4 API Autentikasi (Backend)**
  - Endpoint Register Pengguna (Pembeli).
  - Endpoint Login & Generate Token (JWT / Session).
  - Endpoint Profile & Refresh Token.
  - Middleware Verifikasi Token & Role-based Access (Admin vs Pembeli).
- [ ] **#5 UI Autentikasi (Frontend)**
  - Halaman Register & Validasi Form.
  - Halaman Login & Penyimpanan Token Auth (LocalStorage / Cookie).
  - Proteksi Halaman (Route Guard untuk Admin & Pembeli).

---

## 📦 Milestone 3: Manajemen Katalog & Produk Bangunan
- [ ] **#6 Database Schema Produk & Kategori**
  - Model/Tabel Kategori (Semen, Besi, Cat, Kayu, Alat Tukang, Pipa & Sanitasi, dll).
  - Model/Tabel Produk (Nama, Kode SKU, Deskripsi, Harga, Satuan: *sak/batang/m3/lembar/dus*, Stok, Gambar, Berat/Volume).
- [ ] **#7 API CRUD Produk & Kategori (Admin Backend)**
  - API Tambah, Edit, Hapus, dan Get Kategori.
  - API Tambah Produk (Upload gambar produk).
  - API Edit & Update Stok Produk.
  - API Hapus Produk (Soft delete / Hard delete).
- [ ] **#8 UI Management Produk (Admin Frontend)**
  - Halaman List Produk (Tabel dengan pagination & search).
  - Modal/Form Tambah Produk Baru (dengan preview gambar & satuan produk).
  - Form Edit & Update Stok Produk.

---

## 🛒 Milestone 4: Katalog Publik & Keranjang Belanja
- [ ] **#9 UI Katalog Produk (Frontend)**
  - Halaman Landing Page / Home dengan Banner & Produk Terpopuler.
  - Filter Produk Berdasarkan Kategori & Range Harga.
  - Fitur Search Produk secara *real-time*.
  - Halaman Detail Produk (Deskripsi, Satuan Beli, Cek Ketersediaan Stok).
- [ ] **#10 Fitur Keranjang Belanja (Shopping Cart)**
  - API / State Keranjang Belanja (Tambah item, ubah kuantitas, hapus item).
  - UI Modal / Halaman Keranjang Belanja.
  - Hitung Otomatis Subtotal berdasarkan Satuan & Jumlah Beli.

---

## 🚚 Milestone 5: Transaksi Checkout & Pengiriman
- [ ] **#11 API Checkout & Pemesanan (Backend)**
  - API Buat Pesanan Baru (Order Items, Alamat Pengiriman, Catatan Pengiriman).
  - Logika Pemotongan Stok Produk Otomatis saat checkout.
  - API Hitung Estimasi Pengiriman / Armada Toko (Truk, Pickup, Kargo Berat).
- [ ] **#12 UI Checkout & Konfirmasi Alamat (Frontend)**
  - Halaman Checkout Form (Alamat lengkap, Opsi armada pengiriman).
  - Ringkasan Pesanan & Total Pembayaran.
  - Generasi Kode Transaksi / Nota Pesanan.

---

## 💳 Milestone 6: Pembayaran & Pengiriman Pesanan
- [ ] **#13 Integrasi Pembayaran (Transfer Bank / E-Wallet / COD)**
  - API Upload Bukti Transfer (jika pembayaran manual).
  - API Integrasi Payment Gateway (Midtrans/Xendit) - *Opsional/Fase Lanjutan*.
  - Endpoint Verifikasi Status Pembayaran oleh Admin.
- [ ] **#14 UI Riwayat Pesanan & Status Tracking**
  - Halaman "Pesanan Saya" untuk Pembeli (Status: *Menunggu Pembayaran, Diproses, Dikirim, Selesai*).
  - Halaman Manajemen Pesanan Masuk untuk Admin.
  - Tombol Update Status Pengiriman oleh Admin (Input Nomor Resi / Driver Toko).

---

## 📊 Milestone 7: Dashboard Admin & Laporan Penjualan
- [ ] **#15 API Laporan Penjualan (Backend)**
  - API Ringkasan Penjualan (Total Omset, Jumlah Transaksi, Produk Terlaris).
  - API Notifikasi Stok Menipis.
- [ ] **#16 UI Dashboard Analytics (Admin Frontend)**
  - Widget Statistik (Total Penjualan, Total Produk, Total User).
  - Chart Penjualan Bulanan / Harian.
  - Tabel Peringatan Stok Bahan Bangunan Menipis.
