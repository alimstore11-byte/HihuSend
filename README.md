# HiHuChat — Offline Messenger starter

Ini adalah **starter/MVP tahap 1**, bukan aplikasi setara WhatsApp yang sudah selesai.
Yang sudah disediakan:
- UI percakapan dasar.
- Server TCP lokal dan klien TCP untuk pesan teks melalui WiFi/hotspot yang sama.
- Relay sederhana dari server ke klien lain (grup lokal dasar).
- Tidak memakai server internet atau paket data.

## Membuat proyek Flutter

1. Pasang Flutter SDK dan Android Studio.
2. Di terminal jalankan:
   ```bash
   flutter create --org com.hihuchat hihuchat
   ```
3. Salin isi folder starter ini ke folder proyek hasil `flutter create` (gabungkan folder `lib`, ganti `pubspec.yaml`, dan sesuaikan manifest).
4. Jalankan:
   ```bash
   flutter pub get
   flutter analyze
   flutter run
   ```
5. Untuk membuat APK:
   ```bash
   flutter build apk --release
   ```
   APK biasanya berada di `build/app/outputs/flutter-apk/app-release.apk`.

## Uji coba WiFi/hotspot

1. Pasang aplikasi di dua HP Android.
2. Sambungkan kedua HP ke WiFi yang sama, atau sambungkan HP kedua ke hotspot HP pertama.
3. Pada HP pertama, pilih **Buat jaringan chat**. Catat IP yang ditampilkan.
4. Pada HP kedua, pilih **Gabung ke jaringan teman**, lalu masukkan IP HP pertama.
5. Kirim pesan teks.

Catatan: alamat IP hotspot berbeda-beda menurut merek/versi Android. Fitur server TCP tidak otomatis menyalakan hotspot; hotspot diaktifkan dari pengaturan Android. Pastikan firewall/router tidak mengisolasi perangkat (AP/client isolation).

## Status fitur

| Fitur | Status starter |
|---|---|
| Chat teks WiFi/hotspot | Implementasi awal |
| Grup lokal | Relay sederhana melalui server |
| Bluetooth BLE Android+iOS | Belum diimplementasikan |
| Foto/file | Belum diimplementasikan |
| Voice note/panggilan | Belum diimplementasikan |
| Profil, kontak, read receipt | Belum diimplementasikan |
| Enkripsi end-to-end | Belum diimplementasikan |
| Penyimpanan chat permanen | Belum diimplementasikan |

## Catatan arsitektur penting

- Gunakan BLE (bukan Bluetooth Classic) untuk jalur Bluetooth lintas Android/iOS. Plugin Flutter `bluetooth_low_energy` menyediakan peran central dan peripheral pada Android dan iOS. Implementasi BLE perlu protokol framing/chunking karena payload BLE terbatas.
- Untuk WiFi/hotspot, TCP socket lokal cocok untuk teks, foto, dan file. Tahap produksi sebaiknya memakai penemuan perangkat otomatis (mDNS/NSD), autentikasi pairing, enkripsi, retry, dan antrean pesan.
- Pada iOS, aplikasi tidak boleh mengaktifkan Personal Hotspot pengguna secara diam-diam. Pengguna mengaktifkan hotspot dari Settings; iOS juga meminta izin Local Network dan Bluetooth.
- Pada Android baru, akses jaringan lokal dapat memerlukan izin tambahan sesuai target SDK/versi OS.
- Jangan gunakan untuk pesan sensitif sebelum autentikasi dan enkripsi end-to-end diterapkan.

## Referensi
- Flutter BLE plugin: https://pub.dev/packages/bluetooth_low_energy
- Flutter local network permission: https://docs.flutter.dev/platform-integration/android/local-network-permission
