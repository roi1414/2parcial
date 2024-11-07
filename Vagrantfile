Vagrant.configure("2") do |config|
  # Configuración para el servidor web1
  config.vm.define "web1" do |web1|
    web1.vm.box = "ubuntu/bionic64"
    web1.vm.hostname = "web1"
    web1.vm.network "private_network", ip: "192.168.33.10"
    web1.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    # Instalar Apache y configurar directorio compartido
    web1.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y apache2
      echo "<h1>Servidor web1</h1>" > /var/www/html/index.html
    SHELL
    # Crear directorio compartido con la máquina host
    web1.vm.synced_folder ".", "/var/www/html"
  end

  # Configuración para el servidor web2
  config.vm.define "web2" do |web2|
    web2.vm.box = "ubuntu/bionic64"
    web2.vm.hostname = "web2"
    web2.vm.network "private_network", ip: "192.168.33.11"
    web2.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    # Instalar Apache y configurar directorio compartido
    web2.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y apache2
      echo "<h1>Servidor web2</h1>" > /var/www/html/index.html
    SHELL
    # Crear directorio compartido con la máquina host
    web2.vm.synced_folder ".", "/var/www/html"
  end

  # Configuración para el servidor lb (load balancer)
  config.vm.define "lb" do |lb|
    lb.vm.box = "ubuntu/bionic64"
    lb.vm.hostname = "lb"
    lb.vm.network "private_network", ip: "192.168.33.12"
    lb.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    # Instalar Nginx y configurarlo para hacer balanceo de carga
    lb.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y nginx
      cat <<EOL > /etc/nginx/conf.d/load_balancer.conf
      upstream backend {
          server 192.168.33.10;
          server 192.168.33.11;
      }

      server {
          listen 80;

          location / {
              proxy_pass http://backend;
          }
      }
      EOL
      systemctl restart nginx
    SHELL
  end
end

