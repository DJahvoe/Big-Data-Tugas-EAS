# Electric Production

![workflow](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/workflow.jpg)<br>

## _Business Understanding_

### Kemungkinan proses yang dapat dilakukan dari data

- Mengetahui sebaran data produksi device elektronik.
- Melakukan analisis serta klasifikasi pada runtutan waktu (time series) data produksi device elektronik berdasarkan IP Index.

## _Data Understanding_

![dataset](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/dataset.jpg)<br>

- dataset merupakan data produksi device yang disajikan dalam IP Index
- jumlah data yang digunakan adalah sebanyak **397 baris**
- **DATE**: merupakan **tanggal dan waktu** saat dilakukan pengambilan data.
- **IPG2211A2N**: merupakan index yang digunakan untuk menentukan **banyaknya gas/electric device** yang telah berhasil dimanufaktur pada bulan tertentu.

## _Data Preparation_

![data_prep0](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/data_prep0.jpg)<br>

1. **Setup environment dan membaca data** <br>
   ![data_prep1](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/data_prep1.jpg)<br>

   - Membaca data menggunakan **CSV Reader** <br>
     ![csv_reader](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/csv_reader.jpg)<br>
   - Membuat Big Data Environment menggunakan **Create Local Big Data Environment** <br>
     ![create_local_big_data_env](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/create_local_big_data_env.jpg)<br>

2. **Melakukan load data** <br>
   ![data_prep2](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/data_prep2.jpg)<br>
   Isi dari metanode **Load Data**: <br>
   ![metanode_load_data](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/metanode_load_data.jpg)<br>

   - Membuat koneksi database serta tabel **05111740000130_electric_production** menggunakan **DB Table Creator**.<br>
     ![db_table_creator](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/db_table_creator.jpg)<br>
   - Load data tabel **05111740000130_electric_production** dari database menggunakan **DB Loader**.<br>
     ![db_loader](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/db_loader.jpg)<br>
   - Hasil dari query hive dapat dibaca pada Spark Dataframe dengan mengkonversi menggunakan **Hive to Spark**.

3. **Ekstraksi atribut date-time** <br>
   ![data_prep3](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/data_prep3.jpg)<br>
   Isi dari metanode **Extract date-time attributes**:<br>
   ![metanode_extract_date_time](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/metanode_extract_date_time.jpg)<br>
   Mengeksekusi query menggunakan **Spark SQL Query**

   - Mengkonversi datetime dengan query sebagai berikut:<br>
     ![sql_initial_datetime](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/sql_initial_datetime.jpg)<br>
     Hasil dari Query: <br>
     ![sql_initial_datetime_result](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/sql_initial_datetime_result.jpg)<br>
   - Mengekstrasi detail waktu dari **eventDate**: <br>
     ![sql_extract_new](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/sql_extract_new.jpg)<br>
     Hasil dari Query: <br>
     ![sql_extract_new_result](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/sql_extract_new_result.jpg)<br>
   - Mengklasifikan bulan tiap quarter (4 bulan): <br>
     ![sql_quarter_year](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/sql_quarter_year.jpg)<br>
     Hasil dari Query: <br>
     ![sql_quarter_year_result](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/sql_quarter_year_result.jpg)<br>
   - Mengklasifikasi data bulan **genap** dan **ganjil**: <br>
     ![sql_odd_even_month](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/sql_odd_even_month.jpg)<br>
     Hasil dari Query: <br>
     ![sql_odd_even_month_result](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/sql_odd_even_month_result.jpg)<br>
   - Mengklasifikasi data minggu **genap** dan **ganjil**: <br>
     ![sql_odd_even_week](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/sql_odd_even_week.jpg)<br>
     Hasil dari Query: <br>
     ![sql_odd_even_week_result](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/sql_odd_even_week_result.jpg)<br>

4) **Fungsi agregasi dan time series** <br>
   ![data_prep4](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/data_prep4.jpg)<br>
   Isi dari metanode **Aggregations and time series**<br>
   ![metanode_aggregation](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/Aggregation/metanode_aggregation.jpg)<br>

   - Sebelum dimasukkan ke dalam fungsi agregasi, data akan melewati **Persist Spark DataFrame/RDD** untuk mempercepat akses karena adanya caching<br>
   - Beberapa fungsi agregasi dan time series:
     - Mencari rata-rata produksi device tahunan<br>
       ![1](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/Aggregation/1.jpg)<br>
       ![1_1](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/Aggregation/1_1.jpg)<br>
       penggunaan fungsi agregasi **MEAN** untuk kolom **IPG2211A2N** dengan pengelompokkan berdasarkan **year**<br>
     - Mencari rata-rata produksi device per 3 bulanan<br>
       ![2](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/Aggregation/2.jpg)<br>
       ![2_1](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/Aggregation/2_1.jpg)<br>
       penggunaan fungsi agregasi **MEAN** untuk kolom **IPG2211A2N** dengan pengelompokkan berdasarkan **year** serta pivot terletak pada **quarterYear**<br>
     - Mencari rata-rata produksi device bulan genap dan ganjil<br>
       ![3](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/Aggregation/3.jpg)<br>
       ![3_1](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/Aggregation/3_1.jpg)<br>
       penggunaan fungsi agregasi **MEAN** untuk kolom **IPG2211A2N** dengan pengelompokkan berdasarkan **year** serta pivot terletak pada **evenOddMonth**<br>
     - Mencari rata-rata produksi device minggu genap dan ganjil<br>
       ![4](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/Aggregation/4.jpg)<br>
       ![4_1](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/Aggregation/4_1.jpg)<br>
       penggunaan fungsi agregasi **MEAN** untuk kolom **IPG2211A2N** dengan pengelompokkan berdasarkan **year** serta pivot terletak pada **evenOddWeek**<br>
   - Setelah tiap proses fungsi agregasi dieksekusi, dilakukan rename dengan menggunakan **Spark Column Rename** sesuai kebutuhan.<br>
   - Setelah berhasil di-rename, dilakukan penggabungan hasil agregasi menjadi 1 tabel menggunakan **Spark Joiner**<br>
     ![joiner](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/Aggregation/joiner.jpg)<br>

