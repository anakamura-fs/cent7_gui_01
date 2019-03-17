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
    yum -y update
    yum -y groupinstall "X Window System" "Japanese Support" Xfce

    systemctl get-default | grep graphical.target || (
      echo change runlevel to GUI...
      systemctl set-default graphical.target
      echo REBOOT recommended.
    )

  SHELL

  # for non root vagrant user
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    echo "exec startxfce4" > ~/.xinitrc

  SHELL
end
