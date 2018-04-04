---
title: "Vincent Individual Review 3"
date: 2018-04-04T19:23:26+07:00
author: Vincent
tags: ["Vincent", "US 09", "Merchant", "Sprint 2", "Week 9"]
---

Pada sprint 2 minggu pertama, penulis akan membahas mengenai *Software Development: Agile*, *Software Environment* dan *Exception Handling*.

## Software Development: Agile
*Agile software development* adalah metode pengembangan perangkat lunak dimana pengembangannya dalam jangka pendek dan memerlukan respon yang cepat jika terdapat perubahan.
Berdasarkan pengertian agile software development, agile software development sangat efektif diterapkan pada mata kuliah PPL karena tujuan dari PPL adalah menghasilkan produk yang bekerja dengan baik dalam waktu 1 semester.

Jenis agile yang diterapkan pada perkuliahan PPL adalah Scrum. Pada metode scrum terdapat Scrum master. Ferdinand selaku scrum master bertugas untuk memantau progress kelompok kami dan memimpin jalannya daily scrum. Daily scrum adalah pertemuan untuk membahas progress dari tiap anggota scrum. Daily scrum pada kelompok kami diadakan sebanyak 2 kali, yaitu pada hari Senin pukul 15.00 dan Kamis pukul 12.00. 

Agile manifesto terdiri dari 4 yaitu: 
1. Individuals and interactions over processes and tools
Hal ini saya rasakan setelah sprint meeting, dimana komunikasi antar individu sangat penting karena setiap individu mengerjakan task yang akan membentuk menjadi user story. Interaksi perlu terus dilakukan agar tidak terjadi salah paham mengenai user story yang dikerjakan.
2. Working software over comprehensive documentation
Hal ini diterapkan pada PPL karena produk yang diharuskan harus dapat bekerja dalam waktu dekat. Dokumentasi dapat dinomorduakan.
3. Customer collaboration over contract negotiation
Kolaborasi dari customer (pada kelompok kami yaitu kak Ace) sangat penting agar produk yang dihasilkan dapat sesuai ekspektasi dari customer atau product owner.
4. Responding to change over following a plan
Dengan prinsip ini, ketika terjadi perubahan pada requirement maka kita dapat beradaptasi agar produk yang dihasilkan sesuai dengan yang customer inginkan.

## Software Environment
Pada perkuliahan PPL, software environmentnya terbagi menjadi 3 yaitu cobacoba, sit_uat dan production.
Environment CobaCoba (untuk git branch CobaCoba) digunakan tujuan deployment untuk memeriksa apakah kode dapat berfungsi pada sistem.
Environment sit/uat (untuk git branch sit_uat) digunakan saat sprint review. Setiap branch userstory akan dimerge ke sit uat dan akan dicoba saat sprint meeting.
Environtment production (untuk git branch master) memiliki aplikasi yang berjalan di environment production yang merupakan hasil sprint review yang sudah diapprove oleh Product Owner.

## Error Code dan Exception Handling
Menurut penulis, error code adalah kode apabila terjadi kesalahan yang dilakukan oleh user atau sistem yang ditampilkan kepada user agar mudah dimengerti. Hal ini sudah diterapkan oleh kelompok kami pada backend (Golang) yaitu terdapat pada [link](https://gitlab.com/PPL2018csui/Kelas-D/PPL2018-D2/tree/sit_uat/abeona/src/core/errors).
Exception Handling adalah proses untuk merespon kondisi dimana terdapat kesalahan. Exception Handling bisa terdapat lebih dari 1 untuk sebuah fungsi dimana disesuaikan dengan exception yang terjadi. Hal ini sudah diterapkan pada backend kelompok kami pada [link](https://gitlab.com/PPL2018csui/Kelas-D/PPL2018-D2/blob/sit_uat/abeona/src/admin/admin_handler.go).