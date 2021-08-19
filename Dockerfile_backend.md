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

# mysql -uroot -proot
# show databases;
# use dbclient ;
# select * from clients;

# docker network create app-network
# docker container run --network app-network --name mysql-container -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=dbclient -d mysql:8
# docker build -t frontend:1.0 .
# docker build -t backend:1.0 .
# docker container run --network app-network --name frontend-container -p 4200:80 -d frontend:1.0
# docker container run --network app-network --name backend-container -p 8080:8080 -d backend:1.0
