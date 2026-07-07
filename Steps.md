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
