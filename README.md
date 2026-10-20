# 🎓 LaporKampus

> **Aplikasi Android Pelaporan Kerusakan Fasilitas Kampus**
> Dibuat dengan React Native (Expo) + Node.js (Express) + MongoDB Atlas

---

## 📁 Struktur Proyek

```
LaporKampus/
├── backend/          ← Server API Node.js + Express
└── mobile/           ← Aplikasi React Native (Expo)
```

---

## ⚙️ Setup Backend (Server)

### 1. Konfigurasi MongoDB Atlas

Edit file `backend/.env` dan isi MONGO_URI kamu:

```env
PORT=5000
MONGO_URI=mongodb+srv://admin:PASSWORD@laporkampus.xxxxx.mongodb.net/laporkampus?retryWrites=true&w=majority
```

> **Cara dapat MONGO_URI:**
> 1. Login ke [MongoDB Atlas](https://cloud.mongodb.com)
> 2. Klik **Connect** pada cluster kamu
> 3. Pilih **Drivers** → pilih Node.js
> 4. Copy connection string, ganti `<password>` dengan password kamu

### 2. Jalankan Backend

```bash
cd backend

# Install dependencies (jika belum)
npm install

# Isi database dengan kategori awal (jalankan SEKALI)
npm run seed

# Jalankan server (mode development, auto-restart)
npm run dev

# atau jalankan production
npm start
```

Server berjalan di: `http://localhost:5000`

### 3. Test API dengan Postman

| Method | URL | Body |
|--------|-----|------|
| POST | `/api/register` | `{ "nim": "123", "nama": "Budi", "password": "abc123" }` |
| POST | `/api/login` | `{ "nim": "123", "password": "abc123" }` |
| GET | `/api/categories` | - |
| POST | `/api/reports` | FormData: userId, kategori, lokasi_ruangan, deskripsi, foto |
| GET | `/api/reports/:userId` | - |
| PUT | `/api/reports/:id/status` | `{ "status": "Selesai" }` |

---

## 📱 Setup Mobile (Expo)

### 1. Cari IP Lokal Laptop Kamu

Buka **CMD** di Windows:
```
ipconfig
```
Cari `IPv4 Address` → Contoh: `192.168.1.100`

### 2. Konfigurasi API URL

Edit file `mobile/src/api/api.js`:
```js
const BASE_URL = 'http://192.168.1.100:5000/api'; // ← Ganti dengan IP kamu!
```

> ⚠️ **PENTING:** Jangan pakai `localhost`! HP dan laptop harus terhubung ke **WiFi yang sama**.

### 3. Jalankan Expo

```bash
cd mobile

# Install dependencies (jika belum)
npm install

# Jalankan Expo development server
npx expo start
```

### 4. Buka di HP

1. Install **Expo Go** di HP Android dari Play Store
2. Buka kamera HP → scan **QR Code** yang muncul di terminal
3. Aplikasi akan terbuka di HP!

---

## 🗂️ Endpoint API Lengkap

```
POST   /api/register          → Daftar akun baru
POST   /api/login             → Login, dapat user data
GET    /api/categories        → Daftar kategori kerusakan
POST   /api/reports           → Kirim laporan baru (multipart/form-data)
GET    /api/reports/:userId   → Riwayat laporan milik user
GET    /api/reports/detail/:id → Detail satu laporan
PUT    /api/reports/:id/status → Update status laporan
GET    /uploads/:filename     → Akses foto (static file)
```

---

## 🎨 Layar Aplikasi

| Layar | Deskripsi |
|-------|-----------|
| **Login** | Form NIM + Password dengan session persistent |
| **Register** | Form daftar akun baru dengan validasi |
| **Home** | Dashboard dengan statistik + FlatList laporan + FAB |
| **Form Laporan** | Dropdown kategori + input lokasi/deskripsi + foto |
| **Detail Laporan** | Foto full + badge status + ubah status |

---

## 🔴🟡🟢 Kode Warna Status

- 🔴 **Merah** → Menunggu
- 🟡 **Kuning** → Diproses  
- 🟢 **Hijau** → Selesai

---

## ⚠️ Troubleshooting

**"Network request failed"**
- Pastikan backend server sudah berjalan (`npm run dev`)
- Pastikan IP di `api.js` sudah benar
- Pastikan HP dan laptop di WiFi yang sama
- Coba matikan firewall Windows sementara

**"NIM sudah terdaftar"**
- NIM sudah ada di database. Gunakan NIM berbeda atau langsung login.

**Foto tidak tampil**
- Cek folder `backend/public/uploads/` — apakah file ada di sana?
- Pastikan URL `http://IP:5000/uploads/namafile.jpg` bisa dibuka di browser

---

## 📦 Tech Stack

| Layer | Teknologi | Versi |
|-------|-----------|-------|
| Mobile | React Native + Expo | SDK 57 |
| Navigation | React Navigation v7 | - |
| Backend | Node.js + Express.js | v5 |
| Database | MongoDB Atlas | Cloud |
| Auth | bcrypt | v6 |
| Upload | Multer | v2 |
| HTTP Client | Axios | - |
| Storage | AsyncStorage | - |
