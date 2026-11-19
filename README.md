# DixonMyaz

Toko online brownies dan kerupuk lokal. Frontend statis disajikan oleh Cloudflare Workers Assets, API oleh Workers, database oleh D1.

## Fitur awal
- Katalog produk, pencarian, kategori, keranjang.
- Registrasi/login pelanggan dengan password PBKDF2 dan cookie HttpOnly.
- Checkout hanya untuk pelanggan login; harga dan stok diperiksa ulang di API.
- Pesanan disimpan di D1 lalu pembeli diarahkan ke WhatsApp.
- Endpoint admin terproteksi untuk produk, stok, dan status pesanan.
- Katalog diperbarui otomatis setiap 15 detik tanpa reload halaman.

## Setup
1. Buat D1 database bernama dixonmyaz-db.
2. Jalankan: npx wrangler d1 execute dixonmyaz-db --remote --file=./schema.sql
3. Isi database_id pada wrangler.toml.
4. Isi WHATSAPP_NUMBER dengan nomor toko format internasional tanpa tanda +.
5. Atur password admin: npx wrangler secret put ADMIN_PASSWORD
6. Deploy: npx wrangler deploy

## Produk awal
Tambahkan produk melalui endpoint admin atau SQL D1. Harga dan stok harus diisi sesuai kondisi toko. Jangan gunakan data contoh sebagai harga jual nyata.

## Endpoint admin
- POST /api/admin/login
- GET /api/admin/products
- POST /api/admin/products
- PUT /api/admin/products/:id
- DELETE /api/admin/products/:id (menonaktifkan produk)
- GET /api/admin/orders
- PATCH /api/admin/orders/:id

## Catatan
- Pendaftaran memakai nomor WhatsApp dan password, belum memakai OTP. Nomor pelanggan perlu dikonfirmasi manual oleh admin melalui WhatsApp.
- Belum ada pembayaran online; pembayaran dibicarakan melalui WhatsApp.
- Stok diperiksa saat checkout tetapi belum otomatis dikurangi saat order dibuat. Admin perlu memperbarui stok setelah pesanan dikonfirmasi.
- Tambahkan CAPTCHA/rate limiting dan kebijakan privasi sebelum promosi publik. Kelola data pelanggan seperlunya dan dengan pengawasan orang dewasa yang bertanggung jawab atas toko.
