# 🛣️ Sistem Deteksi dan Visualisasi Lubang Jalan Berbasis Video dan GPS dengan Algoritma YOLOv11

Proyek ini merupakan implementasi sistem deteksi kerusakan jalan berupa lubang (*pothole*) yang terintegrasi dengan pemetaan koordinat geografis. Proyek ini disusun sebagai bagian dari penelitian Tugas Akhir oleh Muhamad Zaki Mubarok di Program Studi Fisika, Fakultas Sains dan Teknologi, UIN Syarif Hidayatullah Jakarta.

Sistem ini mengombinasikan rekaman video dari kamera *smartphone* dengan data koordinat lokasi dari *data logger* portabel buatan sendiri (berbasis Arduino Nano dan modul GPS NEO-7M). Hasil deteksi dan sinkronisasi data divisualisasikan dalam sebuah *dashboard* berbasis peta digital secara lokal.

## Demo Dashboard
- **Input Data (model, video, dan GPS) dan Kalibrasi Waktu**
<img width="773" height="375" alt="image" src="https://github.com/user-attachments/assets/cc74a4d9-9cbc-4769-906a-d59f0bbb8ba3" />

- **Kalibrasi Sistem**
<img width="755" height="396" alt="image" src="https://github.com/user-attachments/assets/dd8f8b24-2ebc-453b-934d-ab8c5d0c6ed8" />

- **Proses Deteksi**
<img width="714" height="373" alt="image" src="https://github.com/user-attachments/assets/407389b0-c4e5-4099-8695-89d57c03f59f" />

- **Hasil Deteksi dan Perhitungan Jarak**
<img width="748" height="374" alt="image" src="https://github.com/user-attachments/assets/9aa21269-faaf-43b0-a784-b51345c8c75c" />

- **Tabel Hasil Pemetaan Geospasial**
<img width="696" height="300" alt="image" src="https://github.com/user-attachments/assets/7d87c5f3-5714-468f-852a-ec1b3637cc2e" />

- **Visualisasi Peta Digital**
<img width="729" height="383" alt="image" src="https://github.com/user-attachments/assets/3789e057-638e-47de-879f-0cb3631ad614" />

<img width="817" height="424" alt="image" src="https://github.com/user-attachments/assets/ebb88fdf-4b6f-4f44-97b8-b0ec5f2a9b91" />



## 🌟 Fitur Utama
- **Deteksi Otomatis Menggunakan YOLOv11**: Sistem mendeteksi objek lubang jalan secara otomatis dari *frame* video menggunakan model YOLOv11 varian *small* (YOLOv11s).
- **Pelacakan Objek Pintar (BoT-SORT)**: Menggunakan algoritma pelacakan untuk memberikan identitas (ID) unik pada lubang yang terdeteksi. Algoritma ini digabungkan dengan batas *Region of Interest* (ROI) untuk mencegah penghitungan ganda (*double counting*) pada *frame* yang berurutan.
- **Koreksi Data GPS (*Map Matching*)**: Data koordinat mentah dari perangkat GPS disesuaikan ke jaringan jalan *OpenStreetMap* menggunakan metode *snap-to-road* dari *Open Source Routing Machine* (OSRM) secara lokal (menggunakan Docker).
- **Sinkronisasi Spasiotemporal**: Sistem melakukan sinkronisasi *frame* video dengan titik koordinat GPS berdasarkan *timestamp* (waktu), dan dilengkapi dengan pengaturan kompensasi latensi (*Time Offset*).
- **Visualisasi Geospasial Interaktif**: Hasil disajikan dalam *dashboard* Streamlit yang menampilkan video hasil deteksi beranotasi, tabel metrik jarak geodesik antar lubang, serta peta lokasi kerusakan berbasis Folium.

## Kalibrasi Sistem
- **Time offset atau Lookahead (detik)**: Parameter ini digunakan untuk menyelaraskan waktu antara video dan data GPS. Karena keduanya direkam oleh perangkat yang berbeda, sering terjadi perbedaan waktu. Dengan offset ini, posisi lubang dari video bisa dipasangkan dengan koordinat GPS yang sesuai.

- **Confidence Treshold**: Ini adalah batas minimum keyakinan model dalam mendeteksi objek. Jika nilai confidence di bawah threshold, maka deteksi akan diabaikan. Semakin tinggi nilainya, hasil deteksi lebih selektif, tetapi berisiko melewatkan beberapa lubang.

- **Garis Batas Jarak (dari atas layar)**: Parameter ini digunakan untuk menentukan area valid deteksi pada frame video. Tujuannya agar sistem hanya mengambil objek yang berada pada posisi tertentu, sehingga mengurangi deteksi yang tidak relevan, misalnya objek yang terlalu jauh.

-  **Jarak Visual antar Lubang (pixel)**: Parameter ini berfungsi untuk menghindari duplikasi deteksi. Jika dua deteksi terlalu dekat dalam jarak pixel tertentu, maka sistem akan menganggapnya sebagai satu lubang yang sama.

- **Batas Durasi Video (detik)**: Parameter ini membatasi durasi video yang diproses. Hal ini berguna untuk efisiensi komputasi dan mempercepat proses analisis, terutama jika video berdurasi panjang.


## 📊 Performa Sistem
Berdasarkan hasil pengujian lapangan pada dataset yang terdiri dari 1.158 citra, sistem ini mencatatkan performa sebagai berikut:
- **Kinerja Model YOLOv11s**: Memiliki nilai *Precision* sebesar 87,6%, *Recall* sebesar 95,6%, *F1-score* 91,4%, dan mAP@0.5 mencapai 97,0%.
- **Akurasi Perangkat GPS**: Modul GPS NEO-7M menghasilkan rata-rata kesalahan (*error*) deviasi jarak spasial sebesar 1,27 meter ketika divalidasi terhadap titik referensi pengukuran langsung di lapangan menggunakan Avenza Maps.

## 🛠️ Teknologi dan Perangkat yang Digunakan
### Perangkat Lunak (*Software*)
- Ultralytics (YOLOv11), OpenCV, dan Python.
- Pandas, Geopy, Streamlit, dan Folium.
- OSRM (*Open Source Routing Machine*) dan Docker.
- Arduino IDE dan C++.

### Perangkat Keras (*Hardware Data Logger*)
- Mikrokontroler Arduino Nano.
- Modul GPS Ublox NEO-7M dengan Antena Aktif Eksternal.
- Modul MicroSD Card Reader dan Layar OLED SSD1306.

## 🚀 Cara Menjalankan Sistem Secara Lokal

1. Kloning repositori ini ke komputer Anda:
   ```bash
   git clone [https://github.com/zakyy7/Pothole-Detection.git](https://github.com/zakyy7/Pothole-Detection.git)
   cd Pothole-Detection
2. Instalasi Pustaka (Dependencies)
Instal semua pustaka Python yang diperlukan agar sistem dapat berjalan tanpa kendala:
   ```bash
   pip install -r requirements.txt
3. Menjalankan Dashboard
Pastikan server OSRM lokal (Docker) sudah aktif jika ingin menggunakan fitur koreksi peta, lalu jalankan perintah:
   ```bash
   python -m streamlit run dashboard_streamlit.py
   

##👤 Penulis
Muhamad Zaki Mubarok
Program Studi Fisika, Universitas Islam Negeri Syarif Hidayatullah Jakarta
