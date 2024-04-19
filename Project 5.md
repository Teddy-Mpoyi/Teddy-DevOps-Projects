# CLIENT-SERVER ARCHITECTURE with MySQL

## Client-Server Architecture

**Client-server architecture is an architecture of a computer network in which many clients (remote processors) request and receive service from a centralized server (host computer). Client computers provide an interface to allow a computer user to request services of the server and to display the results the server returns. Servers wait for requests to arrive from clients and then respond to them. Ideally, a server provides a standardized transparent interface to clients so that clients need not be aware of the specifics of the system (i.e., the hardware and software) that is providing the service. Clients are often situated at workstations or on personal computers, while servers are located elsewhere on the network, usually on more powerful machines. This computing model is especially effective when clients and the server each have distinct tasks that they routinely perform. In hospital data processing, for example, a client computer can be running an application program for entering patient information while the server computer is running another program that manages the database in which the information is permanently stored.**

**Client Server Architecture**

![image](https://user-images.githubusercontent.com/85270361/133891630-fe8bffb2-64bf-4e3a-8fa6-86ac0d0a9d20.png)


**At its simplest, the client/server architecture is about dividing up application processing into two or more logically distinct pieces. The database makes up half of the client/server architecture. The database is the “server”; any application that uses that data is a “client.” In many cases, the client and server reside on separate machines; in most cases, the client application is some sort of user-friendly interface to the database.**

![image](https://user-images.githubusercontent.com/85270361/133891791-09de575c-232e-4aa0-a07b-b80c9de989eb.png)

**A graphical representation of a simple client/server system.**

## TASK - Implement a Client Server Architecture using MySQL Database Management System (DBMS).

* We open up our terminal and install *curl* if it doesnt exist.

```
$ sudo apt -y install curl
```

## In this example, the linux terminal will be the client, while www.propitixhomes.com will be the server

* Send a request from client (Linux Terminal) with the *curl* command below


```
$ curl -Iv www.propitixhomes.com
```

## We should see the response from the remote server in the below output.

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/b3245d25-1807-40a9-b270-d8f164caa21d)

# In this project we will be Implementing a Client Server Architecture using a Database Management System (MySQL).

**The instructions below will be followed to complete the project.**

* We are going to create and configure two Linux-based virtual servers (or EC2 instances) on the AWS platform.

```
Server A - mysql server
Server B - mysql client
```

* On mysql server Linux Server, we will install the MySQL software.

```
$ sudo apt install mysql-server -y
```

* We need to configure our mysql server installation using this command

```
$ sudo mysql_secure_installation
```


![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/c211bbc3-3326-4b30-b5bd-1e18d27d5b72)


![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/521c5202-204a-4ff8-ac6d-8a9cdfee471c)


* To test that our mysql server installation is successful we issue the following command:


![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/84a5f2f8-befa-4ce4-9906-811829e7e071)



* Create a user on the mysql server with the following command:

```
CREATE USER 'teddy_mpoyi'@'%' IDENTIFIED WITH mysql_native_password BY 'password';
```

* Next we create a database named `test_db` by issueing the following command:

```
CREATE DATABASE test_db;
```
```
mysql> GRANT ALL ON test_db.* TO 'teddy_mpoyi'@'%' WITH GRANT OPTION;
```
```
FLUSH PRIVILEGES;
```

### **We should see the below output**


![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/f0a50764-1a53-43b2-abdd-1387d3f30563)


* **On mysql client Linux Server, we will install the MySQL client software.**

```
$ sudo apt install mysql-client -y
```

* Using the ip addr or ifconfig command, We will figure out the IP address of the mysql server Linux Server.

```
ifconfig
or
ipaddr
```

By default, both of your EC2 virual servers are located in the same local virtual network, so they can communicate with each other using their local ip addresses i.e their respective private IP addresses .
use the *mysql server's* local IP address to connect from the *mysql client* . MySQL Server uses TCP port 3306 by default, so you have to open it by creating a new entry in the 'inbound rules' of the '*MySQL server*' security groups.


![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/25fb43a6-cd0a-4079-90ea-93b5e359061b)


For extra security, do not allow all IP Addresses to reach '*MySQL Server*' allow access only to a specific local IP address in this case the '*MySQL Client*' IP address.

1* **You might need to configure MySQL Server to allow connections from remote hosts.**


```
$ sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
```

Replace '127.0.0.1' with '0.0.0.0' like this:

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/2521d2a7-ba17-4241-aff8-a02baf611e7a)

Next we need to restart the service by shooting the following command:
```
sudo systemctl restart mysql
```
From `MySQL Client` Linux server connect remotely to `MySQL Server` DataBase Engine without using `SSH`. You must use the `mysql` utility to perform this action.  


```
$ mysql -u teddy_mpoyi -h 172.31.19.198 -p
```

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/bc61ca2b-6d98-403a-a56a-8622ed6533ee)

Check that you have successfully connected to a remote MySQL Server and can perform MySQL Queries.

```
Show databases;
```
If you see an output similar to the below image, then you have succesfully completed this project. You have deployed a fully functional MySQL Client-Server set up. You can further play around with this set up and practise in creating/dropping databases & tables and inserting/selecting records to and from them.

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/6b8ce965-d172-43ce-aaa8-6a82ff72ea7f)

