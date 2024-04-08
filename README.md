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

## Evaluation
Dalam evaluasi model Collaborative Filtering yang dikembangkan menggunakan TensorFlow dan Keras, Root Mean Square Error (RMSE) digunakan sebagai metrik utama untuk mengukur akurasi prediksi model terhadap rating yang sebenarnya. RMSE memberikan gambaran kuantitatif mengenai tingkat kesalahan prediksi model dalam mengestimasi rating film, dengan fokus pada bagaimana model mampu meminimalkan kesalahan tersebut sepanjang proses pelatihan dan validasi.

RMSE dihitung menggunakan rumus berikut:

![equation](https://latex.codecogs.com/svg.image?%5Ctext%7BRMSE%7D(y,%5Chat%7By%7D)=%5Csqrt%7B%5Cfrac%7B%5Csum_%7Bi=0%7D%5E%7BN-1%7D(y_i-%5Chat%7By%7D_i)%5E2%7D%7BN%7D%7D)

RMSE mengkuadratkan selisih antara nilai sebenarnya dan nilai prediksi untuk memastikan bahwa kesalahan negatif dan positif tidak saling menghilangkan. Kemudian, mengambil akar kuadrat dari rata-rata kesalahan kuadrat tersebut memungkinkan RMSE memiliki satuan yang sama dengan data rating.

Selama pelatihan, model dijalankan melalui beberapa epoch, dimana setiap epoch merupakan satu siklus di mana seluruh dataset training dilewati oleh model. Dalam proses ini, RMSE dihitung untuk setiap batch data sebagai indikator kinerja model. Model bertujuan untuk meminimalkan nilai RMSE dengan mengadjust bobotnya berdasarkan backpropagation dan optimasi gradient descent. Hasil pelatihan divisualisasikan dalam bentuk grafik yang menampilkan Training Loss dan Validation Loss mengilustrasikan perubahan nilai RMSE selama proses pelatihan untuk dataset training dan validation. Grafik ini penting karena memberikan gambaran tentang:
- Peningkatan Akurasi Prediksi: Penurunan RMSE menunjukkan peningkatan kemampuan model dalam memprediksi rating yang akurat.
- Deteksi Overfitting: Jika Validation Loss mulai meningkat sementara Training Loss terus menurun, ini menandakan model mulai overfit terhadap training data dan performanya pada data baru (validation data) mulai menurun.

## Daftar Referensi
[[1]] N. Chakrabarty dan S. Biswas, "Statistical Approach to Adult Census Income Level Prediction," in Proceedings of the International Conference on Machine Learning and Data Engineering (iCMLDE), IEEE, 2018.<br>
[[2]] M. A. Islam, A. Nag, N. Roy, A. R. Dey, S. M. F. A. Fahim, dan A. Ghosh, "An Investigation into the Prediction of Annual Income Levels Through the Utilization of Demographic Features Employing the Modified UCI Adult Dataset," in Proceedings of the International Conference on Advances in Electrical Engineering (ICAEE), IEEE, 2020.<br>
[[3]] F. Ding, M. Hardt, J. Miller, dan L. Schmidt, "Retiring Adult: New Datasets for Fair Machine Learning," in Advances in Neural Information Processing Systems (NeurIPS), 2021.<br>
[[4]] S. R. G. Shashidhar, "Big data in healthcare: management, analysis and future prospects," in Journal of Big Data, vol. 8, no. 1, Springer, 2021.<br>

   [1]: <https://journals.sagepub.com/doi/abs/10.1177/17543371241241847>
   [2]: <https://link.springer.com/article/10.1007/s13198-024-02301-2>
   [3]: <https://jurnalpenyuluhan.ipb.ac.id/index.php/jtep/article/view/52452>
   [4]: <https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4782004>
   [5]: <https://easychair.org/publications/preprint_download/qr7B>
   [6]: <http://begawe.unram.ac.id/index.php/JBTI/article/view/1193>
   [MovieLens]: <://grouplens.org/datasets/movielens/>
