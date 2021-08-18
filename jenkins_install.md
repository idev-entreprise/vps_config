# jenkins/jenkins
**URL :** 
- [jenkins/jenkins](https://hub.docker.com/r/jenkins/jenkins).
- [Usage](https://github.com/jenkinsci/docker/blob/master/README.md).
### 1. Docker Pull Command
``` sh
docker pull jenkins/jenkins:lts-jdk11
```
### 2. Jenkins Container
``` sh
docker run -d -v jenkins_home:/var/jenkins_home -p 8080:8080 -p 50000:50000 --name=jenkins jenkins/jenkins:lts-jdk11
```
``` sh
docker run -d -v jenkins_home:/var/jenkins_home -p 8080:8080 -p 50000:50000 --name=jenkins jenkins/jenkins:jdk11
```
docker pull 

### New Portainer installation

- **Username** : admin
- **Password** : Jenkins.1.2.3.4.5.6.7.8.9

---


# How To Install Jenkins on Ubuntu 20.04
**URL :** 
- [Jenkins on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-install-jenkins-on-ubuntu-20-04).

### Step 1 — Installing Jenkins
``` sh
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
```

``` sh
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
```

``` sh
sudo apt update
```


``` sh
sudo apt install jenkins
```

### Step 2 — Starting Jenkins

``` sh
sudo systemctl start jenkins
```

``` sh
sudo systemctl status jenkins
```

``` sh
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
