# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  ext_ip = "192.168.33.10"

  config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.network "forwarded_port", guest: 81, host: 8081

  #config.vm.network "private_network", ip: ext_ip

  config.vm.provider "virtualbox" do |vb|
    #vb.name = "staticsite.vagrant.vm"
    # Customize the amount of memory on the VM:
    vb.memory = "512"
  end
  
  config.vm.provision "shell", inline: <<-SHELL
    # Install requirements
    #apt-get install -y apache2 php 
     sudo yum install -y httpd
     sudo yum install -y php php-common php-gd  
 	
    # Copy static site
    chmod -v 775 /var/www/html
    chown -vR root:vagrant /var/www/html

    cp -v /vagrant/index.html /var/www/html/

    # Copy dynamic site
    mkdir -pv /var/www/php
    chmod -v 775 /var/www/php
    chown -v root:vagrant /var/www/php

    cp /vagrant/index.php /var/www/php/

   # Create VirtualHost config
    cp /vagrant/001-demosite.conf /etc/httpd/conf.d/
    sed -i -e 's/IP_ADDRESS/#{ext_ip}/g' /etc/httpd/conf.d/001-demosite.conf

    ln -svf /etc/httpd/conf.d/001-demosite.conf /etc/httpd/conf.d/001-demosite.conf

    echo "Listen 81" >>/etc/httpd/conf.d/ports.conf

    # Restart apache service
    service httpd restart

	
  SHELL
end
