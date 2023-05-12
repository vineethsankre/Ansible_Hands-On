Vagrant.configure("2") do |config|
  # Set up four servers
  servers = [
    {
      :hostname => "db1",
      :box => "ubuntu/focal64",
      :ip => "192.168.169.130",
      :ssh_port => "2222"
    },
    {
      :hostname => "web1",
      :box => "ubuntu/focal64",
      :ip => "192.168.169.131",
      :ssh_port => "2223"
    },
    {
      :hostname => "web2",
      :box => "ubuntu/focal64",
      :ip => "192.168.169.132",
      :ssh_port => "2224"
    },
    {
      :hostname => "lb",
      :box => "ubuntu/focal64",
      :ip => "192.168.169.133",
      :ssh_port => "2225"
    }
  ]

  config.vm.base_address = 600

  # Forward SSH agent to VMs for Ansible
  config.ssh.forward_agent = true

  # Set up SSH forwarding for each machine
  servers.each do |machine|
    config.vm.define machine[:hostname] do |node|
      node.vm.box = machine[:box]
      node.vm.hostname = machine[:hostname]

      # Set custom IP and SSH port
      node.vm.network :private_network, ip: machine[:ip]
      node.vm.network "forwarded_port", guest: 22, host: machine[:ssh_port], id: "ssh"

      # Customize VM settings
      node.vm.provider :virtualbox do |v|
        v.customize ["modifyvm", :id, "--memory", 2048]
        v.customize ["modifyvm", :id, "--name", machine[:hostname]]
      end
    end
  end
end
