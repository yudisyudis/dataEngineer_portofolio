# Membangun ETL Untuk Datasource Visualisasi SIGA

## Latar Belakang

Terdapat salah satu aplikasi Web milik Kementerian Pemberdayaan Perempuan dan Pelindungan Anak yang bernama Sistem Informasi Gender dan Anak (SIGA). Salah satu fitur di SIGA adalah adanya berbagai macam visualisasi dari sejumlah database yang dimiliki Kementerian yang mana data tersebut berhubungan dengan Anak dan Gender. Visualisasi dibuat dengan Tableau dan sumber datanya berasal dari database MS SQL.

Saya diamanahkan untuk membuat ETL yang berfungsi secara otomatis menghubungkan file excel yang berisi update data ke dalam database yang menjadi sumber data untuk visualisasi di aplikasi SIGA. 

## Tools yang digunakan

<img src="https://user-images.githubusercontent.com/91902011/208600647-7b2d84c4-2623-459e-b8ad-d6ba79162c21.png" width="150">

* Pentaho Data Integration:
  Digunakan untuk membuat ETL yang mampu secara berkala mengirimkan file yang berisi data untuk update database di MS SQL

<img src="https://user-images.githubusercontent.com/91902011/208601162-0cdf2917-aee3-470c-8dd4-ca5e474abe73.png" width="150">

* Tableau Desktop dan Server:
  Aplikasi visualisasi data Tableau digunakan untuk membuat berbagai visualisasi yang diperlukan, dan mampu secara langsung terhubung dengan data source dan mampu pula responsif terhadap perubahan data source. Tableau server membuat dashboard yang dibuat mampu dideploy di dalam aplikasi yang diinginkan

## Flowchart

![siga drawio(2)](https://user-images.githubusercontent.com/91902011/208608653-8e7787ee-7181-457f-9fa8-770daaf4ba9e.png)

## Penjelasan

### File Update Data

File disediakan oleh tim stastisi, dalam bentuk excel. Perlu kesepakatan untuk konsistensi penamaan file, penamaan sheet, penamaan serta posisi kolom dan row, agar senantiasa terbaca oleh ETL yang akan dibangun.

### Proses ETL

#### Input dan Validasi Data

![image](https://user-images.githubusercontent.com/91902011/208603823-26f2bc95-5adf-4546-996f-f65ffb5a2aed.png)

Step Input dan Validasi Data pada ETL. Input diatur berupa file excel, dengan menspesifikasikan nama dan lokasi file, serta nama sheet, kolom, dan row mana yang hendak diambil datanya. Selanjutnya masuk ke step Validator, yang berfungsi membuang data-data yang kolom pentingnya seperti data kota atau data tahun bernilai null.

#### Penyetaraan ejaan nama Kota

![image](https://user-images.githubusercontent.com/91902011/208604395-9ff21699-cc6f-4190-906f-031f59ba5496.png)

Menggunakan step fuzzy match, dilakukan screening nama-nama kota pada file input lalu menyamakan ejaan tiap-tiap nama Kota dengan apa yang terdapat di database, agar dua kota yang sama tidak terbaca sebagai dua kota yang berbeda hanya karena berbeda ejaan atau berbeda huruf kapital. Lalu data hasil match tersebut pada step Select diambil dan menggantikan kolom nama kota yang lama.

#### Lookup Wilayah dan Tahun

![image](https://user-images.githubusercontent.com/91902011/208605062-5761c1a6-a2cd-48cc-a499-cdbbe6c334ed.png)

Langkah berikutnya adalah melakukan lookup data nama Kota dan Tahun, lalu merubahnya menjadi kode wilayah dan kode tahun sesuai dengan apa yang ada di database, contoh hasilnya sebagai berikut:

![image](https://user-images.githubusercontent.com/91902011/208605992-b1e7d5d5-bed5-4f32-8128-ac9a98951a6d.png)

#### Insert/update Database

![image](https://user-images.githubusercontent.com/91902011/208606104-67983821-55f7-4e73-b834-5ec242d751dd.png)

Hasil akhir dari ETL kemudian dimasuukan ke dalam database MS SQL, dengan metode update agar data sebelumnya tidak hilang seperti ketika menggunakan metode truncate.

### Pembuatan Visualisasi di Tableau Desktop

![image](https://user-images.githubusercontent.com/91902011/208606876-b59f3ea3-35a6-4fd4-8f9e-2d5bffb45c71.png)

Data source berasal dari database di MS SQL yang merupakan muara dari ETL yang dibuat sebelumnya, sifat data source tableau dibuat menjadi Extract, lalu dipublish ke Tableau Server dengan pengaturan refresh berkala setiap harinya pukul 4 pagi.

![image](https://user-images.githubusercontent.com/91902011/208607369-ef94e4cb-5204-4a72-8cd7-c471f17b4dc3.png)

### Deploy Visualisasi di Aplikasi SIGA

![image](https://user-images.githubusercontent.com/91902011/208607921-e6f38816-9a8c-4488-b0f6-889568c0f92a.png)

Visualisasi yang telah dideploy di Tableau Server kemudian di embeded di aplikasi web SIGA. Visualisasi tersebut akan secara responsif berubah tiap kali terdapat perubahan data yang ETL-nya telah berjalan otomatis berkala tiap harinya.




