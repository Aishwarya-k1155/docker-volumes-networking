# docker-volumes-networking
# Docker Volumes & Networking

## Objective
The purpose of this task is to understand how Docker handles data persistence and container networking.  
By default, containers are ephemeral, meaning any data stored inside a container is lost when the container is removed.  
Docker volumes are used to persist data independently of the container lifecycle.  
Docker networking allows multiple containers to communicate with each other securely.

---

## Tools Used
- Docker (Primary)
- Git Bash (CLI on Windows)

---

## Part 1: Docker Volumes (Data Persistence)

### Problem
Data stored inside a container is lost when the container is deleted.

### Solution
Docker volumes store data outside the container lifecycle, ensuring persistence even after container removal.

---

### Step 1: Create a Docker Volume
docker volume create app_data

## Verify the volume:
docker volume ls
docker volume inspect app_data

## Step 2: Attach Volume to a Container
docker run -d \
--name web1 \
-v app_data:/usr/share/nginx/html \
-p 8080:80 \
nginx

## Step 3: Store Data Inside the Volume
docker exec -it web1 bash
echo "Hello from Docker Volume" > /usr/share/nginx/html/index.html
exit

## Access the application in a browser:
arduino
http://localhost:8080

## Step 4: Remove Container and Verify Persistence
docker rm -f web1
Run a new container using the same volume:

docker run -d \
--name web2 \
-v app_data:/usr/share/nginx/html \
-p 8081:80 \
nginx

## Access:
arduino
http://localhost:8081

## Result:
The data is still available, proving that Docker volumes persist data even after container deletion.

## Part 2: Docker Networking (Container Communication)
Objective
Enable communication between multiple containers using a custom Docker network.

## Step 5: Create a Custom Docker Network
docker network create my_bridge_net

##
Verify:
docker network ls
docker network inspect my_bridge_net

## Step 6: Run Containers on the Custom Network
Container 1 (nginx with network alias)
docker run -d \
--name app1 \
--network my_bridge_net \
--network-alias backend \
nginx

## Container 2 (busybox for testing)
docker run -it \
--name app2 \
--network my_bridge_net \
busybox sh

## Step 7: Test Container-to-Container Communication
Inside app2:
ping backend

## Result:
Successful ping confirms communication using Docker network aliases.

## Architecture Diagram
### Volume Persistence
Browser
   |
Nginx Container
   |
Docker Volume (app_data)

## Networking
app1 (nginx)  <---->  app2 (busybox)
       Custom Docker Bridge Network
