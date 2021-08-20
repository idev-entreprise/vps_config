|IMAGE | CONTAINER | CODE | PORT HOST | PORT CONTAINER | URL |
|---- | ----- | ----- | ---- | ---- | ---- |
|mysql:8|cnt_test_mysql8|C-CPM	| 13302	|3306|  |
|phpmyadmin:latest|cnt_test_phpmyadmin|C-CPP	| 17002	|80| [Phpmyadmin](http://62.141.41.189:17002) |
|kcafib|cnt_test_kcafib|C-CPKB	|	10582	|9090 | [kcafib](http://62.141.41.189:10582/swagger-ui.html) |
|kcafif|cnt_test_kcafif|C-CPKF	| 80	|80| [kcafif](http://localhost) |

### Docker network 
``` sh
 docker network create network_kcafi_test
```
 
### 1. Mysql
``` sh
docker run -d --name cnt_test_mysql8 --network network_kcafi_test -v /my/custom:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=passrootdocker -e MYSQL_DATABASE=dbclient -p 13302:3306  mysql:8
```
``` sql
mysql -uroot -proot
show databases;
use dbclient ;
select * from clients;
```
### 2. Phpmyadmin 
``` sh
docker run -d --name cnt_test_phpmyadmin --network network_kcafi_test --link cnt_test_mysql8:db -p 17002:80 -v /some/local/directory/config.user.inc.php:/etc/phpmyadmin/config.user.inc.php phpmyadmin
``` 

### 3. kcafib
``` sh
docker build -t kcafib_test:1.0 .
```
``` sh
docker tag kcafib_test:1.0 98687465/kcafib_test:1.0
```
``` sh
docker push 98687465/kcafib_test:1.0
```
[Docker hub ](https://hub.docker.com/)
``` sh
docker run -d --name cnt_test_kcafib --network network_kcafi_test  -p 10582:9090 98687465/kcafib_test:1.0
```
