# Rest API
Kelas PBO 2025, pertemuan ke-13 (7 Nov)

## 1. Clone repo
```bash
git clone https://github.com/leo42night/rest-api
```
**Buat Repository di GitHub:** Tempat simpan proyek agar vercel dapat koneksi.
```bash
git remote add repoku https://github.com/<username>/<repo> 
# buat sebuah perubahan di proyek agar bisa submit commit baru
git add .
git commit -m "persiapan sebelum deploy"
git push repoku main --force
```

## 2. Config
1. Install PHP & Database
2. Run Database & PHP Server `php -S localhost:3001` (port dapat disesuaikan)
3. Jalankan `db.sql`

## Rute API
Ini adalah beberapa rute API yang tersedia:
```bash
GET / → { "message": "Koneksi success" }
GET /mahasiswa
GET /mahasiswa/1
POST /mahasiswa (body JSON)
PUT /mahasiswa/1 (body JSON)
DELETE /mahasiswa/1
```

## 3. Test API (sesuaikan path)
Path bisa di http local, atau link https deployment (vercel). ganti aja url nya.

### Menggunakan Terminal (pakai terminal yang basis Unix: Git Bash)

- POST (create) data mahasiswa baru.
```bash
curl -X POST http://localhost:3001/mahasiswa \
-H "Authorization: Bearer 12345ABCDEF" \
-H "Content-Type: application/json" \
-d '{
  "nama": "Andi Saputra",
  "jurusan": "Teknik Informatika"
}'
```
Respon Berhasil:
```json
{
  "message": "Data mahasiswa berhasil ditambahkan"
}
```
API Lainnya:

- GET (ambil) Index halaman
```bash
curl -X GET http://localhost:3001/
```

- GET (ambil) semua daya mahasiswas
```bash
curl -X GET http://localhost:3001/mahasiswa \
-H "Authorization: Bearer 12345ABCDEF"
```

- GET (ambil) mahasiswa
```bash
curl -X GET http://localhost:3001/mahasiswa/1 \
-H "Authorization: Bearer 12345ABCDEF"
```

- PUT (update) mahasiswa data
```bash
curl -X PUT http://localhost:3001/mahasiswa/1 \
-H "Authorization: Bearer 12345ABCDEF" \
-H "Content-Type: application/json" \
-d '{
  "nama": "Ahmad Syahroni",
  "jurusan": "Tata Boga"
}'
```

- DELETE (hapus) mahasiswa data
```bash
curl -X DELETE http://localhost:3001/mahasiswa/1 \
-H "Authorization: Bearer 12345ABCDEF"
```

### Alternatif
- Postman (Aplikasi)
- Thunder Client (Ekstensi VSCode)
- EchoAPI for VS Code (Ekstensi VSCode) **[🌟 Disarankan]**

## 4. Deployment

