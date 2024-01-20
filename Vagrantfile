# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.provision "file", source: "files/vagrant_test.pub", destination: "/home/vagrant/.ssh/"

  config.vm.define "controlnode" do |controlnode|
    controlnode.vm.box = "ubuntu/focal64"
    controlnode.vm.hostname = "controlnode"
    controlnode.vm.network "private_network", ip: "192.168.55.4"
    controlnode.vm.synced_folder "./ansible","/home/vagrant/ansible"
    controlnode.vm.synced_folder "./files","/home/vagrant/files/"
    controlnode.vm.provision "file", source: "files/vagrant_test", destination: "/home/vagrant/.ssh/"
    controlnode.vm.provision "shell", inline: <<-SHELL
      sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/#g' /etc/ssh/sshd_config
      service ssh restart
      sudo add-apt-repository ppa:ansible/ansible
      sudo apt update -y && sudo apt -y install sshpass ansible
      ansible-galaxy collection install -r ansible/requirements.yml
      chmod 600 /home/vagrant/.ssh/vagrant_test
      chmod 644 /home/vagrant/.ssh/vagrant_test.pub
      cat /home/vagrant/.ssh/vagrant_test.pub >> /home/vagrant/.ssh/authorized_keys
    SHELL
  end

  config.vm.define "postgres" do |postgres|
    postgres.vm.box = "bento/centos-7.9"
    postgres.vm.hostname = "postgres-server"
#    postgres.vbguest.auto_update = false
    postgres.vm.network "private_network", ip: "192.168.56.41"
    postgres.vm.provision "shell", inline: <<-SHELL
      chmod 644 /home/vagrant/.ssh/vagrant_test.pub
      cat /home/vagrant/.ssh/vagrant_test.pub >> /home/vagrant/.ssh/authorized_keys
    SHELL
  end

  config.vm.define "app_server" do |app_server|
    app_server.vm.box = "bento/centos-7.9"
    app_server.vm.hostname = "app-server"
#    app_server.vbguest.auto_update = false
    app_server.vm.network "private_network", ip: "192.168.56.40"
    app_server.vm.provision "shell", inline: <<-SHELL
      chmod 644 /home/vagrant/.ssh/vagrant_test.pub
      cat /home/vagrant/.ssh/vagrant_test.pub >> /home/vagrant/.ssh/authorized_keys
    SHELL
  end

end
