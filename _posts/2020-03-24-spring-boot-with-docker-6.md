---
title: Spring Boot And Docker-6 Full Stack Applicaiton:Frontend + Backend Build
date: Tue 24-Mar-2020 06:01 P.M.
categories: [Devops, Docker]
language: Turkish
tags: [spring, java, docker, spring boot, container,mysql,image,multi stage build,docker compose]
---

## Başlangıç
Merhabalar bu yazımızda fullstack bir uygulamayı Docker ile nasıl ayağa kaldıracağımızı adım adım inceleyeceğiz. Frontend de React ile geliştirilmiş bir uygulama backend de yine Spring Boot ile geliştirilmiş bir REST API' yi kullanacağız. Temel mantığımız şu olacak uygulamamız yüzde yüz taşınabilir olacak yani hangi sistem üzerinde çalışırsa çalışsın herhangi bir sorun çıkarmayacak.

## Frontend İçin Multi Stage Dockerfile Oluşturma

Frontend uygulamamız temelde `HTML`, `CSS` ve `JavaScript` dosyalarından oluşmaktadır. Ve bu uygulamamızın çalışması için bir web servera ihtiyaç duymaktayız. Biz web server olarak `nginx` kullanacağız. Frontend projemiz için `multi stage` bir `Dockerfile` oluşturacağız.

Dockerfile temel olarak şu adımlardan oluşacak.

* nodejs image üzerinde npm kullanarak projemizi build edeceğiz.
* build edilmiş uygulama dosyalarımızı nginx image üzerinde çalıştıracağız.

Proje klasörümüz içerisinde Dockerfile dosyamızı oluşturacağız.

```shell
## Stage 1 - Lets build the "deployable package"
FROM node:7. 10 as frontend-build
WORKDIR /fullstack/frontend

# Step 1 - Download all package dependencies first.
# We will redownload dependencies only when packages change.
COPY package. json package-lock. json . /
RUN npm install

# Step 2 - Copy all source and run build
COPY . . /
RUN npm run build

## Stage 2 - Let's build a minimal image with the "deployable package"
FROM nginx:1. 12-alpine
COPY --from=frontend-build /fullstack/frontend/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]

```

Dockerfile'ı inceleyecek olursak `npm run build` işlemi var bu işlem sonunda `node_modules` isminde bir klasör oluşur build işleminden sonra bu klasöre ihtiyacımız yoktur. O yüzden Docker'ın bu klasörü es geçmesini istiyoruz bunu sağlamak için `.dockerignore` isimli bir dosya oluşturup bu klasör ismini o dosyaya eklememiz gerekir.

Tüm bu işlemleri yaptıktan sonra frontend Container'ımızı ayağa kaldırmaya hazırız. İlk olarak Dockerfile'dan Image'ımızı oluşturacağız bunun için

`docker build . ` yada `docker build -t tagName . ` komutlarını kullanırız ikinci komuttaki `tagName` kısmına istediğimiz bir tag ismi verebiliriz.

Eğer Image'imizin başarılı bir şekilde oluştuğundan emin olduktan sonra aşağıdaki komut ile Container'ımızı ayağa kaldırabiliriz.

`docker run -p 4200:80 react-app:1. 0.0-SNAPSHOT`

Burada nginx'in 80 portunu host'un 4200 portu ile eşleştirdik.

Bu komut çalıştığında herhangi bir konsol çıktısı üretmez ta ki `localhost:4200` adresine tarayıcımızdan gidene kadar.

Evet sonuç olarak gördüğünüz üzere manuel olarak hiçbir kurulum yapmadan uygulamamızı ayağa kaldırdık. Bu Dockerfile ile herhangi bir sistem üzerinde extradan hiçbir şeye ihtiyaç kalmadan uygulamamızı run edebiliriz.

## Backend İçin Multi Stage Dockerfile Oluşturma

Backend Image'imizide iki stage olarak gerçekleştireceğiz:

* İlk olarak Maven Image'ini indirip projemiz için gerekli tüm dependency'leri indireceğiz.
* Daha sonra openjdk Image'ini indirip uygualamızı run edeceğiz.

Dockerfile'ımız aşağıda ki gibidir.

```shell
##### Stage 1 - Lets build the "deployable package"

FROM maven:3.6.1-jdk-8-alpine as backend-build
WORKDIR /fullstack/backend

### Step 1 - Copy pom. xml and download project dependencies

# Dividing copy into two steps to ensure that we download dependencies
# only when pom. xml changes
COPY pom. xml .
# dependency:go-offline - Goal that resolves all project dependencies,
# including plugins and reports and their dependencies. -B -> Batch mode
RUN mvn dependency:go-offline -B

### Step 2 - Copy source and build "deployable package"
COPY src src
RUN mvn install -DskipTests

# Unzip
RUN mkdir -p target/dependency && (cd target/dependency; jar -xf ../*.jar)

##### Stage 2 - Let's build a minimal image with the "deployable package"
FROM openjdk:8-jdk-alpine
VOLUME /tmp
ARG DEPENDENCY=/fullstack/backend/target/dependency
COPY --from=backend-build ${DEPENDENCY}/BOOT-INF/lib /app/lib
COPY --from=backend-build ${DEPENDENCY}/META-INF /app/META-INF
COPY --from=backend-build ${DEPENDENCY}/BOOT-INF/classes /app
ENTRYPOINT ["java","-cp","app:app/lib/*","com. in28minutes. rest. webservices. restfulwebservices. RestfulWebServicesApplication"]
```

