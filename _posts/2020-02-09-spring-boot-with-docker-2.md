---
title: Spring Boot App With Docker-2
date: Fri 14-Feb-2020 05:22 P.M.
categories: [Devops,Docker]
language: Turkish
tags: [spring, java, docker, spring boot, container, image]
seo:
  date_modified: 2020-02-18 00:11:32 +0100

---

#### Başlangıç
Merhabalar, Docker ile Spring Boot uygulaması geliştirmeyi anlattığımız yazı dizisine kaldığımız yerden devam ediyoruz. Bir önceki yazımızda Docker'a giriş yapmıştık. Bu yazıda Spring Boot uygulamamızı paketleyip Docker Container'ı içine kopyalayıp çalıştıracağız.

Bu yazıda sırasıyla şu adımları gerçekleştireceğiz.

* Basit bir Spring Boot REST API oluşturacağız.
* REST API'mızı Jar olarak paketleyeceğiz.
* Uygulmanın Environment'ını içeren Docker Image'ını indireceğiz.
* Oluşan JAR paketimizi Container içine kopyalayacağız.
* Son olarak uygulamamızı çalıştıracağız.

#### Spring Boot Rest API Oluşturma

Spring Boot uygulamamızı [Spring Initializr](https://start.spring.io/) sitesindeki arayüz yardımı ile oluşturalım.

![Image of spring initializr](/assets/img/posts/spring-boot-docker-2/spring-boot-initializr.png)

Bu sayfada Spring Boot uygulamamızın ilgili alanları seçiyoruz ve en alttaki kısımda dependency olarak uygulamamız için gerekli bağılılıkları seçiyoruz. Generate butonuna basınca oluşan proje zip olarak bilgisayarımıza iniyor. Bu uygulamada bağımlılık olarak Spring Web'i ve Spring Devtools'u seçmemiz yeterli. İnen zip dosyasını IDE'mizde açıyoruz.

pom. xml dosyamızı inceleyecek olursak.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.2.4.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.fsoftdev.blog</groupId>
	<artifactId>SpringBootRestHello</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>SpringBootRestHello</name>
	<packaging>jar</packaging>
	<description>Demo project for Spring Boot</description>

	<properties>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
			<optional>true</optional>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
			<exclusions>
				<exclusion>
					<groupId>org.junit.vintage</groupId>
					<artifactId>junit-vintage-engine</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>

```

Birkaç noktayı revize etmemiz yeterli;

```xml
<packaging>jar</packaging>
```
ekliyoruz. Bu Maven'ın projemizi JAR olarak paketlemesi için gerekli.

Daha sonra properties etiketleri arasına aşağıdaki kodları ekleyelim.

```xml
<java.version>1.8</java.version>
<maven-jar-plugin.version>3.1.1</maven-jar-plugin.version>
```

Projemizi basit bir REST API olarak kodlayacağız. 2 tane endpointimiz olsun.

`/hello` endpointimiz ekrana `Hello Guys...` diye bir string basıyor.

`/date` endpointimiz ekrana şimdiki tarihi basıyor.

Aşağıda `MainController. java` isimli Controller sınıfımız var.

```java
package com.fsoftdev.blog.SpringBootRestHello;

import java.util.Calendar;
import java.util.Date;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MainController {

@RequestMapping(path = "hello",method = RequestMethod.GET)
public String sayHello() {
return "Hello Guys...";
}

@RequestMapping(path = "date",method = RequestMethod.GET)
public String sayTodaysDate() {
Date today = Calendar.getInstance().getTime();
return today.toString();
}

}

```

Uygulamamızı Run ettiğimizde beklediğimizde beklediğimiz çıktıları verdiğini görürüz. Normal şartlarda Spring Boot uygulamamız `localhost` da çalışıyor.
Ana senaryoya dönecek olursak Spring Boot uygulamamızı `JAR` olarak paketleyip Docker Container'ı içerisine alıp çalıştıracaktık.

#### JAR Paketi Oluşturmak

Spring Boot uygulamamızın JAR paketini oluşturmak için `Maven` den faydalanırız.
pom. xml dosyamızda plugin olarak tanımladığımız `spring-boot-maven-plugin` yardımı ile Maven'a Spring Boot desteği sağlarız.
Uygulama dizininde aşağıdaki komutu verirsek Maven bizim için target klasörü içine JAR dosyasını oluşturur. Bu komutun çalışması için sisteminizde Maven'ın kurulu olması gerekmektedir.

```
mvn clean package
```
Enter'e basıp komutu çalıştırdığımızda Maven uygulamamızın testlerini çalıştırıp bizim için JAR paketi haline getirir. Eğer işlem başarılı olmuşsa aşağıdaki çıktıları alırsınız.

![Image of mvn-package-1](/assets/img/posts/spring-boot-docker-2/ss-mvn-package-1.png)
Bu resimde JAR paketinin konumu gösteriyor. Oluşan JAR dosyasının ismini `pom. xml` de `build` bölümünde `finalName` etiketleri arasında tanımlamıştık.

![Image of mvn-package-1](/assets/img/posts/spring-boot-docker-2/ss-mvn-package-2.png)
Burada da işlemin başarıyla sonuçlandığı ve ne kadar sürede gerçekleştiğini görüyoruz.

#### JDK İçin Container Oluşturma

Evet elimizde JAR dosyamız var artık sırada JDK Docker Image'ini indirip çalıştırmak var. Bunun için aşağıdaki komutu veririz.

```
docker run -dit openjdk:8-jdk-alpine
```

Alpine, Alpine Linux temelli minimal bir Docker Image'dir. Yani yukarıdaki komut bizim için minimal bir Linux kurulumu yapacak ve içerisine JDK'yı kuracak ne büyük kolaylık de mi?
Yukarıdaki komutu çalıştırdığımız da Docker local'de openjdk:8-jdk-alpine Image'i arar bulamazsa Docker Hub'dan indirir, sonra bu Image'i çalıştırır. Peki `-dit` ayarı ne demek bu Docker Documentation'dan yardım alacak olursak. `-dit` aslında 3 option içeriyor. Bunlar `-d`,`-i`,`-t` dir.

`-d`: Container'ı detach modda çalıştırır yani Container çalışıyor aynı zamanda aynı terminalde başka komutlar girmemize olanak sağlıyor. Yani Container arka planda çalışmış oluyor. -d aynı zamanda Container Id'sinide döner.

`-i`:Container'ı interactive hale getirir. STDIN'i açık tutar.

`-t`: Container'a sözde bir TTY atar. `-it` sayesinde Container'üzerinde komut çalıştırmamız olanaklı hale gelir.

Evet yukarıdaki komutu çalıştıralım ve ne olduğunu gözlemleyelim.

![Image of docker-run-1](/assets/img/posts/spring-boot-docker-2/ss-docker-run-1.png)

Resimde de gördüğünüz üzere Docker ilk önce Image'ı localde arıyor fakat bulamıyor sonra Docker Hub'dan indiriyor ve detach modda ve üzerinde komut çalıştırmaya hazır bir şekilde çalıştırıyor. Geriye çalışan Container'ın İd sini dönüyor.

```
docker container ls
```

Komutuyla Container'ın çalıştığını gözlemleyebiliriz.

![Image of docker-run-2](/assets/img/posts/spring-boot-docker-2/ss-docker-run-2.png)

Evet Container'ımız çalışır durumda peki bundan sonra ne yapacağız? Gelin Container üzerinde komut çalıştırabiliyor muyuz ona bakalım. Çünkü `-dit` optionu ile bunu sağladığımızı düşünüyoruz.

Şu anda çalışan Container'ımız içerisinde JDK barındıran minimal bir Linux dağıtımıdır. Acaba bu Linux dağıtımız üzerinde çalışan JDK versiyonu nedir. Bunu öğrenmek için Container'ımız üzerinde `java -verison` komutu çalıştıralım bakalım neler olacak.

```
docker exec vigilant_goldberg java -verison
```
Yukarıdaki komutta `vigilant_goldberg` çalışan Container'ımızın ismidir.

Komutumuzun aşağıdaki gibi oldu.

![Image of ss-docker-exec-1](/assets/img/posts/spring-boot-docker-2/ss-docker-exec-1.png)

Evet Container'ımız çalışıyor ve üzerinde Java 8 sürümü çalışıyor.

#### JAR Paketini Container İçine Kopyalama

Şimdi sıra Jar olarak paketlediğimiz Spring Boot uygulamasını Container içine kopyalamaya geldi. Bunu yapmak için aşağıdaki komutu kullanırız. Şu anda Spring Boot uygulamamızın ana dizininde olduğumuzu varsayıyorum değilsek o dizine gidelim.

```
docker container cp target/SpringBootRestHello-0.0.1-SNAPSHOT.jar vigilant_goldberg:/tmp
```
Bu komut ile uygulamamızın `target dizini altındaki jar dosyamızı` `Container içindeki tmp klasörü altına` kopyalıyoruz. Eğer Container için tmp dizini yoksa tmp adında yeni bir dizin oluşturulur. Komutun çalışıp çalışmadığını Container üzerinde aşağıdaki komutu çalıştırarak kontrol edebiliriz. Eğer doğru bir şekilde çalışmışsa Container içine tmp adında bir dizin oluşmuş ve içerisine JAR kopyalanmıştır.

```
docker exec vigilant_goldberg ls /tmp
```
bu komut aşağıdaki çıktıyı üretir.

```
SpringBootRestHello-0.0.1-SNAPSHOT.jar

```

Evet JAR dosyamızı Container içerisine kopyaladık. Son olarak Container'ımızı bu haliyle Image olarak kaydedeceğiz ve başlangıç Script'i olarak Jar dosyamızı çalıştırmasını ayarlayacağız. Yani son durumda Spring Boot uygulamamız tüm bağımlılıklarına sahip ve dağıtılabilir bir Docker Image'i haline gelecek.

Contianer'ı Image olarak kaydetmek için aşağıdaki komut kullanılır.

```
docker container commit containerName newName:Tag
```
Burada `containerName` ilgili Container'ımızın adı `newName` ve `tag` da yeni oluşacak Image'e verilecek isim ve tag bilgisidir. Container'ımızı bu şekilde commit yaparsak içerisnde JAR olan bir Container olur, bu JAR'ın çalışmasını istiyorsak Container'a JAR'ı çalıştıracak komutları manuel olarak vermemiz gerekir ve Container'ın olası durdurulma durumlarında JAR'ı tekrar başlatmamız gerekir. Bu durumu daha efektif hale getirmek için Container her başladığında ilgili JAR dosyasını çalışmasını ayarlayabiliriz bunu aşağıdaki gibi yaparız.

```
docker container commit --change='CMD ["java","-jar","/tmp/SpringBootRestHello-0.0.1-SNAPSHOT.jar"]' vigilant_goldberg spring-boot-hello-world:0.0.1-SNAPSHOT
```
Bu komut Container'ımızı verilen yeni ad ve etiket ile yeni bir Image olarak kaydeder ve geriye Image'in Id'sini döner.

![Image of ss-docker-commit-1](/assets/img/posts/spring-boot-docker-2/docker-commit-1.png)

`docker images` komutu ile sistemdeki tüm Image'leri listeleriz ve yeni Image'in oluşup oluşmadığına bakabiliriz.

#### İçerisinde JAR Olan Container'ı Çalıştırma

Evet şu anda Container olarak başlatıldığında ilgili JAR'ı çalıştıracak Docker Image'ına sahibiz, gelin hep beraber Image'ımızı çalıştırıp Spring Boot uygulamamızın çalışıp çalışmadığını kontrol edelim. Aşağıdaki komut ile Image'ımızı çalıştırırız. Komutun içindeki `-p 8080:8080` ifadesi Spring Boot uygulamamızın hangi portta çalışacağını ifade ediyor.

```
docker run p- 8080:8080 spring-boot-hello-world:0.0.1-SNAPSHOT
```
Bu komutu çalıştırdığımızda aşağıdaki gibi bir hata alıyorsanız 8080 portunun başka bir uygulama tarafından kullanıldığını anlamalısınız.

![Image of ss-docker-run-1](/assets/img/posts/spring-boot-docker-2/docker-run-1.png)

Bu hatayı gidermek için ya 8080 portunu değiştirmemiz ya da bu portu müsait duruma getirmemiz gerekmektedir. Portumuzu müsait hale getirip tekrar aşağıdaki komutu çalıştırdığımızda;

```
docker run -p 8080:8080 spring-boot-hello-world:0.0.1-SNAPSHOT
```

Aşağıdaki gibi Container'ımız ayağa kalkar ve içindeki Spring Boot uygulaması başlangıç Script'i olarak çalışır.

![Image of ss-container-result](/assets/img/posts/spring-boot-docker-2/ss-container-result.png)

Tarayıcımıza `localhost:8080/hello` Url'ini girdiğimizde aşağıdaki sayfa karşımız gelir.

![Image of ss-container-result](/assets/img/posts/spring-boot-docker-2/ss-localhost-hello.png)

`localhost:8080/date` Url'ini girdiğimizde ise aşağıdaki sayfa karşımız gelir.

![Image of ss-container-result](/assets/img/posts/spring-boot-docker-2/ss-localhost-date.png)

Evet bu yazımızda Spring Boot ile geliştirdiğimiz basit bir uygulamayı Container içine alıp (Containerizing), nasıl çaıştıracağımızı öğrenmiş olduk. İşte tam bu noktada değinmek istediğim husus şu ki DEVOPS kültürünün mantığı süreçleri otomatikleştirmeye dayanır. Bu yazıda tam olarak süreci otomatize etmiş olmadık. Spring Boot uygulamamızın build işlemini tam manasıyla otomatize etmek için `DOCKERFILE` denen bir yapıya ihtiyacımız var. Yani bu yazıda anlattığımız tüm süreçleri-komutları `DOCKERFILE` içerisine alıyoruz ve bu DOCKERFILE'dan IMAGE oluşturuyoruz ve oluşan IMAGE'i çalıştırıyoruz. Ve elimizdeki DOCKERFILE'ımızı projemizle beraber paylaşabiliyoruz.

Bir sonraki yazımızda DOCKERFILE oluşturmayı inceleyerek yazı serimize devam edeceğiz.

[Türkçe yazım kuralları denetimi içini turkceyaz.com'dan faydalanıldı.](https://turkceyaz.com/)

