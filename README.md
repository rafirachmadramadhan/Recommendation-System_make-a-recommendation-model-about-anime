# Laporan Proyek Machine Learning - Rafi Rachmad Ramadhan

## Project Overview

Semakin banyaknya anime yang dirilis dewasa ini menghadirkan tantangan bagi pengguna atau penonton maupun penyedia layanan _streaming_ anime media digital. Pengguna ataupun penonton sering mengalami kesulitan untuk menemukan aninime yang cocok untuk ditonton saking banyakya judul-judul anime. Penyedia layanan _streaming_ anime media digital pun juga mengalami kesulitan dengan banyaknya data pengguna, anime, dan genre untuk dipromosikan secara tepat kepada pengguna. Untuk itu perlu adanya sistem rekomendasi untuk menyelesaikan permasalahan tersebut. Sistem rekomendasi ini nantinya pada proyrk ini digunakan untuk menentukan top-N rekomendasi anime untuk setiap pengguna atau penonton. Hal ini penting agar pepenyedia layanan _streaming_ anime media digital dapat mempromosikan anime sesuai kebutuhan pengguna dan pengguna atau penonton bisa mendapatkan anime yang diinginkan.


## _Business Understanding_

Disebuah perusahaan penyedia layanan _streaming_ anime media digital setelah sekian lama beroprasi, perusahaan berhasil mengumpulkan berbagai informasi mengenai pengguna, penonton, anime, genrenya, rating, dan lain-lain. Seluruh informasi tersebut terkumpul dalam dua file csv, yaitu anime.csv dan rating.csv. Data tersebut akan dimanfaatkan untuk membuat pengguna lebih bisa memilih anime sesuai prefensi pengguna tersebut dengan tepat yang bisa membuat pengguna tersebut lebih banyak meluangkan waktu untuk menikmati layanan _streamingnya_.

### _Problem Statements_

Untuk itu akan mengembangkan sebuah sistem rekomendasi untuk menjawab permasalahan berikut.
- Berdasarkan data mengenai pengguna, bagaimana membuat sistem rekomendasi yang dipersonalisasi dengan teknik content-based filtering?
- Dengan data rating yang dimiliki, bagaimana perusahaan dapat merekomendasikan anime lain yang mungkin disukai dan belum pernah ditonton oleh pengguna? 

### _Goals_

Untuk  menjawab pertanyaan tersebut, dibuatlah sistem rekomendasi dengan tujuan atau goals sebagai berikut:
- Menghasilkan sejumlah rekomendasi anime yang dipersonalisasi untuk pengguna dengan teknik _content-based filtering_.
- Menghasilkan sejumlah rekomendasi anime yang sesuai dengan preferensi pengguna dan belum pernah ditonton sebelumnya dengan teknik _collaborative filtering_.

**_Solution statements_**
Untuk meraih _goal_ tersebut ada dua solusi yang akan diberikan untuk membuat sebuah sistem rekomendasi. Solusi yang pertama adalah dengan menggunakan _content-based filtering_ dengan memberikan rekomendasi anime berdsarkan genre yang mirip. Solusi yang kedua adalah dengan menggunakan _collaborative filtering_.

