Vagrant.configure("2") do |config|
  # Specify the box to use
  config.vm.box = "ubuntu/jammy64"

  # Assign a name to the VM
  config.vm.hostname = "ubuntu-vm"

  # Network settings
  config.vm.network "public_network" # To expose the VM to the internet
  config.vm.network "forwarded_port", guest: 80, host: 8080 # HTTP
  config.vm.network "forwarded_port", guest: 443, host: 8443 # HTTPS

  # Customize resources
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048" # Set VM memory
    vb.cpus = 2        # Set VM CPUs
  end

  # Provisioning: Basic system setup without Docker
  config.vm.provision "shell", inline: <<-SHELL
    # Update and upgrade the system
    apt-get update && apt-get upgrade -y

    # Install necessary tools
    apt-get install -y git curl

    # Install Docker Compose
    sudo apt-get update
    sudo apt-get install docker-ce-cli containerd.io -y
    sudo curl -L "https://github.com/docker/compose/releases/download/2.23.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    sudo chmod +x /usr/local/bin/docker-compose
   

    # Add vagrant user to the docker group (if Docker is installed later)
    if getent group docker > /dev/null; then
      usermod -aG docker vagrant
    fi
  SHELL
end
