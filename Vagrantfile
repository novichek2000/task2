Vagrant.configure("2") do |config|
    config.vm.box = "bento/centos-7.5"
    config.vm.provider "virtualbox" do |vb|
        vb.gui = true
        vb.memory = "1024"
    end
    config.vm.define "apache" do |apache|
        apache.vm.network "private_network", ip: "192.168.33.10"
        apache.vm.hostname = "apache"
        apache.vm.network "forwarded_port", guest: 80, host: 18000
        apache.vm.provision "shell", inline: <<-SHELL
            yum install httpd -y
            yum install mc -y
            cp /vagrant/mod_jk.so /etc/httpd/modules/
            touch /etc/httpd/conf/workers.properties
            echo "worker.list=lb" >> /etc/httpd/conf/workers.properties
            echo "worker.lb.type=lb" >> /etc/httpd/conf/workers.properties
            echo "worker.lb.balance_workers=tomcat1, tomcat2" >> /etc/httpd/conf/workers.properties
            echo "worker.tomcat1.host=tomcat1" >> /etc/httpd/conf/workers.properties
            echo "worker.tomcat1.port=8009" >> /etc/httpd/conf/workers.properties
            echo "worker.tomcat1.type=ajp13" >> /etc/httpd/conf/workers.properties 
            echo "worker.tomcat2.host=tomcat2" >> /etc/httpd/conf/workers.properties
            echo "worker.tomcat2.port=8009" >> /etc/httpd/conf/workers.properties
            echo "worker.tomcat2.type=ajp13" >> /etc/httpd/conf/workers.properties 
            echo "LoadModule jk_module modules/mod_jk.so" >> /etc/httpd/conf/httpd.conf 
            echo "JkWorkersFile conf/workers.properties" >> /etc/httpd/conf/httpd.conf
            echo "JkShmFile /tmp/shm" >> /etc/httpd/conf/httpd.conf
            echo "JkLogFile logs/mod_jk.log" >> /etc/httpd/conf/httpd.conf
            echo "JkLogLevel info" >> /etc/httpd/conf/httpd.conf
            echo "JkMount /test* lb" >> /etc/httpd/conf/httpd.conf
            echo "192.168.33.11    tomcat1" >> /etc/hosts
            echo "192.168.33.12    tomcat2" >> /etc/hosts
            systemctl enable httpd
            systemctl start httpd
        SHELL
    end
    config.vm.define "tomcat1" do |tomcat1|
        tomcat1.vm.box = "bento/centos-7.5"
        tomcat1.vm.hostname = "tomcat1"
        tomcat1.vm.network "forwarded_port", guest: 8080, host: 18001
        tomcat1.vm.network "private_network", ip: "192.168.33.11"
        tomcat1.vm.provision "shell", inline: <<-SHELL
            yum -y install java-1.8.0-openjdk
            yum -y install tomcat tomcat-webapps tomcat-admin-webapps
            yum install mc -y
            mkdir /usr/share/tomcat/webapps/test
            echo "11111111111111111111111111111111" > /usr/share/tomcat/webapps/test/index.html
            systemctl enable tomcat
            systemctl start tomcat
            echo "192.168.33.10    apache" >> /etc/hosts
            echo "192.168.33.12    tomcat2" >> /etc/hosts
        SHELL
    end
    config.vm.define "tomcat2" do |tomcat2|
        tomcat2.vm.box = "bento/centos-7.5"
        tomcat2.vm.hostname = "tomcat2"
        tomcat2.vm.network "forwarded_port", guest: 8080, host: 18002
        tomcat2.vm.network "private_network", ip: "192.168.33.12"
        tomcat2.vm.provision "shell", inline: <<-SHELL
            yum -y install java-1.8.0-openjdk
            yum -y install tomcat tomcat-webapps tomcat-admin-webapps
            yum install mc -y
            mkdir /usr/share/tomcat/webapps/test
            echo "22222222222222222222222222222222" > /usr/share/tomcat/webapps/test/index.html
            systemctl enable tomcat
            systemctl start tomcat
            echo "192.168.33.10    apache" >> /etc/hosts
            echo "192.168.33.11    tomcat1" >> /etc/hosts
        SHELL
    end

end   