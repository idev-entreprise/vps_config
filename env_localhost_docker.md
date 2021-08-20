|IMAGE | CONTAINER | CODE | PORT HOST | PORT CONTAINER |
|---- | ----- | ----- | ---- | ---- |
|mysql:8|cnt_portainer|C-PORT	| 9000	|9000|
|mysql:8|cnt_localhost_mysql8|C-CPM	| 13301	|3306|
|phpmyadmin:latest|cnt_localhost_phpmyadmin|C-CPP	| 17001	|80|
|kcafib|cnt_prod_kcafib|C-CPKB	|	10581	|8080 |
|kcafif|cnt_prod_kcafif|C-CPKF	| 80	|80|



### Docker network 
``` sh
 docker network create network_kcafi_localhost
```
 
### 1. Mysql
``` sh
docker run -d --name cnt_prod_mysql8 --network network_kcafi_localhost -v /my/custom:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=passrootdocker -e MYSQL_DATABASE=dbclient -p 13301:3306  mysql:8
```
``` sql
mysql -uroot -proot
show databases;
use dbclient ;
select * from clients;
```
### 2. Phpmyadmin 
``` sh
docker run -d --name cnt_prod_phpmyadmin --network network_kcafi_localhost --link cnt_prod_mysql8:db -p 17001:80 -v /some/local/directory/config.user.inc.php:/etc/phpmyadmin/config.user.inc.php phpmyadmin
``` 

### 3. kcafib

``` sh
docker build -t kcafib:1.0 .
```
``` sh
docker run -d --name cnt_localhost_kcafib --network network_kcafi_localhost  -p 10581:3200 98687465/kcafib:1.0
```
``` sh
docker tag kcafib:1.0 98687465/kcafib:1.0
```
``` sh
docker push 98687465/kcafib:1.0
```
