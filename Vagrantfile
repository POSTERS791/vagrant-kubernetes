# coding: utf-8
# -*- mode: ruby -*-
#

Vagrant.configure(2) do |config|

  # ノード１ 仮想マシンの起動
  #
  config.vm.define 'node1' do |machine|
    machine.vm.box = "ubuntu/xenial64"
    machine.disksize.size = '20GB'  # ポッド数が多いと足りなくなる
    machine.vm.hostname = 'node1'
    machine.vm.network :private_network,ip: "172.16.20.12"
    #machine.vm.network :public_network, ip: "192.168.1.92", bridge: "en0: Ethernet"
    machine.vm.provider "virtualbox" do |vbox|
      vbox.gui = false
      vbox.cpus = 2
      vbox.memory = 8192
      # DISK
      vdisk = "vdisk/sdb-1.vdi"
      # CREATE DISK
      if not File.exist?(vdisk) then
         vbox.customize [
           'createmedium', 'disk',
           '--filename', vdisk,
           '--format', 'VDI',
           '--size', 204800 ]  # 200GiB
      end
      # ATTACH DISK
      vbox.customize [
        'storageattach', :id,
        '--storagectl', 'SCSI',
        '--port', 2,
        '--device', 0,
        '--type', 'hdd',
        '--medium', vdisk]
    end
    machine.vm.synced_folder ".", "/vagrant", owner: "vagrant",
      group: "vagrant", mount_options: ["dmode=700", "fmode=700"]

    # ノード１ docker & k8sのインストール
    #
    machine.vm.provision "ansible_local" do |ansible|
      ansible.playbook       = "ansible-playbook/kubernetes.yml"
      ansible.version        = "latest"
      ansible.verbose        = false
      ansible.install        = true
      ansible.limit          = "node1"
      ansible.inventory_path = "ansible-playbook/hosts"
    end
  end


  # ノード２ 仮想マシンの起動
  #
  config.vm.define 'node2' do |machine|
    machine.vm.box = "ubuntu/xenial64"
    machine.disksize.size = '20GB'    
    machine.vm.hostname = 'node2'
    machine.vm.network :private_network,ip: "172.16.20.13"
    #machine.vm.network :public_network, ip: "192.168.1.93", bridge: "en0: Ethernet"
    machine.vm.provider "virtualbox" do |vbox|
      vbox.gui = false
      vbox.cpus = 2
      vbox.memory = 8192
      # DISK
      vdisk = "vdisk/sdb-2.vdi"
      # CREATE DISK
      if not File.exist?(vdisk) then
         vbox.customize [
           'createmedium', 'disk',
           '--filename', vdisk,
           '--format', 'VDI',
           '--size', 204800 ]  # 200GiB
      end
      # ATTACH DISK
      vbox.customize [
        'storageattach', :id,
        '--storagectl', 'SCSI',
        '--port', 2,
        '--device', 0,
        '--type', 'hdd',
        '--medium', vdisk]
    end
    machine.vm.synced_folder ".", "/vagrant", owner: "vagrant",
      group: "vagrant", mount_options: ["dmode=700", "fmode=700"]

    # ノード２ docker & k8sのインストール
    #
    machine.vm.provision "ansible_local" do |ansible|
      ansible.playbook       = "ansible-playbook/kubernetes.yml"
      ansible.version        = "latest"
      ansible.verbose        = false
      ansible.install        = true
      ansible.limit          = "node2"
      ansible.inventory_path = "ansible-playbook/hosts"
    end
  end

  # ノード３ 仮想マシンの起動
  #
  config.vm.define 'node3' do |machine|
    machine.vm.box = "ubuntu/xenial64"
    machine.disksize.size = '20GB'
    machine.vm.hostname = 'node3'
    machine.vm.network :private_network,ip: "172.16.20.14"
    #machine.vm.network :public_network, ip: "192.168.1.94", bridge: "en0: Ethernet"
    machine.vm.provider "virtualbox" do |vbox|
      vbox.gui = false
      vbox.cpus = 2
      vbox.memory = 8192
      # DISK
      vdisk = "vdisk/sdb-3.vdi"
      # CREATE DISK
      if not File.exist?(vdisk) then
         vbox.customize [
           'createmedium', 'disk',
           '--filename', vdisk,
           '--format', 'VDI',
           '--size', 204800 ]  # 200GiB
      end
      # ATTACH DISK
      vbox.customize [
        'storageattach', :id,
        '--storagectl', 'SCSI',
        '--port', 2,
        '--device', 0,
        '--type', 'hdd',
        '--medium', vdisk]
    end
    machine.vm.synced_folder ".", "/vagrant", owner: "vagrant",
      group: "vagrant", mount_options: ["dmode=700", "fmode=700"]

    # ノード３ docker & k8sのインストール
    #
    machine.vm.provision "ansible_local" do |ansible|
      ansible.playbook       = "ansible-playbook/kubernetes.yml"
      ansible.version        = "latest"
      ansible.verbose        = false
      ansible.install        = true
      ansible.limit          = "node3"
      ansible.inventory_path = "ansible-playbook/hosts"
    end
  end
  

  #
  # マスタ 仮想マシンの起動
  #
  config.vm.define 'master' do |machine|
    machine.vm.box = "ubuntu/xenial64"
    machine.vm.hostname = 'master'
    machine.vm.network :private_network,ip: "172.16.20.11"
    #machine.vm.network :public_network, ip: "192.168.1.91", bridge: "en0: Ethernet"
    machine.vm.provider "virtualbox" do |vbox|
      vbox.gui = false
      vbox.cpus = 2
      vbox.memory = 1024
    end
    machine.vm.synced_folder ".", "/vagrant", owner: "vagrant",    
      group: "vagrant", mount_options: ["dmode=700", "fmode=700"]

    #
    # マスタ docker & k8sのインストール
    #
    machine.vm.provision "ansible_local" do |ansible|
      ansible.playbook       = "ansible-playbook/kubernetes.yml"
      ansible.version        = "latest"
      ansible.verbose        = false
      ansible.install        = true
      ansible.limit          = "master" # or only "nodes" group, etc.
      ansible.inventory_path = "ansible-playbook/hosts"
    end

    # マスタのセットアップ
    #
    machine.vm.provision "ansible_local" do |ansible|
      ansible.playbook      = "ansible-playbook/k8s_master.yml"
      ansible.version        = "latest"
      ansible.verbose        = false
      ansible.install        = false
      ansible.limit          = "master"
      ansible.inventory_path = "ansible-playbook/hosts"
    end

    # ノードのセットアップ
    #
    machine.vm.provision "ansible_local" do |ansible|
      ansible.playbook      = "ansible-playbook/k8s_nodes.yml"
      ansible.version        = "latest"
      ansible.verbose        = false
      ansible.install        = false
      ansible.limit          = "nodes"
      ansible.inventory_path = "ansible-playbook/hosts"
    end

    # ノードのCeph用ストレージの設定
    #
    machine.vm.provision "ansible_local" do |ansible|
      ansible.playbook      = "ansible-playbook/node_storage.yml"
      ansible.version        = "latest"
      ansible.verbose        = false
      ansible.install        = false
      ansible.limit          = "nodes"
      ansible.inventory_path = "ansible-playbook/hosts"
    end
    
  end

  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end

end
