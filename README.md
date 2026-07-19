🚗 AyoRental - Premium Car Marketplace & Rental Portal

AyoRental adalah platform pencarian, penyewaan, penjualan, dan simulasi pembiayaan mobil premium dalam satu halaman aplikasi (Single Page Application). Aplikasi ini didesain menggunakan antarmuka modern, interaktif, bersih, serta terintegrasi penuh dengan database backend real-time Supabase.

🚀 Fitur Utama

Advanced Search Widget: Fitur penyaringan mobil interaktif langsung di area Hero, lengkap dengan tab pencarian dinamis (Search Form vs Visual Body Type Grid).

Dedicated Browse Page (#/browse): Halaman khusus pencarian mobil yang mendukung penyaringan tingkat lanjut (Pencarian nama, Merk, Tipe Bodi, Transmisi, Bahan Bakar, Harga Maksimal) dan pengurutan (sorting) langsung dari database.

Interactive Favorite System (#/favorites): Sistem penanda favorit (like) yang terhubung ke database Supabase dan diamankan lewat Row Level Security (RLS).

Seller Dashboard (#/dashboard-seller): Dashboard khusus bagi penjual untuk memublikasikan iklan mobil baru ke sistem global secara instan atau menghapus listing miliknya yang sudah laku.

Instant Pre-Approval Application: Formulir pengajuan kredit mobil interaktif yang langsung menyimpan data kelayakan finansial pemohon ke database.

Client-Side Hash Router: Routing halaman yang instan tanpa memuat ulang browser, ramah terhadap deployment di layanan hosting statis gratis.

Custom Alert & Modal Layering: Notifikasi melayang kustom dengan pengamanan tingkat tumpukan CSS (z-index) optimal yang bebas dari bug tertimpa elemen lain.

🛠️ Tech Stack & Integrasi

Frontend: Vanilla HTML5, CSS3, Tailwind CSS (v3 CDN with custom config).

Icons: Font Awesome Icons (v6.4.0 via CDN).

Backend / Database: Supabase (Postgres Database, Supabase Auth, Row Level Security Policies).

Deployment: GitHub Pages (Client-side static deployment).

⚙️ Cara Pemasangan Mandiri (Self-Hosting)

1. Persiapan Database (Supabase)

Buat akun gratis dan sebuah proyek baru bernama AyoRental di Supabase.

Masuk ke dashboard proyek Supabase Anda, buka menu SQL Editor di bilah menu kiri.

Klik + New query, tempelkan semua kode dari berkas trigger_patch.sql (atau inisialisasi database Anda), lalu klik Run (Ctrl/Cmd + Enter).

Pastikan status eksekusi berhasil (Success. No rows returned). Tabel dan kebijakan keamanan (RLS) kini telah aktif di database Anda.

2. Pengaturan Autentikasi Pengguna (Penting)

Demi kelancaran demo live pendaftaran akun tanpa konfigurasi server email (SMTP):

Buka dashboard proyek Supabase Anda → Authentication → Providers → klik Email.

Cari opsi Confirm email dan matikan (disable) toggle tersebut.

Klik Save.
(Dengan mematikan opsi ini, proses pendaftaran akun baru di AyoRental akan otomatis langsung aktif tanpa harus menunggu konfirmasi email).

3. Hubungkan Kode Frontend dengan Backend Anda

Buka berkas index.html menggunakan editor kode kesayangan Anda (VS Code, Notepad, dll).

Cari baris kredensial Supabase di bagian bawah tag <script>:

const SUPABASE_URL = "https://femiavcsreumbmuhaegp.supabase.co";
const SUPABASE_ANON_KEY = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImZlbWlhdmNzcmV1bWJtdWhhZWdwIiwicm9sZSI6ImFub24iLCJpYXQiOjE3ODQzOTY5NDMsImV4cCI6MjA5OTk3Mjk0M30.K6-hFKQ0EuaK9rGEjTAP7aXwgUhbs-tLJCljVJ23Cfw";


Gantikan kedua nilai di atas dengan kredensial proyek Supabase milik Anda sendiri yang dapat diambil dari menu Project Settings → API di dashboard Supabase Anda.

Simpan perubahan berkas index.html Anda.

☁️ Cara Online-kan Menggunakan GitHub Pages

Proyek ini berbasis client-side murni, sehingga Anda dapat menghosting aplikasi AyoRental ini secara online secara gratis seumur hidup melalui GitHub Pages:

Buat repositori baru di akun GitHub Anda (Pastikan visibilitas diatur ke Public).

Unggah file index.html dan README.md Anda ke dalam repositori tersebut.

Di halaman repositori GitHub Anda, klik tab Settings (ikon gerigi).

Pada navigasi kiri, cari menu Code and automation → klik Pages.

Pada bagian Build and deployment:

Source: Pilih Deploy from a branch.

Branch: Pilih branch main (atau master) dan folder / (root).

Klik Save.

Tunggu 1–2 menit, lalu segarkan (refresh) halaman tersebut.

Tautan website live Anda akan muncul di bagian atas dalam kotak hijau, contoh:

"Your site is live at https://username_github_anda.github.io/drivenest/"

📄 Lisensi

AyoRental didistribusikan sebagai proyek open-source di bawah lisensi MIT. Silakan gunakan, pelajari, dan modifikasi untuk keperluan portofolio Anda secara bebas!

Dibuat dengan 💙 oleh tim AyoRental.
