<h1>Request  :- </h1>
<h4>In the disaster moment Team request to move server from UAE to another region, and remove from existing infra, Which is have NAT and IGW access, WOrdpress website link to already axisting infra for company platform another application. </h4>
<h1>
Quection:- My instance had only the Private IP dont have Public IP so how can I ssh to my server now?
</h1>
<h4>
Answer :- 
I have log in to the server which have public IP and and ssh to that server which in same VPC and ssh from that server to my wordpress server to get the backup using wordpres's server Private IP.

Issue resolved ....!
</h4>
Now what ? Now I have to get the backups fro wordpress , One is website and sql database .

For that I have used this command which is linux tar command
```bash
sudo tar -czvf wordpress-files.tar.gz /var/www/html

cat /var/www/html/wp-config.php
```
<h2>get the existing database configurations :- </h2>

```bash

define('DB_NAME', 'wordpress');
define('DB_USER', 'wpuser');
define('DB_PASSWORD', 'password');
```
<h4>SQL access </H4>

```bash
sudo mysql
mysqldump -u root payswi_site > payswi_site.sql
```
<h4>Remove unnecerry file </h4>

```bash
rm wordpress.sql
```
<h4>Before copying it, make sure it's a valid dump:</h4>

```bash
head -20 payswi_site.sql
```
copy them with:

```bash
cp /wordpress-files.tar.gz /home/ubuntu/
cp /payswi_site.sql /home/ubuntu/
```
Using Tabby download the files to my local file location , save as backups.

<H1>Next steps</H1>

<h3>1. Launch a new EC2 instance in the target region </h3>

Requir Info:

Ubuntu 24.04 LTS- t3.micro with deafoult vpc and defoult configurations.

2. Install the required software
```bash
sudo apt update
sudo apt install apache2 php php-mysql mariadb-server unzip -y
```

3. Upload the backups to file location in new server :- /home/ubuntu

4. Restore the WordPress files
```bash
sudo tar -xzvf wordpress-files.tar.gz -C /
```

5. Create the database
```bash
sudo mysql
```
Then:
```bash
CREATE DATABASE payswi_site;
CREATE USER 'payswi_site'@'localhost' IDENTIFIED BY 'YourPassword';
GRANT ALL PRIVILEGES ON payswi_site.* TO 'payswi_site'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```
Use the same username and password that are configured in the original wp-config.php, unless you plan to update that file.

6. Import the database
```bash
mysql -u payswi_site -p payswi_site < payswi_site.sql
```

7. Check wp-config.php

Verify these values match the new database:
```bash

DB_NAME
DB_USER
DB_PASSWORD
DB_HOST
```
8. Fix file permissions

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
