# Префикс для LAN сети
PRIVATE_NET="192.168.130."
# Домен который будем использовать для всей площадки
DOMAIN="vagrant-redis.local"
# Массив из хешей, в котором заданы настройки для каждой виртуальной машины
servers=[
  {
    :hostname => "app1." + DOMAIN,
    :ip => PRIVATE_NET + "150",
    :ram => 512,
    :cpu => 1
  },
  {
    :hostname => "app2." + DOMAIN,
    :ip => PRIVATE_NET + "151",
    :ram => 750,
    :cpu => 1
  },
  {
    :hostname => "app3." + DOMAIN,
    :ip => PRIVATE_NET + "152",
    :ram => 441,
    :cpu => 1
  },
  {
    :hostname => "app4." + DOMAIN,
    :ip => PRIVATE_NET + "153",
    :ram => 1024,
    :cpu => 1
  }
]

Vagrant.configure("2") do |config|
  # Отключить дефолтную шару
  config.vm.synced_folder ".", "/vagrant", disabled: true
  # Проходим по элементах массива "servers"
  servers.each do |machine|  
    # Применяем конфигурации для каждой машины. Имя машины(как ее будет видно в Vbox GUI) находится в переменной "machine[:hostname]"
      config.vm.define machine[:hostname] do |node|
          
          # Стандартная Ubuntu 20.04 
          node.vm.box = "bento/ubuntu-20.04"

          #################################################################
          # NETWORK
          # https://www.vagrantup.com/docs/networking/public_network.html        
          #################################################################
          node.vm.network 'private_network', ip: machine[:ip], auto_config: "false"                                                                                   
          #################################################################
          # SSH
          ################################################################# 
          node.ssh.forward_agent = true
          node.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'" # avoids 'stdin: is not a tty' error.
          node.vm.hostname = machine[:hostname]
          node.ssh.username = "vagrant"
          node.ssh.password = "vagrant"
          node.vm.provision "shell", inline: <<-SHELL
             #sudo apt update 2>/dev/null | grep packages | cut -d '.' -f 1 && sudo apt install -y git libssl-dev libsystemd-dev build-essential pkg-config 2>/dev/null | grep packages
             #git clone https://github.com/redis/redis.git && cd redis && sudo make USE_SYSTEMD=yes BUILD_TLS=yes && sudo make test && sudo make install
             #echo "Ок, сценарий завершен"
             sudo apt update 2>/dev/null | grep packages | cut -d '.' -f 1 && sudo apt install -y redis-server 2>/dev/null
          SHELL
          #

          ##################################################################
          # VIRTUALBOX conficurations
          ##################################################################  
          #VirtualBox в качестве провайдера
          node.vm.provider :virtualbox do |vb|
             vb.gui = false
             vb.name = machine[:hostname]
              # Размер RAM памяти
             vb.customize ["modifyvm", :id, "--memory", machine[:ram]]
             vb.customize ["modifyvm", :id, "--cpus",  machine[:cpu]]
             vb.customize ["modifyvm", :id, "--cpuexecutioncap", "100"]
             vb.customize ["modifyvm", "--size", "4096"]

   
          end
      end
  end   
end
