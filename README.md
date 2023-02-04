# Docker workshop


## Install
[Tutorial link](https://docs.docker.com/engine/install/)  

```shell
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin

sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
docker run hello-world
```
![](assets/1.png)  

## Mirror Registry
[Tutorial link](https://docker.ir/)  
A Docker registry is a system for versioning, storing and distributing Docker images. DockerHub is a hosted registry used by default when installing the Docker engine, but there are other hosted registries available for public use such as AWS and Google's own registries.

Add this to `/etc/docker/daemon.json`

```json
{
    "registry-mirrors": ["https://registry.docker.ir"]
}
```

and then restart docker
```shell
systemctl daemon-reload
systemctl restart docker
```


## First Image

```shell
docker pull redis
docker images
docker run --name rds -p 9000:6379 redis
docker ps
sudo apt install redis-tools
redis-cli -p 9000
```
![](assets/2.png)  


`-a` is for attach
```shell
docker stop rds
docker start -a rds
docker rm `docker ps -a -q`
docker run --name rds -p 9000:6379 -d redis
docker exec -it rds bash
docker exec rds redis-cli GET test
```
![](assets/3.png)  
![](assets/4.png)  
![](assets/5.png)  
![](assets/6.png)  

## Serving site with nginx
### Dockerfile
```dockerfile
FROM nginx
COPY frontend/default.conf /etc/nginx/conf.d/default.conf
COPY frontend/*.html /usr/share/nginx/html/
COPY frontend/*.css /usr/share/nginx/html/
COPY frontend/*.ttf /usr/share/nginx/html/
COPY frontend/*.js /usr/share/nginx/html/
COPY frontend/img/* /usr/share/nginx/html/img/
```

### Config file
```
server {
    listen 80;

    location /{
        charset utf-8;
        root /usr/share/nginx/html;
    }
}
```

```shell
docker build -t frontend -f frontend/Dockerfile .
docker run --name nginx -p 9050:80 frontend
# Go to http://localhost:9050/
# Another way:
docker run -v  $(pwd)/frontend:/usr/share/nginx/html -v $(pwd)/frontend/default.conf:/etc/nginx/conf.d/default.conf -p 9050:80 nginx
``` 
![](assets/7.png)  
![](assets/8.png)  