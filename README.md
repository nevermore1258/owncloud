# คำสั่งติดตั้งสคริปท์ รันตามลำดับ

# go to root

sudo su

#Update

sudo apt update

# Install Php and php modules

apt install -y php php-common libapache2-mod-php php-mbstring php-xmlrpc php-soap php-apcu php-smbclient php-ldap php-redis php-gd php-xml php-intl php-json php-imagick php-mysqlnd php-cli php-mcrypt php-ldap php-zip php-curl unzip


# Install apache webserver

sudo apt install apache2 -y


# disable directory listing 

sed -i 's/Options Indexes FollowSymLinks/Options FollowSymLinks/g' /etc/apache2/apache2.conf


#stop/start/enable webserver

systemctl stop apache2.service

systemctl start apache2.service

systemctl enable apache2.service


# Download and setup Owncloud

cd /var/www/html

wget https://raw.githubusercontent.com/nevermore1258/owncloud/master/owncloud.zip

unzip owncloud.zip

chmod -R 755 /var/www/html/owncloud/
chown -R www-data:www-data /var/www/html/owncloud/

cd /etc/apache2/sites-available

wget https://raw.githubusercontent.com/nevermore1258/owncloud/master/owncloud.conf


# Install maria db

apt-get install -y mariadb-server mariadb-client


# stop/start/enable maria db

systemctl stop mariadb.service

systemctl start mariadb.service

systemctl enable mariadb.service



# Secure maria db

mysql_secure_installation


# Restart maria db service 

systemctl restart mariadb.service


# Enable apache modules

a2ensite owncloud.conf

systemctl reload apache2

a2enmod rewrite

systemctl restart apache2

a2enmod env

a2enmod dir

a2enmod mime


# Restart apache

systemctl restart apache2.service

# configure database

mysql -uroot -p

---------------------------------------------------------------------------------

CREATE DATABASE IF NOT EXISTS owncloud;

---------------------------------------------------------------------------------

GRANT ALL PRIVILEGES ON owncloud.* TO 'user'@'localhost' IDENTIFIED BY 'password';

---------------------------------------------------------------------------------

exit;

