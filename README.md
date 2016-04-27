
# Instructions

For simplicity, we'll use vagrant/virtualbox on the local machine
as our sole dependency so that most people can test regardless of their host system (hopefully)

Node1 and Node2 are running the backend application.

Node3 is the ansible controller aka ansbile
and SSH keys will be installed on this node to configure all 3 nodes.

The systems are based on the offcial CentOS 7 box images without any addon.
(so run `vagrant rsync` if you change anything)
As a result, Vagrant needs to provision the bare minimun for Ansible to work.

These steps would normally be baked into the images.
See the Vagrantfile for the provisioning details.

To start the whole setup, run:
`vagrant up`

To test connect to http://172.28.128.13 
or run `./test.sh`

To rebuild/deploy:
 - from your machine, 
 	- edit files/app.go
 	- run
 		- `vagrant rsync node3`
 		- `vagrant ssh node3 -- ansible-playbook -v /home/vagrant/sync/playbooks/node.yml`
 - from node3:
 	- edit sync/files/app.go
 	- run
 		- `ansible-playbook -v /home/vagrant/sync/playbooks/node.yml`
 

# Requirements

On the host, you will need to have installed
  - vagrant (1.7.x)
  - virtualbox (5.0.x)

