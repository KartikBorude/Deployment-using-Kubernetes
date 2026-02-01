# Deployment-using-Kubernetes

## Pre-requisites

-- RDS database (Mariadb)

--EKS cluster

--EC2 instance


step 1 Database setup
--Clone the repository
```shell
   git clone (repostories's url)
   git clone https://github.com/Rohit-1920/EasyCRUD-Updated.git
```

-- Enter in database using

```shell
   mysql -h (RDS endpoint) -u admin -p
```
enter the root password

create new database and user

```shell
CREATE DATABASE student_db;
GRANT ALL PRIVILEGES ON springbackend.* TO 'username'@'localhost' IDENTIFIED BY 'your_password';
```
replace the username and your_password which you have created

use that created database

```shell
use student_db;
```
creat table

```shell
CREATE TABLE `students` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  `email` varchar(255) DEFAULT NULL,
  `course` varchar(255) DEFAULT NULL,
  `student_class` varchar(255) DEFAULT NULL,
  `percentage` double DEFAULT NULL,
  `branch` varchar(255) DEFAULT NULL,
  `mobile_number` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=80 DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;
```
```shell
exit
```

## Now Backend

```shell
cd EasyCRUD-Updated/backend
```
```shell
cp src/main/resources/application.properties .
```
create dockerfile for backend

```shell
nano dockerfile
```
Dockerfile data

```shell
FROM maven:3.8.3-openjdk-17
COPY . /opt 
WORKDIR /opt
RUN rm -rf src/main/resources/application.properties
RUN cp -rf application.properties src/main/resources
RUN mvn clean package
WORKDIR /opt/target
EXPOSE 8080
CMD ["java" , "-jar" , "student-registration-backend-0.0.1-SNAPSHOT.jar"]
```
build backend image
```shell
docker build . -t backend:v1
```
--check the created image

```shell
docker images
```
login to dockerhub using

```shell
docker login -u username
```

tag image

```shell
docker tag old name (username)/newname
```
Push the image to the dockerhub using 

```shell
docker push (username)/backkend:v1
```
Now create Pod file for backend

```shell
nano pod.yaml
```
Pod file data

```shell
apiVersion: v1
kind: Pod
metadata:
  name: backend
  labels:
    app: backend
spec:
  containers:
    - name: backend
      image: kartikborude/backend:latest
      ports:
        - containerPort: 80
```
Create the service file for backend

```shell
nano svc.yaml
```

Data for svc


```shell
apiVersion: v1
kind: Service
metadata:
  name: backend-svc
spec:
  type: LoadBalancer
  selector:
    app: backend
  ports:
   - port: 8080
     targetPort: 8080
```

```shell
kubectl apply -f pod.yaml
kubectl apply -f svc.yaml
```

After that 
```shell
kubectl get svc
```
copy the generated DNS of backend and paste it in the .env file of frontend

## Now Frontend
```shell
cd ../frontend
```

Edit .env file

```shell
nano .env
```
instead of ip address paste the copied dns from backend

VITE_API_URL=http://(___):8080/api
VITE_API_URL=http://(ab0a4bbf1276142afbabac8db6418234-2033286948.ap-southeast-2.elb.amazonaws.com):8080/api

create dockerfile for frontend

```shell
nano dockerfile
```
data for dockerfile

```shell
FROM maven:3.8.3-openjdk-17
COPY . /opt 
WORKDIR /opt
RUN rm -rf src/main/resources/application.properties
RUN cp -rf application.properties src/main/resources
RUN mvn clean package
WORKDIR /opt/target
EXPOSE 8080
CMD ["java" , "-jar" , "student-registration-backend-0.0.1-SNAPSHOT.jar"]
```
build the image using
```shell
docker build . -t frontend:v1
```
tag image
```shell
docker tag old name (username)/newname
```
Push the image to the dockerhub using 

```shell
docker push (username)/frontend:v1
```
Create the Deployment file for frontend
```shell
nano deploy.yaml
```
 data for deploy file
```shell
apiVersion: apps/v1
kind: Deployment
metadata: 
    name: frontend 
    labels:
       app: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      name: frontend
      labels:
        app: frontend
    spec:
      containers:
        - name: frontend
          image: kartikborude/frontend:latest
          ports:
            - containerPort: 80
```
Create service ffile for frontend

```shell
nano svc.yaml
```
data for service file for frontend
```shell
apiVersion: v1
kind: Service
metadata:
  name: frontend-svc
spec:
  type: LoadBalancer
  selector:
    app: frontend
  ports:
  - port: 80
    targetPort: 80
```

Run both file using
```shell
kubectl apply -f deploy.yaml
kubectl apply -f svc.yaml 
```
The created DNS of frontend we have run on google or any browser 

DNS will be visible using
```shell
kubectl get svc
```



