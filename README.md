## Simple Nginx and Swap Memory configuration on EC2 instances using ansible

- Requires Ansible 1.2 or newer
- Expects CentOS/RHEL 7.x host/s

##Nginx Configuration

These playbooks deploy a simple configuration of nginx and displays `Hello World Nginx & Swap memory Configured`
To use, copy the `hosts.example` file to `hosts` and edit the `hosts` inventory file to include the names or URLs of the servers
you want to deploy.

Then run the playbook, like this:

	`ansible-playbook -i hosts site-swap.yml`

The playbooks will configure Nginx and Swap memory on existing ec2 instances and any new instances configured through `terraform` When the run
is complete, entering `web1.dereck.com` on the browser should display `Hello World Nginx & Swap memory Configured`.

##Swap Memory
running playbook with `Hello World Nginx & Swap memory Configured` will configure swap memory of 1G on each ec2 instance defined in `hosts.example`.
A swap file of 1G will be created in `/mnt/swapfile ` and added to `/etc/fstab`.
The swap memory will be enabled by running `swapon /mnt/swapfile` and can be  checked with `free -m`

##Testing with Vagrant
Testing the playbook requires `vagrant`. The first step once you’ve installed Vagrant is to create a Vagrantfile and customize it to suit your needs.

example:
```
Vagrant.configure(2) do |config|

  config.vm.box ="bento/centos-7.3"
  config.vm.hostname = "web1.dereck.com"
  config.vm.network "forwarded_port", guest: 80, host: 80
  config.vm.network "private_network", ip: "192.168.10.21"
  config.vm.provision "shell", inline: <<-SHELL
       sed -i '$ a 192.168.10.22 web1.dereck.com' /etc/hosts           
    SHELL
  config.vm.provision "ansible" do |ansible|
    ansible.verbose = "v"
    ansible.playbook = "site-swap.yml"
  end
end
```

`vagrant up` This will start the VM, and run the provisioning playbook (on the first VM startup).

To re-run a playbook on an existing VM, just run: `vagrant provision`






