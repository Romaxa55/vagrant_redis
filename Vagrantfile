
Vagrant.configure("2") do |config|
  # Стандартная Ubuntu 20.04 
  config.vm.box = "bento/ubuntu-20.04"
  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  #################################################################
  # NETWORK
  # https://www.vagrantup.com/docs/networking/public_network.html        
  #################################################################
  # config.vm.network "public_network", bridge: "Intel(R) 82579LM Gigabit Network Connection"
  # config.vm.network "public_network", type: "dhcp", bridge: "en0: Wi-Fi (AirPort)"
  config.vm.network 'private_network', ip: "192.168.130.10", auto_config: "false"                                                                                   
  # config.vm.network 'private_network', ip: "192.168.130.20", auto_config: "false"
  # config.vm.network 'private_network', ip: "192.168.130.30", auto_config: "false"
  # config.vm.network 'private_network', ip: "192.168.130.40", auto_config: "false"
  #config.vm.network :forwarded_port, guest: 5601, host: 5601
  #config.ssh.port = 2222
  #config.ssh.host = "192.168.130.10"
  #config.vm.network :forwarded_port, guest: 22, host: 2222, id: "ssh", auto_correct: true
  #################################################################
  # SSH
  ################################################################# 
  config.ssh.forward_agent = true
  config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'" # avoids 'stdin: is not a tty' error.
  config.vm.hostname = "server"
  config.ssh.username = "vagrant"
  config.ssh.password = "vagrant"
  #config.vm.provision :shell, :inline => 'sed -i -e "s/PasswordAuthentication no/PasswordAuthentication yes/g" /etc/ssh/sshd_config'
  #config.vm.provision :shell, :inline => 'sudo service sshd restart'
  config.vm.provision "shell", inline: <<-SHELL
     sudo apt update 2>/dev/null | grep packages | cut -d '.' -f 1 && sudo apt install -y git libssl-dev libsystemd-dev build-essential pkg-config 2>/dev/null | grep packages
     git clone https://github.com/redis/redis.git && cd redis && sudo make USE_SYSTEMD=yes BUILD_TLS=yes && sudo make test && sudo make install
     echo "Ок, сценарий завершен"

  SHELL
  #

  ##################################################################
  # VIRTUALBOX conficurations
  ##################################################################  
  #VirtualBox в качестве провайдера
  config.vm.provider :virtualbox do |vb|
  # Don't boot with headless mode
   vb.gui = false
   vb.name = "vagrant-redis"
   vb.customize ["modifyvm", :id, "--memory", "512"]
   vb.customize ["modifyvm", :id, "--vram", "128"]
   vb.customize ["modifyvm", :id, "--cpus", "1"]
   vb.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
   # vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
   # vb.customize ["modifyvm", :id, "--ioapic", "on"]
   # vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
  end
end
