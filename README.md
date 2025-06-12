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
Deployment Model menggunakan [HuggingFace](https://andre770-borobudurrr.hf.space/)

---
## Arsitektur Model Resnet Without Top Layer

![image](https://github.com/user-attachments/assets/c8f72d33-2f31-48cf-b6d5-4379abf652db)

---
## Hasil & Evaluasi 
**Input Image**
<p align="center"> <img src="https://github.com/user-attachments/assets/01a769b9-de72-45cb-a812-a1ef5c0b7487" width="500"/> </p>

| Rank |                                                   Image                                                  | Similarity |
| :--: | :------------------------------------------------------------------------------------------------------: | :--------: |
|   1  | <img src="https://github.com/user-attachments/assets/61b351b5-00c0-48dd-a981-2be702ecd7e0" width="500"/> | **1.0000** |
|   2  | <img src="https://github.com/user-attachments/assets/fcc06c02-4ec5-4041-bac8-a05ad66429e7" width="500"/> | **0.9012** |
|   3  | <img src="https://github.com/user-attachments/assets/bbb37b9a-c775-41b0-9bf2-9bf872a77af2" width="500"/> | **0.8980** |



- Precission@k : Berarti kita melihat berapa banyak gambar yang benar-benar mirip dari k gambar teratas yang ditampilkan.
    - Hasil Precision@5 with threshold 0.5: 1.0000
- Threshold Similarity : Nilai kemiripan gambar lebih tinggi dari nilai 0,5 maka dianggap cukup mirip. Kalau dibawah, dianggap tidak mirip

---
## Requirements dan Installation
```
git clone https://github.com/rezanagita/BorobudurTales-ML-Model.git
cd borobudurTales-Model
pip install -r requirements.txt
```


