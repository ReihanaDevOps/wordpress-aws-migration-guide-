<h1>WordPress Server Migration During Disaster Recovery (UAE Region)</h1>

<h2>1. Problem Scenario </h2>
During a disaster recovery event in the UAE region, the primary WordPress production server becomes inaccessible through its public interface. SSH access is only available via a private IP address within a restricted VPC network. Direct access from external networks is not possible, requiring the use of a bastion host (jump server) to securely connect to the private instance.

<h4>Key Challenges
<br></h4>

>No direct SSH access to the WordPress server.


>Need to securely transfer backups and configuration files.


>Requirement to restore the website on a new EC2 instance in another region or availability zone.

<h2>2. Solution Overview: Bastion Host / Jump Server</h2>
A bastion host acts as an intermediary to access private servers within a VPC. It provides a secure SSH tunnel for administrative operations.
<h4>
Answer :- 
I have log in to the server which have public IP and and ssh to that server which in same VPC and ssh from that server to my wordpress server to get the backup using wordpres's server Private IP.

Issue resolved ....!
</h4>
Now what ? Now I have to get the backups from wordpress server  , website and sql database backups .

<h4>Wordpress files Backup</h4>

For that I have used this command which is linux tar command

```bash
sudo tar -czvf wordpress-files.tar.gz /var/www/html
cat /var/www/html/wp-config.php
```
<h4>SQL access and get database backup</H4>

```bash
sudo mysql
mysqldump -u root payswi_site > payswi_site.sql
```
<h4>Before copying it, make sure it's a valid dump:</h4>

```bash
head -20 payswi_site.sql
```

<h2>get the existing database configurations :- </h2>

```bash

define('DB_NAME', 'wordpress');
define('DB_USER', 'wpuser');
define('DB_PASSWORD', 'password');
```

<h4>Remove unnecerry file </h4>

```bash
rm wordpress.sql
```

copy them with:

```bash
cp /wordpress-files.tar.gz /home/ubuntu/
cp /payswi_site.sql /home/ubuntu/
```
Using Tabby tool I have downloaded the files to my local file location , save as backups.

<H2>Next steps.....!</H2>

<h3>1. Launch a new EC2 instance in the target region </h3>

Requir Info:

Ubuntu 24.04 LTS- t3.micro with deafoult vpc and defoult configurations.

<h3>2. Install the required software Apache, PHP and MariaDB</h3>
   
Step 1: Install Apache
```bash
sudo apt install apache2 -y
```
Enable it:
```bash
sudo systemctl enable apache2
sudo systemctl start apache2
```
Step 2: Install PHP and required extensions
```bash
sudo apt install php php-cli php-common php-mysql php-curl php-gd php-mbstring php-xml php-zip php-intl php-bcmath php-soap php-imagick libapache2-mod-php -y
```
Check the version:

```bash
php -v
```
Step 3 : Install MariaDB
```bash
sudo apt install mariadb-server mariadb-client -y
```
Start and enable it:
```bash
sudo systemctl enable mariadb
sudo systemctl start mariadb
```
Verify:
```bash
sudo systemctl status mariadb
```

<h3>3. Upload the backups to file location in new server :- /home/ubuntu using Tabby upload file feature</h3>

Step 1: Create the WordPress database

Log in:

```bash
sudo mysql
```
Create the database and user. You'll need the same username and password that your original wp-config.php uses.

For example:
```bash
CREATE DATABASE pay_site;
CREATE USER 'pay_site'@'localhost' IDENTIFIED BY 'YourPassword';
GRANT ALL PRIVILEGES ON pay_site.* TO 'pay_site'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```
Important: If your original wp-config.php has a different database password, use that same password here. Otherwise, you'll need to update wp-config.php later.

Step 2: Import the database
```bash
mysql -u pay_site -p pay_site < /home/ubuntu/pay_site.sql
```
Enter the password you created in the previous step.

Step 3. Check wp-config.php

Verify these values match the new database:
```bash

DB_NAME
DB_USER
DB_PASSWORD
DB_HOST
```
Step 4: Restore the WordPress files

Remove the default Apache page:
```bash
sudo rm -rf /var/www/html/*
```

Extract your backup:
```bash
 [!IMPORTANT]
sudo tar -xzvf /home/ubuntu/wordpress-files.tar.gz -C /.
```
Step 5. Fix file permissions

```bash
sudo chown -R www-data:www-data /var/www/html
sudo find /var/www/html -type d -exec chmod 755 {} \;
sudo find /var/www/html -type f -exec chmod 644 {} \;
```

10. Restart services
```bash
sudo systemctl restart apache2
sudo systemctl restart mariadb
```

