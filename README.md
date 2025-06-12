# BorobudurTales - Machine Learning

Repositori ini merupakan implementasi sistem **Content-Based Image Retrieval (CBIR)** untuk mencocokkan gambar relief Candi Borobudur. Proyek ini memanfaatkan **ResNet50** dari TensorFlow untuk mengekstraksi fitur visual, dan menghitung **cosine similarity** untuk menemukan gambar yang paling mirip dari basis data.

Sistem ini bertujuan mendukung pelestarian budaya dengan pendekatan visual berbasis machine learning.

Nilai similarity berada di rentang **\[-1, 1]**, dengan `1` berarti sangat mirip. Sistem ini mendukung pelestarian budaya dengan pendekatan visual berbasis machine learning.

---

## Dataset 
Dataset berisi 160 gambar relief Karmawibhangga dari Candi Borobudur, tersedia di [Kaggle](https://www.kaggle.com/datasets/adisatya/karmawibhangga-of-borobudur), dilengkapi dengan narasi dari Google Sheets [di sini](https://docs.google.com/spreadsheets/d/19FgNjbtW3yRQki3Hm16saZC3NYi7OMmkDkOBfNGcjbk/edit?usp=sharing).
Dataset berisi gambar-gambar relief dengan ukuran yang bervariasi. Untuk memastikan proses ekstraksi fitur berjalan optimal, seluruh gambar perlu diseragamkan ukurannya terlebih dahulu.Variabel yang terdapat dalam dataset narasi cerita yaitu,

| Kolom         | Keterangan                                           |
|---------------|------------------------------------------------------|
| `Filename`    | Nama file gambar                                     |
| `Tema`        | Tema besar cerita                                    |
| `Narasi`      | Cerita/narasi dari relief                            |
| `Makna moral` | Nilai moral yang terkandung dalam cerita             |

---

## Arsitektur Model 
Membangun image similarity menggunakan ResNet50 deep learning-based features yang digunakan untuk ekstrak fitur level tinggi untuk merepsentasikan konten visual. mengukur kesamaan antara dua gambar dengan metrik **cosine similarity**
Sistem dibangun dengan pipeline sebagai berikut:
###  1. Data Preparation
  - Gambar-gambar dari dataset memiliki ukuran yang berbeda-beda.

  - Semua gambar diubah ukurannya menjadi 224 x 224 piksel agar sesuai dengan input standar ResNet50.

  - Gambar valid memiliki ekstensi: .jpg

  - Gambar diproses menjadi array NumPy dan dilakukan normalisasi menggunakan preprocess_input() dari TensorFlow.
Hasil akhir image Preparation  
    ![image](https://github.com/user-attachments/assets/b61cc404-e4cc-43d7-a54e-d1bbd659e456)
###  2. Feature Extraction
  - Menggunakan model ResNet50 dengan include_top=False dan pooling='avg'.

  - Model menghasilkan vektor fitur berdimensi 2048 untuk setiap gambar.
### 3. Similarity Search
  - Gambar masukan (query) dibandingkan dengan database menggunakan cosine similarity.

  - Nilai similarity berkisar antara -1 hingga 1, di mana 1 berarti sangat mirip.

  - Top-K gambar paling mirip diurutkan berdasarkan skor tertinggi.
### 4. Simpan Fitur
  - Fitur disimpan dalam file `.h5` untuk efisiensi dan akses cepat
### 5. Integrasi hasil model image similarity dengan dataset narasi 
  - Proses integrasi dilakukan menggunakan **Flask API**
  - Endpoint: `POST /predict` menerima gambar dan mengembalikan narasi terkait
### 6. Deployment

---
## Arsitektur Model Resnet Without Top Layer

![Screenshot 2025-06-11 213442](https://github.com/user-attachments/assets/5db903ed-4909-4aa1-9e91-e1b0ea20b918)

---
## Evaluasi 
- Precission@k : Berarti kita melihat berapa banyak gambar yang benar-benar mirip dari k gambar teratas yang ditampilkan. hal ini mendapatkan, Precision@5 sebesar 1.0 (100%)
- Threshold Similarity : Nilai kemiripan gambar lebih tinggi dari nilai 0,5 maka dianggap cukup mirip. Kalau dibawah, dianggap tidak mirip
  - Akurasi : 72,97%

---
## Requirements dan Installation
**1. Clone Repository**
```
git clone https://github.com/rezanagita/DBS-CAPSTONE-ML.git
cd capstone
```
**2. Buat Virtual Environment**
```
python -m venv env
source env/bin/activate      # Mac/Linux
env\Scripts\activate         # Windows
```
**3. Intall Dependencies**
```
pip install -r requirements.txt
```
**4. Jalankan Proyek**
```
python app.py
```
**5. Contoh Penggunaan**
```md
1. Upload gambar relief melalui endpoint API.
2. Sistem akan mencari gambar paling mirip dari dataset.
3. Narasi dan nilai moral dari gambar ditampilkan sebagai respons.
```
