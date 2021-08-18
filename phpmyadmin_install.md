# Phpmyadmin
**URL :** 
- [phpmyadmin](https://hub.docker.com/_/phpmyadmin).
### 1. Pull image and create container
``` sh
docker run -d --name container_phpmyadmin --link container_mysql:db -p 7000:80 -v /some/local/directory/config.user.inc.php:/etc/phpmyadmin/config.user.inc.php phpmyadmin
```




