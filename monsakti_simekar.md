# Pengembangan data pipeline dari API aplikasi Monitoring Sistem Informasi Keuangan Tingkat Instansi (Monsakti) ke data warehouse aplikasi Sistem Manajemen Kinerja (Simekar) KemenPPPA.

## Latar Belakang

Kementerian Keuangan mengembangkan sebuah aplikasi untuk memonitor informasi keuangan tiap-tiap kementerian atau lembaga negara yang disebut dengan Monsakti. Tiap Kementerian dapat mengakses Monsakti melalui API yang telah disediakan dengan menambahkan informasi identitas kode Kementerian terkait. API yang disediakan telah dilengkapi dengan bearer token sebagai metode autentikasi, tiap link API hendak diakses, perlu memasukkan token default terlebih dahulu sebelum kemudian token asli tergenerate secara otomatis dan berlaku untuk akses modul yang sama.

Saya diberi amanah untuk mengelola seluruh API Monsakti untuk Kementerian Pemberdayaan Perempuan dan Perlindungan Anak (KPPPA) yang totalnya terdiri dari 6 modul dan 29 sub modul. Seluruh sub modul akan dibuatkan pipeline yang akan mengambil dan memasukkan data ke database Monspan, lalu 3 sub modul spesifik yaitu Realisasi Anggaran, Capaian Program, dan Pagu Anggaran akan lanjut dikirimkan ke data warehouse aplikasi Simekar.

## Tools yang digunakan

<img src="https://user-images.githubusercontent.com/91902011/208005371-159d0258-957c-4700-b580-8adc63014a05.png" width="150">

* Pentaho Data Integration:

    Digunakan untuk membuat ETL dengan fitur yang memungkinkan mengambil data dari API untuk kemudian diarahkan ke database

<img src="https://user-images.githubusercontent.com/91902011/208488789-5b4fd278-13c1-4f11-ac69-bdd095fe3011.png" width="150">

* Postman:
    
    Digunakan untuk mengetes cara akses API, input/output apa yang diperlukan untuk mengakses data yang tersedia di API tersebut.

## Flowchart

![monsakti drawio(3)](https://user-images.githubusercontent.com/91902011/208561181-3a99a6ff-558a-49bf-ac06-7a3bfd3b6407.png)

## Penjelasan

### Persiapan

Tim Monsakti menyediakan API credensial yang berguna untuk mengakses tiap-tiap modul yang diperlukan, diantaranya:

1. API endpoint tiap modul
2. Reset Token

API endpoint terdiri dari alamat host monsakti, kemudian dilengkapi dengan kode kementerian, kode satuan kerja, serta kode kelompok modul dan sub modul yang diinginlkan. Sementara reset token digunakan sebagai autentikasi awal untuk mengenerate token sebenarnya.

### Proses ETL

#### Mengambil data dari API

![image](https://user-images.githubusercontent.com/91902011/208579085-cd73cc10-47a0-4cb4-ac7f-52d44cd6d645.png)

1. Tahap awal dari ETL yang dibuat adalah mengenerate row yang berisikan API endpoint, beserta reset token yang berfungsi sebagai bearer autentikasi API Monsakti. 
2. Row yang telah digenerate kemudian dijadikan parameter untuk step ETL selanjutnya yaitu HTTP Post yang kemudian menghasilkan output berupa token asli untuk mengakses data di API Monsakti.
3. Modified JavaScript digunakan untuk menyiapkan token asli yang dihasilkan pada step sebelumnya untuk menjadi variabel yang diperlukan untuk mengakses data API
4. API Endpoint dan Token asli kemudian dijadikan parameter di step REST Client untuk kemudian data yang diperlukan dapat terambil seluruhnya.

#### Mengelola data yang diperoleh

![image](https://user-images.githubusercontent.com/91902011/208580231-5f6af808-09ba-4ca2-b33b-aabee2515f68.png)

1. Data yang dihasilkan kemudian dipilih mana mana saja yang hendak diolah lebih lanjut menggunakan step select value
2. Data yang dihasilkan berbentuk JSON, lalu perlu untuk masuk ke step JSON input untuk ditransformasikan menjadi berbentuk Table
3. Ditambahkan kolom timestamp melalui step Get System Info

    Data hasil HTTP post yang terdiri dari endpoint dan token asli (token final):

    <img width="500" alt="hasil2" src="https://user-images.githubusercontent.com/91902011/208610485-ee292afd-92ce-425d-a54f-3411961d03b3.png">

    Data hasil dari JSON input:

    <img width="500" alt="hasil1" src="https://user-images.githubusercontent.com/91902011/208609893-b71dc0a1-68cd-4b75-86ba-97ccbf04f0c3.png">

#### Insert Database Monspan

![image](https://user-images.githubusercontent.com/91902011/208580708-ec76d215-721e-4756-a604-7c6d6a975e3c.png)

Data yang telah siap kemudian dimasukkan ke dalam database Monspan, dengan dua opsi insert yaitu dengan metode truncate table atau dengan update table.

#### Transfer data dari Database Monspan ke Database Simekar

![image](https://user-images.githubusercontent.com/91902011/208580919-ddfc3e85-ae45-4fbb-932a-c76183060a46.png)

Dari seluruh modul dan sub modul yang telah dibuatkan ETL dan disimpan dama Database Monspan, 3 diantaranya yaitu Capaian Program, Realisasi Anggaran, dan Pagu Anggaran ditransfer lagi ke Database Simekar untuk kemudian dikelola lebih lanjut oleh operator Simekar.

#### Otomatisasi ETL

![image](https://user-images.githubusercontent.com/91902011/208581274-106f899b-b83a-4072-92c9-757aba0c9944.png)

Setelah seluruh proses selesai, dibuatkan Job yang mampu terus menerus menjalankan ETL secara berkala. Pada job ini diatur ETL akan berjalan tiap hari setiap pukul 2 pagi hari. Sehingga database Simekar akan senantiasa terupdate secara harian.



