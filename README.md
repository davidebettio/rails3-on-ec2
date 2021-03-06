rails3-on-ec2
=============

Instructions for create a ubuntu ec2 instance for rails

ami for Ubuntu LTS 12.04
-------------
    ami-e1e8d395

system update
-------------
    sudo apt-get update
    sudo apt-get upgrade

configure timezone
-------------
    dpkg-reconfigure tzdata

rvm
-------------
    curl -L get.rvm.io | sudo bash -s stable
    usermod -a -G rvm ubuntu
    exit
    rvm install 1.9.3
    rvm use 1.9.3 --default
    exit

rubygems/rails/passenger
-------------
    gem update --system
    gem install rails
    gem install passenger

nginx
-------------
    sudo apt-get install libcurl4-openssl-dev
    rvmsudo passenger-install-nginx-module

nginx configuration
-------------
    /opt/nginx/conf/nginx.conf
    server {
        listen 80;
        server_name www.yourhost.com;
        root /somewhere/public;   # <--- be sure to point to 'public'!
        passenger_enabled on;
    }

guide to nginx
-------------
    located in /usr/local/rvm/gems/ruby-1.9.3-p194/gems/passenger-3.0.12/doc/Users guide Nginx.html

nginx init script
-------------
    git clone https://github.com/hulihanapplications/nginx-init-debian.git
    cd nginx-init-debian
    sudo cp etc/init/nginx.conf /etc/init
    sudo start nginx

mysql
-------------
    sudo apt-get install mysql-server libmysqlclient-dev

mysql configuration
-------------
    add to [client]
        default-character-set=utf8

    add to [mysqld]
        character-set-server=utf8
        collation-server=utf8_general_ci
        init-connect='SET NAMES utf8'
    
    to comment
        bind-address           = 127.0.0.1

database and users
-------------
    mysql -u root -p
    create database db1
    CREATE USER 'user1'@'localhost' IDENTIFIED BY '123456';
    GRANT ALL PRIVILEGES ON db1.* TO 'user1'@'localhost' WITH GRANT OPTION;

nodejs
-------------
    sudo apt-get install python-software-properties
    sudo add-apt-repository ppa:chris-lea/node.js
    sudo apt-get update
    sudo apt-get install nodejs npm

Wordpress automatic update
-------------
    change php-fpm user in /etc/php/fpm/pool.d/www.conf
        user = ubuntu
        group = ubuntu
        
    change listen to socket
        listen = /var/run/php-fpm.sock

crontab for Rails tasks
-------------
    /bin/bash -l -c "cd /home/ubuntu/www/site1 && rake cron"
