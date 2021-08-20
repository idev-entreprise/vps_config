# stage 1
``` dockerfile
FROM maven:3-jdk-8-alpine as create_artifact
WORKDIR /usr/src/app
COPY . /usr/src/app
RUN mvn clean install
RUN mvn package
```

#ENV PORT 5000
#EXPOSE $PORT
#CMD [ "sh", "-c", "mvn -Dserver.port=${PORT} spring-boot:run" ]

# stage 2

``` dockerfile
FROM openjdk:8-jdk-alpine
WORKDIR /app 
COPY --from=create_artifact /usr/src/app/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","/app/app.jar"]
# or ENTRYPOINT ["java","-jar","app.jar"]
```

### Create network  `network_kcafi`
``` sh
docker network create network_kcafi
```

### MYSQL 

``` sh
docker run  -d  --name container_mysql   --network network_kcafi   -e MYSQL_ROOT_PASSWORD=root  -e MYSQL_DATABASE=dbclient  -p 3307:3306  mysql:8
```

``` sql
mysql -uroot -proot
show databases;
use dbclient ;
select * from clients;
```
### Phpmyadmin 
``` sh
docker run -d --name container_phpmyadmin --network network_kcafi --link container_mysql:db -p 7000:80 -v /some/local/directory/config.user.inc.php:/etc/phpmyadmin/config.user.inc.php phpmyadmin
``` 

### Backend
``` sh
docker build -t backend:1.0 .
```
``` sh
docker run -d --network network_kcafi --name container_backend -p 8080:3200 backend:1.0
```
### Frontend
``` sh
docker build -t frontend:1.0 .
```
``` sh
docker run -d --network network_kcafi --name container_frontend -p 4200:80 frontend:1.0
```



