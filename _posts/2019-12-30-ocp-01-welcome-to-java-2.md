---
title: OCP Chapter 1 Welcome to Java-II
date: Mon 30-Dec-2019 02:00 P.M.
categories: [JavaSE, OCP]
language: Turkish
tags: [ocp, oracle java programmer, javase, java standart edition, jar, java, javac,
  javadoc, java compiler, bytecode, jdk, class, sınıf, object, fields, methods, comments]
seo:
  date_modified: 2020-01-28 01:35:14 +0100

---


#### Java' da Sınıf Yapısı

Java'nın nesne yönelimli bir dil olduğundan ve tüm kodların sınıf yapıları içerisine yazıldığından bir önceki bölümde bahsetmiştik.Sınıflar Java'daki en basit building block yapısıdır.Java sınıfları **class** keywordu kullanılarak tanımlanırlar.Java'da sınfları kullanmak için nesneler (**object**) kullanılır.Nesne bir sınıfın hafızadaki örneğidir.Hafızadaki nesneye işaret eden değişkene **referans** denir.

#### Alanlar ve Metodlar

Sınıfları oluşturan elemanlardan ikisi **fields(alanlar)** ve **methodlardır**.Field olarak bilinen alanlar diğer dillerde **değişken (variable)** olarak , metodlarda diğer dillerde fonksiyon yada prosedür olarak karşımıza çıkar.Sınıfı oluşturan tüm bu elemanlara **members(sınıfın üyeleri)** denir.

Gelin hep beraber ilk Java sınıfımız yazalım.Sınıfımızın adı MrRobot olsun ve **_isim,numara,güç adında üç alana_** _**koş ateşEt ve uç**_ adında üç metoda sahip olsun.Kodlarımızı yazarken değişken isimlerimiz valid karakterler olmak koşuluyla istediğimiz dilde yazabiliriz biz ingilizce yazmayı tercih edeceğiz.

```java
public class MrRobot { 
    
    String name;
    String number;
    int power;

    public void run() {
        // Bu metod robotumuzu koşturan kodları içercek.
    }

    public void fire(int bullet) {
        // Bu metod robotumuzun ateş etmesini sağlayan kodları içerir.
    }

    public void fly() {
        // Bu metod robotumuzun uçmasını sağlar.
    }
}

```

Şimdi sırasıyla kodlarımızı incelemeye başlayalım.

* <span style="color:#2398AB;">**public:**</span> Access modifier( Erişim belirleyici ) olarak adlandırılır.Bu sınıfımız public erişim belirleyicisine sahip olduğu için diğer sınıflar tarafından erişilebilir durumdadır.
* <span style="color:#2398AB;">**class:**</span> Sınıf tanımlamak için kullandığımız anahtar kelimedir.
* <span style="color:#2398AB;">**MrRobot:**</span> Sınıfımızın ismidir.
* <span style="color:#2398AB;">**String name:**</span> name alanımızın Sting türünde olduğunu ifade eder.Alanlarımız **int** , **boolean** , **double** vs gibi başka türlerde de olabilir.
* <span style="color:#2398AB;">**void:**</span> Metodları tanımlarken kullandığımız void anahtar kelimesi metodun geriye hiçbir değer döndürmediğini ifade eder. 

```java
public void fire(int bullet) {
        // Bu metod robotumuzun ateş etmesini sağlayan kodları içerir.
    }
```

fire metodumuzun içindeki int bullet ifadesi bu metodun **parametresidir**.fire metodu **integer türünde ve bullet isminde** bir parametre almş.Metodlar hiç parametre almayacağı gibi bir veya daha fazla sayıda parametre alabilirler.Metodlarımızın **ismi** ve **parametre** listesi **metodun imzasıdır(method signature)**.


#### Yorum Satırları

Yazdığımız kodların okunabilirliğini arttırmak için açıklama ifadelerine ihtiyaç duyarız.Bu ifadeler yazdığımız kodun başka programcılar tarafından daha kolay anlaşılabilmesini sağlar.Programlama dillerindeki yorum ifadeleri çalıştırılabilir ifadeler değildir derleyici tarfından derlenmezler.

Java dilinde 3 farklı şekilde yorum ifadeleri oluşturabiliriz bunlar;

* Tek satırlık yorumlar (Single line comment)
* Çok satırlık yorumlar ( Multi line comment)
* JavaDoc yorum ifadeleri

Bu üç yorum şekline örnek vermek gerekirse.

1. Tek satırlık yorumlar

```java
// Bu cümle tek satırlık yorum ifadesidir.
```

2. Çok satırlık yorumlar