## _Data Understanding_
Data yang saya gunakan adalah [Anime Recommendations Database](https://www.kaggle.com/datasets/CooperUnion/anime-recommendations-database?select=anime.csv) yang diunduh dari kaggle. Kumpulan data ini berisi informasi tentang data preferensi pengguna dari 73.516 pengguna di 12.294 anime.

### Variabel-variabel pada Anime Recommendations Database adalah sebagai berikut:
**Anime.csv**
- _**anime_id**_     : ID unik myanimelist.net yang mengidentifikasi anime.     
- _**name**_         : nama lengkap anime.  
- _**genre**_        : daftar genre yang dipisahkan koma untuk anime ini.   
- _**type**_         : _movie_, TV, OVA, dan lain-lain.  
- _**episodes**_     : berapa episode dalam acara ini. (1 jika film).      
- _**rating**_       : rating rata-rata dari 10 untuk anime ini.    
- _**members**_      : jumlah anggota komunitas yang ada di "grup" anime ini.

**Rating.csv**
- _**user_id**_       : ID pengguna yang dibuat secara acak tidak dapat diidentifikasi.     
- _**anime_id**_      : anime yang telah dinilai oleh pengguna ini.  
- _**rating**_        : peringkat dari 10 yang telah ditetapkan pengguna ini (-1 jika pengguna menontonnya tetapi tidak memberikan peringkat).    

**Explaratory Data Analysis dan Visualisasi Data**:
- _**anime.csv data**_

|    |   anime_id | name                             | genre                                                        | type   |   episodes |   rating |   members |
|---:|-----------:|:---------------------------------|:-------------------------------------------------------------|:-------|-----------:|---------:|----------:|
|  0 |      32281 | Kimi no Na wa.                   | Drama, Romance, School, Supernatural                         | Movie  |          1 |     9.37 |    200630 |
|  1 |       5114 | Fullmetal Alchemist: Brotherhood | Action, Adventure, Drama, Fantasy, Magic, Military, Shounen  | TV     |         64 |     9.26 |    793665 |
|  2 |      28977 | Gintama°                         | Action, Comedy, Historical, Parody, Samurai, Sci-Fi, Shounen | TV     |         51 |     9.25 |    114262 |
|  3 |       9253 | Steins;Gate                      | Sci-Fi, Thriller                                             | TV     |         24 |     9.17 |    673572 |
|  4 |       9969 | Gintama&#039;                    | Action, Comedy, Historical, Parody, Samurai, Sci-Fi, Shounen | TV     |         51 |     9.16 |    151266 |

- _**anime.csv info**_
RangeIndex: 12294 entries, 0 to 12293
Data columns (total 7 columns):

|#   |Column    |Non-Null Count  |Dtype   | 
|--- | ------   | -------------- | -----  |  
| 0  | anime_id | 12294 non-null | int64  |  
| 1  | name     | 12294 non-null | object  |
| 2  | genre    | 12232 non-null | object  | 
| 3  | type     | 12269 non-null | object  | 
| 4  | episodes | 12294 non-null | object  | 
| 5  | rating   | 12064 non-null | float64  |6
| 6  | members  | 12294 non-null | int64  |
dtypes: float64(1), int64(2), object(4)
memory usage: 672.5+ KB

![animeInf](https://user-images.githubusercontent.com/111117217/192739482-cdd728b3-0a2c-46e5-aa6c-7ba77e214646.png)
- _**banyak anime_id tanpa nilai yang sama**_
![dataUniqAnime](https://user-images.githubusercontent.com/111117217/192739537-38bbc35c-7a82-40ac-a85c-7d67c3612529.png)
- _**rating.csv data**_

|    |   user_id |   anime_id |   rating |
|---:|----------:|-----------:|---------:|
|  0 |         1 |         20 |       -1 |
|  1 |         1 |         24 |       -1 |
|  2 |         1 |         79 |       -1 |
|  3 |         1 |        226 |       -1 |
|  4 |         1 |        241 |       -1 |

- _**rating.csv info**_
RangeIndex: 7813737 entries, 0 to 7813736
Data columns (total 3 columns):

|#   |Column    | Dtype |
|--- | ------   |  -----|
| 0  | user_id  |  int64|
| 1  | anime_id |  int64|
| 2  | rating   |  int64|
dtypes: int64(3)
memory usage: 178.8 MB

- _**banyak user_id tanpa nilai yang sama**_
![dataUniqUser](https://user-images.githubusercontent.com/111117217/192739552-9e20b817-a943-4cc5-bafb-592501c3d1ed.png)

## _Data Preparation_
**_Data Preparation General_**
Pada bagian ini diterapkan satu cara untuk mempersiapkan data yaitu mengatasi _missing value_. Penanganan ini dilakukan supaya kinerja model nantinya bisa lebih baik. Pada proses ini data akan dicek apakah ada _missing value_ atau tidak. Setelah dicek tenyata terdapat beberapa _missing value_.
![missing](https://user-images.githubusercontent.com/111117217/192739570-a366e4e1-56cd-47dc-8beb-8ded05ab817c.png)
Terdapat _missing value_ pada 10 _name_, 120 _genre_, 14 _type_, 10 _episodes_, 16 *rating_y*, dan 10 _member_. Jumlah tersebut terhitung kecil dibandigkan data yang memiliki sebesar 7,8 juta. Oleh karena itu, data yang terdapat _missing value_ akan _didrop_ pada proses kali ini.  

**_Data Preparation: Content Based Filtering_**
Pada bagian ini diterapkan satu cara untuk mempersiapkan data yaitu mengilangkan variabel yang tidak diperlukan. Hal ini dilakukan karena pada _content based filtering_ hanya diperlukan beberapa variabel saja. Pada proses ini beberapa variabel akan _didrop_ dan akan menyisahkan variabel *anime_id*, _name_, dam *list*.
|    |   user_id |   anime_id |   rating_x | name                      | genre                                                                | type   |   episodes |   rating_y |   members |
|---:|----------:|-----------:|-----------:|:--------------------------|:---------------------------------------------------------------------|:-------|-----------:|-----------:|----------:|
|  0 |         1 |         20 |         -1 | Naruto                    | Action, Comedy, Martial Arts, Shounen, Super Power                   | TV     |        220 |       7.81 |    683297 |
|  1 |         1 |         24 |         -1 | School Rumble             | Comedy, Romance, School, Shounen                                     | TV     |         26 |       8.06 |    178553 |
|  2 |         1 |         79 |         -1 | Shuffle!                  | Comedy, Drama, Ecchi, Fantasy, Harem, Magic, Romance, School, Seinen | TV     |         24 |       7.31 |    158772 |
|  3 |         1 |        226 |         -1 | Elfen Lied                | Action, Drama, Horror, Psychological, Romance, Seinen, Supernatural  | TV     |         13 |       7.85 |    623511 |
|  4 |         1 |        241 |         -1 | Girls Bravo: First Season | Comedy, Ecchi, Fantasy, Harem, Romance, School                       | TV     |         11 |       6.69 |     84395 |

**_Data Preparation: Collaborative Filtering_**
Pada bagian ini diterapkan dua cara untuk mempersiapkan data yaitu menyadikan (_encode_) fitur "*user_id*" dan "*anime_id*" dan membagi data utuk training dan validasi.
- _**Menyadikan (_encode_) fitur "*user_id*" dan "*anime_id*"**_ Tahap persiapan ini penting dilakukan agar data siap digunakan untuk pemodelan. Pada proses ini dilakukan dengan mengubah "*user_id*" dan "*anime_id*" menjadi list tanpa nilai yang sama, selanjutya melakukan _encoding_ "*user_id*" dan "*anime_id*", selanjutnya melakukan _encoding_ angka ke "*user_id*" dan "*anime_id*", dan yang terakhir memetakan "*user_id*" dan "*anime_id*" ke _dataframe_ yang berkaitan.

- _**Membagi data utuk training dan validasi**_ sebelum melakukan pembagian data _train_ dan _validation_, diacak dataya agar distribusinya menjadi _random_.

|         |   user_id |   anime_id |   rating |   user |   anime |
|--------:|----------:|-----------:|---------:|-------:|--------:|
| 7806172 |     73424 |       7785 |       10 |  73422 |    1067 |
| 6470262 |     59789 |      19769 |        6 |  59787 |     256 |
| 5975474 |     55960 |         59 |       10 |  55958 |     603 |
| 7617688 |     71461 |       3342 |       10 |  71459 |    1361 |
| 5932334 |     55390 |        986 |        8 |  55388 |    1408 |

Pada proses ini dilakukan dengan membagi data menjadi data _train_ dan _validation_ dengan komposisi 80:20. Sebelumnya, perlu memetakan (_mapping_) data _user_ dan _anime_ menjadi satu _value_ terlebih dahulu. Lalu, buatlah rating dalam skala 0 sampai 1 agar mudah dalam melakukan proses _training_. 
![TandV](https://user-images.githubusercontent.com/111117217/192739766-842d5ca7-236e-4fef-a38b-59a4009b071f.png)


## Modeling
Pada tahapan ini akan digunakan dua pendekatan untuk menyelesaikan sebuah masalah sistem rekomendasi ini. diantaranya.

**_Content Based Filtering_**
_Content-based filtering_ ini mempelajari profil minat pengguna baru berdasarkan data dari objek yang telah dinilai pengguna. Kelebihan _content-based filtering_ ini memiliki kemampuan untuk merekomendasikan tanpa harus menunggu pengguna lain merating item tersebut. Namun, _content-based filtering_ ini memiliki kelemahan dimana penyaringan berbasis konten sulit untuk menghasilkan rekomendasi yang tidak terduga karena semua informasi dipilih dan direkomendasikan berdsarkan konten. 

Pada tahap ini, akan dibangun sistem rekomendasi sederhana berdasarkan genre dari anime. Untuk menemukan representasi fitur penting dari setiap genre akan digunakan TF-IDF vectorizer.

Setelah selesai akan dilihatkan matriks tf-idf untuk beberapa anime dan genre.

|   AnimeID |   arts |     life |   hentai |   psychological |   super |   shoujo |   kids |   samurai |   martial |   josei |   parody |   shounen |   adventure |   action |   sports |       of |   demons |   mecha |   historical |   ai |   yuri |   game |   ecchi |       fi |   thriller |   music |   police |   vampire |   military |    drama |   harem |   seinen |   power |   mystery |    slice |   fantasy |      sci |   yaoi |   cars |   horror |   supernatural |   dementia |   comedy |    magic |   space |   school |   romance |
|----------:|-------:|---------:|---------:|----------------:|--------:|---------:|-------:|----------:|----------:|--------:|---------:|----------:|------------:|---------:|---------:|---------:|---------:|--------:|-------------:|-----:|-------:|-------:|--------:|---------:|-----------:|--------:|---------:|----------:|-----------:|---------:|--------:|---------:|--------:|----------:|---------:|----------:|---------:|-------:|-------:|---------:|---------------:|-----------:|---------:|---------:|--------:|---------:|----------:|
|      2180 |      0 | 0.385399 | 0        |               0 |       0 |        0 |      0 |         0 |         0 |       0 |        0 |         0 |           0 |        0 | 0.477005 | 0.385399 |        0 |       0 |            0 |    0 |      0 |      0 |       0 | 0        |          0 |       0 |        0 |         0 |          0 | 0        |       0 |        0 |       0 |         0 | 0.385399 |         0 | 0        |      0 |      0 |        0 |       0        |          0 | 0.228767 | 0        |       0 | 0.383326 |  0.357206 |
|      5603 |      0 | 0        | 0.599196 |               0 |       0 |        0 |      0 |         0 |         0 |       0 |        0 |         0 |           0 |        0 | 0        | 0        |        0 |       0 |            0 |    0 |      0 |      0 |       0 | 0        |          0 |       0 |        0 |         0 |          0 | 0.503862 |       0 |        0 |       0 |         0 | 0        |         0 | 0        |      0 |      0 |        0 |       0.622164 |          0 | 0        | 0        |       0 | 0        |  0        |
|     31559 |      0 | 0        | 0        |               0 |       0 |        0 |      0 |         0 |         0 |       0 |        0 |         0 |           0 |        0 | 1        | 0        |        0 |       0 |            0 |    0 |      0 |      0 |       0 | 0        |          0 |       0 |        0 |         0 |          0 | 0        |       0 |        0 |       0 |         0 | 0        |         0 | 0        |      0 |      0 |        0 |       0        |          0 | 0        | 0        |       0 | 0        |  0        |
|      5190 |      0 | 0        | 1        |               0 |       0 |        0 |      0 |         0 |         0 |       0 |        0 |         0 |           0 |        0 | 0        | 0        |        0 |       0 |            0 |    0 |      0 |      0 |       0 | 0        |          0 |       0 |        0 |         0 |          0 | 0        |       0 |        0 |       0 |         0 | 0        |         0 | 0        |      0 |      0 |        0 |       0        |          0 | 0        | 0        |       0 | 0        |  0        |
|      3747 |      0 | 0        | 0.571974 |               0 |       0 |        0 |      0 |         0 |         0 |       0 |        0 |         0 |           0 |        0 | 0        | 0        |        0 |       0 |            0 |    0 |      0 |      0 |       0 | 0        |          0 |       0 |        0 |         0 |          0 | 0        |       0 |        0 |       0 |         0 | 0        |         0 | 0        |      0 |      0 |        0 |       0.593898 |          0 | 0        | 0        |       0 | 0.5658   |  0        |
|     14563 |      0 | 0        | 0        |               0 |       0 |        0 |      0 |         0 |         0 |       0 |        0 |         0 |           0 |        0 | 0        | 0        |        0 |       0 |            0 |    0 |      0 |      0 |       0 | 0        |          0 |       0 |        0 |         0 |          0 | 0        |       0 |        0 |       0 |         0 | 0        |         0 | 0        |      0 |      0 |        0 |       0        |          0 | 0.463224 | 0.886241 |       0 | 0        |  0        |
|      9322 |      0 | 0        | 1        |               0 |       0 |        0 |      0 |         0 |         0 |       0 |        0 |         0 |           0 |        0 | 0        | 0        |        0 |       0 |            0 |    0 |      0 |      0 |       0 | 0        |          0 |       0 |        0 |         0 |          0 | 0        |       0 |        0 |       0 |         0 | 0        |         0 | 0        |      0 |      0 |        0 |       0        |          0 | 0        | 0        |       0 | 0        |  0        |
|       630 |      0 | 0        | 0        |               0 |       0 |        0 |      0 |         0 |         0 |       0 |        0 |         0 |           0 |        0 | 0        | 0        |        0 |       0 |            0 |    0 |      0 |      0 |       0 | 0.42015  |          0 |       0 |        0 |         0 |          0 | 0        |       0 |        0 |       0 |         0 | 0        |         0 | 0.42015  |      0 |      0 |        0 |       0        |          0 | 0.301893 | 0.577582 |       0 | 0        |  0.471388 |
|     11681 |      0 | 0        | 0        |               0 |       0 |        0 |      0 |         0 |         0 |       0 |        0 |         0 |           0 |        0 | 0        | 0        |        0 |       0 |            0 |    0 |      0 |      0 |       0 | 0        |          0 |       0 |        0 |         0 |          0 | 0        |       0 |        0 |       0 |         0 | 0        |         1 | 0        |      0 |      0 |        0 |       0        |          0 | 0        | 0        |       0 | 0        |  0        |
|      4640 |      0 | 0        | 0        |               0 |       0 |        0 |      0 |         0 |         0 |       0 |        0 |         0 |           0 |        0 | 0        | 0        |        0 |       0 |            0 |    0 |      0 |      0 |       0 | 0.630405 |          0 |       0 |        0 |         0 |          0 | 0        |       0 |        0 |       0 |         0 | 0        |         0 | 0.630405 |      0 |      0 |        0 |       0        |          0 | 0.452968 | 0        |       0 | 0        |  0        |
Output matriks tf-idf di atas menunjukkan salah satu anime dengan id 2180 bergenre salah satunya _slice of life_, _sport_, _school_, _romance_, dan _comedy_.

Selanjutnya, akan dihitung derajat kesamaan (_similarity degree_) antar anime dengan teknik _cosine similarity_. _Output_ sebagai berikut.
| AnimeTitle                                                      |   Monkey Magic |   Tonari no Kaibutsu-kun: Tonari no Gokudou-kun |   Maoyuu Maou Yuusha |   Attack No.1: Namida no Kaiten Receive |   Cyborg 009 |
|:----------------------------------------------------------------|---------------:|------------------------------------------------:|---------------------:|----------------------------------------:|-------------:|
| Tamala 2010: A Punk Cat in Space                                |       0        |                                       0.0716333 |             0        |                                0        |     0.27274  |
| Haru no Shikumi                                                 |       0        |                                       0         |             0        |                                0        |     0        |
| Saiyuuki                                                        |       0.537033 |                                       0         |             0.340817 |                                0        |     0.396774 |
| Prince of Tennis: Another Story - Messages From Past and Future |       0        |                                       0.0938553 |             0        |                                0.421748 |     0.199585 |
| Eikoku Koi Monogatari Emma: Molders-hen                         |       0        |                                       0.400451  |             0.338085 |                                0.138129 |     0.131356 |
| Dororo                                                          |       0        |                                       0         |             0        |                                0        |     0.366412 |
| Fushigi no Kuni no Alice                                        |       1        |                                       0         |             0.371936 |                                0        |     0.213081 |
| Takamiya Nasuno Desu!: Teekyuu Spin-off                         |       0        |                                       0.169507  |             0        |                                0        |     0.360461 |
| Sword Gai                                                       |       0        |                                       0         |             0        |                                0        |     0.151449 |
| Cream Lemon                                                     |       0.134768 |                                       0.188231  |             0.198222 |                                0.114807 |     0.317629 |
Perhatikanlah pada output matriks di atas. Angka diantara 0 sampai 1  mengindikasikan bahwa anime pada kolom X (horizontal) memiliki kesamaan dengan anime pada baris Y (vertikal). Sebagai contoh, anime _Saiyuuki_ teridentifikasi hampir sama (similar) dengan _Monkey Magic_.

Selanjutnya saatnya menghasilkan sejumlah anime yang akan direkomendasikan kepada pengguna. Di sini, kita membuat fungsi anime_recommendations. Selanjutnya, menemukan rekomendasi anime yang mirip dengan Naruto.

|    |   AnimeID | AnimeTitle   | AnimeGenre                                         |
|---:|----------:|:-------------|:---------------------------------------------------|
| 10 |        20 | Naruto       | Action, Comedy, Martial Arts, Shounen, Super Power |
Naruto masuk dalam anime bergenre action, comedy, shounen, dan lain-lain. Setelah fungsi dijalankan untuk melihat rekomendasi anime yang mirip naruto, hasilnya sebagai berikut.

|    | AnimeTitle                                                                | AnimeGenre                                         |
|---:|:--------------------------------------------------------------------------|:---------------------------------------------------|
|  0 | Naruto: Shippuuden Movie 4 - The Lost Tower                               | Action, Comedy, Martial Arts, Shounen, Super Power |
|  1 | Naruto: Shippuuden Movie 3 - Hi no Ishi wo Tsugu Mono                     | Action, Comedy, Martial Arts, Shounen, Super Power |
|  2 | Boruto: Naruto the Movie - Naruto ga Hokage ni Natta Hi                   | Action, Comedy, Martial Arts, Shounen, Super Power |
|  3 | Naruto Soyokazeden Movie: Naruto to Mashin to Mitsu no Onegai Dattebayo!! | Action, Comedy, Martial Arts, Shounen, Super Power |
|  4 | Naruto Shippuuden: Sunny Side Battle                                      | Action, Comedy, Martial Arts, Shounen, Super Power |

**_Collaborative Filtering_**
_Collaborative Filtering_ ini bergantung pada pendapat komunitas pengguna. Ia tidak memerlukan atribut untuk setiap itemnya seperti pada sistem berbasis konten. Namun, ia ketika mendapat content baru tidak dapat langsung memberi rekomendasi karena ia bergantung pada pendapat komunitas. Pada tahap kali ini, akan dibuat class RecommenderNet dengan keras Model class. Kode class RecommenderNet ini terinspirasi dari tutorial dalam situs Keras dengan beberapa adaptasi sesuai kasus yang sedang kita selesaikan. hasil output dari hasil trainingnya adalah sebagai berikut.
![training](https://user-images.githubusercontent.com/111117217/192739783-3f8c4b5d-64c0-4afc-9b84-a808e2fd9813.png)
Untuk mendapatkan rekomendasi anime, pertama kita ambil sampel user secara acak. Definisikan variabel _animenowatch_ yang merupakan daftar anime yang belum pernah dikunjungi oleh pengguna. kita perlu membuat variabel _animenowatch_ sebagai daftar restoran untuk direkomendasikan pada pengguna. Selanjutnya, untuk memperoleh rekomendasi restoran, digunakan fungsi model.predict() dari library Keras. hasilnya sebagai berikut.
![recob](https://user-images.githubusercontent.com/111117217/192739735-7b3a9f11-93ed-4b7f-b8d5-9ff3d2718310.png)

## Evaluation
**_Content Based Filtering_**
Pada tahap dengan _Content Based Filtering_  dievaluasi degan metrik pecision. precision adalah jumlah item rekomendasi yang relevan. Metrik precision ini ditulis sebagai berikut. 

$Pecision = \frac{of-recommendation-that-are-relevant}{of-item-we-recommend}$

Kali ini dievaluasi menggunakan sample anime Naruto dimana Naruto adalah anime bergenre action, comedy, shounen, dan lain-lain. Tentu rekomendasi yang diberikan haruslah anime dengan genre yang mirip dan benar saja sistem rekomendasi dengan cara _content Based Filtering_ ini menghasilkan rekomendasi yang serupa genrenya dengan Naruto. hasil rekomendasinya sebagai berikut.
|    | AnimeTitle                                                                | AnimeGenre                                         |
|---:|:--------------------------------------------------------------------------|:---------------------------------------------------|
|  0 | Naruto: Shippuuden Movie 4 - The Lost Tower                               | Action, Comedy, Martial Arts, Shounen, Super Power |
|  1 | Naruto: Shippuuden Movie 3 - Hi no Ishi wo Tsugu Mono                     | Action, Comedy, Martial Arts, Shounen, Super Power |
|  2 | Boruto: Naruto the Movie - Naruto ga Hokage ni Natta Hi                   | Action, Comedy, Martial Arts, Shounen, Super Power |
|  3 | Naruto Soyokazeden Movie: Naruto to Mashin to Mitsu no Onegai Dattebayo!! | Action, Comedy, Martial Arts, Shounen, Super Power |
|  4 | Naruto Shippuuden: Sunny Side Battle                                      | Action, Comedy, Martial Arts, Shounen, Super Power |

Menggunakan metrik precision diperoleh hasil yang maksimal dimana hasil dari kelima rekomendasi menunjukan hasil yang relevan, jadi presisinya 100%. Hasil ini menunjukkan hasil presisi yang sempurna.

**_Collaborative Filtering_**
Pada tahap dengan _Collaborative Filtering_ kita mengevaluasinya menggunkan metrik Root Mean Square Error (RMSE). Root Mean Square Error adalah metode pengukuran dengan mengukur perbedaan nilai dari prediksi sebuah model sebagai estimasi atas nilai yang diobservasi. Root Mean Square Error adalah hasil dari akar kuadrat Mean Square Error. Cara Menghitung Root Mean Square Error (RMSE) adalah dengan mengurangi nilai aktual dengan nilai peramalan kemudian dikuadratkan dan dijumlahkan keseluruhan hasilnya kemudian dibagi dengan banyaknya data. Hasil perhitungan tersebut selanjutnya dihitung kembali untuk mencari nilai dari akar kuadrat.

$RMSE = \sqrt{\frac{1}{n}\Sigma_{i=1}^{n}{\Big(\frac{d_i -f_i}{\sigma_i}\Big)^2}}$

Untuk melihat visualisasi proses training, mari kita plot metrik evaluasi dengan matplotlib.
![plot](https://user-images.githubusercontent.com/111117217/192739620-adbcf0a9-3e0a-42c5-a098-5e8f9695ff73.png)
diperoleh nilai error akhir sebesar sekitar 0.3923 dan error pada data validasi sebesar 0.3666. Nilai tersebut cukup bagus untuk sistem rekomendasi.

## Kesimpulan
Dari proyek sistem rekomendasi dengan dataset _anime recommendations database_ ini dapat disimpulkan bahwasannya dari rekomendasi dengan cara _Content Based Filtering_ menghasilkan rekomendasi yang sempurna dengan presisinya yang 100%, begitu juga pada rekomendasi dengan cara _Collaborative Filtering_ menghasilkan error yang cukup kecil yang mana error akhir sebesar sekitar 0.3923 dan error pada data validasi sebesar 0.3666. Hal ini menunjukan bahwasannya hasil proyek ini sudah memnuhi _goal_.

## Referensi
- Anthea Adellya Pradnya Devi, David Boy Tonara, “[Rancang Bangun Recommender System dengan Menggunakan Metode Collaborative Filtering untuk Studi Kasus Tempat Kuliner di Surabaya](https://dspace.uc.ac.id/bitstream/handle/123456789/1104/RS1512093.pdf?sequence=1&isAllowed=y#:~:text=Kelebihan%20recommender%20system%20dengan%20pendekatan,pernah%20diberi%20nilai%20rating%20tinggi),” Jurnal Informatika dan Sistem Informasi, vol. 01, no. 02, pp. 102–112, August. 2015. 
- khoiri.com. (2020, 23 December). Pengertian dan Cara Menghitung Root Mean Square Error (RMSE). Diakses pada 28 September 2022, dari https://www.khoiri.com/2020/12/cara-menghitung-root-mean-square-error-rmse.html