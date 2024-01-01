# FIRST PROJECT WEB STACK IMPLEMENTATION
## WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS

The LAMP stack is a widely-used technology stack for web development, comprising Linux (the operating system), Apache (the web server), MySQL (the relational database management system), and PHP/Perl/Python (the server-side scripting language). This open-source stack enables the creation of dynamic web applications, with Linux providing a stable foundation, Apache handling web server functions, MySQL managing the database, and PHP/Perl/Python facilitating server-side scripting for dynamic content generation. The seamless integration of these components supports the efficient development and deployment of robust and scalable web applications.

Since I'm the account owner, I'm going to create an IAM user for myself which I will be using for my day-to-day tasks.
Once I managed to log into AWS successfully, I automatically landed on the main console page which gives an overview of all the services.

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/15c8482c-a6a4-4798-b5e4-f06318a19c55)

Before proceeding with the creation of an EC2 instance, I first Selected the desired and closest region to me.
Let me list certain parameters when choosing an AWS region which factors should you consider:
- Latency and proximity
- Security and regulatory compliance
- Service Level Agreement (SLA)
- And finally the most important of all is the **Cost**

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/dc3c599e-8aa9-41ab-8e24-cdec26eb7a30)

Click on "Services" at the upper left corner and you will see all the Services available on AWS. then Click on "EC2"

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/4f1fd16c-bb81-46ec-9ba9-1d3bef0c20ec)

Select EC2, scroll down the page and then click on "Launch Instance"

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/efbc00c8-a5c9-457c-af0a-ee353eef2414)

Launch a new "EC2" instance of t2.micro family with Ubuntu Server 22.04 LTS(HVM) 64 bit

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/425b11c7-0e3e-4558-bdee-c0451a57ef15)

Click on Next: to choose an instance type

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/96a8e757-b163-4b8c-b8d2-cdd4ba760c18)

Before you launch an instance, you need to select a key pair which is then required for SSH access to the Server.
To create a new key-pair, Select "Create a new key-pair" from the drop down menu, give a name to the key-pair, and download it.
IMPORTANT - save your private key (.pem file) securely and do not share it with anyone! If you lose it, you will not be able
to connect to your server ever again!
As for me, I already created a key-pair, so I selected "Choose an existing key-pair" and i checked the acknowledgment box

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/34781384-4d8d-4534-a5fd-e4f3dc705b99)

Under network settings configure security groups. A security group acts as a virtual firewall for your EC2 instances to control incoming and outgoing traffic
Inbound rules control the incoming traffic to your instance, and outbound rules control the outgoing traffic from your instance. When you
launch an instance, you can specify one or more security groups.

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/dafd6ca3-dacf-4617-ba2f-1c9fe365e30d)

**Next**: Add Storage; to add or configure the Storage size and type. Every time you launch an instance from an AMI, a root storage device is created for that instance. The root storage device contains all the information necessary to boot the instance. You can specify storage volumes in addition to the root device volume when you create an AMI or launch an instance using block device

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/57d7d0f3-b696-4b0d-bc10-4f2482cc6df8)

**Next** Add Tags. A tag is a label that you assign to an AWS resource. ... Tags enable you to categorize your AWS resources in different ways, for
example, by purpose, ownership, or environment. For example, you could define a set of tags for your account's Amazon EC2 instances that helps
you track each instance's owner and stack level.

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/934a49f6-b5cb-48b0-afd1-29acab2f5eb2)

**Next** click on "Launch instance" to spin up the virtual server.

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/3d1832ea-7d90-4ca3-94d0-b26b1bb03f54)

The next phase is to view your instance state, click on your instance to view it. At this stage, you will see if your instance is running or still
pending. mine is running, now i can view my instance details

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/1eeebec0-e839-4512-ac4a-721532a29ecd)

