# Laporan Proyek Kedua Sistem Rekomendasi - Gina Cahya Utami

## Project Overview
Pada proyek ini, saya mengangkat topik mengenai sistem rekomendasi pada aplikasi toko buku online. Latar belakang proyek yang saya angkat yakni dikarenakan akan ada dua tipe pengguna dalam aplikasi toko buku online. Pengguna yang pertama yakni pengguna yang hanya akan mencari barang yang sesuai dengan apa yang dia butuhkan, pengguna yang kedua ada juga pengguna yang benar-benar tidak mempunyai ide atau referensi apapun ketika membuka aplikasi toko buku tersebut. Dengan menggunakan dataset yang berisi data mengenai user(pengguna), informasi buku, dan informasi rating pada buku, saya akan  membuat sebuah sistem rekomendasi buku yang dapat digunakan untuk pengguna aplikasi toko buku online untuk mengangkat permasalahan dari latar belakang tersebut.

Proyek ini penting untuk diselesaikan karena dapat memudahkan pelanggan untuk melihat rekomendasi buku lain ketika berbelanja di toko buku online tersebut. Dengan membaca pola data, mudah bagi kita merekomendasikan buku kepada pelanggan. Selain dapat membantu pengguna dalam memberikan referensi buku, kita juga mendapat keuntungan yakni dalam meningkatkan minat berbelanja dan keingin-tahuan pelanggan terhadap buku yang direkomendasikan sehingga tingkat penjualan pun akan meningkat.

## Business Understanding
Dengan menggunakan dataset mengenai data user(pengguna), informasi mengenai buku, dan rating yang diberikan oleh pengguna kepada buku, kita akan membuat sebuah sistem rekomendasi untuk memberikan referensi Top-N Buku yang akan direkomendasikan kepada pengguna aplikasi toko online. Dengan adanya referensi tersebut, penjualan pun akan meningkat karena pengguna mempunyai referensi baru dalam daftar belanjaan buku mereka.

### Problem Statements
Berdasarkan kondisi yang telah diuraikan sebelumnya, berikut merupakan permasalahan yang perlu dipecahkan :  
- Dari serangkaian dataset yang ada, bagaimana cara membuat sistem rekomendasi menggunakan teknik Content-Based Filtering?
- Bagaimana aplikasi toko online tersebut dapat merekomendasikan buku lain yang mungkin akan disukai dan belum pernah dipilih/dibaca oleh user(pengguna)?

### Goals
Berikut goals untuk menjawab permasalahan di atas :
- Membuat sistem rekomendasi mengenai buku pada aplikasi toko buku menggunakan teknik Content-Based filtering.
- Membuat sistem rekomendasi mengenai buku lain yang mungkin akan disukai dan belum pernah dipilih/dibaca oleh pengguna aplikasi toko buku menggunakan teknik Collaborative Filtering.

### Solution statements
Untuk menyelesaikan permasalahan di atas, kita akan menggunakan dua algoritma sistem rekomendasi sebagai solusi permasalahan, yakni Content-Based Filtering dan yang kedua yakni Collaborative Filtering.
- **Content Based Filtering**. Algoritma ini akan merekomendasikan item yang mirip dengan item yang disukai pengguna di masa lalu. Kelebihan dari algoritma ini yakni  semakin banyak informasi yang diberikan pengguna, semakin baik akurasi sistem rekomendasi. Kekurangannya yakni Sistem hanya akan menunjukkan item yang nilainya tinggi untuk dicocokkan dengan profil pengguna, maka pengguna akan selalu menemukan item serupa dengan yang sudah direkomendasikan sebelumnya.
- **Collaborative Filtering**. Collaborative filtering bergantung pada pendapat komunitas pengguna. Ia tidak memerlukan atribut untuk setiap itemnya seperti pada sistem berbasis konten. Kelebihan dari teknik ini yakni dapat membantu pengguna menemukan minat baru. Kekurangannya yakni tidak dapat menangani item baru/fresh. Jadi, jika item tidak terlihat selama pelatihan, sistem tidak dapat melakukan proses embedding untuk item tersebut dan tidak dapat mengkueri model dengan item ini.

