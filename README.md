# Laporan Proyek Machine Learning

## Project Overview

### Latar Belakang

Indonesia dikenal dengan kekayaan alam dan budaya yang melimpah, menjadikannya salah satu destinasi wisata utama di dunia. Berdasarkan data dari Badan Pusat Statistik (BPS), pada tahun 2019, jumlah wisatawan mancanegara yang berkunjung ke Indonesia tercatat mencapai 16,11 juta orang, yang menunjukkan peningkatan dibandingkan tahun-tahun sebelumnya [[1](https://www.bps.go.id/id/pressrelease/2020/02/03/1711/jumlah-kunjungan-wisman-ke-indonesia-desember-2019-mencapai-1-38-juta-kunjungan-.html)]. Namun, akibat pandemi COVID-19, kunjungan wisatawan menurun tajam pada tahun 2020 dan 2021, dengan penurunan sekitar 75% dibandingkan periode sebelumnya [[2](https://www.unwto.org/impact-assessment-of-the-covid-19-outbreak-on-international-tourism)]. Meskipun demikian, sektor pariwisata domestik tetap menjadi tumpuan ekonomi, dan pemulihan sektor ini kini menjadi prioritas utama pemerintah Indonesia.

Untuk mempercepat pemulihan dan meningkatkan sektor pariwisata, diperlukan pendekatan yang efektif dalam menarik minat wisatawan. Salah satu tantangan utama adalah membantu wisatawan dalam memilih destinasi yang sesuai dengan preferensi mereka, mengingat banyaknya pilihan yang tersedia. Tanpa panduan yang tepat, wisatawan dapat merasa bingung, yang pada gilirannya dapat mengurangi kepuasan mereka selama berwisata [[3](https://senti.ft.ugm.ac.id/wp-content/uploads/sites/454/2024/10/Pengembangan-Sistem-Rekomendasi-Rencana-Perjalanan-Literature-Review.pdf)]. Oleh karena itu, pengembangan sistem rekomendasi destinasi wisata sangat penting untuk membantu wisatawan menemukan pilihan yang tepat sesuai dengan minat dan kebutuhan mereka.

Banyak penelitian telah dilakukan terkait dengan pengembangan sistem rekomendasi untuk sektor pariwisata. Sebagai contoh, penelitian oleh Naatonis (2019) mengembangkan sistem rekomendasi destinasi wisata di Kota Kupang menggunakan metode Weighted Product, yang mempertimbangkan kriteria biaya, fasilitas, dan ulasan pengunjung [[4](https://publikasi.uyelindo.ac.id/index.php/hoaq/article/view/15)]. Selain itu, penelitian oleh Murzani dan Sari (2023) menggunakan metode collaborative filtering untuk memberikan rekomendasi destinasi wisata di Aceh, menghasilkan 7 rekomendasi destinasi teratas bagi pengguna [[5](https://jurnal.usk.ac.id/kitektro/article/view/36168)].

Namun, meskipun metode collaborative filtering yang memanfaatkan data rating pengguna dapat memberikan rekomendasi yang lebih akurat dengan mengenali pola preferensi pengguna, metode ini menghadapi tantangan dalam mengatasi masalah cold-start. Ini terjadi ketika sistem kesulitan memberikan rekomendasi untuk pengguna baru atau destinasi baru yang belum memiliki data interaksi [[6](https://link.springer.com/book/10.1007/978-1-4899-7637-6)].

Proyek ini bertujuan untuk mengatasi tantangan tersebut dengan menggabungkan metode TF-IDF (Term Frequency-Inverse Document Frequency) [[7](https://www.emerald.com/insight/content/doi/10.1108/00220410410560582/full/html)], content-based filtering berbasis cosine similarity, serta collaborative filtering berbasis neural network. TF-IDF akan digunakan untuk mengubah deskripsi destinasi menjadi vektor numerik, memungkinkan sistem untuk mengukur kesamaan antar destinasi menggunakan cosine similarity. Pendekatan ini akan memungkinkan sistem untuk memberikan rekomendasi yang lebih relevan berdasarkan fitur teks, seperti deskripsi dan kategori destinasi [[6](https://link.springer.com/book/10.1007/978-1-4899-7637-6)]. Di sisi lain, collaborative filtering berbasis neural network akan membantu mengatasi masalah cold-start dengan memetakan preferensi pengguna ke dalam embedding yang dapat diperbarui secara dinamis [[8](https://ieeexplore.ieee.org/document/9338389)].

Dengan demikian, sistem rekomendasi ini diharapkan dapat mendukung pemulihan sektor pariwisata Indonesia dan membantu wisatawan dalam menemukan destinasi wisata yang sesuai dengan preferensi mereka. Dengan menggabungkan berbagai metode ini, proyek ini bertujuan untuk memberikan hasil rekomendasi yang akurat dan relevan, meskipun terdapat berbagai variasi preferensi pengguna.

## Business Understanding
1. **Bagaimana cara memberikan rekomendasi destinasi wisata yang relevan berdasarkan deskripsi dan kategori konten?**  
   Destinasi yang direkomendasikan harus sesuai dengan preferensi pengguna, dengan mempertimbangkan informasi yang ada pada deskripsi dan kategori destinasi, seperti budaya, alam, atau kategori lainnya. Tantangan ini memerlukan solusi yang dapat memanfaatkan informasi detail tentang setiap tempat.

2. **Bagaimana cara menggunakan data rating pengguna untuk meningkatkan akurasi rekomendasi?**  
   Sistem rekomendasi perlu memperhitungkan rating yang diberikan oleh pengguna sebelumnya untuk meningkatkan kualitas rekomendasi yang disarankan.

3. **Bagaimana cara mengukur relevansi rekomendasi agar sistem dapat memberikan hasil yang tepat?**  
   Sistem rekomendasi harus mampu memberikan hasil yang sesuai dengan kebutuhan pengguna. Oleh karena itu, diperlukan metrik evaluasi yang efektif untuk memantau dan mengukur sejauh mana sistem mampu menghasilkan rekomendasi yang relevan dan tepat sasaran.

### Tujuan
1. **Membangun sistem rekomendasi berbasis konten yang efisien dengan memanfaatkan deskripsi dan kategori destinasi wisata.**  
   Menggunakan informasi yang terdapat pada deskripsi dan kategori destinasi untuk mencocokkan rekomendasi dengan minat pengguna yang sudah pernah mengunjungi tempat wisata tertentu.

2. **Mengembangkan sistem rekomendasi destinasi wisata berdasarkan pola penilaian pengguna.**  
   Membangun sistem rekomendasi yang mempertimbangkan interaksi dan rating pengguna, untuk memastikan rekomendasi mencerminkan pengalaman dan preferensi pengguna lainnya.

3. **Menilai kinerja sistem rekomendasi dengan menggunakan metrik evaluasi yang relevan.**  
   Metrik evaluasi akan digunakan untuk memonitor relevansi, akurasi, dan konsistensi dari hasil rekomendasi yang diberikan oleh sistem.

### Pendekatan Solusi
1. **Content-based Filtering**  
   Sistem ini memanfaatkan informasi deskripsi dan kategori destinasi untuk memberikan rekomendasi berdasarkan karakteristik tempat. Langkah-langkah yang dilakukan adalah sebagai berikut:
   - **Preprocessing**  
     Data teks dibersihkan dengan menggabungkan deskripsi dan kategori, kemudian diubah menjadi bentuk numerik menggunakan metode TF-IDF Vectorizer.
   - **Pembentukan Model**  
     Setelah proses preprocessing, sistem menghitung kesamaan antar destinasi menggunakan cosine similarity dari vektor TF-IDF untuk mengukur kesamaan antar tempat wisata.
   - **Evaluasi**  
     Metrik yang digunakan untuk evaluasi content-based filtering adalah Precision@10, yang mengukur proporsi dari 10 rekomendasi teratas yang relevan dengan preferensi pengguna.

2. **Collaborative Filtering**  
   Sistem ini menggunakan data rating yang diberikan oleh pengguna untuk memberikan rekomendasi berdasarkan pola kesukaan dari pengguna lain yang memiliki preferensi serupa. Berikut adalah langkah-langkah yang dilakukan:
   - **Encoding Data**  
     ID pengguna dan ID destinasi diubah menjadi indeks numerik agar dapat digunakan dalam model neural network.
   - **Normalisasi Rating**  
     Rating dinormalisasi agar berada dalam rentang 0 hingga 1, untuk meningkatkan performa model dalam belajar.
   - **Modeling**  
     Model neural network dikembangkan dengan menggunakan embedding layer untuk pengguna dan destinasi, yang menghasilkan prediksi rating untuk setiap pasangan pengguna dan destinasi.
   - **Evaluasi**  
     Metrik yang digunakan untuk evaluasi collaborative filtering meliputi RMSE dan MAE, yang mengukur perbedaan antara rating prediksi dan rating aktual.

## Data Understanding
### Informasi Dataset

Link Dataset: [Indonesia Tourism Destination](https://www.kaggle.com/datasets/aprabowo/indonesia-tourism-destination)

Proyek ini menggunakan dataset Indonesia Tourism Destination yang dapat diakses melalui [Kaggle](https://www.kaggle.com/datasets/aprabowo/indonesia-tourism-destination). Dataset ini mencakup beberapa tempat wisata di lima kota besar di Indonesia, yaitu Jakarta, Yogyakarta, Semarang, Bandung, dan Surabaya. Data yang digunakan berasal dari dua file, yaitu `tourism_with_id.csv` dan `tourism_rating.csv`.  
- **File `tourism_with_id.csv`** berisi informasi tentang destinasi wisata di lima kota besar, dengan total 437 data.
- **File `tourism_rating.csv`** berisi 10.000 data rating pengguna terhadap destinasi wisata tertentu.

### Deskripsi Atribut Dataset

#### 1. File `tourism_with_id.csv`

| No. | Kode Atribut | Deskripsi                                      | Tipe Data |
|-----|--------------|------------------------------------------------|-----------|
| 1   | Place_Id     | Identifikasi unik untuk setiap destinasi wisata.| Integer   |
| 2   | Place_Name   | Nama destinasi wisata.                         | String    |
| 3   | Description  | Deskripsi singkat tentang destinasi.           | String    |
| 4   | Category     | Kategori destinasi (misalnya: Budaya, Alam).   | String    |
| 5   | City         | Kota tempat destinasi berada.                  | String    |
| 6   | Price        | Harga tiket masuk.                             | Integer   |
| 7   | Rating       | Rating rata-rata dari pengguna (0-5).          | Float     |
| 8   | Time_Minutes | Waktu yang dibutuhkan untuk mengunjungi (dalam menit). | Float     |
| 9   | Coordinate   | Koordinat geografis.                           | String    |
| 10  | Lat          | Latitude lokasi.                               | Float     |
| 11  | Long         | Longitude lokasi.                              | Float     |
| 12  | Unnamed: 11  | Kolom kosong.                                  | Float     |
| 13  | Unnamed: 12  | Nilai duplikat dari Place_id                   | Integer   |

#### 2. File `tourism_rating.csv`

| No. | Kode Atribut  | Deskripsi                                                   | Tipe Data |
|-----|---------------|-------------------------------------------------------------|-----------|
| 1   | User_Id       | Identifikasi unik untuk setiap pengguna                    | Integer   |
| 2   | Place_Id      | Identifikasi unik untuk setiap destinasi wisata             | Integer   |
| 3   | Place_Ratings | Rating yang diberikan oleh pengguna terhadap destinasi wisata | Integer (0-5)   |

**Keterangan tipe data:**
- **Integer**: Angka bulat tanpa desimal.
- **Float**: Angka dengan desimal.
- **String**: Teks atau karakter.

**Catatan:**
- **Place_Id** pada dataset kedua berfungsi sebagai **foreign key** yang menghubungkan ke **Place_Id** pada dataset pertama (`tourism_with_id.csv`), memungkinkan integrasi data antara informasi destinasi wisata dan rating pengguna.
- **User_Id** memungkinkan analisis perilaku pengguna secara individual serta pengelompokan pengguna berdasarkan preferensi mereka.

# Data Preparation
Pada tahap **data preparation**, dilakukan serangkaian langkah untuk memastikan bahwa data yang digunakan dalam proses pemodelan bersih, terstruktur, dan siap untuk dianalisis. Tahapan ini mencakup pembersihan data, penggabungan dataset, pengolahan teks, encoding variabel kategorikal, normalisasi rating, dan pembagian data menjadi set pelatihan dan pengujian. Berikut adalah uraian mendetail mengenai setiap langkah yang dilakukan:
## Langkah-langkah yang Dilakukan:
### 1. Nilai yang Hilang (Missing Values) pada Dataset

Dataset ini memiliki beberapa kolom yang mengandung nilai yang hilang. Berikut adalah tabel yang menunjukkan jumlah nilai yang hilang pada setiap kolom untuk dua dataset yang berbeda: `tourism_with_id` dan `tourism_rating`.

| Kolom           | `tourism_with_id` | `tourism_rating` |
|-----------------|-------------------|------------------|
| **Place_Id**    | 0                 | 0                |
| **Place_Name**  | 0                 | 0                |
| **Description** | 0                 | 0                |
| **Category**    | 0                 | 0                |
| **City**        | 0                 | -                |
| **Price**       | 0                 | -                |
| **Rating**      | 0                 | 0                |
| **Time_Minutes**| 232               | -                |
| **Coordinate**  | 0                 | -                |
| **Lat**         | 0                 | -                |
| **Long**        | 0                 | -                |
| **Unnamed: 11** | 437               | -                |
| **Unnamed: 12** | 0                 | -                |

### Penjelasan:
- Kolom dengan nilai **0** menunjukkan bahwa tidak ada nilai yang hilang (missing).
- Kolom dengan nilai selain **0** menunjukkan jumlah nilai yang hilang dalam dataset.
- Dataset `tourism_with_id` memiliki nilai hilang pada kolom **Time_Minutes** (232 nilai hilang) dan **Unnamed: 11** (437 nilai hilang), yang kemungkinan besar tidak relevan atau tidak memiliki data penting.
- Dataset `tourism_rating` tidak memiliki nilai hilang di semua kolom.


### 2. Penghapusan Kolom dalam Dataset

Setelah dilakukan penghapusan kolom, dataset **`tourism_with_id`** hanya menyisakan lima kolom, yaitu:
- **Place_Id**: ID unik untuk setiap tempat wisata.
- **Place_Name**: Nama tempat wisata.
- **Description**: Deskripsi tempat wisata.
- **Category**: Kategori tempat wisata (misalnya, pantai, gunung, budaya, dll).
- **Rating**: Rating atau penilaian tempat wisata.

Beberapa kolom dihapus dari dataset untuk alasan-alasan berikut:
1. **Time_Minutes**: Kolom ini memiliki 232 nilai hilang, sehingga diputuskan untuk dihapus karena data yang hilang cukup banyak.
2. **Unnamed: 11**: Kolom ini berisi banyak nilai hilang (437 nilai hilang) dan kemungkinan besar tidak relevan untuk analisis lebih lanjut.
3. Kolom-kolom seperti **City**, **Price**, **Coordinate**, **Lat**, dan **Long** bisa saja dihapus karena data yang hilang atau kolom tersebut tidak memberikan kontribusi signifikan terhadap analisis.

Penghapusan kolom-kolom ini bertujuan untuk menyederhanakan dataset dan menghilangkan data yang tidak relevan atau tidak lengkap. Dengan demikian, dataset yang tersisa menjadi lebih efisien dan fokus pada kolom-kolom yang lebih relevan untuk analisis dan pemodelan.

### 3. Data Rekomendasi Setelah Penggabungan (Train Data)

Dataset ini merupakan data rekomendasi tempat wisata yang telah digabungkan dari dua sumber yaitu **tourism_with_id** dan **tourism_rating**. Data ini digunakan untuk memberikan rekomendasi tempat wisata berdasarkan rating dan kategori tempat wisata.

### 4. Struktur Data

Setelah penggabungan, dataset memiliki kolom-kolom sebagai berikut:

| **Kolom**       | **Deskripsi**                                                                                         |
|-----------------|-------------------------------------------------------------------------------------------------------|
| **Place_Id**    | ID unik untuk setiap tempat wisata. ID ini digunakan untuk mengidentifikasi tempat secara individual.  |
| **Place_Ratings**| Rata-rata rating yang diberikan oleh pengunjung untuk setiap tempat wisata. Nilai ini dihitung berdasarkan ulasan dari pengunjung. |
| **Place_Name**  | Nama tempat wisata yang terdaftar, seperti "Monumen Nasional" atau "Kota Tua".                       |
| **Description** | Deskripsi singkat mengenai tempat wisata, termasuk informasi tentang lokasi, sejarah, dan jenis atraksi yang ada. |
| **Category**    | Kategori atau jenis tempat wisata, seperti "Budaya", "Taman Hiburan", "Pantai", dll.                  |
| **Rating**      | Rating atau penilaian tempat wisata menurut sistem tertentu (misalnya rating bintang yang diberikan oleh pengguna). |

### 5. Contoh Data

Berikut adalah contoh data rekomendasi setelah penggabungan:

| **Place_Id** | **Place_Ratings** | **Place_Name**                         | **Description**                                                                                 | **Category**      | **Rating** |
|--------------|-------------------|----------------------------------------|-------------------------------------------------------------------------------------------------|-------------------|------------|
| 1            | 3.722222          | Monumen Nasional                       | Monumen Nasional atau yang populer disingkat di Monas, merupakan ikon kota Jakarta yang menjadi... | Budaya            | 4.6        |
| 2            | 2.840000          | Kota Tua                               | Kota tua di Jakarta, yang juga bernama Kota Tua, memiliki banyak bangunan bersejarah yang...     | Budaya            | 4.6        |
| 3            | 2.526316          | Dunia Fantasi                          | Dunia Fantasi atau disebut juga Dufan adalah taman hiburan yang terletak di kawasan Ancol...     | Taman Hiburan     | 4.6        |
| 4            | 2.857143          | Taman Mini Indonesia Indah (TMII)      | Taman Mini Indonesia Indah merupakan suatu kawasan wisata budaya yang memperkenalkan...         | Taman Hiburan     | 4.5        |
| 5            | 3.520000          | Atlantis Water Adventure               | Atlantis Water Adventure atau dikenal dengan Atlantis adalah taman rekreasi air yang terletak... | Taman Hiburan     | 4.5        |

### 6. Penjelasan Kolom
- **Place_Id**: ID unik untuk setiap tempat wisata yang memudahkan identifikasi tempat.
- **Place_Ratings**: Rating rata-rata yang dihitung berdasarkan ulasan dari pengunjung tempat wisata.
- **Place_Name**: Nama tempat wisata yang terdaftar dalam dataset.
- **Description**: Deskripsi singkat mengenai tempat wisata yang berisi informasi penting seperti lokasi dan atraksi utama.
- **Category**: Kategori tempat wisata, yang menggambarkan jenis atau kategori tempat (misalnya Budaya, Taman Hiburan, dll).
- **Rating**: Penilaian atau rating bintang yang diberikan oleh pengguna atau sistem berdasarkan kualitas tempat wisata.

### 7. Penggunaan Data
Dataset ini digunakan untuk merekomendasikan tempat wisata berdasarkan rating dan kategori. Data ini dapat digunakan dalam berbagai aplikasi atau analisis untuk memberikan rekomendasi kepada pengunjung berdasarkan tempat wisata dengan rating terbaik dalam kategori yang relevan.

---
### Mengisi Nilai Hilang pada Atribut Teks

Untuk memastikan konsistensi data teks, nilai yang hilang pada kolom `Description` dan `Category` diisi dengan string kosong.

```python
recommendation_data['Description'] = recommendation_data['Description'].fillna('')
recommendation_data['Category'] = recommendation_data['Category'].fillna('')
```

**Alasan:**  
Mengisi nilai yang hilang dengan string kosong memungkinkan proses preprocessing teks berjalan lancar tanpa mengganggu analisis selanjutnya.

### Pemeriksaan Duplikasi Data

Untuk menjaga integritas data, dilakukan pemeriksaan terhadap duplikasi pada atribut `Place_Name`.

```python
if recommendation_data['Place_Name'].duplicated().any():
```

**Hasil:**  
Tidak ditemukan data duplikasi

### Preprocessing Data untuk Content-based Filtering

**a. Penggabungan Deskripsi dan Kategori Menjadi Satu Teks**  

Untuk memanfaatkan informasi deskripsi dan kategori dalam sistem rekomendasi berbasis konten, kedua atribut ini digabungkan menjadi satu atribut baru bernama `Tags`.

```python
recommendation_data['Tags'] = recommendation_data['Description'] + ' ' + recommendation_data['Category']
```

**b. Pembersihan Teks (Preprocessing)**  

Teks pada atribut `Tags` dan `Description` dibersihkan dengan langkah-langkah berikut:

1. **Lowercasing:** Mengubah semua huruf menjadi huruf kecil untuk konsistensi.
2. **Stemming:** Mengurangi kata ke bentuk dasarnya menggunakan library Sastrawi.
3. **Stopword Removal:** Menghapus kata-kata yang tidak memiliki makna signifikan (stopwords).

```python
recommendation_data['Tags'] = recommendation_data['Tags'].apply(preprocessing)
recommendation_data['Description_Preprocessed'] = recommendation_data['Description'].apply(preprocessing)
```

**Isi Fungsi Preprocessing:**

```python
def preprocessing(text):
    text = text.lower()
    text = stemmer.stem(text)
    text = stopword.remove(text)
    return text
```

**Alasan:**
- **Lowercasing** dilakukan untuk menghindari duplikasi kata yang sama namun berbeda kapitalisasi.
- **Stemming dan Stopword Removal** dilakukan untuk mengurangi kompleksitas teks dan meningkatkan relevansi fitur teks yang digunakan dalam vektorisasi.

**c. Vektorisasi Teks dengan TF-IDF Vectorizer**  

Setelah pembersihan teks, teks diubah menjadi representasi numerik menggunakan `TfidfVectorizer`. Dua atribut vektorisasi dilakukan:

1. **Tags (Deskripsi + Kategori):**

   ```python
   vectors_tags = tv_tags.fit_transform(recommendation_data['Tags'])
   ```

2. **Description Saja:**

   ```python
   vectors_desc = tv_desc.fit_transform(recommendation_data['Description_Preprocessed'])
   ```

**Alasan:**  
**TF-IDF Vectorizer** dapat mengubah teks menjadi vektor numerik yang memungkinkan pengukuran kesamaan antar destinasi menggunakan metrik cosine similarity.

**d. Menghitung Cosine Similarity**

Untuk mengukur kesamaan antar destinasi wisata, dihitung cosine similarity berdasarkan vektor TF-IDF yang telah dibuat.

1. **Similarity Berdasarkan Tags:**

   ```python
   similarity_tags = cosine_similarity(vectors_tags, dense_output=False)
   ```

2. **Similarity Berdasarkan Description:**

   ```python
   similarity_desc = cosine_similarity(vectors_desc, dense_output=False)
   ```

**Alasan:**  
Metrik ini efektif untuk mengukur kesamaan antar dokumen teks, sehingga cocok untuk sistem rekomendasi berbasis konten.

### Persiapan Data untuk Collaborative Filtering

**a. Encoding User_Id dan Place_Id**

Untuk keperluan pemodelan, `User_Id` dan `Place_Id` diubah menjadi indeks numerik.

```python
user_ids = data_tourism_rating['User_Id'].unique().tolist()
place_ids = data_tourism_rating['Place_Id'].unique().tolist()

user_to_user_encoded = {x: i for i, x in enumerate(user_ids)}
user_encoded_to_user = {i: x for i, x in enumerate(user_ids)}
place_to_place_encoded = {x: i for i, x in enumerate(place_ids)}
place_encoded_to_place = {i: x for i, x in enumerate(place_ids)}

data_collab = data_tourism_rating.copy()
data_collab['user'] = data_collab['User_Id'].map(user_to_user_encoded)
data_collab['place'] = data_collab['Place_Id'].map(place_to_place_encoded)
```

**Alasan:**  
Mengubah ID ke indeks numerik memungkinkan penggunaan embedding layers dalam model neural network untuk Collaborative Filtering.

**b. Normalisasi Rating**

Rating yang diberikan oleh pengguna dinormalisasi agar berada dalam rentang antara 0 dan 1.

```python
min_rating = data_collab['Place_Ratings'].min()
max_rating = data_collab['Place_Ratings'].max()
data_collab['normalized_rating'] = data_collab['Place_Ratings'].apply(lambda x: (x - min_rating) / (max_rating - min_rating))
```

**Alasan:**
**Normalisasi** membantu model neural network dalam proses pelatihan dengan memastikan bahwa nilai input berada dalam rentang yang seragam, sehingga mempercepat konvergensi dan meningkatkan performa model.

**c. Membagi Data Menjadi Train dan Test**

Data dibagi menjadi data latih (train) dan data validasi (validation) untuk evaluasi model.

```python
train_size_cf = int(0.8 * len(data_collab))
x_train_cf = data_collab[['user', 'place']].values[:train_size_cf]
y_train_cf = data_collab['normalized_rating'].values[:train_size_cf]
x_val_cf = data_collab[['user', 'place']].values[train_size_cf:]
y_val_cf = data_collab['normalized_rating'].values[train_size_cf:]
```

**Alasan:**  
Memisahkan data menjadi train dan test memungkinkan evaluasi performa model pada data yang belum pernah dilihat selama pelatihan, sehingga memberikan indikasi yang lebih akurat mengenai generalisasi model.

# Modeling and Result

Pada tahap ini, dua sistem rekomendasi yang berbeda, yaitu **Content-based Filtering** dan **Collaborative Filtering** dibangun dan dievaluasi. Masing-masing pendekatan memiliki metode dan algoritma yang unik untuk memberikan rekomendasi destinasi wisata kepada pengguna.

### 1. Content-based Filtering

**Content-based Filtering** menggunakan informasi konten dari destinasi wisata, seperti deskripsi dan kategori, untuk memberikan rekomendasi yang relevan kepada pengguna. Pendekatan ini menganalisis kesamaan antara destinasi wisata berdasarkan atribut-atribut tersebut.

**Langkah-langkah:**

- Memastikan nama destinasi wisata (`place_name`) ada dalam data.
- Mendapatkan indeks destinasi wisata yang diminta. Indeks digunakan untuk mengakses matriks similarity.
- Menghitung skor kesamaan (similarity score) untuk destinasi tersebut. Proses ini menggunakan matriks similarity yang telah dihitung sebelumnya.
- Mengurutkan destinasi berdasarkan skor kesamaan.
- Menghindari merekomendasikan destinasi yang sama dengan yang diminta. Hal ini dilakukan dengan menghapus indeks destinasi tersebut dari daftar destinasi yang direkomendasikan.
- Mendapatkan top-n rekomendasi (misalnya top 10).

**Implementasi Kode:**

```python
def get_content_based_recommendations(place_name, data, similarity_matrix, top_n=10):
    """
    Mengembalikan rekomendasi Content-based Filtering untuk sebuah destinasi wisata.

    Parameters:
    - place_name (str): Nama destinasi wisata.
    - data (DataFrame): Dataset destinasi wisata (recommendation_data).
    - similarity_matrix (ndarray): Matriks similarity antar destinasi.
    - top_n (int): Jumlah rekomendasi yang diinginkan.

    Returns:
    - list: Daftar Place_Id yang direkomendasikan.
    """
    # Memastikan place_name ada dalam data
    if place_name not in data['Place_Name'].values:
        print(f"'{place_name}' TIDAK ditemukan dalam dataset.")
        return []

    # Mendapatkan indeks destinasi yang diminta
    place_idx = data[data['Place_Name'] == place_name].index[0]

    # Mendapatkan skor similarity untuk destinasi tersebut dan mengonversi ke dense array
    place_similarity = similarity_matrix[place_idx].toarray().flatten()

    # Mengurutkan destinasi berdasarkan similarity score
    similar_indices = place_similarity.argsort()[::-1]

    # Menghindari rekomendasi diri sendiri
    similar_indices = similar_indices[similar_indices != place_idx]

    # Mendapatkan top-n rekomendasi
    top_indices = similar_indices[:top_n]
    recommended_place_ids = data.iloc[top_indices]['Place_Id'].tolist()

    return recommended_place_ids
```

**Hasil Rekomendasi:**

Sebagai contoh, kami melakukan rekomendasi untuk destinasi wisata **'Wisata Alam Kalibiru'**.

| No. | Place Name                         | Category      | Rating | Description                                                                             |
|-----|------------------------------------|---------------|--------|-----------------------------------------------------------------------------------------|
| 1   | Watu Lumbung                       | Cagar Alam    | 4.3    | Letak Kampung Edukasi Watu Lumbung yang berada...                                        |
| 2   | Wisata Kaliurang                   | Cagar Alam    | 4.4    | Jogja selalu menarik untuk dikulik, terlebih t...                                        |
| 3   | Ciwangun Indah Camp Official       | Cagar Alam    | 4.3    | Ciwangun Indah Camp atau CIC adalah sebuah tem...                                        |
| 4   | Curug Cilengkrang                  | Cagar Alam    | 4.0    | Curug Cilengkrang bisa menjadi pilihan tujuan ...                                        |
| 5   | Happyfarm Ciwidey                  | Cagar Alam    | 4.2    | Objek wisata alam dan edukasi tengah banyak me...                                        |
| 6   | Hutan Wisata Tinjomoyo Semarang    | Cagar Alam    | 4.3    | Awalnya taman wisata hutan Tinjomoyo Semarang ...                                        |
| 7   | Umbul Sidomukti                    | Cagar Alam    | 4.6    | Kawasan wisata umbul Sidomukti merupakan salah...                                        |
| 8   | Wisata Alam Wana Wisata Penggaron  | Cagar Alam    | 4.1    | Berada sekitar 2 KM dari Kota Ungaran atau sek...                                        |
| 9   | Kampoeng Kopi Banaran              | Taman Hiburan | 4.3    | Kampoeng Kopi Banaran, sebuah agro wisata perk...                                       |
| 10  | Air Terjun Semirang                | Cagar Alam    | 4.4    | Terletak di lereng Gunung Ungaran bagian utara...                                       |

**Kelebihan dan Kekurangan:**

| **Kelebihan**                                                                 | **Kekurangan**                                                            |
|-------------------------------------------------------------------------------|---------------------------------------------------------------------------|
| Dapat memberikan rekomendasi yang spesifik berdasarkan preferensi konten.  | Memerlukan data deskripsi dan kategori yang cukup banyak dan berkualitas.      |
| Tidak memerlukan data interaksi pengguna (*rating*).                         | Rekomendasi terbatas pada konten yang sudah ada dalam dataset.         |
| Cocok untuk pengguna baru (*cold start*) yang belum memiliki interaksi.       | Rentan terhadap overfitting jika data konten tidak bervariasi.          |

### 2. Collaborative Filtering

**Collaborative Filtering** memanfaatkan data interaksi pengguna, seperti rating yang diberikan, untuk memberikan rekomendasi berdasarkan pola preferensi pengguna lain yang serupa. Pendekatan ini dapat menangkap preferensi yang lebih kompleks dan dinamis.

**Langkah-langkah:**

1. **Membangun Model Neural Network**  
   - Menggunakan embedding layers untuk pengguna dan destinasi wisata.
   - Mengombinasikan embedding dengan bias dan menghitung dot product.
   - Menggunakan *sigmoid activation* untuk output prediksi rating.

5. **Melatih Model**  
   Melatih model selama 20 epoch dengan batch size 32 menggunakan optimizer Adam.

6. **Fungsi Rekomendasi**  
   Membuat fungsi `recommend_by_collaborative_filtering` yang mengambil `User_Id` dan mengembalikan daftar rekomendasi berdasarkan prediksi rating tertinggi.

**Implementasi Kode:**

```python
class TourismRecNet(tf.keras.Model):
    def __init__(self, num_users, num_places, embedding_size, **kwargs):
        """
        Inisialisasi model Collaborative Filtering.

        Parameters:
        - num_users (int): Jumlah pengguna.
        - num_places (int): Jumlah tempat wisata.
        - embedding_size (int): Ukuran embedding untuk pengguna dan tempat.
        """
        super(TourismRecNet, self).__init__(**kwargs)
        self.user_embedding = layers.Embedding(num_users, embedding_size, embeddings_initializer='he_normal', embeddings_regularizer=keras.regularizers.l2(1e-6))
        self.user_bias = layers.Embedding(num_users, 1)
        self.place_embedding = layers.Embedding(num_places, embedding_size, embeddings_initializer='he_normal', embeddings_regularizer=keras.regularizers.l2(1e-6))
        self.place_bias = layers.Embedding(num_places, 1)

    def call(self, inputs):
        """
        Melakukan forward pass melalui model.

        Parameters:
        - inputs (tf.Tensor): Input tensor dengan shape (batch_size, 2)

        Returns:
        - tf.nn.sigmoid: Prediksi rating yang dihasilkan.
        """
        user_vector = self.user_embedding(inputs[:, 0])
        user_bias = self.user_bias(inputs[:, 0])
        place_vector = self.place_embedding(inputs[:, 1])
        place_bias = self.place_bias(inputs[:, 1])

        dot_product = tf.tensordot(user_vector, place_vector, 2)
        x = dot_product + user_bias + place_bias
        return tf.nn.sigmoid(x)

# Inisialisasi dan compile model dengan RMSE sebagai loss function
model = TourismRecNet(num_users, num_places, 50)
model.compile(
    loss=tf.keras.losses.MeanSquaredError(),
    optimizer=keras.optimizers.Adam(learning_rate=0.001),
    metrics=[tf.keras.metrics.RootMeanSquaredError()]
)

# Melatih model
history = model.fit(
    x=x_train_cf,
    y=y_train_cf,
    batch_size=32,
    epochs=20,
    validation_data=(x_val_cf, y_val_cf)
)
```

**Hasil Pelatihan Model:**

**Plot RMSE selama Pelatihan:**

![image](https://github.com/user-attachments/assets/d211a81f-d94a-4e17-8f65-1933fabb0ce0)
  

Grafik menunjukkan penurunan nilai RMSE pada data train dan validation seiring bertambahnya epoch, namun penurunannya tidaklah signifikan. Maka, epoch tambahan dirasa tidak diperlukan lagi.

**Hasil Rekomendasi:**

Sebagai contoh, kami melakukan rekomendasi untuk seorang pengguna dengan `User_Id` terpilih.

Rekomendasi berdasarkan Collaborative Filtering untuk User ID 206:

| No  | Nama Tempat                         | Kategori      | Rating | Deskripsi                                                                                      |
| --- | ------------------------------------ | ------------- | ------ | ------------------------------------------------------------------------------------------------ |
| 1   | Pantai Baron                         | Bahari        | 4.4    | Pantai Baron adalah salah satu objek wisata terbaik di Yogyakarta dengan pesona pantai yang indah. |
| 2   | Pintoe Langit Dahromo                | Cagar Alam    | 4.4    | Pintoe Langit Dahromo menawarkan pemandangan alam dari ketinggian yang spektakuler, cocok untuk fotografi. |
| 3   | Stone Garden Citatah                 | Taman Hiburan | 4.4    | Stone Garden di Citatah, Bandung, menawarkan formasi batu unik dan spot foto yang populer.         |
| 4   | Taman Lansia                          | Taman Hiburan | 4.4    | Taman Lansia adalah tempat santai yang nyaman, cocok untuk berlibur di akhir pekan.                |
| 5   | Teras Cikapundung BBWS               | Taman Hiburan | 4.3    | Teras Cikapundung Bandung adalah taman yang indah dengan aliran sungai, cocok untuk piknik dan bersantai. |
| 6   | Museum Barli                         | Budaya        | 4.4    | Museum Barli di Bandung didedikasikan untuk mengenang karya seni Barli Sasmitawinata.               |
| 7   | Museum Pos Indonesia                 | Budaya        | 4.5    | Museum Pos Indonesia menyimpan koleksi sejarah pos dan telekomunikasi dari masa Hindia Belanda.     |
| 8   | Curug Batu Templek                   | Cagar Alam    | 4.1    | Curug Batu Templek adalah air terjun yang terletak di Bandung, menawarkan pemandangan alam yang asri. |
| 9   | Masjid Agung Trans Studio Bandung    | Tempat Ibadah | 4.8    | Masjid Agung Trans Studio Bandung adalah masjid megah di kompleks Trans Studio, juga sebagai ikon arsitektur. |
| 10  | Curug Cilengkrang                    | Cagar Alam    | 4.0    | Curug Cilengkrang adalah air terjun di Bandung, cocok untuk menikmati keindahan alam dan suasana tenang. |



**Kelebihan dan Kekurangan**

| **Kelebihan**                                                                 | **Kekurangan**                                                            |
|-------------------------------------------------------------------------------|---------------------------------------------------------------------------|
| Dapat menangkap data yang lebih kompleks dan dinamis.                 | Memerlukan data interaksi pengguna yang cukup untuk model belajar.     |
| Mampu memberikan rekomendasi yang lebih personal dan relevan.               | Rentan terhadap masalah *sparsity* jika data interaksi pengguna rendah.   |
| Dapat mengidentifikasi pola tersembunyi dalam data interaksi pengguna.      | Tidak cocok untuk pengguna baru yang belum memiliki interaksi (*cold start*). |

## Evaluasi

Evaluasi merupakan langkah untuk menilai sejauh mana performa sistem rekomendasi yang telah dibangun memenuhi tujuan yang telah ditetapkan. Dalam proyek ini, terdapat tiga metrik evaluasi utama yaitu:
- **Precision** untuk menilai pendekatan sistem **Content-based Filtering**
- **Root Mean Squared Error (RMSE)** serta **Mean Absolute Error (MAE)** untuk menilai pendekatan sistem rekomendasi **Collaborative Filtering**.

### 1. Metrik Evaluasi yang Digunakan

**a. Precision**  
Precision mengukur proporsi rekomendasi yang relevan di antara 10 rekomendasi teratas yang diberikan kepada pengguna. Metrik ini memberikan indikasi seberapa akurat rekomendasi yang dihasilkan oleh sistem.

**Formula:**  
$$\text{Precision} = \frac{\text{TP}}{TP+FP}$$  

Di mana:
- $TP$ (True Positive) adalah jumlah rekomendasi yang relevan di antara 10 rekomendasi teratas,
- $FP$ (False Positive) adalah jumlah rekomendasi yang tidak relevan di antara 10 rekomendasi teratas.

**Interpretasi:**
- Jika nilai precision tinggi, artinya sebagian besar rekomendasi yang diberikan relevan dengan preferensi pengguna.
- Jika nilai precision rendah, artinya sebagian besar rekomendasi yang diberikan tidak relevan.

Dalam implementasi kode, alur untuk menentukan jumlah rekomendasi yang relevan (TP) dan kebalikannya (FP) adalah sebagai berikut:

**Langkah-langkah Algoritma:**

- Algoritma memilih satu atau beberapa destinasi wisata (`places_to_evaluate`) yang akan dievaluasi kualitas rekomendasinya.

- Untuk setiap destinasi wisata yang dipilih (`place`), algoritma menggunakan fungsi `get_content_based_recommendations` untuk menghasilkan daftar 10 rekomendasi destinasi wisata teratas (`recommended_place_ids`) berdasarkan kesamaan konten (deskripsi dan kategori).

- Dari daftar rekomendasi yang dihasilkan, algoritma mengambil informasi detail mengenai setiap destinasi wisata yang direkomendasikan, seperti `Place_Name`, `Category`, `Rating`, dan `Description_Preprocessed`.

- Algoritma menghitung kesamaan deskripsi (`Description_Similarity`) antara destinasi wisata input (`place`) dan setiap destinasi yang direkomendasikan menggunakan matriks kesamaan deskripsi (`similarity_desc`).

- **Menentukan Relevansi Rekomendasi**  
  Setiap rekomendasi dievaluasi apakah relevan atau tidak berdasarkan dua kriteria:
     - **Kecocokan Kategori:** Jika kategori destinasi wisata yang direkomendasikan sama dengan kategori destinasi input.
     - **Kesamaan Deskripsi:** Jika kesamaan deskripsi antara destinasi input dan destinasi yang direkomendasikan melebihi ambang batas tertentu (`desc_threshold`), misalnya 0.5.  

  Jika salah satu dari kedua kriteria tersebut terpenuhi, rekomendasi dianggap relevan (`Relevance = 1`), sebaliknya tidak relevan (`Relevance = 0`).

- **Menghitung True Positives (TP) dan False Positives (FP)**
   - **True Positives (TP):** Jumlah rekomendasi yang relevan (Relevance = 1) dari total 10 rekomendasi.
   - **False Positives (FP):** Jumlah rekomendasi yang tidak relevan, dihitung sebagai `top_n - TP`.

- **Menghitung Precision** dengan rumus yang telah disebutkan sebelumnya.

**b. Root Mean Squared Error (RMSE)**

RMSE mengukur rata-rata perbedaan antara nilai prediksi model dan nilai aktual yang diamati, dengan memberikan bobot lebih pada kesalahan yang lebih besar.

**Formula:**  
$$RMSE = \sqrt{\frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2}$$  
Di mana:
- $y_i$ adalah nilai aktual,
- $\hat{y}_i$ adalah nilai prediksi,
- $n$ adalah jumlah sampel.
- $i$ adalah indeks sampel.

**Interpretasi:**
- Jika nilai RMSE rendah, berarti prediksi model mendekati nilai aktual.
- Jika nilai RMSE tinggi, berarti ada kesalahan prediksi yang signifikan.

**c. Mean Absolute Error (MAE)**

MAE mengukur rata-rata kesalahan absolut antara nilai prediksi model dan nilai aktual yang diamati.

**Formula:**  
$$MAE = \frac{1}{n} \sum_{i=1}^{n} |y_i - \hat{y}_i|$$  

Di mana:
- $y_i$ adalah nilai aktual,
- $\hat{y}_i$ adalah nilai prediksi,
- $n$ adalah jumlah sampel.

**Interpretasi:**
- Jika nilai MAE rendah, artinya model memiliki akurasi prediksi yang baik.
- Jika nilai MAE tinggi, artinya terdapat kesalahan prediksi yang besar secara konsisten.

### 2. Hasil Evaluasi

**a. Evaluasi Precision untuk Content-based Filtering**

Evaluasi Precision berhasil dilakukan untuk beberapa destinasi wisata yang dipilih, misalnya **'Museum Fatahillah'**. Hasilnya adalah:

| Place               | Precision@10 |
|---------------------|--------------|
| Museum Fatahillah   | 100.00%      |

Precision sebesar **100.00%** menunjukkan bahwa 10 dari 10 rekomendasi yang diberikan kepada pengguna terkait **'Museum Fatahillah'** relevan. Artinya, performanya baik dalam memberikan rekomendasi yang sesuai berdasarkan konten deskripsi dan kategori.

**b. Evaluasi RMSE dan MAE untuk Collaborative Filtering**

Performa model Collaborative Filtering dievaluasi pada data validasi dengan menghitung nilai **RMSE** dan **MAE**. Hasilnya adalah:

```
Collaborative Filtering - RMSE: 0.3581
Collaborative Filtering - MAE: 0.3102
```

**Interpretasi:**
- Nilai RMSE sebesar **0.3581** menunjukkan bahwa rata-rata model memiliki performa yang cukup baik dalam memprediksi rating pengguna. Namun, masih terdapat ruang untuk meningkatkan akurasi prediksi melalui tuning hyperparameter dan/atau eksperimen dengan arsitektur model yang berbeda.
- Nilai MAE sebesar **0.3102** juga menunjukkan tingkat akurasi yang baik dalam prediksi rating, meskipun terdapat ruang untuk peningkatan.

### 3. Visualisasi Hasil Evaluasi

**Evaluasi RMSE dan MAE untuk Collaborative Filtering**

![image](https://github.com/user-attachments/assets/e04e1fac-9ce6-4447-aeba-0664c780a9ed)

**Interpretasi:**  
Grafik batang menunjukkan nilai RMSE dan MAE yang cukup rendah, menandakan bahwa model Collaborative Filtering mampu memprediksi rating dengan baik.

### 4. Evaluasi Keseluruhan

Hasil evaluasi terhadap dua model rekomendasi, yaitu **Content-based Filtering** dan **Collaborative Filtering**, menunjukkan bahwa kedua model telah mencapai hasil yang sesuai dengan tujuan yang diharapkan.

Model **Content-based Filtering** berhasil memberikan rekomendasi yang sangat relevan untuk data **Museum Fatahillah**, dengan tingkat **Precision** mencapai **100.00%**. Ini menunjukkan bahwa sebagian besar rekomendasi yang diberikan sesuai dengan preferensi pengguna berdasarkan konten deskripsi dan kategori destinasi wisata. Dengan demikian, masalah pertama mengenai penyediaan rekomendasi yang relevan berdasarkan konten deskripsi dan kategori telah teratasi dengan baik. Model ini efektif memanfaatkan informasi tekstual untuk menemukan kesamaan antar destinasi, sehingga mampu memberikan rekomendasi yang tepat sasaran.

Sementara itu, model **Collaborative Filtering** menunjukkan kinerja yang cukup memadai, dengan nilai **RMSE** sebesar **0.3581** dan **MAE** sebesar **0.3102**. Walaupun hasilnya cukup baik, masih ada ruang untuk perbaikan agar model ini lebih mendekati prediksi yang sempurna. Namun, model ini berhasil menangkap pola preferensi pengguna berdasarkan data rating yang diberikan, yang menyelesaikan masalah kedua terkait pemanfaatan data rating untuk meningkatkan akurasi rekomendasi. Dengan menganalisis pola interaksi pengguna, model ini dapat memberikan rekomendasi yang lebih personal dan relevan, meskipun masih perlu optimasi lebih lanjut untuk meningkatkan akurasinya.

Evaluasi menggunakan metrik **Precision**, **RMSE**, dan **MAE** memastikan bahwa sistem rekomendasi tidak hanya relevan, tetapi juga akurat dalam memenuhi kebutuhan pengguna. Precision menunjukkan bahwa rekomendasi dari model Content-based Filtering cukup relevan, sementara RMSE dan MAE pada model Collaborative Filtering mengindikasikan bahwa prediksi rating pengguna cukup akurat. Dengan demikian, sistem rekomendasi ini telah berhasil mengukur relevansi dan akurasi, serta memenuhi tujuan ketiga dalam mengukur performa sistem rekomendasi dengan metrik evaluasi yang sesuai.

Secara keseluruhan, kedua model rekomendasi—**Content-based Filtering** dan **Collaborative Filtering**—telah berhasil menyelesaikan masalah yang diidentifikasi dan mencapai tujuan yang diharapkan. Model Content-based Filtering memberikan rekomendasi yang spesifik dan relevan berdasarkan konten, sementara Collaborative Filtering berhasil memprediksi preferensi pengguna dengan menganalisis pola interaksi. Dampak positif dari kedua model ini terhadap bisnis sangat terasa, karena keduanya meningkatkan kemampuan sistem dalam memberikan rekomendasi yang akurat dan relevan, yang pada akhirnya berpotensi meningkatkan kepuasan dan loyalitas pengguna. Oleh karena itu, solusi yang diimplementasikan dalam proyek ini telah memberikan kontribusi positif terhadap pemahaman bisnis, memenuhi kebutuhan pengguna, dan mencapai tujuan yang diinginkan.

### 5. Saran

Ada beberapa aspek yang masih dapat diperbaiki. Untuk model Collaborative Filtering, optimasi lebih lanjut, seperti penyetelan hyperparameter atau eksplorasi arsitektur model yang berbeda, dapat dilakukan untuk meningkatkan akurasinya. Selain itu, menggabungkan kedua pendekatan ini dalam sebuah **Hybrid Approach** dapat memberikan keuntungan tambahan dengan menggabungkan keunggulan masing-masing metode, sehingga menghasilkan rekomendasi yang lebih akurat dan relevan secara keseluruhan.

## Referensi

1. Badan Pusat Statistik. (2020). *Jumlah Wisatawan Mancanegara ke Indonesia Tahun 2019*. Diambil dari [https://www.bps.go.id/id/pressrelease/2020/02/03/1711/jumlah-kunjungan-wisman-ke-indonesia-desember-2019-mencapai-1-38-juta-kunjungan-.html](https://www.bps.go.id/id/pressrelease/2020/02/03/1711/jumlah-kunjungan-wisman-ke-indonesia-desember-2019-mencapai-1-38-juta-kunjungan-.html)
2. World Tourism Organization. (2021). *Impact Assessment of the COVID-19 Outbreak on International Tourism*. Diambil dari [https://www.unwto.org/impact-assessment-of-the-covid-19-outbreak-on-international-tourism](https://www.unwto.org/impact-assessment-of-the-covid-19-outbreak-on-international-tourism)
3. Rezkia, S. M., & Wibowo, B. S. (2024). Pengembangan Sistem Rekomendasi Rencana Perjalanan: Literature Review. *Prosiding Seminar Nasional Teknik Industri (SeNTI)*.
4. Overbeek, M. V., & Naatonis, R. N. (2019). Sistem Rekomendasi Destinasi Wisata Di Kota Kupang Dengan Metode Weighted Product. *HOAQ (High Education of Organization Archive Quality): Jurnal Teknologi Informasi*, 10(1), 30-34.
5. Murzani, F. F., & Arianto, D. B. (2023). Implementasi Metode Collaborative Filtering pada Algoritma Sistem Rekomendasi Destinasi Wisata di Aceh. *Jurnal Komputer, Informasi Teknologi, dan Elektro*, 8(3).
6. Ricci, F., Rokach, L., & Shapira, B. (2015). *Recommender Systems Handbook*. Springer.
7. Robertson, S. (2004). Understanding inverse document frequency: on theoretical arguments for IDF. *Journal of Documentation*, 60(5), 503-520. https://doi.org/10.1108/00220410410560582
8. Zhang, Y., Chen, C., & Wang, X. (2020). *Fast Adaptation for Cold-Start Collaborative Filtering with Meta-Learning*. Di dalam *2020 IEEE International Conference on Data Mining (ICDM)* (hal. 661-670). Sorrento, Italia: IEEE. https://doi.org/10.1109/ICDM50108.2020.00075







