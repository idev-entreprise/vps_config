---
#  `ENV dev-docker`
---

|IMAGE | CONTAINER | CODE | PORT HOST | PORT CONTAINER | URL |
|---- | ----- | ----- | ---- | ---- | ---- |
|portainer/portainer-ce:latest|	cnt_portainer	|C-PORT	|9000	|9000| [Portainer](http://localhost:9000/#!/auth) |
|mysql:8|cnt_localhost_mysql8|C-CPM	| 13301	|3306|  |
|phpmyadmin:latest|cnt_localhost_phpmyadmin|C-CPP	| 17001	|80| [Phpmyadmin](http://localhost:17001) |
|kcafib|cnt_localhost_kcafib|C-CPKB	|	10581	|8080 | [kcafib](http://localhost:10581/swagger-ui.html) |
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

``` sh
docker build --build-arg PROFILE=dev-docker -t kcafib_dev:1.0 . 
docker run -d --name cnt_localhost_kcafib -e PROFILE=dev-docker  --network network_kcafi_localhost  -p 10581:9090 kcafib_dev:1.0 
```
#### - [kcafib test](http://62.141.41.189:10582/swagger-ui.html)
``` sh
docker build --build-arg PROFILE=serv-test -t kcafib_test:1.0 . 
docker tag kcafib_test:1.0 98687465/kcafib_test:1.0
docker push 98687465/kcafib_test:1.0
```
``` sh
docker run -d --name cnt_test_kcafib -e PROFILE=serv-test --network network_kcafi_test  -p 10581:9090 98687465/kcafib_test:1.0
```
#### - [kcafib prod](http://62.141.41.189:10581/swagger-ui.html)

``` bash
mkdir C:\Users\Lenovo\Documents\GitHub\kcafi_backend
git init
git remote add origin https://github.com/idev-entreprise/idev_caf_backend.git
git pull -branch main https://github.com/idev-entreprise/idev_caf_backend.git
```

``` sh
docker build --build-arg PROFILE=serv-prod -t kcafib_prod:1.0 . 
docker tag kcafib_prod:1.0 98687465/kcafib_prod:1.0
docker push 98687465/kcafib_prod:1.0
```
``` sh
docker run -d --name cnt_prod_kcafib -e PROFILE=serv-test --network network_kcafi_prod  -p 10581:9090 98687465/kcafib_test:1.0
```
---
# `ENV serv`
---

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


---
# `ENV serv-prod`
---

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
``` sh
docker build --build-arg PROFILE=serv-prod -t kcafib_prod:1.0 . 
docker tag kcafib_prod:1.0 98687465/kcafib_prod:1.0
docker push 98687465/kcafib_prod:1.0
```
``` sh
docker run -d --name cnt_prod_kcafib -e PROFILE=serv-prod --network network_kcafi_prod  -p 10581:9090 98687465/kcafib_prod:1.0
```


---
# `ENV serv-test`
---

|IMAGE | CONTAINER | CODE | PORT HOST | PORT CONTAINER | URL |
|---- | ----- | ----- | ---- | ---- | ---- |
|mysql:8|cnt_test_mysql8|C-CPM	| 13302	|3306| |
|phpmyadmin:latest|cnt_test_phpmyadmin|C-CPP	| 17002	|80|[Phpmyadmin](http://62.141.41.189:17002) |
|kcafib|cnt_test_kcafib|C-CPKB	|	10582	|8080 | [kcafib](http://62.141.41.189:10582/swagger-ui.html) |
|kcafif|cnt_test_kcafif|C-CPKF	| 80	|80|[kcafif](http://62.141.41.189) |

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
``` sh
docker build --build-arg PROFILE=serv-test -t kcafib_test:1.0 . 
docker tag kcafib_test:1.0 98687465/kcafib_test:1.0
docker push 98687465/kcafib_test:1.0
```
``` sh
docker run -d --name cnt_test_kcafib -e PROFILE=serv-test --network network_kcafi_test  -p 10582:9090 98687465/kcafib_test:1.0
```




