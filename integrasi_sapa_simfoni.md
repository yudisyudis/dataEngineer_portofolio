# Integrasi data antara layanan **Sahabat Perempuan dan Anak (SAPA 129)** dengan aplikasi **Sistem Informasi Online Perlindungan Perempuan dan Anak (SIMFONI PPA)**

## Latar Belakang

KemenPPPA memiliki dua program yang pada awalnya berdiri sendiri-sendiri yang pertama yaitu layanan SAPA 129 yang merupakan portal penerimaan pengaduan kekerasan perempuan dan anak, yang kedua adalah aplikasi Simfoni PPA yang berisikan berbagai macam info grafik berkenaan dengan data-data kekerasan terhadap perempuan dan anak. SAPA 129 bersifat terpusat, seluruh data pengaduan yang masuk diarahkan ke pusat lalau dikumpulkan dalam sebuah report file berbentuk CSV, sementara sumber data yang divisualisasikan di aplikasi Simfoni PPA berasal dari input yang dilakukan operator di tiap-tiap dinas PPPA provinsi di seluruh Indonesia. Kedua data di dua program tersebut memiliki format yang tentu berbeda-beda satu sama lain

Saya diamanahi untuk merancang alur integrasi yang memungkinkan data SAPA 129 dapat ditransformasikan hingga formatnya sesuai dengan format data Simfoni PPA, lalu kemudian mentabulasikan data SAPA 129 hingga kemudian dapat digabungkan dan divisualisasikan di aplikasi Simfoni PPA.

## Tools yang digunakan

<img src="https://user-images.githubusercontent.com/91902011/208005371-159d0258-957c-4700-b580-8adc63014a05.png" width="150">

* Pentaho Data Integration:

    Pentaho digunakan dalam merancang ETL untuk transformasi data SAPA 129 agar dapat menyesuaikan dengan format data Simfoni PPA 
    
<img src="https://www.kursuswebsite.org/wp-content/uploads/2017/03/postgresql-logo.png" width="150">

* PostgreSQL:

    SQL digunakan sebagai query untuk merekap dan mentabulasikan data SAPA 129 sehingga siap diakumulasikan dengan data Simfoni PPA lalu divisualisasikan. Database management system PostgreSQL digunakan sebagai tempat Data Warehouse Simfoni berada.
    
## Flowchart

![Untitled Diagram drawio(3)](https://user-images.githubusercontent.com/91902011/208010199-dab69319-70b9-4ae3-82be-ded8d1757782.png)
