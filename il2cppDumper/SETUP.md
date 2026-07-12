# 📖 Il2CppDumper — Panduan Setup & Penggunaan

**Version:** 6.1.0  
**Platform:** Android  
**Source:** [rodroidmods/rodroid-il2cppdumper](https://github.com/rodroidmods/rodroid-il2cppdumper)  
**Stack:** Tauri 2 + Rust + SvelteKit + Svelte 5

---

## 📥 Download APK

Download APK dari halaman [Releases](https://github.com/Kztutorial99/ITOOLS/releases/tag/il2cpp-v6.1.0):

```
Il2cppDumper_6.1.0.apk  (~347MB)
```

---

## ⚙️ Persyaratan

| Kebutuhan | Detail |
|-----------|--------|
| Android | Minimal Android 7.0 (API 24) |
| Root | **TIDAK diperlukan** |
| Storage | Min. 500MB kosong |
| Arsitektur | arm64-v8a / x86_64 |

---

## 🚀 Cara Install & Pakai

### 1. Install APK
```
Aktifkan "Unknown Sources" di Settings → Security
Install APK yang sudah di-download
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

## 🤖 GitHub Actions Workflows

| Workflow | File | Trigger | Fungsi |
|----------|------|---------|--------|
| **🤖 Build APK Android** | `android-build.yml` | Manual | Compile APK dari source code |
| **🚀 Release** | `release.yml` | Tag / Manual | Buat/update GitHub Release |
| **✅ Validate** | `validate.yml` | Push ke main | Cek struktur folder & file |

---

## 🔨 Cara Build APK dari Source

### Via GitHub Actions (Recommended)

1. Buka **[Actions](https://github.com/Kztutorial99/ITOOLS/actions/workflows/android-build.yml)**
2. Klik **Run workflow**
3. Isi:
   - **Version**: `6.1.0`
   - **Upload ke Release**: ✅ centang
4. Klik **Run workflow**
5. Tunggu ±45–90 menit
6. APK tersedia di:
   - **Artifacts** tab di run tersebut
   - **Releases** (jika upload_release = true)

### Build Flow Diagram

```
GitHub Actions Runner (ubuntu-latest)
│
├── Clone source: rodroidmods/rodroid-il2cppdumper
├── Setup JDK 17 + Android SDK + NDK 27
├── Setup Rust + targets:
│     aarch64-linux-android
│     x86_64-linux-android
├── npm ci  (install frontend deps)
├── tauri android init  (generate Android project)
├── Inject signing keystore (PKCS12)
├── tauri android build --apk --target aarch64 --target x86_64
├── Rename APK → Il2cppDumper_6.1.0_built.apk
├── Upload ke Action Artifacts (30 hari)
└── Upload ke GitHub Release il2cpp-v6.1.0
```

### GitHub Secrets yang Dipakai

| Secret | Isi |
|--------|-----|
| `ANDROID_KEYSTORE_BASE64` | Signing keystore (PKCS12, base64) |
| `KEY_STORE_PASSWORD` | Password keystore |
| `KEY_ALIAS` | Alias key di keystore |
| `KEY_PASSWORD` | Password private key |

> Secrets sudah diset — tidak perlu diubah kecuali mau pakai keystore sendiri.

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

## ❓ Troubleshooting Install

| Error | Solusi |
|-------|--------|
| `App not installed` | Uninstall versi lama dulu |
| `Parse error` | APK corrupt, re-download dari Releases |
| `libil2cpp.so not found` | Game tidak pakai IL2CPP |
| `Metadata version mismatch` | Coba versi dumper berbeda |

---

## ❓ Troubleshooting Build

| Error | Solusi |
|-------|--------|
| `NDK not found` | NDK 27 otomatis diinstall di workflow |
| `Rust target not found` | Sudah include di workflow targets |
| `Signing failed` | Cek secrets ANDROID_KEYSTORE_BASE64 |
| `tauri android init failed` | Pastikan JDK 17 aktif |
| Build timeout (>90 menit) | Re-run, Rust cache mempercepat run berikutnya |

---

## 📌 Catatan

- Tool ini hanya untuk **edukasi & research**
- Jangan gunakan untuk cheat/modifikasi online game
- APK pre-built tersedia di Releases, tidak perlu build manual
