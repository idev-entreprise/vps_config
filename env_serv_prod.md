|IMAGE | CONTAINER | CODE | PORT HOST | PORT CONTAINER | URL |
|---- | ----- | ----- | ---- | ---- | ---- |
|mysql:8|cnt_prod_mysql8|C-CPM	| 13301	|3306| |
|phpmyadmin:latest|cnt_prod_phpmyadmin|C-CPP	| 17001	|80|[Phpmyadmin](http://62.141.41.189:17001) |
|kcafib|cnt_prod_kcafib|C-CPKB	|	10581	|8080 | [kcafib](http://62.141.41.189:10581/swagger-ui.html) |
|kcafif|cnt_prod_kcafif|C-CPKF	| 80	|80|[kcafif](http://62.141.41.189) |


### Docker network 
``` sh
 docker network create network_kcafi_prod
```
 
### 1. Mysql
``` sh
docker run -d --name cnt_prod_mysql8 --network network_kcafi_prod -v /my/custom:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=passrootdocker -e MYSQL_DATABASE=dbclient -p 13301:3306  mysql:8
```
``` sql
mysql -uroot -proot
show databases;
use dbclient ;
select * from clients;
```
### 2. Phpmyadmin 
``` sh
docker run -d --name cnt_prod_phpmyadmin --network network_kcafi_prod --link cnt_prod_mysql8:db -p 17001:80 -v /some/local/directory/config.user.inc.php:/etc/phpmyadmin/config.user.inc.php phpmyadmin
``` 

### 3. kcafib
``` sh
docker build -t kcafib_prod:1.0 .
```
``` sh
docker tag kcafib_prod:1.0 98687465/kcafib_prod:1.0
```
``` sh
docker push 98687465/kcafib_prod:1.0
```
[Docker hub ](https://hub.docker.com/)
``` sh
docker run -d --name cnt_prod_kcafib --network network_kcafi_prod  -p 10581:9090 98687465/kcafib_prod:1.0
```
