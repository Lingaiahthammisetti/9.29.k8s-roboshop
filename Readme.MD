* Roboshop project components connections:
    * Web connecting catalogue, cart, user, shipping, payment, dispatch
    * catalogue connecting mongodb
    * cart connecting redis, catalogue
    * user connecting mongodb, redis
    * shipping connecting MySQL
    * payment connecting RabbitMQ

Trouble shooting k8s pods:
kubens roboshop
kubectl get pods

kubectl describe pod mysql-XXXXX
kubectl logs pod mysql-XXXX
kubectl exec -it mysql-XXXX -- bash

kubectl describe pod backend-XXXXX
kubectl logs pod backend-XXXX
kubectl exec -it backend-XXXX -- bash
mysql -h <mysql-service-name> -u root -pExpenseApp@1
telnet <mysql-service-name> 3306

kubectl describe pod frontend-XXXXX
kubectl logs pod frontend-XXXX
kubectl exec -it frontend-XXXX -- bash
telnet <backend-service-name> 8080
curl http://backend.daws78s.online:80

Test Connection from Shipping Pod
You can test from inside the shipping pod:
kubectl exec -it <shipping-pod-name> -- sh

# Inside the pod
telnet mysql 3306
# OR if MySQL client is available
mysql -h mysql -u root -p

* To check connections and data in a microservices-based Kubernetes app like the one you've described, you need to:
✅ Verify each service's connection to its dependency (e.g., cart to redis)
✅ Check the actual data in the dependent service (e.g., documents in mongodb, keys in redis, queues in RabbitMQ, etc.)

🔧 GENERAL APPROACH PER SERVICE
Let’s go one by one for each service you're working with:

🔹 Web connects to:
catalogue
cart
user
shipping
payment
dispatch

✅ Check connections from Web pod:
kubectl exec -it <web-pod> -- sh
curl http://catalogue:8080
curl http://cart:8080
curl http://user:8080
curl http://shipping:8080
curl http://payment:8080
curl http://dispatch:8080
Replace 8080 with the actual service port.

🔹 Catalogue connects to MongoDB
✅ Test connection from catalogue pod:
kubectl exec -it <catalogue-pod> -- sh
mongo mongodb://<user>:<password>@mongodb:27017/<dbname>
MongoDB URI usually comes from an environment variable.

✅ Check MongoDB data:
use catalogue
show collections
db.products.find().pretty()
🔹 Cart connects to Redis and Catalogue
✅ Test Redis connection:
kubectl exec -it <cart-pod> -- sh
redis-cli -h redis
PING
Should return PONG.

✅ Test connection to Catalogue (via HTTP):
curl http://catalogue:8080/products
🔹 User connects to MongoDB and Redis
✅ MongoDB:
kubectl exec -it <user-pod> -- mongo mongodb://<user>:<password>@mongodb:27017/users
use users
show collections
db.users.find().pretty()
✅ Redis:
kubectl exec -it <user-pod> -- redis-cli -h redis
KEYS *
🔹 Shipping connects to MySQL
✅ Test MySQL connection:
kubectl exec -it <shipping-pod> -- \
mysql -h mysql -u root -p
Or use your actual DB user/pass.

✅ Check data:
SHOW DATABASES;
USE shipping;
SHOW TABLES;
SELECT * FROM shipments LIMIT 5;
🔹 Payment connects to RabbitMQ
✅ Check RabbitMQ connection:
Use rabbitmqadmin, curl, or amqp-tools. If those aren't available in the pod, you can:
kubectl exec -it <payment-pod> -- sh
# Install or use a built-in AMQP client if available
Or access RabbitMQ Management UI if enabled:

http://rabbitmq:15672
# Login with user/pass
✅ Check queues:
rabbitmqadmin list queues
or via HTTP API:

curl -u guest:guest http://rabbitmq:15672/api/queues
✅ Summary: Connection Check Matrix
Service	Depends On	Command to Check Connection
Web	catalogue	curl http://catalogue:8080
Web	cart	curl http://cart:8080
Web	user	curl http://user:8080
Web	shipping	curl http://shipping:8080
Web	payment	curl http://payment:8080
Catalogue	MongoDB	mongo mongodb://...
Cart	Redis	redis-cli -h redis
Cart	Catalogue	curl http://catalogue:8080
User	MongoDB	mongo mongodb://...
User	Redis	redis-cli -h redis
Shipping	MySQL	mysql -h mysql -u root -p
Payment	RabbitMQ	curl http://rabbitmq:15672 or rabbitmqadmin


# Installing Roboshop Docker and Kubernetes:

```
docker pull redis

```
```
docker tag redis:latest redis:v1.5

```

# How to build and push the images in DockerHub?
```
docker login -u lingadevops  -p Welcome#100
```

```
docker build -t mysql:v1.5 ./mysql \
&& docker tag mysql:v1.5 lingadevops/mysql:v1.5 \
&& docker push lingadevops/mysql:v1.5

```
```
docker build -t mongodb:v1.5 ./mongodb \
&& docker tag mongodb:v1.5 lingadevops/mongodb:v1.5 \
&& docker push lingadevops/mongodb:v1.5

```
```
docker build -t rabbitmq:v1.5 ./rabbitmq \
&& docker tag rabbitmq:v1.5 lingadevops/rabbitmq:v1.5 \
&& docker push lingadevops/rabbitmq:v1.5
```
```
docker push lingadevops/redis:v1.5
```
```
docker build -t catalogue:v1.5 ./catalogue \
&& docker tag catalogue:v1.5 lingadevops/catalogue:v1.5 \
&& docker push lingadevops/catalogue:v1.5
```
```
docker build -t cart:v1.5 ./cart \
&& docker tag cart:v1.5 lingadevops/cart:v1.5 \
&& docker push lingadevops/cart:v1.5
```
```
docker build -t user:v1.5 ./user \
&& docker tag user:v1.5 lingadevops/user:v1.5 \
&& docker push lingadevops/user:v1.5
```
```
docker build -t shipping:v1.5 ./shipping \
&& docker tag shipping:v1.5 lingadevops/shipping:v1.5 \
&& docker push lingadevops/shipping:v1.5
```
```
docker build -t payment:v1.5 ./payment \
&& docker tag payment:v1.5 lingadevops/payment:v1.5 \
&& docker push lingadevops/payment:v1.5
```
```
docker build -t web:v1.5 ./web \
&& docker tag web:v1.5 lingadevops/web:v1.5 \
&& docker push lingadevops/web:v1.5
```
```
docker build -t debug:v1.5 ./debug \
&& docker tag debug:v1.5 lingadevops/debug:v1.5 \
&& docker push lingadevops/debug:v1.5
```
```
docker build -t dispatch:v1.5 ./dispatch \
&& docker tag dispatch:v1.5 lingadevops/dispatch:v1.5 \
&& docker push lingadevops/dispatch:v1.5
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


```
for i in mysql mongodb rabbitmq catalogue cart user shipping payment web dispatch debug ; do cd $i ; docker build -t $i:v1.5 . ; cd ..; done

```
