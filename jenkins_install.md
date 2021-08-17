# jenkins/jenkins

### 1. Docker Pull Command
``` sh
docker pull jenkins/jenkins
```
### 2. Jenkins Container
``` sh
docker run -d -v jenkins_home:/var/jenkins_home -p 8080:8080 -p 50000:50000 jenkins/jenkins:lts-jdk11
```

### New Portainer installation

- **Username** : admin
- **Password** : Portainer.1.2.3.4.5.6.7.8.9






