# Laporan Proyek Machine Learning - Ni Putu Sintia Wati

## Project Overview

Film menjadi salah satu hiburan yang tidak terkalahkan. Seiring perkembangan jaman, munculnya berbagai genre film serta layanan streaming yang beragam. Beragamnya genre membuat penikmat film bingung akan film yang ingin ditontonnya. Dalam hal ini, sistem rekomendasi menjadi salah satu cara untuk memberi saran kepada penikmat film tersebut.

## Business Understanding

### Problem Statements

- Bagaimana layanan streaming dapat merekomendasikan film lain yang mungkin disukai dan belum pernah ditonton oleh penikmat film?

### Goals

- Menghasilkan sejumlah rekomendasi film yang sesuai dengan preferensi pengguna dan belum pernah ditonton sebelumnya dengan teknik collaborative filtering.

### Solution statements
pada kasus ini, kami mengajukan metode collaborative filtering sebagai solusi permasalahan diatas.

- **collaborative filtering**\
collaborative filtering merupakan teknik penyeleksian pada sistem rekomendasi yang memanfaatkan kesamaan antara pengguna dan item secara bersamaan untuk memberi rekomendasi. Collaborative filtering terdiri dari dua kategori, yaitu: model based (metode berbasis model machine learning) dan memory based (metode berbasis memori).