**Next** is to connect my instance to an SSH Client. In my own case, am using MOBAXTERM, you can decide to use PUTTY or any other existing
ones. And to connect to MOBAXTERM, you need to copy the public ip address of the instance you want to ssh into.

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/62367a41-a19c-4afd-93fc-b4ab8b534418)

**Next** I opened my MOBAXTERM, I clicked on SESSION, SSH, on the REMOTE HOST I pasted my public-ip address I copied from my instance. 
I checked the box and specified the username as *ec2-user*

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/70d22a75-31b1-4851-8e27-378df61d5723)

**Next** I clicked on ADVANCE SSH settings, I checked the box for private key, and then I browsed my downloaded key.pem the next I clicked on **OK**.

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/fdd6ad38-9ef7-4421-8d7f-c52908d29ab5)

In order for a web application to work smoothly, it has to include an operating system, a web server, a database, and a programming language. The name LAMP is an acronym of the following:

~~~
* Linux Operating System
* Apache HTTP Server
* MySQL database management system
* PHP progamming language
~~~

**In this project, I will implement a web solution based on LAMP stack on a Linux server by implementing the 
steps below:**

# Step 1-Installing Apache and Updating the Firewall
- We need to Install Apache using Ubuntu’s package manager, apt:
~~~
$ sudo apt update

$ sudo apt install apache2
~~~
To verify that apache2 is running as a Service in our OS, use the following command:
~~~
 sudo systemctl status apache2
~~~

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/ff7341c7-d21a-4e86-aba8-99a2f1f31f85)

Before we can receive any traffic by our Web Server, we need to open TCP port 80 which is the default port that web browsers use to access
web pages on the Internet.

To open our port 80, I went back to the instance, clicked on security, clicked on edit inbound rules and I add rules. I added port 80 for http and port 443 https and I cliked on save rules

See my output:

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/9610a618-d790-4a66-a0e2-047448488bb0)

# Our server is running and we can access it locally and from the Internet (Source 0.0.0.0/0 means ‘from any IP address’).

- To verify and check that we can access it locally in our ubuntu shell and from the internet, run:
~~~
$ curl http://localhost:80
or
$ curl http://127.0.0.1:80
~~~

**As shown in the output you can see some strangely formatted test, do not worry, we just made sure that our Apache web service responds to ‘curl’
command with some payload.**

Open a web browser of your choice and try to access following url

~~~
http://<Public-ip-address>:80
~~~
- **Another way to retrieve your Public IP address, other than to check it in AWS Web Console, is to use the following command.**
~~~
$ curl -s http://169.254.169.254/latest/meta-data/public-ipv4
~~~
- If you see the following page, then your web server is now correctly installed and accessible through your firewall.
  
Here is my **output**:

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/0eafbf87-e838-4056-b90f-344a167a0d32)

# STEP 2 - Installing MySQL

## We need to install the database system to be able to store and manage data for our website. MySQL is a popular database management system used within PHP environments.

- To install mysql-server, run:
~~~
sudo apt -y install mysql-server
~~~
- After the installation it is recommended that we run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lockdown access to our database system.
- Start the interactive Script by running:
~~~
sudo mysql_secure_installation
~~~
- This will ask if you want to configure the VALIDATE PASSWORD PLUGIN.
  
# Note: Enabling this feature is something of a judgment call. If enabled, passwords which don’t match the specified criteria will be rejected by MySQL with an error. It is safe to leave validation disabled, but you should always use strong, unique passwords for database credentials.

Answer Y for yes, or anything else to continue without enabling.
~~~
VALIDATE PASSWORD PLUGIN can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD plugin?

Press y|Y for Yes, any other key for No:
~~~

- If you answer “yes”, you’ll be asked to select a level of password validation. Keep in mind that if you enter 2 for the strongest level, you will receive errors when attempting to set any password which does not contain numbers, uppercase and lowercase letters, and special characters, or which is based on common dictionary words.
  
### If we enabled password validation, we’ll be shown the password strength for the root password we just entered and our server will ask if we want to continue with that password. If we are happy with our current password, enter Y for “yes” at the prompt:

