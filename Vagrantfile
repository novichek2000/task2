Vagrant.configure("2") do |config|
    config.vm.define "server1" do |server1|
        server1.vm.box = "bento/centos-7.5"
        server1.vm.hostname = "server1"
        server1.vm.network "forwarded_port", guest: 80, host: 8080
        server1.vm.network "private_network", ip: "192.168.33.10"
        server1.vm.network "public_network"
        server1.vm.provision "shell", inline: <<-SHELL
            yum install git -y
            yum install mc -y
            mkdir task2
            cd task2
            git clone https://github.com/novichek2000/task2.git -b task2
            less task2/novichek.txt
        SHELL
    end
    config.vm.define "server2" do |server2|
        server2.vm.box = "bento/centos-7.5"
        server2.vm.hostname = "server2"
        server2.vm.network "forwarded_port", guest: 80, host: 8081
        server2.vm.network "private_network", ip: "192.168.33.11"
    end
    config.vm.provider "virtualbox" do |vb|
        vb.gui = true
        vb.memory = "1024"
    config.vm.provision "shell", inline: "echo 192.168.33.10  server1.local >> /etc/hosts"
    config.vm.provision "shell", inline: "echo 192.168.33.11  server2.local >> /etc/hosts"
    end 
end   