## Data Understanding
Data yang saya gunakan yakni diunduh dari situs [Book Recommendation Dataset - Kaggle](https://www.kaggle.com/arashnic/book-recommendation-dataset). Dengan mengunduh dataset dari link tersebut, kita akan mendapatkan 3 dataset dengan ekstensi .csv(Comma-separated values). Berikut penjelasannya :
- Dataset yang pertama yakni 'Users' yang memiliki jumlah 278.858 data dan 3 kolom, yakni :
    1. Kolom 'User-ID', berisi ID pengguna dari toko buku online
    2. Kolom 'Location', berisi lokasi pengguna.
    3. Kolom 'Age', berisi usia pengguna.
- Dataset yang kedua yakni 'Books' yang memiliki jumlah 271.360 data dan memiliki 8 kolom, diantaranya :
    1. Kolom 'ISBN', merupakan identifikasi dari masing-masing buku.
    2. Kolom 'Book-Title', merupakan judul buku.
    3. Kolom 'Book-Author', merupakan penulis buku.
    4. Kolom 'Year-Of-Publication', merupakan tahun dipublikasikannya buku.
    5. Kolom 'Publisher', merupakan penerbit buku.
    6. Kolom 'Image-URL-S', marupakan URL gambar cover buku dalam ukuran S(Small)
    7. Kolom 'Image-URL-M', marupakan URL gambar cover buku dalam ukuran M(Medium)
    8. Kolom 'Image-URL-L', marupakan URL gambar cover buku dalam ukuran L(Large)
- Dataset yang kedua yakni 'Ratings' yang memiliki jumlah 1.149.780 data dan memiliki 3 kolom, berikut penjelasan mengenai kolom-kolomnya :
    1. Kolom 'User-ID', yang berisi ID dari user yang memberikan rating terhadap buku.
    2. Kolom 'ISBN', merupakan identifikasi buku atau nomor buku yang diberi rating oleh user
    3. Kolom 'Book-Rating', berisi nilai Rating dari buku, skala yang ada dalam rating ini yakni dari 0-10.
    
Dalam tahap ini saya melakukan Data loading dan proses EDA(Exploratory Data Analysis) yang menjelaskan variabel-variabel yang sudah dijelaskan sebelumnya. Dikarenakan ketiga dataset yang kita gunakan merupakan dataset dalam jumlah yang banyak yakni lebih dair 200.000, maka di pada proses ini saya hanya mengambil 7.000 data pertama dari setiap variabel di atas dalam pembuatan sistem rekomendasi ini.

## Data Preparation
- Tahapan Data Preparation yang saya gunakan dalam teknik Content-Based Filtering adalah :
    1. **Mengatasi Missing Value**. Alasan mengapa diperlukan tahapan ini dalam data preparation yakni karena dengan tidak adanya missing value akan membuat performa dalam pembuatan model menjadi lebih baik. Langkah pertama yang saya gunakan di sini adalah mengecek apakah ada missing value atau tidak menggunakan fungsi .isnull().sum() . Jika terdapat nilai yang hilang atau null, maka kita atasi menggunakan fungsi .dropna()
    2. **Mengatasi data duplikat**. Alasan mengapa diperlukan tahapan ini dalam data preparation yakni kita hanya akan menggunakan data unik untuk dimasukkan ke dalam proses pemodelan, sehingga perlu mengatasi data duplikat dengan cara menghapusnya. Langkah untuk menghapus data duplikat yakni dengan menggunakan fungsi .drop_duplicates() dan kita definisikan variabel mana yang kita hindari duplikatnya, variabel tersebut adalah 'ISBN', sehingga fungsinya menjadi seperti berikut : .drop_duplicates('ISBN')
    3. **Konversi data series menjadi list**. Alasan mengapa diperlukan tahapan ini dalam data preparation yakni kedepannya kita akan membuat sebuah dictionary yang di dalam nya terdapat kumpulan dari informasi id, book_author, dan book_title. Sebelum membuat sebuah dictionary, kita ubah informasi tersebut ke dalam sebuah list dulu dikarenakan sebelumnya informasi tersebut dalam bentuk data series. Untuk mengonversi data series menjadi list, kita menggunakan fungsi tolist() dari library numpy. Kita akan konversi variabel 'ISBN', 'Book-Author', dan 'Book-Title' menjadi list.
    4. **Membuat Dictionary** Alasan mengapa diperlukan tahapan ini dalam data preparation yakni dari informasi mengenai id, book_author, dan book_title yang sudah dalam bentuk list, selanjutnya kita akan membuat sebuah dictionary. Untuk data book_title, book_author, dan book_id, ini berfungsi untuk menentukan pasangan key-value pada data book_title, book_author, dan book_id.
- Tahapan Data Preparation yang saya gunakan dalam teknik Colaborative Filtering adalah :
    1. **Melakukan persiapan data untuk menyandikan (encode) fitur 'User-ID' dan 'ISBN' ke dalam indeks integer.** Alasan mengapa diperlukan tahapan ini dalam data preparation yakni proses encoding merupakan proses pada data yang berupa kategorikal harus diubah terlebih dahulu menjadi data numerikal agar model Machine Learning bisa memproses data tersebut.
    2. **Memetakan ‘User-ID’ dan ‘ISBN’ ke dataframe yang berkaitan.** Alasan mengapa diperlukan tahapan ini dalam data preparation yakni setelah membuat sebuah data berisi fitur 'User-ID' dan 'ISBN' yang telah menjalani proses encoding, selanjutnya kita masukan kedua fitur tersebut ke dataframe user dan buku.
    3. **Mengecek beberapa hal dalam data seperti jumlah user, jumlah buku, kemudian mengubah nilai rating menjadi float.** Alasan mengapa diperlukan tahapan mengubah nilai float pada rating dalam data preparation yakni agar memudahkan penjumlahan ketika proses pembagian train dan test data.
    4. **Membagi Data untuk Training dan Validasi.** Alasannya karena teknik ini berguna untuk membagi data menjadi data uji dan data latih, kita gunakan rasio 80:20.

## Modeling
Pada tahap ini, saya mengembangkan model machine learning dengan dua algoritma untuk menyelesaikan permasalahan dan **menyajikan top-N recommendation sebagai solusi.**, yakni dengan menggunakan algoritma Content-Based Filtering dan Collaborative Filtering.
- **Content-Based Filtering**. Berikut Prosesnya :
    1. Melakukan TF-IDF Vectorizer. TF-IDF Vectorizer akan digunakan pada sistem rekomendasi untuk menemukan representasi fitur penting dari setiap kategori buku. Kita akan menggunakan fungsi tfidfvectorizer() dari library sklearn.
    2. Melakukan fit pada label 'book_title' lalu ditransformasikan ke bentuk matrix.
    3. Mengubah vektor tf-idf dalam bentuk matriks dengan fungsi todense().
    4. Membuat dataframe untuk melihat tf-idf matrix, dengan kolom dan juga baris yang diisi dengan judul buku.
    5. Menghitung cosine similarity pada matrix tf-idf
    6. Membuat dataframe dari variabel cosine_sim dengan baris dan kolom berupa author buku
    7. Membuat fungsi bernama book_recommendations untuk mendapatkan Rekomendasi dengan memasukkan parameter berupa nama penulis buku, penulis yang saya gunakan yakni Peter Carey. Berikut hasil rekomendasi yang kita dapatkan dari fungsi tersebut :
    
        | No | book_author | book_title |
        | -- | ----------- | ---------- |
        | 0 | DONNA TARTT | Secret History |
        | 1 | Jostein Gaarder | Sophies world: a novel about the history of philosophy |
        | 2 | Sebastian Junger | The Perfect Storm: A True Story of Men Against the Sea |
        | 3 | Adeline Yen Mah | Falling Leaves: The Memoir of an Unwanted Chinese Daughter |
        | 4 | Wally Lamb | I Know This Much Is True |
        | 5 | Wally Lamb | She's Come Undone (Oprah's Book Club (Paperback)) |
        | 6 | Wally Lamb | She's Come Undone (Oprah's Book Club) |
        | 7 | Marc Levy | If Only It Were True |
        | 8 | Stan Redding | Catch Me If You Can: The True Story of a Real Fake |
        | 9 | WILLIAM GOLDMAN | The Princess Bride: S. Morgenstern's Classic Tale of True Love and High Adventure |

    Penjelasan dari Hasil rekomendasi di atas yakni kita telah mampu menampilkan 5 buku rekomendasi dengan kategori yang mirip dengan buku yang ditulis oleh Karen Rauch Carter.