### Vercel
Vercel bisa menjalankan PHP lewat custom runtime open-source bernama [vercel-php](https://github.com/vercel-community/php)
### a. **Persiapkan file konfigurasi Vecel:**
```
/api
  ├── index.php
vercel.json
```
- **api/index.php**
```php
<?php  

require __DIR__ . "/../index.php";
```

- **vercel.json**
```json
{
  "functions": {
    "api/*.php": {
      "runtime": "vercel-php@0.7.4"
    }
  },
  "routes": [
    { "src": "/(.*)",  "dest": "/api/index.php" }
  ]
}
```
### b. Install Vercel CLI  
Anda perlu [install NPM](https://www.google.com/search?q=install+node+package+manager+di+windows) (Node Package Manager) lebih dulu
```bash
npm install -g vercel
```
### c. Login ke akunmu (pakai Github)
```bash
vercel login
```
### d. Deploy proyek
```bash
vercel
```
Akan ada konfigurasi (tekan saja enter):
```bash
? Set up and deploy “c:\oop\rest-api”? yes
? Which scope do you want to deploy to? leo42night's projects
? Link to existing project? no
? What’s your project’s name? rest-api
? In which directory is your code located? ./
? Want to modify these settings? no
```

Vercel akan otomatis mendeteksi file **vercel.json** dan menggunakan runtime @vercel/php.
Setelah selesai, kamu akan dapat URL publik `Domains` singkat seperti (lihat pada halaman proyek vercel):
```bash
https://rest-api.vercel.app
```

### e. **Database Postgres**

Di halaman proyek vercel kalian, buka stores (contoh: **vercel.com/_nama-teams_/_nama-proyek_/stores**)
- [Create Database]
  - Pilih Provider: `Supabase (Postgres backend)`
  - Region: `Singapore`
  - Public Env Variabel Prefix: `PG_` (bebas kasih prefix, ini biar rapi aja)
  - Instalation Plans: Free
- [Continue]
  - Database Name: `supabase-rest-api` (bebas kasih nama)
- [Create] (akan memakan waktu load)
- Selagi menunggu, buat file `.env.local` di folder proyek.
- di web ketika selesai create, akan ada popup untuk koneksi ke supabase, langsung click [Connect] agar masuk ke config supabase.
- Salin isi `.env.local` yang ada di config supabase ke `env.local` di folder proyek.
- di halaman cnfig supabase, klik [Open In Supabase].
- [Table Editor] -> [New table]
  - **Name**: **"mahasiswa"** (nama tabel)
  - Isi Kolom: 
    - **"id"** (type [int], primary [check])
    - **"created_at"** (type [timestamps], default [now()])
    - **"jurusan"** (type [varchar])

- lihat kembali `env.local`, terdapat variabel dengan struktur `POSTGRES_URL_NON_POOLING`:`postgres://<username>:<password>@<host>:<port>/<database>?<options>`.

- **(dalam proyek vercel) Buka Settings > Environment Variables** (url nya seperti ini **vercel.com/_nama-teams_/_nama-proyek_/settings/environment-variables**): Set variabel database berikut, sesuaikan dengan isi variabel `...URL_NON_POOLING` sebelumnya.  
```bash
DB_TYPE: pgsql
DB_HOST: aws-1-ap-southeast-1.pooler.supabase.com
DB_PORT: 5432 # pakai port NON_POOLING
DB_USER: postgres.itzxopxshpvcjotxxxxx
DB_PASS: ETnWsKT1Q9xxxxx
DB_SSLMODE: require
```

- Jika sudah selesai setup variabel DB di vercel, sekarang coba deploy ulang (tombolnya akan muncul setelah ada perubahan variabel).

- (opsional) anda dapat akses database Supabase (PostgreSQL) pakai HeidiSQL, ikuti config sebelumya, dan pakai config ini:
  - Network Type: **PostgreSQL (TCP/IP)**
  - Library: **libpq-12.dll**

## 5. Test API link Deployment

- POST (create) data mahasiswa baru.
```bash
curl -X POST http://<url-domain>.vercel.app/mahasiswa \
-H "Authorization: Bearer 12345ABCDEF" \
-H "Content-Type: application/json" \
-d '{
  "nama": "Andi Saputra",
  "jurusan": "Teknik Informatika"
}'
```
Respon Berhasil:
```json
{
  "message": "Data mahasiswa berhasil ditambahkan"
}
```
API Lainnya:

- GET (ambil) Index halaman
```bash
curl -X GET http://<url-domain>.vercel.app/
```

- GET (ambil) semua daya mahasiswas
```bash
curl -X GET http://<url-domain>.vercel.app/mahasiswa \
-H "Authorization: Bearer 12345ABCDEF"
```

- GET (ambil) mahasiswa
```bash
curl -X GET http://<url-domain>.vercel.app/mahasiswa/1 \
-H "Authorization: Bearer 12345ABCDEF"
```

- PUT (update) mahasiswa data
```bash
curl -X PUT http://<url-domain>.vercel.app/mahasiswa/1 \
-H "Authorization: Bearer 12345ABCDEF" \
-H "Content-Type: application/json" \
-d '{
  "nama": "Ahmad Syahroni",
  "jurusan": "Tata Boga"
}'
```

- DELETE (hapus) mahasiswa data
```bash
curl -X DELETE http://<url-domain>.vercel.app/mahasiswa/1 \
-H "Authorization: Bearer 12345ABCDEF"
```