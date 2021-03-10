# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<-SCRIPT
sudo apt-get -qq update

DB_NAME=aulavagrant
DB_USER=aulavagrant
DB_PASS=password

debconf-set-selections <<< "mysql-server mysql-server/root_password password $DB_PASS"
debconf-set-selections <<< "mysql-server mysql-server/root_password_again password $DB_PASS"

apt-get -y install mysql-server

mysql -uroot -p$DB_PASS -e "CREATE DATABASE $DB_NAME"
mysql -uroot -p$DB_PASS -e "grant all privileges on $DB_NAME.* to '$DB_USER'@'%' identified by '$DB_PASS'"

cd /vagrant

sudo sed -i "s/.*bind-address.*/bind-address = 0.0.0.0/" /etc/mysql/mysql.conf.d/mysqld.cnf
sudo service mysql restart

SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "base"
  config.vm.box = "ubuntu/bionic64"

  config.vm.define "mysqlserver" do |mysql|
    mysql.vm.network "private_network", ip: "10.10.10.4"
    
    mysql.vm.provision "shell", inline: $script    
  end    
end