```java
/* Çok satırlık
*  yorumlar
*  bu şekilde yazılır.
*/
```

3. Javadoc yorumları

```java
/**
 * Javadoc çok satırlı yorum
 * 
 */
```

Java da sınıfımızı yazdıktan sonra dosyamızı **.java** uzantısı ile kayıt ederiz.Burada dikkat etmemiz gereken iki durum var ;
* Dosya adımız public olan sınıfımız ile aynı olmalı.
* Bir dosyada birden çok sınıf tanımlanabilir tek şart dosya adı ile aynı isme sahip sınıfın **public** olmasıdır.

#### Main Metodu

Java kodları main maetodu ile çalışmaya başlar.Hemen örnek bir main metodu yazıp kodlarını inceleyelim.

```java
public class MrRobot {

    public static void main(String[] args) {

    }
}
```

Main metodumuzu inceleyecek olursak;

1. <span style="color:#2398AB;">**public:**</span> Erişim belirleyicsidir.
2. <span style="color:#2398AB;">**static:**</span> Bu anahtar kelime metodu sınıfa bağlar yani bu metodu çağırmak için sınfımızdan nesne üretmemize gerek yoktur.
3. <span style="color:#2398AB;">**void:**</span> Metodumuzun geriye hiçbir değer döndürmediğini söyleyen anahtar kelimedir.
4. <span style="color:#2398AB;">**main:**</span> Metodumuzun ismidir.
5. <span style="color:#2398AB;">**String[] args:**</span> Metodumuzun parametre listesidir.Main metodumuz args isminde String türünde bir liste yapısında parametre alır.Burada ek olarak şunları belirtmekte fayda 
var.args ismi valid olacak şekilde herhangi bir isim olabilir ve main metodunun paramete listesi 3 farklı şeilde yazılabilir bunlar;

    * <span style="color:#2398AB;">**String[] args** </span>
    * <span style="color:#2398AB;">**String args[]** </span>
    * <span style="color:#2398AB;">**String... args** </span>

Main metodu olmayan bir java sınıfını compile ettiğimizde derleyici hata fırlatır.Aynı zamanda static olmayan bir main metodu derleyici tarafından bulunamaz.

Main metodunun parametre listesi sayesinde JVM başladığında okunacak olan parametreler tanımlanabilir.Bu durumu örnek üzerinden açıklayalım.

```java
public class MrRobot {

    public static void main(String[] args) {
        System.out.println(args[0]);
        System.out.println(args[1]);
    }
}
```

Şimdi terminal yardımı ile sınıfımızı compile edelim.

    javac MrRobot.java


Bu komut çalıştıktan sonra aynı klasör içinde **MrRobot.class** dosyası oluşur daha sonra;

    java MrRobot "Name: Voltran" "Number: 1"

**_OUTPUT_**

    Name: Voltran
    Number: 1

Burada dikkat edecek olursanız kodlarımızda args[0] ve args[1] ' i yazdırıyoruz, yani parametre listemizin ilk iki elemanını, bu iki elemanı java komutunu çalıştırdığımız esnada aralarına boşluk koyarak verdik.

Burada dikkat etmemiz gereken kısımlar şunlardır;

* Tüm parametreler string olarak okunur.
* Parametereleri ayırmak için boşluk kullanılır.
* Eğer bir parametre birden çok kelimeden oluşuyorsa ve içerisinde boşluk karakteri varsa tırnak işaretleri arasına yazarız.Eğer tek kelimelik bir parametre ise tırnak arasına yazmaya gerek yoktur.
* Eğer kodda beklendiği kadar parametre girilmezse örneğin yukarıdaki kodda args[1] yani ikinci parametre girilmezse derleyici hata fırlatır.Bu hata _**ArrayIndexOutOfBounds**_ hatasıdır.

#### Java Programımızı Tek Satırda Çalıştırma

Java 11 ile birlikte **single file source code** diye bir özellik geldi bu özellikle birlikte javac kullanmadan aşağıdaki şekilde programımızı çalıştırabiliriz.

```java
public class MrRobot {

    public static void main(String[] args) {
        System.out.println(args[0]);
    }
}
```

    java MrRobot.java "Name: Voltran"

**_OUTPUT_**

    Name: Voltran

Dikkat ettiyseniz javac kullanarak bytecode dosyası oluşturma adımını atlamış olduk bu özelliği kullanmak için tek şart;
* Programımız sadece bir **.java** dosyasından oluşmalıdır.Eğer birden fazla  .java dosyamız varsa hala javac komutuna ihtiyacımız vardır.
 
Bir sonraki yazımızda Java'da paket yapısını anlatarak devam edeceğiz.

