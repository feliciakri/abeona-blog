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

### Extract Method

### Lazily Initialized Attribute


