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

