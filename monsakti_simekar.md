# Pengembangan data pipeline dari API aplikasi Monitoring Sistem Informasi Keuangan Tingkat Instansi (Monsakti) ke data warehouse aplikasi Sistem Manajemen Kinerja (Simekar) KemenPPPA.

## Latar Belakang

Kementerian Keuangan mengembangkan sebuah aplikasi untuk memonitor informasi keuangan tiap-tiap kementerian atau lembaga negara yang disebut dengan Monsakti. Tiap Kementerian dapat mengakses Monsakti melalui API yang telah disediakan dengan menambahkan informasi identitas kode Kementerian terkait. API yang disediakan telah dilengkapi dengan bearer token sebagai metode autentikasi, tiap URL API hendak diakses, perlu memasukkan token default terlebih dahulu sebelum kemudian token asli tergenerate secara otomatis dan berlaku untuk akses modul yang sama.

Saya diberi amanah untuk mengelola seluruh API Monsakti untuk Kementerian Pemberdayaan Perempuan dan Perlindungan Anak (KPPPA) yang totalnya terdiri dari 6 modul dan 29 sub modul. Seluruh sub modul akan dibuatkan pipeline yang akan mengambil dan memasukkan data ke database Monspan, lalu 3 sub modul spesifik yaitu Realisasi Anggaran, Capaian Program, dan Pagu Anggaran akan lanjut dikirimkan ke data warehouse aplikasi Simekar.

## Tools yang digunakan

<img src="https://user-images.githubusercontent.com/91902011/208005371-159d0258-957c-4700-b580-8adc63014a05.png" width="150">

* Pentaho Data Integration:

    Digunakan untuk membuat ETL dengan fitur yang memungkinkan mengambil data dari API untuk kemudian diarahkan ke database

<img src="https://user-images.githubusercontent.com/91902011/208488789-5b4fd278-13c1-4f11-ac69-bdd095fe3011.png" width="150">

* Postman:
    
    Digunakan untuk mengetes cara akses API, input/output apa yang diperlukan untuk mengakses data yang tersedia di API tersebut.

## Flowchart