- **Collaborative Filtering**. Berikut Prosesnya :
    1. Kita membuat class RecommenderNet yang didalamnya terdapat proses menghitung skor kecocokan antara user dan buku dengan teknik embedding.
    2. Menginisialisasi model ReccomenderNet
    3. Compile model tersebut dengan menggunakan loss BinaryCrossentropy(), optimizernya kita menggunakan Adam, metrik nya kita gunakan RMSE atau RootMeanSquaredError()
    4. Melakukan proses training menggunakan proses .fit() dengan parameternya yakni batch_size = 8, dan epochsnya yakni sebesar 100.
    5. Mendapatkan Rekomendasi Resto dengan cara definisikan variabel book_not_visited yang merupakan daftar resto yang belum pernah dikunjungi oleh pengguna dengan menerapkan kode berikut
        ```
        book_df = book_new
        data = pd.read_csv('/content/drive/MyDrive/Colab Notebooks/Proyek Kedua Sistem Rekomendasi/Ratings.csv')
        df = data[1:7001]
        
        # Mengambil sample user
        user_id = df['User-ID'].sample(100).iloc[30]
        book_visited_by_user = df[df['User-ID'] == user_id]
         
        # Operator bitwise (~)
        book_not_visited = book_df[~book_df['id'].isin(book_visited_by_user.ISBN.values)]['id'] 
        book_not_visited = list(
            set(book_not_visited)
            .intersection(set(book_to_book_encoded.keys()))
        )
         
        book_not_visited = [[book_to_book_encoded.get(x)] for x in book_not_visited]
        user_encoder = user_to_user_encoded.get(user_id)
        user_book_array = np.hstack(
            ([[user_encoder]] * len(book_not_visited), book_not_visited)
        ```
        Selanjutnya, untuk memperoleh rekomendasi restoran, gunakan fungsi model.predict() dari library Keras dengan menerapkan kode berikut.
        ```
        rating = model.predict(user_book_array).flatten()
        top_rating_indices = rating.argsort()[-10:][::-1]
        recommended_book_ids = [
            book_encoded_to_book.get(book_not_visited[x][0]) for x in top_rating_indices
        ]
        print('Menampilkan Rekomendasi Book Author untuk Pengguna dengan Total : {} Pengguna'.format(user_id))
        print('===' * 11)
        top_book_user = (
            book_visited_by_user.sort_values(
                by = 'Book-Rating',
                ascending=False
            )
            .head(5)
            .ISBN.values
        )
         
        book_df_rows = book_df[book_df['id'].isin(top_book_user)]
        for row in book_df_rows.itertuples():
            print(row.book_name, ':', row.book_title)
         
        print('---' * 11)
        print('Top 10 Book Author Recommendation')
        print('---' * 11)
         
        recommended_book = book_df[book_df['id'].isin(recommended_book_ids)]
        for row in recommended_book.itertuples():
            print(row.book_author, ':', row.book_title)
        ```
        output dari kode program yang telah kita jalankan di atas yakni :
        Menampilkan Rekomendasi Book Author untuk Pengguna dengan Total : 278418 Pengguna
        |=================================
        |-------------------------------------
        |--------------------------------------------------------------
        |Top 10 Book Author Recommendation
        |--------------------------------------------------------------
        |James Finn Garner : Politically Correct Bedtime Stories: Modern Tales for |Our Life and Times
        |Richard Matheson : I Am Legend
        |Douglas Adams : Restaurant At the End of the Universe
        |Judith Guest : Ordinary People
        |Andre Dubus III : House of Sand and Fog
        |Margaret Atwood : Oryx and Crake
        |William Gerald Golding : Lord of the Flies
        |RITA MAE BROWN : The Tail of the Tip-Off
        |Jack Canfield : Chicken Soup for the Woman's Soul (Chicken Soup for the |Soul Series (Paper))
        |Dean Koontz : Die zweite Haut.
        Penjelasan dari hasil rekomendasi di atas yakni kita telah mampu menampilkan 10 rekomendasi buku yang mungkin akan disukai dan belum pernah dipilih/dibaca oleh pengguna sebelumnya menggunakan teknik Collaboratory Filtering.
        
