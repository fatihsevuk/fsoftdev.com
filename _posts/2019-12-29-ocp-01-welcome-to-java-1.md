---
title: OCP Chapter 1 Welcome to Java-I
date: Sun 29-Dec-2019 04:08 P.M.
language: Turkish
categories: [Blogging, JavaSE, OCP]
tags: [ocp, oracle java programmer, javase, java standart edition, jar, java, javac,
  javadoc, java compiler, bytecode, jdk]
seo:
  date_modified: 2020-01-28 01:35:14 +0100
---

#### Giriş - Java Ortamı

Java dilinde geliştirme yapmak için gerekli olan ortama **JDK (Java Development Kit)** denir.JDK java kodu yazıp çalıştırmamız için gerekli yapıları içierir, bu yapılardan bahsedecek olursak; yazdığımız kodları **.java** uzantısı ile kaydederiz ve JDK içerisinde gelen **java derleyicisini(java compiler) (javac)** kullanarak bu .java dosyalarını **.class** uzantılı **bytecode** formatına çeviririz.JDK içeriside gelen bir diğer yapıda derlenen java dosyalarını çalıştırmamıza yarayan **launcher** **java** yapısıdır.Programlarımızı komut satırı yardımı ile derleyip çalıştırmak için **javac** ve **java** yapılarını kullanacağız.JDK ile gelen diğer iki yapıda **jar** ve **javadoc** yapılarıdır.**jar** komutu java dosyalarını paketlemek için , **javadoc** komutu ise dökümantasyon oluşturmak için kullanılır. 

**javac** komutu bytecode diye adlandırılan **.class** dosyalarını üretir, .class dosyaları **java** komutu ile çaliştırılırlar.**java** komutu kodu çalıştırmadan önce **Java sanal makinesi' ni (JVM - Java Virtual Machine )** başlatır.JVM .class dosyalrının gerçek makinede nasıl çalışacağını bilen yapıdır.

Java 11 den önce **JRE(Java Runtime Environment)** JDK'nın bir alt kümesi olarak geliyordu yani JDK'yı indirdiğimizde klasör içinde jre adında bir klasör ile geliyordu.JRE Java programlarını çalıştırmamızı sağlayan fakat kod derleme yapmayan bir yapıdır.Günlük hayatta kullandığımız ve java ile yazılan tüm programları kurmak için ön şart olarak Java'yı indirmemiz söylenir burada Java diye bahsedilen aslında JRE' dir.

Program yazarken algoritmalarımızı gerçeklemek için gerekli olan yapıları çoğunlukla kendimiz yazmayız; örnek vermek gerekirse iki metnin eşit olup olmadığını karşılaştırmak için gerekli olan kodları çoğu zaman kendimiz yazmayız daha çok programlama dilinin sunduğu yapıları kullanırız işte bu yapıların tamamına **application programming interfaces (APIs)** denir.Java çok geniş bir API' ye sahiptir.

Java programları geliştirmek için bir çok seçeneğimiz vardır.Sistemimizde JDK kurulu olduktan sonra **terminal** başta olmak üzere **notpad**,**vim**,**emacs** gibi text editörleri yada **Eclipse**,**NetBeans**,**Intellij** gibi daha gelişmiş **IDE' ler ( Integrated Development Environment )** kullanılabilir.

Bilgisayarımızda JDK' nın kurulu olup olmadığını terminal ekranına aşağıdaki kodları yazarak kontrol edebiliriz.

        java --version

**_OUTPUT_**


        
        
        openjdk 11.0.5 2019-10-15
        OpenJDK Runtime Environment (build 11.0.5+10-post-Ubuntu-0ubuntu1.118.04)
        OpenJDK 64-Bit Server VM (build 11.0.5+10-post-Ubuntu-0ubuntu1.118.04, mixed mode, sharing)
 
 ---

        javac --version

**_OUTPUT_**

        javac 11.0.5

Eğer bu komutları çalıştırdıktan sonra yukarıdaki gibi çıktılar gelmiyorsa sisteminizde java kurulu değildir.Java kurmak için Oracle'ın sitesindeki yönergelerden faydalanılabilir.

## Java' nın Avantajları

#### Object Oriented - Nesneye Yönelik
Java _nesneye yönelimli(**object oriented**)_ bir dildir.Yani tüm java kodları sınıflar içerisinde tanımlanır ve bu sınıfların çoğunluğu nesnelere örneklendirilir.Java'dan önceki bir çok dil prosedüreldi yani sınıf yapıları yoktu kodlar metodlar yada rutinler içerisinde yazılıyordu.Günümüzde nesne yönelimli yaklaşımın dışında yaygın olarak kullanılan bir diğer yaklaşımda fonksiyonel yaklaşımdır.Java 8 sürümü ile birlikte Java dili fonksiyonel özellik de kazanmıştır.

#### Encapsulation - Kapsülleme
Java dili verilerin istenmeyen erişimlerden korumak için **access modifiers(erişim belirleyicileri)** yapısına sahiptir. 

#### Platform Bağımsız
Java **_Write Once Run Everywhere_** mottosu ile çıkmıştır.Ön derleme işleminden geçip bytecode halini alan java kodları her makinede çalışırlar.

#### Güçlü
C++ dilinde hafıza yönetimi programcıya aitti bu da program yapma işini oldukça zorlaştıran bir durumdu çünkü **gabage collecting (çöp toplama)** programcı tarafından yapılıyordu programcı işe yaramayan nesneleri yok etmek durumundaydı.Kötü bir hafıza yönetimi bir çok hata çıkmasına sebebiyet veriyordu java ile birlikte tüm bu işlemler javanın kontrolune geçip otomatikleşti.

#### Basit
Java C++ ' dan basit bir yapıda tasarlanmıştır.Pointer yapısı C++ dilini oldukça zor anlaşılır bir yapı haline getirmekteydi Java ' da pointer yapısı yoktur.Operatörlerin aşırı yüklenmesi durumu Java' da yoktur.

#### Güvenli 
Java kodları JVM içerisinde oluşturulan sanal bir alanda(**sandbox**) çalışır bu durum da kodların, çalışacağı gerçek makineye doğrudan bir zarar vermesine olanak vermez.

#### Multithread
Java dili bir çok kod parçacığının aynı anda eş zamanlı çalışmasına olanak sağlayacak şekilde tasarlanmıştır.

#### Backward Compatibility - Geriye Dönük Uyumluluk
Java dili eski Java sürümlerinde yazılan kodların sonraki sürümlerde çalışmasına olanak sağlayacak şekilde geliştirilir.


> Bu yazı dizisinde linkini paylaştığım **_OCP Oracle Certified Professional Java SE 11 Programmer I Study Guide: Exam 1Z0-815_** kitabından tuttuğum notların derlenmiş halini yazmaya çalışacağım.
https://www.amazon.com/gp/product/1119584701/ref=ox_sc_act_title_1?smid=A2SQ87CI0X1ZUS&psc=1




