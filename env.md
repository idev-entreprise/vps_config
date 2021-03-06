# Contents
   - [ENV dev-docker](#ENV-dev-docker) 
   - [ENV serv](#ENV-serv) 
   - [ENV serv-prod](#ENV-serv-prod) 
   - [ENV serv-test](#ENV-serv-test) 

# ENV dev-docker
|IMAGE | CONTAINER | CODE | PORT HOST | PORT CONTAINER | URL |
|---- | ----- | ----- | ---- | ---- | ---- |
|portainer/portainer-ce:latest|	cnt_portainer	|C-PORT	|9000	|9000| [Portainer](http://localhost:9000/#!/auth) |
|mysql:8|cnt_localhost_mysql8|C-CPM	| 13301	|3306|  |
|phpmyadmin:latest|cnt_localhost_phpmyadmin|C-CPP	| 17001	|80| [Phpmyadmin](http://localhost:17001) |
|kcafib|cnt_localhost_kcafib|C-CPKB	|	10581	|9090 | [kcafib](http://localhost:10581/swagger-ui.html) |
|kcafif|cnt_localhost_kcafif|C-CPKF	| 80	|80| [kcafif](http://localhost) |
### 1. Docker network 
``` sh
 docker network create network_kcafi_localhost
```
### 2. [Portainer Container](http://localhost:9000/#!/auth).
``` sh
docker volume create portainer_data
docker run -d -p 8000:8000 -p 9000:9000 --name=cnt_portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce
```
- **Username** : admin
- **Password** : Portainer.1.2.3.4.5.6.7.8.9

### 3. Mysql
``` sh
docker run -d --name cnt_localhost_mysql8 --network network_kcafi_localhost -v /my/custom:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=passrootdocker -e MYSQL_DATABASE=dbclient -p 13301:3306  mysql:8
```
### 4. [Phpmyadmin](http://localhost:17001)
``` sh
docker run -d --name cnt_localhost_phpmyadmin --network network_kcafi_localhost --link cnt_localhost_mysql8:db -p 17001:80 -v /some/local/directory/config.user.inc.php:/etc/phpmyadmin/config.user.inc.php phpmyadmin
``` 
### 5. kcafib
#### - [kcafib dev](http://localhost:10581/swagger-ui.html)
``` bash
mkdir C:\Users\Lenovo\Documents\GitHub\kcafi_backend
cd C:\Users\Lenovo\Documents\GitHub\kcafi_backend
git init
git remote add origin https://github.com/idev-entreprise/idev_caf_backend.git
git checkout -b main
git pull https://github.com/idev-entreprise/idev_caf_backend.git
```
``` sh
cd C:\Users\Lenovo\Documents\GitHub\kcafi_backend
mvn -DskipTests=true install
docker container stop cnt_localhost_kcafib
docker container rm cnt_localhost_kcafib
docker rmi -f  kcafib_dev:1.0 
docker build --build-arg PROFILE=dev-docker -t kcafib_dev:1.0 . 
docker run -d --name cnt_localhost_kcafib -e PROFILE=dev-docker  --network network_kcafi_localhost  -p 10581:9090 kcafib_dev:1.0 
```
### 6. kcafif
#### - [kcafif dev](http://localhost)
``` bash
mkdir C:\Users\Lenovo\Documents\GitHub\kcafi_frontend
cd C:\Users\Lenovo\Documents\GitHub\kcafi_frontend
git init
git remote add origin https://github.com/idev-entreprise/idev_caf_frontend.git
git checkout -b main
git pull https://github.com/idev-entreprise/idev_caf_frontend.git
```
``` sh
cd C:\Users\Lenovo\Documents\GitHub\kcafi_frontend
ng build  --configuration=dev
docker container stop cnt_localhost_kcafif
docker container rm cnt_localhost_kcafif
docker  rmi -f  kcafif_dev:1.0 
docker build --build-arg PROFILE=dev-docker -t kcafif_dev:1.0 . 
docker run -d --name cnt_localhost_kcafif -e PROFILE=dev-docker  --network network_kcafi_localhost  -p 80:80 kcafif_dev:1.0 
```






# ENV serv
|IMAGE | CONTAINER | CODE | PORT HOST | PORT CONTAINER | URL |
|---- | ----- | ----- | ---- | ---- |---- |
|jenkins/jenkins:jdk11	|cnt_jenkins|	C-JNKS|	8080	|8080 | [jenkins](http://62.141.41.189:8080) |
|portainer/portainer-ce:latest|	cnt_portainer	|C-PORT	|9000	|9000|[Portainer](http://62.141.41.189:9000/#!/auth) |

### 1. [Jenkins Container](http://62.141.41.189:8080) 
``` sh
docker run -d -p 8080:8080 -p 50000:50000 --name=cnt_jenkins -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts-jdk11
```
- **Username** : admin
- **Password** : Jenkins.1.2.3.4.5.6.7.8.9

### 2. [Portainer Container](http://62.141.41.189:9000/#!/auth)
``` sh
docker volume create portainer_data
docker run -d -p 8000:8000 -p 9000:9000 --name=cnt_portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce
```
- **Username** : admin
- **Password** : Portainer.1.2.3.4.5.6.7.8.9








# ENV serv-prod
|IMAGE | CONTAINER | CODE | PORT HOST | PORT CONTAINER | URL |
|---- | ----- | ----- | ---- | ---- | ---- |
|mysql:8|cnt_prod_mysql8|C-CPM	| 13301	|3306| |
|phpmyadmin:latest|cnt_prod_phpmyadmin|C-CPP	| 17001	|80|[Phpmyadmin](http://62.141.41.189:17001) |
|kcafib|cnt_prod_kcafib|C-CPKB	|	10581	|8080 | [kcafib](http://62.141.41.189:10581/swagger-ui.html) |
|kcafif|cnt_prod_kcafif|C-CPKF	| 80	|80|[kcafif](http://62.141.41.189) |
### 1. Docker network 
``` sh
 docker network create network_kcafi_prod
```
### 2. Mysql
``` sh
docker run -d --name cnt_prod_mysql8 --network network_kcafi_prod -v /my/custom:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=passrootdocker -e MYSQL_DATABASE=dbclient -p 13301:3306  mysql:8
``` 
### 3. [Phpmyadmin](http://62.141.41.189:17001) 
``` sh
docker run -d --name cnt_prod_phpmyadmin --network network_kcafi_prod --link cnt_prod_mysql8:db -p 17001:80 -v /some/local/directory/config.user.inc.php:/etc/phpmyadmin/config.user.inc.php phpmyadmin
``` 
### 4. [kcafib prod](http://62.141.41.189:10581/swagger-ui.html)
**Localhost**
``` sh
cd C:\Users\Lenovo\Documents\GitHub\kcafi_backend
mvn -DskipTests=true install
docker container stop cnt_prod_kcafib
docker container rm cnt_prod_kcafib
docker rmi 98687465/kcafib_prod:1.0
docker rmi kcafib_prod:1.0
docker build --build-arg PROFILE=serv-prod -t kcafib_prod:1.0 . 
docker tag kcafib_prod:1.0 98687465/kcafib_prod:1.0
docker push 98687465/kcafib_prod:1.0
```
**Serveur**
``` sh
docker container stop cnt_prod_kcafib
docker container rm cnt_prod_kcafib
docker rmi 98687465/kcafib_prod:1.0
docker run -d --name cnt_prod_kcafib -e PROFILE=serv-prod --network network_kcafi_prod  -p 10581:9090 98687465/kcafib_prod:1.0
```
### 5. [kcafif prod](http://62.141.41.189/)
**Localhost**
``` sh
cd C:\Users\Lenovo\Documents\GitHub\kcafi_frontend
ng build  --configuration prod
docker container stop cnt_prod_kcafif
docker container rm cnt_prod_kcafif
docker rmi 98687465/kcafif_prod:1.0
docker rmi kcafif_prod:1.0
docker build --build-arg PROFILE=serv-prod -t kcafif_prod:1.0 . 
docker tag kcafif_prod:1.0 98687465/kcafif_prod:1.0
docker push 98687465/kcafif_prod:1.0
```
**Serveur**
``` sh
docker container stop cnt_prod_kcafif
docker container rm cnt_prod_kcafif
docker rmi 98687465/kcafif_prod:1.0
docker run -d --name cnt_prod_kcafif -e PROFILE=serv-prod --network network_kcafi_prod  -p 80:80 98687465/kcafif_prod:1.0 
```










# ENV serv-test
|IMAGE | CONTAINER | CODE | PORT HOST | PORT CONTAINER | URL |
|---- | ----- | ----- | ---- | ---- | ---- |
|mysql:8|cnt_test_mysql8|C-CPM	| 13302	|3306| |
|phpmyadmin:latest|cnt_test_phpmyadmin|C-CPP	| 17002	|80|[Phpmyadmin](http://62.141.41.189:17002) |
|kcafib|cnt_test_kcafib|C-CPKB	|	10582	|8080 | [kcafib](http://62.141.41.189:10582/swagger-ui.html) |
|kcafif|cnt_test_kcafif|C-CPKF	| 4200	|80|[kcafif](http://62.141.41.189) |

### 1. Docker network 
``` sh
 docker network create network_kcafi_test
```
 
### 2. Mysql
``` sh
docker run -d --name cnt_test_mysql8 --network network_kcafi_test -v /my/custom:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=passrootdocker -e MYSQL_DATABASE=dbclient -p 13302:3306  mysql:8
``` 
### 3. [Phpmyadmin](http://62.141.41.189:17002) 
``` sh
docker run -d --name cnt_test_phpmyadmin --network network_kcafi_test --link cnt_test_mysql8:db -p 17002:80 -v /some/local/directory/config.user.inc.php:/etc/phpmyadmin/config.user.inc.php phpmyadmin
``` 

### 4. [kcafib test](http://62.141.41.189:10582/swagger-ui.html)
**Localhost**
``` sh
cd C:\Users\Lenovo\Documents\GitHub\kcafi_backend
mvn -DskipTests=true install
docker container stop cnt_test_kcafib
docker container rm cnt_test_kcafib
docker rmi 98687465/kcafib_test:1.0
docker rmi kcafib_test:1.0
docker build --build-arg PROFILE=serv-test -t kcafib_test:1.0 . 
docker tag kcafib_test:1.0 98687465/kcafib_test:1.0
docker push 98687465/kcafib_test:1.0
```
**Serveur**
``` sh
docker container stop cnt_test_kcafib
docker container rm cnt_test_kcafib
docker rmi 98687465/kcafib_test:1.0
docker run -d --name cnt_test_kcafib -e PROFILE=serv-test --network network_kcafi_test  -p 10582:9090 98687465/kcafib_test:1.0
```
### 5. [kcafif test](http://62.141.41.189:4200)
**Localhost**
``` sh
cd C:\Users\Lenovo\Documents\GitHub\kcafi_frontend
ng build  --configuration test
docker container stop cnt_test_kcafif
docker container rm cnt_test_kcafif
docker rmi 98687465/kcafif_test:1.0
docker rmi kcafif_test:1.0
docker build --build-arg PROFILE=serv-test -t kcafif_test:1.0 . 
docker tag kcafif_test:1.0 98687465/kcafif_test:1.0
docker push 98687465/kcafif_test:1.0
```
**Serveur**
``` sh
docker container stop cnt_test_kcafif
docker container rm cnt_test_kcafif
docker rmi 98687465/kcafif_test:1.0
docker run -d --name cnt_test_kcafif -e PROFILE=serv-test --network network_kcafi_test  -p 4200:80 98687465/kcafif_test:1.0 
```



