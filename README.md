# Docker workshop


## Install
[Tutorial link](https://docs.docker.com/engine/install/)  

```console
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
```

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
```console
systemctl daemon-reload
systemctl restart docker
```


## First Image

```console
docker pull redis
docker image list
docker run --name rds -p 9000:6379 redis
docker ps
sudo apt install redis-tools
redis-cli -p 9000
```

Output:  
```
127.0.0.1:9000> SET test "Hello"
OK
127.0.0.1:9000> GET test
"Hello"
```

`-a` is for attach
```console
docker stop rds
docker start -a rds
docker rm `docker ps -a -q`
docker run --name rds -p 9000:6379 -d redis
docker exec -it rds bash
docker exec rds redis-cli GET test
```
