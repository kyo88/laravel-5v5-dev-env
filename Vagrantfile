Vagrant.configure("2") do |config|

  config.vm.define "web" do |web|
    web.vm.box = "centos/7"
    web.vm.network "private_network", ip: "192.168.33.88"
    web.vm.synced_folder "./playbooks", "/home/vagrant/playbooks", owner: "vagrant", group: "vagrant", :create => true, :mount_options => ["fmode=600"]
    web.vm.synced_folder "./app", "/var/www/web", type: "nfs"

    web.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 1024]
      v.customize ["modifyvm", :id, "--name", "web"]
    end
    web.vm.provision "shell", inline: <<-SHELL
        sudo yum update -y
        sudo yum install epel-release -y
        sudo yum install wget -y
        sudo yum install ansible -y
    SHELL

    web.vm.provision "file", source: "./playbooks/keys/id_vagrant.pub", destination: "/home/vagrant/.ssh/id_vagrant.pub"
    web.vm.provision "shell", inline: "> /home/vagrant/.ssh/known_hosts && chmod 664 /home/vagrant/.ssh/known_hosts && chown -R vagrant.vagrant /home/vagrant/.ssh/known_hosts"
    web.vm.provision "shell", inline: "cat /home/vagrant/.ssh/id_vagrant.pub >> /home/vagrant/.ssh/authorized_keys && chmod 600 /home/vagrant/.ssh/id_vagrant.pub"
  end
end