5) **Menghapus baris yang memiliki data yang tidak lengkap** <br>
   ![data_prep5](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/data_prep5.jpg)<br>
   eksekusi dilakukan dengan menggunakan **Spark Missing Value**<br>

6) **Menghitung persentase perbandingan antara tiap rata-rata kolom dengan rata-rata tahunan**
   ![data_prep6](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/data_prep6.jpg)<br>
   ![percentage](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/percentage.jpg)<br>
   eksekusi dilakukan dengan menggunakan **Spark SQL Query**<br>

7. **Hasil dari data preparation didapatkan dengan kolom tabel sebagai berikut**<br>
   ![data_prep_end](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/data_prep_end.jpg)<br>

8. **Output dari SQL Query menjadi input dari metanode _PCA, K-means, Scatter Plot_**<br>
   ![data_prep_output](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/data_prep_output.jpg)<br>
   dengan node-nodenya sebagai berikut<br>
   ![metanode_pca_kmeans_scatter](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/metanode_pca_kmeans_scatter.jpg)<br>

## _Modelling_

![modelling](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/modelling.jpg)<br>

### Proses modelling:

1. Data yang telah diinput akan dinormalisasi terlebih dahulu dengan **nilai minimal 0** dan **nilai maksimal 1** menggunakan **Spark Normalizer**.
2. **Spark PCA** akan mengurangi dimensi dari data yang diinput, serta **Spark k-Means** akan membuat cluster dengan algoritma **k-Means**<br>
   ![pca](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/pca.jpg)<br>
   ![kmean](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/kmean.jpg)<br>
3. Data pada **Spark k-Means** selanjutnya akan di-filter menggunakan **Spark Column Filter** untuk mendapatkan kolom **year** dan hasil **Cluster** saja.<br>
   ![col_filter](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/col_filter.jpg)<br>
4. Data tabel hasil eksekusi dari **Spark PCA** dan **Spark k-Means** yang telah di-filter selanjutnya digabungkan dengan menggunakan **Spark Joiner**
5. Agar dapat dibaca menggunakan node KNIME, Data harus dikonversi dari Spark Dataframe menjadi Table menggunakan **Spark to Table**
6. Hasil konversi kemudian dikembalikan nilai rangenya menggunakan **Denormalizer (PMML)**.
7. Didapatkan data tabel dari hasil tahap modelling sebagai berikut<br>
   ![denorm](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/denorm.jpg)<br>

## _Evaluation_

![evaluation](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/evaluation.jpg)<br>
Model dievaluasi menggunakan **Scatter Plot** dan **Table View** dengan cara

1. Data hasil proses modelling selanjutnya dikonversi menjadi string menggunakan **Number To String** agar mudah untuk melakukan plotting
2. Dilakukan assignment warna pada **Color Manager** untuk menampilkan cluster dengan jelas<br>
   ![colormanager](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/colormanager.jpg)<br>
3. Tampilan yang didapatkan pada **Scatter Plot**<br>
   ![scatterplot](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/scatterplot.jpg)<br>
4. Tampilan yang didapatkan pada **Table View**<br>
   ![tableview](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/tableview.jpg)<br>

Kesimpulan:<br>
Dari visualisasi yang didapatkan melalui **Scatter Plot** bisa dilihat bahwa cluster1 (biru) memiliki hubungan korelasi yang lebih dekat dengan cluster2 (hijau) dibandingkan dengan cluster0 (merah), sedangkan cluster2 (hijau) memiliki korelasi yang seimbang antara kedua cluster.

## _Deployment_

![deployment](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/deployment.jpg)<br>
![spark_to_parquet](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/spark_to_parquet.jpg)<br>
![spark_to_hive](https://github.com/DJahvoe/Big-Data-Tugas-EAS/blob/master/screenshot/Electric/spark_to_hive.jpg)<br>
Deployment dapat dilakukan dengan menggunakan **Spark to Parquet** untuk deploy dalam bentuk file parquet ataupun menggunakan **Spark to Hive** apabila ingin menyimpan hasil dalam bentuk hive table.
