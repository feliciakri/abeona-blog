---
title: "Refactoring Android Code"
date: 2018-05-02T09:18:23+07:00
Author: Fersandi Pratama
tags: ["refactor", "android", "sprint 3", "kotlin"]
---

# Refactoring Code

> …is a disciplined technique for restructuring an existing body of code,
> altering its internal structure without changing its external behavior.

> Its heart is a series of small behavior preserving transformations. Each transformation 
> (called a “refactoring”) does little, but a sequence of transformations can produce a significant
> restructuring. Since each refactoring is small, it’s less likely to go wrong. The system is kept
> fully working after each small refactoring, reducing the chances that a system can get seriously
> broken during the restructuring.

*Refactoring* adalah teknik untuk mekonstruksi kode tanpa mengubah perilaku dan fungsi dari kode
tersebut. Tujuan dari *refactoring* itu sendiri adalah meningkatkan *readability* dan mengurangi 
kompleksitas kode. Joshua Kerievsky dalam bukunya **Refactoring to Patterns** mengatakan bahwa
dengan memperbaiki desain kode secara berkelanjutan akan membuat pekerjaan lebih mudah, jika kita
memiliki kebiasaan *refactoring* secara berkala maka kita akan menemukan bahwa memelihara dan
mengembangkan kode menjadi lebih mudah. 

Berdasarkan website [refactoring.com](www.refactoring.com) terdapat banyak cara 
untuk melakukan *refactoring*. Beberapa *refactoring* yang dilakaukan pada proyek Abeona ini
khususnya *source code* Android diantaranya adalah :

### Extract Interface

Terdapat beberapa `Activity` yang memerlukan pengecekan autentikasi. `Activity` tersebut memerlukan
pengecekan apakah *user* telah *login* atau tidak. Hal ini diperlukan supaya user yang tidak
terautentikasi tidak dapat mengakses `Activity` yang membutuhkan informasi dari user itu sendiri.

Oleh karena itu, dibuatlah *interface* `Authable` yang diimplementasikan pada beberapa `Activity`
yang membutuhkan fungsionalitas auth. *Interface* `Authable` memiliki satu buah `method` yaitu
`checkLoginCallback` yang berfungsi untuk melakukan suatu tindakan jika user telah login atau belum.

`Authable.kt`
```kotlin
package ga.abeona.abeona.view.auth

interface Authable {
    fun checkLoginCallback(isLogin : Boolean, user : FirebaseUser?)
}
```

Beberapa `activity` yang mengimplementasikan `Authable.kt` :

```kotlin
class EmailAndPassActivity : AppCompatActivity(), View.OnClickListener, Authable {
	override fun checkLoginCallback(isLogin: Boolean, user: FirebaseUser?) {
        if (isLogin){
            startActivity(Intent(this, HomeActivity::class.java))
            finish()
        }
    }
}

class SplashActivity : AppCompatActivity(), Authable {
	override fun checkLoginCallback(isLogin: Boolean, user: FirebaseUser?) {
        when (isLogin){
            true -> startActivity(Intent(this, HomeActivity::class.java))
            false -> startActivity(Intent(this, LoginActivity::class.java))
        }
    }
}
```

### Replace Method with Method Object

Dalam membuat objek *API Service*, yang menggunakan library *retrofit*. Terdapat redudansi kode pada
saat membuat objek `Retrofit`, dapat dilihat pada kode berikut :

```kotlin
val baseUrl = BuildConfig.BASE_URL

Retrofit.Builder().addCallAdapterFactory(
                            RxJava2CallAdapterFactory.create()
                    )
                    .addConverterFactory(
                            GsonConverterFactory.create()
                    )
                    .baseUrl(baseUrl)
                    .build()
                    .create(ApiServiceClass::class.java)
```

Pada kode tersebut hal yang membedakan untuk tiap pembuatan objek `retrofit` adalah pada bagian
`.create(ApiServiceClass::class.java)` dimana paramater *method* tersebut adalah kelas yang akan
dibuat untuk berkomunikasi dengan server *backend*. Karena untuk tiap kelas `API service` pembuatan
objek retrofit-nya hampir memiliki kode yang sama, maka method untuk membuat objek retrofit diganti
dengan sebuah objek yang bernama `BaseService.kt`. 

