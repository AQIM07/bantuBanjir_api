# Bantu Banjir API

API backend untuk aplikasi Bantu Banjir, yang digunakan untuk melaporkan kondisi banjir di suatu lokasi. Aplikasi ini dibangun menggunakan Node.js, Express, dan Prisma ORM dengan database PostgreSQL melalui Supabase.

## Fitur Utama

- Registrasi dan login pengguna
- Membuat laporan banjir dengan lokasi, koordinat, tingkat air, deskripsi, dan gambar
- Melihat daftar laporan
- Update dan hapus laporan (untuk pengguna yang membuat)
- Upload gambar ke Supabase Storage

## Prerequisites

Sebelum menjalankan proyek ini, pastikan Anda memiliki:

- Node.js (versi 16 atau lebih baru)
- npm (biasanya sudah terinstall dengan Node.js)
- Akun Supabase untuk database dan storage
- Git (untuk clone repository)

## Instalasi dan Setup

### 1. Clone Repository

```bash
git clone <url-repository>
cd bantuBanjir-api
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Setup Environment Variables

Buat file `.env` di root directory dan isi dengan variabel berikut:

```
DATABASE_URL="postgresql://[username]:[password]@[host]:[port]/[database]?pgbouncer=true"
DIRECT_URL="postgresql://[username]:[password]@[host]:[port]/[database]"
SERVICE_ROLE="[supabase-service-role-key]"
SUPABASE_URL="https://[supabase-project-id].supabase.co"
JWT_SECRET="[random-secret-string]"
```

**Catatan:** Ganti nilai-nilai placeholder dengan kredensial Supabase Anda. Untuk JWT_SECRET, gunakan string acak yang aman.

### 4. Setup Database

Jalankan migrasi Prisma untuk membuat tabel di database:

```bash
npx prisma migrate deploy
```

### 5. Generate Prisma Client

```bash
npx prisma generate
```

### 6. Jalankan Server

```bash
npm start
```

Server akan berjalan di `http://localhost:5000` (atau port yang ditentukan di environment variable PORT).

## API Endpoints

### Authentication

- `POST /api/auth/register` - Registrasi pengguna baru
  - Body: `{ "name": "string", "email": "string", "password": "string" }`
- `POST /api/auth/login` - Login pengguna
  - Body: `{ "email": "string", "password": "string" }`

### Reports

- `GET /api/reports` - Mendapatkan semua laporan
- `GET /api/reports/total-user` - Mendapatkan total pengguna
- `POST /api/reports` - Membuat laporan baru (butuh autentikasi)
  - Headers: `Authorization: Bearer [token]`
  - Body: `{ "location": "string", "coordinates": { "lat": number, "lng": number }, "waterLevel": number, "description": "string" }`
  - File: `image` (opsional)
- `PUT /api/reports/:id` - Update laporan (butuh autentikasi, hanya pemilik)
  - Headers: `Authorization: Bearer [token]`
  - Body: sama dengan POST
- `DELETE /api/reports/:id` - Hapus laporan (butuh autentikasi, hanya pemilik)
  - Headers: `Authorization: Bearer [token]`

## Teknologi yang Digunakan

- **Backend:** Node.js, Express.js
- **Database:** PostgreSQL (via Supabase)
- **ORM:** Prisma
- **Authentication:** JWT (JSON Web Token)
- **Storage:** Supabase Storage untuk gambar
- **Deployment:** Vercel

## Deployment

Proyek ini dikonfigurasi untuk deployment di Vercel. File `vercel.json` sudah disediakan untuk konfigurasi deployment.

Untuk deploy:

1. Push kode ke repository GitHub
2. Connect repository ke Vercel
3. Set environment variables di Vercel dashboard
4. Deploy

## Troubleshooting

- Jika ada error terkait database, pastikan DATABASE_URL dan DIRECT_URL benar
- Untuk upload gambar, pastikan bucket "banjirImage" sudah dibuat di Supabase Storage
- Jika server tidak start, cek apakah port 5000 sudah digunakan

## Kontribusi

Untuk berkontribusi, silakan buat issue atau pull request di repository ini.

## Lisensi

[Masukkan lisensi jika ada]
