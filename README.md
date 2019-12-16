# WordPress-walkthrough
quick and easy guide to install wordpress 
1.sudo apt-get update && apt-get upgrade
2. sudo apt-get install lamp-server^
lamp install removes the step by step installation of apache, php,and mysql instead getting them all at once

^LAMP may install php 7.0 on ubuntu 16.04 if so do the following
	
	1.sudo apt-get update && apt-get upgrade
	2.sudo apt-get install software-properties-common
	3.sudo add-apt-repository ppa:ondrej/php
	4.sudo apt-get update
	5.sudo apt-get install php7.2
	6. sudo apt-get install php-pear php7.2-curl php7.2-dev php7.2-gd 	php7.2-mbstring php7.2-zip php7.2-mysql php7.2-xml
	7.sudo a2dismod php7.0
	8.sudo a2enmod php7.2
	9.sudo service apache2 restart
	

4. Test that php has installed succesfully by running the following command 

	sudo nano /var/www/html/info.php 

and writing: 

	<?php
​	phpinfo();
​	?>

then go to http://yourdomain/info.php
if you see a page telling you your php specs you're in the clear :)


SECTION 2 MYSQL 
1.enter in the following command:

	mysql -u root -p

in the previous install of lamp you should have been asked to make a password, if so input your password.

2. create a database, you do this by inputting the following command

	CREATE DATABASE wp_database; 

the lower case lettering is the name of the database which can be whatever you want, however picking something you'll remember is best.

3. Next is to grant control to you as the user. Note the '.*' is important and must be added on and the quotes in the command are necessary.

GRANT ALL PRIVILEGES ON yourdatabasename.* TO 'yourusername'@'localhost' IDENTIFIED BY 'a password of your choice'

4. This tells the database to actually execute the commands you've set
	
	FLUSH PRIVILEGES

now you can exit mysql by typing EXIT

SECTION 3 INSTALLING WORDPRESS
1. navigate to var/www/html if you do not know how the following command will take you there. 
	cd /var/www/html
2. use this command to get the zip from online
	
	wget -c http://wordpress.org/latest.tar.gz
	
3. Opn the file in the folder you were directed to.
	
	tar -xzvf latest.tar.gz
	
4. We need to give certain files permissions so that they can properly function, there are a few ways to do this
option 1. use this command
	
	sudo chown -R www-data:www-data /var/www/html/wordpress
	sudo chmod -R 755 /var/www/html/wordpress

option 2. If you have a file transfer app such as filezilla you can connect to your server find the file wp-config.php, rightclick it and change permissions to 644;

5. enter the wordpress directory (cd /var/www/html/wordpress)
6. move the sample config file to the main config file with 
	
	sudo mv wp-config-sample.php wp-config.php
	sudo nano wp-config.php
	
7. edit the file so your database information that was created earlier matches the queries in the file

8. go to this link https://api.wordpress.org/secret-key/1.1/salt/
to generate keys for the auth section, paste new keys into auth section "in same wp-config file, near bottom";

SECTION 4 APACHE ROUTING 

1. enter the sites available using
	
	cd /etc/apache2/sites-available/
	sudo nano wordpress.conf
	
2. paste the following into the file you just created
	
	<VirtualHost *:80>
     	ServerAdmin admin@example.com
     	DocumentRoot /var/www/html/wordpress/
     	ServerName example.com
     	ServerAlias www.example.com

    	<Directory /var/www/html/wordpress/>
        Options +FollowSymlinks
        AllowOverride All
        Require all granted
     	</Directory>

     	ErrorLog ${APACHE_LOG_DIR}/error.log
     	CustomLog ${APACHE_LOG_DIR}/access.log combined

	</VirtualHost>
3. enable your new website
	
	sudo a2ensite wordpress.conf
	sudo service apache2 reload

Navigate to your website at 'yourwebsite.com'
You should now have a wordpress install screen providing your database information is correct
