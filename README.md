# Warung Tarka 02

Aplikasi pemesanan makanan berbahasa Indonesia dengan katalog, keranjang, checkout, pelacakan pesanan, dan dashboard admin yang responsif.

## Menjalankan secara lokal

1. Pastikan Node.js 18 atau lebih baru tersedia.
2. Jalankan `npm install`.
3. Salin `.env.example` menjadi `.env` (opsional untuk mode demo).
4. Jalankan `npm run dev`, lalu buka alamat yang ditampilkan Vite.

Tanpa konfigurasi Supabase, aplikasi berjalan langsung dalam mode demo. Menu, pengaturan, dan pesanan disimpan pada browser yang sedang digunakan. Login admin demo memakai password `tarka02` (atau nilai `VITE_ADMIN_PASSWORD` di `.env`); mode ini hanya untuk demo lokal dan bukan autentikasi yang aman untuk produksi.

## Supabase untuk deployment

1. Buat proyek Supabase dan isi `VITE_SUPABASE_URL` serta `VITE_SUPABASE_ANON_KEY` di `.env`.
2. Jalankan isi [`supabase/schema.sql`](./supabase/schema.sql) melalui SQL Editor Supabase.
3. Buat pengguna admin dari Supabase Dashboard → Authentication → Users dan nonaktifkan pendaftaran publik. Tambahkan UUID pengguna itu ke `public.admin_users` melalui SQL Editor agar hanya akun tersebut yang dapat mengelola warung.
4. Jalankan [`supabase/seed.sql`](./supabase/seed.sql) untuk memasukkan 14 menu contoh dan pengaturan awal.
5. Deploy ke Vercel atau Netlify dengan kedua variabel environment Supabase yang sama.

Supabase Auth dan daftar `admin_users` melindungi perubahan menu, pengaturan, foto, dan status pesanan. Aktifkan Realtime untuk tabel `orders` dari Supabase Database → Publications agar notifikasi pesanan baru muncul langsung di dashboard. Pembuatan pesanan, pembacaan katalog/pengaturan, dan pelacakan status dengan nomor pesanan tersedia bagi pelanggan tanpa login. Mode demo menyimpan data hanya pada browser lokal.

## Mengubah identitas dan menu

- Nomor WhatsApp, jam buka, ongkos kirim, rekening, QRIS, dan waktu persiapan dapat diubah dari **Admin → Pengaturan**. Nomor WhatsApp harus memakai kode negara tanpa tanda `+`, misalnya `6281234567890`.
- Nama dan tagline dapat diedit di awal `src/data.js` (`initialSettings`); ubah juga seed `shop_settings` Supabase jika memakai backend.
- Menu awal beserta harga, kategori, stok, foto, dan tambahan dapat diedit di `src/data.js`, atau dikelola langsung dari halaman **Admin → Menu**.
- Logo menggunakan teks nama warung di header, sehingga perubahan nama otomatis memperbarui logo.

## Catatan operasional

Supabase mode memerlukan koneksi internet dan menampilkan kesalahan koneksi secara eksplisit. Nomor pesanan dikirim ke WhatsApp pemilik melalui tautan `wa.me` setelah checkout; pengiriman WhatsApp dilakukan oleh pelanggan dari perangkatnya. Simpan file gambar menu yang diunggah di ukuran wajar.
