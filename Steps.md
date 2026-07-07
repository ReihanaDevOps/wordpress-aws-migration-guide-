Request  :- 
In the disaster moment Team request to move server from UAE to another region, and remove from existing infra, Which is have NAT and IGW access, WOrdpress website link to already axisting infra for company platform another application. 

Quection:- My instance had only the Private IP dont have Public IP so how can I ssh to my server now?

Answer :- 
I have log in to the server which have public IP and and ssh to that server which in same VPC and ssh from that server to my wordpress server to get the backup using wordpres's server Private IP.

Issue resolved ....!

Now what ? Now I have to get the backups fro wordpress , One is website and sql database .

For that I have used this command which is linux tar command

sudo tar -czvf wordpress-files.tar.gz /var/www/html

cat /var/www/html/wp-config.php

get the existing database configurations :- 

define('DB_NAME', 'wordpress');
define('DB_USER', 'wpuser');
define('DB_PASSWORD', 'password');


sudo mysql
mysqldump -u root payswi_site > payswi_site.sql

Remove unnecerry file 
rm wordpress.sql

Before copying it, make sure it's a valid dump:
head -20 payswi_site.sql
