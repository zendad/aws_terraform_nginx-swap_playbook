Vagrant.configure(2) do |config|

  config.vm.box ="bento/centos-7.3"
  config.vm.hostname = "web1.dereck.com"
  config.vm.network "private_network", ip: "192.168.10.21"
  config.vm.network "forwarded_port", guest: 80, host: 80
  config.vm.provision "shell", inline: <<-SHELL
       sed -i '$ a 192.168.10.22 web1.dereck.com' /etc/hosts           
    SHELL
  config.vm.provision "ansible" do |ansible|
    ansible.verbose = "v"
    ansible.playbook = "site-swap.yml"
  end
end
