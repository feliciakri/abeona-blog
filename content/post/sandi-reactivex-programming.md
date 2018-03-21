---
title: "A Brief Introduction to Reactive Programming"
date: 2018-03-21T09:54:39+07:00
author: Fersandi Pratama
tags: ["Fersandi", "android", "Sprint 1", "Reactivex", "RxJava", "Reactive Programming"]
---

# Reactive Programming

![ReactiveX Image](/img/reactivex.png)

*Reactive programming* adalah paradigma pemrograman yang memperhatikan *data stream* dan perubahan
dari data tersebut. Paradigma ini seperti layaknya *observer pattern* , terdapat objek *observable*
yang akan melakukan *streaming* data dan terdapat objek *subscription* yang akan bereaksi ketika
*observable* mengeluarkan data dan data tersebut akan dimasukkan pada *callback function* yang ada
pada objek *subscription*. Objek apa saja yang dapat menjadi *observable*? semua objek yang dapat
mengeluarkan data dapat menjadi *observable*. Contohnya adalah : Array, List, bahkan event klik 
sekalipun dapat dijadikan *observable*. *Reactive Programming* bersifat *asynchronous* dan dapat
dijalankan pada *thread* lain sehingga tidak melakukan blocking pada *thread* utama. Hal ini penting
ketika kita ingin menggunakan reactive programming pada android.

Terdapat istilah yang di sebut sebagai *functional reactive programming*. Istilah ini berarti bahwa 
kita menambahkan metode-metode fungsional pada *reactive programming*. Artinya data yang di keluarkan
oleh *observable* dapat dimanipulasi terlebih dahulu sebelum di konsumsi oleh *subscription*. Fungsi-fungsi
yang dapat digunakan untuk memanipulasi antara lain : `map`, `flatMap`, `filter`, dll.

Salah satu contoh manipulasi data dengan menggunakan fungsi `filter` :

![Marble Diagram](/img/marble-diagram.png)

Dalam pengembangan aplikasi android, salah satu *pattern* yang digunakan untuk melakuakn pengambilan
dan menampilkan data dapat dilakukan menggunakan *reactive programming* dengan menggunakan library 
`RxJava`, dan `RxAndroid`.

## RxJava

`RxJava` adalah *library* *reactive programming* yang dapat digunakan untuk bahasa pemrograman `java`.
Semua contoh kode pada blog ini akan ditulis menggunakan bahasa `kotlin`.

### Contoh RxJava

#### Observe String hello world
```kotlin
val observable = Observable.just("Hello, world")
				.subscribe(
					{text -> System.out.println(text)},
					{error -> System.out.println(error.getMessage())}
					)
					
```

Pada kode diatas objek *observable* hanya berisi datu data yaitu `string` "Hello, world". Data 
tersebut akan dikirim ke objek *subscriber* yang pada kode diatas adalah objek anonim di dalam 
fungsi `subscribe`. Objek *subscriber* dapat terdiri dari 1,2, ataupun 3 buah *method*. *Method*
pertama merupakan fungsi yang akan dijalankan ketika data berhasil di konsumsi, *method* kedua akan dijalankan ketika terjadi *exception* saat pengambilan data, *method* ketiga akan dijalankan ketika semua data berhasil di konsumsi.

### Penggunaan RxJava pada Android

Pada projek abeona, pengambilan data dari *database* menggunakan `retrofit` dan `RxAndroid`.
Salah satu contoh pengambilan data tersebut adalah sebagai berikut :

**product.kt**
```kotlin
data class Product(val _key : String,
                   val name : String,
                   val isAvailable : Boolean,
                   val price : Double,
                   val type: String,
                   val description: String,
                   val merchant: Merchant,
                   val location: Location,
                   val rating: Float,
                   val isWishlist: Boolean,
                   val phoneNumber: String?,
                   val website: String?,
                   val coverPhoto: String,
                   val galleryPhotos: Array<String>? = null,
                   val nearByProducts: Array<Product>? = null)
```

**productApiService.kt**
```kotlin
interface ProductApiService {
    @GET("product")
    fun singleProductData(
            @Query("_key") key : String
    ) : Observable<Product>

   companion object {
        fun create(): ProductApiService {
            val baseUrl = BuildConfig.BASE_URL

            val retrofit = Retrofit.Builder()
                    .addCallAdapterFactory(
                            RxJava2CallAdapterFactory.create()
                    )
                    .addConverterFactory(
                            GsonConverterFactory.create()
                    )
                    .baseUrl(baseUrl)
                    .build()

            return retrofit.create(ProductApiService::class.java)
        }
    }
```

**productPagePresenter.kt**
```kotlin
override fun loadData(productKey : String) {
        disposable = productApiService.singleProductData(productKey)
                   .subscribeOn(backgroundScheduler)
                   .observeOn(mainScheduler)
                   .subscribe(
                    {product -> view.showProductData(product)},
                    {error -> view.showError(error.message.orEmpty())}
                    )
}
```

Pada kode diatas objek *observable* adalah interface retrofit yang akan
melakukan request ke backend, setelah request berhasil dan data diambil, data
tersebut akan dimasukkan ke dalam method `view.shoProductData()`.

## CLosing

*Reactive Programming* merupakan salah satu pilihan untuk melakukan
*asynchronous programming*. Paradigma ini sangat berguna dalam pengembangan
aplikasi android, sehingga data yang akan diambil tidak melakukan 
penghambatan dalam menjalankan aplikasi.
