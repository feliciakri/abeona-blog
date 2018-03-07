---
title: "Graph Database, Gitlab CI, and Docker"
date: 2018-03-07T11:06:28+07:00
author: Fersandi Pratama
tags: ["Fersandi", "CI", "Database", "Docker", "Sprint 1"]
---

Pada sampai saat blog ini ditulis, penulis melakukan beberapa hal mengenai teknologi *database* yang akan digunakan pada aplikasi yang akan dibuat. Tahap-tahap yang dilakukan adalah belajar mengenai teknologi tersebut, membandingkannyadengan  teknologi lain, dan mencoba teknologi tersebut. Setelah melakukan hal tersebut penulis dan tim memutuskan untuk menggunakan *NoSQL* sebagai *database* utama, khususnya *graph database*.

Penulis juga belajar menggunakan gitlab CI sebagai *tools* untuk melakukan *continuous integration* pada *repository* gitlab, dan belajar mengenai docker untuk mempermudah pemasangan aplikasi-aplikasi pada laptop masing-masing tim dan
mempermudah proses *deployment*.

## Graph Database

*Graph Database* adalah basis data yang memodelkan datanya berdasarkan teori graf. Data terdiri dari dua bagian yaitu *vertex* dan *edge*. *Vertex* merepresentasikan sebuah entitas, seperti tabel pada *SQL database* dan *edge* merepresentasikan hubungan antara dua *vertex*, dalam *SQL database* membuat relasi  antara dua tabel dapat dilakukan menggunakan *foreign key*. Kami memutuskan untuk menggunakan *graph database* karena berdasarkan pengalaman penulis sendiri yang pernah membuat suatu website e-commerce dengan berbagai jenis produk, sehingga tiap jenis produk dibuat tabel sendiri untuk menghindari nilai *null*. Hal ini membuat eksekusi *query* mempunyai banyak operasi *join* sehingga secara komputasi cukup mahal. Salah satu faktor lain adalah karena fitur *schemaless* sehingga tiap data dapat memiliki atribut yang berbeda-beda.


#### ArangoDB

![ArangoDB Image](/static/img/arangodb.png)

