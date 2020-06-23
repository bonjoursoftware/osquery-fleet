# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"

  config.vm.provision "shell", inline: <<-SHELL
    wget https://pkg.osquery.io/deb/osquery_4.3.0_1.linux.amd64.deb
    sudo dpkg -i osquery_4.3.0_1.linux.amd64.deb
    sudo systemctl enable osqueryd.service
    sudo cp /vagrant/cert/kolide.cert /etc/osquery/
    sudo cp /vagrant/enroll_secret /etc/osquery/
    sudo cp /vagrant/osquery.flags /etc/osquery/
    sudo service osqueryd start
  SHELL
end