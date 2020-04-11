---
title: OCP-I Chapter2 Java Building Blocks-II
date: Sat 11-Apr-2020 02:18 A.M.
categories: [JavaSE, OCP]
language: Turkish
tags: [ocp, oracle java programmer, javase, java standart edition, java]
seo:
  date_modified: 2020-04-11 02:34:56 +0200
---

## Literal'lerin(Değişmezler) Yazımı 

Kodun içinde gördüğümüz sayısal ifadelere `literal` denir. Sayısal değerleri illa decimal(10) tabanda yazmamıza gerek yoktur. Binary(2), octal(8) veya hexadecimal(16) tabanda da yazabiliriz.

Bir sayı eğer

* Sadece 0 ve 1 değerlerinden oluşmuşsa ve 0b veya 0B ön eki ile başlıyorsa o sayı binary(2 lik) tabandadır.
* Sadece 0-9 aralığında numeric değer ve A ile F arasında harf değerinden oluşuyorsa ve 0X ile başlıyorsa hexadecimal(16 lık) tabandadır.
* Sadece 0-7 aralığında numeric değer alabiliyorsa octal(8 lik) tabandadır.

Sayısal değerleri okunabilirlik açısından basmakları arasına `_` koyarak ifade edebiliriz. `1_234_345_234` gibi. Yalnızca başa,sona ve ondalık değerin başına ve sonuna konulmaz.

## Java'da Referans Tipleri

Oluşturduğumuz nesnelerin hafızadaki adreslerini tutan değerlere `o nesnenin referansı` denir. C dilinde verilerin hafızadaki fiziksel adreslerini bilebiliyorduk fakat Java buna izin vermez. Aynı tipteki nesneler arasında referans atamsı olabilir. new anahtar kelimesi ile yeni oluşturulan nesnelere atanabilir.

```java
String text=new String("Hello from Mr Robot");
```

Kod örneğimizde `text` ifadesi yeni oluşturulan String nesnesinin referansıdır.
```java
String text=new String("Hello from Mr Robot");
text=new String("Good Bye from Mr Robot");
```

`text` referansına aynı türden oluşturduğumuz başka bir nesnenin fiziksel adresini bağladık.

```java
String hello=new String("Hello from Mr Robot");
String goodBye=new String("Good Bye from Mr Robot");
hello=goodBye;
```
Burada da `hello` referansına `goodBye` referansını atadık yani artık her iki referansta `new String("Good Bye from Mr Robot");` ifadesi ile oluşan nesneyi refere eder.

Primitiv türlerin aksine referans değerlere `null` değeri vererek hafızadaki işaret ettiği nesne ile bağını keseriz ve bu ilgili nesne artık Garbage Collector'ın ilgi sahasına girer.

Referans türler vasıtasıyla refere ettikleri nesnenin üyelerine erişebiliriz. Fakat primit türlerde böyle bir durum söz konusu değildir.

## Java Variables (Değişkenler)
Tüm diler de olduğu gibi Java dilinde de hafızada veri saklamak için değişkenler kullanılır. Aşağıda String, int ve long türünde 3 farklı değişken tanımı vardır.

```java

String sValue="I am a String value";
int iValue=21;
long lValue=232L;

```

Yukarıdaki kodda `sValue`,`iValue` ve `lValue` bizim değişkenlerimizdir ve `sahip oldukları veri tipinin kapasitesi miktarınca` hafızada alana sahiptirler.

## Naming Conversion
Java'da sınıflara değişkenlere metotlar vs. isim verirken şunlara dikkat etmemiz gerekir.

* Bir harf yada `$` veya `_` (Java 9 dan beri _ ye izin yok.) ile başlayabilir.
* Sayı ile başlayamaz fakat bitebilir.
* Java keywordlerden biriyle aynı isme sahip olamaz.
* goto ve const Java'da olamamasına rağmen reserved word olarak Java tarafından ayrılmıştır.
* true,false ve null reserved değil literaldir bu yüzden kullanılmazlar.

Java'da isimlendirme yaparken aşağıdaki stillerden birini kullanabiliriz.

* Camel Case
  * myVariableName
* Snake Case
  * my_variable_name
* Kebab Case
  * my-variable-name
* Pascal Case
  * MyVariableName

Java'da ayniçerisindeçerisnde birden çok değişken tanılanabilir ve değer ataması yapılabilir tabi ki değişkenler aynı türde olmak koşuluyla.

