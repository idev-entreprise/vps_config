# jenkins/jenkins
**URL :** 
- [jenkins/jenkins](https://hub.docker.com/r/jenkins/jenkins).
- [Usage](https://github.com/jenkinsci/docker/blob/master/README.md).
### 1. Docker Pull Command
``` sh
docker pull jenkins/jenkins
```
### 2. Jenkins Container
``` sh
docker run -d -v jenkins_home:/var/jenkins_home -p 8080:8080 -p 50000:50000 --name=jenkins jenkins/jenkins
```

### New Portainer installation

- **Username** : admin
- **Password** : Jenkins.1.2.3.4.5.6.7.8.9