Teknologi *graph database* yang digunakan adalah [ArangoDB](https://www.arangodb.com/ "Arango DB"). Pemilihan ArangoDB sendiri disebabkan karena lisensi yang digunakan menggunakan *Apache License 2.0* yang bisa digunakan untuk aplikasi komersil dan berdasarkan oleh *benchmark* yang dilakukan oleh ArangoDB sendiri yang dapat dilihat pada [blog](https://www.arangodb.com/2018/02/nosql-performance-benchmark-2018-mongodb-postgresql-orientdb-neo4j-arangodb/) mereka. Dapat dilihat bahwa performa ArangoDB cukup baik untuk digunakan dibandingkan dengan teknologi *database* yang lain.

Server database staging sudah dapat diakses pada [link](http://128.199.161.1:8529) berikut.

Schema yang telah dibuat dapat dilihat pada [halaman](https://gitlab.com/PPL2018csui/Kelas-D/PPL2018-D2/blob/us04_seed_migrate_database/migration/schema.py#L82) ini (menggunakan *pyArango*).

#### Todo Next
- Menghubungkan *database* dengan *backend server*.
- Mempelajari AQL (*Arango Query Language*).
- Mendefinisikan *indexes* yang akan digunakan untuk meningkatkan performa *query*.

## Gitlab CI

Gitlab CI adalah *CI/CD tools* yang terintegrasi dengan gitlab. Gitlab CI berfugsi untuk melakukan *build*, *test*, dan *deploy* secara otomatis. *Basicly*, CI memungkinkan repository project kita untuk di *clone* pada *isolated environment* dan menjalankan *script* pada *environment* tersebut.

Penulis ditugaskan untuk menulis CI untuk android apps. Hal yang dilakukan pada CI terdiri dari 2 hal yaitu membuat APK-nya menjadi *artifacts* dan melakukan *unit testing* serta menghitung coverage-nya. 

Berikut script yang telah dibuat oleh penulis :

```yaml
build-debug:android:
  image: openjdk:8-jdk
  stage: build
  before_script:
    - cd android
    - apt-get --quiet update --yes
    - apt-get --quiet install --yes wget tar unzip lib32stdc++6 lib32z1
    - wget --quiet --output-document=android-sdk.tgz https://dl.google.com/android/android-sdk_r${ANDROID_SDK_TOOLS}-linux.tgz
    - tar --extract --gzip --file=android-sdk.tgz
    - echo y | android-sdk-linux/tools/android --silent update sdk --no-ui --all --filter android-${ANDROID_COMPILE_SDK}
    - echo y | android-sdk-linux/tools/android --silent update sdk --no-ui --all --filter platform-tools
    - echo y | android-sdk-linux/tools/android --silent update sdk --no-ui --all --filter build-tools-${ANDROID_BUILD_TOOLS}
    - echo y | android-sdk-linux/tools/android --silent update sdk --no-ui --all --filter extra-android-m2repository
    - echo y | android-sdk-linux/tools/android --silent update sdk --no-ui --all --filter extra-google-google_play_services
    - echo y | android-sdk-linux/tools/android --silent update sdk --no-ui --all --filter extra-google-m2repository
    - export ANDROID_HOME=$PWD/android-sdk-linux
    - export PATH=$PATH:$PWD/android-sdk-linux/platform-tools/
    - chmod +x ./gradlew
  script:
    - ./gradlew assembleDebug
  artifacts:
    when: on_success
    paths:
    - android/app/build/outputs/
    expire_in: 1 week
  only:
    - sit_uat

build-release:android:
  image: openjdk:8-jdk
  stage: build
  before_script:
    - cd android
    - apt-get --quiet update --yes
    - apt-get --quiet install --yes wget tar unzip lib32stdc++6 lib32z1
    - wget --quiet --output-document=android-sdk.tgz https://dl.google.com/android/android-sdk_r${ANDROID_SDK_TOOLS}-linux.tgz
    - tar --extract --gzip --file=android-sdk.tgz
    - echo y | android-sdk-linux/tools/android --silent update sdk --no-ui --all --filter android-${ANDROID_COMPILE_SDK}
    - echo y | android-sdk-linux/tools/android --silent update sdk --no-ui --all --filter platform-tools
    - echo y | android-sdk-linux/tools/android --silent update sdk --no-ui --all --filter build-tools-${ANDROID_BUILD_TOOLS}
    - echo y | android-sdk-linux/tools/android --silent update sdk --no-ui --all --filter extra-android-m2repository
    - echo y | android-sdk-linux/tools/android --silent update sdk --no-ui --all --filter extra-google-google_play_services
    - echo y | android-sdk-linux/tools/android --silent update sdk --no-ui --all --filter extra-google-m2repository
    - export ANDROID_HOME=$PWD/android-sdk-linux
    - export PATH=$PATH:$PWD/android-sdk-linux/platform-tools/
    - chmod +x ./gradlew
  script:
    - ./gradlew assembleRelease
  artifacts:
    when: on_success
    paths:
    - android/app/build/outputs/
    expire_in: 1 week
  only:
    - master

unitTests:android:
  image: openjdk:8-jdk
  stage: test
  before_script:
    - cd android
    - apt-get --quiet update --yes
    - apt-get --quiet install --yes wget tar unzip lib32stdc++6 lib32z1
    - wget --quiet --output-document=android-sdk.tgz https://dl.google.com/android/android-sdk_r${ANDROID_SDK_TOOLS}-linux.tgz
    - tar --extract --gzip --file=android-sdk.tgz
    - echo y | android-sdk-linux/tools/android --silent update sdk --no-ui --all --filter android-${ANDROID_COMPILE_SDK}
    - echo y | android-sdk-linux/tools/android --silent update sdk --no-ui --all --filter platform-tools
    - echo y | android-sdk-linux/tools/android --silent update sdk --no-ui --all --filter build-tools-${ANDROID_BUILD_TOOLS}
    - echo y | android-sdk-linux/tools/android --silent update sdk --no-ui --all --filter extra-android-m2repository
    - echo y | android-sdk-linux/tools/android --silent update sdk --no-ui --all --filter extra-google-google_play_services
    - echo y | android-sdk-linux/tools/android --silent update sdk --no-ui --all --filter extra-google-m2repository
    - export ANDROID_HOME=$PWD/android-sdk-linux
    - export PATH=$PATH:$PWD/android-sdk-linux/platform-tools/
    - chmod +x ./gradlew
  script:
    - ./gradlew jacocoTestReport
    - cat app/build/reports/jacoco/jacocoTestReport/html/index.html
  coverage: '/Total.*?([0-9]{1,3})%/'

```

Kode dapat dilihat pada [link](https://gitlab.com/PPL2018csui/Kelas-D/PPL2018-D2/blob/coba_coba/.gitlab-ci.yml) ini.

#### Todo Next
- Membuat CI untuk *web apps*.

## Docker & Docker-compose

![Docker Images](/img/docker.jpg)

Docker adalah platform untuk melakukan pembuatan, dan pendistribusian aplikasi. Aplikasi yang dibangun menggunakan docker akan dijalankan dalam lingkungan yang terisolasi sehingga tidak terdistraksi dengan program-program lain yang berjalan pada komputer. Docker pada konteks projek PPL ini digunakan untuk melakukan pemasangan ArangoDB pada komputer masing-masing anggota tim.

Kami menggunakan tools docker-compose untuk melakukan pemasangan aplikasi-aplikasi pada laptop masing-masing. Docker-compose akan melakukan instalasi *container* yang terdefinisi pada file yang bernama `docker-compose.yml`. Penggunaan docker-compose memungkinkan pemasangan aplikasi hanya dengan satu perintah yaitu `docker-compose up`.

Berikut file `docker-compose.yml` yang telah dibuat penulis untuk melakukan pemasangan ArangoDB :
```yaml
version: '3'

services:
  arangodb:
    image: 'arangodb:3.3.3'
    container_name: arangodb
    volumes:
      - arangodb:/var/lib/arandgodb3
      - arangodb_apps:/var/lib/arandgodb3-apps
    environment:
      - ARANGO_NO_AUTH=1
      - ARANGO_STORAGE_ENGINE=rocksdb
    ports:
      - '8529:8529'
volumes:
  arangodb:
  arangodb_apps:
```
Kode sumber dapat dilihat pada [link](https://gitlab.com/PPL2018csui/Kelas-D/PPL2018-D2/blob/coba_coba/docker-compose.yml) ini.

#### Todo Next
- Melakukan dokerisasi pada back-end apps, sehingga anggota tim lain tidah harus melakukan instalasi go yang cukup kompleks.
- Melakukan dokerasisi pada web-apps.
- Menggunakan docker image untuk proses *deployment* dengan memanfaatkan *container registry* pada gitlab.

## Todo General

Berikut adalah hal-hal yang akan dilakukan penulis selanjutnya yang tidak berhubungan dengan 3 hal di atas :
- Belajar arsitektur android apps, dan membangun aplikasi android yang sesuai dengan *requirement* yang ada.
- Belajar menggunakan *react* untuk membangun web-apps.