# Web-Solution-With-Wordpress

## WordPress (WP, WordPress.org) is a free and open-source content management system (CMS) written in PHP[4] and paired with a MySQL or MariaDB database. Features include a plugin architecture and a template system, referred to within WordPress as Themes. WordPress was originally created as a blog-publishing system but has evolved to support other web content types including more traditional mailing lists and forums, media galleries, membership sites, learning management systems (LMS) and online stores. 


## To function, WordPress has to be installed on a web server, either part of an Internet hosting service like WordPress.com or a computer running the software package WordPress.org in order to serve as a network host in its own right.


## Three-tier Architecture

### Generally, web, or mobile solutions are implemented based on what is called the Three-tier Architecture.


![image](https://user-images.githubusercontent.com/85270361/134702570-9fe3ec31-4ba7-4545-8f47-1104eda0a93d.png)


## 3-Tier Setup

* A Laptop or PC to serve as a client
* An EC2 Linux Server as a web server (This is where you will install WordPress)
* An EC2 Linux server as a database (DB) server

**Three-tier Architecture** allows any one of the three tiers to be upgraded or replaced independently.

The user interface is implemented on any platform such as a desktop PC, smartphone or tablet as a native application, web app, mobile app, voice interface, etc. It uses a standard graphical user interface with different modules running on the application server.

The relational database management system on the database server contains the computer data storage logic.

The middle tiers are usually multitiered.

Since the three are not physical but logical in nature, they may run in different servers both in on-premises based solutions, as well as in software-as-a-service (SaaS).


## Benefit of a Three-Tier Architecture

* it provides great freedom to development teams who can independently update or replace only specific parts of the application without affecting the product as a whole.

* The application can be scaled up and out rather easily by detaching the front-end application from the databases that are selected according to the individual needs of the customer.

* New hardware, such as new servers, can also be added at a later time to deal with massive amounts of data or particularly demanding services.

* A three-tier-architecture also provides a higher degree of flexibility to enterprises who may want to adopt a new technology as soon as it becomes available.

* Critical components of the application can be encapsulated and retained while the whole system keeps evolving organically.

* Development cycle or upgrade times are significantly improved ensuring minimal disruption in customer’s experience.

* Different teams can work on different sections of the application rather than on the full stack according to their areas of expertise, improving their efficiency and speed.


The Three Tiers in a Three-Tier Architecture

### Presentation Tier

* Occupies the top level and displays information related to services commonly available on a web browser or web-based application in the form of a graphical user interface (GUI).

* It constitutes the front-end layer of the application and the interface with which end-users will interact directly.

* This tier is usually built on web development frameworks, such as CSS or JavaScript, and communicates with other tiers by sending results to the browser and other tiers in the network through API calls.

### Application Tier

* This tier — also called the middle tier, business logic or logic tier — is pulled from the presentation tier.

* It controls the application’s core functionality by performing detailed processing and is usually coded in programming languages, such as Python, Java, C++, .NET, etc.

### Data Tier

* Houses database servers where information is stored and retrieved.

* Data in this tier is kept independent of application servers or business logic, and is managed and accessed with programs, such as MongoDB, Oracle, MySQL, and Microsoft SQL Server.


# In this project, We will have the hands-on experience that showcases Three-tier Architecture while also ensuring that the disks used to store files on the Linux servers are adequately partitioned and managed through programs such as `gdisk` and `LVM` respectively. The following steps will be followed to achieve our aim and objectives of this project.



# LAUNCH AN EC2 INSTANCE THAT WILL SERVE AS “WEB SERVER”.

## Step 1 — Prepare the Web Server

1. **Launch an EC2 instance that will serve as "Web Server". Create 3 volumes in the same AZ as your Web Server EC2, each of 10 GiB.**

2. **Attach all three volumes one at a time to your Web Server EC2 instance**

3. **Open MobaXterm and connect into your EC2 instance using the public IP Address and begin configuration and use the `lsblk` command to inspect what block devices are attached to the server**
   
```
# lsblk
```

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/49459b52-c3ba-41a2-989d-83134e97bd97)


4. **Use `df -h` command to see all mounts and free space on your server**
```
df -h 
```

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/c59aa7e6-56c0-4a8c-ac7e-75abfe303987)

5. **Use the `gdisk` utility to create a single partition on each of the 3 disks**
 