`BaseService.kt`
```kotlin
object BaseService {
        fun createRetrofitObject() : Retrofit{
            val baseUrl = BuildConfig.BASE_URL
             return Retrofit.Builder()
                    .addCallAdapterFactory(
                            RxJava2CallAdapterFactory.create()
                    )
                    .addConverterFactory(
                            GsonConverterFactory.create()
                    )
                    .baseUrl(baseUrl)
                    .build()
        }
}
```

`LoginApiService.kt`
```kotlin
interface LoginApiService {

    @GET("/m/onboard/check")
    fun checkUser(@Query("firebase_id") firebaseId : String) : Observable<CheckUserResponse>

    companion object {
        fun create() : LoginApiService{
            return BaseService.createRetrofitObject()
                    .create(LoginApiService::class.java)
        }
    }
}
```

### Extract Superclass / Interface (Kotlin/ Java 8)

Penggunaan *reactive programming* dengan library `RxJava` dan `RxAndroid` membuat komunikasi dengan
*backend server* asinkronus dengan flow aplikasi android. Hal ini membuat kode `Presenter` berjalan
pada *thread* yang berbeda dari *thread* utama android. Masalah terjadi ketika `Activity` akan
di-"destroy",  *thread* pada `presenter` harus dihentikan. Cara untuk menghentikan *thread* tersebut
dapat dilihat pada kode berikut :

```kotlin
class OnboardingCheckPresenter(
        val view : OnboardingCheckContract.View,
        val backgroundScheduler: Scheduler,
        val mainScheduler: Scheduler)
    : OnboardingCheckContract.Presenter {
    var apiService = LoginApiService.create()
    override var compositeDisposable = CompositeDisposable()

    override fun checkLogin(firebaseId : String){
        val disposable = apiService.checkUser(firebaseId)
                .subscribeOn(backgroundScheduler)
                .observeOn(mainScheduler)
                .subscribe(
                        { response ->
                            if (response.data.exists) {
                                view.openOnboarding()
                            }
                        },
                        {
                            view.showError(parseErrorMessage(it.message))
                        }
                )

        compositeDisposable.add(disposable)

    }

    fun onDestroy(){
    	compositeDisposable.dispose()
    }
}
```

Fungsi `onDestroy()` akan menghentikan semua *thread* yang berhubungan dengan pengambilan data pada
*backend server*. Namun dengan semakin banyaknya `presenter` yang dibuat, method `onDestroy()` untuk
tiap `presenter` memiliki perilaku yang sama. Method tersebut akhirnya di letakkan pada *interface*
`BasePresenter.kt`. Pada bahasa pemrograman `kotlin` atau `Java` versi 8, `Interface` dapat 
memberikan implementasi terhadap method-method-nya.

`BasePresenter.kt`
```kotlin
interface BasePresenter<T : BaseView> {
    var compositeDisposable : CompositeDisposable

    fun onDestroy(){
        compositeDisposable.dispose()
	}
}
```

### Lazily Initialized Attribute

Kotlin memiliki fitur untuk menginisialisasi suatu atribut dengan *lazy* artinya variabel tersebut
akan di inisialisasi pada saat pertama kali digunakan/diakses. Sebagai contoh jika tidak menggunakan
*lazy* maka kode : 

```kotlin
val cls = SomeClass(someAttribute)

cls.someMethod()
```

Akan dieksekusi sebagaimana mestinya. Pada baris pertama maka fungsi *constructor* dari kelas
`SomeClass` akan langsung dieksekusi. Sedangkan kode :

```kotlin
val cls by lazy {
	SomeClass(someAttribute)
}

cls.someMethod()
```

Inisiasilisasi `cls` akan dilakukan pada saat seksekusi `cls.someMethod()`. Hal ini berguna agar
inisiasi suatu variabel tidak menghambat alur *activity* pada android.

Dalam proyek ini, beberapa **presenter** yang diinjeksi ke dalam kelas `Activity` di inisiasi pada
method `onCreate()`. Hal ini dapat memperlambat inisiasi tampilan pada android, sedangkan presenter
belum digunakan pada saat itu.

Contoh *refactoring* yang dilakukan :

```kotlin
lateinit var presenter : OnboardingCheckPresenter

override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_home)

        presenter = OnboardingCheckPresenter(this, Schedulers.io(), AndroidSchedulers.mainThread())

        checkOnboarding()
}
```

menjadi :

```kotlin
val presenter by lazy {
        OnboardingCheckPresenter(this, Schedulers.io(), AndroidSchedulers.mainThread())
}

override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_home)

        checkOnboarding()
}
```
