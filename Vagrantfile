Vagrant.configure("2") do |config|

    # Configuración del Servidor Apache 1
    config.vm.define "apache1" do |apache1|
      apache1.vm.box = "ubuntu/bionic64"
      apache1.vm.network "private_network", ip: "192.168.56.11"
      apache1.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      apache1.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y apache2
        systemctl enable apache2
      SHELL
      apache1.vm.synced_folder "./src/server1", "/var/www/html"
    end
  
    # Configuración del Servidor Apache 2
    config.vm.define "apache2" do |apache2|
      apache2.vm.box = "ubuntu/bionic64"
      apache2.vm.network "private_network", ip: "192.168.56.12"
      apache2.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      apache2.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y apache2
        systemctl enable apache2
      SHELL
      apache2.vm.synced_folder "./src/server2", "/var/www/html"
    end
  
    # Configuración del Servidor Nginx (Load Balancer)
    config.vm.define "nginx" do |nginx|
      nginx.vm.box = "ubuntu/bionic64"
      nginx.vm.network "private_network", ip: "192.168.56.13"
      nginx.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      nginx.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y nginx
        cat <<EOT > /etc/nginx/conf.d/load_balancer.conf
        upstream apache_servers {
            server 192.168.56.11;
            server 192.168.56.12;
        }
        server {
            listen 80;
            location / {
                proxy_pass http://apache_servers;
            }
        }
        EOT
        systemctl restart nginx
      SHELL
    end
  
  end
  