# Introduction

## Docker Hub
Container repositories: 

https://hub.docker.com/

## Pull, List and Run


if you want just download a image

```bash
docker pull ubuntu 
```

Now, i want to run this container

## Notes
in this moment, ubuntu just will run bash command and will exit, because there isn't a continuous job.

We can prove using a bash command 

```bash
docker run ubuntu sleep 1d

docker ps
CONTAINER ID   IMAGE     COMMAND      CREATED          STATUS          PORTS     NAMES
718a970cd2e1   ubuntu    "sleep 1d"   18 seconds ago   Up 17 seconds             
```

If you want to list all container

```bash
docker container ls -a

7a2bb388bc3f   ubuntu                                                 "/bin/bash"              3 seconds ago   Exited (0) 2 seconds ago
```

## Stop and re-execute container
```bash


docker stop 718a970cd2e1

docker ps
CONTAINER ID   IMAGE     COMMAND      CREATED          STATUS          PORTS     NAMES

docker start 718a970cd2e1

docker ps
CONTAINER ID   IMAGE     COMMAND      CREATED          STATUS          PORTS     NAMES
718a970cd2e1   ubuntu    "sleep 1d"   18 seconds ago   Up 17 seconds             
```

## Iterative mode

```bash
docker exec -it 718a970cd2e1 bash

root@718a970cd2e1:/# 
```

## Pause and Resume container

```bash
docker pause 718a970cd2e1

docker unpause 718a970cd2e1
```

## instant stop

```bash
docker stop -t=0 718a970cd2e1
```

## remove container

```bash
docker rm 718a970cd2e1
```

## Self destroy


```bash
docker exec -it ubuntu bash

root@718a970cd2e1:/# 
```

After leave, this container will be destroyed

## detach

```bash
docker run -d dockersamples/static-site
```

Container will create a port 80 and 443, but this is just inside. We need to expose this port to our host.


```bash
docker run -d -P dockersamples/static-site

CONTAINER ID   IMAGE                       COMMAND                  CREATED          STATUS          PORTS                                           NAMES
56ad98ce76ac   dockersamples/static-site   "/bin/sh -c 'cd /usr…"   12 seconds ago   Up 11 seconds   0.0.0.0:55001->80/tcp, 0.0.0.0:55000->443/tcp   flamboyant_moser
2ec26cd78f9e   ubuntu                      "sleep 1d"               16 minutes ago   Up 16 minutes

```

ignore ubuntu container, and lets show the sample container port

```bash
docker port 56ad98ce76ac

443/tcp -> 0.0.0.0:55000
80/tcp -> 0.0.0.0:55001
```

we can open in our browser: http://localhost:55001/

## Force remove

```bash
docker rm 56ad98ce76ac --force
```

## Expose especific port 

we can use `-p` flag with  `externalport:internalport`

```bash
docker run -d -p 8080:80 dockersamples/static-site
```

## Listing images

```bash
➜  ~ docker images
REPOSITORY                                      TAG             IMAGE ID       CREATED       SIZE
ubuntu                                          latest          a6be1f66f70f   2 weeks ago   69.2MB
dockersamples/static-site                       latest          f589ccde7957   6 years ago   191MB
```

## Inspect and History

Images are read-only, and container uses layers to compose many others containers.


```bash
docker inspect a6be1f66f70f

docker history a6be1f66f70f
```


### Extra Study

Imagine 1 container have 4 layers, and you create 10 containers based on the same container.

This wont create 4x10 layers. 

Each container re-use this 4 layers and add 1 read/write layer.

Its like a github repository, with 10 branches, and each branch just have the difference between main branch.

# Build container 

The example will be created on `node-example` folder.


```bash
cd node-example

docker build -t alexcastrodev/app-node:1.0 .

[...]
naming to docker.io/alexcastrodev/app-node:1.0
```


This command will use Dockerfile base to create a new image

With name alexcastrodev/app-node

and version: 1.0

and build directory: . (current)

```bash
➜  node-example docker images
REPOSITORY                                      TAG             IMAGE ID       CREATED              SIZE
alexcastrodev/app-node                          1.0             004c26bad02f   About a minute ago   952MB
```

Now, i will run my container

```bash
docker run -d -p 8080:3000 alexcastrodev/app-node:1.0
```