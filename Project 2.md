# LEMP STACK IMPLEMENTATION (LEMP STACK)
- I already had an AWS account and proceeded to launch an EC2 instance
- I created a new EC2 Instance of t2.micro family with Ubuntu Server 22.04 LTS (HVM) 64bit
- I downloaded and configured my git
- Launched Git Bash and ran the following command:
~~~
ssh -i "PBL.pem" ubuntu@ec2-50-16-32-87.compute-1.amazonaws.com
~~~
It will look like this:

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/38c5bd17-f0fa-4c3f-a075-0e69c5950814)

# Step 1 - Installing the Nginx Web Server

- In order to display web pages to our site visitors we are going to employ Nginx, a high performance Web Server. We'll use the apt Package Manager to install this package.

- Since this is our first time using apt for this session, start of by updating your server's package index. Following that, you can use apt install to get Nginx installed:
~~~
$ sudo apt update

$ sudo apt install nginx
~~~
To verify that nginx was successfully installed and running as a service in Ubuntu, run:
~~~
$ sudo systemctl status nginx
~~~

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/0cfba252-2585-4d68-bbed-15dc1056490d)

- As shown in the above screenshot the server is active and running - meaning we have just launched our first web Server in the Cloud!
- Before we can receive any traffic by our Web Server, we need to open TCP port 80 which is the default port that web browsers use to access web pages hosted on our server.
- As we know, we have TCP port 22 open by default on our EC2 machine to access it via SSH, so we need to add a rule to the EC2 security group to open inbound connection through port 80:

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/358b4502-9899-4c13-afec-6f38735de222)

- Our server is running and we can access it locally and from the Internet (Source 0.0.0.0/0 means ‘from any IP address’).
- First, let us try to check how we can access it locally in our Ubuntu shell, run:
~~~
$ curl http://localhost:80
or
$ curl http://127.0.0.1:80
~~~
![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/59814e33-c376-4485-a284-4121455ea56c)

These 2 commands above actually do pretty much the same thing.
- As seen in the output we observed some strangely formatted text, do not worry, we just made sure that our Nginx web service responds to ‘curl’ command with some payload.
We are going to open a web browser of our choice and try to access the following url:
~~~
http://<Public-IP-Address>:80
~~~
Another way to retrieve our Public IP address, other than to check it in AWS Web console, is to use the following command:
~~~
curl -s http://169.254.169.254/latest/meta-data/public-ipv4
~~~
The URL in browser shall also work if you do not specify port number since all web browsers use port 80 by default.
Our web server is now correctly installed and accessible through our firewall as shown below:

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/e98d826a-252b-42e3-87e9-62c0fa2302fa)

In fact, it is the same content that was previously output by the ‘curl’ command, but represented in an HTML format by our web browser.

# Step 2 - Installing MySQL Database

Now that we have a Web Server up and running we need to install a Database Management System (DBMS) tool to be able
to store and manage data for our site in a relational database. MySQL is a popular relational database management
system used within PHP environments, so we will use it in our project. Again we are going to use apt to acquire and install this software:
~~~
$ sudo apt install mysql-server
~~~
When prompted, confirm installation by typing Y, and then ENTER.
When the installation is finished, it's recommended that we run a security script that comes pre-installed with MySQL. 
This Script will remove some insecure default settings and lock down access to our database system. Let's begin the interactive script by running:
~~~
$ sudo mysql_secure_installation
~~~
This will ask if you want to configure the VALIDATE PASSWORD PLUGIN. Answer Y for "yes" or anything else to continue without enabling.
If you answered "yes", you'll be asked to select a level of password validation. Keep in mind that if you enter 2 for the strongest level you will receive errors when attempting to set any password which does not contain numbers, upper and lowercase letters, and special charachers, or which is based on common dictionary words.
Regardless of whether you chose to set up the VALIDATE PASSWORD PLUGIN, your Server will next ask you to select and confirm a password for MySQL root user. This is not to be confused with the system root. The database root user is an administrative user with full privileges over the database system. Even though the default authentication method for the MySQL root user dispenses the use of a password, even when one is set, you should define a strong password here as an additional safety measure.
If you enabled password validation, you'll be shown the password strength for the root password you just entered and your Server will ask if you want to continue with that password. If you are happy with your current password, enter Y for "yes" at the prompt.
For the rest of the questions, press Y and hit ENTER key at the prompt. This will remove some anonymous users and the test database, disable remote root logins, and load these new rules so that MySQL immediately adheres to the changes you have made.
When you are finished, test if you're able to log into the MySQL console by typing:
~~~
sudo mysql
~~~
This will connect to the MySQL Server as the administrative database user root, which is infered by the use of sudo when running this command.
You should see an output like this :

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/7f8e2b68-4ced-45e3-a204-a3ddca912827)

