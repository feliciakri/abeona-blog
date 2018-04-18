---
title: "Vincent Individual Review 4"
date: 2018-04-18T16:20:07+07:00
author: Vincent
tags: ["Vincent", Sprint 2", "Week 11"]
---

Pada sprint 2 minggu ketiga, penulis akan membahas mengenai *Software Environment*, *Error Code dan Exception Handling* dan *Software Architecture*.

## Software Environment
Pada perkuliahan PPL, software environmentnya terbagi menjadi 3 yaitu cobacoba, sit_uat dan production.
Environment CobaCoba (untuk git branch CobaCoba) digunakan tujuan deployment untuk memeriksa apakah kode dapat berfungsi pada sistem.
Environment CobaCoba merupakan tempat developer melakukan percobaan sebelum fitur yang dikerjakannya diuji.
Environment sit/uat (untuk git branch sit_uat) digunakan saat sprint review. Setiap branch userstory akan dimerge ke sit uat dan akan dicoba saat sprint meeting.
Peran developer pada environtment sit/uat adalah tempat pengujian sebelum fitur dapat menjadi produk yang siap digunakan.
Peran product owner dan pihak - pihak yang melakukan sit/uat adalah mencoba software yang sudah dideploy oleh developer dan memberikan feedback sebelum dideploy pada production.
Environtment production (untuk git branch master) memiliki aplikasi yang berjalan di environment production yang merupakan hasil sprint review yang sudah diapprove oleh Product Owner.
Peran developer masih ada pada environtment ini yaitu melakukan monitoring pada fitur yang dibuat dan cepat tanggap apabila terdapat bug.
Peran pengguna adalah menggunakan aplikasi yang dideploy pada environtment production.

## Error Code dan Exception Handling
Menurut penulis, error code adalah kode apabila terjadi kesalahan yang dilakukan oleh user atau sistem yang ditampilkan kepada user agar mudah dimengerti. Hal ini sudah diterapkan oleh kelompok kami pada backend (Golang) yaitu terdapat pada [link](https://gitlab.com/PPL2018csui/Kelas-D/PPL2018-D2/tree/sit_uat/abeona/src/core/errors) dan [link](https://gitlab.com/PPL2018csui/Kelas-D/PPL2018-D2/blob/sit_uat/abeona/src/admin/admin_handler.go).
Arti dari error code yang kelompok kami tentukan terdapat pada digit pertama dan keduanya.
Untuk 1xxx, error - error umum pada saat pemrosesan data.
Untuk 21xx, error yang terjadi pada sistem abeona saat dilakukan operasi mengenai product pada database.
Untuk 22xx, error yang terjadi pada sistem abeona saat dilakukan operasi mengenai merchant pada database.
Untuk 31xx, error yang terjadi karena input admin tentang product.
Untuk 32xx, error yang terjadi karena input admin tentang merchant.
Dari sudut pandang user (error code akan ditambahkan lagi untuk user story lainnya), error code dapat digunakan sebagai panduan untuk berkonsultasi pada pihak abeona untuk melakukan troubleshooting atau dengan membaca list error dari sistem abeona.
Untuk admin, pihak abeona dapat memberikan panduan error agar admin dapat mengetahui akar dari permasalahannya jika terjadi masalah. Admin juga dapat membedakan kesalahan pada sistem abeona atau kesalahan pada input admin.
Untuk pihak developer, konvensi error code perlu dibuat agar tidak membuat bingung user dan admin dan error pada kodingan yang berbeda dapat disamaratakan.

Exception Handling adalah proses untuk merespon kondisi dimana terdapat kesalahan. Exception Handling bisa terdapat lebih dari 1 untuk sebuah fungsi dimana disesuaikan dengan exception yang terjadi. Hal ini sudah diterapkan pada backend kelompok kami pada [link](https://gitlab.com/PPL2018csui/Kelas-D/PPL2018-D2/blob/sit_uat/abeona/src/admin/admin_handler.go).

## Software Architecture
Arsitektur abeona terdiri dari beberapa komponen, antara lain : - Android Apps - Database Server - Backend Service - Admin webapps

Flow android apps adalah aplikasi akan mengirim request menggunakan REST API ke endpoint backend server dengan bahasa golang, lalu backend akan memproses request tersebut dan memprosesnya untuk disimpan ke dalam database server.
Untuk flow admin webapps sendiri mengirim request menggunakan REST API ke endpoint backend server dan diproses serta disimpan pada database server.

Kelompok abeona juga menggunakan docker untuk menghilangkan proses konfigurasi pada server.