```
sudo gdisk /dev/xvdf
```

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/9d5b3e62-a82e-4b27-8313-f4ada06fe513)


```
sudo gdisk /dev/xvdg
```

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/972c5b17-ef9d-43e4-bd54-78754e5af4d9)


```
sudo gdisk /dev/xvdh
```

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/73b3f3ad-6763-466e-8d75-f3876b363c17)


6. **We will use the `lsblk`command to view the newly configured partition on each of the 3 disks**


```
# lsblk
```

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/e24c59e4-90d6-469a-ab5a-03c51c040afa)


7. **We install `lvm2` package using `sudo yum install lvm2` and then run the `sudo lvmdiskscan`command to check for available storage for Logical Volume Manager (LVM)**

```
# sudo lvmdiskscan
```



8. **Use the `pvcreate` utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM**

```
sudo pvcreate /dev/xvdf1
sudo pvcreate /dev/xvdg1
sudo pvcreate /dev/xvdh1
```

```
sudo pvs
```


![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/16524e49-a00b-48e9-b6af-3a657d5df38d)


9. **Use the `vgcreate` utility to add all 3 PVs to a volume group (VG). Name the VG webdata-vg**


```
sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1
```

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/5b24d963-8c35-4bff-9842-6a0892896fb0)


10. **Verify that your VG has been created successfully by running**

```
sudo vgs
```

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/c85cb08c-c118-49e3-a028-ec6bec12778e)


11. **Use the `lvcreate` utility to create 2 logical volumes. `apps-lv` (*Use half of the PV size*), and `logs-lv` *Use the remaining space of the PV size*. NOTE: apps-lv will be used to store data for the Website while, logs-lv will be used to store data for logs.**


```
sudo lvcreate -n apps-lv -L 14G webdata-vg
sudo lvcreate -n logs-lv -L 14G webdata-vg
```

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/687d478c-955e-42e7-9f6b-5b1a434a81c9)


12. **Verify that your Logical Volume has been created successfully**


![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/724eed37-abef-4e42-a01d-81a1cae2d210)


13. **Verify the entire setup**

```
sudo vgdisplay -v #view complete setup - VG, PV, and LV
sudo lsblk 
```

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/26a9571c-0be2-4b19-99ec-163bf9fea297)
![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/ed7ee1ff-6157-433a-aef1-7602165f7c4f)


14. **Use `mkfs.ext4` to format the logical volumes with *ext4* filesystem**

```
sudo mkfs -t ext4 /dev/vg-webdata/app-lv
sudo mkfs -t ext4 /dev/vg-webdata/logs-lv
```

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/e60b1a48-4e8a-4eaf-b50a-08869d4d97b1)


15. **Create */var/www/html* directory to store website files**

```
sudo mkdir -p /var/www/html
```

16. **Create */home/recovery/logs* directory to store backup of logs data**

```
sudo mkdir -p /home/recovery/logs
```

17. **Mount */var/www/html* on *apps-lv* logical volume**


```
sudo mount /dev/vg-webdata/app-lv /var/www/html/
```

18. **Use the `rsync` utility to backup all the files in the log directory */var/log* into the */home/recovery/logs* directory (This is required before mounting the filesystem)**

```
sudo rsync -av /var/log/. /home/recovery/logs/
```


19. **Mount /var/log on *logs-lv* logical volume. (Note that all the existing data on /var/log will be deleted. That is why step 18 above is very important)**

```
sudo mount /dev/vg-webdata/logs-lv /var/log
```

19. **Restore log files back into */var/log* directory**

```
sudo rsync -av /home/recovery/logs/log/ /var/log
```


20. **Update `/etc/fstab` file so that the mount configuration will persist after restart of the server.**


21. **The UUID of the device will be used to update the /etc/fstab file;**

```
sudo blkid
```

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/36d61d90-3b35-46e6-bb2b-a0746e934121)

```
sudo vi /etc/fstab
```

22. **Update /etc/fstab in this format using your own UUID and rememeber to remove the leading and trailing quotes.**

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/2016b06b-01bc-4571-9f5b-acd806bfb459)


23. **Test the configuration and reload the daemon**

```
sudo mount -a
sudo systemctl daemon-reload
```

24.** Verify your setup by running `df -h`, output must look like this:**


```
df -h 
```

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/2cda8220-14a2-4824-ae81-e4a2b186104c)


## Step 2 — Prepare the Database Server

