# Analisis, Rancangan, dan Perbandingan Metode AI

Dokumen ini berisi ringkasan teknis dan perbandingan metode AI yang relevan untuk proyek deteksi kabut di _greenhouse_ anggrek.

---

## 1. Analisis, Rancangan, dan Metode

### **🎯 Analisis Masalah (Inti Masalah)**

Proyek ini berangkat dari identifikasi tiga masalah fundamental yang disebabkan oleh kabut di dalam _greenhouse_, yang tidak dapat diselesaikan secara efektif oleh sistem pemantauan konvensional.

### **🔬 Ancaman Agronomi**

Kabut secara langsung menciptakan lingkungan mikro yang ideal bagi patogen. Kelembaban udara yang jenuh (>95%) secara signifikan meningkatkan risiko serangan **penyakit jamur** seperti _Botrytis_ (busuk bunga) dan busuk hitam, yang dapat menyebar dengan cepat dan menyebabkan kerugian panen. Lebih dari itu, kabut **menghambat proses transpirasi** tanaman, yaitu pelepasan uap air yang esensial untuk menarik nutrisi dari akar. Jika proses ini terganggu dalam waktu lama, pertumbuhan anggrek dapat terhambat dan menjadi kerdil.

### **📸 Kegagalan Sistem Monitoring**

_Greenhouse_ modern mengandalkan sistem monitoring visual berbasis AI untuk memantau kesehatan tanaman. Kabut berfungsi sebagai **penghalang visual** yang membuat input data dari kamera menjadi tidak dapat diandalkan. Ini menyebabkan **kegagalan beruntun**: model AI untuk deteksi hama atau penyakit akan salah menginterpretasikan gambar yang buram, data analisis pertumbuhan menjadi tidak valid, dan kemampuan pengelola untuk memantau secara visual dari jarak jauh menjadi lumpuh total.

### **📟 Keterbatasan Sensor Konvensional**

Sensor kelembaban (seperti DHT22) memang dapat mendeteksi udara yang lembab, namun memiliki kelemahan kritis:

- **Tidak Membedakan Sumber**: Sensor tidak bisa membedakan antara kelembaban tinggi akibat penyiraman, penguapan normal, atau kabut tebal yang menghalangi pandangan.
- **Tidak Mewakili Kondisi Visual**: Nilai kelembaban 99% tidak memberi tahu kita apakah kamera masih bisa "melihat" atau sudah "buta" total.
  Oleh karena itu, **deteksi berbasis citra (AI Camera)** menjadi satu-satunya solusi yang dapat mengukur dampak kabut terhadap visibilitas secara langsung.

---

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

## 2. Pengembangan Ide dan Inovasi (Fitur Lanjutan)

Untuk meningkatkan nilai kebaruan proyek, berikut adalah beberapa ide inovatif yang dapat dikembangkan dari sistem dasar, dengan kompleksitas yang masih masuk akal untuk tugas akhir:

### **💡 Ide 1: Deteksi Kabut Prediktif**

- **Konsep**: Alih-alih hanya bereaksi saat kabut sudah tebal, sistem dapat **memprediksi** potensi terbentuknya kabut tebal.
- **Cara Kerja**: Sistem tidak hanya menganalisis satu gambar, tetapi **membandingkan beberapa gambar terakhir** (misalnya, gambar dari 5 dan 10 menit yang lalu). Jika model mendeteksi adanya **peningkatan densitas kabut yang signifikan dan cepat**, sistem dapat mengaktifkan kipas secara **preventif** sebelum visibilitas hilang total.
- **Tingkat Kesulitan**: Menengah. Memerlukan penambahan logika untuk menyimpan dan membandingkan hasil prediksi sebelumnya.

### **💡 Ide 2: Kontrol Kipas Adaptif**

- **Konsep**: Daripada hanya ON/OFF, kecepatan putaran kipas **disesuaikan dengan tingkat ketebalan kabut**.
- **Cara Kerja**: Nilai _confidence_ dari prediksi AI (misalnya 85% vs 98% `Berkabut`) dapat dipetakan untuk mengatur kecepatan kipas melalui sinyal **PWM (Pulse Width Modulation)** ke _driver motor_. Jika kabut tipis (keyakinan 85%), kipas berputar pelan. Jika kabut sangat tebal (keyakinan 98%), kipas berputar dengan kecepatan maksimal.
- **Tingkat Kesulitan**: Menengah. Memerlukan penambahan komponen _driver motor_ L298N dan sedikit modifikasi pada kode kontrol. Inovasi ini sangat baik karena menonjolkan aspek **efisiensi energi**.

### **💡 Ide 3: Integrasi Data Cuaca Eksternal**

- **Konsep**: Membuat sistem lebih "sadar" akan kondisi di luar _greenhouse_.
- **Cara Kerja**: Sistem secara periodik mengambil data **prakiraan cuaca** dari internet melalui **API publik** (seperti OpenWeatherMap). Jika prakiraan menunjukkan akan ada **penurunan suhu drastis** atau **hujan lebat** dalam satu jam ke depan (kondisi pemicu kabut), sistem dapat masuk ke mode **"Siaga Tinggi"**, di mana frekuensi pengambilan gambar ditingkatkan (misalnya dari setiap 10 menit menjadi setiap 2 menit) untuk deteksi yang lebih cepat.
- **Tingkat Kesulitan**: Rendah ke Menengah. Hanya memerlukan penambahan beberapa baris kode untuk melakukan permintaan API.

---

## 3. Perbandingan Metode AI (Selain CNN Standar)

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
