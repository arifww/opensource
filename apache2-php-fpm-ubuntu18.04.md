Install Apache2, PHP7-FPM, MYSQL On Ubuntu 18.04
==============

##Upgrade OS Library and packages
* sudo apt update
* sudo apt upgrade

##Add PPA for PHP 7.3
* sudo apt install software-properties-common
* sudo add-apt-repository ppa:ondrej/php
* sudo apt update

##Install PHP 7.3 FPM
* sudo apt install php7.3-fpm php7.3-common php7.3-mysql php7.3-xml php7.3-xmlrpc php7.3-curl php7.3-gd php7.3-imagick php7.3-cli php7.3-dev php7.3-imap php7.3-mbstring php7.3-soap php7.3-zip php7.3-bcmath -y
* sudo service php7.3-fpm status

##Install Apache
* sudo apt install apache2
* sudo a2dissite 000-default
* sudo a2enmod proxy_fcgi
* vi /etc/apache2/sites-available/site-01.conf

<VirtualHost *:80>
     ServerName External_IP_Address
     DocumentRoot /var/www/html

     <Directory /var/www/html>
          Options Indexes FollowSymLinks
         AllowOverride All
         Require all granted
     </Directory>

     <FilesMatch ".php$"> 
         SetHandler "proxy:unix:/var/run/php/php7.3-fpm.sock|fcgi://localhost/"          
      </FilesMatch>
 
      ErrorLog ${APACHE_LOG_DIR}/error.log
      CustomLog ${APACHE_LOG_DIR}/access.log combined  
</VirtualHost>

* sudo a2ensite site-01.conf
* sudo service apache2 restart

