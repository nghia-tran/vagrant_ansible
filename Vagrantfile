# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

    config.ssh.insert_key = false

    ##########################
    ## Routing Engine  #######
    ##########################
    config.vm.define "vqfx" do |vqfx|
        vqfx.vm.hostname = "vqfx"
        vqfx.vm.box = 'juniper/vqfx10k-re'

        # DO NOT REMOVE / NO VMtools installed
        vqfx.vm.synced_folder '.', '/vagrant', disabled: true

        # Management port (em1 / em2)
        vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "reserved-bridge"
        vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', intnet: "reserved-bridge"

        # (em3  em4)
        (1..2).each do |seg_id|
            vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', intnet: "segment"
        end
    end
    if !Vagrant::Util::Platform.windows?
        config.vm.provision "ansible" do |ansible|
            ansible.groups = {
		'all'          => ["vqfx"]
                #"vqfx10k"      => ["spine1", "spine2", "leaf1", "leaf2", "leaf3" ],
                #"spine"        => ["spine1", "spine2"],
                #"leaf"         => ["leaf1", "leaf2", "leaf3" ],
                #"server"       => ["srv1", "srv2", "srv3" ],
                #"all:children" => ["vqfx10k", "server" ]
            }
            ansible.playbook = "main.yaml"
        end
    end
end