### For the rest of the questions, We press Y and hit the ENTER key at each prompt. This will remove some anonymous users and the test database, disable remote root logins, and load these new rules so that MySQL immediately respects the changes we have made.

When you are done, you will get the following **output**:

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/18900454-7291-428a-8dcd-e96b49ec8cb4)

- When you're finished, test if you're able to login to the MySQL Console by typing:
~~~
sudo mysql
~~~

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/941f0c7b-279e-4809-9922-c133d8a63772)

- To exit the MySQL Console, type exit as shown below:
~~~
mysql> exit
~~~
### Setting a password for the root MySQL account works as a safeguard, in case the default authentication method is changed from unix_socket to password. For increased security, it’s best to have dedicated user accounts with less expansive privileges set up for every database, especially if we plan on having multiple databases hosted on our server.

### *Note*: At the time of this writing, the native MySQL PHP library mysqlnd doesn’t support caching_sha2_authentication, the default authentication method for MySQL 8. For that reason, when creating database users for PHP applications on MySQL 8, we’ll need to make sure they’re configured to use mysql_native_password instead. We’ll demonstrate how to do that later.

### Our MySQL server is now installed, up and running and secured.

**Next**, we will install PHP, the final component in the LAMP Stack.

# Step 3 — Installing PHP
### We have Apache installed to serve our content and MySQL installed to store and manage our data. PHP is the component of our setup that will process the code to display dynamic content to the end user. In addition to the php package, we’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. We’ll also need libapache2-mod-php to enable Apache to handle PHP files. Core PHP packages will automatically be installed as dependencies.
- To install these 3 packages at once, run:
~~~
$ sudo apt -y install php libapache2-mod-php php-mysql
~~~
- Once the installation is finished, you can run the following command to confirm your PHP version:
~~~
php -v
~~~
![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/788574e5-04a3-4e12-9569-2d95a9527a6d)

- At this point, our LAMP stack is completely installed and fully operational.
- To test our setup with a PHP Script, it's best to setup a proper Apache Virtual Host to host our website's file and folders. Virtual Host allows you to have multiple websites hosted on a single machine and the end users of the websites will not even notice that.

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/930d92e9-7a66-49d6-b4e2-2c32cf5d2178)

- We will configure our first Virtual Host in the next step.

# Step 4 — Creating a Virtual Host for our Website using Apache

In this project, we will set up a domain called projectlamp, but this can be replaced by any other domain.

Apache on Ubuntu 20.04 has one server block enabled by default that is configured to serve documents from the /var/www/html directory. We will leave this configuration as is and will add our own directory next to the default one.

Create the directory for projectlamp using ‘mkdir’ command as follows:
~~~
sudo mkdir /var/www/projectlamp
~~~
**Next**, assign ownership of the directory with the $USER environment variable, which will reference our current user.
~~~
$ sudo chown -R $USER:$USER /var/www/projectlamp
~~~
- We can now open a new configuration file in Apache’s sites-available directory using our preferred command-line editor. Here, we’ll be using vi
~~~
$ sudo vi /etc/apache2/sites-available/projectlamp.conf
~~~
This will create a new blank file. Paste in the following bare-bones configuration by hitting on i on the keyboard to enter the insert mode, and paste the text:

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/dfdb4da5-99e8-459d-8c01-ab0341a201ed)

To save and close the file, simply follow the steps below:
- Hit the esc button on the keyboard
- Type `:`
- Type `:wq` w for write and q for quit
- Hit ENTER to save the file
**You can use the *ls* command to show the new file in the sites-available directory**
  ~~~
  $ sudo ls /etc/apache2/sites-available
  ~~~
  -Here is my output:
  
  ![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/67ebf84d-8514-4d58-badf-15f8f7240349)

