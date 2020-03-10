---
title: Bilgisayar Bilimine Giris
date: Thu 5-Dec-2019 11:10 P.M.
language: Turkish

categories: [CS,Intro]
tags: [computer science, bilgisayar bilimine giriş, algorithms, pseudocode, algoritma,
  sözde kod]
seo:
  date_modified: 2020-02-18 00:08:57 +0100
---

## Bilgisayar Bilimine Giriş

Bilgisayar biliminin amacı temel olarak problem çözmektir.Problem çözmek için programlama ve başka teknikler kullanılır.Problem çözmeyi sorun hakkında bazı **inputlar** alarak probleme çözüm olacak **output** değerleri üretme işlemi olarak düşünebiliriz.İputların çözüme yönelik outputlara dönüşmesi için gerekli teknikler bilgisayar biliminin ilgi alanına girer. Bilgisayar biliminde giriş ve çıkışların  bazı standart yollarla ifade edilmesi gerekir.Bilgisayar verileri ikili formatta saklar.İkili formatta yalnızca 2 basamak vardır.Bunlar 0 ve 1 dir ve bu durum bilgisayarımızın elektriği nasıl kullanıldığı ile alakalıdır elektrik devrelerinde iki durum vardır açık ya da kapalı.


## Sayı Sistemleri

Örnek olarak biz insanlar 123 ifadesi "yüz yirmi üç olarak" biliriz. **Bu ifadeyi bilgisayar binary olarak anlar.123 ifadesi binary formatta 1111011 dir..Bilgisayarda tek bir binary değere 1 bit denir.**1 bit verileri göstermek için kullanacağımız en küçük birimidir.Bitleri kolay yönetmek için bayt kavramı kullanılır.**8 bit 1 bayt eder.**1 byte ile 0 ile 255 arasındaki herhangi bir sayıyı saklayabiliriz. Yani 256 adet 0 ve 1  kombinasyonundan oluşan sayı seçeneğimiz vardır. 

Bir byte’ın neden 8 bit olarak belirlendiğinin sebebi bilgisayarda bir metin belgesi saklarken ,8 bitin kullanılacak olası her dil karakterine karşılık gelebilecek tekil bir sayı atamak için yeterli olduğu düşüncesidir.Sayıların binary sisteme dönüşümünü anlatmış olduk fakat Ya harfler?

Bilgisayarların harfleri anlaması için harflerin sayı karşılığına ihtiyaç vardır.Bu işlem için **ASCII** adı verilen standart oluşturulmuştur.Ascii ‘de A sayısı 65 , B sayısı 66 değerine sahiptir.**ASCII** (İngilizce: **A**merican **S**tandard **C**ode for **I**nformation **I**nterchange, Türkçe: **Bilgi Değişimi İçin Amerikan Standart Kodlama Sistemi**) Latin alfabesi üzerine kurulu 7 bitlik bir karakter kümesidir. İlk kez 1963 yılında ANSI tarafından standart olarak sunulmuştur. 
ASCII'de 33 tane basılmayan kontrol karakteri ve 95 tane basılan karakter bulunur. Kontrol karakterleri metnin akışını kontrol eden, ekranda çıkmayan karakterlerdir. Basılan karakterler ise ekranda görünen, okuduğumuz metni oluşturan karakterlerdir. 

Bilgisayar programları, kodunun içeriğine dayalı olarak, ikili sayıların sayı mı, harf mi, yada resim mi olduğunu bilir.

Ascii standardına ek olarak olarak harfleri çoklu baytlar halinde göstermek için **Unicode** standartı geliştirilmiştir.Örnek verecek olursak bir emoji mesajı aldığımızda aslında almış olduğumuz decimal  128514 gibi bir sayıdır.(binary olarak 11111011000000010 sayısıdır.)

Bilgisayar tüm resimleri de binary olarak algılar.Örneğin bir manzara resmi milyonlarca pixelden oluşmuş olabilir.Herhangi bir resmi zoom yaparak büyütürsek pixellerini görebiliriz.
Her pixel küçük bir renk karesidir.Bilgisayar 3 temel rengi kullanarak diğer renkleri oluşturur.Bu temel renkler kırmızı , mavi ve yeşildir.Tüm diğer renkler bu renklerden oluşur.Mesela sarı renk bu 3 rengin birleşiminden oluşur.Yani bu renklerin byte değerleri kullanılarak milyonlarca renk kombinasyonu üretebiliriz.Yani bir resim bilgisayar tarafından sayı olarak algılanır.

