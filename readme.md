# Containerization of App

this app is based on react, nodejs and mongoDB

Features:

- All the layers are dockerised and exist on separate container
- nodejs container exposes API via port 80
- react container exposes port 3000
- react container supports dyamic change from local to container, we can update the container code locally
- mongo DB supports authentication via username/password

# with docker compose:

to run docker compose file(by default run in attached mode)

> docker-compose up

and to run in detached mode

> docker-compose up -d

to build image only

> docker-compose build

to tear down the containers, networks

> docker-compose down

to tear down the containers, networks including volumes

> docker-compose down -v

## without docker compose:

we need to perform following steps manually:

---

## Create Network

docker network create goals-net

---

## Run MongoDB Container

docker run --name mongodb \
 -e MONGO_INITDB_ROOT_USERNAME=root \
 -e MONGO_INITDB_ROOT_PASSWORD=secret \
 -v data:/data/db \
 --rm \
 -d \
 --network goals-net \
 mongo

---

## Build Node API Image

docker build -t goals-node .

---

## Run Node API Container

docker run --name goals-backend \
 -e MONGODB_USERNAME=root \
 -e MONGODB_PASSWORD=secret \
 -v logs:/app/logs \
 -v <folder-path>/backend:/app \
 -v /app/node_modules \
 --rm \
 -d \
 --network goals-net \
 -p 80:80 \
 goals-node

---

## Build React SPA Image

docker build -t goals-react .

---

## Run React SPA Container

docker run --name goals-frontend \
 -v <folder-path>frontend/src:/app/src \
 --rm \
 -d \
 -p 3000:3000 \
 -it \
 goals-react

---

## Stop all Containers

docker stop mongodb goals-backend goals-frontend
