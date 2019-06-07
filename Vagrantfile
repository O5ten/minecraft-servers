# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  config.vm.box = "aeciopires/ubuntu-18.04-64-docker"
  config.vm.provider :virtualbox do |vb|
      vb.memory = 2048
      vb.customize [ "modifyvm", :id, "--uartmode1", "disconnected" ]
  end
  config.vm.network "public_network", ip: "192.168.1.111"
  config.vm.synced_folder "server_data", "/home/vagrant/server_data", create: true, SharedFoldersEnableSymlinksCreate: false
  config.vm.provision "shell", inline: <<-SHELL
      echo ==================== Launching Köhns 1 ====================
      mkdir -p /home/vagrant/server_data/kohns-1
      docker run --rm -d -p 25567:25565 \
                 -v /home/vagrant/server_data/kohns-1:/data \
                 -e MAX_TICK_TIME=120000 \
                 -e EULA=TRUE \
                 -e MEMORY=1500m \
                 -e WHITELIST=kohnwincent,kitain,irongon,diamondcrusher31 \
                 -e OPS=kohnwincent,kitain \
                 -e SERVER_NAME=Kohns-1 \
                 -e ICON=https://media-minecraftforum.cursecdn.com/attachments/6/180/635404578945887159.png \ 
                 --name kohns-1 \
                 --restart=always \
                 itzg/minecraft-server:latest
  SHELL
end
