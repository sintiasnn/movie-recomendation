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

Menggabungkan dataset dengan key movieId.

  ![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/011.jpg?raw=true)
  
Melihat jumlah missing value. 

  ![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/012.jpg?raw=true)
  
 Membuat variabel sorting untuk mengurutkan data berdasarkan movieId.
 
   ![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/013.jpg?raw=true)

Menghapus data duplikat. 

  ![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/014.jpg?raw=true)

Selanjutnya, kita perlu melakukan konversi data series menjadi list. Dalam hal ini, kita menggunakan fungsi tolist() dari library numpy. Lalu cek jumlah masing-masing variable. 

  ![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/015.jpg?raw=true)

hasilnya : 
- movies_id = 9724
- movies_title = 9724
- movies_genre = 9724

Tahap berikutnya, kita akan membuat dictionary untuk menentukan pasangan key-value pada data movies_id, movies_title, dan movies_genre yang telah disiapkan sebelumnya.

```python
# membuat kamus data 
movies_final = pd.DataFrame({
    'id' : movies_id,
    'title' : movies_title,
    'genre' : movies_genre
})

movies_final

```

  ![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/016.jpg?raw=true)


## Modeling

Collaborative filtering merupakan salah satu metode untuk membuat sistem rekomendasi. Teknik ini membutuhkan data rating dari user.

Goal proyek kali ini adalah menghasilkan rekomendasi sejumlah film yang sesuai dengan preferensi pengguna berdasarkan rating yang telah diberikan sebelumnya. Dari data rating pengguna, akan mengidentifikasi film-film yang mirip dan belum pernah ditonton oleh pengguna untuk direkomendasikan.

### Data Understanding
Pada penerapan model ini, file yang digunakan yaitu file **rating.csv**. Agar tidak tertukar dengan fitur rating yang digunakan sebelumnya, kita namakan file menjadi variabel *df*
  
  ![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/017.jpg?raw=true)

### Data Preparation
Pada tahap ini, perlu dilakukan persiapan data untuk menyandikan (encode) fitur ‘UserId’ dan ‘MovieId’ ke dalam indeks integer.

```python
#mengubah userID menjadi list unique
userId_enc = df['userId'].unique().tolist()

#encoding userId
userId_enc_final = {x: i for i, x in enumerate(userId_enc)}

#encoding hasil encoding sebelumnya ke user
userId_enc_to_user = {i: x for i, x in enumerate(userId_enc)}

#mengubah movieId menjadi list unique
movieId_enc = df['movieId'].unique().tolist()

#encoding movieId
movieId_enc_final = {x: i for i, x in enumerate(movieId_enc)}

#encoding angka ke movieId
movieId_enc_to_movie = {i: x for i, x in enumerate(movieId_enc)}

#petakan UserID dan movieID yang telah di-encoding sebelumnya
df['user'] = df['userId'].map(userId_enc_final)
df['movie'] = df['movieId'].map(movieId_enc_final)

```
Terakhir, cek beberapa hal dalam data seperti jumlah user, jumlah resto, dan mengubah nilai rating menjadi float.

  ![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/018.jpg?raw=true)
  
Hasilnya : 
- num_user = 610
- num_movie = 9724
- min_rating = 0.5
- max_rating = 5.0

Selanjutnya membagi Data Training dan data Valid. Sebelum membagi data menjadi data training dan data validasi, data diacak terlebih dahulu agar distribusi menjadi random.

 ![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/019.jpg?raw=true)
 
 Selanjutnya, kita bagi data train dan validasi dengan komposisi 80:20. Namun sebelumnya, kita perlu memetakan (mapping) data user dan film menjadi satu value terlebih dahulu. Lalu, buatlah rating dalam skala 0 sampai 1 agar mudah dalam melakukan proses training.
 
 ```python
 # buat variabel x untuk mencocokkan data user dan movie menjadi satu
x = df[['user', 'movie']].values

#buat variable y untuk membuat rating dari hasil
y = df['rating'].apply(lambda x: (x - min_rating) / (max_rating - min_rating)).values
 ```
 ```python
 #bagi data dengan ratio 80:20 (80% data uji dan 20% data valid)
dt_train = int(0.8*df.shape[0])
x_train, x_valid, y_train, y_valid = (
    x[:dt_train],
    x[dt_train:],
    y[:dt_train],
    y[dt_train:]
)

print(x,y)
 ```
 
  ![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/020.jpg?raw=true)
  
### Proses Training 

Pada tahap ini, model menghitung skor kecocokan antara pengguna dan film dengan teknik embedding. Pertama, kita melakukan proses embedding terhadap data user dan film. Selanjutnya, lakukan operasi perkalian dot product antara embedding user dan film. Selain itu, kita juga dapat menambahkan bias untuk setiap user dan film. Skor kecocokan ditetapkan dalam skala [0,1] dengan fungsi aktivasi sigmoid.

