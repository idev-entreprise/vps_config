|IMAGE | CONTAINER | CODE | PORT HOST | PORT CONTAINER |
|---- | ----- | ----- | ---- | ---- |
|portainer/portainer-ce:latest|	cnt_portainer	|C-PORT	|9000	|9000|

### 1. Portainer Container
``` sh
docker volume create portainer_data
```
```sh
docker run -d -p 8000:8000 -p 9000:9000 --name=cnt_portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce
```


**URL :** [Portainer](http://localhost:9000/#!/auth).

- **Username** : admin
- **Password** : Portainer.1.2.3.4.5.6.7.8.9

---

|IMAGE | CONTAINER | CODE | PORT HOST | PORT CONTAINER |
|---- | ----- | ----- | ---- | ---- |
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
docker run -d --name cnt_localhost_mysql8 --network network_kcafi_localhost -v /my/custom:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=passrootdocker -e MYSQL_DATABASE=dbclient -p 13301:3306  mysql:8
```
``` sql
mysql -uroot -proot
show databases;
use dbclient ;
select * from clients;
```
### 2. Phpmyadmin 
``` sh
docker run -d --name cnt_localhost_phpmyadmin --network network_kcafi_localhost --link cnt_localhost_mysql8:db -p 17001:80 -v /some/local/directory/config.user.inc.php:/etc/phpmyadmin/config.user.inc.php phpmyadmin
``` 

**URL :** [Phpmyadmin](http://localhost:17001).

### 3. kcafib

``` sh
docker build -t kcafib:1.0 .
```
``` sh
docker run -d --name cnt_localhost_kcafib --network network_kcafi_localhost  -p 10581:3200 98687465/kcafib:1.0
```
**URL :** [kcafib](http://localhost:10581/swagger-ui.html).
``` sh
docker tag kcafib:1.0 98687465/kcafib:1.0
```
``` sh
docker push 98687465/kcafib:1.0
```
