# Laporan Proyek Machine Learning - Nabila Alawiyah
## Project Overview
Musik digital kini menjadi media utama konsumsi hiburan global melalui platform streaming seperti Spotify, yang mendominasi distribusi dan akses musik. Popularitas lagu di platform ini sangat menentukan kesuksesan komersial artis dan industri musik secara keseluruhan.

Faktor yang memengaruhi popularitas tidak hanya berasal dari preferensi pendengar, tetapi juga terkait dengan karakteristik audio lagu seperti tempo, loudness, dan valence, yang memengaruhi respon emosional pendengar (Celma & Herrera, 2008). Selain itu, algoritma rekomendasi yang diterapkan platform berperan besar dalam meningkatkan visibilitas lagu (Schedl, 2017).

Dataset Spotify yang berisi fitur akustik dan data popularitas lagu dari berbagai negara memberikan kesempatan untuk menganalisis hubungan antara atribut lagu dan tingkat kepopulerannya, sehingga membantu strategi produksi dan pemasaran musik yang lebih efektif.

## Business Understanding
### Problem Statements
Rumusan masalah dari masalah latar belakang diatas adalah:
1. Siapa saja top 10 penyanyi di Indonesia?
2. Faktor apa saja yang memengaruhi popularitas lagu?
2. Bagaimana cara membuat sistem rekomendasi terbaik yang dapat diimplementasikan?

### Goals
Berdasarkan problem statements, berikut tujuan yang ingin dicapai pada proyek ini.
1. Mengetahui siapa saja top 10 penyanyi di Indonesia?
2. Mengetahui faktor apa saja yang memengaruhi popularitas lagu
3. Mengetahui cara membuat sistem rekomendasi terbaik yang dapat diimplementasikan.

### Solution Approach
Untuk mencapai goals tersebut, solution statements yang diusulkan adalah:
1. Melakukan analisis dengan memfilter data berdasarkan negara Indonesia. Artis diekstrak dari kolom artists dan dipisah per individu, kemudian dihitung rata-rata popularitas serta jumlah lagu Top 50 untuk masing-masing artis. Selanjutnya, dibuat skor gabungan dari kedua metrik tersebut, dan diambil 10 artis dengan skor tertinggi.
2. Menganalisis hubungan antar fitur untuk mengukur kekuatan kontribusi tiap faktor terhadap popularitas lagu, sehingga faktor-faktor utama yang paling berpengaruh dapat diidentifikasi.
3. Menggunakan algoritma cosine similarity untuk membangun sistem rekomendasi, kemudian mengevaluasi performanya guna menjamin keakuratan sistem rekomendasi tersebut.

