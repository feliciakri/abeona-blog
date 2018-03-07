---
title: "Perkenalan dan Proses Pengembangan"
date: 2018-03-07T12:07:51+07:00
author: Stefanus James Aditya
tags: ["Stefanus", "Sprint 0", "Sprint 1", "Android Studio", "Firebase"]
---

## Introduction

Halo semua! Nama saya Stefanus James Aditya, saya bertugas sebagai *Hustler* dari kelompok ini. Untuk post pertama saya ini saya akan menceritakan hal-hal yang sudah terjadi selama masa pengembangan aplikasi dari awal sampai minggu ke-5 sekarang ini.

## Sprint 0

Sprint 0 adalah saat pertama kali pembentukan tim untuk pengembangan aplikasi yang akan kami lakukan pada 1 semester ini. Aplikasi yang akan kami kembangkan adalah sebuah aplikasi Android yang berguna sebagai *Digital Travelling Assistant*. Keren kan! Nah, kami bekerja sama dengan [Halonesia](https://halonesia.co.id/) dengan Halonesia sendiri berperan sebagai *Product Owner* dari aplikasi yang akan kami buat.

![week1](/img/week1.jpg)

Pada minggu pertama, kami mendapat *lecture* tentang *overview* dan *brainstorming* untuk pengembangan aplikasi ini. Pada minggu pertama ini juga kami bertemu dengan Kak Ace dari Halonesia untuk ideasi tentang aplikasi yang akan dibuat.

Pada minggu kedua, kami membuat *project proposal* dan *mockup* aplikasi kami. Oh iya, pada saat ini juga kami setuju untuk memberikan nama aplikasi ini Abeona. Kami memilih nama Abeona karena menurut kami nama tersebut cocok dengan tema traveling dari aplikasi yang kami buat ini.

Pada minggu ketiga, kami mendapat *lecture* lagi, kali ini 2 orang *lecturer* merupakan alumni Fasilkom UI angkatan 2008 dan 2009. Mereka yang sudah bekerja di industri perangkat lunak menjelaskan kepada kami tentang Scrum. Kami pun telah membuat *Scrum Board* yang merupakan hasil dari ideasi dari Abeona nanti.

Di minggu keempat ini, kami sudah memulai pengembangan aplikasi secara teknis. Kami berdiskusi untuk menggunakan *tools-tools* apa yang cocok untuk membuat Abeona. *Tools-tools* tersebut akan dijelaskan pada postingan lain. Pada minggu ini juga kami menyelesaikan *Product Backlog* pada *Scrum Board* untuk Abeona dan melakukan *Sprint Planning* untuk *Sprint* 1. Pada pembagian *task*, saya mengambil *task* untuk otenfikasi akun aplikasi Abeona menggunakan Firebase.

![week4](/img/week4.jpg)

## Sprint 1

Sprint 1 dimulai pada minggu kelima. Pada *start sprint* 1 ini, saya mempelajari teknologi-teknologi yang digunakan untuk membuat Abeona dan juga mencoba menggunakan teknologi-teknologi tersebut. Karena ini adalah kali pertama saya menggunakan teknologi-teknologi tersebut, makanya saya masih harus belajar lagi. Nah, teknologi-teknologi yang saya pelajari dan gunakan tersebut adalah Android Studio dan Firebase. Berikut saya jelaskan sedikit tentang teknologi tersebut, tapi tidak banyak karena saya masih *newbie* (heh).

## Android Studio

[Android Studio](https://developer.android.com/studio/index.html) adalah IDE (Integrated Development Environment) yang khusus digunakan untuk aplikasi Android.

![android-studio](/img/android-studio.jpg)

Penggunaan bahasa pemrograman awalnya ingin menggunakan Kotlin, tetapi karena saya belum biasa menggunakan Kotlin, jadi untuk saat ini masih menggunakan Java.

#### Todo
- Mempelajari Kotlin
- Mendalami fitur-fitur yang ada dalam Android Studio
- Membiasakan diri dalam menggunakan Android Studio

## Firebase

[Firebase](https://firebase.google.com/) adalah *tool* yang disediakan oleh Google yang langsung terintegrasi dengan Android Studio. 

![firebase](/img/firebase.png)

Firebase memiliki *tools* yang berguna untuk otenfikasi akun, *crash analytics*, *cloud storage*, *database*, dan lain-lain.  Dengan menggunakan firebase, hal-hal tersebut dipermudah.
Penggunaan firebase pada aplikasi ini adalah untuk otenfikasi registrasi dan login akun menggunakan e-mail, Google Sign-In, dan Facebook. Pada saat ini, saya baru membuat script untuk registrasi menggunakan e-mail. Tetapi masih belum selesai, belum ditest, dan belum dibuat script untuk testing.

Masalah yang saya hadapi adalah untuk menge-test harus ada *user interface* yang bisa digunakan untuk mencobanya secara langsung, tetapi hal tersebut masih belum dibuat. Jadinya saya masih tidak tahu bagaimana hasilnya.

```Java
private void createAccount(String email, String password){
        mAuth.createUserWithEmailAndPassword(email, password)
            .addOnCompleteListener(this, new OnCompleteListener<AuthResult>() {
                @Override
                public void onComplete(@NonNull Task<AuthResult> task) {
                    if (task.isSuccessful()) {
                        Log.d(TAG, "createUserWithEmail:success");
                        FirebaseUser user = mAuth.getCurrentUser();
                        updateUI(user);
                    } else {
                        Log.w(TAG, "createUserWithEmail:failure", task.getException());
                        Toast.makeText(EmailAndPassActivity.this, "Authentication failed.",
                                Toast.LENGTH_SHORT).show();
                        updateUI(null);
                    }
                }
            });
}

private void signIn(String email, String password){
    mAuth.signInWithEmailAndPassword(email, password)
            .addOnCompleteListener(this, new OnCompleteListener<AuthResult>() {
                @Override
                public void onComplete(@NonNull Task<AuthResult> task) {
                    if (task.isSuccessful()) {
                        Log.d(TAG, "signInWithEmail:success");
                        FirebaseUser user = mAuth.getCurrentUser();
                        updateUI(user);
                    } else {
                        Log.w(TAG, "signInWithEmail:failure", task.getException());
                        Toast.makeText(EmailAndPassActivity.this, "Authentication failed.",
                                Toast.LENGTH_SHORT).show();
                        updateUI(null);
                    }
                }
            });
}
```

#### Todo
- Mendalami firebase
- Mencoba menggunakan fitur yang sudah dibuat dengan membuat *user interface*
- Integrasi dengan Facebook untuk otenfikasi menggunakan Facebook
- Setup Facebook Login di console firebase