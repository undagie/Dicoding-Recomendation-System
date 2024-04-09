# Laporan Proyek Machine Learning - Muhammad Edya Rosadi
## Project Overview

Dalam era digital saat ini, sistem rekomendasi telah menjadi komponen esensial untuk meningkatkan keterlibatan pengguna dan personalisasi layanan di berbagai platform online. Kemampuan untuk menyediakan rekomendasi yang relevan dan personal sangat bergantung pada efektivitas algoritma yang digunakan[[1]]. Dua pendekatan utama yang sering dibandingkan dalam literatur adalah content-based filtering dan collaborative filtering. Content-based filtering berfokus pada karakteristik item, merekomendasikan item serupa dengan yang telah dinikmati pengguna sebelumnya[[2]]. Sementara itu, collaborative filtering memanfaatkan preferensi pengguna lain untuk merekomendasikan item, berdasarkan gagasan bahwa orang dengan preferensi serupa cenderung menyukai item yang sama[[3]].

Eksperimen ini memanfaatkan dataset [MovieLens], yang merupakan kumpulan besar rating dan tag film, sebagai kasus studi untuk mengevaluasi dan membandingkan efektivitas kedua metode tersebut. Penggunaan dataset ini tidak hanya memungkinkan penilaian terhadap akurasi rekomendasi tetapi juga terhadap kemampuan sistem dalam menangani isu-isu seperti cold start dan sparsity. Pentingnya penelitian ini terletak pada potensinya untuk memperbaiki pengalaman pengguna di berbagai platform, memastikan bahwa pengguna menemukan konten yang menarik dan relevan dengan preferensi mereka.

Literatur terkait mengungkapkan berbagai teknik dan metodologi yang telah dikembangkan untuk mengoptimalkan sistem rekomendasi[[4]][[5]], namun masih terdapat ruang untuk peningkatan. Terutama, tantangan seperti personalisasi yang efektif, penanganan data yang tidak lengkap, dan scalabilitas memerlukan solusi inovatif[[6]].

Proyek ini bertujuan untuk menyelidiki secara mendalam efektivitas content-based filtering dibandingkan dengan collaborative filtering, menggunakan dataset MovieLens sebagai kasus studi. Dengan membandingkan kedua metode ini dalam konteks nyata, diharapkan dapat memberikan kontribusi penting dalam merancang sistem rekomendasi yang lebih akurat dan personal, serta mengatasi beberapa tantangan kunci yang saat ini dihadapi dalam bidang ini.

## Business Understanding

**Problem Statements**
Dalam konteks proyek ini, masalah yang ingin diatasi dapat dirumuskan dalam problem statements sebagai berikut:
1. Apa saja faktor-faktor yang mempengaruhi efektivitas model rekomendasi pada dataset MovieLens?
2. Bagaimana membuat model content-based filtering dan collaborative filtering dalam merekomendasikan film yang relevan kepada pengguna?
3. Bagaimana meningkatkan kualitas rekomendasi yang dihasilkan melalui kedua pendekatan tersebut?

**Goals**
Tujuan dari proyek ini dirancang untuk menjawab problem statements di atas dengan cara sebagai berikut:
1. Mengidentifikasi dan menganalisis faktor-faktor yang mempengaruhi efektivitas kedua metode rekomendasi pada dataset MovieLens.
2. Membuat model content-based filtering dan collaborative filtering dalam merekomendasikan film yang relevan kepada pengguna
3. Meningkatkan kualitas rekomendasi yang dihasilkan dengan membandingkan dan mengoptimalkan kedua metode tersebut.

**Solution Approach**
Untuk mencapai tujuan tersebut, beberapa solusi yang diusulkan meliputi:
1. Melakukan analisis eksploratif data untuk memahami distribusi rating dan karakteristik film yang ada dalam dataset MovieLens.
2. Menerapkan content-based filtering dengan memanfaatkan fitur konten film, seperti genre dan tag.
3. Menerapkan collaborative filtering dengan memanfaatkan informasi rating dari pengguna.

## Data Understanding
Dataset yang digunakan dalam proyek ini adalah dataset MovieLens yang berisi 100.836 rating dan 3.683 tag di 9.742 film oleh 610 pengguna dari periode 29 Maret 1996 hingga 24 September 2018. Dataset ini tersedia untuk diunduh di https://grouplens.org/datasets/movielens/.

