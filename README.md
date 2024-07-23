# Laporan Proyek Machine Learning - Muhammad Abiya Makruf

## Project Overview

**Latar Belakang Proyek**

Seiring dengan perkembangan teknologi, pasar ponsel pintar semakin berkembang pesat dengan banyaknya pilihan model yang tersedia. Pengguna seringkali mengalami kesulitan dalam memilih ponsel yang tepat karena banyaknya pilihan yang ada. Selain itu, spesifikasi teknis yang kompleks dan beragam membuat proses pemilihan ponsel menjadi semakin sulit bagi konsumen awam. Hal ini menimbulkan masalah bagi pengguna yang ingin mendapatkan ponsel yang sesuai dengan kebutuhan mereka tanpa harus melalui proses penelitian yang memakan waktu.

Sistem rekomendasi adalah alat untuk berinteraksi dengan ruang informasi yang besar dan kompleks[1]. Oleh karena itu, penting untuk memiliki sistem rekomendasi yang dapat menyederhanakan proses ini dengan memberikan rekomendasi yang relevan berdasarkan kebutuhan dan preferensi pengguna. Dengan memanfaatkan data rating pengguna sebelumnya dan spesifikasi teknis ponsel, sistem rekomendasi ini diharapkan dapat membantu pengguna menemukan ponsel yang paling sesuai dengan kebutuhan mereka, baik dari segi performa, harga, maupun fitur lainnya.

**Pentingnya Proyek**

Proyek ini penting karena: 
- Peningkatan Pengalaman Pengguna: Membantu pengguna menemukan ponsel yang sesuai dengan preferensi mereka, meningkatkan kepuasan dan pengalaman pengguna.
- Efisiensi: Mengurangi waktu dan usaha yang dibutuhkan pengguna dalam mencari dan membandingkan berbagai model ponsel.
- Personalisasi: Memberikan rekomendasi yang dipersonalisasi berdasarkan data rating pengguna.

## Business Understanding

### Problem Statements
- Bagaimana kita bisa membantu pengguna menemukan ponsel yang paling sesuai dengan kebutuhan dan preferensi mereka?
- Bagaimana kita bisa membantu pengguna menemukan ponsel yang mirip dengan ponsel lamanya meskipun pengguna tidak mengerti spesifikasi teknis ponsel lamanya?

### Goals
Menjelaskan tujuan proyek yang menjawab pernyataan masalah:
- Mengembangkan sistem rekomendasi yang dapat memberikan daftar ponsel terbaik berdasarkan preferensi pengguna (Rating terhadap ponsel lama nya).
- Membangun sistem rekomendasi yang dapat memberikan daftar ponsel terbaik berdasarkan model ponsel lamanya (Contoh: iPhone XR).

### Solution statements
- Content-Based Filtering: Menggunakan fitur deskriptif dari ponsel (brand, model, dan operating system) untuk memberikan rekomendasi.
- Collaborative Filtering: Menggunakan data rating dari pengguna untuk memberikan rekomendasi berdasarkan kesamaan preferensi dengan pengguna lain.

