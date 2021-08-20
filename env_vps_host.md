|IMAGE | CONTAINER | CODE | PORT HOST | PORT CONTAINER |
|---- | ----- | ----- | ---- | ---- |
|jenkins/jenkins:jdk11	|cnt_jenkins|	C-JNKS|	8080	|8080 |
|portainer/portainer-ce:latest|	cnt_portainer	|C-PORT	|9000	|9000|

### 2. Jenkins Container
``` sh
docker run -d -v jenkins_home:/var/jenkins_home -p 8080:8080 -p 50000:50000 --name=jenkins jenkins/jenkins:lts-jdk11
```
- **Username** : admin
- **Password** : Jenkins.1.2.3.4.5.6.7.8.9

### 2. Portainer Container
``` sh
docker volume create portainer_data
```
```sh
docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce
```
- **Username** : admin
- **Password** : Portainer.1.2.3.4.5.6.7.8.9
