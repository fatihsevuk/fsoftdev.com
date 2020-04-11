---
title: OCP-I Chapter 1 Welcome to Java-III
date: Mon 30-Dec-2019 06:19 P.M.
categories: [JavaSE, OCP]
language: Turkish
tags: [package, paket, ocp, oracle java programmer, javase, java standart edition,
  java, jar, classpath]
seo:
  date_modified: 2020-03-20 18:32:27 +0100
---

## Java'da Paket Yapısı

Java programlama dilinde sınıfları mantıksal olarak sınıflandırmamıza yarayan yapılara paket denir.Java , içerisinde binlerce built-in paket ile gelir.JDK' ya ait paketler **java** ve **javax** ile başlar.Bir paketi kodumuza ekleyip o paketteki sınıfları kullanabilmemiz için ilgili paketi kodumuza **import** etmemiz gerekmektedir.Kullandığımız herhangi bir sınıfın paketini import etmezsek kodumuz hata verir.Java da import edilmeden kullanılan tek sınıf **System** sınıfıdır.Ekrana bir satır basmak için kullanılan **System.out.println();**  ifadesindeki System sınıfı **java.lang** paketindedir ve bu paketi dışarıdan import etmemize gerek yoktur bu paket otomatik olarak import edilir. 

Paket yapısı ile ilgii kod örneğimizi inceleyecek olursak;

```java
import java.util.ArrayList;

public class MrRobot { 
    
    public static void main(String... args) {
        ArrayList<String> names = new ArrayList<String>();
        System.out.println(names.toString());
    }
}
```

kodumuzda main metodu içerisinde ArrayList sınıfından bir nesne oluşturduk dikkat edecek olursanız sınıf tanımının üstünde ArrayList sınıfını import ettiğimizi görürsünüz.Eğer bu import etme işlemini yapmazsak kodumuz hata verir.

Java'da bir paket altındaki tüm sınıfları koda import etmek mümkündür bunu yapmak için * operatörünü kullanırız.Yukarıdaki kodda java.util paketi altındaki tüm sınıfları import etmek için aşağıdaki ifade kullanılır.

```java
import java.util.*;
```

Burda dikkat etmemiz gereken en önemli husus şudur ki; * operatörü paket içerisindeki sınıfları import eder alt paketleri koda dahil etmez.

## İsim Çakışmaları

Farklı paketler içerisnde aynı isimde paketler bulunabilir.Bu gibi durumlarda **conflict(çakışma)** olmaması için kullanılacak sınıfın hangi paket altında olduğu açıkça belirtilmeli derleyici hangi paket olduğunu anlamalıdır.Kod üzerinden örnek verecek olursak;



```java


public class MrRobot { 
    Date birthdate;
}
```

kodumuzda Date türünde bir alan vardır fakat bu Date sınfının hangi pakette olduğu belli değildir.Date isminde sınıf içeren iki paket vardır bunlar **java.util.Date** ve **java.sql.Date** dir.

Kodumuzu aşağıdaki şekilde değiştirirsek neler olur gelin beraber inceleyelim;

```java

import java.util.Date;
import java.sql.Date;

public class MrRobot { 
    Date birthdate;
}
```
bu durumda derleyici hangi paketteki Date sınıfını kullanacağına karar veremez ve aşağıdaki hatayı fırlatır.

    error: reference to Date is ambiguous

Peki bu durumda ne yapmamız gerekir bu durumda Java ya hangi paketi kullanması gerektiğini açıkça bildirmemiz gerekir yani hangi paketteki Date sınıfını kullanmamız gerektiğine karar verip diğer import ifadesini kaldırmalıyız.

Bir başka çözüm adımıda şu olabilir;


```java

import java.util.*;
import java.sql.Date;

public class MrRobot { 
    Date birthdate;
}
```
gelin beraber düşünelim acaba Java burada hangi paketteki Date sınıfını kullanacak? Derleyici açıkça belirtilmiş sınıf ismini * karakterine göre önceliklendirir.Yani **java.sql** paketindeki Date sınıfı kullanılır.

Gelin durumu biraz daha zorlu hale getirelim diyelim ki programcı her iki paketteki Date sınıfınıda kodunda kullanmak istiyor bu durumda ne yapması gerekir? Cevabımızı kod üzerinden anlatacak olursak;

```java

import java.sql.Date;

public class MrRobot { 
    Date birthdate;
    java.util.Date otherDate;
}
```

yada 


```java


public class MrRobot { 
    java.sql.Date birthdate;
    java.util.Date otherDate;
}
```

yukarıdaki iki kod bloğuda sorumuzun cevabıdır.

## Yeni Paket Oluşturmak

Eğer paket tanımlamazsak kodumuz default package içindedir.Yeni paket oluşturmak için aşağıdaki yapı kullanılır.

    package package_name;


## Paket İle Çalıştırma

Paket içierisndeki kodlarımızı javac ile derlemek için;

    javac packageName/MrRobot.java


bir paket altındaki tüm sınıfları derlemek için

    javac packageName/*.java


komutları kullanılır.

## Alternatif Derleme Klasörü

Varsayılan olarak kaynak kodlar ve derlenmiş kodlar aynı klasör içindedir.Derleme sırasında derlenmiş kod için yeni bir yol belirtebiliriz bu işlem aşağıdaki gibi yapılır.

    javac -d alternateDirectory package/MrRobot.java

derleme sonucu üretilen .class dosyaları alternateDirectory içerisine yerleştirilir.


Derleme sonucu oluşan .class dosyalarını kullanıp programımızı çalıştırma aşamasında alternatif klasörün yolunu java ya bildirmemiz gerekir bunu 3 farklı şekilde yapabiliriz.

    java -cp alternateDirectory package.MrRobot
    java -classpath alternateDirectory package.MrRobot
    java --class-path alternateDirectory package.MrRobot

yukarıdaki üç komutta aynı işlemi yapar.


## Jar Dosyasıyla Birlikte Derleme

Derleme işlemimize herhangi bir jar dosyasını dahil edebiliriz bunu Linux işletim sisteminde aşağıdaki şekilde yaparız.

    java -cp ".:/temp/otherLocation:/temp/myJar.jar" package.MrRobot

yukarıdaki kodda : ile ayrılmış kısımlara dikkat edecek olursak;

* derleme işlemine **/temp/otherLocation** içindeki tüm jar dosyaları alınır fakat alt klasörler alınmaz

* aynı zamanda **/temp/myJar.jar** dosyasıda derleme işlemine dahil edilir.

## Yeni Jar Dosyası Oluşturma

Aşağıdaki komutlar yardımı ile jar dosyaları oluşturabiliriz.

    jar -cvf myNewJar.jar

    jar --create-verbose-file myNewJar.jar

herhangi bir klasör içine jar dosyası oluşturmak için

    jar -cvf myNewJar.jar -C directory

## Paket ile Birlikte Tek Satırda Derleme

Eğer programımızı **javac** kullanmadan tek satırda **java** java komutu ile derlemek istiyorsak tek şart şudur;Programımız JDK paketleri hariç başka bir paketi import etmemelidir.

## Sınıf Elemenlarını Sıralama

1. package
2. imports
3. class decleration
4. fields
5. methods

1,2,3 sıralaması şarttır 4 ile 5 nolu elemanlar yer değiştirebilir.

OCP Chapter 1 sonuna geldik bir sonraki chapterda Java yapı bloklarını anlatarak devam edeceğiz.