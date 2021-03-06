# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "centos/7"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.10.10"

  # Share an additional folder to the guest VM.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end

  # Enable provisioning with a shell script.
  config.vm.provision "bootstrap", type: "shell", inline: <<-SHELL
     yum install epel-release -y
     yum install -y https://rpms.remirepo.net/enterprise/remi-release-7.rpm
     yum update -y
     yum install httpd -y
     yum-config-manager --disable 'remi-php*'
     yum-config-manager --enable remi-php80
     yum -y install php php-{cli,fpm,mysqlnd,zip,devel,gd,mbstring,curl,xml,pear,bcmath,json}
     yum install mariadb-server -y
     systemctl enable --now php-fpm
     systemctl enable --now httpd
     systemctl enable --now mariadb
     systemctl enable --now firewalld
     firewall-cmd --remove-service=dhcpv6-client --permanent
     firewall-cmd --add-service={http,https,ssh} --permanent
     firewall-cmd --reload
     echo "<?php phpinfo(); ?>" > /var/www/html/index.php
   SHELL
end
