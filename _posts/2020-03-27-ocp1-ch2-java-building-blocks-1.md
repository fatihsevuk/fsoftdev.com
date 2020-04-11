---
title: OCP-I Chapter2 Java Building Blocks-I
date: Fri 27-Mar-2020 07:46 P.M.
categories: [JavaSE, OCP]
language: Turkish
tags: [ocp, oracle java programmer, javase, java standart edition,
java]
---

## Giriş

Merhabalar OCP yazı dizimize kaldığımız yerden devam ediyoruz. Bu yazımızda Java'da nesne oluşturma, veri türleri, değişken tanımlama gibi kavramları öğreneceğiz.

## Nesne Nedir

Java'nın nesne yönelimli bir dil olduğunu ve tüm kodlarımızın sınıflar içerisine yazıldığı öğrenmiştik. Java sınıflarımızı kullanmak için onlardan nesneler oluştururuz. Bir Java sınıfından(**class**) üretilen nesne(*object*) o sınıfın bir örneğidir(**instance**).

Java'da nesne oluşturmak için **new** anahtar kelimesini kullanırız.

```java
Robot mrRobot=new Robot();
```
Bu tanımlamada
* Robot oluşacak nesnenin türüdür.
* mrRobot nesnemizin referansıdır,değişken ismimizdir.
* Robot() ifadesi yapılandırıcı(**constructor**) metodumuzdur.

## Constructor( Yapılandırıcı ) Nedir

Sınıflardan nesne oluşturmamızı sağlayan özel bir tür metottur. Constructor;

* Sınıf ismi ile aynı isme sahip olmalı
* Herhangi bir değer dönmemeli

Constructor'ın temel amacı nesne oluşturmamıza olanak sağlamasıdır aynı zamanda sınıfın alanlarına(**fields**) ilk değer vermemize imkan tanır.

```java

public class Robot {
  String name;
  int power;

  public Robot() {
    name="Mr Robot";
    power=100;
  }

  public void robotInfo(){
    System.out.printf("My name is: %s \n",name);
    System.out.printf("I have %d percent power \n",power);
  }
}

```

`Robot.java` sınıfımızı inceleyecek olursak `name` ve `power` diye iki `field`'a , `robotInfo` isminde bir metoda sahiptir. Ayrıca field'lara ilk değerlerinin atandığı bir constructor'a sahiptir.

Bu sınıfımızdan nesne oluşturup ve robotInfo metodu yardımıyla robotumuzun bilgilerini ekrana yazdırmak istiyoruz. Aşağıdaki kodlarımızı inceleyecek olursak.

```java
public class MrRobot {
  public static void main(String[] args){
    Robot mrRobot=new Robot();
    mrRobot.robotInfo();
  }
}

```
`Robot mrRobot=new Robot();` satırında Robot sınıfının constructor'ı ile yeni bir nesne oluşturuyoruz.

`mrRobot.robotInfo();` sınıfımıza ait robotInfo() metodunu çağırıyoruz. Dikkat ettiyseniz constructor içerisinde field'lara ilk değerlerini verdik bu işlemi field'ları tanımlarken de yapabilirdik.

Her sınıfın içi dolu bir yapılandırıcıya sahip olmasına gerek yoktur. Peki böyle sınıflardan nasıl nesne oluştururuz. Cevabı basit Java'da her sınıf içi boş bir `default constructor`'a sahiptir. Yani bu tür sınıflar dan obje oluştururken default constructor çağrılır.

Java'da bazı sınıfların constructor haricinde nesne oluşturmak için built-in metotları vardır.

## Java'da Kod Blokları

Java'da bir kaç farklı kod bloku vardır. Kod blokları süslü parantezler `{ //... Kodlar }` ile tanımlanır. Java'daki kod blokları aşağıdaki gibidir.

* class kod bloku
* metod kod bloku
* metod içi-inner kod bloku
* instance initializer kod bloku - her zaman metotların dışında bulunur.

Kod bloklarının initialization sırası şu şekildedir.

1. **Fields** ve **instance initializer blokları** dosya içinde göründükleri sıra ile çalışırlar.

2. Tüm filed'lar ve instance initializer'lar çalıştıktan sonra **constructor** çalışır.

Aşağıdaki örnek kodlarımızda bu sırayı irdeleyelim.

```java
public class Robot {
  private String name="Base Robot v.121";
  {
  System.out.println("Base Robot is getting ready for cyber war.... ");
  }

  public Robot() {
    name="MrRobot v.2.0";
    System.out.println("New version of MrRobot is constructuring... ");
  }

  public static void main(String... args) {
    Robot mrRobot=new Robot();
    System.out.println(mrRobot.name);
  }

}
```

Yukarıdaki sınıfımızı çalıştırdığımızda çıktı aşağıdaki gibi olur.

```shell
Base Robot is getting ready for cyber war....
New version of MrRobot is constructuring...
MrRobot v.2.0
```

