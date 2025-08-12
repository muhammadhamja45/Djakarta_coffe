# Di Kopi Jakarta — Premium Coffee Experience

Website statis untuk brand kopi "Di Kopi Jakarta" dengan fitur katalog produk, keranjang belanja, dan checkout via WhatsApp. Dibangun menggunakan Tailwind CSS (CDN), AOS untuk animasi, dan SweetAlert2 untuk notifikasi.

## Fitur
- **Katalog produk**: Dua kategori (Kopi kemasan cup dan botol) dengan harga dan deskripsi singkat.
- **Keranjang belanja**: Tambah/kurangi jumlah, hapus item, total otomatis, badge jumlah item.
- **Checkout via WhatsApp**: Modal untuk input nama dan alamat; membuka WhatsApp dengan pesan terformat.
- **Navigasi responsif**: Navbar, menu mobile slide-in, overlay.
- **Animasi halus**: AOS pada berbagai elemen untuk pengalaman UI lebih baik.
- **Notifikasi**: SweetAlert2 saat produk ditambahkan ke keranjang dan setelah proses checkout.

## Teknologi yang digunakan
- Tailwind CSS (via CDN)
- AOS (Animate On Scroll)
- SweetAlert2
- Google Analytics (gtag.js)
- HTML, CSS, dan JavaScript murni (tanpa backend)

## Struktur Proyek
```
/workspace
├─ index.html              # Halaman utama (semua UI dan logika keranjang)
└─ img/                    # Aset gambar produk
   ├─ 3.jpg
   ├─ 4.jpg
   ├─ 5.jpg
   ├─ 6.jpg
   ├─ 7.jpg
   └─ 1                    # File tanpa ekstensi (tidak digunakan)
```

## Cara Menjalankan Secara Lokal
Karena ini website statis, Anda bisa langsung membuka `index.html` di browser. Untuk pengalaman yang lebih mirip produksi (terutama akses file lokal oleh browser), jalankan server statis:

- Python 3:
  ```bash
  python3 -m http.server 8080
  ```
  Lalu buka `http://localhost:8080` di browser.

- Node (opsional, jika ada):
  ```bash
  npx serve .
  ```

## Cara Deploy
- **GitHub Pages**: Commit file ke repository publik dan aktifkan Pages pada branch `main` (folder root).
- **Netlify/Vercel**: Deploy sebagai proyek statis, `index.html` sebagai entri.
- **Hosting statis lain**: Unggah seluruh isi folder ke hosting.

## Penjelasan Alur Utama
- Data keranjang disimpan di memori (variabel `cart` di `index.html`), tidak ada penyimpanan permanen.
- Tombol "Tambah" pada tiap produk memiliki atribut `data-*` yang dibaca untuk membentuk item keranjang:
  - `data-id`, `data-name`, `data-price`, `data-image`
- Drawer keranjang bisa dibuka dari navbar (desktop) atau menu mobile.
- Tombol "Pesan via WhatsApp" membuka modal checkout untuk input **Nama** dan **Alamat**.
- Saat submit, akan membuka WhatsApp ke nomor yang ditentukan dengan pesan terformat yang memuat rincian pesanan dan total.

## Konfigurasi Penting
- **Nomor WhatsApp**: Ubah di `index.html` (fungsi submit form checkout)
  - Cari baris yang membentuk URL seperti berikut:
    ```js
    const whatsappURL = `https://wa.me/628889275189?text=${whatsappMessage}`;
    ```
  - Ganti `628889275189` dengan nomor bisnis Anda (format internasional tanpa `+`).

- **Google Analytics (gtag.js)**: Ubah ID di `<head>`
  ```html
  <script async src="https://www.googletagmanager.com/gtag/js?id=G-HEW6V6SN7L"></script>
  <script>
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);} 
    gtag('js', new Date());
    gtag('config', 'G-HEW6V6SN7L');
  </script>
  ```
  Ganti `G-HEW6V6SN7L` dengan GA Measurement ID milik Anda.

- **Gambar produk di keranjang**: Pastikan atribut `data-image` pada tombol "Tambah" menunjuk ke file gambar yang valid di folder `img/`.
  - Saat ini, beberapa produk menggunakan placeholder `/api/placeholder/120/120` yang akan 404 pada hosting statis. Ubah menjadi, misalnya:
    ```html
    data-image="img/3.jpg"
    ```

- **Kontak di Footer**: Ubah teks di bagian Footer untuk nomor, email, dan lokasi sesuai bisnis Anda.

## Menambah/Mengubah Produk
1. Buka `index.html` dan pergi ke bagian "Menu Kami".
2. Duplikasi blok produk dengan kelas `product-card`.
3. Sesuaikan:
   - Gambar: `img/*.jpg`
   - Nama: teks judul dalam `<h4>`
   - Deskripsi: paragraf kecil di bawah judul
   - Harga: teks harga dan `data-price` (dalam rupiah tanpa tanda titik/koma)
   - ID unik: `data-id` (hindari duplikasi)
   - Gambar untuk keranjang: `data-image` (pastikan path valid)

Contoh atribut tombol "Tambah":
```html
<button 
  class="add-to-cart ..." 
  data-id="cup-1" 
  data-name="Kopi Cup Original" 
  data-price="15000" 
  data-image="img/3.jpg">
  Tambah
</button>
```

## Praktik Terbaik
- Gunakan gambar terkompresi agar halaman cepat dimuat.
- Konsistenkan format harga: simpan sebagai angka (misalnya `18000`) dan format tampilan memakai `Intl.NumberFormat('id-ID', { style: 'currency', currency: 'IDR' })` yang sudah digunakan.
- Pastikan setiap `data-id` unik untuk mencegah benturan saat update jumlah/hapus item.
- Perbarui meta `<title>` dan deskripsi (opsional, tambahkan `<meta name="description" ...>`) untuk SEO.

## Keterbatasan Saat Ini
- Tidak ada backend; semua state keranjang berada di memori dan akan hilang saat reload.
- Tidak ada integrasi payment gateway; checkout hanya melalui WhatsApp.
- Tidak ada manajemen stok/varian.

## Lisensi
Hak cipta pemilik brand. Jika ingin dijadikan open-source, tambahkan file LICENSE sesuai kebutuhan (mis. MIT).

## Kontak
- Telepon/WhatsApp: ubah nomor di `index.html` sesuai kebutuhan.
- Email: `info@kopiDjakarta.com` (sesuaikan di footer).
- Lokasi: Di Jakarta, Indonesia (sesuaikan di footer).