# 📖 Il2CppDumper — Panduan Setup & Penggunaan

**Version:** 6.1.0  
**Platform:** Android  
**Fungsi:** Dump metadata IL2CPP dari game Unity untuk reverse engineering

---

## 📥 Download APK

Download APK dari halaman [Releases](https://github.com/Kztutorial99/ITOOLS/releases/tag/il2cpp-v6.1.0):

```
Il2cppDumper_6.1.0.apk
```

---

## ⚙️ Persyaratan

| Kebutuhan | Detail |
|-----------|--------|
| Android | Minimal Android 7.0 (API 24) |
| Root | **TIDAK diperlukan** |
| Storage | Min. 500MB kosong |
| Arsitektur | arm64-v8a / armeabi-v7a |

---

## 🚀 Cara Install & Pakai

### 1. Install APK
```bash
# Aktifkan "Unknown Sources" di Settings → Security
# Install APK yang sudah di-download
```

### 2. Pilih File Target
Buka app → pilih file dari game target:
- **`libil2cpp.so`** — library utama IL2CPP
- **`global-metadata.dat`** — file metadata Unity

Path umum di Android:
```
/data/app/<package.name>/lib/arm64/libil2cpp.so
/data/data/<package.name>/files/il2cpp/Metadata/global-metadata.dat
```

### 3. Output Dump
Hasil dump akan tersimpan di:
```
/sdcard/Il2CppDumper/
├── dump.cs          ← Class/method definitions
├── script.json      ← Untuk Frida scripting
├── il2cpp.h         ← Header file (offset)
└── DummyDll/        ← Dummy DLL files
```

---

## 🔧 Cara Pakai Hasil Dump

### Dengan Frida (Dynamic Analysis)
```bash
# Install frida-server di device
adb push frida-server /data/local/tmp/
adb shell chmod +x /data/local/tmp/frida-server
adb shell /data/local/tmp/frida-server &

# Gunakan script.json dari dump
frida -U -l il2cpp.js -p <PID>
```

### Dengan Ghidra / IDA Pro (Static Analysis)
1. Buka `libil2cpp.so` di Ghidra/IDA
2. Gunakan `il2cpp.h` sebagai header reference
3. Gunakan `dump.cs` untuk mencocokan nama method/field

---

## 🤖 GitHub Actions — Auto Build Info

Workflow tersedia di [`.github/workflows/`](../.github/workflows/):

| Workflow | Trigger | Fungsi |
|----------|---------|--------|
| `release.yml` | Push tag `v*` | Auto-release APK ke GitHub Releases |
| `validate.yml` | Push / PR | Validasi file & cek APK integrity |

### Cara Trigger Build Manual:
1. Buka tab **Actions** di repo
2. Pilih workflow **Release Il2CppDumper**
3. Klik **Run workflow** → masukkan versi → **Run**

---

## ❓ Troubleshooting

| Error | Solusi |
|-------|--------|
| `App not installed` | Uninstall versi lama dulu |
| `Parse error` | APK corrupt, re-download |
| `libil2cpp.so not found` | Game tidak pakai IL2CPP |
| `Metadata version mismatch` | Versi dumper tidak cocok dengan Unity game |

---

## 📌 Catatan

- Tool ini hanya untuk **edukasi & research**
- Jangan gunakan untuk cheat/modifikasi online game
- Hasil dump bersifat read-only, tidak memodifikasi game
