# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # Use the Fedora 40 Cloud Base box
  config.vm.box = "fedora/40-cloud-base"
  config.vm.disk :disk, size: 40, primary: true
  config.vm.network "private_network", ip: "192.168.121.125"

  config.vm.provider "libvirt" do |libvirt|
    libvirt.uri = 'qemu:///system'
    libvirt.memory = 8192
    libvirt.cpus = 8
    libvirt.nic_model_type = 'virtio'  # Use virtio for network interface
    libvirt.machine_virtual_size = 40
  end

  # Shell provisioning script
  config.vm.provision "shell", inline: <<-SHELL
    dnf update -y
    dnf install -y software-properties-common
    dnf-add-repository ppa:ansible/ansible -y
    dnf update -y
    dnf install -y ansible
    ansible --version
    dnf install -y cloud-utils-growpart parted
    growpart /dev/vda 4
    btrfs filesystem resize max /
    nmcli connection modify "Wired connection 2" ipv4.addresses 192.168.121.125/24
    nmcli connection modify "Wired connection 2" ipv4.method manual
    nmcli connection down "Wired connection 2" && sudo nmcli connection up "Wired connection 2"
    reboot
  SHELL
end

