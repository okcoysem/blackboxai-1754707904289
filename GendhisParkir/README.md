# GendhisParkir - Android WebView Application

GendhisParkir adalah aplikasi Android berbasis WebView yang menampilkan sistem monitoring parkir dari https://dp.gendhisraya.com/ dengan fitur pencetakan thermal dan akses kamera.

## Fitur Utama

- **WebView Integration**: Menampilkan web aplikasi parkir Sadewa Parking
- **Thermal Printer Support**: 
  - Dukungan printer built-in Sunmi V2/V2 Pro
  - Dukungan printer Bluetooth universal (58mm)
  - ESC/POS command support
- **Camera & Gallery Access**: Capture gambar dari kamera dan akses galeri
- **JavaScript Bridge**: Interface untuk komunikasi antara web dan native Android

## Spesifikasi Minimum

- **Android Version**: 7.0 (API Level 24) atau lebih tinggi
- **RAM**: Minimum 1GB
- **Storage**: 50MB ruang kosong
- **Hardware**: 
  - Bluetooth untuk printer eksternal
  - Kamera (opsional)
  - Akses internet

## Target Devices

- **Primary**: Sunmi V2, Sunmi V2 Pro (dengan printer built-in)
- **Secondary**: Android devices dengan Bluetooth printer support

## Struktur Project

```
GendhisParkir/
├── app/
│   ├── src/main/
│   │   ├── java/com/gendhisraya/gendhisparkir/
│   │   │   ├── MainActivity.java              # Activity utama dengan WebView
│   │   │   ├── PrinterManager.java            # Manager untuk printer functionality
│   │   │   └── WebViewJavaScriptInterface.java # JavaScript bridge
│   │   ├── res/
│   │   │   ├── layout/activity_main.xml       # Layout utama
│   │   │   ├── values/                        # Resources (strings, colors, themes)
│   │   │   └── xml/                          # File paths dan backup rules
│   │   └── AndroidManifest.xml               # Manifest dengan permissions
│   ├── build.gradle                          # App-level build configuration
│   └── proguard-rules.pro                    # ProGuard rules
├── build.gradle                              # Project-level build configuration
├── settings.gradle                           # Project settings
└── gradle.properties                         # Gradle properties
```

## Permissions

Aplikasi memerlukan permissions berikut:
- `INTERNET` - Untuk akses web
- `CAMERA` - Untuk capture gambar
- `BLUETOOTH` / `BLUETOOTH_CONNECT` - Untuk printer
- `READ_EXTERNAL_STORAGE` / `READ_MEDIA_IMAGES` - Untuk akses galeri

## JavaScript API

Aplikasi menyediakan JavaScript API untuk web application:

```javascript
// Print thermal receipt
window.printThermal(content, {
    align: 1,        // 0=left, 1=center, 2=right
    fontSize: 1.0    // Font size multiplier
});

// Connect to printer
window.connectPrinter();

// Get printer status
var status = window.getPrinterStatus(); // "connected_sunmi", "connected_bluetooth", "disconnected"
```

## Build Instructions

1. **Prerequisites**:
   - Android Studio Arctic Fox atau lebih baru
   - Android SDK dengan API Level 24+
   - Java 8 atau lebih tinggi

2. **Build Steps**:
   ```bash
   # Clone atau copy project ke Android Studio
   # Sync project dengan Gradle files
   # Build APK
   ./gradlew assembleDebug
   
   # Atau build release APK
   ./gradlew assembleRelease
   ```

3. **Installation**:
   ```bash
   # Install via ADB
   adb install app/build/outputs/apk/debug/app-debug.apk
   ```

## Printer Setup

### Sunmi Devices
- Printer built-in akan terdeteksi otomatis
- Tidak perlu pairing manual

### Bluetooth Printer
1. Pair printer dengan device Android melalui Settings > Bluetooth
2. Pastikan printer dalam mode pairing
3. Aplikasi akan mencari printer yang sudah di-pair dengan nama yang mengandung:
   - "printer"
   - "thermal" 
   - "pos"
   - "receipt"

## Troubleshooting

### Printer Issues
- **Sunmi tidak terdeteksi**: Pastikan Sunmi SDK service berjalan
- **Bluetooth printer tidak connect**: Cek pairing dan jarak device
- **Print gagal**: Periksa kertas dan status printer

### WebView Issues
- **Page tidak load**: Periksa koneksi internet
- **JavaScript error**: Enable JavaScript di WebView settings
- **Camera tidak berfungsi**: Berikan permission kamera

## Development Notes

- Aplikasi menggunakan `cleartext traffic` untuk HTTP connections
- WebView dikonfigurasi dengan JavaScript enabled dan DOM storage
- File provider dikonfigurasi untuk camera functionality
- ProGuard rules sudah dikonfigurasi untuk production build

## License

Copyright © 2024 Gendhis Raya. All rights reserved.