`docker build . ` komutu ile uygulama Image'ini oluştururuz daha sonra,

`docker run -p 8080:8080 imageName:tag` komutu ile Container'ımızı ayağa kaldırırız burada `imageName:Tag` kısmına ilgili Image ile ilgili bilgiler gelir.

## Docker Compose

Şu ana kadar ki anlatımlarımızda maximum 2 Container ile uygulama geliştirdik fakat real world durumlarda Container sayısı bu kadar az olmaz. Container sayısı arttıkça bu Container'ları yönetmek içinden çıkılamaz bir hâl alabilir. Bu durumu yönetmek için Docker Compose iyi bir çözümdür.

Docker Compose kurulumunu Docker offical dökümanından yapabilirsiniz. Kurulumu yaptıktan sonra projemiz için `docker-compose. yml` dosyamızı oluştururuz.

```yml
version: '3.7'
# Removed subprocess. CalledProcessError: Command '['/usr/local/bin/docker-credential-desktop', 'get']' returned non-zero exit status 1
# I had this:
# cat ~/. docker/config. json
# {"auths":{},"credsStore":"", "credsStore":"desktop","stackOrchestrator":"swarm"}
# I updated to this:
# {"auths":{},"credsStore":"","stackOrchestrator":"swarm"}
services:
todo-web-application:
image: in28min/todo-web-application-mysql:0.0.1-SNAPSHOT
#build:
#context: .
#dockerfile: Dockerfile
ports:
- "8080:8080"
restart: always
depends_on: # Start the depends_on first
- mysql
environment:
RDS_HOSTNAME: mysql
RDS_PORT: 3306
RDS_DB_NAME: todos
RDS_USERNAME: todos-user
RDS_PASSWORD: dummytodos
networks:
- todo-web-application-network

mysql:
image: mysql:5.7
ports:
- "3306:3306"
restart: always
environment:
MYSQL_ROOT_PASSWORD: root
MYSQL_ROOT_PASSWORD: dummypassword
MYSQL_USER: todos-user
MYSQL_PASSWORD: dummytodos
MYSQL_DATABASE: todos
volumes:
- mysql-database-data-volume:/var/lib/mysql
networks:
- todo-web-application-network

# Volumes
volumes:
mysql-database-data-volume:

networks:
todo-web-application-network:
```

Evet docker-compose. yml dosyamızı oluşturdaktan sonra

```
docker-compose up
```
komutu ile Container'larımızı ayağa kaldırırız.

## Docker Compose Komutları

```
docker-compose down
```
komutu ile Container'larımızı durdururuz.

```
docker-compose events
```
ile çalışan Container'larımız ile ilgili bilgileri alırız.

```
docker-compose config
```
ile docker-compose dosyamızdaki ayarlara erişiriz.

```
docker-compose images
```
ile hangi Image'lerin çalıştığını görürüz.

```
docker-compose ps
```
ile kullanılan Container'ların durumunu hangi port üzerinde çalıştığını ve hangi komutlarla başladığını görürüz.

```
docker-compose top
```
Her Container'ın içerindeki tüm processleri gösterir.

```
docker-compose pause
```
Tüm Container'ları `paused` durumuna çeker.

```
docker-compose unpause
```
`paused` Container'ları çalıştırır.

```
docker-compose stop
```
Tüm Container'ları durdurur.

```
docker compose kill
```

Tüm Container'ları kill yapar.

```
docker compose rm
```
Durmuş Containerları siler.

```
docker compose build
```
Bu komut şu şekilde kullanılır `docker-compose. yml` dosyasındaki servislere ait Image'ler oluşmamışsa ya da biz yeniden oluşturmak istiyorsak aşağıdaki comment kısımları aktif hale getiririz ve komutu çalıştır.

```yml
#build:
#context: .
#dockerfile: Dockerfile
```

Evet Romalılar bu yazının ve de aynı zamanda yazı dizimizin sonuna gelmiş bulunmaktayız. Şimdiye kadar ki anlatımlardan anlayacağınız üzere ne Docker'ı ne deSpring Boot'un detayına inmedik çünkü amacımız bu iki konsepti entegre etmekti ve bunu da 6 yazı serisinde tüm hatlarıyla ele aldık.

Bu yazı serisinin başında da belirttiğim üzere bu seriyi yazarken

[Master Docker With Java-Devops For Spring Microservices](https://www.udemy.com/course/docker-course-with-java-and-spring-boot-for-beginners/)

kursunda tuttuğum notlardan ve kursun github adresindeki kodlardan faydalandım.

[Docker Crash Course Github ](https://github.com/in28minutes/docker-crash-course)

Bu seri daha çok kendim için bir kaynak oluşturmak ve takıldığım yerlerde bakabileceğim derli toplu bir doküman olması amacıyla yazıldı umarım sizlerinde bir nebze olsun işinize yaramıştır.

## Kaynaklar

[https://github.com/in28minutes/docker-crash-course](https://github.com/in28minutes/docker-crash-course)

[https://www.udemy.com/course/docker-course-with-java-and-spring-boot-for-beginners/](https://www.udemy.com/course/docker-course-with-java-and-spring-boot-for-beginners/)

[https://docs.docker.com/compose/](https://docs.docker.com/compose/)

