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

![siga drawio](https://user-images.githubusercontent.com/91902011/208602722-e88fc1ce-460f-4d36-8ddc-fcfdc8962913.png)