## Data Understanding
Data yang digunakan untuk membuat sistem rekomendasi musik diambil dari platform open source Kaggle dan dipublikasikan oleh asaniczka, [Top Spotify Songs in 73 Countries (Daily Updated)](https://www.kaggle.com/datasets/asaniczka/top-spotify-songs-in-73-countries-daily-updated)

Dataset ini terdiri atas 2.049.119 baris dan 25 kolom. Terdiri atas 7 kolom yang bertipe data object, 8 kolom bertipe data int64, 9 kolom bertipe data float64, dan 1 kolom bertipe data boolean. Setiap kolom memiliki arti sebagai berikut.

| Variabel           | Tipe Data | Keterangan                                                                 |
|--------------------|-----------|---------------------------------------------------------------------------|
| `spotify_id`       | object    | ID unik untuk lagu dalam database Spotify.                                |
| `name`             | object    | Judul lagu.                                                               |
| `artists`          | object    | Nama artis yang membawakan lagu. |
| `daily_rank`       | integer   | Peringkat harian lagu di daftar Top 50.                                  |
| `daily_movement`   | integer   | Perubahan peringkat dibandingkan dengan hari sebelumnya.                 |
| `weekly_movement`  | integer   | Perubahan peringkat dibandingkan dengan minggu sebelumnya.               |
| `country`          | object    | Kode ISO negara asal playlist Top 50. Jika bernilai Null, berarti berasal dari playlist Global Top 50. |
| `snapshot_date`    | object    | Tanggal pengambilan data dari API Spotify.                               |
| `popularity`       | integer   | Skor popularitas lagu di Spotify saat ini (0–100).                       |
| `is_explicit`      | bool      | Menunjukkan apakah lagu mengandung lirik eksplisit (TRUE or FALSE).  |
| `duration_ms`      | integer   | Durasi lagu dalam satuan milidetik.                                      |
| `album_name`       | object    | Nama album tempat lagu tersebut dirilis.                                 |
| `album_release_date` | object  | Tanggal rilis album.                                                     |
| `danceability`     | float     | Seberapa cocok lagu untuk menari (0.0 – 1.0).                            |
| `energy`           | float     | Tingkat intensitas dan aktivitas lagu (0.0 – 1.0).                       |
| `key`              | integer   | Kunci musikal lagu, direpresentasikan sebagai angka 0–11.                |
| `loudness`         | float     | Tingkat volume rata-rata lagu dalam desibel (bernilai negatif).|
| `mode`             | integer   | Modus lagu: 1 untuk mayor, 0 untuk minor.                                |
| `speechiness`      | float     | Indikator seberapa banyak kata-kata yang terdapat dalam lagu.           |
| `acousticness`     | float     | Kemungkinan bahwa lagu bersifat akustik (0.0 – 1.0).                     |
| `instrumentalness` | float     | Perkiraan seberapa besar lagu bersifat instrumental (tanpa vokal).      |
| `liveness`         | float     | Estimasi apakah lagu direkam secara live (0.0 – 1.0).                    |
| `valence`          | float     | Menggambarkan mood lagu: bahagia (1.0) hingga sedih (0.0).               |
| `tempo`            | float     | Kecepatan lagu dalam satuan BPM (beats per minute).                      |
| `time_signature`   | integer   | Birama lagu, misalnya 4 untuk 4/4.                                       |
### Exploratory Data Analysis
#### 1. Analysis Popularitas Berdasarkan Variable is_explicit
<p align="center">
  <img src="https://github.com/user-attachments/assets/e812c45a-8eef-4491-8216-cc5e3d15d171" width="600"/>
</p>

  > **Insight:**
  > Grafik di atas membandingkan rata-rata popularitas lagu eksplisit dan non-eksplisit. Lagu eksplisit sedikit lebih populer, tapi perbedaannya tidak signifikan, menunjukkan label eksplisit tidak berpengaruh besar pada popularitas lagu.

#### 2. Analysis Popularitas Berdasarkan Negara asal playlist Top 50
<p align="center">
  <img src="https://github.com/user-attachments/assets/493ce204-e4f8-4f95-b6fa-619da81c2bff" width="1000"/>
</p>

  > **Insight:**
  > * Top tiga negara dengan popularitas tertinggi adalah Australia (AU), New Zealand (NZ), dan Canada (CA)
  >  * Top tiga negara dengan popilaritas terendah adalah Islandia (IS), Egypt (EG), dan Bulgaria (BG)

#### 3. Analysis Hubungan Popularitas Berdasarkan Fitur Lagu
<p align="center">
  <img src="https://github.com/user-attachments/assets/be4fabec-2c93-4847-8914-ccd08b2e6519" width="1000"/>
</p>

  > **Insight:**
  > 1. Hasil korelasi menunjukkan bahwa tidak ada hubungan yang kuat antara popularitas dan fitur-fitur tersebut, karena nilai korelasinya sangat rendah.
  > 2. Terdapat korelasi positif yang cukup kuat (0.43) antara danceability dan valence, yang berarti bahwa lagu yang enak untuk berdansa biasanya juga terasa lebih ceria atau menyenangkan. Sebaliknya, fitur acousticness berkorelasi negatif dengan danceability (-0.25) dan valence (-0.16), menunjukkan bahwa lagu yang lebih akustik cenderung terdengar lebih tenang dan kurang cocok untuk berdansa.
  >
  > **Kesimpulan:** Fitur-fitur seperti danceability, acousticness, dan instrumentalness memang saling berhubungan, tetapi tidak berpengaruh besar terhadap popularitas lagu.

#### 4. Top 10 Artist di Indonesia
<p align="center">
  <img src="https://github.com/user-attachments/assets/e83c3c80-8608-45f4-84a8-2bba3a8f3f51" width="1000"/>
</p>

  > **Insight:**
  > * Bernadya jadi artis paling populer di Spotify Indonesia (skor: 521.5) berkat kombinasi popularitas tinggi dan jumlah lagu terbanyak (1556 lagu).
  > * Juicy Luicy dan Hindia menyusul di posisi 2 dan 3, menunjukkan konsistensi rilisan dan basis pendengar yang kuat.
  > * Lyodra dan Feby Putri punya popularitas rata-rata tertinggi (79.17 & 78.51) meski jumlah lagu lebih sedikit. Menandakan kualitas lagu yang tinggi.
  > * Tulus tetap masuk top 5 meskipun produktivitasnya lebih rendah dari yang lain. Menunjukkan lagu-lagunya tahan lama dan terus diputar.

### Data Quality Verification
#### Memeriksa Kesesuaian tipe data dan Menentukan prioritas kolom
<p align="center">
  <img src="https://github.com/user-attachments/assets/aa76bef6-49bf-4f5c-9dcb-5812f8a8d2ce" width="600"/>
</p>

> **Insight:**
> * Untuk melakukan content-based filtering, dapat dilakukan penghapusan beberapa kolom yaitu: daily_rank, daily_movement, weekly_movement, country, snapshot_date, album_name, album_release_date, key, mode, time_signature.
> * Melakukan konversi pada kolom duration_ms ke menit.
> * Kolom loudness bernilai negatif, sehingga perlu dilakukan standarisasi
> * Kolom is_explicit diubah ke dalam numerik (TRUE : 1, dan FALSE : 0)
> * Perlu dilakukan standarisasi pada numerical feature

#### Memeriksa Data duplikat
<p align="center">
  <img src="https://github.com/user-attachments/assets/39b7272d-57d8-41fc-a12c-aefaeb3f0664" width="1000"/>
</p>

> **Insight:**
>
> * Kolom country memiliki banyak nilai null (28.058), yang berarti sebagian besar data berasal dari playlist Global Top 50.
> * Kolom name dan artists memiliki sedikit nilai null (sekitar 29-30), yang harus diperhatikan karena penting untuk analisis lagu dan artis.
> * Kolom album_name dan album_release_date juga memiliki cukup banyak nilai null (822 dan 659), bisa jadi karena beberapa lagu belum ada data album lengkapnya.
> * Sebagian besar kolom fitur audio dan metadata lainnya tidak memiliki nilai null, jadi data cukup lengkap di aspek teknis.

#### Memeriksa Outliers dengan IQR Method
##### 1. Menampilkan analisis statistik
<p align="center">
  <img src="https://github.com/user-attachments/assets/80627d43-7d0e-428e-946d-517bc24d2676" width="1000"/>
</p>

  > Fungsi `describe()` memberikan informasi statistik pada masing-masing kolom, antara lain:
  > - `Count` adalah jumlah sampel pada data.
  > - `Mean` adalah nilai rata-rata.
  > - `Std` adalah standar deviasi.
  > - `Min` yaitu nilai minimum setiap kolom.
  > - `25%` adalah kuartil pertama. Kuartil adalah nilai yang menandai batas interval dalam empat bagian sebaran yang sama.
  > - `50%` adalah kuartil kedua, atau biasa juga disebut median (nilai tengah).
  > -` 75%` adalah kuartil ketiga.
  > - `Max` adalah nilai maksimum.

##### 2. Mengecek data outlier dan memvisualisasikannya
<p align="center">
  <img src="https://github.com/user-attachments/assets/6ec16bdc-d915-4a57-9ffe-1dfc8d8b4784" width="1000"/>
</p>

  > Program di atas akan menampilkan hasil visualisasi sebagai berikut:

<p align="center">
  <img src="https://github.com/user-attachments/assets/350ea572-4cfd-4c34-8d9e-31586eabd778" width="1000"/>
</p>

> **Interpretasi Boxplot:**
> 1. Daily Rank & Popularity.
   Sebagian besar lagu memiliki ranking harian dan tingkat popularitas yang cukup merata. Tidak terdapat outlier yang signifikan pada kedua kolom ini.
> 2. Daily Movement & Weekly Movement.
   Nilai perpindahan peringkat harian dan mingguan sebagian besar berada di sekitar nol, namun terdapat beberapa outlier yang menandakan lagu dengan perubahan peringkat yang sangat drastis.
> 3. Duration (duration\_ms). 
   Durasi lagu umumnya berada antara 150.000 hingga 250.000 milidetik (2,5–4 menit), dengan beberapa outlier berdurasi sangat pendek maupun panjang.
> 4. Danceability, Energy, & Valence. 
   Sebagian besar lagu memiliki nilai danceability, energy, dan valence yang tinggi (di atas 0.5), menandakan lagu-lagu cenderung mudah dinikmati, enerjik, dan ceria.
> 5. Key & Mode. 
   Penyebaran nada dasar (`key`) cukup merata dari 0 sampai 11. Sementara itu, `mode` cenderung seimbang antara mayor dan minor.
> 6. Speechiness. 
   Terdapat banyak outlier pada fitur ini, menandakan adanya sejumlah lagu dengan unsur lirik berbicara yang tinggi, seperti rap atau spoken word.
> 7. Acousticness. 
   Mayoritas lagu memiliki tingkat akustik yang rendah hingga sedang. Terdapat outlier untuk lagu yang sangat akustik.
> 8. Instrumentalness. 
   Sebagian besar lagu memiliki nilai instrumentalness sangat rendah, menunjukkan dominasi lagu dengan vokal. Beberapa outlier mengindikasikan lagu instrumental penuh.
> 9. Liveness 
   Mayoritas lagu direkam di studio dengan sedikit unsur live, meskipun ada beberapa outlier yang menunjukkan kemungkinan lagu live performance.
> 10. Loudness & Tempo  
    Nilai loudness menunjukkan lagu umumnya cukup keras, dengan rentang antara -8 hingga -4 dB. Tempo lagu bervariasi, umumnya berada antara 90–150 BPM, dengan beberapa outlier bertempo sangat tinggi.
>
> **Kesimpulan:**
> Sebagian besar fitur numerik memiliki distribusi data yang wajar dan tidak ekstrem. Beberapa fitur seperti `speechiness`, `instrumentalness`, dan `tempo` mengandung outlier, namun hal ini bisa mencerminkan keragaman jenis lagu (misalnya lagu instrumental, rap, atau lagu tempo cepat). Oleh karena itu, outlier tidak akan dihapus karena tetap relevan dan mencerminkan variasi alami dalam musik.


 
## Data Preparation


**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan proses data preparation yang dilakukan
- Menjelaskan alasan mengapa diperlukan tahapan data preparation tersebut.

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