Fitur pada dataset ini meliputi:
- ratings.csv: userId, movieId, rating, timestamp
- tags.csv: userId, movieId, tag, timestamp
- movies.csv: movieId, title, genres
- links.csv: movieId, imdbId, tmdbId

**Analisis Univariate dan Multivariate**
***Statistik rating***
Tabel 1. Tabel statistik rating
| Jenis | Nilai |
| ------ | ------ |
|count|100836.000000|
|mean|3.501557|
|std|1.042529|
|min|0.500000|
|25%| 3.000000|
|50%|3.500000|
|75%|4.000000|
|max|5.000000|
|modus|4.000000|
|Name: rating|dtype: float64|
Berikut adalah statistik deskriptif dasar untuk rating seperti tabel 1:
- Jumlah Rating: 100836
- Rata-Rata (Mean): 3.5016
- Standar Deviasi: 1.0425
- Nilai Minimum: 0.5
- Kuartil Pertama (25%): 3.0
- Median (50%): 3.5
- Kuartil Ketiga (75%): 4.0
- Nilai Maksimum: 5.0
- Modus: 4.0

Dari data ini, kita bisa melihat bahwa rating rata-rata berada di sekitar 3.5, dengan modus rating adalah 4.0, yang menunjukkan bahwa sebagian besar pengguna cenderung memberikan rating tinggi. Distribusi rating menunjukkan bahwa pengguna MovieLens umumnya memberikan rating positif, dengan skewness ringan ke arah rating yang lebih tinggi.

***Distribusi Rating***

