# Laravel 7 LAMP
Laravel Installation on Ubuntu18.04 LAMP
__This is my personal setup guide for installing Laravel 7__, I set it public so anyone ca use
## Installing Apache

1. Install Apache via apt
```
sudo apt install apache2
```

2. Open Port for "Apache"
If you don`t have ufw, install using this code
```
sudo apt install ufw
```
Then run this code
```
sudo ufw allow "Apache Full"
```

## Install Mysql Server
1. Install via apt
```
sudo apt install mysql-server
```
2. Modify root to have password
```
sudo mysql
```
then run this query to change the password.
change **yourpassword** to you desired password
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'yourpassword';
```
next
```
FLUSH PRIVIEGES;
```
then exit
```
exit;
```

## Install PHP
1. Run this command
```
sudo apt install php7.2 libapache2-mod-php7.2 php7.2-mbstring php7.2-xmlrpc php7.2-soap php7.2-gd php7.2-xml php7.2-cli php7.2-zip php-mysql php-curl
```
2. Make apache look first on .PHP
```
sudo nano /etc/apache2/mods-enabled/dir.conf
```
you will see this 
```
<IfModule mod_dir.c>
    DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
</IfModule>
```
now change it from this, Save
```
<IfModule mod_dir.c>
    DirectoryIndex index.php index.cgi index.pl index.html index.xhtml index.htm
</IfModule>
```
restart apache2
```
sudo systemctl restart apache2
```
## Install Composer
Run this Command
```
curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer
```
## Download Laravel
Move to the directory where you want to save your Project, In this case I use **/var/www/myproj/**
First run this command
```
cd /var/www/
```
then this
```
sudo composer create-project laravel/laravel myproj --prefer-dist
```
change permission
```
sudo chown -R www-data:www-data /var/www/myproj/
sudo chmod -R 755 /var/www/myproj/
```
## Configure Apache to Use Laravel
Run this command
you can change **myproj.conf** to your desired filename
```
sudo nano /etc/apache2/sites-available/myproj.conf
```
paste this code, then save
```
<VirtualHost *:80>   
  ServerAdmin admin@example.com
     DocumentRoot /var/www/myproj/public
     ServerName example.com

     <Directory /var/www/myproj/public>
        Options +FollowSymlinks
        AllowOverride All
        Require all granted
     </Directory>

     ErrorLog ${APACHE_LOG_DIR}/error.log
     CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
then Enable Laravel and disable default
```
sudo a2dissite 000-default.conf
sudo a2ensite myproj.conf
sudo a2enmod rewrite
sudo systemctl restart apache2
```
