VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  nodes = {
    "loadbalancer" => "192.168.70.100",
    "web1" => "192.168.70.101",
    "web2" => "192.168.70.102",
    "web3" => "192.168.70.103",
    "web4" => "192.168.70.104"
  }

  config.vm.box = "ubuntu/jammy64"

  nodes.each do |name, ip|
    config.vm.define name do |node|
      node.vm.hostname = name
      node.vm.network "private_network", ip: ip

      node.vm.provider "virtualbox" do |vb|
        vb.memory = 2048
        vb.cpus = 2
      end

      node.vm.provision "shell", inline: <<-SHELL
        apt update -y
        apt install -y nginx curl
      SHELL

      if name.start_with?("web")
        node.vm.synced_folder "./frontend/build", "/var/www/react-app", type: "virtualbox"
        node.vm.provision "shell", inline: <<-SHELL
          rm -rf /var/www/html
          ln -s /var/www/react-app /var/www/html
        SHELL
      end

      if name == "loadbalancer"
        node.vm.provision "shell", path: "lb-setup.sh"
      end

    end
  end
end