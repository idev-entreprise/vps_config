# Installing Apache Maven on Ubuntu 20.04 with apt
**URL :** 
- [maven](https://linuxize.com/post/how-to-install-apache-maven-on-ubuntu-20-04/).
### 1. Update the package index
``` sh
sudo apt update
```
### 2. Install Maven
``` sh
sudo apt install maven
```
### 3. Verify the installation
``` sh
mvn -version
```
# Installing Apache Maven on Docker
``` sh
docker run -d -it --rm --name my-maven-project -v "$(pwd)":/usr/src/mymaven -w /usr/src/mymaven maven:3.3-jdk-8
```
