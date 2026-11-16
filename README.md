# MyPortfolio Mobile

Aplikasi mobile Flutter untuk portofolio pribadi Fildza Hasnalia Nabila. Aplikasi ini adalah klien mobile dari website portofolio Django (`myportofolio`) dan nantinya akan terhubung ke REST API `/api/v1/` yang dibuat di Tutorial 07 dan Tugas 6.

- **Nama:** Fildza Hasnalia Nabila
- **NPM:** 2506625003
- **Kelas:** PBP A

## Setup

1. Pasang Flutter SDK dan pastikan `flutter --version` bisa dijalankan.
2. Clone repositori ini, lalu masuk ke foldernya:
   ```
   git clone https://github.com/fildzasnaliaa/myportofolio_mobile.git
   cd myportofolio_mobile
   ```
3. Unduh dependensi:
   ```
   flutter pub get
   ```
4. Jalankan aplikasi di Chrome:
   ```
   flutter run -d chrome
   ```
5. Cek kualitas kode:
   ```
   flutter analyze
   ```

## Progres Mingguan

- **Tutorial 08:** setup Flutter, membuat proyek, memisahkan `menu.dart` dari `main.dart`, membuat `InfoCard` dan `ItemCard`.
- **Tugas 7:** mengganti data contoh dengan identitas sendiri, membuat tombol Lihat Experience, Tambah Experience, dan Logout dengan warna berbeda, serta membuat halaman bisa di-scroll agar tidak overflow di layar pendek.

### Tugas 7

1. **Widget tree dan hubungan parent-child**
Di Flutter, seluruh tampilan disusun dari widget yang saling bersarang dan membentuk struktur pohon (widget tree). Widget paling atas (root) adalah `MyApp`, di bawahnya `MaterialApp`, lalu `Scaffold`, dan seterusnya sampai widget terkecil seperti `Text` dan `Icon`. Sebuah widget induk (parent) membungkus satu widget anak (lewat properti `child`) atau beberapa anak (lewat `children`). Parent menentukan batas ukuran (constraints) untuk anaknya, anak menentukan ukurannya sendiri di dalam batas itu, lalu parent mengatur posisi anak tersebut. Contohnya di aplikasi ini, `Row` adalah parent dari tiga `InfoCard` dan menyusunnya sejajar secara horizontal. Data juga mengalir dari atas ke bawah, misalnya `ItemCard` menerima objek `ItemHomepage` dari `MyHomePage`, dan tema dari `MaterialApp` bisa diakses oleh widget di bawahnya lewat `Theme.of(context)`.

2. **Widget yang digunakan**
- `MaterialApp`: root aplikasi yang menyediakan tema, judul, dan halaman awal (`home`).
- `Scaffold`: kerangka halaman yang menyediakan tempat untuk `AppBar`, `body`, dan `SnackBar`.
- `AppBar`: bar di bagian atas halaman yang menampilkan judul "MyPortfolio".
- `Text`: menampilkan teks, misalnya judul, isi kartu, dan nama tombol.
- `SingleChildScrollView`: membuat isi halaman bisa di-scroll sehingga tidak overflow di layar yang pendek.
- `Padding`: memberi jarak di sekitar sebuah widget.
- `Column`: menyusun widget secara vertikal.
- `Row`: menyusun widget secara horizontal, dipakai untuk tiga `InfoCard`.
- `SizedBox`: memberi jarak kosong dengan ukuran tertentu.
- `Card`: wadah dengan bayangan (elevation) untuk tampilan `InfoCard`.
- `Container`: mengatur lebar dan padding isi kartu.
- `GridView.count`: menampilkan tombol dalam bentuk kisi 3 kolom.
- `Material`: memberi warna latar dan sudut melengkung pada `ItemCard`, sekaligus menjadi permukaan untuk efek sentuh.
- `InkWell`: membuat kartu bisa ditekan dan menampilkan efek ripple.
- `Center`: menempatkan isi kartu tombol di tengah.
- `Icon`: menampilkan ikon pada tiap tombol.
- `SnackBar`: notifikasi singkat di bawah layar saat tombol ditekan, ditampilkan lewat `ScaffoldMessenger`.
- `InfoCard` dan `ItemCard`: widget buatan sendiri (custom) untuk kartu identitas dan kartu tombol aksi.