## Data Understanding
Dataset yang digunakan berisi informasi mengenai berbagai model ponsel, termasuk brand, model, operating system, dan beberapa fitur lainnya. Dataset ini dapat diunduh dari [kaggle](https://www.kaggle.com/datasets/meirnizri/cellphones-recommendations/data).

Dataset terbagi menjadi 3 yaitu cellphones data, cellphones rating, dan cellphones users.

Cellphones data memiliki jumlah baris 33 dan kolom 32 tanpa missing value. Dataset ini berisikan tentang spesifikasi detail dari sebuah handphone, berikut adalah cellphones data info dan sampel data:
|   Column           |  Dtype   |
|--------------------|----------|
| cellphone_id       | int64    |
| brand              | object   |
| model              | object   |
| operating system   | object   |
| internal memory    | int64    |
| RAM                | int64    |
| performance        | float64  |
| main camera        | int64    |
| selfie camera      | int64    |
| battery size       | int64    |
| screen size        | float64  |
| weight             | int64    |
| price              | int64    |
| release date       | object   |

|index|cellphone\_id|brand|model|operating system|internal memory|RAM|performance|main camera|selfie camera|battery size|screen size|weight|price|release date|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|0|0|Apple|iPhone SE \(2022\)|iOS|128|4|7\.23|12|7|2018|4\.7|144|429|18/03/2022|
|1|1|Apple|iPhone 13 Mini|iOS|128|4|7\.72|12|12|2438|5\.4|141|699|24/09/2021|

Cellphones rating memiliki 990 baris dan 3 kolom tanpa missing value. Dataset ini berisikan nilai rating yang diberikan user X terhadap cellphone Y, berikut adalah cellphones rating info dan sampel data:

|   Column           |  Dtype   |
|--------------------|----------|
| user_id            | int64    |
| cellphone_id       | int64    |
| rating             | int64    |

|index|user\_id|cellphone\_id|rating|
|---|---|---|---|
|0|0|30|1|
|1|0|5|3|

Cellphones users memiliki 99 baris dan 4 kolom tanpa missing value. Dataset ini berisikan data-data dari seorang user, berikut adalah cellphones users info dan sampel data:

|   Column           |  Dtype   |
|--------------------|----------|
| user_id            | int64    |
| age                | int64    |
| gender             | object   |
| occupation         | object   |

|index|user\_id|age|gender|occupation|
|---|---|---|---|---|
|0|0|38|Female|Data analyst|
|1|1|40|Female|team worker in it|

Variabel-variabel pada dataset adalah sebagai berikut:
- data:
  - `cellphone_id`: ID unik untuk setiap ponsel.
  - `brand`: Merek ponsel.
  - `model`: Model ponsel.
  - `operating system`: Sistem operasi ponsel.
  - `internal memory`: Memori internal ponsel dalam GB.
  - `RAM`: RAM ponsel dalam GB.
  - `performance`: Skor kinerja ponsel.
  - `main camera`: Resolusi kamera utama dalam MP.
  - `selfie camera`: Resolusi kamera depan dalam MP.
  - `battery size`: Kapasitas baterai dalam mAh.
  - `screen size`: Ukuran layar dalam inci.
  - `weight`: Berat ponsel dalam gram.
  - `price`: Harga ponsel dalam USD.
  - `release date`: Tanggal rilis ponsel.

- rating:
  - `user_id`: ID unik untuk setiap pengguna.
  - `cellphone_id`: ID unik untuk setiap ponsel (mengacu pada cellphones_data).
  - `rating`: Rating yang diberikan pengguna untuk ponsel tertentu (skala 1-10).

- users:
  - `user_id`: ID unik untuk setiap pengguna.
  - `age`: Usia pengguna.
  - `gender`: Jenis kelamin pengguna.
  - `occupation`: Pekerjaan pengguna.

**Exploratory Data Analysis (EDA)**
- Distribusi Brand Ponsel
  ![Distribusi Brand Ponsel](https://raw.githubusercontent.com/AbiyaMakruf/Dicoding-ModelSistemRekomendasi/main/image/image1.png)
  - Gambar ini menunjukkan jumlah ponsel dari berbagai brand dalam dataset.
  - Samsung memiliki jumlah ponsel terbanyak dengan 8 unit.
  - Apple berada di posisi kedua dengan 6 unit.
  - Brand Asus, Oppo, Vivo, dan Sony memiliki jumlah yang lebih sedikit.  

- Distribusi Sistem Operasi Ponsel
  ![Distribusi Sistem Operasi Ponsel](https://raw.githubusercontent.com/AbiyaMakruf/Dicoding-ModelSistemRekomendasi/main/image/image2.png)
  - Gambar ini menunjukkan distribusi sistem operasi pada ponsel dalam dataset.
  - Android merupakan sistem operasi yang paling umum dengan lebih dari 25 unit.
  - iOS memiliki sekitar 6 unit.

- Distribusi Tahun Rilis Ponsel
  ![Distribusi Tahun Rilis Ponsel](https://raw.githubusercontent.com/AbiyaMakruf/Dicoding-ModelSistemRekomendasi/main/image/image3.png)
  - Gambar ini menunjukkan distribusi ponsel berdasarkan tahun rilis.
  - Tahun 2021 dan 2022 memiliki jumlah ponsel yang hampir sama, masing-masing lebih dari 15 unit.
  - Tahun 2018 memiliki jumlah ponsel yang sangat sedikit.
  - Ponsel yang dirilis pada tahun 2018 memiliki representasi yang sangat rendah, menunjukkan bahwa dataset lebih fokus pada model-model terbaru.

- Distribusi Rating Ponsel
  ![Distribusi Tahun Rilis Ponsel](https://raw.githubusercontent.com/AbiyaMakruf/Dicoding-ModelSistemRekomendasi/main/image/image4.png)
  - Gambar ini menunjukkan distribusi rating yang diberikan pengguna untuk ponsel dalam dataset.
  - Rating 8 adalah yang paling umum, diikuti oleh rating 7 dan 10.
  - Rating yang lebih rendah, seperti 2, 3, dan 4, memiliki jumlah yang lebih sedikit.
  - Sebagian besar ponsel dalam dataset mendapatkan rating yang cukup tinggi (7-10).

## Data Preparation
**Teknik Data Preparation**
- Menggabungkan dataset menjadi satu.
- Handling Missing Values: Menghapus atau mengisi nilai yang hilang dalam dataset.
- Removing Outliers: Menghapus data yang memiliki nilai outliers pada kolom tertentu.
- Mengubah format penulisan. 
- Mereplace value.
- Pembagian dataset untuk train-test (80:20).

**Proses Data Preparation**
- Menggabungkan dataset cellphones data, cellphones rating, dan cellphones user menjadi satu dataframe.
- Menghapus nilai Null pada kolom `occupation`.
- Menghapus outlier pada kolom `rating` yang memiliki value 18.
- Menghapus outlier pada kolom `gender` yang memiliki value -Select Gender-.
- Value pada kolom `occupation` diubah menjadi lowercase.
- Terdapat beberapa penulisan yang salah pada kolom `occupation`. Value 'Healthare' menjadi 'healthcare' dan value 'it' menjadi 'information technology'.
- Pembagian dataset train-test dengan skema 80:20.

|                  | Jumlah |
|------------------|------- |
| Whole Dataset    | 989    |
| Train            | 791    |
| Test             | 198    |

**Alasan Tahapan Data Preparation**
  - Handling Missing Values: Untuk memastikan tidak ada data yang hilang yang dapat mempengaruhi hasil analisis dan model.
  - Removing outliers : Untuk meningkatkan akurasi model dengan menghilangkan data yang dapat mempengaruhi performa model.
  - Mengubah format penulisan : Memasatikan bahwa memiliki value format penulisan yang sama sehingga tidak dianggap sebagai data yang berbeda padahal value yang sama.
  - Mereplace value : Mengubah beberapa nilai karena memiliki kesalahan penulisan.

## Modeling
Pada tahap ini akan membahas dua pendekatan utama yang digunakan dalam membangun sistem rekomendasi: Content-Based Filtering dan Collaborative Filtering. Berikut adalah penjelasan lebih lanjut mengenai parameter yang digunakan, kelebihan, dan kekurangan dari masing-masing pendekatan, serta beberapa potongan kode yang relevan.

**Model Sistem Rekomendasi Content Based Filtering**

Content-Based Filtering menggunakan deskripsi dan fitur dari item itu sendiri untuk memberikan rekomendasi. Berikut adalah parameter untuk pendekatan ini.

Parameter yang Digunakan:
  - TF-IDF Vectorizer: Untuk mengubah deskripsi teks menjadi vektor numerik.
  - Cosine Similarity: Untuk menghitung kesamaan antara vektor item.

Tahapan proses:

- Karena TF-IDF hanya cocok untuk data teks maka hanya kolom yang bertipe object saja yang dipilih.
  ```python
  #Menyiapkan dataframe
  phone_new = pd.DataFrame({
      'cellphone_id': cellphone_id,
      'brand': brand,
      'model': model,
      'operating_system': operating_system,
  })
  ```

- Membangun sistem rekomendasi menggunakan TfidfVectorizer()
  ```python
  #Inisialisasi TfidfVectorizer
  tf = TfidfVectorizer()
  
  #Melakukan perhitungan idf pada data brand
  tf.fit(data['brand']) 

  #Melakukan fit lalu ditransformasikan ke bentuk matrix
  tfidf_matrix = tf.fit_transform(data['brand']) 
  ```

- Menghitung derajat kesamaan (similarity degree)
  ```python
  #Menghitung cosine_similatiry
  cosine_sim = cosine_similarity(tfidf_matrix)

  #Membuat dataframe dari variabel cosine_sim
  cosine_sim_df = pd.DataFrame(cosine_sim, index=data['model'], columns=data['model'])
  ```

- Membangun fungsi model_recommendations 
  ```python
  #Code lengkap pada .ipynb
  def model_recommendations(model, similarity_data=cosine_sim_df, items=phone_new[['model','brand','operating_system']], k=4):
  ```

**Top-N Recommendation Content Based Filtering**

Menampilkan hasil rekomendasi
  - model_recommendations('iPhone XR')

  |index|model|brand|operating\_system|
  |---|---|---|---|
  |0|iPhone 13 Pro Max|Apple|iOS|
  |1|iPhone SE \(2022\)|Apple|iOS|
  |2|iPhone 13 Pro|Apple|iOS|
  |3|iPhone 13|Apple|iOS|

  - model_recommendations('Galaxy S22')

  |index|model|brand|operating\_system|
  |---|---|---|---|
  |0|Galaxy S22 Plus|Samsung|Android|
  |1|Galaxy Z Fold 3|Samsung|Android|
  |2|Galaxy S22 Ultra|Samsung|Android|
  |3|Galaxy A13|Samsung|Android|

**Model Sistem Rekomendasi Collaborative Filtering (Alternatif)**
Collaborative Filtering menggunakan interaksi pengguna-item (rating) untuk memberikan rekomendasi. Berikut adalah parameter untuk pendekatan ini.

Parameter yang Digunakan:
  - loss = BinaryCrossentropy
  - optimizer = Adam
  - learning_rate = 0.001
  - metrics = RootMeanSquaredError

Tahapan proses:
- Menyiapkan dataframe
  ```python
  #Membaca dataset
  df = rating
  ```

- Membersihkan dataset seperti drop missing value dan handle outliers.

- Melakukan acak terhadap dataset.
  ```python
  #Mengacak dataset
  df = df.sample(frac=1, random_state=42)
  ```

- Membagi data train-test (80:20)

- Membuat class RecommenderNet

- Inisialisasi dan compile model
  ```python
  #inisialisasi model
  model = RecommenderNet(num_users, num_cellphone, 50) 

  #model compile
  model.compile(
      loss = tf.keras.losses.BinaryCrossentropy(),
      optimizer = keras.optimizers.Adam(learning_rate=0.001),
      metrics=[tf.keras.metrics.RootMeanSquaredError()]
  )
  ```
- Training model
  ```python
  #Memulai training
  history = model.fit(
      x = x_train,
      y = y_train,
      batch_size = 8,
      epochs = 100,
      validation_data = (x_val, y_val)
  )
  ```
**Top-N Recommendation Collaborative Filtering (Alternatif)**
```
Showing recommendations for users: 237
===========================
cellphone with high ratings from user
--------------------------------
Oppo : Find X5 Pro
OnePlus : Nord 2T
OnePlus : 10 Pro
Apple : iPhone 13
Xiaomi : 11T Pro
--------------------------------
Top 10 cellphone recommendation
--------------------------------
Apple : iPhone XR
Samsung : Galaxy S22
Samsung : Galaxy A53
Vivo : X80 Pro
Apple : iPhone 13 Pro
Apple : iPhone SE (2022)
Samsung : Galaxy S22 Ultra
Apple : iPhone 13 Mini
Google : Pixel 6 Pro 
Apple : iPhone 13 Pro Max
```
**Kelebihan dan Kekurangan Pendekatan**

1. Content Based Filtering
  - Kelebihan:
    - Independensi Data Pengguna: Tidak memerlukan data pengguna lain, hanya berdasarkan item itu sendiri.
    - Rekomendasi Baru: Dapat merekomendasikan item baru yang belum pernah diulas sebelumnya.

  - Kekurangan:
    - Keterbatasan Fitur: Hanya dapat merekomendasikan item yang memiliki fitur serupa.
    - Overfitting: Terkadang terlalu fokus pada fitur tertentu dan tidak bisa merekomendasikan item yang berbeda secara radikal.

2. Collaborative Filtering
  - Kelebihan:
    - Menggunakan Data Pengguna: Memanfaatkan interaksi pengguna-item sehingga dapat merekomendasikan item yang tidak mirip tetapi disukai oleh pengguna dengan preferensi serupa.
    - Menangani Data Besar: Dapat bekerja dengan data besar dan menemukan pola-pola kompleks.

  - Kekurangan:
    - Cold Start Problem: Kesulitan merekomendasikan item baru atau kepada pengguna baru yang belum memiliki cukup interaksi.

*Untuk tahapan proses yang lebih lengkap silahkan baca [Dicoding_ModelSistemRekomendasi.ipynb](https://github.com/AbiyaMakruf/Dicoding-ModelSistemRekomendasi/blob/main/Dicoding_ModelSistemRekomendasi.ipynb)*

## Evaluation
Pada bagian ini, akan mengevaluasi model rekomendasi yang telah dibangun menggunakan metrik evaluasi yang tepat. Untuk model prediksi rating, kita akan menggunakan Root Mean Squared Error (RMSE) sebagai metrik evaluasi. Selain itu, akan mengevaluasi apakah proyek ini berhasil menjawab problem statement dan memberikan solusi yang diinginkan.

**Metrik Evaluasi**

Root Mean Squared Error (RMSE) adalah akar kuadrat dari rata-rata kuadrat kesalahan. Ini memberikan gambaran seberapa jauh prediksi model berbeda dari nilai sebenarnya dalam satuan yang sama dengan variabel yang diprediksi. RMSE sangat berguna karena memberikan penalti lebih besar untuk kesalahan yang lebih besar.

$$
\text{RMSE} = \sqrt{\frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2}
$$

Di mana:

  - $y_i$ adalah nilai sebenarnya
  - $\hat y_i$ adalah nilai prediksi
  - $n$ adalah jumlah observasi

- RMSE yang kecil mengindikasikan bahwa model memiliki performa yang baik karena kesalahan antara prediksi dan nilai aktual rendah.
- RMSE yang besar mengindikasikan bahwa model memiliki performa yang buruk karena kesalahan antara prediksi dan nilai aktual tinggi.

**Hasil Proyek**
|            | Train  | Test  |
|------------|------- |-------|
| RMSE       | 0.2063 | 0.6416|


![Grafik train vs test](https://raw.githubusercontent.com/AbiyaMakruf/Dicoding-ModelSistemRekomendasi/main/image/image6.png)

RMSE yang dihitung memberikan indikasi bahwa model prediksi rating memiliki tingkat kesalahan yang dapat diterima, sehingga memadai untuk tujuan rekomendasi.

**Evaluasi Terhadap Business Understanding**
- Menjawab Problem Statement: Model yang dibuat berhasil menjawab problem statement dengan memberikan rekomendasi ponsel berdasarkan model yang ada dan memprediksi rating ponsel yang belum diulas oleh pengguna. Pendekatan content-based filtering menggunakan model ponsel untuk memberikan rekomendasi yang relevan berdasarkan kesamaan model, brand, dan operating system, sementara collaborative filtering memanfaatkan interaksi pengguna-item (rating) untuk menemukan pola preferensi pengguna.

- Mencapai Goals: Model content-based filtering dengan cosine similarity dan collaborative filtering dengan RecommenderNet berhasil mencapai tujuan untuk memberikan rekomendasi ponsel yang relevan. Content-based filtering menggunakan data deskriptif yaitu model ponsel untuk membuat profil item, sehingga meningkatkan akurasi rekomendasi dengan memperhitungkan kesamaan fitur (model, brand, operating system). Di sisi lain, collaborative filtering memanfaatkan data rating dari pengguna untuk menemukan pola preferensi dan merekomendasikan ponsel yang sesuai dengan kesukaan pengguna.

- Dampak dari Solution Statement: Penggunaan beberapa pendekatan algoritma (content-based dan collaborative filtering) dan teknik evaluasi seperti RMSE memberikan dampak positif dengan meningkatkan relevansi dan akurasi rekomendasi. Content-based filtering memastikan bahwa model ponsel yang diberikan dapat memberikan rekomendasi yang relevan dengan ponsel lamanya karena mempertimbangkan kesamaan fitur (model, brand, operating system) bagi pengguna. Sementara itu, collaborative filtering memungkinkan sistem untuk memahami preferensi pengguna berdasarkan interaksi sebelumnya, memberikan rekomendasi yang dipersonalisasi. Solusi yang direncanakan memberikan hasil yang signifikan dalam mencapai tujuan proyek, memastikan bahwa rekomendasi yang diberikan sesuai dengan kebutuhan dan preferensi pengguna.

## Kesimpulan
Dengan menggunakan kedua pendekatan ini, kita dapat membangun sistem rekomendasi yang lebih robust dan fleksibel. Content-Based Filtering cocok untuk memberikan rekomendasi berdasarkan fitur-fitur item itu sendiri, sementara Collaborative Filtering efektif dalam menemukan pola-pola preferensi pengguna dari data interaksi yang ada. Memahami kelebihan dan kekurangan masing-masing pendekatan membantu kita memilih metode yang paling sesuai dengan kebutuhan dan konteks spesifik dari sistem rekomendasi yang sedang dibangun.

## Referensi

[1] [R. Burke, A. Felfernig, and M. H. Göker, “Recommender Systems: An Overview”, AIMag, vol. 32, no. 3, pp. 13-18, Jun. 2011.](https://ojs.aaai.org/aimagazine/index.php/aimagazine/article/view/2361) 