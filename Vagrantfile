Vagrant.configure("2") do |config|

  config.vm.define "php" do |php|
    php.vm.box = "centos/7"
    php.vm.network "private_network", ip: "192.168.33.88"
    php.vm.synced_folder "./playbooks", "/home/vagrant/playbooks", owner: "vagrant", group: "vagrant", :create => true, :mount_options => ["fmode=600"]
    php.vm.synced_folder "./app", "/var/www/web", type: "nfs"

    php.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 1024]
      v.customize ["modifyvm", :id, "--name", "php"]
    end
    php.vm.provision "shell", inline: <<-SHELL
        sudo yum update -y
        sudo yum install epel-release -y
        sudo yum install wget -y
        sudo yum install ansible -y
    SHELL

    php.vm.provision "file", source: "./keys/id_vagrant.pub", destination: "/home/vagrant/.ssh/id_vagrant.pub"
    php.vm.provision "shell", inline: "> /home/vagrant/.ssh/known_hosts && chmod 664 /home/vagrant/.ssh/known_hosts && chown -R vagrant.vagrant /home/vagrant/.ssh/known_hosts"
    php.vm.provision "shell", inline: "cat /home/vagrant/.ssh/id_vagrant.pub >> /home/vagrant/.ssh/authorized_keys && chmod 600 /home/vagrant/.ssh/id_vagrant.pub"
  end
end
