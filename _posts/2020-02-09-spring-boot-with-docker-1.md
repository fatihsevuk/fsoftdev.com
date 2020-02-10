---
title: Spring Boot App With Docker-1
date: Sun 9-Feb-2020 05:32 P.M.
categories: [Blogging, Backend, Spring, Docker]
language: Turkish
tags: [spring, java, docker, spring boot]
seo:
  date_modified: 2020-02-11 00:38:53 +0100

---

#### Başlangıç
Merhabalar, yeni bir yazı serisine başlıyorum. Bu seride Spring Boot ve Docker kullanarak uygulamalarızı nasıl geliştirebileceğimizi anlatacağız. Genel hatları ile konu sıralamamız aşağıdaki şekilde olacak.

* Docker'a giriş
* Spring Boot web uygulamamızı konteyner içine almak.
* MongoDB konteyner oluşturma.
* Uygulama ve veri tabanı konteynırlarınıı entegre etme.
* Docker Compose ile konteynırlarımızıı yönetme.

#### Docker'a Giriş

Docker 2013 yılında piyasaya sürülen ve işletim sistemi seviyesinde sanallaştırma yapan bir yazılımdır. Docker kurulumu ve komutlarına geçmeden önce neden Docker kullandığımıza ve konteynırlara değinelim.

#### Konteyner Nedir
Docker'ın resmi sitesindeki tanımlardan faydalanacak olursak.
Yazdığımız kodları paketleyip farklı geliştirme ortamları arasında paylaşmak için gerekli standart birime **Container** denir. Daha kolay anlaşılması için bir case üzerinden anlatacak olursak. Spring Boot ve MongoDB kullanarak geliştirdiğimiz web uygulamasını test ekibine verdiğimizde sorunsuz çalışması için bizim bilgisayarımızdaki environment ile test edecek kişinin bilgisayarındaki environment tüm özellikleri ile aynı olmalı. Kullandığımız JDK 'nın sürümünden MongoDB versiyonuna kadar tüm environment aynı olmalı ki sağlık bir şekilde çalışsın. İşte developerları bu sıkıntılı durumdan kurtarmak için **Container** çözümlerine başvurulur.

Bir Docker **Container Image** kodun sağlıklı bir şekilde çalışması için gerekli tüm bağımlılıkları içerir.

Docker Image'leri çalışma zamanında Docker Container'larına dönüşür. Hem Linux hem de Windows tabanlı yazılımlarda kullanılabilir olan Container yapısı altyapıdan bağımsız olarak aynı çalışır. Docker Container' lar **Docker Engine** üzerinde çalışırlar.

#### Sanal Makine( Virtual Machine ) Nedir

Docker işletim sistemi düzeyinde sanallaştırma sağlar dedik. Bu durumu açıklamadan önce sanallaştırmanın (**Virtualization**) ne olduğuna ve sanal makinelere (**Virtual Machine**) değinelim.

Sanal makineler (VM) var olan donanım alt yapısı üzerine kurulan yazılımsal bilgisayarlardır. Sanal makineler kendine ait ve kurulum esnasında tanımlanan bellek, depolama ,işletim sistemi gibi niteliklere sahiptirler. Nasıl ki fiziksel bilgisayarımıza işletim sistemi ve uygulama kurabiliyorsak sanal makine üzerine de kurabiliriz. Örnek verecek olursak Windows bir makine kullanıyoruz ve Linux'u da deneyimlemek istiyoruz bu durumda ne yapabiliriz; Ya Windows'un yanına ikinci bir işletim sistemi olarak Linux kurarız ya da Windows içine belli ölçüde bellek ve depolama alanı vererek bir sanal makine kurup Linux işletim sistemini yükleriz.

Peki neden sanal makinelere ihtiyaç duyarız;Sanal makine sistem içerisinde korunmuş bir alan oluşturur ve sistemin geri kalanı bu alanda olan hiçbir şeyden etkilenmez. Örnek verecek olursak virüs bulaşmış dosyaları ayıklamak için yeni bir sanal makine kurup dosyaları inceleyebiliriz bu durumda virüs ana sistemimize ulaşmaz.

Ayrıca sanal makineler server sanallaştırması içinde kullanılabilir. Mesela elimizde bir Linux Server var ve biz bu server da Windows için geliştirilen bir yazılımı host etmek istiyoruz bu durumda ihtiyacımız olan bir sanal makine kurup Windows çalıştırmaktır.

#### Hypervisor Nedir

Bir işletim sistemi üzerinde birden fazla VM çalışabilir. Bu durumda ana işletim sitemine **host** üzerinde çalışan her bir VM' yede **guest** denir. Host üzerindeki Guest'lerin yönetimini sağlayan yazılıma **Hypervisor** denir. Hypervisor olarak kullanılan yazılımlara örnek verecek olursak;

* OracleVM
* VMware
* Microsoft Hyper-V

örnek olarak verilerbilir.

Gelelim en can alıcı noktaya VM'ler varsa ve çok kolaylık sağlıyorsa Docker' a neden ihtiyaç duyalım?

Docker' ın işletim sistemi düzeyinde bir sanallaştırma teknolojisi olduğunu söyledik. Bu nasıl oluyor. Aşağıdaki görsele bakacak olursak.

![Image of docker-vs-vm](/assets/img/posts/docker-vs-vm.png)

