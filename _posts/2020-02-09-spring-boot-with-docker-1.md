---
title: Spring Boot App With Docker
date: Sun 9-Feb-2020 05:32 P.M.
categories: [Blogging, Backend, Spring, Docker]
language: Turkish
tags: [spring, java, docker, spring boot]
seo:
  date_modified: 2020-02-11 00:33:48 +0100

---

#### Spring Boot İle Docker
Merhabalar, yeni bir yazı serisine başlıyorum.Bu seride Spring Boot ve Docker kullanarak uygalamalarızı nasıl geliştirebileceğimizi anlatacağız.Genel hatları ile konu sıralamamız aşağıdaki şekilde olacak.

* Docker'a giriş
* Spring Boot web uygulamamızı konteyner içine almak.
* MongoDB konteyner oluşturma.
* Uygulama ve veri tabanı konteynerlarını entegre etme.
* Docker Compose ile konteynerlarımızı yönetme.


#### Docker'a Giriş

Docker 2013 yılında piyasaya sürülen ve işletim sistemi seviyesinde sanallaştırma yapan bir yazılımdır.Docker kurulumu ve komutlarına geçmeden önce neden Docker kullandığımıza ve konteynerlara değinelim.

#### Konteyner Nedir
Docker'ın resmi sitesindeki tanımlardan faydalanacak olursak.
Yazdığımız kodları paketleyip farklı geliştirme ortamları arasında paylaşmak için gerekli standart birime **Container** denir.Daha kolay anlaşılması için bir case üzerinden anlatacak olursak.Spring Boot ve MongoDB kullanarak geliştirdiğimiz web uygulamasını test ekibine verdiğimizde sorunsuz çalışması için bizim bilgisayarımızdaki environment ile test edecek kişinin bilgisayarındaki environment tüm özellikleri ile aynı olmalı.Kullandığımız JDK 'nın sürümünden MongoDB versiyonuna kadar tüm environment aynı olmalı ki sağlık bir şekilde çalışsın.İşte developerları bu sıkıntılı durumdan kurtarmak için **Container** çözümlerine başvurulur.

Bir Docker **Container Image** kodun sağlıklı bir şekilde çalışması için gerekli tüm bağımlılıkları içerir.

Docker Image'leri çalışma zamanında Docker Container'larına dönüşür.Hem Linux hemde Windows tabanlı yazılımlarda kullanılabilir olan Container yapısı altyapıdan bağımsız olarak aynı çalışır.Docker Container' lar **Docker Engine** üzerinde çalışırlar. 

#### Sanal Makine( Virtual Machine ) Nedir  

Docker işletim sistemi düzeyinde sanallaştırma sağlar dedik.Bu durumu açıklamadan önce sanallaştırmanın (**Virtualization**) ne olduğuna ve sanal makinelere (**Virtual Machine**) değinelim.

Sanal makineler (VM) var olan doananım alt yapısı üzerine kurulan yazılımsal bilgisayarlardır.Sanal makineler kendine ait ve kurulum esnasında tanımlanan bellek, depolama ,işletim sistemi gibi niteliklere sahiptirler.Nasıl ki fiziksel bilgisayarımıza işletim sistemi ve uygulama kurabiliyorsak sanal makine üzerine de kurabiliriz.Örnek verecek olursak Windows bir makine kullanıyoruz ve Linux'u da deneyimlemek istiyoruz bu durumda ne yapabiliriz;Ya windowsun yanına ikinci bir işletim sistemi olarak Linux kurarız yada Windows içine belli ölçüde bellek ve depolama alanı vererek bir sanal makine kurup Linux işletim sistemini yükleriz.

Peki neden sanal makinelere ihtiyaç duyarız;Sanal makine sistem içerisinde korunmuş bir alan oluşturur ve sistemin geri kalanı bu alanda olan hiçbirşeyden etkilenmez.Örnek verecek olursak virüs bulaşmış dosyaları ayılkamak için yeni bir sanal makine kurup dosyaları inceleyebiliriz bu durumda virüs ana sistemimize ulaşmaz.

Ayrıca sanal makineler server sanallaştırması içinde kullanılabilir.Mesela elimizde bir Linux Server var ve biz bu serverda Windows için geliştirilen bir yazılımı host etmek istiyoruz bu durumda ihtiyacımız olan bir sanal makine kurup Windows çalıştırmaktır.

#### Hypervisor Nedir

Bir işletim sistemi üzerinde birden fazla VM çalışabilir.Bu durumda ana işletim sitemine **host** üzerinde çalışan herbir VM' yede **guest** denir.Host üzerindeki guest' lerin yönetimini sağlayan yzılıma **Hypervisor** denir.Hypervisor olarak kullanılan yazılımlara örnek vercek olursak;

* OracleVM
* VMware
* Microsoft Hyper-V

örnek olarak verilerbilir.

Gelelim en can alıcı noktaya VM'ler varsa ve çok kolaylık sağlıyorsa Docker' a neden ihityaç duyalım?

Docker' ın işletim sistemi düzeyinde bir sanallaştırma teknolojisi olduğunu söyledik.Bu nasıl oluyor.Aşağıdaki görsele bakacak olursak.

![Image of algorithms graphic](/assets/img/posts/docker-vs-vm.png)

Docker direkt olarak Host işletim sistemi üzerinde çalışır.Bu da bellek ve depolama olarak Docker'ı ön plana çıkarmaktadır.VM'lerin ihtiyacı olan bellek,depolama sistem için büyük bir yük olyuştururken Docker ile bu durum minimalize edilmiştir.

Bu yazının kapsamı Docker'a giriş niteliğinde olduğundan çok fazla detaya girmeden Docker'ı nasıl yükleriz ve kullanırız buna bakalım.Docker'ın detaylarını daha geniş bir yazı dizisine havale ederek Docker kullanımına geçelim.

Docker'ın çok sade ve kolay bir dökümanı var kurulum adımlarını [bu dökümana](https://docs.docker.com/install/) havale ederek biz Docker kullanımına geçiş yapalım.

Docker'ı indirip bilgisayarınıza kurduktan sonra terminalde aşağıdaki komutu yazarak başarılı bir şekilde çalışıp çalımadığını kontrol edebilirsiniz.

```
docker -v
```
Bu komutu yazıp entera bastığımızda Docker'ın versiyon numarası ve build numarasını içeren bir çıktı gelmesi lazım.

![Image of algorithms graphic](/assets/img/posts/docker-v.png)





#### Kaynaklar

https://www.docker.com/resources/what-container

https://www.vmware.com/topics/glossary/content/virtual-machine

https://phoenixnap.com/kb/what-is-hypervisor-type-1-2

https://docs.docker.com/install/





