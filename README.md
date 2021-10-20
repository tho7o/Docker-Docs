# DOCKERS


* Installing on Raspbian

```
sudo apt install raspberrypi-kernel raspberrypi-kernel-headers
curl -sSL https://get.docker.com | sh
sudo usermod -aG docker pi
```

* Installing on X86

```
https://docs.docker.com/engine/install/ubuntu/
```

## COMMANDS

* Show Docker Version
```
docker version
```

* Controlar el demonio
```
sudo systemctl start docker
sudo service docker start
```

* Create image from dockerfile
```
docker build -t <OS>:<tag> <.>
```
ex: docker build -t centos:prueba .

* listar imagen descargadas (after <pull>)
```
sudo docker image ls
sudo docker images | grep <name>
```

* Montar imagen con argumentos

```
sudo docker create --name=transmission -e PUID=1000 -e PGID=1000 -e TZ=America/Santiago -e USER=tho7o -e PASS=18406477 -p 9091:9091 -p 51413:51413 -p 51413:51413/udp linuxserver/transmission
```

* Show Root Dir DOCKER in you localhost
```
docker info | grep -i root
```

* Create Container

-d: run in background
-p: port PC:Container
--name: name container
-m: limit container ram memory (mb, gb) [show memory statics: docker stats <name_container>]
--cpuset-cpus <#> pueden ser de 0 a 3 si tienes 4. [show cpuinfo: grep "model name" /proc/cpuinfo] [show how many cpu you have: grep "model name" /proc/cpuinfo | wc -l]

```
docker run -d -p 3305:3305 --name my_container
```
* Create container with mini HTTPServer
```
docker run -d -p 8080:8080 centos python -m SimpleHTTPServer 8080
```

Create temporal container

--rm: lo borrará despues de salir
bash: entrará en el OS del containerdespués de crearlo

```
docker run --rm -d -p 8080:8080 centos bash
```

* Inspeccionar info containers

```
docker inspect <Container_Name>
```

* Arrancar container
```
sudo docker start <name_container or ID>
```

* list containers
```
sudo docker container ls <-a> (mostrará vivos y umertos)
docker ps
```

* List Last created containers
```
docker ps -l
```

* Remove a container without VOLUME
```
docker rm -f <container_ID>
```

* Remove a Container and volumen
```
docker rm -fv <name_container>
```

* Remove All Containers and volumen
```
docker rm -fv $(docker ps -aq)
```

* access to container Terminal

-u: acceder como un usuario determinado al OS ex: -u Ricardo

```
docker exec -ti <ID or Container_Name> bash
```

* Pasar un archivo dentro del contenedor al HOST
```
sudo docker cp <ID_Container>:/<path_dentro_del_container> <path_hots>
sudo docker cp <path_hots> <ID_Container>:/<path_dentro_del_container>
```

ex: sudo docker cp 6ab330acebaa:/downloads .


* Shows container consumption memory
```
docker stats <name_container>
```


* Show container logs
```
docker logs -f <name_container>
```

* VOLUMEN
Create VOLUME (will be created in /home/doish/docker/volumes)
este volumen no se borrará cuando se borre un container con el arg -fv
```
docker volume create <name_vol>
```

* Asign created volumen to a cantainer
```
docker run -d --name db -p 3306:3306 -e "MYSQL_ROOT_PASSWORD=1234567" -v <name_vol>:/var/lib/mysql mysql:5.7
```

* Create a container with a VOLUME shared between localhost and container
-v: <localhost_folder>:<container_folder>
-e: environment variable (for mysql in this case)
mysql: <version>
<mongo_image> : imagen descargada de mongo


```
docker run -d --name db -p 3306:3306 -e "MYSQL_ROOT_PASSWORD=1234567" -v /opt/mysql/://var/lib/mysql mysql:5.7
docker run -d --name mongo -p 27017:27017 -v <localhost_path_folder>:/data/db <mongo_image>
```

* VOLUME list
```
docker volume ls
```

* List dangling volumen (volumenes sin container)
-f: filter
```
docker ls -f dangling=true
```

* Remove dangling volumen
```
docker ls -f dangling=true | xargs docker volume rm
```

* NETWORKS
```
docker network create --help
```

* CREATE CUSTOMIZE NETWORK
```
docker network create <network_name>
docker network create -d bridge --subnet <172.124.10.0/24> --gateway <172.124.10.1> <network_name>
```

* SHOW ALL NETWORKS OPTIONS
```
docker network
```

* SHOW DEFAULT IP DOCKER (interface DOCKER0)
```
ip a | grep docker
```

* list all docker network by DEFAULT (por defecto se llama BRIDGE)
```
docker network ls | grep bridge
docker network inspect bridge
```

* ping between CONTAINERS
```
docker exect <Container_Name_origin> bash -c "ping <destiny_ip_Container>"
```

* Launch a command into docker without enter into it.
```
docker exec -d <docker_name> <command> 
```

## DOCKER COMPOSE

* Installation
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

* CREATE CONTAINERS FROM docker-compose.yml
```
docker-compose up -d
```

* REMOVE CONTAINERS
```
docker-compose down
```

## ERROR FIX:

* ERROR: Couldn't connect to Docker daemon at http+docker://localunixsocket - is it running?

```
sudo groupadd docker 
sudo gpasswd -a $USER 
docker newgrp docker # or relogin to make the new group known to the system
sudo chown $USER /var/run/docker.sock
```
