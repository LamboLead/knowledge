---
id: biisn57w2vhtc6ta3jvzo69
title: Failed FTP connection
desc: ''
updated: 1671666352078
created: 1671662451347
---

> Tags: FTP, Theme installation, Plugin installation, FTP credentials.

When you've just installed Wordpress and want to install some plugins and themes, you may encounter this message:

![Message when FTP isn't set up](/assets/images/2022-12-21-17-44-20.png)

This issue appears when WordPress doesn't have access to the FTP of your server, maybe due to not putting the right information (or not set it up at all) or a failed connection to it.

## Troubleshooting

### Insert your FTP credentials
Try to insert the FTP credentials. WordPress should connect to the FTP right away.

### Check and update the folder permissions
If you insert your FTP credentials and WordPress refuses to connect:
1. Go to _`Tools > Site Health`_ menu. 
2. Click on the _`Info`_ tab and open the _`Filesystem Permissions`_ ribbon. You should see the folder permissions.
3. Check that all the options in the table are **_writable_**. If they aren't, you need to update the folder permissions using your FTP client or PuTTy, or set the connection method as _direct_.

**Updating the folder permissions using your FTP client**

Access your server and go to the `/var/www/your_website_folder` directory. Right-click on the `wp-content` folder and change its permissions to _read_, _write_ and _execute for all the types of users (owner, group, others). Do it recursively.

![Desired folder permissions screenshot](/assets/images/2022-12-21-18-39-26.png)

This method is more reliable than doing it from the command line, but it may take much more time.

**Updating the folder permissions using PuTTy**

1. (optional) Add a new user to control the filesystem

	```sh
		adduser <username> your_username
	```

2. Set the folder permissions to the user set up in WordPress.

	```sh
		chown -R your_username:your_username /var/www/your_website_folder
		chmod -R g+rw /var/www/your_website folder
	```

**Setting the connection method to _direct_**

Inside the `wp-config.php` file (inside your website directory), copy and paste the following line.

```php
	define('FS_METHOD', 'direct');
```

This is a less-secure method because you're skipping the authentication procedure for WordPress to access your filesystem. Do it at your own risk.


	