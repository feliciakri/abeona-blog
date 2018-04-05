---
title: "Abeona Architecture"
date: 2018-04-05T00:16:38+07:00
author: Fersandi Pratama
tags: ["Fersandi", "agile", "Sprint 2", "architecture", "server", "devOps"]
---

# Abeona Architecture

![Abeona Architecture image](/img/abeona-architecture.jpg)

Aplikasi abeona terdiri dari beberapa komponen, antara lain :
- Android Apps
- Database Server
- Backend Service
- Admin webapps

Flow android apps adalah aplikasi akan mengirim request menggunakan **REST API** ke endpoint backend server yang menggunakan golang,
lalu backend akan memproses request tersebut dan memprosesnya untuk disimpan ke dalam *database server*.

Untuk autentikasi menggunakan server firebase, aplikasi android akan langsung terhubung ke firebase server lalu, autentikasi yang didapatkan akan dikirim ke backend server.



