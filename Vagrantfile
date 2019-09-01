# -*- mode: ruby -*-
# vi: set ft=ruby ts=100 sw=2:

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vm.box_version = "1901.01"

  # config.vm.box_check_update = false

  # config.vm.synced_folder "../data", "/vagrant_data"

  config.vm.provider "virtualbox" do |vb|
    vb.gui = true
    vb.memory = "2048"
    vb.cpus = 2
    vb.customize ["modifyvm", :id, "--ioapic", "on"] # I/O APICの有効化
  end

  config.vm.provision "shell", inline: <<-SHELL
    yum -y install epel-release
    yum -y groupinstall "X Window System" "Japanese Support" "Input Methods"

    yum -y install lxqt-about lxqt-common lxqt-config lxqt-globalkeys lxqt-notificationd lxqt-openssh-askpass lxqt-panel lxqt-policykit lxqt-powermanagement lxqt-qtplugin lxqt-runner lxqt-session network-manager-applet nm-connection-editor pcmanfm-qt qterminal-qt5 openbox

    # 「CentOS7にxfce4をインストールする - Qiita」 https://qiita.com/n-yamanaka/items/cd02aa3f9737d66f42d3
    yum -y install vlgothic-* ipa-gothic-fonts ipa-mincho-fonts ipa-pgothic-fonts ipa-pmincho-fonts

    #systemctl get-default | grep graphical.target || (
      #echo change runlevel to GUI...
      #systemctl set-default graphical.target
      #echo REBOOT recommended.
    #)

    which docker || (
      yum install -y yum-utils device-mapper-persistent-data lvm2
      yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
      yum install -y docker-ce docker-ce-cli containerd.io
      systemctl start docker
      systemctl enable docker
    )
    which docker-compose || (
      curl -L https://github.com/docker/compose/releases/download/1.24.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
      chmod +x /usr/local/bin/docker-compose
    )

    # vagrant -> docker group
    grep docker /etc/group || (
      groupadd docker
    )
    TARGET_USER=vagrant
    groups $TARGET_USER | grep docker || (
      usermod -G docker $TARGET_USER
    )

  SHELL

  # for non root vagrant user
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    (
      echo "export GTK_IM_MODULE=ibus"
      echo "export XMODIFIERS=@im=ibus"
      echo "export QT_IM_MODULE=ibus"
      echo "ibus-daemon -drx"

      echo "export LANG=ja_JP.UTF-8"
      echo "exec startlxqt"
    ) > ~/.xinitrc

  SHELL
end
