# Installing Roboshop Docker and Kubernetes:

```
docker pull redis

```
```
docker tag redis:latest redis:v1.5

```
```
for i in mysql mongodb rabbitmq catalogue cart user shipping payment web dispatch debug ; do cd $i ; docker build -t $i:v1.5 . ; cd ..; done

```
# How to build and push the images in DockerHub?
```
docker login -u lingadevops  -p Welcome#100
```
```
docker build -t lingadevops/mysql:v1.5 .   && docker push lingadevops/mysql:v1.5
```
```
docker build -t lingadevops/mongodb:v1.5 . && docker push lingadevops/mongodb:v1.5
```
```
docker build -t lingadevops/rabbitmq:v1.5 . && docker push lingadevops/rabbitmq:v1.5
```
```
docker push lingadevops/redis:v1.5
```
```
docker build -t lingadevops/catalogue:v1.5 . && docker push lingadevops/catalogue:v1.5
```
```
docker build -t lingadevops/cart:v1.5 . && docker push lingadevops/cart:v1.5
```
```
docker build -t lingadevops/user:v1.5 . && docker push lingadevops/user:v1.5
```
```
docker build -t lingadevops/shipping:v1.5 . && docker push lingadevops/shipping:v1.5
```
```
docker build -t lingadevops/payment:1.1 . && docker push lingadevops/payment:v1.5
```
```
docker build -t lingadevops/web:v1.5 . && docker push lingadevops/web:v1.5
```
```
docker build -t lingadevops/dispatch:v1.5 . && docker push lingadevops/debug:v1.5
```
```
docker build -t lingadevops/debug:v1.5 . && docker push lingadevops/dispatch:v1.5
```

# Run the application in kubernetes
```
kubectl apply -f mysql/manifest.yaml
```
```
kubectl apply -f mongodb/manifest.yaml
```
```
kubectl apply -f rabbitmq/manifest.yaml
```
```
kubectl apply -f redis/manifest.yaml
```
```
kubectl apply -f catalogue/manifest.yaml
```
```
kubectl apply -f cart/manifest.yaml
```
```
kubectl apply -f user/manifest.yaml
```
```
kubectl apply -f shipping/manifest.yaml
```
```
kubectl apply -f payment/manifest.yaml
```
```
kubectl apply -f web/manifest.yaml
```
```
kubectl apply -f debug/manifest.yaml
```
```
kubectl apply -f dispatch/manifest.yaml
```


# Run the applications from Docker image
docker run -d -p 3306:3306 --name mysql --network roboshop mysql:v1.5

docker run -d -p 27017:27017 --name mongodb --network roboshop mongodb:v1.5

docker run -d -p 6379:6379 --name redis --network roboshop redis:latest

docker run -d -p 5672:5672 --name rabbitmq --network roboshop rabbitmq:v1.5

docker run -d -p 8081:8080 --name catalogue --network roboshop catalogue:v1.5

docker run -d -p 8082:8080 --name cart --network roboshop cart:v1.5

docker run -d -p 8083:8080 --name user --network roboshop user:v1.5

docker run -d -p 8084:8080 --ulimit nofile=65536:65536 --name shipping --network roboshop shipping:v1.5

docker run -d -p 8085:8080 --name payment --network roboshop payment:v1.5

docker run -d -p 80:80 --name web --network roboshop web:v1.5

docker run -d -p 8086:8080 --name dispatch --network roboshop dispatch:v1.5

docker run -d -p 81:81 --name debug --network roboshop debug:v1.5



