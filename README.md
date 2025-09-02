# 🌫️ Sistem Kontrol Kabut Otomatis untuk Greenhouse Anggrek

![Status](https://img.shields.io/badge/Status-Tahap%20Perancangan-blue.svg)
![Python](https://img.shields.io/badge/Python-3.9%2B-blue.svg)
![Framework](https://img.shields.io/badge/Framework-TensorFlow-orange.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)

Repositori ini adalah pusat pengembangan untuk Tugas Akhir berjudul **"Rancang Bangun Sistem Kontrol Kabut Otomatis pada Greenhouse Anggrek Berbasis Artificial Intelligence dan Internet of Things"**.

---

### Daftar Isi

1.  [Latar Belakang Masalah](#-latar-belakang-masalah)
2.  [Solusi yang Diusulkan](#-solusi-yang-diusulkan)
3.  [Fitur Utama](#-fitur-utama)
4.  [Arsitektur Sistem](#️-arsitektur-sistem)
5.  [Demonstrasi (Mockup)](#-demonstrasi-mockup)
6.  [Teknologi yang Digunakan](#-teknologi-yang-digunakan)
7.  [Struktur Repositori](#-struktur-repositori)
8.  [Instalasi dan Penggunaan](#-instalasi-dan-penggunaan)

---

### 📜 Latar Belakang Masalah

Budidaya anggrek dalam _greenhouse_ menghadapi dua tantangan utama yang disebabkan oleh **kabut**:

- **Kegagalan Monitoring Visual**: Kabut tebal menghalangi pandangan kamera, menyebabkan sistem pemantauan cerdas (AI) untuk hama dan penyakit menjadi tidak efektif dan menghasilkan data yang tidak valid.
- **Ancaman Agronomi**: Kabut menciptakan lingkungan dengan kelembaban ekstrem (>95%), yang merupakan kondisi ideal bagi perkembangan penyakit jamur (_Botrytis_, busuk hitam) dan dapat menghambat pertumbuhan tanaman.

---

### 💡 Solusi yang Diusulkan

Proyek ini membangun sebuah sistem cerdas dan otonom yang mampu:

1.  **Mendeteksi** keberadaan kabut secara visual menggunakan model _Convolutional Neural Network_ (CNN) yang dijalankan pada perangkat _edge_.
2.  **Mengontrol** kondisi lingkungan dengan mengaktifkan kipas _exhaust_ secara otomatis ketika tingkat kabut melampaui ambang batas yang ditentukan.
3.  **Memantau** status deteksi dan aktuator dari jarak jauh melalui _dashboard_ berbasis _cloud_.

**➡️ Untuk analisis yang lebih mendalam, silakan lihat [Analisis dan Metode](docs/analisis_dan_metode.md).**

---

### ✨ Fitur Utama

- **Deteksi Cerdas**: Klasifikasi kondisi `Berkabut` / `Tidak Berkabut` secara _real-time_.
- **Kontrol Otomatis**: Aktivasi kipas _exhaust_ berdasarkan hasil prediksi AI.
- **_Edge Computing_**: Seluruh proses AI berjalan langsung di Raspberry Pi, mengurangi latensi.
- **Monitoring Jarak Jauh**: Hasil deteksi dapat dipantau melalui platform _cloud_.

---

### 🛠️ Arsitektur Sistem

Sistem ini dirancang dengan arsitektur terdistribusi yang terdiri dari unit perangkat keras di lapangan dan _platform_ monitoring di _cloud_.

#### **Perangkat Keras**

| Peran             | Komponen                       | Fungsi                                    |
| :---------------- | :----------------------------- | :---------------------------------------- |
| **Otak Sistem**   | Raspberry Pi 4 Model B         | Menjalankan skrip dan inferensi model AI. |
| **Mata Sistem**   | Raspberry Pi Camera Module 3   | Mengambil gambar _greenhouse_.            |
| **Tangan Sistem** | Modul Relay 5V & Kipas Exhaust | Mengaktifkan/menonaktifkan kipas.         |

#### **Alur Kerja Perangkat Lunak**

1.  **Jadwal**: Sistem berjalan secara periodik (misal: setiap 5 menit).
2.  **Akuisisi**: Kamera menangkap citra lingkungan.
3.  **Prediksi**: Model AI (CNN) mengklasifikasikan citra.
4.  **Aksi**: Jika prediksi `Berkabut > 80%`, sistem mengaktifkan kipas selama durasi tertentu.
5.  **Monitoring**: Hasil dan status aksi dikirim ke _cloud_ dan dicatat dalam _log_.

---

### 🎬 Demonstrasi (Mockup)

Berikut adalah gambaran bagaimana sistem direncanakan akan bekerja. Simulasi ini menunjukkan alur kerja dari deteksi kondisi "Berkabut" hingga aktivasi kipas, lalu kembali ke kondisi normal.

![Simulasi Deteksi Kabut](assets/simulasi_mockup.gif)

---

### 💻 Teknologi yang Digunakan

- **Bahasa Pemrograman**: Python
- **Framework AI**: TensorFlow / Keras
- **Perangkat Keras**: Raspberry Pi
- **Penjadwalan**: APScheduler / Cron
- **Dokumentasi**: Markdown

---

### 📂 Struktur Repositori
