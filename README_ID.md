# Rimba Pet Android — Project Android Studio

Project ini membungkus antarmuka toko Rimba Pet ke dalam aplikasi Android berbasis WebView.
Fitur katalog, pencarian, keranjang, checkout WhatsApp, dashboard admin lokal, pencatatan
pesanan, pengajuan penarikan (catatan lokal), dan editor desain berasal dari versi web.

## Cara menghasilkan APK
1. Instal Android Studio versi terbaru.
2. Ekstrak folder `RimbaPetAndroid`.
3. Android Studio > Open > pilih folder `RimbaPetAndroid`.
4. Tunggu Gradle Sync dan instal Android SDK Platform 35 jika diminta.
5. Pilih Build > Build Bundle(s) / APK(s) > Build APK(s).
6. APK debug tersedia di `app/build/outputs/apk/debug/app-debug.apk`.

## Batasan penting versi 1.0
- Ini adalah project sumber Android yang siap dibuka dan dibangun di Android Studio.
  File APK biner belum dapat dibuat di lingkungan ini karena Android SDK/Gradle tidak tersedia.
- Data toko masih tersimpan lokal pada perangkat. Belum ada database/server sinkronisasi.
- Tombol login Google dan autentikasi pelanggan/admin belum terhubung ke Firebase.
- Pilihan QRIS/transfer/e-wallet hanya mengumpulkan preferensi checkout. Pembayaran belum
  diproses otomatis dan tidak boleh dianggap sudah dibayar.
- Pengajuan penarikan hanya dicatat lokal; tidak ada transfer uang otomatis.
- Untuk transaksi sungguhan, buat backend HTTPS, validasi callback/webhook pembayaran di server,
  verifikasi merchant, dan implementasikan payout sesuai dokumentasi penyedia.
- Atur nomor WhatsApp penjual pada konstanta `WA_NUMBER` di `app/src/main/assets/index.html`.

## Langkah integrasi sebelum rilis publik
1. Buat proyek Firebase, aktifkan Google Authentication, daftarkan package
   `id.rimbapet.app`, dan konfigurasi SHA-1/SHA-256.
2. Implementasikan otorisasi berbasis peran di server (admin tidak boleh ditentukan dari
   input pengguna atau sekadar email yang diketik).
3. Tambahkan `google-services.json` dan Firebase SDK sesuai dokumentasi Android Firebase.
4. Integrasikan Midtrans/Xendit melalui backend; jangan pernah menyimpan secret key di aplikasi.
5. Hubungkan Firestore/Cloud Storage dengan aturan keamanan yang ketat.
6. Uji transaksi, refund, stok, dokumen hewan, privasi, dan keamanan sebelum publikasi.
7. Lengkapi identitas usaha/perizinan sesuai jenis hewan dan aktivitas perdagangan.

## Package
`id.rimbapet.app`


## Build APK otomatis dari HP Android (GitHub Actions)

Project ini sudah dilengkapi workflow `.github/workflows/android.yml`.

1. Ekstrak ZIP ini di HP.
2. Buat repository baru di https://github.com/new (misalnya `rimba-pet-android`).
3. Di repository GitHub, pilih **Add file > Upload files**. Unggah seluruh ISI folder `RimbaPetAndroid` (termasuk folder tersembunyi `.github` yang berisi workflow). Jika browser HP tidak menampilkan folder tersembunyi, aktifkan tampilan file tersembunyi atau unggah `android.yml` melalui GitHub dengan membuat path `.github/workflows/android.yml`.
4. Commit ke branch `main`.
5. Buka tab **Actions**, pilih workflow **Build Rimba Pet APK**, lalu tekan **Run workflow** jika belum berjalan otomatis.
6. Tunggu sampai status selesai dengan tanda centang hijau. Buka hasil run, gulir ke **Artifacts**, lalu unduh `RimbaPet-debug-apk`.
7. Ekstrak artifact ZIP. Di dalamnya ada `app-debug.apk`; buka file itu untuk memasang aplikasi.

Catatan: GitHub harus diizinkan menjalankan Actions untuk repository. Build ini menghasilkan APK debug untuk uji coba, bukan APK release bertanda tangan untuk distribusi Play Store. Workflow hanya membangun aplikasi yang ada di project ini; fitur login Google, backend online, pembayaran gateway, dan payout bank belum terintegrasi.