To exit the MySQL console, type:
~~~
mysql> exit
~~~
Notice that we didn't need to provide a password to connect as the root user, even though you have defined one when running the mysql_secure_installation script. That is because the default authentication method for the administrative MySQL user is unix_socket instead of password. Even though this might look like a security concern at first, it makes the database server more secure because the only users allowed to log is as the root MySQL user are the system users with sudo privileges connecting from the console or through an application running with the same privileges. In practical terms, that means you won't be able to use the administrative database root user to connect from your PHP application. Setting a password for the root MySQL accounts works as a safeguard, in case the default authentication method is changed from unix_socket to password.
For increased security, it's best to have dedicated user accounts with less expansive privileges set up for every database, especially if you plan on having multiple databases hosted on your server
Our MySQL Server is now installed and secured. Next, we will install PHP, the final component in the LEMP Stack

# STEP 3 - Installing PHP

We have Nginx installed to serve our content and MySQL installed to store and manage our data. Now we can install PHP to process code and generate dynamic content for the Web Server.

While Apache embeds the PHP interpreter in each request, Nginx requires an external program to handle PHP processing and acts as a bridge between the PHP interpreter itself and the Web Server. This allows for a better performance in most PHP-based websites, but it requires additional configuration. We'll need to install php-fpm, which stands for "PHP fastCGI Process Manager", and tell Nginx to pass PHP requests to this software for processing. Additionally we'll need php-mysql, a PHP module that allows PHP to communicate with SQL-based databases. Core PHP packages will automatically be installed as dependencies.
To install these 2 packages at once, run:
~~~
$ sudo apt install php-fpm php-mysql
~~~
When prompted, type Y and press ENTER to confirm installation. We now have our PHP components installed. Next, We will configure Nginx to use them

# Step 4 - Configuring Nginx to Use PHP Processor

When using the Nginx web server, we can create server blocks (similar to virtual hosts in Apache) to encapsulate configuration details and host more than one domain on a single server. We will use projectLEMP as an example domain name.

On Ubuntu 22.04, Nginx has one server block enabled by default and is configured to serve documents out of a directory at /var/www/html. While this works well for a single site, it can become difficult to manage if you are hosting multiple sites. Instead of modifying /var/www/html, we’ll create a directory structure within /var/www for the your_domain website, leaving /var/www/html in place as the default directory to be served if a client request does not match any other sites.

Let's create the root web directory for your_domain as follows:
~~~
$ sudo mkdir /var/www/projectLEMP
~~~
Next, assign ownership of the directory with the $USER environment variable, which will reference your current system user:
~~~
$ sudo chown -R $USER:$USER /var/www/projectLEMP
~~~
Then, open a new configuration file in Nginx’s sites-available directory using your preferred command-line editor. Here, we’ll use nano:
~~~
$ sudo nano /etc/nginx/sites-available/projectLEMP
~~~
This will create a new blank file. Paste in the following bare-bones configuration:
~~~
#/etc/nginx/sites-available/projectLEMP

server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}
~~~

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/a57037d4-055b-4567-be5c-741001cad029)

Here’s what each of these directives and location blocks do:

listen — Defines what port Nginx will listen on. In this case, it will listen on port 80, the default port for HTTP.