Her bir video resimlerin ard arda hızlı bir şekilde sıralanması ile oluşur.Bu yüzden temelde bir video da bilgisayar tarafından milyonlarca bitten oluşan bir sayı olarak algılanır..Sonuç olarak videoları resimler üzerinden,resimleri pixeller üzerinden , pixelleride bitler üzerinden soyutlayabiliriz.

## Algoritmalar

Şimdi başta bahsettiğimiz problem çözümünde inputlasın standartlaştırılıp soyutlanmasını anlatmış olduk.Problem ile alakalı inputları bilgisayarın anlayacağı formata dönüştürmüş olduk.Sırada problemi adım adım adım çözmek var.

Problem çözümüne yönelik adımlara **algoritma** denir.Algoritmalar bir çok farklı şekilde ifade edilebilir.Bir problem çözümünü sözel olarak da ifade edebilriz , matematiksel semboller ile yada şekiller ile de gösterebiliriz.Bir algoritnma örneği verecek olursak; örneğin arkadaşımız Fatih’in ismini telefon defterinden bulmak istiyoruz problemimiz bu bunu algoritmik olarak çözüme ulaştırmamız gerekir.

Çözüm olarak şunları yapabiliriz;

1. Kitabın başından sonuna doğru aradığımız ismi bulana kadar sayfa çevirmek.
2. Aradığımız ismi bulana kadar 2 sayfa çevirmek.
3. Kitabı tam ortasından ikiye ayırmak ve aradığımız ismin kitabın sağındamı yoksa solundamı olduğuna bakma ve doğru tarafa geçip kalan kısmı ortadan ikiye ayırmak ve aradığımız kelimenin kitabın sağında mı yada solunda mı olduğuna bakmak bu işlemi aradığımız kelimeyi bulana kadar devam ettirmek.

3.yol en efektif yoldur ve bu algoritma en hızlı çalışan algoritmadır.Algoritmaları grafik üzerinde gösterecek olursak;

![Image of algorithms graphic](/assets/img/posts/algorithms.png)

Turuncu grafik 1. algoritma, kırmızı 2. algoritma, yeşil olan 3. algoritma içindir.

## Pseudocode (Sözde Kod)

Şimdi sözel olarak belirttiğimiz çözüm adımını bir derece daha ileriye taşıyalım.Bu noktada **pseudocode** kavramına değineceğiz şöyle ki;
Pseudocode algoritmamızı kodlarken ilk adımımızdır.Bu adımda insan dili ile adım adım algoritmamızı kodluyoruz yazdığımız bu kodlara **pseudocode( sözde kod )** denir.

```
 0 Telefon defterini al
 1 Defterin en ortasını aç
 2 İsimlere bak
 3 Fatih isimler arasında ise
 4     Aliyi çağır
 5 Fatih kitabın sol tarafında ise
 6     Sol tarafı ikiye 0 Telefon defterini al
 1 Defterin en ortasını aç
 2 İsimlere bak
 3 Fatih isimler arasında ise
 4     Aliyi çağır
 5 Fatih kitabın sol tarafında ise
 6     Sol tarafı ikiye böl 
 7     2. adıma git
 8 Fatih kitabın sağ tarafında ise
 9	Sağ tarafı ikiye böl     
10    	 2. adıma git
11 Fatih isimler arasında değilse
12    Kitabı kapat. 
10    	 2. adıma git
11 Fatih isimler arasında değilse
12    Kitabı kapat.

```

Dikkat ettiyseniz sözde kodumuzda bazı yapılar kullandık bunlar;

```
Şart ifadeleri ; ise,değilse…
Emir kipleri; Aç,kapat,bak,çağır,böl bunlar fonksiyonlardır.
Mantıksal ifadeler;kitabın sol tarafında ise,sağ tarafında ise
Döngüler;2. adıma git

```

Gördüğünüz üzere sözde koddan asıl koda doğru bir adım daha attık.Yukarıda bahsettiğimiz kavramlar her programlama dilinde vardır.

```
Şart ifadeleri ; if-else
Emir kipleri;function(),method() 
Mantıksal ifadeler;true,false
Döngüler;While,for
```



Bu adımdan sonra problemimizin çözümü için en uygun programlama dilini seçip kodlamaya geçebiliriz.

**Kaynaklar**

[https://online-learning.harvard.edu/course/cs50-introduction-computer-science](https://online-learning.harvard.edu/course/cs50-introduction-computer-science)

[http://www.plainenglish.info/Computer+Science/Computer+Architecture](http://www.plainenglish.info/Computer+Science/Computer+Architecture)

[https://tr.wikipedia.org/wiki/ASCII](https://tr.wikipedia.org/wiki/ASCII)