1* **Launch a second RedHat EC2 instance that will have a role – ‘DB Server’. Repeat the same steps as for the Web Server, but instead of `app-lv` create `db-lv` and mount it to `/db` directory instead of `/var/www/html/`**

2* **Create and Attach all three volumes one by one to your DB Server EC2 instance**

3* **Open MobaXterm and connect Ec2 instances using public IP Address and begin configuration**



* **We need to create a single partition on each of the 3 disks we added**

```
sudo gdisk /dev/xvdf
sudo gdisk /dev/xvdg
sudo gdisk /dev/xvdh
```


<img width="617" alt="first for database" src="https://user-images.githubusercontent.com/85270361/135596418-5eb0ddc5-878d-4758-80ed-4558a1851512.PNG">

<img width="754" alt="second for database" src="https://user-images.githubusercontent.com/85270361/135596464-8f316205-5761-477c-815b-69081e75ec6f.PNG">

<img width="748" alt="third for database" src="https://user-images.githubusercontent.com/85270361/135596627-84557816-62b5-4c2c-9a20-10781fd06799.PNG">



* **We will use the command below to view the newly configured partition on each of the 3 disks**


```
$ sudo lsblk
```



<img width="420" alt="sudo lblk" src="https://user-images.githubusercontent.com/85270361/135604005-2e91b51f-c233-43fc-b80d-9723fe84a18c.PNG">



* **We use this command to check for available storage for Logical Volume Manager (LVM)**


```
$ sudo lvmdiskscan
```


<img width="379" alt="livscan" src="https://user-images.githubusercontent.com/85270361/135591419-ff220a9f-994c-465f-ba66-ec302ecf10ad.PNG">


* **Use pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM**


```
sudo pvcreate /dev/xvdf1
sudo pvcreate /dev/xvdg1
sudo pvcreate /dev/xvdh1
```


* **Verify that your Physical volume has been created successfully by running**


```
sudo pvs
```

<img width="374" alt="pvs" src="https://user-images.githubusercontent.com/85270361/135592258-9367afc6-0537-45b2-a53c-d9dfcd2bc912.PNG">


* **Use vgcreate utility to add all 3 PVs to a volume group (VG). Name the VG database-vg**


```
sudo vgcreate vg-database /dev/xvdh1 /dev/xvdg1 /dev/xvdf1
```

<img width="471" alt="sudo vgcrearte db" src="https://user-images.githubusercontent.com/85270361/135602228-3238dc9f-2fe4-43be-ba9e-3445f45bb807.PNG">



* **Verify that your VG has been created successfully by running**


```
sudo vgs
```

<img width="360" alt="database vcg" src="https://user-images.githubusercontent.com/85270361/135598420-542ad595-ddb7-475f-854c-7550e7322794.PNG">



* **Use lvcreate utility to create 2 logical volumes. apps-lv (Use half of the PV size), and logs-lv Use the remaining space of the PV size. NOTE: apps-lv will be used to store data for the Website while, logs-lv will be used to store data for logs.**


```
sudo lvcreate -n apps-lv -L 14G database-vg
sudo lvcreate -n logs-lv -L 14G database-vg
```



* **Verify that your Logical Volume has been created successfully by running**


```
sudo lvs
```

<img width="561" alt="database lvm" src="https://user-images.githubusercontent.com/85270361/135598878-8e91912e-41d0-42ae-a784-79873e579c7a.PNG">



* **Verify the entire setup**


```
sudo vgdisplay -v #view complete setup - VG, PV, and LV
sudo lsblk 
```


<img width="517" alt="db lsblk" src="https://user-images.githubusercontent.com/85270361/135598831-568ac86f-1a45-4675-b177-5b4a863e7520.PNG">



* **Use mkfs.ext4 to format the logical volumes with ext4 filesystem**

```
sudo mkfs -t ext4 /dev/vg-database/db-lv
```


<img width="482" alt="mkfs db" src="https://user-images.githubusercontent.com/85270361/135601938-3db6cfb5-a995-4dca-b2cd-ba09f91db192.PNG">



* **We need to create /db directory to store our database files**

```
$ sudo mkdir /db
```


* **We need to create /home/recovery/logs to store backup of log data**


```
$ sudo mkdir -p /home/recovery/logs
```


* **We need to Mount /db on db-lv logical volume**


```
sudo mount /dev/vg-database/db-lv /db
df -h
```

