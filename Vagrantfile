# -*- mode: ruby -*-
# vi: set ft=ruby :

NODE_SCRIPT = <<EOF.freeze
  echo "Preparing node..."
  # ensure the time is up to date
  yum -y install ntp
  systemctl start ntpd
  systemctl enable ntpd
  ln -sf /usr/share/zoneinfo/Asia/Colombo /etc/localtime

  sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2

  sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

  sudo yum-config-manager --enable docker-ce-edge
  sudo yum-config-manager --enable docker-ce-test
  sudo yum install -y docker-ce
  sudo systemctl start docker
  sudo yum-config-manager --disable docker-ce-edge
  sudo gpasswd -a vagrant docker
  sudo setenforce 0
EOF

Vagrant.configure(2) do |config|

  #base vmbox
  config.vm.box = 'centos/7'
  config.vbguest.auto_update = false
  config.vbguest.no_remote = true



  config.vm.define 'node1' do |n|
    n.vm.hostname = 'node1'
    n.vm.network :private_network, ip: '192.168.10.43'
    n.vm.provision :shell, inline: NODE_SCRIPT.dup
    n.vm.network :forwarded_port, guest: 8080, host: 8081
  end

  config.vm.define 'node2' do |n|
    n.vm.hostname = 'node2'
    n.vm.network :private_network, ip: '192.168.10.44'
    n.vm.provision :shell, inline: NODE_SCRIPT.dup
    n.vm.network :forwarded_port, guest: 8081, host: 8082
  end

   config.vm.define 'node3' do |n|
    n.vm.hostname = 'node3'
    n.vm.network :private_network, ip: '192.168.10.45'
    n.vm.provision :shell, inline: NODE_SCRIPT.dup
    n.vm.network :forwarded_port, guest: 8081, host: 8083
  end




end
