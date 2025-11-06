# Rest API
Kelas PBO 2025, pertemuan ke-13 (7 Nov)

## Clone repo
```bash
git clone https://github.com/leo42night/rest-api
```

## Config
1. Install PHP & Database
2. Run Database & PHP Server `php -S localhost:3000` (port dapat disesuaikan)

## Rute Di Akses
```bash
GET / → { "message": "Koneksi success" }
GET /mahasiswa
GET /mahasiswa/1
POST /mahasiswa (body JSON)
```

## Test (sesuaikan path)

### Menggunakan Terminal (pakai terminal yang basis Unix: Git Bash)

```bash
curl -X POST http://localhost:8000/mahasiswa \
-H "Authorization: Bearer 12345ABCDEF" \
-H "Content-Type: application/json" \
-d '{
  "nama": "Andi Saputra",
  "jurusan": "Teknik Informatika"
}'
```

respon Berhasil:
```json
{
  "message": "Data mahasiswa berhasil ditambahkan"
}
```

### Alternatif
- Postman (Aplikasi)
- Thunder Client (Ekstensi VSCode)

## Deployment

### Render
1. **Tambahkan composer.json:** Render akan mengenali aplikasi PHP kalau ada file composer.json. Jika belum ada:
```json
{
  "name": "pbo13/rest-api-php",
  "description": "REST API PHP Native untuk belajar CRUD",
  "require": {
    "php": ">=8.1",
    "ext-pdo": "*"
  },
  "scripts": {
    "start": "php -S 0.0.0.0:10000"
  }
}
```
2. **Buat Repository di GitHub:** Tempat simpan proyek
```bash
git add remote myrepo https://github.com/<username>/<repo>
git add .
git commit -m "persiapan sebelum deploy"
gut push myrepo main --force
```
3. **Deploy ke Render**
- Buka [render.com](https://render.com)
- Login / Daftar (bisa pakai GitHub)
- Klik **New → Web Service**
- Pilih GitHub repo kamu (rest-api-php)
- Isi konfigurasi:
  - **Environment**: `PHP`
  - **Region**: Singapore (disarankan untuk Asia)
  - **Build Command**: `composer install`
  - **Start Command**: `php -S 0.0.0.0:10000`
  - **Port**: Render otomatis akan arahkan port `10000`
- Klik **Create Web Service**
4. **Menyambung Database**
- Opsi A: Gunakan MySQL di Planetscale
  - Buat akun di [Planetscale.com](https://planetscale.com/)
  - Buat database baru
  - Dapatkan kredensial:
```yaml
host: aws.connect.planetscale.com
port: 3306
username: user123
password: abcd1234
database: kampus_db
```
5. Tambahkan ke Render → Dashboard → Environment Variables
```ini
DB_HOST=aws.connect.planetscale.com
DB_PORT=3306
DB_NAME=kampus_db
DB_USER=user123
DB_PASS=abcd1234

```
6. Testing API: Setelah deploy, Render akan memberi URL seperti
```
https://rest-api-php.onrender.com
```