### With this VirtualHost configuration, we’re telling Apache to serve propitixhomes.local using /var/www/projectlamp as the web root directory. If we like to test Apache without a domain name, we can remove or comment out the options ServerName and ServerAlias by adding a # character in the beginning of each option’s lines. Adding the # character there will tell the program to skip processing the instructions on those lines.

You can now use a2ensite command to enable the new virtual host:
~~~
$ sudo a2ensite projectlamp
~~~

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/29f4aeca-3b0b-4f63-b435-5bc95eb6f544)

- We might want to disable the default website that comes installed with Apache. This is required if we are not using a custom domain name, because in this case Apache’s default configuration would overwrite our virtual host. To disable Apache’s default website use a2dissite command, type:
~~~
$ sudo a2dissite 000-default
~~~

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/c5ff0e6f-fe80-4ce2-90fe-ec53309121ad)

- To make sure our configuration file doesn't contain syntax errors, we test this by running:
~~~
$ sudo apache2ctl configtest
~~~

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/78f56372-8c11-4207-ba65-2b9cab7eed07)

- Finally, we reload Apache so that these changes take effect:
~~~
$ sudo systemctl reload apache2
~~~
Our new website is now active, but the web root /var/www/projectlamp is still empty. Create an index.html file in that location so that we can test that the virtual host works as expected:
- Type and run :
~~~
$ sudo vi /var/www/projectlamp/index.html
~~~
Below is the input:

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/556aece0-28f3-42c5-b6ca-82cf81e18071)

Now go let's go our browser and try to open your website URL using IP address:
~~~
http://<public-ip-address>:80
~~~
or we can also browse using our public dns. The result is the same
~~~
http://<Public-DNS-Name>:80
~~~
See below **Output**

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/c5131032-78f3-4ac2-8fe8-54dbfe52ec1d)

We can leave this file in place as a temporary landing page for our application until we set up an index.php file to replace it. Once we do that, remember to remove or rename the index.html file from your document root, as it would take precedence over an index.php file by default.
- To check our Public IP from the Ubuntu shell, run :
~~~
$(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 
$(curl -s http://169.254.169.254/latest/meta-data/public-ipv4)
~~~

# Step 5 — Enable PHP on the website

With the default DirectoryIndex settings on Apache, a file named index.html will always take precedence over an index.php file. This is useful for setting up maintenance pages in PHP applications, by creating a temporary index.html file containing an informative message to visitors. Because this page will take precedence over the index.php page, it will then become the landing page for the application. Once maintenance is over, the index.html is renamed or removed from the document root, bringing back the regular application page.

In case you want to change this behavior, you’ll need to edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive:
~~~
sudo vim /etc/apache2/mods-enabled/dir.conf
~~~
#Change this: #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm #To this: DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
After saving and closing the file, you will need to reload Apache so the changes take effect:
~~~
$ sudo systemctl reload apache2
~~~
- Finally, we will create a PHP Script to test the PHP is correctly installed and configured on our Server.
- Now that we have a custom location to host our website's files and folders, we'll create a PHP test script to confirm that Apache is able to handle and process requests for PHP files.
- Create a new file named index.php inside our custom web root folder:
~~~
$ vim /var/www/projectlamp/index.php
~~~
This will open a blank file. Add the following text, which is valid PHP code, inside the file:
~~~
<?php
phpinfo();
~~~

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/ab356c3e-5863-48c0-a1a0-ce0955fc8665)

**When you are finished, save and close the file, refresh the page and you will see a page similar to this :**

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/5df1daee-29e1-4d98-8900-35f1ec42d87f)

- This page provides information about our server from the perspective of PHP. It is useful for debugging and to ensure that our settings are being applied correctly.
- Since we have been able to see this page in our browser, that means our PHP installation is working as expected.
- After checking the relevant information about our PHP server through that page, it’s best to remove the file we created as it contains sensitive information about our PHP environment -and our Ubuntu server. We can use rm command to do so:
~~~
$ sudo rm /var/www/projectlamp/index.php
~~~


  









































































