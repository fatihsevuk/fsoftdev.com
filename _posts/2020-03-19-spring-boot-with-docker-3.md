---
title: Spring Boot App With Docker-3
date: Thu 19-Mar-2020 01:38 P.M.
categories: [Devops, Docker]
toc: true
language: Turkish
tags: [spring, java, docker, spring boot, container, image, docker file, spotify maven
    plugin, jib maven plugin, DOCKERFILE]
seo:
  date_modified: 2020-03-19 18:32:11 +0100

---

#### Başlangıç
Merhabalar, yazı dizimize kaldığımız yerden devam ediyoruz. Bu yazıda `Dockerfile` oluşturmayı inceleyeceğiz. Hatırlayacağınız üzere aşağıdaki adımları gerçekleştirmiştik.

* Spring Boot Rest API oluşturduk.
* Bu Rest Api'yi JAR olarak paketledik.
* JDK içeren Docker Image'i indirdik.
* İndirdiğimiz Image'i kullanarak Container'ı ayağa kaldırdık.
* JAR dosyamızı Container içerisine kopyaladık.
* Son olarak Container'ımızın başlarken JAR dosyamızı çalıştırmasını ayarladık.


Bu saydığımız tüm adımları terminal yardımıyla teker teker yazarak yaptık.Bu işlemleri otomatikleştirmek için `Dockerfile` oluşturulur.

#### Dockerfile Oluşturma

Şimdi gelin hep beraber ilk `Dockerfile` dosyamızı oluşturalım.Proje dizinimizde `Dockerfile` isminde bir dosya oluşturup içerisine aşağıdaki kodları yazalım.

```
FROM openjdk:8-jdk-alpine
ADD target/SpringBootRestHello-0.0.1-SNAPSHOT.jar SpringBootRestHello-0.0.1-SNAPSHOT.jar
ENTRYPOINT ["sh","-c","java -jar /SpringBootRestHello-0.0.1-SNAPSHOT.jar"]
```
Evet yukarıda gördüğünüz kodları açıklayacak olursak.

`FROM openjdk:8-jdk-alpine` ifadesi ile OpenJdk Image'ini çekiyoruz.

`ADD target/SpringBootRestHello-0.0.1-SNAPSHOT.jar SpringBootRestHello-0.0.1-SNAPSHOT.jar` ifadesi ile target dizinindeki JAR dosyasını Container içerisine aynı isimle kopyalar.

`ENTRYPOINT ["sh","-c","java -jar /SpringBootRestHello-0.0.1-SNAPSHOT.jar"]` ifadesi ile de Container'ımıza başlangıçta ilgili JAR'ı çalıştırmasını söylüyoruz.

#### Dockerfile'ı Kullanma

Evet şimdi gelin hep beraber oluşturduğumuz Dockerfile'ı nasıl kullanacağımıza bakalım.

Proje dizinimizin içindeyken aşağıdaki komutu çalıştırıyoruz.

```
docker build -t rest-api:with-dockerfile .
```

`-t` parametresi tag anlamına gelir build işleminin sonucunda oluşacak olana Image'in isim:tag bilgisini yazarız.

Komutun sonundaki nokta ilgili Dockerfile'ın şu anki dizinde olduğunu belirtir. Komut çalıştığında aşağıdaki gibi bir çıktı oluşur.

![Image of ss-docker-build-1](/assets/img/posts/spring-boot-docker-3/docker-build-1.png)

Evet bu işlem sonucunda JAR'ımızı içeren Docker Image'ımız oluştu, isterseniz,

`docker images` komutu yardımıyla kontrol edebilriz.

![Image of ss-docker-image-1](/assets/img/posts/spring-boot-docker-3/docker-images-1.png)

Yukarıdaki çıktıda da gördüğünüz üzere en üstte `rest-api` isminde ve `with-dockerfile` etiketine sahip bir Image oluştu.

Şimdi sıra bu Image'i kullanarak Container'ımızı ayağa kaldırmaya geldi.Bu işlemi gerçekleştirmek için aşağıdaki komutu kullanırız.

