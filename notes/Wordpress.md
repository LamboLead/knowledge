---
id: hfpkgs57wxxazckgi298miw
title: Wordpress
desc: ''
updated: 1671647190053
created: 1668193801117
---

Hello there! This is a guide on how to set up a Wordpress page in a VPS. This installation guide will apply to Linux servers with Ubuntu v18+.

## Getting started

1. Update all the packages

	```sh
		apt update apt upgrade
	```
	
2. (optional) Install the LAMP stack

	For Wordpress to function, you need the LAMP stack (Linux, Apache, MySQL and PHP)

  ```sh
	 	apt install apache2 mariadb-server mariadb-client php
		php-mysql
	```

	Check its installation and startup by executing:

	```sh
	 	sytemctl status apache2
	```
	You can verify further by opening your browser and going into your server's IP address. A welcome page should appear.

3. (optional) Confirm PHP installation

   Confirm that PHP is installed by creating a `info.php` file at `/var/www/html/`, and insert the following lines:
   ```php
	 	<?php
			phpinfo();
		?>
	 ```
	Now, open your browser and append `info.php` to the server's url. The output should be a page with all the information about PHP.

4. Create your WordPress DB

	Log into your MariaDB database as root:

	```sh
		mysql -u root -p
	```

	Enter your password and create a DB for your WordPress installation

	```SQL
		> CREATE DATABASE your_wordpress_db;
	```

	Create a database user for your Wordpress setup

	```SQL
		> CREATE USER 'your_wp_user'@'localhost' IDENTIFIED BY 'your_password'
	```

	Grant privileges to your user

	```SQL
		> GRANT ALL ON your_wordpress_db.* TO 'your_wp_user'@'localhost' IDENTIFIED BY 'password';
	```

	You can now apply changes and exit MariaDB

	```SQL
		> FLUSH PRIVILEGES;
		> exit;
	```

5. Install WordPress CMS and change privileges

	Go to the `/var/www/` directory, download the latest WordPress file and uncompress it

	```sh
		cd /var/www/
		wget https://wordpress.org/latest.tar.gz
		tar -xvf latest.tar.gz
	```

	This will create a `/wordpress` directory. Rename this folder to your desired name

	```sh
		mv wordpress your_new_folder
	```

	Change permissions for this folder

	```sh
		chown -R root:root /var/www/html/your_new_folder
		chmod -R 755 /var/www/html/your_new_folder
	```

	Create the `/uploads` directory and set privileges

	```sh
		mkdir /var/www/html/your_new_folder/wp-content/uploads
		chown -R root:root /var/www/html/wordpress/wp-content/uploads
	```

6. First steps with WordPress

	