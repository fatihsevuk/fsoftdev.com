---
title: Internet Nasıl Çalışır
language: Turkish
date: Wed 4-Dec-2019 03:23 P.M.
categories: [CS, Intro]
tags: [internet, domain, ip]
seo:
  date_modified: 2020-03-10 18:13:28 +0100
---

## Giriş

İnternet Web’in omurgasıdır. Web’i olası kılan teknik alt yapıdır. En temelde internet birbirleriyle iletişim halindeki bilgisayarların geniş ağıdır.İnternetin tarihi biraz belirsizdir . 1960'larda Amerikan ordusu tarafından finanse edilen araştırma projelerinin daha sonra 1980'lerde birçok üniversite ve özel şirketin desteklemesi ile genel bir altyapıya dönüşümünü içerir.

İnternete bağlı cihazların zaman içerisinde değişimi interneti değiştirmedi.İnternet en temelde bu bilgisayarların birbirleriyle olan bağlantısı ve bu bağlantının ayakta kalmasını sağlayan yapıdır.

## Basit Bir Ağ

Eğer iki bilgisayarı birbirine bağlamak istersek bunu iki şekilde yapabiliriz birinci yol bu iki bilgisayarı kablo ile birbirine bağlamak ikinci yol ise bu iki bilgisayarı kablosuz bir şekilde birbirine bağlamak günümüzde modern bilgisayarlar bu iki bağlantı şeklini de desteklemektedir.

Ama bir bilgisayar ağı genellikle iki bilgisayarla sınırlı değildir.İstediğiniz kadar bilgisayarı bağlayabilirsiniz fakat bilgisayar sayısı arttıkça Networkte karmaşık hale gelecektir. Eğer 10 bilgisayarı birbirine bağlamak istiyorsanız 45 kablo gerekir.

## Ağların Ağı

Şimdi bilgisayar sayısı arttıkça bağlantının ne kadar karmaşık hale gelebileceğini gördük bu problemi çözmek için **Router** denilen özel küçük bilgisayarlar üretilmiştir. Routerların yalnızca bir görevi vardır buda  kaynak bilgisayardan gelen mesajın hedef bilgisayara doğru bir şekilde iletilmesini sağlamaktır Eğer A bilgisayarı B bilgisayarına bir mesaj göndermek istiyorsa yapması gereken mesajı routera göndermektir router mesajı alıp doğru bilgisayara göndermekle görevlidir. Router olmadığı zaman 10 bilgisayarı birbirine bağlamak için 45 kablo gerekmekteydi fakat Router olduğu zaman 10 bilgisayar için 10 kablo gerekir.

Peki gelin şimdi yüzlerce binlerce ve milyonlarca bilgisayarın birbirine bağlandığı yapıları düşünelim. Bu kadar çok cihazı bir Router ile birbirine bağlamak zordur fakat yukarıda da bahsettiğimiz gibi router bir bilgisayardır ve başka bir bilgisayarla bağlantı kurabilir yani yüksek sayıdaki bilgisayarları birbirine bağlamak için iki routerı da birbirine bağlayabiliriz.

Bilgisayarları routerlara , routerlarıda başka routerlara bağlayarak networku sınırsız bir şekilde ölçekleyebiliriz. Bu bahsettiğimiz yapı internet dediğimiz yapıya oldukça yaklaştı fakat bir şeyler eksik kaldı.Biz kendi amaçlarımız için bir ağ oluşturduk fakat dışarıda arkadaşlarımızın komşularımızın ya da başka birilerinin sahip olduğu başka ağlar da var ve biz bu ağlara bağlı değiliz.Kendi evimizden ya da işyerimizde oluşturduğumuz ağın diğer insanların ağlarıyla olan iletişimini nasıl sağlayacağız?

Evlerimizde zaten var olan telefon kabloları var ve telefon altyapısı zaten bizi dünya üzerinde herhangi birine bağlayabiliyor bu yüzden internet ağlarını birbirlerine bağlamak için gerekli olan altyapı telefon alt yapısıdır. Bilgisayar ağlarımızı telefon altyapısına bağlamak için **Modem** diye adlandırılan ekipmanlara ihtiyacımız vardır.Modem bilgisayar ağından gelen bilgiyi telefon ağı tarafından yönetilebilir bir bilgiye dönüştürür ya da tam tersini yapar.

Evet şimdi telefon altyapısına bağlandık bir sonraki adımımız kendi ağımızdan istediğimiz herhangi bir ağa mesaj göndermek bunu yapmak için kendi ağımızı bir **İnternet Servis Sağlayıcısına(ISP)** bağlayacağız. ISP birbirine bağlı özel routerları yöneten aynı zamanda başka internet servis sağlayıcılarının routerlarına ulaşan bir şirkettir. Bizim ağzımızdan çıkan mesaj internet servis sağlayıcısı ağları üzerinden hedef ağa ulaşır işte internet bütün bu ağ alt yapısından oluşur.

## Bilgisayarları Bulmak

Eğer bir bilgisayara bir mesaj göndermek istiyorsak o bilgisayarın hangisi olduğunu tanımlamak zorundayız.Bu yüzden network'e bağlı her bilgisayar **IP Adresi** olarak adlandırılan tekil bir adrese sahiptir.IP adresii noktalarla ayrılmış 4 sayı grubunun birleşiminden oluşur.**192.168.2.10 örnek bir IP Adresidir.**

İnsanlar için bu sayıları akılda tutmak veya hatırlamak oldukça zordur. Bu yüzden bu IP Adresine daha kolay hatırlayabileceğimiz bir isim veririz. Bu isme **Alan adı(domain name)** denilir.**Google.com örnek bir alan adıdır.**

## İnternet ve Web

Dikkat ederseniz bir web sitesini web üzerinde aramak için onun alan adını kullanırız. Peki internet ve web aynı şeyler midir?Açıklayalım.Anlatıldığı üzere internet milyonlarca bilgisayarın birbirine bağlanmasına ve bilgisayarlar arasında doğru bir şekilde mesaj iletimine olanak sağlayan teknik bir alt yapıdır.Bu bilgisayarlar arasında **Web Server** olarak adlandırılan bilgisayarlarda vardır. Bu web serverlar web tarayıcılarına anlaşılır mesajlar gönderirler.Sonuç olarak internet teknik bir alt yapıdır.Web ise bu altyapının üstüne inşa edilmiş bir servistir. İnternet üzerinde webe benzeyen birçok servis vardır bunlardan bazıları **email servisi ve IRC dir.**

**Kaynaklar**

[https://developer.mozilla.org/en-US/docs/Learn/Common_questions/How_does_the_Internet_work](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/How_does_the_Internet_work)


