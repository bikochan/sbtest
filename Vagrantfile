# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  # This box uses rsync vs guest-addons, run vagrant rsync-auto after VM boot to auto sync if needed.
  config.vm.box = "centos/7"
  N=3
  (1..N).each do |i|

    hostname = "node#{i}"
    config.vm.define hostname do |node|
      node.vm.network "private_network", ip: "172.28.128.#{10+i}"
      node.vm.provider "virtualbox" do |vb|
        vb.gui    = false
        vb.memory = "768"
        vb.name   = hostname
      end

      # Install the SSH public key on all nodes
      node.vm.provision "shell", inline: <<-ALL_NODES
        cat /home/vagrant/sync/files/ansible.pub >> /home/vagrant/.ssh/authorized_keys
      ALL_NODES

      # The last node is the controller which ensures the other nodes are booted first
      # Install EPEL, ansible 2.0 and a default config so we don't have to move stuffs around
      # then run the playbook
      # Also install the private SSH key.
      # In real world, that key would not be stored in GIT nor copied this way
      if i == N
        node.vm.provision "shell", inline: <<-CONTROLER
          yum install -y epel-release
          yum install -y ansible
          cp /home/vagrant/sync/files/ansible.cfg /home/vagrant/.ansible.cfg
          cp /home/vagrant/sync/files/ansible /home/vagrant/.ssh/id_rsa
          chown vagrant:vagrant /home/vagrant/.ssh/id_rsa
          chmod 0600 /home/vagrant/.ssh/id_rsa
          sudo -H -n -u vagrant ansible-playbook -v /home/vagrant/sync/playbooks/node.yml
        CONTROLER
        node.vm.post_up_message = "To test connect to http://172.28.128.13 or run `./test.sh`"

      end

    end

  end


end