3. **Fungsi MaterialApp**
`MaterialApp` adalah widget pembungkus yang menyiapkan semua yang dibutuhkan aplikasi bergaya Material Design: tema warna (`ThemeData`), sistem navigasi antarhalaman (`Navigator`), `ScaffoldMessenger` untuk SnackBar, pengaturan bahasa, dan judul aplikasi. Widget ini dipakai sebagai root karena banyak widget Material lain bergantung pada hal-hal yang disediakannya. Misalnya `Scaffold` butuh `ScaffoldMessenger` untuk menampilkan SnackBar, dan `Theme.of(context)` di `AppBar` hanya bisa membaca tema kalau di atasnya ada `MaterialApp`. Dengan menaruhnya di paling atas, semua widget di bawahnya otomatis mendapat akses ke tema dan fitur tersebut.

4. **StatelessWidget vs StatefulWidget**
`StatelessWidget` adalah widget yang tampilannya hanya bergantung pada data yang diberikan saat dibuat dan tidak berubah selama widget itu hidup. `StatefulWidget` punya objek `State` terpisah yang bisa menyimpan data yang berubah, lalu memperbarui tampilan dengan memanggil `setState()`. Pilih `StatelessWidget` kalau tampilannya statis, seperti halaman menu di tugas ini yang hanya menampilkan identitas dan tombol tetap. Pilih `StatefulWidget` kalau ada data yang berubah karena interaksi atau waktu, misalnya penghitung, isian form, checkbox, atau data yang dimuat dari API. Karena itu `MyHomePage` diubah dari Stateful menjadi Stateless: counter bawaan Flutter sudah tidak dipakai, sehingga tidak ada state yang perlu disimpan.

5. **BuildContext**
`BuildContext` adalah penanda posisi sebuah widget di dalam widget tree. Lewat `context`, sebuah widget bisa mencari dan memakai data dari widget di atasnya. Setiap metode `build(BuildContext context)` menerima context ini dari Flutter. Di proyek ini, `context` dipakai untuk `Theme.of(context).colorScheme.primary` (mengambil warna tema untuk `AppBar`), `MediaQuery.of(context).size.width` (mengambil lebar layar untuk menentukan lebar `InfoCard`), dan `ScaffoldMessenger.of(context)` (menampilkan SnackBar). Context ini penting karena hasilnya bergantung pada posisi widget. Context yang posisinya berada di atas `MaterialApp` tidak akan menemukan tema atau `ScaffoldMessenger`.

6. **Hot reload vs hot restart**
Hot reload (tekan `r` di terminal) menyuntikkan kode yang baru diubah ke aplikasi yang sedang berjalan, lalu membangun ulang widget tree tanpa menghapus state. Hasilnya muncul dalam hitungan detik, dan data yang sedang tampil (misalnya isian form) tetap ada. Hot restart (tekan `R`) memulai ulang aplikasi dari `main()` sehingga semua state kembali ke awal, tapi tetap lebih cepat daripada menjalankan ulang dari nol. Hot reload tidak cukup ketika perubahan menyentuh hal yang hanya dijalankan sekali saat aplikasi dimulai, misalnya isi fungsi `main()`, nilai awal variabel global atau `static`, isi `initState()`, atau perubahan sebuah widget dari Stateless menjadi Stateful. Untuk itu perlu hot restart. Full restart (hentikan dengan `q` lalu `flutter run` lagi) dibutuhkan ketika menambah package baru di `pubspec.yaml`, mengubah aset, atau mengubah kode native di folder `android/` maupun `ios/`.

## AI Disclosure

- **Tools:** Claude, sebagai asisten pengembangan.
- **Strategi:** ketika ada tampilan yang bermasalah, saya mengirimkan screenshot-nya untuk mencari tahu penyebabnya, lalu mencari solusi yang tetap mempertahankan struktur widget dari Tutorial 08. Contohnya saat muncul tampilan "BOTTOM OVERFLOWED" di Chrome, yang kemudian diatasi dengan `SingleChildScrollView`.
- **Keterbatasan AI:** AI tidak bisa menjalankan Flutter, sehingga tidak bisa melihat tampilan akhir atau memastikan kode benar-benar berjalan. Saran yang diberikan juga tidak selalu sesuai konteks tugas, jadi tetap perlu dicocokkan dengan instruksi tutorial dan soal.
- **Perbaikan dan verifikasi manual:** masalah overflow saya temukan sendiri saat menjalankan aplikasi di Chrome. Setelah perbaikan, saya mengecek ulang data di `InfoCard`, warna tiap tombol, dan pesan SnackBar satu per satu, lalu memastikan `flutter analyze` tidak menemukan masalah.