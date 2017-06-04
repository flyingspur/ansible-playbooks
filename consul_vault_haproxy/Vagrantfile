# -*- mode: ruby -*-
# vi: set ft=ruby :

servers=[
  {
    :hostname => "nodea",
    :ip => "192.168.33.10"
  },
  {
    :hostname => "nodeb",
    :ip => "192.168.33.11"
  },
  {
    :hostname => "nodec",
    :ip => "192.168.33.12"
  },
  {
    :hostname => "noded",
    :ip => "192.168.33.13"
  }
]
Vagrant.configure(2) do |config|
    config.ssh.insert_key = false
    servers.each_with_index do |machine, index|
        config.vm.define machine[:hostname] do |node|
            node.vm.box_check_update = false
            node.vm.box = "centos7_mini"
            node.vm.hostname = "#{machine[:hostname]}.example.org"
            node.vm.network "private_network", ip: machine[:ip]
            node.vm.network "forwarded_port", guest: 80, host: "819#{index}"
            node.vm.network "forwarded_port", guest: 8500, host: "885#{index}"
            node.vm.network "forwarded_port", guest: 8200, host: "825#{index}"
            node.vm.network "forwarded_port", guest: 1936, host: "1193#{index}"
            node.vm.synced_folder ".", "/vagrant"
            node.vm.provider "virtualbox" do |vb|
                vb.customize ["modifyvm", :id, "--memory", 1024]
                vb.linked_clone = true if Vagrant::VERSION =~ /^1.8/
            end
            config.vm.provision "shell",  inline: <<-SHELL
              sudo systemctl restart rsyslog
              sudo yum upgrade -y
            SHELL
        end
    end
    config.vm.provision "ansible" do |ansible|
      ansible.verbose="v"
      ansible.playbook="consul_vault_haproxy.yml"
      ansible.groups = {
      "bootstrap_server" => ["nodea"],
      "regular_server" => ["nodeb", "nodec"],
      "webserver" => ["noded"]
      }
    end
end
