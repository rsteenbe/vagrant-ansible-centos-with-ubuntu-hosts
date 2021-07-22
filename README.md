# Ansible on CentOS8 and several Host servers on Ubuntu 20

Run following commands in the command line:

`vagrant ssh ansible`

`sudo ansible all -i /vagrant/ansible/inventory -m ping`

If this succeeds, it means all servers are running.