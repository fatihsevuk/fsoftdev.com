---
title: Spring Boot And Docker-5 Containerizing Spring Boot MYSQL Application
date: Sun 22-Mar-2020 08:14 P.M.
categories: [Devops, Docker]
language: Turkish
tags: [spring, java, docker, spring boot, container,mysql,image,docker networking,docker volume]
---

## Başlangıç

Merhabalar bu yazımızda Spring Boot ve MySQL veri tabanı kullanarak geliştirdiğimiz bir uygulamayı nasıl Container içerisine alıp çalıştırabileceğimizi öğreneceğiz. Yapacağımız işlemleri kuş bakışı senarize edecek olursak ilk olarak MySQL Image'den Container oluşturuyoruz. Daha sonra Spring Boot uygulamamızı Containerize ediyoruz. Tabii ki Spring Boot Container' ımız MySQL Container'ımız ile entegre ediliyor. Şimdi gelin bu adımları hep beraber yapalım.

## MySQL Container Oluşturma

İlk olarak `docker run mysql:5.7` komutunu terminale yazıp çalıştırdığımızda neler oluyor bir bakalım.Bu komut lokalde MySQL Image'ini arar ve çalıştırır eğer bulamazsa Docker Hub dan önce MySQL Image'ini çeker ve çalıştırır.Bu komut şöyle bir error üretir.

```
2020-03-22 19:34:13+00:00 [ERROR] [Entrypoint]: Database is uninitialized and password option is not specified
	You need to specify one of MYSQL_ROOT_PASSWORD, MYSQL_ALLOW_EMPTY_PASSWORD and MYSQL_RANDOM_ROOT_PASSWORD

```

Bu hata veri tabanı için Root parolası tanımlamamızı söylüyor bunu şu şekilde yapacağız.

```shell
docker run -d -e MYSQL_ROOT_PASSWORD=dummyrootpw -e MYSQL_DATABASE=students -e MYSQL_PASSWORD=dummyuserpw -e MYSQL_USER=dummyuser -p 3306:3306 --name mysql mysql:5.7
```
Evet bu komutu hatasız girdiğimizde oluşan Containe'ın id si ekrana basılır. Aynı zamanda `container ls` komutu ile Container'ımızın çalışıp çalışmadığını kontrol edebiliriz.

Evet şu anda çalışan bir MySQL veri tabanına sahibiz bu veritabanına ister lokalde herhangi bir uygulamamızı isterse Container içerisinde herhangi bir uygulamayı bağlayabiliriz. Biz containerize edeceğimiz bir Spring Boot uygulamasını bağlayacağız.

## Spring Boot Container Oluşturma

MySQL Container oluştururken environment olarak ayarladığımız `MYSQL_DATABASE` , `MYSQL_USER` ve `MYSQL_PASSWORD` değerlerini Spring uygulamamız içerisindeki `application.properties` dosyası içerisine aşağıdaki gibi yazmamız gerekiyor.

```properties
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.hibernate.ddl-auto=update
spring.datasource.url=jdbc:mysql://${RDS_HOSTNAME:localhost}:${RDS_PORT:3306}/${RDS_DB_NAME:students}
spring.datasource.username=${RDS_USERNAME:dummyuser}
spring.datasource.password=${RDS_PASSWORD:dummyuserpw}
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL57Dialect
```
Uygulamamız içerisinde veri tabanına bağlanmak için gerekli ayarlamaları yaptıktan sonra uygulamamızı Container olarak yağa kaldırabiliriz.Bunun için daha önceki yazılarda bahsettiğimiz `Spotify Dockerfile Plugin` kullanacağız.

Dockerfile içeriğimiz aşağıdaki gibidir.

```
FROM tomcat:8.0.51-jre8-alpine
RUN rm -rf /usr/local/tomcat/webapps/*
COPY ./target/*.war /usr/local/tomcat/webapps/ROOT.war
CMD ["catalina.sh","run"]
```
`mvn clean package -DskipTests` komutunu çalıştırdığımızda uygulama klasörü altında ki target dizini altına .war arşivimiz oluşur ve Image içerisine kopyalanır.

Evet Spring Boot uygulamamızı içeren Image'imiz hazır aynı zamanda MySQL Container'ımızda çalışır durumda geriye ikisi arasındaki bağlantıyı sağlamak kaldı.Bunun için uygulama Image'ımızı aşağıdaki şekilde run etmemiz gerekir.

## Spring Boot Container İle MySQL Container'i Bağlama

```
docker container run -p 8080:8080 --link=mysql -e RDS_HOSTNAME=mysql spring-mysql-app:1. 0.0-SNAPSHOT
```

Yukarıdaki komutta `--link mysql` parametresi ile spring uygulamamızı mysql ile bağlıyoruz. `--link` bağlayacağımız Container'ın adını parametre alır. Fakat `--link` deprecated
bir parametredir. Bunun yerine Docker network yapılarını kullanacağız. Şimdilik --link ile yapalım.