root — Defines the document root where the files served by this website are stored.

index — Defines in which order Nginx will prioritize index files for this website. It is a common practice to list index.html files with a higher precedence than index.php files to allow for quickly setting up a maintenance landing page in PHP applications. You can adjust these settings to better suit your application needs.

server_name — Defines which domain names and/or IP addresses this server block should respond for. Point this directive to your server’s domain name or public IP address.

location / — The first location block includes a try_files directive, which checks for the existence of files or directories matching a URI request. If Nginx cannot find the appropriate resource, it will return a 404 error.

location ~ .php$ — This location block handles the actual PHP processing by pointing Nginx to the fastcgi-php.conf configuration file and the php7.4-fpm.sock file, which declares what socket is associated with php-fpm.

location ~ /.ht — The last location block deals with .htaccess files, which Nginx does not process. By adding the deny all directive, if any .htaccess files happen to find their way into the document root ,they will not be served to visitors.

When you’re done editing, save and close the file. If you’re using nano, you can do so by typing CTRL+X and then y and ENTER to confirm.

Activate your configuration by linking to the config file from Nginx’s sites-enabled directory:
~~~
$ sudo ln -s /etc//nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
~~~
This will tell Nginx to use the configuration the next time it is reloaded. You can test your configuration for syntax errors by typing:
~~~
$ sudo nginx -t
~~~
You shall see following message:
~~~
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
~~~

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/e3c3a3c7-676b-4c41-a7c5-ebb9465e21a5)

If any errors are reported, go back to your configuration file to review its contents before continuing.

We also need to disable the default Nginx host that is currently configured to listen on port 80, for this run:
~~~
$ sudo unlink /etc/nginx/sites-enabled/default
~~~
When you are ready, reload Nginx to apply the changes:
~~~
$ sudo systemctl reload nginx
~~~

Our new website is now active, but the web root /var/www/projectLEMP is still empty. Let's Create an index.html file in that location so that we can test that our new server block works as expected:

Now let's go to our browser and try to open our website URL using the public IP address:
~~~
http://<Public-ip>:80
~~~
You can also access your website in your browser by public DNS name, not only by IP - try it out, the result must be the same (port is optional)
~~~
http://<Public-DNS-Name>:80
~~~
We can leave this file in place as a temporary landing page for our application until we set up an index.php file to replace it. Once we do that, remember to remove or rename the index.html file from your document root, as it would take precedence over an index.php file by default.

Oour LEMP stack is now fully configured. In the next step, we’ll create a PHP script to test that Nginx is in fact able to handle .php files within our newly configured website.

# Step 5 – Testing PHP with Nginx

Oour LEMP stack should now be completely set up.

At this point, our LEMP stack is completely installed and fully operational.

We can test it to validate that Nginx can correctly hand .php files off to our PHP processor.

We can do this by creating a test PHP file in our document root. Let's Open a new file called info.php within our document root in our text editor:
~~~
$ nano /var/www/projectLEMP/info.php
~~~
Type or paste the following lines into the new file. This is valid PHP code that will return information about our server:
~~~
<?php
phpinfo();
~~~

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/646bc031-e915-4c42-b9cb-aeb2973c4e15)

You can now access this page in your web browser by visiting the domain name or public IP address you’ve set up in your Nginx configuration file, followed by /info.php:
~~~
http://`server_domain_or_IP`/info.php
~~~
You will see a web page containing detailed information about your server:

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/4e60cb09-fa90-40ee-94f5-64977e100e67)

After checking the relevant information about your PHP server through that page, it’s best to remove the file you created as it contains sensitive information about your PHP environment and your Ubuntu server. You can use rm to remove that file:
~~~
$ sudo rm /var/www/your_domain/info.php
~~~
You can always regenerate this file if you need it later.

# STEP 6 - Retrieving Data from MySQL Database with PHP

In this step we will create a test database (DB) with simple “To do list” and configure access to it, so the Nginx website would be able to query data from the DB and display it.

