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



