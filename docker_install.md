# Install Docker Engine on Ubuntu

### 1. Uninstall old versions
```shell
sudo apt-get remove docker docker-engine docker.io containerd runc
```
### 2. Set up the repository
```shell
sudo apt-get update
```

```sh
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```
## 3. Add Docker’s official GPG key
``` sh
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg 
```

## 4. Set up the stable repository
``` sh
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

## 5. Install Docker Engine
``` sh
sudo apt-get update
```
``` sh
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

## 6. Verify that Docker Engine is installed correctly by running the hello-world image
``` sh
sudo docker run hello-world
```
