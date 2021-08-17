# Install Docker Engine on Ubuntu

## Uninstall old versions
``` sh
sudo apt-get remove docker docker-engine docker.io containerd runc
```
## Set up the repository
``` sh
sudo apt-get update
```

``` sh
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```
## Add Docker’s official GPG key
``` sh
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg 
```

## Set up the stable repository
``` sh
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

## Install Docker Engine
``` sh
sudo apt-get update
```
``` sh
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

## Verify that Docker Engine is installed correctly by running the hello-world image
``` sh
sudo docker run hello-world
```
