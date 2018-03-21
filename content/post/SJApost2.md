---
title: "Progress so far..."
date: 2018-03-21T12:02:34+07:00
author: Stefanus James Aditya
tags: ["Stefanus", "Sprint 1", "Android Studio", "Firebase", "XML"]
---

Halo semua! Kembali lagi ke blog post oleh saya, Stefanus, pada postingan kali ini saya ingin menceritakan perjalanan pengembangan aplikasi Abeona sejak postingan blog pertama saya.

## Old Task

Selama 1 minggu dari postingan blog pertama saya, saya masih mengorek-orek tentang penggunaan Firebase sebagai tools untuk otenfikasi registrasi dan login akun untuk aplikasi ini. Task saya ini agak mengalami gangguan karena satu dan beberapa hal. Yang pertama adalah untuk mencoba keberhasilan penggunaan Firebase dibutuhkan view, tetapi task tersebut belum ada yang mengambil sehingga saya harus membuat sendiri di luar issue dari task yang ada. Masalah kedua adalah OnClickButtonListener sepertinya tidak berjalan, karena saya sudah mencoba mengisi text field email dan password, dan saat button sign-up ditekan, tidak terjadi apa-apa. Masalah ini masih coba saya teliti sampai sekarang. Masalah terakhir adalah karena wajibnya penggunaan TDD, unit testing untuk firebase masih saya pertanyakan, karena firebase sendiri adalah tool dari luar aplikasi, dimana aplikasi kita harus melakukan koneksi ke firebasenya sendiri dan fungsi yang memanggil koneksi tersebut tidak me-*return* nilai apapun (void), sehingga saya bingung membuat unit testnya. Solusi untuk masalah ini adalah studi lebih lanjut tentang unit test untuk fungsi seperti ini, saya sendiri sudah mendapatkan bacaan mengenai hal ini.

Source code untuk Firebase sekarang ini bisa diakses di [sini](https://gitlab.com/PPL2018csui/Kelas-D/PPL2018-D2/tree/us01_register/android/app/src/main/java/ga/abeona/abeona)

## Lecture & Learning

Pada minggu ke-6, di pertemuan pertama pada hari Rabu terdapat kuliah tamu lagi, kali ini pembahasannya adalah tentang clean code, awalnya ditanya apakah clean code itu? Di presentasi disebutkan sebuah gambar bahwa clean code adalah jumlah kata *WTF* yang keluar per menit saat membaca seseorang membaca code(Ha!), tetapi sebenarnya clean code mencakup juga tentang comment pada code, error handling, tidak hanya penamaan variabel atau fungsi atau kode yang mudah dimengerti saja. Pada pertemuan kedua, sayangnya saya tidak masuk jadinya saya tidak bisa sedikit menceritakan pengalaman saya.

## Diskusi dengan Product Owner

Beberapa hari sebelum Jumat minggu ke-6, saya membuat janji dengan Product Owner aplikasi kami, Kak Ace, untuk bertemu dengan *development team* pada hari Jumat minggu ke-6. Pada pertemuan ini kami membahas tentang progress pengembangan aplikasi Abeona dan meminta saran Kak Ace dalam pengembangan aplikasi ini. Kak Ace agak bingung dengan sprint planning dan MVP*(Minimum Viable Product)* untuk sprint 1 kami ini, karena menurutnya untuk sprint 1 ini fitur register dan login masih belum diperlukan untuk MVP kami yang hanya dapat melihat list product dan product detail serta lokasi product. Kami kurang mengerti juga awalnya karena hanya mengikuti petunjuk scrum master yaitu memperhatikan bobot untuk pemilihan backlog item untuk sprint ini, tetapi akhirnya kami belajar lebih lanjut dari Kak Ace tentang scrum ini. Diskusi dengan Kak Ace ini berjalan dengan lancar dan baik, dimana kami mempelajari lebih dalam tentang pengembangan aplikasi dan Kak Ace dapat lebih mengerti tujuan dari sprint 1 ini. Untuk saya sendiri akhirnya, task Firebase di *postpone* karena masalah yang sudah disebutkan di atas.

## New Task

Pada minggu ke-6 akhir scrum meeting hari Kamis, saya mendapatkan task baru, yaitu [issue #28](https://gitlab.com/PPL2018csui/Kelas-D/PPL2018-D2/issues/28) untuk membuat view untuk product page list. Progress saya dalam task ini sudah cukup lancar, biarpun view yang saya buat tidak secantik dengan mockup yang sudah dibuat oleh desainer tim kami. Implementasi belum selesai sepenuhnya dan unit test belum selesai juga.

![ProductListPageView](/img/ProdListPage.JPG)

## More Learning

Dari sekarang ini saya sudah mencoba untuk melakukan TDD.
TDD sendiri setelah saya melakukan studi literatur kembali, yaitu memang untuk membuat test yang pasti akan gagal karena implementasi yang tidak ada. Jadi kita tidak perlu takut untuk code yang gagal pada awalnya. Yang penting kita membuat *unit test* untuk menjelaskan kode program nantinya, lalu kita ulang tahap ini berkali-kali sampai akhirnya lolos *unit test*.

#### Todo
- Mempercantik view
- Melengkapi implementasi
- Menyelesaikan test