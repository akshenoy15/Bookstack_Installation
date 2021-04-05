# Bookstack Installation Guide

Bookstack is an open-source application platform to store and organize information. It is readily available on Github and can be installed free of cost. The content can be in pages, chapters or books.

Bookstack provides multiple configuration options including setting up space with name, logo and registration of your choice. The privacy settings allow you to set the visibility of your system to public or private.

**Pre-Requisites:**

1. PHP 7.2.5 and above.

{% hint style="info" %}
Need to run from command-line for installation and maintenance.
{% endhint %}

2. MySQL 5.6 and above.     

{% hint style="info" %}
For storing information from Bookstack in a proper database
{% endhint %}

3. [Composer](https://getcomposer.org/)

4. Git version control \(optional\).

{% hint style="info" %}
Install Ubuntu 18.04 LTS in your system from [Ubuntu for desktop or laptop](https://ubuntu.com/download/desktop).
{% endhint %}

**Installation:**

Below is the script to install Bookstack on Ubuntu 18.04 LTS

1. Checking the release, hostname and fetching essential properties and release requisites.

```c
lsb_release -cd
hostname
hostname -I
whoami
getconf LONG_BIT
apt install -y build-essential software-properties-common curl gdebi net-tools wget curl sqlite3 dirmngr nano lsb-release
apt-transport-https
```

2.  Including Apache2, PHP, MariaDB modules:

```c
add-apt-repository ppa:ondrej/php -y
apt-get update ; apt install -y apache2 mariadb-server mariadb-client php7.0 php7.0-cli php7.0-fpm php7.0-cgi php7.0-bcmath php7.0-curl php7.0-gd php7.0-intl php7.0-json php7.0-mbstring php7.0-mysql php7.0-opcache php7.0-sqlite3 php7.0-xml php7.0-zip php7.0-snmp php7.0-json php7.0-imap php7.0-common php7.0-tidy php7.0-mcrypt php7.0-pgsql php-pear
a2enmod dir env headers mime rewrite setenvif
sed -i "s/;date.timezone.*/date.timezone = UTC/" /etc/php/7.0/cli/php.ini 
echo ServerName 127.0.0.1 >> /etc/apache2/apache2.conf
systemctl start apache2 mariadb 
systemctl enable apache2 mariadb
```

3.  Securely installing MySQL:

```c
mysql_secure_installation
```

4. Creating a MariaDB database:

   After installing Mysql you will find a SQL prompt. Execute the following

```c
mysql -u root -p
create database db;
grant all on db.* to 'dbuser'@'localhost' identified by 'dbpass’;
quit
curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer
cd /var/www/html/ ; git clone https://github.com/BookStackApp/BookStack.git --branch release --single-branch
cd BookStack/ ; composer install
cp .env.example .env
nano .env
php artisan key:generate
php artisan migrate
chown -R www-data:www-data /var/www/html/ ;
chmod -R 755 /var/www/html/
```

5. Next set up an Apache virtual host.

```c
gedit /etc/apache2/sites-available/new-domain.conf
```

{% hint style="info" %}
_If gedit is not supported install it using_ _**apt gedit install**_ _on your screen. Include this script in the.conf file._
{% endhint %}

```c
<VirtualHost *:80>
 ServerAdmin admin@new-domain.com
 DocumentRoot /var/www/html/BookStack/public
 ServerName www.new-domain.com
 <Directory /var/www/html/BookStack/public/>
 Options FollowSymlinks
 AllowOverride All
 Require all granted
 </Directory>
 ErrorLog ${APACHE_LOG_DIR}/new-domain_error.log
 CustomLog ${APACHE_LOG_DIR}/new-domain_access.log combined
</VirtualHost>
```

 6. After editing, run the following commands

```c
a2ensite new-domain
a2dissite 000-default.conf
apache2ctl configtest
echo "192.168.1.50 www.new-domain.com" >> /etc/hosts
systemctl reload apache2
```

 7.  After reloading apache2 server login with the provided credentials in the new domain.

[http://www.new-domain.com](http://www.new-domain.com/)

_Username_: [admin@admin.com](mailto:admin@admin.com)

_Password_: password

![](.gitbook/assets/0.png)

To test the UI platform and gain first hand experience try the [demo site](https://demo.bookstackapp.com/) provided by Bookstack. You can also check their features, overviews and settings [here](https://www.bookstackapp.com/).

