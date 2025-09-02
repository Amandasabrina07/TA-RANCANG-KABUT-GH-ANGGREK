# Analisis, Rancangan, dan Perbandingan Metode AI

Dokumen ini berisi ringkasan teknis dan perbandingan metode AI yang relevan untuk proyek deteksi kabut di _greenhouse_ anggrek.

---

## 1. Ringkasan Proyek: Analisis, Rancangan, dan Metode

### **🎯 Analisis Masalah (Inti Masalah)**

- **Masalah Visual**: Kabut menghalangi kamera, membuat sistem monitoring AI lain (untuk hama/penyakit) menjadi tidak efektif.
- **Masalah Agronomi**: Kabut menciptakan kelembaban ekstrem (>95%), yang memicu penyakit jamur dan menghambat pertumbuhan anggrek.
- **Solusi**: Sistem AI yang secara visual mengenali tekstur kabut dan secara otomatis mengaktifkan kipas untuk menormalkan kondisi.

### **⚙️ Rancangan Sistem (Alur Kerja)**

1.  **Input**: Kamera pada Raspberry Pi mengambil gambar _greenhouse_ secara periodik.
2.  **Proses**: Model AI di Raspberry Pi mengklasifikasikan gambar sebagai **`Berkabut`** atau **`Tidak Berkabut`**.
3.  **Logika**: Jika `Berkabut` dengan keyakinan > 80%, sistem mengirim sinyal aktif.
4.  **Output**: Sinyal mengaktifkan _relay_ yang menyalakan kipas _exhaust_.
5.  **Monitoring**: Hasil deteksi dan status kipas dikirim ke _cloud_ untuk dipantau.

### **🧠 Metode AI yang Digunakan (Mesin Penggerak)**

- **Metode Utama**: **CNN (_Convolutional Neural Network_)**.
- **Arsitektur Spesifik**: **MobileNetV2** atau **EfficientNet-Lite**. Keduanya adalah arsitektur CNN modern yang ringan dan sangat efisien, dirancang khusus untuk perangkat _edge_ seperti Raspberry Pi.
- **Teknik**: **_Transfer Learning_**. Menggunakan model yang sudah terlatih dan melatihnya kembali dengan dataset gambar kabut untuk mencapai akurasi tinggi dengan cepat.

---

## 2. Perbandingan Metode AI (Selain CNN Standar)

Meskipun CNN adalah pilihan yang sangat solid, ada arsitektur lain yang bisa dipertimbangkan. "Lebih baik" di sini diukur berdasarkan potensi akurasi dan efisiensi.

| Metode AI                    | Cara Kerja Singkat                                                                                                                                                   | Kelebihan (Potensi "Lebih Baik")                                                                                                                               | Kekurangan (Tantangan Implementasi)                                                                                                                                |
| :--------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **CNN (Baseline)**           | Mengekstrak fitur lokal (tepi, tekstur, warna) melalui filter-filter kecil yang bergeser di seluruh gambar.                                                          | ✅ Sangat efisien & cepat (terutama MobileNet/EfficientNet).<br>✅ Andal untuk klasifikasi tekstur seperti kabut.<br>✅ Teknologi matang, banyak referensi.    | ❌ Kurang baik dalam memahami hubungan antar bagian gambar yang jauh (konteks global).                                                                             |
| **Vision Transformer (ViT)** | Memecah gambar menjadi beberapa "potongan" (seperti kata dalam kalimat), lalu menganalisis hubungan dan kepentingan setiap potongan terhadap semua potongan lainnya. | ✅ **Potensi Akurasi Tertinggi**. Sangat baik dalam memahami konteks global seluruh gambar.<br>✅ Arsitektur modern yang menjadi standar baru di banyak riset. | ❌ **Sangat berat** secara komputasi, sulit dijalankan di Raspberry Pi.<br>❌ Butuh **dataset yang jauh lebih besar** daripada CNN untuk bisa belajar dengan baik. |
| **Ensemble Learning**        | Menggabungkan prediksi dari **beberapa model AI yang berbeda** (misalnya 2 model CNN dengan arsitektur berbeda) dan mengambil keputusan berdasarkan suara mayoritas. | ✅ **Sangat Andal dan Stabil**. Dapat mengurangi kesalahan prediksi dari satu model tunggal.<br>✅ Meningkatkan kepercayaan diri pada hasil akhir.             | ❌ Menjalankan beberapa model sekaligus akan **memperlambat proses** dan membebani Raspberry Pi.<br>❌ Lebih rumit dalam perancangan kode.                         |

### **🏆 Rekomendasi Terbaik untuk Proyek Anda**

Untuk skala Tugas Akhir yang diimplementasikan pada Raspberry Pi, pilihan "yang lebih baik dari CNN standar" bukanlah beralih ke ViT yang sangat berat, melainkan **menggunakan varian CNN yang paling canggih dan efisien**.

**Rekomendasi**: Tetap gunakan metode **CNN**, tetapi dengan arsitektur **EfficientNetV2** atau **MobileNetV3**.

**Alasan:**

- **Keseimbangan Terbaik**: Arsitektur ini memberikan **akurasi mendekati _state-of-the-art_** dengan **kebutuhan komputasi yang sangat rendah**, menjadikannya pilihan paling optimal untuk perangkat _edge_ seperti Raspberry Pi.
- **Kelayakan**: Anda mendapatkan peningkatan performa signifikan tanpa meningkatkan kompleksitas proyek secara drastis seperti jika menggunakan ViT atau Ensemble Learning.
