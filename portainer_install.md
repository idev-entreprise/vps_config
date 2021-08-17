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
docker run -d -p 9001:9001 --name portainer_agent --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /var/lib/docker/volumes:/var/lib/docker/volumes portainer/agent
```