## Evaluation
**1. CONTENT-BASED FILTERING**
Saya menggunakan dua metrik evaluasi yang digunakan untuk mengukur kinerja model pada content-based filtering, yang pertama menggunakan metrik Precision dan metrik Recall, berikut penjelasannya :
1. **Precision pada Top-N,** merupakan proporsi item yang direkomendasikan dalam set     Top-N yang relevan. Berikut rumusnya :
    Precision = ((k of recommendation that are relevant) / (k of item we recommend)) . 100 %
    Pada contoh rekomendasi buku dapat kita simpulkan bahwa :
    k of recommendation that are relevant = 8 buku
    k of item we recommend = 10 buku
    Precision = ((8)/(10)) . 100 %
    Jadi presisinya = 80%
    Kenapa kita mendapatkan 8 buku? Karena kita mencari buku yang ditulis oleh Peter Carey yang berjudul True History of the Kelly Gang, sehingga terdapat 8 buku yang judulnya hampir mirip dengan judul buku True History of the Kelly Gang.
2. **Recall**
    Recall pada Top-N, merupakan proporsi item relevan yang ditemukan di rekomendasi top-N. Berikut rumusnya :
    Recall = (k of recommendation that are relevant) / (total k of relevant items)
    Pada contoh rekomendasi buku dapat kita simpulkan bahwa :

    a. Pada recall@6
        k of recommendation that are relevant = 5 buku
        total k of relevant items = 8 buku
        Recall @ 6 = (5)/(8)
        Jadi presisinya = 5/8
        
    b. Pada recall@10
        k of recommendation that are relevant = 8 buku
        total k of relevant items = 8 buku
        Recall @ 10 = (8)/(8)
        Jadi presisinya = 8/8

