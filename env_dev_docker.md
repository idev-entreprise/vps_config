|IMAGE | CONTAINER | CODE | PORT HOST | PORT CONTAINER | URL |
|---- | ----- | ----- | ---- | ---- | ---- |
|portainer/portainer-ce:latest|	cnt_portainer	|C-PORT	|9000	|9000| [Portainer](http://localhost:9000/#!/auth) |

### 1. Portainer Container  [Portainer](http://localhost:9000/#!/auth).
``` sh
docker volume create portainer_data
```
```sh
docker run -d -p 8000:8000 -p 9000:9000 --name=cnt_portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce
```
- **Username** : admin
- **Password** : Portainer.1.2.3.4.5.6.7.8.9

---

|IMAGE | CONTAINER | CODE | PORT HOST | PORT CONTAINER | URL |
|---- | ----- | ----- | ---- | ---- | ---- |
|mysql:8|cnt_localhost_mysql8|C-CPM	| 13301	|3306|  |
|phpmyadmin:latest|cnt_localhost_phpmyadmin|C-CPP	| 17001	|80| [Phpmyadmin](http://localhost:17001) |
|kcafib|cnt_localhost_kcafib|C-CPKB	|	10581	|8080 | [kcafib](http://localhost:10581/swagger-ui.html) |
|kcafif|cnt_localhost_kcafif|C-CPKF	| 80	|80| [kcafif](http://localhost) |



### Docker network 
``` sh
 docker network create network_kcafi_localhost
```
 
### 1. Mysql
``` sh
docker run -d --name cnt_localhost_mysql8 --network network_kcafi_localhost -v /my/custom:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=passrootdocker -e MYSQL_DATABASE=dbclient -p 13301:3306  mysql:8
```
``` sql
mysql -uroot -proot
show databases;
use dbclient ;
select * from clients;
```
### 2. Phpmyadmin [Phpmyadmin](http://localhost:17001)
``` sh
docker run -d --name cnt_localhost_phpmyadmin --network network_kcafi_localhost --link cnt_localhost_mysql8:db -p 17001:80 -v /some/local/directory/config.user.inc.php:/etc/phpmyadmin/config.user.inc.php phpmyadmin
``` 
### 3. kcafib

### - [kcafib dev](http://localhost:10581/swagger-ui.html)

``` sh
docker build --build-arg PROFILE=dev-docker -t kcafib_dev:1.0 . 
docker run -d --name cnt_localhost_kcafib -e PROFILE=dev-docker  --network network_kcafi_localhost  -p 10581:9090 kcafib_dev:1.0 
```


### - [kcafib test](http://http://62.141.41.189:10582/swagger-ui.html)

``` sh
docker build --build-arg PROFILE=serv-test -t kcafib_test:1.0 . 
docker tag kcafib_test:1.0 98687465/kcafib_test:1.0
docker push 98687465/kcafib_test:1.0
```
``` sh
docker run -d --name cnt_test_kcafib -e PROFILE=serv-test --network network_kcafi_test  -p 10581:9090 98687465/kcafib_test:1.0
```


### - [kcafib prod](http://http://62.141.41.189:10581/swagger-ui.html)

``` sh
docker build --build-arg PROFILE=serv-prod -t kcafib_prod:1.0 . 
docker tag kcafib_prod:1.0 98687465/kcafib_prod:1.0
docker push 98687465/kcafib_prod:1.0
```
``` sh
docker run -d --name cnt_prod_kcafib -e PROFILE=serv-test --network network_kcafi_prod  -p 10581:9090 98687465/kcafib_test:1.0
```



