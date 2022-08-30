Vagrant.configure("2") do |config|
  config.vm.box = "generic/centos7"
    config.vm.provision "shell", inline: <<-SHELL
	  #задание 1
      #файл с конфигурацией сервиса
      echo -en '# Configuration file for my watchdog service\n# Place it to /etc/sysconfig\n\n# File and word in that file that we will be monit\nWORD="ALERT"\nLOG=/var/log/watchlog.log\n\n' | sudo tee /etc/sysconfig/watchlog
      #скрипт
      echo -en '#!/bin/bash\n\nWORD=$1\nLOG=$2\nDATE=`date`\n\nif grep $WORD $LOG &> /dev/null\nthen\n   logger "$DATE: I found word, Master!"\nelse\n   exit 0\nfi\n\n' | sudo tee /opt/watchlog.sh
      sudo chmod +x /opt/watchlog.sh
      #юнит сервиса
      echo -en '[Unit]\nDescription=My watchlog service\n\n[Service]\nType=oneshot\nEnvironmentFile=/etc/sysconfig/watchlog\nExecStart=/opt/watchlog.sh $WORD $LOG\n\n' | sudo tee /etc/systemd/system/watchlog.service
      #юнит таймера
      echo -en '[Unit]\nDescription=Run watchlog script every 30 second\n\n[Timer]\n# Run every 30 second\nOnUnitActiveSec=30\nUnit=watchlog.service\n\n[Install]\nWantedBy=multi-user.target\n\n' | sudo tee /etc/systemd/system/watchlog.timer
      echo -en 'hdhfusidhfif\neggregregerwg\ngregregreg\nALERT\n\n' | sudo tee /var/log/watchlog.log
      sudo systemctl start watchlog.timer
	  #задание 2
	  yum install -y epel-release && yum install -y spawn-fcgi php php-cli mod_fcgid httpd
	  sudo sed -i 's/#SOCKET=/SOCKET=/;s/#OPTIONS=/OPTIONS=/' /etc/sysconfig/spawn-fcgi
	  echo -en '[Unit]\nDescription=Spawn-fcgi startup service by Otus\nAfter=network.target\n\n[Service]\nType=simple\nPIDFile=/var/run/spawn-fcgi.pid\nEnvironmentFile=/etc/sysconfig/spawn-fcgi\nExecStart=/usr/bin/spawn-fcgi -n $OPTIONS\nKillMode=process\n\n[Install]\nWantedBy=multi-user.target\n' | sudo tee /etc/systemd/system/spawn-fcgi.service
	  #задание 3
	  sudo sed -i 's:EnvironmentFile=/etc/sysconfig/httpd:EnvironmentFile=/etc/sysconfig/httpd-%I:' /usr/lib/systemd/system/httpd.service
	  
	  
	  #конфиг-файлы
	  
	  
    SHELL
end
