---
title: "Individual Review 4"
date: 2018-04-19T01:13:53+07:00
author: Stefanus James Aditya
tags: ["Stefanus", "Sprint 2", "Git", "Software Environment"]
---

Pada postingan kali ini saya akan sedikit menjelaskan tentang git flow dan software environment yang digunakan untuk pengenmbangan software Abeona.

## Git

Pada repository pengembangan Abeona, terdapat 4 branch utama, yaitu: Master, sit_uat, User_Story, dan coba coba
Tetapi dalam perkuliahan PPL ini, terdapat juga 2 branch lain yang tidak wajib dilakukan, yaitu Hotfix dan Coldfix

#### Master
Branch Master merupakan branch utama yang menyimpan source code yang sudah siap dideploy ke production environment. Pada perkuliahan PPL, branch ini hanya boleh di merge saat sudah melakukan sprint review dengan syarat-syarat lain dari product owner. Oleh karena itu, code yang di merge harus benar-benar dijaga, karena branch inilah yang langsung di rilis di market.

#### sit_uat
Branch ini berfungsi dalam proses pengembangan Abeona. Fungsi branch ini adalah untuk menyatukan semua user story yang sudah selesai dari branch cobacoba, dan nantinya akan di merge ke branch master. Branch ini yang akan dinilai saat sprint review, oleh karena itu nantinya akan di merge ke branch master.

#### User_Story
User_story akan ada sebanyak user story yang akan dikerjakan. Kelompok kami menggunakan format penamaan sebagai berikut usxx_[NamaStory]. Nantinya user story yang sudah selesai akan di merge ke branch coba coba. Proses merging ini juga harus dilakukan dengan baik, yaitu dengan setidaknya 2 anggota tim yang lain harus mereview code dan menyetujui code yang mau di gabungkan ke branch sit_uat.


#### coba coba
Branch coba-coba, sesuai dengan namanya berfungsi untuk melakukan percobaan, branch ini merupakan gabungan dari branch user_story yang nantinya akan di merge ke branch sit_uat untuk dinilai pada sprint review nantinya.

![git flow](/img/git_flow.jpg)

## Software Environment

Dalam pengembangan Abeona, software environment terbagi menjadi 3, yaitu: CobaCoba, SIT/UAT, dan PRODUCTION
Pembagian software environment ini dilakukan untuk mempermudah pengembangan.

#### CobaCoba
Environment cobacoba merupakan environment tujuan deployment branch user_story, gunanya untuk mengetes apakah code berjalan dengan baik atau tidak.

#### SIT/UAT
Environment ini merupakan tujuan deployment dari branch sit_uat, dan merupakan environment yang digunakan untuk sprint review.

#### PRODUCTION
Environment production merupakan environment deployment dari branch master, artinya semua kode yang dijalankan disini sudah baik dan sudah di approve oleh product owner.