# DEBUG LOG — Latihan P10 Katalog Karya
---
BUG-01: Logo Navbar Hilang (G1)
- Gejala: Ikon logo di bagian navbar atas gagal dimuat dan menampilkan ikon rusak (broken image).
- Langkah Reproduksi:Buka halaman `index.html` di browser, lalu amati bagian pojok kiri atas navbar.
- Hipotesis Awal: Berkas SVG logo rusak atau korup. *(Gugur: Setelah dicek langsung, berkas logo.svg valid dan ada di dalam direktori proyek).*
- Hipotesis Kedua: Path atau alamat direktori berkas gambar di dalam kode HTML salah. *(Terbukti benar).*
- Bukti DevTools: Tab **Network** menunjukkan status `404 Not Found` pada permintaan berkas `img/logo.svg`.
- Perbaikan:** 
  - Berkas: `index.html` (Baris ~12)
  - Perubahan: Mengubah atribut `src="img/logo.svg"` menjadi `src="assets/logo.svg"`.
- Uji Ulang: Muat ulang halaman, logo berhasil tampil dengan sempurna dan tab Network menunjukkan status `200 OK`.

---

BUG-02: Tata Letak Kartu Karya Tersusun Satu Kolom Memanjang (G2)
- Gejala: Kartu-kartu katalog karya berjejer menumpuk secara vertikal menjadi satu kolom meskipun layar dibesarkan.
- Langkah Reproduksi:Buka halaman web pada layar berukuran desktop/laptop, perhatikan bagian elemen galeri.
- Hipotesis: Elemen pembungkus galeri kehilangan class grid sistem CSS Bootstrap. *(Terbukti benar).*
- Bukti DevTools:Tab **Elements** menunjukkan elemen `#galeri` tidak memiliki pengatur tata letak kolom berbasis baris.
- Perbaikan: 
  - Berkas: `index.html` (Baris ~31)
  - Perubahan: Menambahkan class `row` pada elemen galeri `<div id="galeri" class="row g-3"></div>`.
- Uji Ulang:Tampilan kartu berubah menjadi responsif dan tersusun rapi dalam format grid multi-kolom.

---

## BUG-03: Filter Kategori Mati dan Menyebabkan Error di Console (G3)
- Gejala: Pemilihan kategori pada dropdown tidak berfungsi menyaring karya, dan muncul pesan error merah di Console.
- Langkah Reproduksi: Ubah pilihan pada dropdown filter kategori.
- Hipotesis: Variabel atau pemilih elemen DOM untuk event listener salah sasaran sehingga bernilai `null`. *(Terbukti benar).*
- Bukti DevTools:** Tab **Console** menampilkan pesan `Uncaught TypeError: Cannot read properties of null (reading 'addEventListener')`.
- Perbaikan: 
  - Berkas: `main.js` (Baris akhir)
  - Perubahan: Mengubah selector `#filter-kategori` yang salah menjadi pemanggilan variabel yang benar (`filterKategori.addEventListener('change', tampil);`).
- Uji Ulang:Dropdown kategori dapat merespons perubahan dan memicu fungsi penyaringan data.

---

## BUG-04: Teks Jumlah Karya Tidak Dinamis (G4)
- Gejala: Teks informasi yang menampilkan jumlah karya selalu statis menampilkan total keseluruhan (misal: "Menampilkan 6 dari 6 karya") meskipun sudah difilter.
- Langkah Reproduksi:Lakukan filter kategori tertentu, lalu lihat teks statistik di bawah filter.
- Hipotesis: Fungsi perender menghitung panjang data berdasarkan array total (`KARYA.length`) alih-alih array hasil saringan (`hasil.length`). *(Terbukti benar).*
- Bukti DevTools: Inspeksi elemen teks statistik menunjukkan nilai properti panjang array yang digunakan selalu merujuk ke data penuh.
- Perbaikan:
  - Berkas: `main.js` (Baris ~39)
  - Perubahan: Mengubah baris teks menjadi `jumlah.textContent = \`Menampilkan \${hasil.length} dari \${KARYA.length} karya\`;`.
- Uji Ulang: Teks jumlah berubah secara dinamis mengikuti jumlah data yang berhasil disaring.

---

## BUG-05: Tombol Detail Mati Setelah Melakukan Pencarian (G5)
- Gejala: Tombol "Detail" pada kartu karya tidak bisa diklik atau tidak berfungsi setelah pengguna melakukan pencarian judul atau filter kategori.
- Langkah Reproduksi: Ketik sesuatu di kolom pencarian, lalu klik tombol "Detail" pada kartu yang muncul.
- Hipotesis: Event listener tombol terhapus saat elemen galeri dirender ulang. *(Terbukti benar).*
- Bukti DevTools: Elemen kartu diperbarui secara dinamis oleh fungsi `replaceChildren`, namun fungsi pemasangan event detail (`pasangDetail()`) tidak dipanggil ulang.
- Perbaikan: 
  - Berkas: `main.js` (Di dalam fungsi `tampil()`)
  - Perubahan: Menambahkan pemanggilan fungsi `pasangDetail()` tepat di bawah perbaruan galeri.
- Uji Ulang: Tombol detail kembali berfungsi Normal meskipun galeri telah disaring atau dicari.

---

## BUG-06: Pencarian Bersifat Case-Sensitive (G6)
- Gejala: Pencarian judul karya gagal menemukan hasil jika pengguna mengetik menggunakan huruf kecil (misal mengetik "animasi" tidak memunculkan "Animasi Logo Himpunan").
- Langkah Reproduksi: Ketik teks dengan huruf kecil di kolom pencarian yang judul aslinya mengandung huruf kapital.
- Hipotesis: Perbandingan string JavaScript dilakukan secara langsung tanpa menyamakan format kapitalisasinya. *(Terbukti benar).*
- Bukti DevTools: Fungsi filter `includes` mengembalikan nilai salah karena perbedaan besar/kecil huruf.
- Perbaikan: 
  - Berkas: `main.js` (Fungsi `saring()`)
  - Perubahan: Menambahkan method `.toLowerCase()` pada kata kunci pencarian dan judul data (`k.judul.toLowerCase().includes(kata)`).
- Uji Ulang: Pencarian menjadi fleksibel (*case-insensitive*) dan berhasil menemukan data terlepas dari penggunaan huruf kapital.