Kodu inceleyecek olursak;
1. İlk önce main metodu çalışır.
2. Daha sonra Robot sınıfından nesne oluşturulur.
3. Program akışı Robot sınıfının field'larını çalıştırır.
4. Sonraki adımda instance initializer blokları çalışır.
5. Son olarak akış Robot sınıfının constructor'ı içerisine girer.

**Not:** Eğer bu sınıfta main metodundan sonra bir kod olsaydı en son o çalışırdı.

**Not:** Program akışında bir değişken tanımlanmadan önce kullanılmaz.

## Java'da Veri Türleri (Data Types)

Java'da hafızada değişik türlerde verileri tutarız, farklı türlerde ve boyutlarda verileri tutmak için Java'da 8 tane built-in veri tipi vardır. Bunlara `primitive type` denir.

Programlama dünyasında verilerimiz genelde aşağıdaki formlarda olurlar.

* Mantıksal veriler
* Sayısal veriler
  * Tam sayılar
  * Noktalı sayılar
* Metinsel veriler

Yukarıda saydığımız bu veri türlerinin Java dilindeki karşılıkları aşağıdaki gibidir.

1. **boolean** - 8 bit - mantıksal veriler (true/false)
2. **byte** - 8 bit - tam sayılar
3. **short** - 16 bit - tam sayılar
4. **int** - 32 bit - tam sayılar
5. **long** - 64 bit - tam sayılar
6. **float** - 32 bit - noktalı sayılar
7. **double** - 64 bit - noktalı sayılar
8. **char** - 16 bit - unicode

**NOT:** Java'da metinsel verileri String türünde turulur fakat String bir primitive tür değildir, nesnedir.

**NOT:** Float değerler sonuna `f harfi` almalıdır yoksa Java bunların `double` olduğunu da düşünebilir ve fazladan alan ayırabilir.

**Not:** Java'da sayısal veri tipleri işaretlidir. Yani bir biti işaret bitidir.

> İşaretli ve işaretsiz sayıları anlamak için örnek vermek gerekirse; Java dilinde `char` 16 bit uzunluğunda işaretsiz sayıları tutar buna 0 da dahildir yani char sadece pozitif değer alabilir.
>16 bit uzunluğunda verileri tutan bir başka veri tipide `short` tur. Short integral değer olarak hem negatif hem pozitif değerler alabilir. Pozitif olarak `char` `short` dan daha büyük değerler alabilir. Bu iki veri tipi birbiri yerine kullanılabilir. Tabi ki değer aralıkları göz önünde bulundurulmak koşuluyla.
>Aşağıdaki kod örneklerini inceleyecek olursak mesele daha da açıklığa kavuşacaktır.

```java
public class ShortCharUsing{
  public static void main(String... args) {
    short sValue='e';
    char cValue=(short)74;
    System.out.println("sValue: "+sValue);
    System.out.println("cValue: "+cValue);
  }
}
```
Kodumuzu incelediğimizde ``'e'`` nin `ASCII` tablosundaki değeri `sValue` değişkenine atanır.

`ASCII` tablosunda ``74`` e karşılık gelen harf yani `J` değeri ise `cValue` değişkenine atanır.

Kodumuzun çıktısı aşağıdaki gibi olur.

```shell
sValue: 101
cValue: J

```
Evet bu kullanımda aralıkları göz ardı etmemeye dikkat etmeliyiz. Mesela `cVlaue` ya aşağıdaki gibi bir atama yaparsak

```java
char cValue=(short)-4;
```
aşağıdaki gibi bir hata ile karşılaşırız.

```shell
ShortCharUsing. java:4: error: incompatible types: possible lossy conversion from short to char
char cValue=(short)-4;
^
1 error
error: compilation failed

```
Bu hatadan anlamamız gereken char tipi negatif değer almaz. Ek olarak eğer short değişkene -32768 ve 32767 aralığı dışında bir değer verirsek

```java
short sValue=45000;
//yada short sValue=-32769
```
yukarıdaki ile aynı hatayı alırız.

```shell
ShortCharUsing. java:3: error: incompatible types: possible lossy conversion from int to short
short sValue=45000;
^
1 error
```
Bu hatalara genel olarak `Incompatible types error` diyebiliriz.

**Not:** Java'da veri tiplerinin kaç bit uzunluğunda değer alacaklarını biliriz burdan yola çıkarak veri tipinin hangi, aralıkta olduğunu bulabiliriz. Short tipi 16 bit uzunluğundadır. Yani 2^16 tane değer alır.

Yani **short** `2^16 = 65536` değer alır. Decimal aralık olarak

`[ -32768 ....... 0 .......... 32767 ] `aralığında değer alır. Sıfır dahildir ve bu tipin varsayılan değeri 0 dır.

Bilgisayar sistemlerinde ondalıklı sayılar bilimsel notasyonda tutulurlar. Bilimsel notasyon hakkında bilgi almak için aşağıdaki bağlantıya tıklayabilirsiniz.

[Noktalı sayıların bilimsel notasyonla ifade edilişi](https://en.wikipedia.org/wiki/Scientific_notation)

