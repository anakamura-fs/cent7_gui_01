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
  end

  config.vm.provision "shell", inline: <<-SHELL
    yum -y install epel-release
    yum -y groupinstall "X Window System" "Japanese Support"
    yum -y install lxqt-about lxqt-common lxqt-config lxqt-globalkeys lxqt-notificationd lxqt-openssh-askpass lxqt-panel lxqt-policykit lxqt-powermanagement lxqt-qtplugin lxqt-runner lxqt-session network-manager-applet nm-connection-editor pcmanfm-qt qterminal-qt5 openbox
  SHELL
end