![Gambar distribusi rating pada dataset MovieLens](https://github.com/undagie/Dicoding-Recomendation-System/blob/main/img/distribusi_rating.png?raw=true)

Gambar 1. Distribusi rating
Visualisasi gambar 1 menunjukkan distribusi rating pada dataset MovieLens. Dari grafik, kita bisa melihat rating dengan nilai 4.0 adalah yang paling sering diberikan, sesuai dengan nilai modus yang kita hitung sebelumnya. Rating dengan nilai 3.0 dan 4.0 bersama-sama mendominasi distribusi, menunjukkan bahwa kebanyakan film menerima penilaian positif dari pengguna. Distribusi rating menunjukkan kecenderungan pengguna untuk memberikan rating yang lebih tinggi, dengan jumlah rating yang lebih rendah (seperti 0.5 dan 1.0) jauh lebih sedikit dibandingkan dengan rating tinggi.

***Frekuensi Genre Film***

![Gambar frekuensi genre film](https://github.com/undagie/Dicoding-Recomendation-System/blob/main/img/frekuensi_genre.png?raw=true)

Gambar 2. Frekuensi genre film
Visualisasi gambar 2 data frekuensi genre film dalam dataset MovieLens menunjukkan:
- Drama adalah genre paling populer, diikuti oleh Comedy dan Thriller. Ini menunjukkan preferensi pengguna terhadap cerita yang mendalam atau menghibur.
- Genre seperti Action, Romance, dan Adventure juga cukup populer, menandakan kecenderungan umum pengguna terhadap film dengan narasi yang kuat atau elemen petualangan.
- Di sisi lain, genre seperti Film-Noir dan IMAX memiliki frekuensi yang lebih rendah, menunjukkan bahwa genre ini kurang umum atau spesifik niche dibandingkan dengan genre utama lainnya.
- Genre (no genres listed) menunjukkan bahwa ada beberapa film di dataset yang tidak memiliki genre yang terdaftar.

***Korelasi Rating dan Genre Film***

![Gambar korelasi rating dan genre film](https://github.com/undagie/Dicoding-Recomendation-System/blob/main/img/korelasi_rating_genre.png?raw=true)

Gambar 3. Korelasi rating dan genre film
Visualisasi dan analisis rating rata-rata per genre film seperti gambar 3 menunjukkan:
- Film-Noir, War, dan Documentary adalah genre dengan rating rata-rata tertinggi, menunjukkan bahwa film-film dalam genre ini cenderung sangat dihargai oleh pengguna.
- Di sisi lain, Comedy dan Horror memiliki rating rata-rata yang lebih rendah dibandingkan dengan genre lain, meskipun perbedaan rating antar genre tidak terlalu signifikan.
- Drama dan Crime, yang merupakan dua genre paling populer berdasarkan frekuensi, juga memiliki rating rata-rata yang cukup tinggi, menegaskan popularitas dan apresiasi mereka di kalangan pengguna.
- Genre (no genres listed), meskipun memiliki jumlah film yang relatif rendah, menunjukkan rating rata-rata yang sebanding dengan genre populer lainnya.

***Tag Paling Populer***

![Gambar tag paling populer](https://github.com/undagie/Dicoding-Recomendation-System/blob/main/img/tag.png?raw=true)

Gambar 4. Tag paling populer
Visualisasi dan daftar 20 tag paling populer seperti gambar 4 menunjukkan:
- In Netflix queue adalah tag paling populer, dengan jauh melebihi frekuensi tag lainnya. Ini menunjukkan ketertarikan pengguna untuk menandai film yang mereka ingin tonton di Netflix.
- Tag seperti atmospheric, thought-provoking, dan superhero juga populer, mencerminkan apresiasi pengguna terhadap suasana film, konten yang memicu pemikiran, dan genre superhero.
Disney, surreal, dan funny termasuk dalam tag populer lainnya, menunjukkan ketertarikan pengguna pada film-film dengan elemen fantasi, humor, atau yang diproduksi oleh Disney.
- Tag seperti religion, sci-fi, crime, dan politics menunjukkan ketertarikan pengguna pada tema-tema spesifik dalam film.

***Film Paling Populer***

Tabel 2. Film Paling Populer
| Judul Film | Jumlah Rating | Rata-rata Rating |
| ------ | ------ | ------ |
| Forrest Gump (1994) | 329 | 4.16 |
| Shawshank Redemption, The (1994) | 317 | 4.43 |
| Pulp Fiction (1994) | 307 | 4.20 |
| Silence of the Lambs, The (1991)| 279 | 4.16 |
| Matrix, The (1999)| 278 | 4.19 |
| Star Wars: Episode IV - A New Hope (1977) | 251 | 4.23 |
| Jurassic Park (1993) | 238 | 3.75 |
| Braveheart (1995) | 237 | 4.03 |
| Terminator 2: Judgment Day (1991) | 224 | 3.97 |
| Schindler's List (1993) | 220 | 4.23 |

## Data Preparation
Tahapan data preparation dalam eksperimen sistem rekomendasi yang menggunakan dataset MovieLens pada proyek ini meliputi langkah-langkah berikut, dirancang untuk mempersiapkan dan memproses data agar sesuai dengan kebutuhan analisis dan pemodelan:
1. Menggabungkan Data Movies dengan Tags:
Data dari tabel movies dan tags digabungkan berdasarkan movieId. Untuk film yang tidak memiliki tag, nilai NaN pada kolom tag digantikan dengan string kosong. Tujuan menggabungkan ini memperkaya informasi setiap film dengan detail tambahan dari tag, memungkinkan sistem rekomendasi berbasis konten untuk lebih akurat dalam menilai kesamaan antar film berdasarkan konten dan preferensi pengguna.
2. Membuat Kolom Combined Features:
Kolom combined_features dibuat dengan menggabungkan informasi dari kolom genres dan tag, menggunakan | sebagai pemisah. Ini bertujuan untuk memfasilitasi pembuatan vektor fitur dari teks untuk analisis kesamaan, yang krusial dalam sistem rekomendasi berbasis konten. Ini membantu model dalam memahami dan membandingkan kompleksitas konten film secara lebih efektif.
3. Encoding untuk Collaborative Filtering:
userId dan movieId diubah menjadi urutan angka berurutan dengan menggunakan .astype('category').cat.codes. Proses ini juga melibatkan pembuatan pemetaan ulang antara kode kategori dengan ID asli. Encoding ini menyederhanakan data dan mengurangi kompleksitas komputasional, memungkinkan algoritma collaborative filtering untuk lebih efisien dalam memproses data. Ini penting untuk teknik embedding yang digunakan dalam model collaborative filtering, yang membutuhkan input numerik.
4. Pembagian Data untuk Training dan Validasi:
Data dibagi menjadi set pelatihan dan validasi menggunakan train_test_split, dengan proporsi tertentu dari data dialokasikan untuk validasi. Pembagian ini memastikan bahwa model dapat diuji pada data yang belum pernah dilihat sebelumnya, mengukur kemampuan model dalam memprediksi rating dengan akurat pada situasi nyata. Hal ini membantu dalam mengidentifikasi dan mengatasi overfitting.

Setiap tahapan data preparation ini untuk memastikan bahwa data yang digunakan dalam pemodelan sistem rekomendasi bersih, konsisten, dan siap untuk analisis lebih lanjut. Langkah-langkah ini dirancang untuk memaksimalkan efektivitas model rekomendasi baik dalam pendekatan berbasis konten maupun collaborative filtering.

## Modeling and Result
**Model**
Dalam eksperimen ini, dua model sistem rekomendasi dibuat untuk menangani permasalahan rekomendasi film kepada pengguna: Content-Based Filtering dan Collaborative Filtering.

1. Content-Based Filtering
    - Pembuatan Model: Menggunakan TF-IDF Vectorizer untuk mengonversi combined_features menjadi matriks fitur dan menghitung cosine similarity antara film-film berdasarkan matriks tersebut. Sistem kemudian merekomendasikan film yang memiliki kesamaan konten tertinggi dengan film yang disukai pengguna.
    - Output: Top-N rekomendasi disajikan berdasarkan skor kesamaan tertinggi.
    - Kelebihan:
        - Rekomendasi sangat personal dan relevan dengan preferensi spesifik pengguna berdasarkan konten.
        - Sistem tetap efektif bahkan dengan jumlah pengguna yang terbatas.
    - Kekurangan:
        - Terbatas pada konten yang mirip dengan apa yang sudah diketahui; dapat menghasilkan rekomendasi yang monoton.
        - Tidak mempertimbangkan pendapat atau preferensi pengguna lain, yang bisa membatasi keragaman rekomendasi.
2. Collaborative Filtering
    - Pembuatan Model: Menggunakan teknik Deep Learning dengan TensorFlow dan Keras untuk membangun model yang memprediksi rating film berdasarkan histori interaksi antara pengguna dan film. Model memanfaatkan struktur embedding untuk pengguna dan film, serta menggunakan input dari userId dan movieId.
    - Output: Sistem merekomendasikan Top-N film yang belum ditonton oleh pengguna berdasarkan prediksi rating tertinggi.
    - Kelebihan:
        - Mampu mengidentifikasi pola dan preferensi tersembunyi yang mungkin tidak terlihat dalam konten film itu sendiri, menawarkan rekomendasi yang lebih beragam.
        - Dapat meningkatkan dengan penambahan data; semakin banyak interaksi yang dianalisis, semakin akurat rekomendasinya.
    - Kekurangan:
        - Memerlukan data yang cukup besar untuk pelatihan model agar efektif, dikenal sebagai masalah cold start.
        - Kompleksitas komputasi lebih tinggi dibandingkan content-based filtering, membutuhkan sumber daya komputasi yang lebih besar.

Kedua pendekatan memiliki tempatnya masing-masing dalam ekosistem rekomendasi, dengan pilihan antara keduanya bergantung pada konteks aplikasi dan ketersediaan data.
- Content-Based Filtering lebih cocok untuk aplikasi yang kontennya sangat spesifik dan personalisasi yang mendalam sangat penting, atau ketika informasi tentang preferensi pengguna lebih mudah diakses daripada data interaksi pengguna lain.
- Collaborative Filtering cocok untuk platform dengan banyak pengguna dan interaksi, di mana dapat memanfaatkan pola besar tersebut untuk menghasilkan rekomendasi yang relevan dan beragam.

Dalam praktiknya, kombinasi keduanya sering kali memberikan hasil terbaik, menggabungkan kekuatan kedua pendekatan untuk mencapai rekomendasi yang personal, relevan, dan beragam.

**Result**
**Top-N Rekomendasi Content-Based Filtering**
***Judul film:*** Cocoanuts, The (1929)
***Genre:*** Comedy|Musical
***Tag:***
***Nilai Precision:*** 1

Tabel 3. Top-N Rekomendasi Content-Based Filtering:
| No | Judul Film                                      | Tahun | Genre          | Skor Kesamaan |
|----|-------------------------------------------------|-------|----------------|---------------|
| 1  | Band Wagon, The (1953)                          | 1953  | Comedy\|Musical | 1.0           |
| 2  | Great Race, The (1965)                          | 1965  | Comedy\|Musical | 1.0           |
| 3  | History of the World: Part I (1981)             | 1981  | Comedy\|Musical | 1.0           |
| 4  | Help! (1965)                                    | 1965  | Comedy\|Musical | 1.0           |
| 5  | Holiday Inn (1942)                              | 1942  | Comedy\|Musical | 1.0           |
| 6  | Anchors Aweigh (1945)                           | 1945  | Comedy\|Musical | 1.0           |
| 7  | Beach Blanket Bingo (1965)                      | 1965  | Comedy\|Musical | 1.0           |
| 8  | Can't Stop the Music (1980)                     | 1980  | Comedy\|Musical | 1.0           |
| 9  | À nous la liberté (Freedom for Us) (1931)       | 1931  | Comedy\|Musical | 1.0           |
| 10 | Girls! Girls! Girls! (1962)                     | 1962  | Comedy\|Musical | 1.0           |

**Top-N Rekomendasi Collaborative Filtering**
***User ID:*** 584

Tabel 4. Top-N Rekomendasi Collaborative Filtering
| No | Judul Film                                          |
|----|-----------------------------------------------------|
| 1  | Perez Family, The (1995)                           |
| 2  | Last Supper, The (1995)                            |
| 3  | Wallace & Gromit: The Best of Aardman Animation    |
| 4  | Iron Giant, The (1999)                             |
| 5  | South Pacific (1958)                               |

## Evaluation
Untuk evaluasi model Content Based Filtering digunakan metrik precision yang mengukur proporsi rekomendasi yang relevan terhadap jumlah total rekomendasi yang diberikan. Rumusnya adalah:

![equation](https://latex.codecogs.com/svg.image?Precision=%5Cfrac%7BJumlah%20Rekomendasi%20yang%20Relevan%7D%7BJumlah%20Total%20Rekomendasi%7D)

Sedangkan dalam evaluasi model Collaborative Filtering yang dikembangkan menggunakan TensorFlow dan Keras, Mean Square Error (MSE) digunakan sebagai metrik utama untuk mengukur akurasi prediksi model terhadap rating yang sebenarnya. MSE memberikan gambaran kuantitatif mengenai tingkat kesalahan prediksi model dalam mengestimasi rating film, dengan fokus pada bagaimana model mampu meminimalkan kesalahan tersebut sepanjang proses pelatihan dan validasi.

MSE dihitung menggunakan rumus berikut:

![equation](https://latex.codecogs.com/svg.image?MSE=%5Csum_%7Bi=1%7D%5E%7BD%7D(x_i-y_i)%5E2)

Selama pelatihan, model dijalankan melalui beberapa epoch, dimana setiap epoch merupakan satu siklus di mana seluruh dataset training dilewati oleh model. Dalam proses ini, RMSE dihitung untuk setiap batch data sebagai indikator kinerja model. Model bertujuan untuk meminimalkan nilai RMSE dengan mengadjust bobotnya berdasarkan backpropagation dan optimasi gradient descent. Hasil pelatihan divisualisasikan dalam bentuk grafik yang menampilkan Training Loss dan Validation Loss mengilustrasikan perubahan nilai RMSE selama proses pelatihan untuk dataset training dan validation. Grafik ini penting karena memberikan gambaran tentang:
- Peningkatan Akurasi Prediksi: Penurunan RMSE menunjukkan peningkatan kemampuan model dalam memprediksi rating yang akurat.
- Deteksi Overfitting: Jika Validation Loss mulai meningkat sementara Training Loss terus menurun, ini menandakan model mulai overfit terhadap training data dan performanya pada data baru (validation data) mulai menurun.

**Kesimpulan**
Hasil evaluasi model Content Based Filtering mempunyai nilai precision yang sempurna (1) menandakan bahwa semua rekomendasi yang diberikan relevan. Meskipun pada pandangan pertama ini bisa dianggap sebagai indikasi sukses total, tetap harus mempertimbangkan potensi bias dalam evaluasi atau kurangnya variasi dalam skenario pengujian.

Sedangkan hasil Collaborative Filtering menggunakan Mean Squared Error (MSE) sebagai metrik seperti gambar 5, teramati penurunan yang konsisten dalam loss pelatihan dan validasi selama iterasi, yang menandakan peningkatan kemampuan prediktif model. Penurunan ini mencapai stabilitas tanpa adanya divergensi antara keduanya, mengindikasikan bahwa model telah menghindari overfitting, yang sering menjadi tantangan dalam sistem rekomendasi.

![Gambar hasil training dan validasi](https://github.com/undagie/Dicoding-Recomendation-System/blob/main/img/train_val_vis.png?raw=true)

Gambar 5. Visualisasi metrik training dan loss validasi

Secara lebih khusus, loss validasi konvergen ke nilai yang dekat dengan loss pelatihan, mendekati nilai 0.9. Ini menunjukkan bahwa model mampu melakukan generalisasi dari data yang dilihat selama pelatihan ke data yang tidak dikenal, yang tercermin dalam dataset validasi.

Melihat kembali pernyataan masalah dan tujuan dari proyek ini, yang bertujuan untuk memberikan rekomendasi yang relevan dan personal kepada pengguna, hasil yang diperoleh menunjukkan kesuksesan terhadap pencapaian tujuan tersebut. Namun, untuk menyelesaikan masalah yang diangkat dengan lebih komprehensif, direkomendasikan untuk melakukan pengujian lebih lanjut, khususnya dalam setting yang lebih beragam dan dengan kasus penggunaan yang lebih luas. Proyek ini telah berhasil menjawab pertanyaan-pertanyaan yang diajukan dalam pernyataan masalah dengan memanfaatkan dataset MovieLens untuk membangun dan mengevaluasi model rekomendasi berbasis content-based filtering dan collaborative filtering. Faktor-faktor yang mempengaruhi efektivitas model rekomendasi teridentifikasi melalui analisis eksploratif yang mendalam, yang mencakup distribusi rating dan analisis genre serta tag dari film.

Hasil analisis ini telah membantu dalam pembuatan fitur dan pemahaman konten yang relevan dengan preferensi pengguna. Kedua model rekomendasi yang dikembangkan menunjukkan kemampuan untuk merekomendasikan film yang relevan kepada pengguna. Model content-based filtering terutama efektif dalam merekomendasikan film dengan konten yang serupa dengan preferensi pengguna sebelumnya, sementara model collaborative filtering memanfaatkan pola rating dari banyak pengguna untuk mengidentifikasi film yang mungkin disukai oleh pengguna tertentu, meskipun pengguna tersebut belum pernah menilai film yang serupa.

## Daftar Referensi
[[1]] Orue-Saiz, M. Rico-González, J. Pino-Ortega, and A. Méndez-Zorrilla, "Improving diet through a recommendation system using physical activity data and healthy diet indexes of female futsal players," Proceedings of the Institution of Mechanical Engineers, Part P: Journal of Sports Engineering and Technology, vol. 0, no. 0, 2024.<br>
[[2]] R.N. Ravikumar, S. Jain, and M. Sarkar, "AdaptiLearn: real-time personalized course recommendation system using whale optimized recurrent neural network," Int J Syst Assur Eng Manag, 2024.<br>
[[3]] H. Imantho, K. B. Seminar, E. Damayanthi, N. E. Suyatma, K. Priandana, B. W. Ligar, and A. U. Seminar, "An Intelligent Food Recommendation System for Dine-in Customers with Non-Communicable Diseases History," Jurnal Keteknikan Pertanian, vol. 12, no. 1, pp. 140-152, 2024.<br>
[[4]] M. KabirMamdouh, Alireza, and A. Gürhan Kök, "A Personalized Content-Based Method to Predict Customers’ Preferences in an Online Apparel Retailer." SSRN, 2024.<br>
[[5]] D. Leela Krishna Reddy and H. Vaghela, "Music Recommendation System Using Machine Learning," EasyChair Preprint no. 12850, Mar. 31, 2024.<br>
[[6]] M. Surya Negara and A. Z. M., "Implementasi Machine Learning dengan Metode Collaborative Filtering dan Content-Based Filtering pada Aplikasi Mobile Travel (Bangkit Academy)," in JBegaTI, vol. 5, no. 1, Mar. 2024, pp. 126-136.

   [1]: <https://journals.sagepub.com/doi/abs/10.1177/17543371241241847>
   [2]: <https://link.springer.com/article/10.1007/s13198-024-02301-2>
   [3]: <https://jurnalpenyuluhan.ipb.ac.id/index.php/jtep/article/view/52452>
   [4]: <https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4782004>
   [5]: <https://easychair.org/publications/preprint_download/qr7B>
   [6]: <http://begawe.unram.ac.id/index.php/JBTI/article/view/1193>
   [MovieLens]: <://grouplens.org/datasets/movielens/>
