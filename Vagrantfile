Vagrant.configure(2) do |config|

  config.vm.box ="bento/centos-7.3"
  config.vm.hostname = "web1.dereck.com"
  config.vm.network :forwarded_port, guest: 80, host: 80
  config.vm.network :private_network, ip: "10.0.0.10"
  config.vm.provision "shell", inline: <<-SHELL
       sed -i '$ a 127.0.0.1 web1.dereck.com' /etc/hosts           
    SHELL
  config.vm.provision "ansible" do |ansible|
    ansible.verbose = "v"
    ansible.playbook = "site-swap.yml"
  end
  
  config.vm.provider "virtualbox" do |v|
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
  end
end
