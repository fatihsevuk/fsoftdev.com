---
title: Spring Boot And Docker-4 Containerizing Spring Boot Web Application
date: Sun 22-Mar-2020 04:22 P.M.
categories: [Devops, Docker]
language: Turkish
tags: [spring, java, docker, spring boot, container,apache tomcat,image]
---

## Başlangıç

Merhabalar Spring Boot Web uygulamasını nasıl containerize edeceğimizi anlatarak yazı dizimize devam ediyoruz.Spring Boot web uygulamamızı `war` uzantılı olarak oluşturup `Apacha Tomcat Servlet Container` üzerinde çalıştıracağız.

Spring Boot ile nasıl web uygulaması geliştireceğiz bunu bir başka yazı dizisine havale edip biz Docker ile ilgili kısmını anlatacağız.Düşünelim ki elimizde kodlarıyla hazır bir Spring Boot uygulamamız var bu uygulamayı container içine alıp nasıl çalıştırırız.

 İlk olarak pom.xml dosyamızda `<packaging>war</packaging>` olarak ayarlıyoruz. Daha sonra `Spotify Dockerfile Plugin` için gerekli kodları ekleriz.

 ```xml
 <plugin>
	<groupId>com.spotify</groupId>
	<artifactId>dockerfile-maven-plugin</artifactId>
	<version>1.4.10</version>
	<executions>
		<execution>
			<id>default</id>
			<goals>
				<goal>build</goal>
			</goals>
		</execution>
	</executions>
	<configuration>
		<repository>${project.name}</repository>
		<tag>${project.version}</tag>
		<skipDockerInfo>true</skipDockerInfo>
	</configuration>
</plugin>
 ```
Daha sonra `Dockerfile` oluştururuz.

```
FROM tomcat:8.0.51-jre8-alpine
EXPOSE 8080
RUN rm -rf /usr/local/tomcat/webapps/*
COPY target/*.war /usr/local/tomcat/webapps/ROOT.war
CMD ["catalina.sh","run"]
```

Dockerfile'ı inceleyecek olursak; 
* İlk olarak tomcat içeren bir alpine linux distrosu indiriliyor.
* İkinci satırdaki `EXPOSE 8080` ifadesi oluşacak Container'ın çalışma zamanında ağdaki 8080 portunu dinlediğini ifade ediyor.
* Üçüncü satırda Container içindeki tomcat klasörünün içerisindeki webapps klasörünü temizliyoruz.
* Dördüncü satırda localde oluşturduğumuz `war` paketini Container içerisindeki tomcat içerisine kopyalıyoruz.
* Son olarak Container'ın başlangıç scripti olarak Tomcati çalıştırmasını ayarlıyoruz.

Tüm bu düzenlemeleri yaptıktan sonra proje klasöründe  `mvn package -DskipTests` komutunu verip `war` dosyamızı ve `Image`'imizi oluşturuyoruz.

![Image of ss-mvn-package-1](/assets/img/posts/spring-boot-docker-4/ss-mvn-package-1.png)

Evet yukarıda görüğünüz gibi war dosyamız oluştu ve oluşan Image içerisine kopylandı.Son olarak Container'ımızı ayağa kaldırıyoruz.

`docker run -p 8080:8080 spring-web-app:0.0.1-SNAPSHOT`

komutu ile Container'ı ayağa kaldırıyoruz.

**Not:** Dockerfile içerisinde `ADD` yerine `COPY` ve  `ENTRYPOINT` yerine `CMD` kullandık;

* COPY ve ADD komutlarının ikisi de Container içerisine dosya kopyalamaya yarar,fark olarak ADD ile uzak bir URL den Image içerisine veri kopyalanabilir.Bir diğer farkda lokaldeki özel formattaki arşiv dosyalarını Image içerisine kopyalayıp genişletebilir.Docker best practice'lerine göre COPY kullanımı önerilmektedir.

* CMD ile verilen komutlar Container çalıştıktan sonra overwrite edilebilir. ENTRYPOINT ile verilen komutlar overwrite edilmez.
