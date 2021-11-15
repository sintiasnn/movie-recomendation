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
Data yang digunakan untuk projek kali ini yaitu movie lens small latest dataset yang diunduh dari kaggle. (https://www.kaggle.com/shubhammehta21/movie-lens-small-latest-dataset).

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

Pada data loading, telah dideklarasikan variabel yang akan dipakai. Variable tersebut diantaranya :

- movies : informasi tentang film.
- links : informasi yang digunakan untuk menautkan film ke sumber lainnya. 
- ratings : penilaian pada film.
- tags : informasi (metadata) yang dibuat tentang film tersebut. 

tahap eksplorasi dilakukan untuk memahami variable serta menemukan korelasi antar variable. 

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

## Modeling
Tahapan ini membahas mengenai model sisten rekomendasi yang Anda buat untuk menyelesaikan permasalahan. Sajikan top-N recommendation sebagai output.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menyajikan dua solusi rekomendasi dengan algoritma yang berbeda.
- Menjelaskan kelebihan dan kekurangan dari solusi/pendekatan yang dipilih.

## Evaluation
Pada bagian ini Anda perlu menyebutkan metrik evaluasi yang digunakan. Kemudian, jelaskan hasil proyek berdasarkan metrik evaluasi tersebut.

Ingatlah, metrik evaluasi yang digunakan harus sesuai dengan konteks data, problem statement, dan solusi yang diinginkan.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan formula metrik dan bagaimana metrik tersebut bekerja.

**---Ini adalah bagian akhir laporan---**

_Catatan:_
- _Anda dapat menambahkan gambar, kode, atau tabel ke dalam laporan jika diperlukan. Temukan caranya pada contoh dokumen markdown di situs editor [Dillinger](https://dillinger.io/), [Github Guides: Mastering markdown](https://guides.github.com/features/mastering-markdown/), atau sumber lain di internet. Semangat!_
- Jika terdapat penjelasan yang harus menyertakan code snippet, tuliskan dengan sewajarnya. Tidak perlu menuliskan keseluruhan kode project, cukup bagian yang ingin dijelaskan saja.
