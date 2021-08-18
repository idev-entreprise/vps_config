# Mysql
**URL :** 
- [mysql](https://hub.docker.com/_/phpmyadmin).
### 1. Pull image and create container
``` sh
docker run -d --name container_mysql -v /my/custom:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=passrootdocker mysql:8
```




