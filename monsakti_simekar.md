# Pengembangan data pipeline dari API aplikasi Monitoring Sistem Informasi Keuangan Tingkat Instansi (Monsakti) ke data warehouse aplikasi Sistem Manajemen Kinerja (Simekar) KemenPPPA.

## Latar Belakang

Kementerian Keuangan mengembangkan sebuah aplikasi untuk memonitor informasi keuangan tiap-tiap kementerian atau lembaga negara yang disebut dengan Monsakti. Tiap Kementerian dapat mengakses Monsakti melalui API yang telah disediakan dengan menambahkan informasi identitas kode Kementerian terkait. API yang disediakan telah dilengkapi dengan bearer token sebagai metode autentikasi, tiap URL API hendak diakses, perlu memasukkan token default terlebih dahulu sebelum kemudian token asli tergenerate secara otomatis dan berlaku untuk akses modul yang sama.

Saya diberi amanah untuk mengelola seluruh API Monsakti untuk Kementerian Pemberdayaan Perempuan dan Perlindungan Anak (KPPPA) yang totalnya terdiri dari 6 modul dan 29 sub modul. Seluruh sub modul akan dibuatkan pipeline yang akan mengambil dan memasukkan data ke database Monspan, lalu 3 sub modul spesifik yaitu Realisasi Anggaran, Capaian Program, dan Pagu Anggaran akan lanjut dikirimkan ke data warehouse aplikasi Simekar.