```
docker run -p 8080:8080 rest-ap:with-dockerfile
```
![Image of ss-docker-run-1](/assets/img/posts/spring-boot-docker-3/docker-run-1.png)

Evet yukarıdaki çıktıdan da gördüğünüz gibi Container'ımız ayağa kalktı ve başlangıç scripti olarak ayarladığımız JAR dosyasını çalıştırma işlemini başarılı bir şekilde yaptı. Sonuç olarak tarayıcımızda `localhost:8080/hello` ya da `localhost:8080/date` endpointlerine gidersek uygulamamızın başarılı bir şekilde çalıştığınız görürüz.

Eğer Dockerfile içerisinde `EXPOSE 8080` diye bir ifade kullanırsak docker run komutu içerisinde `-p 8080 ` diye bir ifade kullanmadan uygulamamız 8080 portunda çalışmaya başlar.

#### Spotify Maven Plug-in ile Image Oluşturma

Bu Maven plugin'i kullanarak JAR oluşturma aşamasında aynı zamanda ilgili Dockerfile'ımızdan Image oluşturabiliriz. Bu plugin'i kullanmak için pom. xml dosyasında `<plugins></plugins>` etiketleri arasına aşağıdaki kodlar eklenir.

```xml
<plugin>
        <groupId>com.spotify</groupId>
        <artifactId>dockerfile-maven-plugin</artifactId>
        <version>1.4.13</version>
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

Bu plugini ekledikten sonra 

`mvn package -DskipTests`

komutunu çalıştırırsak aşağıdaki gibi bir çıktı ile karşılaşırız.

![Image of ss-spotify-maven-plugin](/assets/img/posts/spring-boot-docker-3/spotify-maven-plugin.png)

Evet görüldüğü üzere kodlarımız derlendi ve paketlendi sonra Image'imiz oluşturuldu ve içerisine JAR'ımız kopyalandı. Oluşan Image çalıştırılmak için hazır.

`docker run spring-boot-rest-hello:0.0.1-SNAPSHOt`

komutu ile Image'imizi çalıştırırız. `localhost:8080/hello` endpointine tarayıcımızdan gidersek Container'ımızın çalıştığını görürüz.

Daha generic ve reusable bir Dockerfile oluşturmak için aşağıdaki düzenlemeleri yapabiliriz.

```
FROM openjdk:8-jdk-alpine
ADD target/*.jar app.jar
ENTRYPOINT ["sh","-c","java -jar /app.jar"]
```

#### Cached Dependency - No Fat Jar

Şimdiye kadar anlattığımız kısımlarda JAR'ımız çok fazla bağımlılığa sahip olmadığı için boyutu da çok büyük olmadı fakat başka projelerde çok fazla bağımlılık olabilir buda JAR boyutunu oldukça büyük hale getirir bunun önüne geçmek ve build aşamasında her bağımlılığın tekrar indirilmesini önlemek için `Maven Dependency Plugin` kullanılır. Bu plugin'i kullanmak için Spotify plugin'i kaldırıp ya da comment yapabiliriz.

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-dependency-plugin</artifactId>
    <executions>
        <execution>
            <id>unpack</id>
            <phase>package</phase>
            <goals>
                <goal>unpack</goal>
            </goals>
            <configuration>
                <artifactItems>
                    <artifactItem>
                        <groupId>${project.groupId}</groupId>
                        <artifactId>${project.artifactId}</artifactId>
                        <version>${project.version}</version>
                    </artifactItem>
                </artifactItems>
            </configuration>
        </execution>
    </executions>
</plugin>
```
Bu plugin'i pom. xml'e ekledikten sonra `mvn package` komutunu çalıştırdığımızda aşağıdaki gibi bir durum oluşur. Oluşan JAR dosyası çözülür sonuçta olarak

`target/classes`

`target/dependency`

Klasörleri oluşur, classes altında derlenmiş `.class` uzantılı sınıflarımız bulunur.

dependecy altında ise aşağıdaki yapıda projenin bağımlılıkları bulunur.

![Image of target-dep](/assets/img/posts/spring-boot-docker-3/target-dep.png)

Evet projemiz derlendi ve bağımlılıkları oluşturuldu artık Image'imizi oluşturabiliriz bunun için `Dockerfile` içerisinde bazı değişiklikler yapmamız gerekiyor.

```
FROM openjdk:8-jdk-alpine
ARG DEPENDENCY=target/dependency
COPY ${DEPENDENCY}/BOOT-INF/lib /app/lib
COPY ${DEPENDENCY}/META-INF /app/META-INF
COPY ${DEPENDENCY}/BOOT-INF/classes /app
ENTRYPOINT ["java","-cp","app:app/lib/*","com/fsoftdev/blog/SpringBootRestHello/SpringBootRestHelloApplication"]
```

Evet kısaca bahsedecek olursak oluşan bağımlılıkları Image içine kopyalıyoruz ve başlangıç scripti olarak Application sınıfımızın çalıştırılmasını ayarlıyoruz.

`docker build -t rest-api:no-fat-jar .` komutunu çalıştırdığımızda aşağıda gördüğünüz gibi Image'imiz oluşur.

![Image of no fat jar](/assets/img/posts/spring-boot-docker-3/no-fat-jar-image.png)

Son olarak oluşan Image'ımızı çalıştırıp localhost'tan kontrollerimizi yapıyoruz.

`docker run -p 8080:8080 rest-api:no-fat-jar`

#### Google JIB Plug-in ile Image Oluşturma

Bu plugin'i kullandığımız zaman Dockerfile'a ihtiyacımız olmaz. Çünkü Dockerfile içerisindeki ayarları plugin'e config olarak vereceğiz.

pom. xml dosyasına aşağıdaki plugin kodlarını ekleyerek bu plugin'i kullanmaya başlayabiliriz.

```xml
	<plugin>
		<groupId>com.google.cloud.tools</groupId>
		<artifactId>jib-maven-plugin</artifactId>
		<version>1.6.1</version>
		<configuration>
			<container>
				<creationTime>USE_CURRENT_TIMESTAMP</creationTime>
			</container>
		</configuration>
		<executions>
			<execution>
				<phase>package</phase>
				<goals>
					<goal>dockerBuild</goal>
				</goals>
			</execution>
		</executions>
	</plugin>
```
pom. xml'i kaydedip `mvn clean package -DskipTests` komutunu çalıştırdığımızda Image'imiz oluşur burada ilginç olan durum bu plugin'in birçok şeyi bizden herhangi bir config alamdan halletmesidir. JDK için `distroless java` diye bir base image indiriyor, daha sonra bizim Application sınıfımızı kendisi algılıyor. Eğer bu configleri özelleştirmek istiyorsak `<configuration></configuration>` etiketleri arasına aşağıdaki gibi bir config yazabiliriz. **Eğer config yazmayacaksak eklenti Image adını artifactId kısmından alacaktır ve eğer docker image name standartlarına göre gir artifact idmiz yoksa hata oluşacaktır. Yani artifact id küçük harf sayı ve sembollerden oluşabilir.**

```xml
<configuration>
	<from>
		<image>openjdk:alpine</image>
	</from>
	<to>
		<image>in28min/${project.name}</image>
		<tags>
			<tag>${project.version}</tag>
			<tag>latest</tag>
		</tags>
	</to>
	<container>
		<jvmFlags>
			<jvmFlag>-Xms512m</jvmFlag>
		</jvmFlags>
		<mainClass>com/fsoftdev/blog/SpringBootRestHello/SpringBootRestHelloApplication</mainClass>
		<ports>
			<port>8080</port>
		</ports>
	</container>
</configuration>
```

Image'imiz oluştuktan sonra docker run komutu ile çalıştırabiliriz.

Bu yazınında sonuna gelmiş bulunmaktayız bir sonraki yazıda Spring Boot Web uygulamasını nasıl containerize edeceğimizi öğreneceğiz.