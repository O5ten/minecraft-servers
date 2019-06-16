# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  config.vm.box = "aeciopires/ubuntu-18.04-64-docker"
  config.vm.provider :virtualbox do |vb|
      vb.memory = 4096
      vb.cpus = 4
      vb.customize [ "modifyvm", :id, "--uartmode1", "disconnected" ]
  end

  # Köhns-1
  config.vm.network "forwarded_port", guest: 25567, host: 25567
  config.vm.synced_folder "server_data", "/home/vagrant/server_data", create: true, SharedFoldersEnableSymlinksCreate: false
  config.vm.provision "shell", inline: <<-SHELL
      echo ==================== Launching Köhns 1 ====================
      
      mkdir -p /home/vagrant/server_data/kohns-1
      docker run  -d -p 25567:25565 \
                  -v /home/vagrant/server_data/kohns-1:/data \
                  --name kohns-1 \
                  --restart=always \
                  -e MAX_TICK_TIME=120000 \
                  -e EULA=TRUE \
                  -e MEMORY=2500m \
                  -e WHITELIST=kohnwincent,kitain,irongon,diamondcrusher31 \
                  -e OPS=kohnwincent,kitain \
                  -e SERVER_NAME=Kohns-1 \
                  -e ICON="https://media-minecraftforum.cursecdn.com/attachments/6/180/635404578945887159.png" itzg/minecraft-server:latest 
  SHELL
  
  # Minecraft Router
  config.vm.network "forwarded_port", guest: 25565, host: 25565
  config.vm.network "forwarded_port", guest: 80, host: 8086
  config.vm.provision "shell", inline: <<-SHELL 
      echo ==================== Launching Minecraft Router ====================
      docker run -d -p 25565:25565 -p 80:80 \
                  --link kohns-1:kohns-1 \
                  --name minecraft-router \
                  itzg/mc-router --port=25565 \
                                 --mapping norrland.05ten.se=kohns-1:25565 \
                                 --api-binding :80
  SHELL

  # Minecraft Status http://minecraft.05ten.se/status
  config.vm.network "forwarded_port", guest: 8085, host: 8085
  config.vm.provision "shell", inline: <<-SHELL
      echo ==================== Launching Minecraft Status ====================
      docker run -d -p 8085:8080 \
                --link kohns-1 \
                --name minecraft-status \
                itzg/mc-status --mcstatus.servers=kohns-1 --cors.allowAll=true
  SHELL
end
