<div align='center'>

# ** WordPress Configuration for Ubuntu **

</div>

## Install and Configure Apache

### Update System Packages

Before installing packages, ensure that your packages are up to date.

```Bash
sudo apt update && upgrade -y
```

### Install Apache2

In this tutorial I use Apache2 as a Web Server. The command line for Ubuntu Distro to install apache2:

```bash
sudo apt install apache2 -y
```

The next step is to check in a browser using the local domain or local IP. We’ll discuss creating IP addresses in more detail later.

```bash
http://localhost
http://192.168.58.41
```

![sorry](/ubuntu_wordpress_configuration/img/defaultApache.png)

### Creating Your Own Website

The first thing to create your own website is by creating a directory in `/var/www/`.

```bash
sudo mkdir /var/www/wilma123.local
```

I have it named wilm123.local but any name will work.

Now we have directory to create for our site. Lets have an HTML file in it.

```bash
cd /var/www/wilma123.local/
sudo nano index.html
```

Paste the following code in the index.html

```bash
<html>
<head>
  <title> Website Ubuntu </title>
</head>
<body>
  <p> Welcome to my website!
</body>
</html>
```

![sorry](/ubuntu_wordpress_configuration/img/website.png)

### Setting up the VirtualHost Configuration File

We start from change directory first

```bash
cd /etc/apache2/sites-available
```

Since Apache came with a default file, let's use our file.

```bash
sudo cp 000-default.conf websiteku.conf
```

You could use another name though.

Open the file with nano command

```bash
sudo nano websiteku.conf
```

Fill it in just like in the photo:
![sorry](/ubuntu_wordpress_configuration/img/websitekuConf.png)

Change the ServerAdmin to your Gmail and your ServerName.

> [!CAUTION]
> Don't forget to use your own directory name!

### Activating VirtualHost file

```bash
sudo a2ensite wilma123.local.conf
```

![sorry](/ubuntu_wordpress_configuration/img/a2ensite.png)

## DNS Configuration

Install BIND9 and its utilities

```bash
sudo apt install bind9 bind9utils -y
```

The next step is change directory to `/etc/bind`

```bash
cd /etc/bind
ls
```

Copy file `db.local and db.127`

```bash
cp db.local db.websiteku
cp db.127 db.192
```

![sorry](/ubuntu_wordpress_configuration/img/listBind.png)

- Config `db.websiteku` like the photo below:
  ![sorry](/ubuntu_wordpress_configuration/img/dbWebsiteku.png)

> [!NOTE]
> Configure the domain so that it points to the IP address which will be translated to your domain. For example: my domain is wilma123.local with my ip address `192.168.58.41`

- Config db.192 like in this photo:
  ![sorry](/ubuntu_wordpress_configuration/img/db192.png)

> [!NOTE]The number of `41` represents the last octet of my IP address.
> `db.192` is reverse lookup zone file. Reverse DNS is used to map an IP address to a hostname.

- Edit file `named.conf.options` to create forwarders. When we make a request to another domain that we don't handle, our DNS server will forward it to the forwarders we've set up.
  ![sorry](/ubuntu_wordpress_configuration/img/namedConfOps.png)

- Restart bind9 and check with `nslookup`

```bash
sudo systemctl restart bind9
nslookup 192.168.58.41
```

### Configure the IP local

```bash
sudo nano /etc/netplan/01-netcfg.yaml
```

![sorry](/ubuntu_wordpress_configuration/img/yaml.png)
The last two oktet `58.41`is up to you. You can change it.

## MySQL Configuration

### Install MySQL

Install MySQL for the WordPress database.

```bash
sudo apt install mysql-server -y
```

### Securing Database

After installation, secure your database server:

```bash
sudo mysql_secure-installation
```

Then check the status with:

```bash
sudo systemstl status mysql
```

If `Active (running)` your database is ready to set.

![sorry](/ubuntu_wordpress_configuration/img/statusMysql.png)

### Configure Database

We need create MySQL database:

```bash
sudo mysql -u root
```

And then, create a new database

```bash
CREATE DATABASE wordpress_db;
```

Next, create a new user that will have all access to that database

```bash
CREATE USER 'wordpress_user'@'localhost' IDENTIFIED BY '12345';

```

Last thing is grant all permissions to the user for the database

```bash
GRANT ALL PRIVILEGES ON wordpress_db.* TO 'wordpress_user'@'localhost';
FLUSH PRIVILEGES;
EXIT
```

## Install PHP and Required Extensions

Php is essential for running WordpRess. Install PHP and the commonly used extensions.

```bash
sudo apt install php php-mysql php-xml php-gd php-curl php-mbstring php-zip php-intl libapache2-mod-php -y
```

### Restart Apache

```bash
sudo systemctl restart bind9
```

After installing PHP, restart Apache to apply the changes.

```bash
sudo systemctl restart apache2
```

## Install and Configure WordPress

### Install WordPress

Create the installation directory and download the file from WordPress.org

```bash
sudo mkdir -p /var/www/wilma123.local
sudo chown www-data: /srv/www/wilm123.local
curl https://wordpress.org/latest.tar.gz | sudo -u www-data tar zx -C /srv/www/wilma123.local
```

### Configure Apache for WordPress

Create Apache site for WordPress. Create file in directory:

```bash
cd /etc/apache2/sites-available
sudo nano wilma123.local.conf
```

Create conf with following lines:

![sorry](/ubuntu_wordpress_configuration/img/configVirtual.png)

Enable the site:

```bash
sudo a2ensite wilma123.local.conf
```

Enable URL rewriting:

```bash
sudo a2enmod rewrite
```

Disable the default 'It Works'

```bash
sudo a2dissite 000-default
```

Finally, reload apache2 to apply all the changes

```bash
sudo service apache2 reload
```

### THE LAST STEP

Check with the domain that you have created.

![sorry](/ubuntu_wordpress_configuration//img/wordpress.png)

YOUR WORDPRESS IS READY TO USE!!!