<img width="466" alt="db vl" src="https://user-images.githubusercontent.com/85270361/135603121-44ac41ac-b9e0-4f04-ada2-e8b3a262f134.PNG">


* **We need to backup all the files in the log directory /var/log into /home/recovery/logs (This is required before mounting the file system)**


```
$ sudo rsync -r /var/log/ /home/recovery/logs/
```


* **We need to Mount /var/log on logs-lv logical volume**


```
$ sudo mount /dev/dbdata-vg/logs-lv /var/log
```


* **We need to restore log files back into /var/log directory.**


```
$ sudo cp -r /home/recovery/logs/.* /var/log
```


* **We need to update the /etc/fstab file so that the mount configuration will persist upon restart of the server.**



## Step 3 — Install WordPress on your Web Server EC2


1* **Update the repository**


```
sudo yum -y update
```

2* **Install wget, Apache and it’s dependencies**


```
sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json
```


3* **Start Apache**


```
sudo systemctl enable httpd
sudo systemctl start httpd
sudo systemctl status httpd
```

4* **To install PHP and it’s depemdencies**


```
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
sudo yum module list php
sudo yum module reset php
sudo yum module enable php:remi-7.4
sudo yum install php php-opcache php-gd php-curl php-mysqlnd
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
setsebool -P httpd_execmem 1
```

5* **Restart Apache**


```
sudo systemctl restart httpd
```

6* **Download wordpress and copy wordpress to var/www/html**


```
  mkdir wordpress
  cd   wordpress
  sudo wget http://wordpress.org/latest.tar.gz
  sudo tar xzvf latest.tar.gz
  sudo rm -rf latest.tar.gz
  cp wordpress/wp-config-sample.php wordpress/wp-config.php
  cp -R wordpress /var/www/html/
  ```
  
  7* **Configure SELinux Policies**


```
sudo chown -R apache:apache /var/www/html/wordpress
sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R
sudo setsebool -P httpd_can_network_connect=1
```


## Step 4 — Install MySQL on your DB Server EC2


```
sudo yum update
sudo yum install mysql_server_installation
```


<img width="479" alt="Capture 1" src="https://user-images.githubusercontent.com/85270361/135607628-ff550122-e678-4aa5-8253-ebe91f36b4ec.PNG">


* **Verify that the service is up and running**


```
sudo systemctl restart mysqld
sudo systemctl enable mysqld
```

## Step 5 — Configure DB to work with WordPress


```
sudo mysql
CREATE DATABASE wordpress;
CREATE USER `myuser`@`<Web-Server-Private-IP-Address>` IDENTIFIED BY 'wordpres';
GRANT ALL ON wordpres.* TO 'myuser'@'<Web-Server-Private-IP-Address>';
FLUSH PRIVILEGES;
SHOW DATABASES;
exit
```


## Step 6 — Configure WordPress to connect to remote database.

* Do not forget to open MySQL port 3306 on DB Server EC2. For extra security, you shall allow access to the DB server ONLY from your Web Server’s IP address, so in the Inbound Rule configuration specify source as /32


1* **Install MySQL client and test that you can connect from your Web Server to your DB server by using mysql-client**


```
sudo yum install mysql
sudo mysql -h <DB-Server-Private-IP-address> -u wordpres -p
```

<img width="577" alt="connecting web to db mysql" src="https://user-images.githubusercontent.com/85270361/135610631-715cadfd-5232-4d45-8763-530b44be7016.PNG">



2* **Verify if you can successfully execute SHOW DATABASES; command and see a list of existing databases.**


<img width="576" alt="last show databases" src="https://user-images.githubusercontent.com/85270361/135610838-c288e92f-2dea-44a5-9253-b732ff08b1c4.PNG">


3* **Change permissions and configuration so Apache could use WordPress:**


4* **Enable TCP port 80 in Inbound Rules configuration for your Web Server EC2 (enable from everywhere 0.0.0.0/0 or from your workstation’s IP)**


5* **Try to access from your browser the link to your WordPress http://<Web-Server-Public-IP-Address>/wordpress/
  
  
<img width="885" alt="first wordpress page" src="https://user-images.githubusercontent.com/85270361/135611523-517d013a-9466-4894-b399-92104d527aed.PNG">


<img width="960" alt="welcome to wordpress pass" src="https://user-images.githubusercontent.com/85270361/135611591-3d25af8f-d706-4871-b4cb-fa659c118e2a.PNG">