## Data Understanding
Data yang digunakan untuk projek kali ini yaitu movie lens small latest dataset yang diunduh dari [kaggle](https://www.kaggle.com/shubhammehta21/movie-lens-small-latest-dataset).

file yang terdapat pada dataset diatas adalah sebagai berikut:

- links.csv
- movies.csv
- ratings.csv
- tags.csv

### Data Loading
Memuat data yang akan digunakan untuk modeling nanti. Pertama, data akan didownload melalui kaggle. Lalu file tersebut akan diekstrak agar file dapat digunakan. Tidak lupa juga untuk import library yang akan digunakan. Setelah itu, lalu kita tampilkan jumlah data pada masing-masing file dengan fungsi len().

### Exploratory Data Analysis
Exploratory Data Analysis (EDA) merupakan proses pengenalan data untuk menganalisis karakteristik, menemukan pola, anomali dan memeriksa asumsi data.

**Unvariate EDA** 

Pada data loading, terdapat variable yang akan digunakan, diantaranya :
- movies : informasi tentang film.
- links : informasi yang digunakan untuk menautkan film ke sumber lainnya. 
- ratings : penilaian pada film.
- tags : informasi (metadata) yang dibuat tentang film tersebut. 

Tahap EDA dilakukan untuk memahami variable serta menemukan korelasi antar variable. 

**Movies Variable**

 ![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/001.jpg?raw=true)
  ![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/002.jpg?raw=true)
  
Berdasarkan hasil diatas, terdapat 9742 entri. terdapat dua variable diantaranya, movieId, title, dan genres.

- MovieId merupakan ID film,
- title merupakan judul film, dan
- genres merupakan jenis/gaya film.

**Links Variable**

 ![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/003.jpg?raw=true)
  ![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/004.jpg?raw=true)

Berdasarkan info diatas, terdapat 9742 entri pada imdbId dan 9734 untuk tmdbId. Terdapat beberapa variable diantaranya :

- movieId untuk ID film,
- imdbId untuk ID imdb (internet movie database), dan
- tmdbId untuk ID tmdb (the movie database).

**Ratings Variable**

 ![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/005.jpg?raw=true)
  ![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/006.jpg?raw=true)
  
Berdasarkan info diatas, terdapat 100836 entri data. Terdapat beberapa variable diantaranya

- userId untuk ID pengguna,
- movieId untuk ID film,
- rating untuk penilaian film yang secara umum rentang nilainya dari 0 hingga 5,
- Timestamp merupakan waktu default UTC yang terhitung sejak 1 januari 1970

Kita dapat check nilai minimun pada data rating dengan fungsi *describe()*

![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/009.jpg?raw=true)

Hasilnya, rating terendah yaitu 0 dan rating tertinggi yaitu 5.

**Tags Variable**

 ![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/007.jpg?raw=true)
  ![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/008.jpg?raw=true)
  
Berdasarkan informasi diatas, terdapat 3683 entri data. Terdapat beberapa variabel yang digunakan, diantaranya :

- userId untuk ID user,
- movieId untuk Id film,
- tag untuk kata kunci yang disisipkan untuk pencarian film,
- timestamp untuk waktu default UTC yang terhitung sejak 1 januari 1970.

## Data Preparation
Data preparation bertujuan untuk menyiapkan data sebelum masuk ke proses modeling. Selain itu, data preparation juga berguna untuk meningkatkan akurasi saat training data. Pada dataset ini, yang akan kita lakukan yaitu menggabungkan dataset dengan fungsi merge() dan key movieId, menghapus missing value serta menurut dataset berdasarkan movieId serta menghapus hasil duplikat.

1. Menggabungkan bagian-bagian dataset dengan key movieId\
Tujuan penggabungan dataset agar meminimalisir terjadinya missing value. Penggabungan dataset menggunakan key movieId dengan perintah *pd.merge()*
  ![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/011.jpg?raw=true)
  
2. Melihat jumlah missing value\
Missing Value menjadi penyebab berkurangnya akurasi pada saat proses training. Untuk mengetahui jumlah missing value dari masing-masing variable kita dapat menggunakan fungsi *isnull().sum()*. Hasilnya tidak terdapat missing value pada masing-masing variable. 
![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/012.jpg?raw=true)
  
3. Membuat variabel sorting untuk mengurutkan data berdasarkan movieId\
Sorting dilakukan untuk menentukan data-data yang duplikat. 
![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/013.jpg?raw=true)

4. Menghapus data duplikat\
Menghapus data duplikat bertujuan untuk meningkatkan akurasi saat proses training. 
![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/014.jpg?raw=true)

Tahap berikutnya, kita akan membuat dictionary untuk menentukan pasangan key-value pada data movies_id, movies_title, dan movies_genre yang telah disiapkan sebelumnya. Pembuatan dictionary dilakukan setelah proses penghapusan data duplikat. Proses membuat dictionary berguna untuk mendapatkan data unik untuk memudahkan dalam proses pemodelan.
![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/016.jpg?raw=true)


## Modeling

Collaborative filtering merupakan salah satu metode untuk membuat sistem rekomendasi. Teknik ini membutuhkan data rating dari user.

Goal proyek kali ini adalah menghasilkan rekomendasi 10 film yang sesuai dengan preferensi pengguna berdasarkan rating yang telah diberikan sebelumnya. Dari data rating pengguna, akan mengidentifikasi film-film yang mirip dan belum pernah ditonton oleh pengguna untuk direkomendasikan.

Sampel user diambil secara acak kemudian didefinisikan sebagai variabel movie_not_visited yang merupakan daftar film yang belum pernah di putar oleh user. Pada kasus ini, kita mendapatkan user dengan userId 91. 

Untuk memperoleh rekomendasi film menggunakan fungsi *model.predict()* dari library keras, dari output tersebut kita dapat membandingkan antara Movie with high ratings from user dan Top 10 movie recomendation.

![hasil dari user 91](https://github.com/sintiasnn/movie-recomendation/blob/main/023.jpg?raw=true)

Hasil diatas menunjukkan kategori film dengan sesuai dengan rating user. Pada 10 top recommendation terdapat 2 film genre adventure, 3 film genre crime, dan 5 film genre drama. 


## Evaluation
Metrik yang digunakan pada kasus ini, yaitu RMSE (Root Mean Squared Error) yaitu menghitung rata-rata kuadrat kesalahan antara label dan prediksi. Dari proses ini, memperoleh nilai error akhir sebesar sekitar 0.17 dan error pada data validasi sebesar 0.19.

  ![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/022.jpg?raw=true)
  
