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

## Penjelasan

### Input Data

<img width="1000" alt="data1" src="https://user-images.githubusercontent.com/91902011/208219984-08a698a8-e332-476e-8d42-136de78275c3.png">

<img width="1000" alt="data2" src="https://user-images.githubusercontent.com/91902011/208219997-3be3ff0c-7764-42c0-9244-301e082187b6.png">

<img width="1000" alt="data3" src="https://user-images.githubusercontent.com/91902011/208220007-5ae98f05-2c61-47ef-97e3-d44eceb4a88e.png">

<img width="1000" alt="data4" src="https://user-images.githubusercontent.com/91902011/208220015-15d7f429-b55b-44fa-9623-317de3c7e30b.png">

Input data dari layanan SAPA 129 terdiri dari 64 kolom dan 1045 baris data untuk tahun 2021. Dari sekian banyak data kemudian ditransformasikan mengikuti format yang diterima oleh aplikasi Simfoni PPA yang diantaranya memerlukan perubahan nama kolom, format data, normalisasi data, dan lain sebagainya. Data dalam contoh gambar diatas disamarkan untuk menjaga privasi korban.

### Proses ETL

Alur ETL yang dirancang terdiri dari berbagai fitur yang dimanfaatkan sesuai dengan kebutuhan, diantara jenis-jenis fitur yang digunakan adalah:

#### Input Data, Seleksi Data, Penambahan Data yang tidak tersedia
![image](https://user-images.githubusercontent.com/91902011/208221610-b286971b-bd82-4725-8f7a-b22badb9e500.png)

* Input data dengan format CSV
* Memfilter data yang tidak ada isinya
* Menambahkan data-data default yang diperlukan Simfoni tapi tidak terdapat di data input seperti tahun input, instansi penginput, dan sebagainya

#### Pemanfaatan fitur modified JavaScript
![image](https://user-images.githubusercontent.com/91902011/208221725-e1d6faa0-d055-4ffd-afe1-6d01f364f430.png)

JavaScript digunakan untuk berbagai fungsi diantaranya:

* Mengenerate data berdasarkan data baru dengan fungsi berdasarkan data yang telah tersedia
* Memperbaiki salah ejaan pada beberapa field
* Mapping Kode Provinsi sesuai data input Provinsi
* Mengenerate kelompok data (usia)
* Mengelompokkan jenis kekerasan, jenis layanan, dan kategori korban
* Mengenerate tanggal perkiraan bagi data dengan data tanggal yang kosong

#### Lookup data dengan database
![image](https://user-images.githubusercontent.com/91902011/208221945-3f2653f4-7d42-45e9-85b8-e712a20320e5.png)

Database Lookup digunakan untuk melakukan penyetaraan data dengan data master yang bersumber dari database, langkah ini penting karena perbedaan sifat data antara SAPA yang merupakan data mentah hasil isian operator penerima pengaduan, dengan Simfoni yang tipe datanya sudah teratur dan tidak bisa menerima sembarang jenis data. Data SAPA yang banyak tidak beraturan harus diformat sedemikian rupa sehingga sesuai dengan format Simfoni.

#### Row Normaliser pada beberapa jenis data
![image](https://user-images.githubusercontent.com/91902011/208251301-6ccfdb61-215d-4a89-a21a-f76a146ce23d.png)

Jenis Kekerasan dan Jenis Layanan merupakan contoh dari bagian yang dapat diisi lebih dari satu nilai. Pada SAPA, nilai kedua dan seterusnya ditulis dalam kolom yang berbeda, sedangkan di Simfoni nilai kedua dan seterusnya ditulis dalam row yang berbeda di satu kolom yang sama. Kondisi demikian mengharuskan data input SAPA pada bagian tersebut dinormalisasikan sehingga kolom isian dikemas menjadi hanya satu kolom, dengan penambahan row jika nilai lebih dari satu.

#### Insert hasil ke Database
![image](https://user-images.githubusercontent.com/91902011/208251456-77ac2f84-3c78-44f4-aaa4-e7f40705e5a1.png)

Hasil akhir dari data yang telah melewati proses ETL kemudian dimasukkan ke dalam fitur select_final yang berfungsi mengecek nama label dan urutan kolom agar benar-benar sesuai dengan simfoni, setelahnya data yang telah sesuai tersebut dimasukkan ke dalam beberapa database yang diperlukan yaitu Database lokal sebagai backup, database server yang bertindak sebagai Data Warehouse Simfoni, dan backup lainnya dalam bentuk Excel.

### Rekap Query SQL

Data hasil ETL kemudian direkap menggunakan SQL untuk memperoleh data tabulasi yang siap untuk diagregasikan dengan data Simfoni lalu divisualisasikan bersama. Rekap dilakukan menggunakan PostgeSQL dengan memanfaatkan database management bawaannya yaitu pgAdmin. Beberapa fitur SQL yang digunakan diantaranya:

#### Common Table Expression

```
with 

jumkasus as (
select a.kode_prov_kasus,a.tahun_input,a.bulan_input, 
count(DISTINCT kasus_id)as jumlah_kasus
from hasil_sapa_perempuan_rev a
group by a.kode_prov_kasus,a.tahun_input,a.bulan_input
),

jumkorban as (
select a.kode_prov_kasus,a.tahun_input,a.bulan_input, 
count(DISTINCT korban_id)as jumlah_korban
from hasil_sapa_perempuan_rev a
group by a.kode_prov_kasus,a.tahun_input,a.bulan_input
),

jumtindak as (
select a.kode_prov_kasus,a.tahun_input,a.bulan_input,
case when jenis_kekerasan='Fisik' then count(DISTINCT korban_id)end as k_fisik, 
case when jenis_kekerasan='Psikis' then count(DISTINCT korban_id)end as k_psikis, 
case when jenis_kekerasan='Seksual' then count(DISTINCT korban_id)end as k_seksual, 
case when jenis_kekerasan='Trafficking' then count(DISTINCT korban_id)end as k_trafficking, 
case when jenis_kekerasan='Penelantaran' then count(DISTINCT korban_id)end as k_penelantaran, 
case when jenis_kekerasan='Eksploitasi' then count(DISTINCT korban_id)end as k_eksploitasi, 
case when jenis_kekerasan='Lainnya' then count(DISTINCT korban_id)end as k_lainnya
from hasil_sapa_perempuan_rev a
group by a.kode_prov_kasus,a.tahun_input,a.bulan_input,jenis_kekerasan
),
```

Dasar dari rekapan SQL yang dilakukan adalah dengan memanfaatkan fitur CTE agar rekap per field yang diperlukan menjadi lebih fleksibel, dengan membuat pengelompokan pada masing masing field untuk kemudian dapat dilakukan rekapan terpisah melalui CTE yang dibuat. CTE dasar ini sebagaimana dapat dilihat di gambar di atas berisi perintah untuk mengkonversi input tiap opsi jawaban pada tiap-tiap kolom menjadi angka, kemudian dilakukan groupping berdasarkan kode provinsi, tahun dan bulan input, serta field masing-masing. 

Setelah seluruh data dikonversi menjadi numerik, kemudian digunakan kembali fitur CTE untuk rekap tahap kedua yang berfungsi untuk mengagregasi data-data input yang telah bersifat numerik hasil dari CTE tahap awal sebelumnya seperti contoh query berikut:
```
rekap_kasus as(
select kode_prov_kasus,tahun_input,bulan_input
,sum(jumlah_kasus)jumlah_kasus
from jumkasus
GROUP BY kode_prov_kasus,tahun_input,bulan_input),

rekap_korban as(
select kode_prov_kasus,tahun_input,bulan_input
,sum(jumlah_korban)jumlah_korban
from jumkorban
GROUP BY kode_prov_kasus,tahun_input,bulan_input),

rekap_tindak as(
select kode_prov_kasus,tahun_input,bulan_input
,sum(k_fisik)k_fisik
,sum(k_psikis)k_psikis
,sum(k_seksual)k_seksual
,sum(k_trafficking)k_trafficking
,sum(k_penelantaran)k_penelantaran
,sum(k_eksploitasi)k_eksploitasi
,sum(k_lainnya)k_lainnya
from jumtindak  
GROUP BY kode_prov_kasus,tahun_input,bulan_input),
```

Data yang telah diagregasi setelah melewati dua tahap CTE, lalu dilakukan join dengan master data pada kolom-kolom yang perlu untuk dipetakan seperti kode provinsi, tahun dan bulan input, dengan contoh query SQL sebagai berikut:
```
from wil aa
join rekap_kasus a on aa.kode::text=a.kode_prov_kasus
left join rekap_korban b on aa.kode::text=b.kode_prov_kasus and a.tahun_input=b.tahun_input and a.bulan_input=b.bulan_input
left join rekap_tindak c on aa.kode::text=c.kode_prov_kasus and a.tahun_input=c.tahun_input and a.bulan_input=c.bulan_input
left join rekap_layanan d on aa.kode::text=d.kode_prov_kasus and a.tahun_input=d.tahun_input and a.bulan_input=d.bulan_input
left join rekap_usia e on aa.kode::text=e.kode_prov_kasus and a.tahun_input=e.tahun_input and a.bulan_input=e.bulan_input
left join rekap_pendidikan f on aa.kode::text=f.kode_prov_kasus and a.tahun_input=f.tahun_input and a.bulan_input=f.bulan_input
left join rekap_tempat g on aa.kode::text=g.kode_prov_kasus and a.tahun_input=g.tahun_input and a.bulan_input=g.bulan_input
left join rekap_pekerjaan h on aa.kode::text=h.kode_prov_kasus and a.tahun_input=h.tahun_input and a.bulan_input=h.bulan_input
left join rekap_status i on aa.kode::text=i.kode_prov_kasus and a.tahun_input=i.tahun_input and a.bulan_input=i.bulan_input
left join rekap_hubungan j on aa.kode::text=j.kode_prov_kasus and a.tahun_input=j.tahun_input and a.bulan_input=j.bulan_input

order by kode,bulan_input
```

Setelah melalui berbagai tahapan yang dijelaskan, maka data telah siap untuk digabungkan dengan Simfoni PPA
