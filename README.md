# 🚗 AyoRental — Premium Car Marketplace & Rental Portal

AyoRental adalah platform all-in-one untuk pencarian, penyewaan, penjualan, dan simulasi pembiayaan mobil premium. Dibangun sebagai **Single Page Application (SPA)** dengan antarmuka modern, interaktif, dan terintegrasi penuh dengan backend real-time **Supabase**.

---

## ✨ Fitur Utama

- **Advanced Search Widget** — Filter mobil interaktif langsung di area Hero, dengan tab dinamis antara Search Form dan Visual Body Type Grid.
- **Dedicated Browse Page (`#/browse`)** — Pencarian & filter lanjutan (nama, merk, tipe bodi, transmisi, bahan bakar, harga) yang terhubung langsung ke database.
- **Interactive Favorite System (`#/favorites`)** — Sistem like/favorit yang aman dengan Row Level Security (RLS) Supabase.
- **Seller Dashboard (`#/dashboard-seller`)** — Dashboard penjual untuk mempublikasikan atau menghapus listing mobil secara instan.
- **Instant Pre-Approval Form** — Formulir pengajuan kredit interaktif yang langsung menyimpan data ke database.
- **Client-Side Hash Router** — Navigasi halaman instan tanpa page reload, ramah untuk hosting statis.
- **Custom Alert & Modal System** — Notifikasi melayang kustom dengan z-index management yang bebas bug.

---

## 🛠️ Tech Stack

| Layer | Teknologi |
|---|---|
| Frontend | Vanilla HTML5, CSS3, Tailwind CSS v3 (CDN) |
| Icons | Font Awesome v6.4.0 (CDN) |
| Backend | Supabase (Postgres, Auth, RLS) |
| Deployment | GitHub Pages |

---

## ⚙️ Cara Setup Mandiri

### 1. Persiapan Database (Supabase)

1. Buat akun gratis dan proyek baru di [supabase.com](https://supabase.com).
2. Buka **SQL Editor** di dashboard Supabase.
3. Klik **+ New Query**, tempelkan semua isi dari file `trigger_patch.sql`, lalu klik **Run**.
4. Pastikan eksekusi berhasil: `Success. No rows returned`.

### 2. Konfigurasi Autentikasi

Agar registrasi akun bisa langsung aktif tanpa konfirmasi email:

1. Buka **Authentication → Providers → Email** di dashboard Supabase.
2. Matikan toggle **Confirm email**.
3. Klik **Save**.

### 3. Hubungkan Frontend ke Backend

Buka `index.html` dan ganti kredensial Supabase berikut dengan milik Anda:

```js
const SUPABASE_URL = "https://YOUR_PROJECT_ID.supabase.co";
const SUPABASE_ANON_KEY = "YOUR_ANON_KEY";
```

Kredensial dapat ditemukan di **Project Settings → API** pada dashboard Supabase Anda.

---

## ☁️ Deploy ke GitHub Pages

1. Buat repositori **Public** baru di GitHub.
2. Upload file `index.html` dan `README.md` ke repositori.
3. Buka **Settings → Pages**.
4. Pada bagian **Build and deployment**:
   - Source: `Deploy from a branch`
   - Branch: `main` / `master`, folder: `/ (root)`
5. Klik **Save**, tunggu 1–2 menit.
6. Situs Anda akan live di: `https://username.github.io/nama-repo/`

---

## 📄 Lisensi

Didistribusikan di bawah lisensi **MIT**. Bebas digunakan, dipelajari, dan dimodifikasi untuk keperluan portofolio.

---

Dibuat dengan 💙 oleh tim AyoRental.