`-e RDS_HOSTNAME=mysql` komutu da ağ üzerindeki mysql veritabanına bağlanmak için gereklidir.

## Docker Networking

Evet buraya kadar işin büyük bir kısmını hallettik.Uygulama Container'ı ile MySQL Container'ı bağlamak için `--link=mysql` yapısını kullandık fakat --link Docker tarafından kullanımdan kaldırıldı.Bunun yerine Docker Network kavramını kullanacağız.

Container'ler birbirleri ve dış dünya ile haberleşmek için farklı türlerde ağlar kullanırlar.Docker kurduğumuzda default olarak üç network tanımlıdır bunları görmek için
`docker network ls` komutunu çalıştırırız.

* host
* bridge
* none

`docker inspect bridge`

`docker inspect host`

komutları ile ilgili network üzerinde hangi Container'ler çalışıyor görebiliriz.Default olarak Container'ler `bridge` networkünü kullanırlar. Bizim Spring Boot uygulamamızda MySQL ile `bridge` networku üzerinden haberleşmektedir.

**Not:** `host` networku yalnızca linux sistemlerde vardır.

Herhangi bir Image'i bir network üzerinde çalıştırmak için şu şekilde bir komut yazarız.

`docker run --network=host imageNanme`

Evet bu bilgiler ışığında uygulamamıza dönecek olursak eğer host networkunu kullanırsak uygulamamız herhangi bir ek tanımlamaya gerek duymadan mysql ile haberleşir şöyle ki

```shell

docker container run -p 8080:8080 --network=host spring-mysql-app:0.0.1-SNAPSHOT

```
Bu komuta bakacak olursak port bilgisi ve rds_host bilgisi verilmedi. Çünkü her iki Container'da host networku içerisindedir.


## Özel Network Oluşturma

Eğer kendimize özel bir ağ oluşturup Container'larımızı bu ağ üzerinde haberleştirmek istiyorsak şöyle yaparız.

```shel
docker network create web-mysql-network
```
Bu konutu girdiğimizde web-mysql-network isminde bir network oluşur. Kontrol amacıyla `docker network ls` komutunu kullanırsak `web-mysql-network` isminde ve `bridge` türünde bir ağın oluştuğunu görürüz.

Sonuç olarak her iki Container'ımızı bu ağ üzerinde çalıştırırsak birbirleri ile haberleşirler. Custom network olursa iki Container'ı birbirine bağlamaya gerek yoktur.

MySQL Container'ını özel ağımızda çalıştırmak için aşağıdaki komutu kullanırız.

```shell
docker run -d -e MYSQL_ROOT_PASSWORD=dummyrootpw -e MYSQL_DATABASE=students -e MYSQL_PASSWORD=dummyuserpw -e MYSQL_USER=dummyuser -p 3306:3306 --network=web-mysql-network --name mysql mysql:5.7

```

Spring Boot uygulama Container'ını çalıştırmak için ise

```shell
docker container run -p 8080:8080 --network=web-mysql-network -e RDS_HOSTNAME=mysql spring-mysql-app:0.0.1-SNAPSHOT
```

evet Container'larımız başarılı bir şekilde ayağa kalktı ve birbirleriyle iletişim halindeler.

`docker inspect web-mysql-network` komutunu çalıştırısak oluşan çıktıda Containers bölümünde uygulama ve mysql Container'lerimizi görürüz.

![Image of ss-network-inspect-1](/assets/img/posts/spring-boot-docker-5/ss-network-inspect-1.png)

## Veri Kalıcılığını Sağlama-Docker Volumes

MySQL Container'ımız çalıştı ve uygulamamıza veri girmeye başladık diyelim eğer bu verileri `Docker Volume` kullanarak kalıcı hale getirmezsek Container'lar durduktan sonra veriler silinir.

Docker Container'ları yapıları itibari ile üzerinde çalıştıkları makiden bağımsız bir şekilde çalışırlar ve herhangi bir durdurulma senaryosunda sıfırdan başlarlar fakat birçok uygulama bu duruma uygun değildir mesela bizim MySQL Container'ımız uygulama verilerini saklamakla görevlidir yani bu Container dursa bile tekrar başladığında verilerimizin kaybolmaması gerekir. Bunu sağlamak için Container içerisindeki verilerin tutulduğu dizini host makine üzerindeki bir dizinle eşleştiriyoruz bu şekilde Container fdursa bile yeni bir Container başlattığımızda host üzerindeki bu dizini volume olarak parametre alırsa veriler kaybolmadan başlamış olur.

`--volume data-directory-in-host-machine:/var/lib/mysql/` Container'ımızı ilk defa başlattığımızda bu ayarlamayı yaparsak verilerimiz host üzerinde ilgili klasöre yedeklenmiş olur.












 




