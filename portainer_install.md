# portainer/portainer

### 1. Docker Pull Command
``` sh
docker pull portainer/portainer
```
### 2. Portainer Server Deployment
``` sh
docker volume create portainer_data
```
```sh
docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce
```

### New Portainer installation

- **Username** : admin
- **Password** : Portainer.1.2.3.4.5.6.7.8.9

