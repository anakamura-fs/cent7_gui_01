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
    vb.customize ["modifyvm", :id, "--ioapic", "on"] # I/O APIC‚Ì—LŒø‰»
  end

  config.vm.provision "shell", inline: <<-SHELL
    yum -y install epel-release
    # yum -y update # needed at first time
    yum -y groupinstall "X Window System" "Japanese Support" Fonts Xfce "Input Methods"

    systemctl get-default | grep graphical.target || (
      #echo change runlevel to GUI...
      #systemctl set-default graphical.target
      #echo REBOOT recommended.
      :
    )

    localectl status | grep "System Locale: LANG=ja_JP.UTF-8" || (
      localectl set-locale LANG=ja_JP.UTF-8 LANGUAGE="ja_JP:ja"
      localectl set-keymap jp106
      #localectl set-x11-keymap jp
    )

    yum install -y vlgothic-* ipa-gothic-fonts ipa-mincho-fonts ipa-pgothic-fonts ipa-pmincho-fonts ibus-kkc
  SHELL

  # for non root vagrant user
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    (
    echo "export LANG=ja_JP.UTF-8"
    # echo "exec startxfce4"

    # thank you: https://qiita.com/msakamoto_sf/items/bf2e37b22ae6694440c3
    echo "export GTK_IM_MODULE=ibus"
    echo "export XMODIFIERS=@im=ibus"
    echo "export QT_IM_MODULE=ibus"
    echo "ibus-daemon -drx"

    echo "exec /usr/bin/xfce4-session"
    ) > ~/.xinitrc
  SHELL
end
