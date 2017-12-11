# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.vm.box = "centos/7"

  config.vm.hostname = "centos-server"

  if Vagrant::Util::Platform.windows?
    ENV["VAGRANT_DETECTED_OS"] = ENV["VAGRANT_DETECTED_OS"].to_s + " cygwin"
  end

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  config.vm.network "private_network", ip: "192.168.33.10"
#  config.vm.network "forwarded_port", guest: 3000, host: 3000
  config.vm.network "forwarded_port", guest: 8983, host: 8983
  config.vm.network "forwarded_port", guest: 3306, host: 3306

  config.vm.synced_folder ".", "/vagrant", disabled: true
  # If you are using Windows, it may be more faster and to use the RSync.
  # type: "rsync"
  config.vm.synced_folder "ansible", "/home/vagrant/ansible",
    :owner => 'vagrant', :group => 'vagrant'
#    , :mount_options => ['dmode=777', 'fmode=777']
  config.vm.synced_folder "repositories", "/home/vagrant/repositories",
    :owner => 'vagrant', :group => 'vagrant'
#    , :mount_options => ['dmode=777', 'fmode=766']
# 全許可にしなくてもホストOSの設定がVirtualBoxの共有だと維持されていたのでコメントアウト

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    vb.cpus = "2"
  end

  config.vm.boot_timeout = 5400

  config.ssh.forward_agent = true

  # set auto_update to false, if you do NOT want to check the correct
  # additions version when booting this machine
  # config.vbguest.auto_update = false

#  # Base provisioning
#  config.vm.provision "shell", inline: <<-SHELL
#    sudo yum -y update kernel
#    sudo yum -y install kernel-devel kernel-headers dkms gcc gcc-c++
#  SHELL
#  config.vm.provision "reload"

  config.vm.provision "shell", :privileged => false, inline: <<-SHELL
    ssh-keygen -f ~/.ssh/id_rsa -t rsa -N ""
    cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
  SHELL

  # Developing environment constructing
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "/home/vagrant/ansible/local.yml"
    ansible.provisioning_path = "/home/vagrant/ansible"
    ansible.limit = "all"
    ansible.install = true
    ansible.inventory_path = "/home/vagrant/ansible/local"
    #ansible.tags = "server,java,solr"
    #ansible.skip_tags = ""
  end

end
