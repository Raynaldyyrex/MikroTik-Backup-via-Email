# MikroTik Backup via Email
Mikrotik Ros v.6 &amp; v.7

Script otomatis untuk melakukan **backup konfigurasi MikroTik** dan mengirimkan hasil backup langsung ke email menggunakan **SMTP2GO**. Script ini kompatibel dengan **RouterOS v6** maupun **RouterOS v7**, sehingga memudahkan administrator jaringan dalam menjaga keamanan konfigurasi router.

---

## ✨ Fitur

- Backup konfigurasi RouterOS secara otomatis.
- Mengirim file backup langsung ke email.
- Nama file backup dibuat otomatis berdasarkan **tanggal** dan **identitas router**.
- Mendukung **RouterOS v6** dan **RouterOS v7**.
- Dapat dijalankan secara otomatis menggunakan **Scheduler**.
- Telah diuji menggunakan layanan **SMTP2GO**.

---

## 📋 Persyaratan

Sebelum menggunakan script ini, pastikan telah melakukan konfigurasi berikut:

- Router MikroTik dengan RouterOS v6 atau v7.
- Akun **SMTP2GO** yang aktif.
- Konfigurasi SMTP pada MikroTik telah sesuai.
- Router memiliki akses internet.

---

## 🚀 Instalasi

1. Salin script ke menu **System → Scripts** pada MikroTik.
2. Konfigurasikan pengaturan SMTP menggunakan akun SMTP2GO.
3. Buat **Scheduler** agar script berjalan secara otomatis sesuai jadwal.
4. Pastikan email backup berhasil diterima.

---

## 📂 Contoh Nama File Backup

```
17Jul2026-Office.rsc
```

Format:

```
[Tanggal]-[Nama Router].rsc
```

---

## 💡 Kegunaan

Script ini cocok digunakan untuk:

- Backup konfigurasi harian.
- Mengantisipasi kehilangan konfigurasi router.
- Dokumentasi perubahan konfigurasi.
- Mempermudah proses pemulihan (restore) apabila terjadi kerusakan atau kesalahan konfigurasi.

---

## 📧 SMTP yang Digunakan

Saat ini saya menggunakan layanan **SMTP2GO** sebagai SMTP Relay untuk mengirimkan email backup secara aman menggunakan koneksi TLS.

---

## 🤝 Kontribusi

Kontribusi berupa perbaikan, pengembangan fitur, maupun pelaporan bug sangat diterima melalui **Issues** atau **Pull Request**.

---

## 📄 Lisensi

Project ini menggunakan lisensi **MIT License**.
