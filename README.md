# docker_lab2
lab2
# ITI - Docker Lab Two🐋
## Task 1: Run a container using nginx image, and mount a directory from your host into the Docker container.
example: /home/samy/nginx:/home/nginx (bind mount)

### Steps
#### 1. Create Bind Mount Directory
```bash
mkdir bind_mount_dir

```

#### 2. Run a container using nginx image
```bash
docker run -d --name bind_mount_dir -v /root/bind_mount_dir:/user/share/nginx/html nginx
docker exec -it bind_mount_dir bash
```

#### 3. Echo any content to show when curl ip-address
```bash
cd /user/share/nginx/html
echo "Hello iam bainh" > index.html
exit
docker inspect -f '{{.NetworkSettings.IPAddress}}' bind_mount_dir   ->172.17.0.3
curl 172.17.0.3
```

---
## Task 2: Create 2 docker network (net-1 & net-2)

### Steps
#### 1. Create 2 docker network (net-1 & net-2)
```bash
docker network create network1
docker network create network2
```

#### 2. Run 2 new containers using nginx:alpine image, and attach the net-1 to them.
```bash
docker run -d --name nginx_network1 --net network1 nginx:alpine
docker run -d --name nginx_network2 --net network2 nginx:alpine
```

#### 3. Run another 1 new containers using nginx:alpine image, and attach the net-2 to them.
```bash
docker run -d --name nginx_network3 --net network2 nginx:alpine
```

#### 4. Inspect the 3 containers to know their IPs and write them aside.
```bash
docker inspect -f '{{.NetworkSettings.Networks.network_1.IPAddress}}' nginx_network1
172.18.0.2

docker inspect -f '{{.NetworkSettings.Networks.network_1.IPAddress}}' nginx_network2
172.18.0.3

docker inspect -f '{{.NetworkSettings.Networks.network_2.IPAddress}}' nginx_network3
172.20.0.2
 docker inspect nginx_network1 | grep IPAddress
```

#### 5. Enter a container in the net-1 network and try to ping a container in the net-2 network (What do you notice?)
```bash
docker exec -it nginx_network1 sh 
Use ping or curl
ping 172.20.0.2
ping fails it is a same network
```

#### 6. Enter a container in the net-1 network and try to ping the other container in the same network (What do you notice?)
```bash
docker exec -it nginx_network1 sh 
curl 172.18.0.2
ping 172.18.0.3
ping success because exist in same netwoerk
```

## Task 3: Explain the difference between Docker volumes and Bind Mount.
Docker volumes are managed by Docker and provide more flexibility and portability, making them suitable for production environments.
Bind mounts provide a way to directly access files and directories on the host machine from within a container, making them useful for development and debugging but less portable.
Docker volumes are preferred for managing persistent data in Dockerized applications, while bind mounts are often used for sharing files and directories between the host and the container during development.