```python
class RecommenderNet(tf.keras.Model):

  # inisialisasi fungsi
  def __init__(self, num_user, num_movie, embedding_size, **kwargs):
    super(RecommenderNet, self).__init__(**kwargs)
    self.num_user = num_user
    self.num_movie = num_movie
    self.embedding_size = embedding_size
    self.user_embedding = layers.Embedding( # layer embedding user
        num_user,
        embedding_size,
        embeddings_initializer = 'he_normal',
        embeddings_regularizer = keras.regularizers.l2(1e-6)
    )
    self.user_bias = layers.Embedding(num_user, 1) # layer embedding user bias
    self.movie_embedding = layers.Embedding( # layer embedding movie
        num_movie,
        embedding_size,
        embeddings_initializer = 'he_normal',
        embeddings_regularizer = keras.regularizers.l2(1e-6)
    )
    self.movie_bias= layers.Embedding(num_movie, 1) # layer embedding movie bias

  def call(self, inputs):
    user_vector = self.user_embedding(inputs[:,0]) # memanggil layer embedding 1
    user_bias = self.user_bias(inputs[:,0]) # memanggil layer embedding 2
    movie_vector = self.movie_embedding(inputs[:, 1]) # memanggil layer embedding 3
    movie_bias = self.movie_bias(inputs[:, 1]) # memanggil layer embedding 4

    dot_user_movie = tf.tensordot(user_vector, movie_vector, 2)

    x = dot_user_movie + user_bias + movie_bias

    return tf.nn.sigmoid(x) # activation sigmoid
```

Selanjutnya, lakukan proses compile terhadap model.

```python
model = RecommenderNet(num_user, num_movie, 50) # inisialisasi model

model.compile(
    loss = tf.keras.losses.BinaryCrossentropy(),
    optimizer = keras.optimizers.Adam(learning_rate=0.001),
    metrics=[tf.keras.metrics.RootMeanSquaredError()]
)
```
Model ini menggunakan Binary Crossentropy untuk menghitung loss function, Adam (Adaptive Moment Estimation) sebagai optimizer, dan root mean squared error (RMSE) sebagai metrics evaluation.

Kemudian, lakukanlah proses training.

  ![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/021.jpg?raw=true)

## Evaluation
Metrik yang digunakan pada kasus ini, yaitu RMSE (Root Mean Squared Error) yaitu menghitung rata-rata kuadrat kesalahan antara label dan prediksi. Berikut hasil perbandingan rmse dan loss.

  ![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/022.jpg?raw=true)
  
## Pengujian - Mendapatkan Rekomendasi Film

Untuk mendapatkan rekomendasi film, ambil sampel user secara acak dan definisikan variabel **movie_not_watched** yang merupakan daftar film yang belum pernah ditonton oleh pengguna. 

Sebelumnya, pengguna telah memberi rating pada beberapa film yang telah mereka tonton. Kita menggunakan rating ini untuk membuat rekomendasi film yang mungkin cocok untuk pengguna. 

Variabel **movie_not_watched** diperoleh dengan menggunakan operator bitwise (~) pada variabel **movie_watched_by_user**.

```python
movie_df = movies_final
df = pd.read_csv('/content/ratings.csv')

#mengambil sample user
userId = df.userId.sample(1).iloc[0]
movie_watched_by_user = df[df.userId == userId]

movie_not_watched = movie_df[~movie_df['id'].isin(movie_watched_by_user.movieId.values)]['id'] 
movie_not_watched = list(
    set(movie_not_watched)
    .intersection(set(movieId_enc_final.keys()))
)
     
movie_not_watched = [[movieId_enc_final.get(x)] for x in movie_not_watched]
user_encoder = userId_enc_final.get(userId)
user_movie_array = np.hstack(
    ([[user_encoder]] * len(movie_not_watched), movie_not_watched)
)
```

Selanjutnya, untuk memperoleh rekomendasi film, gunakan fungsi model.predict() dari library Keras.

```python
ratings = model.predict(user_movie_array).flatten()
 
top_ratings_indices = ratings.argsort()[-10:][::-1]
recommended_movie_ids = [
    movieId_enc_to_movie.get(movie_not_watched[x][0]) for x in top_ratings_indices
]

print('Showing recommendations for users: {}'.format(userId))
print('   ' * 9)
print('   ' * 9)
print('Movie with high ratings from user')
print('----' * 8)

top_movie_user = (
    movie_watched_by_user.sort_values(
        by = 'rating',
        ascending=False
    )
    .head(5)
    .movieId.values
)

movie_df_rows = movie_df[movie_df['id'].isin(top_movie_user)]
for row in movie_df_rows.itertuples():
    print(row.title, ': Genre', row.genre)

print('   ' * 8)
print('   ' * 8)
print('Top 10 movie recommendation')
print('----' * 8)

recommended_movie = movie_df[movie_df['id'].isin(recommended_movie_ids)]
for row in recommended_movie.itertuples():
    print(row.title, ': Genre', row.genre)
```
  hasil : 
  
  ![hasil info](https://github.com/sintiasnn/movie-recomendation/blob/main/023.jpg?raw=true)
  
