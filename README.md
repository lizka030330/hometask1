"#hometask1" 

Create a CentOS virtual machine with Nginx web server using Vagrant and verify that the site is available on port 8888.

Structure:

hometask1/

   Vagrantfile          
   www-content/         
   .gitignore          

Vagrantfile:

    Vagrant.configure("2") do |config|
    config.vm.box = "generic/centos9s"
    config.vm.box_version = "4.3.12"
    config.vm.network "forwarded_port", guest: 80, host: 8888, host_ip: "127.0.0.1" config.vm.synced_folder "./www-content",         "/var/www/html", type: "rsync", rsync__auto: true
    config.vm.provision "shell", inline: <<-SHELL
       yum clean all
       yum install -y epel-release
       yum install -y nginx rsync
       setenforce 0
       sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config
       rm -f /etc/nginx/conf.d/default.conf
       cat > /etc/nginx/conf.d/site.conf << 'EOF'

    server {
       listen 80 default_server;
       server_name _;
       root /var/www/html;
       index index.html;

    location / {
        try_files $uri $uri/ =404;
        }
    }
    EOF

    systemctl start nginx
    systemctl enable nginx
    systemctl disable firewalld
    systemctl stop firewalld
    SHELL
    end



How to Run:
1. Open the folder in Far Manager or VS Code terminal.
2. Start the virtual machine: vagrant up
3. Open in browser:http://localhost:8888
You should see your uploaded site from www-content.

Stop or Remove VM:
1. To stop: vagrant halt
2. To completely remove: vagrant destroy -f


Author: Yelizaveta Yefremenko

Course: DevOps 

Date: 09.09.2025
