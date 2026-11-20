# BrOwಗ

**Brownies Lembut, Keripik Ubi Mengembang—Dua Camilan Satu Tempat.**

Toko online brownies dan keripik ubi. Frontend statis menggunakan Cloudflare Workers Assets, API memakai Cloudflare Workers, dan data disimpan di Cloudflare D1.

## Fitur
- Katalog Brownies dan Keripik Ubi, pencarian, keranjang, checkout, dan pesanan WhatsApp.
- Registrasi/login pelanggan.
- Rating bintang 1–5 dan ulasan produk.
- Komplain pelanggan dengan percakapan balasan pelanggan/admin.
- Admin dan Super Admin; Super Admin dapat membuat akun admin lain.
- Moderasi akun: Teguran, SP1, SP2, Suspend sementara, Banned permanen, dan pemulihan akun.
- Riwayat tindakan moderasi per pelanggan.
- Kelola produk, stok, harga, pesanan, dan status pesanan.
- Upload resep/menu khusus admin; foto dapat dipilih dari galeri perangkat.
- Halaman admin terpisah di /admin.html.
- Katalog diperbarui otomatis setiap 15 detik tanpa reload.

## Setup / migrasi database
Repository ini sudah memiliki database D1 versi awal. Untuk mempertahankan data lama, **jangan jalankan schema.sql ulang**. Jalankan migrasi satu kali:

```sh
npx wrangler d1 execute dixonmyaz-db --remote --file=./migration_v2.sql
```

Pastikan `database_id` pada `wrangler.toml` sudah diisi dengan ID D1 yang benar.

## Login admin
Login Super Admin awal menggunakan secret Cloudflare Workers `ADMIN_PASSWORD` yang sudah dipakai versi sebelumnya:

```sh
npx wrangler secret put ADMIN_PASSWORD
```

Di halaman /admin.html, kosongkan username untuk masuk sebagai Super Admin awal. Setelah masuk, buat akun admin lain dari tab Admin. Admin tambahan masuk menggunakan username dan password masing-masing.

## Deploy
```sh
npx wrangler deploy
```

## Catatan penting
- Foto yang diunggah dari galeri saat ini disimpan sebagai data URL di D1 dan dibatasi 800 KB per gambar. Untuk katalog besar, pindahkan penyimpanan media ke Cloudflare R2.
- Pendaftaran menggunakan nomor WhatsApp dan password, belum memakai OTP.
- Pembayaran belum terintegrasi; konfirmasi pesanan dilakukan melalui WhatsApp.
- Stok perlu diperbarui admin setelah pesanan dikonfirmasi.
- Sebelum toko dibuka untuk publik, uji migrasi di database salinan, konfigurasi nomor WhatsApp, siapkan kebijakan privasi, rate limiting/CAPTCHA, dan lakukan pengujian keamanan.
