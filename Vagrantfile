Vagrant.configure("2") do |config|
  # Configuración del primer servidor Apache
  config.vm.define "apache1" do |apache1|
    apache1.vm.box = "ubuntu/bionic64"
    apache1.vm.hostname = "apache1"
    apache1.vm.network "private_network", ip: "192.168.33.10"
    apache1.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    apache1.vm.provision "shell", inline: <<-SHELL
      sudo apt update
      sudo apt install -y apache2
      echo "<h1>Servidor Apache 1</h1>" | sudo tee /var/www/html/index.html
      sudo systemctl enable apache2
      sudo systemctl start apache2
    SHELL
    apache1.vm.synced_folder ".", "/vagrant"
  end

  # Configuración del segundo servidor Apache
  config.vm.define "apache2" do |apache2|
    apache2.vm.box = "ubuntu/bionic64"
    apache2.vm.hostname = "apache2"
    apache2.vm.network "private_network", ip: "192.168.33.11"
    apache2.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    apache2.vm.provision "shell", inline: <<-SHELL
      sudo apt update
      sudo apt install -y apache2
      echo "<h1>Servidor Apache 2</h1>" | sudo tee /var/www/html/index.html
      sudo systemctl enable apache2
      sudo systemctl start apache2
    SHELL
    apache2.vm.synced_folder ".", "/vagrant"
  end

  # Configuración del servidor Nginx para balanceo de carga
  config.vm.define "nginx" do |nginx|
    nginx.vm.box = "ubuntu/bionic64"
    nginx.vm.hostname = "nginx"
    nginx.vm.network "private_network", ip: "192.168.33.12"
    nginx.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    nginx.vm.provision "shell", inline: <<-SHELL
      sudo apt update
      sudo apt install -y nginx
      echo "upstream backend {
          server 192.168.33.10;
          server 192.168.33.11;
      }
      server {
          listen 80;
          location / {
              proxy_pass http://backend;
          }
      }" | sudo tee /etc/nginx/sites-available/default
      sudo nginx -t
      sudo systemctl restart nginx
    SHELL
  end
end