**2. COLLABORATIVE FILTERING**
    Saya menggunakan dua metrik evaluasi yang digunakan untuk mengukur kinerja model pada collaborative filtering, yang pertama menggunakan metrik Root Mean Squared Error (RMSE) dan metrik Accuracy, berikut penjelasannya :
1. **Root Mean Squared Error (RMSE)** merupakan besarnya tingkat kesalahan hasil 
prediksi, dimana semakin kecil (mendekati 0) nilai RMSE maka hasil prediksi 
akan semakin akurat. Berikut kode programnya :
    ```
    metrics=[tf.keras.metrics.RootMeanSquaredError()]
    ```
    Kemudian saya visualisasikan metrik tersebut menggunakan plot dari library 
    ```
    matplotlib.pyplot, berikut  kode programnya :
    plt.plot(history.history['root_mean_squared_error'])
    plt.plot(history.history['val_root_mean_squared_error'])
    plt.title('model_metrics')
    plt.ylabel('root_mean_squared_error')
    plt.xlabel('epoch')
    plt.legend(['train', 'test'], loc='upper left')
    plt.show()
    ```
    Dari plot di atas dapat kita memperoleh nilai error akhir sebesar sekitar 0.30 dan error pada data validasi kurang dari  0.10. Nilai tersebut cukup bagus untuk sistem rekomendasi.
2. **Mean Squared Error (MSE)**. Teknik ini menghitung selisih rata-rata nilai sebenarnya dengan nilai prediksi. Berikut kode programnya :
    ```
    from sklearn.metrics import mean_squared_error
    print("MSE dari pada data train = ", mean_squared_error(y_true=y_train, y_pred=model.predict(x_train))/1e3)
    print("MSE dari pada data validation = ", mean_squared_error(y_true=y_val, y_pred=model.predict(x_val))/1e3)
    ```
    Hasil dari kode program di atas yakni :
    MSE dari pada data train =  3.054160906056424e-05
    MSE dari pada data validation =  8.885263383941311e-05

> **Ini adalah bagian akhir laporan**