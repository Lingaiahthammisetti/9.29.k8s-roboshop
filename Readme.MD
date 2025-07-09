# Installing Roboshop Docker and Kubernetes:

```
for i in mysql mongodb rabbitmq catalogue cart user shipping payment web dispatch debug ; do cd $i ; docker build -t $i:v1 . ; cd ..; done

```

```
docker pull redis

```

# Run the applications from Docker image
docker run -d -p 3306:3306 --name mysql --network roboshop mysql:v1.1

docker run -d -p 27017:27017 --name mongodb --network roboshop mongodb:v1.1

docker run -d -p 6379:6379 --name redis --network roboshop redis:latest

docker run -d -p 5672:5672 --name rabbitmq --network roboshop rabbitmq:v1.1

docker run -d -p 8081:8080 --name catalogue --network roboshop catalogue:v1.1

docker run -d -p 8082:8080 --name cart --network roboshop cart:v1.1

docker run -d -p 8083:8080 --name user --network roboshop user:v1.1

docker run -d -p 8084:8080 --ulimit nofile=65536:65536 --name shipping --network roboshop shipping:v1.1

docker run -d -p 8085:8080 --name payment --network roboshop payment:v1.1

docker run -d -p 80:80 --name web --network roboshop web:v1.1

docker run -d -p 8086:8080 --name dispatch --network roboshop dispatch:v1.1

docker run -d -p 81:81 --name debug --network roboshop debug:v1.1


# How to build and push the images in DockerHyb?

docker build -t lingadevops/mysql:v1.1 .

docker build -t lingadevops/mongodb:v1.1 .

docker build -t lingadevops/rabbitmq:v1.1 .

docker build -t lingadevops/catalogue:v1.1 .

docker build -t lingadevops/cart:v1.1 .

docker build -t lingadevops/user:v1.1 .

docker build -t lingadevops/shipping:v1.1 .

docker build -t lingadevops/payment:1.1 .

docker build -t lingadevops/web:v1.1 .

docker build -t lingadevops/dispatch:v1.1 .

docker build -t lingadevops/debug:v1.1 .