At the time of this writing, the native MySQL PHP library mysqlnd doesn’t support caching_sha2_authentication, the default authentication method for MySQL 8. We’ll need to create a new user with the mysql_native_password authentication method in order to be able to connect to the MySQL database from PHP.

We will create a database named example_database and a user named example_user, but you can replace these names with different values.

First, connect to the MySQL console using the root account:
~~~
$ sudo mysql
~~~
To create a new database, run the following command from your MySQL console:
~~~
mysql> CREATE DATABASE `example_database`;
~~~
Now we can create a new user and grant him full privileges on the database we have just created.

The following command creates a new user named example_user, using mysql_native_password as default authentication method. We’re defining this user’s password as password, but you should replace this value with a secure password of your own choosing.
~~~
mysql>  CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';
~~~
Now we need to give this user permission over the example_database database:
~~~
mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';
~~~
Now we need to give this user permission over the example_database database:
~~~
mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';
~~~
This will give the example_user user full privileges over the example_database database, while preventing this user from creating or modifying other databases on your server.

Now exit the MySQL shell with:
~~~
mysql> exit
~~~
You can test if the new user has the proper permissions by logging in to the MySQL console again, this time using the custom user credentials:
~~~
$ mysql -u example_user -p
~~~
Notice the -p flag in this command, which will prompt you for the password used when creating the example_user user. 
After logging in to the MySQL console, confirm that you have access to the example_database database:
~~~
mysql> SHOW DATABASES;
~~~
This will give you the following output:
Output
~~~
+--------------------+
| Database           |
+--------------------+
| example_database   |
| information_schema |
| performance_schema |
+--------------------+
3 rows in set (0.07 sec)
~~~
Next, we’ll create a test table named **todo_list**. From the MySQL console, run the following statement:
~~~
CREATE TABLE example_database.todo_list (
mysql>     item_id INT AUTO_INCREMENT,
mysql>     content VARCHAR(255),
mysql>     PRIMARY KEY(item_id)
mysql> );
~~~

***NOTE, actual Syntax: CREATE TABLE example_database.todo_list (item_id INT AUTO_INCREMENT, content VARCHAR(255), PRIMARY KEY(item_id));***

Insert a few rows of content in the test table. You might want to repeat the next command a few times, using different VALUES:
~~~
mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");
~~~
To confirm that the data was successfully saved to your table, run:
~~~
mysql>  SELECT * FROM example_database.todo_list;
~~~
You’ll see the following output:
Output
~~~
+---------+-------------------------+
| item_id | content                 |
+---------+-------------------------+
|       1 | My first important item |
+---------+-------------------------+
1 row in set (0.00 sec)
~~~
After confirming that you have valid data in your test table, you can exit the MySQL console:
~~~
mysql> exit
~~~
Create a new PHP file in your custom web root directory using your preferred editor. We’ll use vi for that:
~~~
$ nano /var/www/projectLEMP/todo_list.php
~~~

The following PHP script connects to the MySQL database and queries for the content of the todo_list table, displays the results in a list. If there is a problem with the database connection, it will throw an exception.

Copy this content into your todo_list.php script:
~~~
<?php
$user = "example_user";
$password = "password";
$database = "example_database";
$table = "todo_list";

try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  echo "<h2>TODO</h2><ol>";
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo"<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PD0Exception $e) {
    print "Error!: " . $e->getMessage() . "<br/>;
    die();
}
~~~

I can now access this page in my web browser by visiting the domain name or public IP address configured for my website or buy using the default localhost, followed by /todo_list.php. ``` http://localhost/todo_list.php ``` You should see a page like this, showing the content I’ve inserted in your test table:

![image](https://user-images.githubusercontent.com/85270361/126221158-23f22489-e0a5-4bc4-8d53-a587b925dc12.png) 

That means our PHP environment is ready to connect and interact with our MySQL server. 

Congratulations! 

In this guide, we have built a flexible foundation for serving PHP websites and applications to our web visitors, using Nginx as web server and MySQL as database management system.



