Docker direkt olarak Host işletim sistemi üzerinde çalışır. Bu da bellek ve depolama olarak Docker'ı ön plana çıkarmaktadır. VM'lerin ihtiyacı olan bellek,depolama sistem için büyük bir yük oluştururken Docker ile bu durum minimalize edilmiştir.

Bu yazının kapsamı Docker'a giriş niteliğinde olduğundan çok fazla detaya girmeden Docker'ı nasıl yükleriz ve kullanırız buna bakalım. Docker'ın detaylarını daha geniş bir yazı dizisine havale ederek Docker kullanımına geçelim.

Docker'ın çok sade ve kolay bir dökümanı var kurulum adımlarını [bu dökümana](https://docs.docker.com/install/) havale ederek biz Docker kullanımına geçiş yapalım.

Docker'ı indirip bilgisayarınıza kurduktan sonra terminalde aşağıdaki komutu yazarak başarılı bir şekilde çalışıp çalışmadığını kontrol edebilirsiniz.

```
docker -v
```
Bu komutu yazıp Enter'e bastığımızda Docker'ın versiyon numarası ve build numarasını içeren bir çıktı gelmesi lazım.

![Image of docker-v](/assets/img/posts/docker-v.png)

Docker'ımız kurulu artık Docker ile oynayabiliriz.

```
docker ps -a
```

Bu komut sistemimizde çalışan ya da durmuş tüm Container'ları listeler. Aynı işlevi gerçekleştiren bir başka komutta şudur;

```
docker container ls -a
```
bu iki komutta da gördüğünüz üzere **-a** parametresi kullanılmıştır, bu Container durdurulmuş (stop) olsa dahi listelemeye yarar.

Sisteminizde hiç kontainer olmadığını varsıyorum. Bir Container'ı çalıştırmak için ilgili Image dosyasını run etmemiz lazımdır. Örnek verecek olursak hello-world isimli Container'ı ayağa kaldırmak istiyoruz. Docker'a aşağıdaki komutu verdiğimizde Docker ilk olarak ilgili Image dosyasını local sistemimizde arar eğer varsa Image'i localden yükler. Eğer local sistemde ilgili Image yoksa Docker ilgili Image dosyasını **DockerHub**'dan indirir.

```
docker run hello-world
```

Bu komutu yazıpEnter'ea bastığımızda aşağıdaki gibi bir çıktı oluşur. Çıktıyı inceleyecek olursak.

![Image of docker-run-hello](/assets/img/posts/docker-run-hello.png)

İlk satırda Docker hello-world image'ini local sistemde bulamadığını söylüyor. Daha sonra ilgili hello-world Image'inin **latest** yani son sürümünün DockerHub dan çekildiğini gösteriyor.

Peki sonuç olarak ne oldu? Image indirildi ve çalıştırıldı.

```
docker container ls -a
```
Komutunu çalıştırırsak sistemdeki tüm Container'ları görürüz. Sonuç olarak Container'ımızın bilgilerini içeren bir ekran gelir.

![Image of docker-container-ls](/assets/img/posts/docker-container-ls.png)

Kullanacağımız diğer Docker komutlarını inceleyecek olursak.

```
docker images
---------------
Sistemdeki Docker Image'larını listeler.
```

```
docker pull imageName
---------------
Parametre olarak aldığı Image'ı çeker.
```
```
docker image history imageId
---------------
Parametre olarak aldığı id li Image'in tarihçesini gösterir.
```

```
docker image inspect imageId
---------------
Parametre olarak aldığı id li Image'in detaylı bilgisini gösterir.
```

```
docker image remove imageId
---------------
Parametre olarak aldığı id li Image'i sistemden siler.
```

```
docker container pause containerId
---------------
Parametre olarak aldığı id li Container'ı duraklatır.
```

```
docker container unpause containerId
---------------
Parametre olarak aldığı id li duraklatılmış Container'ı tekrar çalıştırır.
```
```
docker container stop containerId
---------------
Parametre olarak aldığı id li Container'ı durdurur.
```

```
docker container start containerId
---------------
Parametre olarak aldığı id li Container'ı başlatır.
```

```
docker container inspect containerId
---------------
Parametre olarak aldığı id li Container'in detaylı bilgisini gösterir.
```
```
docker container prune
---------------
Durdurulmuş tüm Container'ları sistemden kaldırır.
```
```
docker events
---------------
Tüm Docker olaylarını listeler.
```
```
docker top containerId
---------------
İlgili Container'ın çalışan processeslerini gösterir.
```
```
docker stats
---------------
Çalışan tüm Container'ların kaynak kullanım istatistiklerini canlı akış olarak gösterir.
```

Docker'a giriş niteliğindeki yazımızın sonuna geldik, bundan sonraki yazılarda Spring Boot uygulamamızı hazırlayıp Dockerize edeceğiz.

#### Kaynaklar

[https://www.docker.com/resources/what-container
](https://www.docker.com/resources/what-container)

[https://www.vmware.com/topics/glossary/content/virtual-machine
](https://www.vmware.com/topics/glossary/content/virtual-machine)

[https://phoenixnap.com/kb/what-is-hypervisor-type-1-2
](https://phoenixnap.com/kb/what-is-hypervisor-type-1-2)

[https://docs.docker.com/install/](https://docs.docker.com/install/)

[https://docs.docker.com/](https://docs.docker.com/)