```java
int weight,height,age;
String country="Turkey",language="Turkish";
String city , int code; // DOESNT COMPILE
```
## Değişkenlere İlk Değer Atama (Initializing Variables)

### Yerel(Local) Değişkenler
Java'da bir kod bloğu, constructor ya da metot içerisinde tanımlanan değişkenlerdir. Değer ataması yapılmadan kullanılamaz çünkü default değerlere sahip değillerdir. Atama yapılmadan kullanılmaları halinde derleyici hata verir.

```java
public int fire() {
int power=100;
int bullet;
int fire=power+bullet;// DOESNT COMPILE
// bullet değişkeni initialize edilmediği için derleyici hata verir.
return fire;
}
```

Metotlara ve constructorlara geçirilen parametreler öncelikle initialize edilmelidir.

### Class ve Instance Değişkenleri

Instance değişkenleri field olarak adlandırılırlar. Sınıftan üretilen her nesne bu field'lara sahiptir fakat aynı değere sahip olmayabilirler. Class değişkenlerine ise nesne oluşturmadan erişmek mümkündür. Çünkü static olarak tanımlanırlar. Bu iki değişkende default değere sahiptir ve kullanacağımız zaman ilk değer ataması yapmamıza gerek yoktur.

**Defult değerler**

* String- `null`
* byte,int,long,shor - `0`
* char - `'\u0000' (NULL)`
* boolean - `false`
* double , `float - 0.0`

## Java'da _var_ Kullanımı

Java'da var 'ın formal ismi `Local variable interface type` olarak geçmektedir. Yalnızca local değişkenlerde kullanılır. Sınıf değişkenlerinde kullanımı derleme hatasına sebep olur.

```java
public String getNameAndAge() {
var name="MrRobot";
var age="20"
return name+age;
}
```

var ile tanımlama yapılan ilk satırda derleyici atanan değerin tütüne bakarak değişkenin tipini belirler ve değişkene yapılacak sonraki değer atamalarında bu tipi zorunlu tutar. Farklı bir türde değer ataması yapılırsa derleyici hata verir. Burada JS deki var yapısından farkı ortaya çıkar.

var ile tanımlanan değişkenin tipi sonradan değiştirilemez fakat değeri değiştirilebilir.

**Not:** Eğer var kullanacaksak tanımladığımız satırda değer ataması yapmalıyız çünkü daha sonra değer ataması yaparsak derleyici değişken tipini bilmediği için hat verir.

**Not:** Tek satırda birden çok değişken tanımlana ifadelerde kullanılmaz.

**Not:** var değişkene null ataması ilk satırda yapılmaz null ataması ilk satır harici ve pirimitive olmayan var değişkenlerde yapılır. Çünkü pirimitivlere null ataması yapılmaz.

**Not:** var isminde bir değişken tanımlayabiliriz çünkü reserved word değildir fakat bir type yani sınıf, interface ya da enum tanımlayamayız çünkü var Java'dfa reserved typedır.

## Variable Scope

```java
public double calculateBMI(int height , int weight) {
double bmi=weight/height*height;
return bmi;
}
```

Yukarıdaki metodda bmi,height ve weight local değişkenlerdir. Ve metot scope'una shiptirler.

Metot içerisndeki başka kod blokları kendi scopuna sahiptirler. Her `{...}` arası bir scoptur. Scopun dışındaki bir değişkene erişmek istediğimizde derleyici `Cannot find symbo`l hatası verir.

## Class Scope

Instance variable nesne içerisinde tanımlıdır tanımlandığı yerde başlar ve nesnenin yaşam süresince o da yaşar. Sınıf değişkenleri ise tanımlandığı yerde başlar ve programın yaşam süresince hayattadır.

## Nesneleri Yok Etme

* Garbage Collection Java tarafından otomatik yapılır.
* Tüm Java nesneleri `heap` bellekte saklanır.
* Çöp toplama programcının kontrolunde olsa programcı hafızayı kontrol etmek zorundadır çünkü eğer heap dolarsa program durur. `OutOfMemory Exception. `
* Ya da programcı referanssız nesneleri silmezse hafızada sensitive veriler kalabilir bu da güvenlik açıklarına sebep olur.
* Çöp toplama için birçok algoritma mevcuttur.
* `System.gc(); ` metodu garbage collectoru çağırır fakat çöp toplama işlemini garanti etmez kontrol her zaman JVM dedir.
* JVM OutOfMemory olmadan çöp toplamayı hedefler.









