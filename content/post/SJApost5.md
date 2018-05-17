---
title: "Individual Review 5"
date: 2018-05-02T12:12:52+07:00
author: Stefanus James Aditya
tags: ["Stefanus", "Sprint 2", "Sprint Retrospective", "Sprint Review", "Architecture"]
---

Pada postingan ini saya ingin membahas tentang perkembangan tim sejak sesudah sprint review pertama, dan juga membahas sedikit tentang software architecture.

## Sprint Review & Sprint Retrospective

Sesudah sprint review pertama yang tidak begitu lancar. Pada retrospective, kami setuju untuk melakukan eksperimen perbaikan dengan mengganti platform chat yang menghubungkan anggota kelompok, dari menggunakan LINE menjadi Slack. Eksperimen ini berjalan dengan baik, dan anggota kelompok lebih sering untuk membalas chat di aplikasi ini. Beberapa anggota kelompok lain juga sudah bekerja secara pair programming. Load kerja masih terlalu berat ke salah satu anggota kelompok. Pekerjaan beberapa ada yang terhambat karena menunggu hasil pekerjaan dari anggota lain. Selama sprint 2 berjalan hal ini hanya sedikit berkurang.

Sprint review kelompok Abeona ke 2 pun tidak berjalan dengan lancar. Ada anggota kelompok yang tidak masuk dan terlmabat karena ketiduran. Ya, hal ini terjadi karena semalam sebelum sprint review, kami melakukan hackathon, begadang semalaman dan baru sempat tidur sebentar pada subuh. Showcase deliverable pun terhambat karena hal ini.

Pada sprint retrospective, kurang 1 anggota karena tidak masuk. Karena banyak hal negatif dibandingkan dengan hal positif yang kami rasakan, maka saya akan langsung menulis eksperimen perbaikan. Kami yang hadir setuju untuk melakukan pair programming, untuk selalu hadir pada biweekly scrum meeting, dan menggilir orang yang menjadi posisi hustler setiap harinya.

## Software Architecture

Abeona memiliki beberapa komponen yang membangun aplikasi Abeona, yaitu:
- Aplikasi android
- Aplikasi web untuk admin
- Server database
- Backend service yang menggunakan go language