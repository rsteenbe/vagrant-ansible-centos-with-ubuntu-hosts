servers=[
  {
    :hostname => "host1",
    :ip => "192.168.100.11",
    :box => "bento/ubuntu-20.04",
    :ram => 2048,
    :cpu => 2,
	:port => 81
  },
  {
    :hostname => "host2",
    :ip => "192.168.100.12",
    :box => "bento/ubuntu-20.04",
    :ram => 2048,
    :cpu => 2,
	:port => 82
  },
  {
    :hostname => "host3",
    :ip => "192.168.100.13",
    :box => "bento/ubuntu-20.04",
    :ram => 2048,
    :cpu => 2,
	:port => 83
  },
  {
    :hostname => "host4",
    :ip => "192.168.100.14",
    :box => "bento/ubuntu-20.04",
    :ram => 2048,
    :cpu => 2,
	:port => 84
  }
]

Vagrant.configure(2) do |config|

	config.vm.provision "shell", inline: "echo Ansible CentOS server and host Ubuntu servers"
	
    config.vm.define "ansible" do |ansible|
    ansible.vm.box = "bento/centos-8"
	ansible.vm.network "private_network", ip: "192.168.100.10"
	ansible.vm.provider "virtualbox" do |v|
		v.memory = 4096
		v.cpus = 2
	end
	ansible.vm.provision "shell", inline: <<-SHELL
	  /bin/echo "================= Installation Ansible Started ================"
	  sudo yum update
	  sudo yum install epel-release -y
	  sudo yum install ansible -y
	  sudo yum install sshpass -y
	  # ssh-keyscan -H 192.168.50.10 >> ~/.ssh/known_hosts
	  sed -i 's/#host_key_checking = False/host_key_checking = False/' /etc/ansible/ansible.cfg
	  SHELL
    end

    servers.each do |machine|
        config.vm.define machine[:hostname] do |node|
		    node.vm.network "forwarded_port", guest: 80, host: machine[:port] 
            node.vm.box = machine[:box]
            node.vm.hostname = machine[:hostname]
            node.vm.network "private_network", ip: machine[:ip]
            node.vm.provider "virtualbox" do |vb|
                vb.customize ["modifyvm", :id, "--memory", machine[:ram]]
            end
        end
    end
end