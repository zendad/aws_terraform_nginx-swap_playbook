## Simple Nginx and Swap Memory configuration on EC2 instances using ansible

- Requires Ansible 1.2 or newer
- Expects CentOS/RHEL 7.x host/s

##Nginx Configuration

These playbooks deploy a simple configuration of nginx and displays `Hello World Nginx & Swap memory Configured`
To use, copy the `hosts` file to `hosts` and edit the `hosts` inventory file to include the names or URLs of the servers
you want to deploy.

Then run the playbook, like this:

	`ansible-playbook -i hosts site-swap.yml`

The playbooks will configure Nginx and Swap memory on existing ec2 instances and any new instances configured through `terraform` When the run
is complete, entering `web1.dereck.com` on the browser should display `Hello World Nginx & Swap memory Configured`.

##Swap Memory
running playbook with `Hello World Nginx & Swap memory Configured` will configure swap memory of 1G on each ec2 instance defined in `hosts`.
A swap file of 1G will be created in `/swapfile ` and added to `/etc/fstab`.
The swap memory will be enabled by running `swapon -a` and can be  checked with `free -m`

##Testing with Vagrant
The playbook can also be tested with `vagrant`. The first step once you’ve installed Vagrant is to create a Vagrantfile and customize it to suit your needs.

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

## Running on localhost
The playbook can be run as localhost where OS Centos 7.* is running with the results like below:

```
aws_terraform_nginx-swap_playbook]# ansible-playbook site-swap.yml 

PLAY [Install Nginx and Configure swap space] **********************************

TASK [setup] *******************************************************************
ok: [localhost]

TASK [nginx : Install nginx] ***************************************************
changed: [localhost]

TASK [nginx : Creates directory] ***********************************************
ok: [localhost]

TASK [nginx : Remove default site] *********************************************
changed: [localhost] => (item=/usr/share/nginx/html)
ok: [localhost] => (item=/etc/nginx/sites-enabled/default)
ok: [localhost] => (item=/etc/nginx/sites-available/default)

TASK [nginx : Make sure the sites-available, sites-enabled and conf.d directories exist] ***
changed: [localhost] => (item=sites-available)
changed: [localhost] => (item=sites-enabled)
changed: [localhost] => (item=conf.d)

TASK [nginx : Make sure the nginx configuration is updated] ********************
changed: [localhost]

TASK [nginx : Set up the nginx default site configuration file] ****************
changed: [localhost]

TASK [nginx : Enable the nginx default site] ***********************************
changed: [localhost]

TASK [nginx : Copy nginx configuration for hello world] ************************
ok: [localhost]

TASK [nginx : insert firewalld rule for nginx] *********************************
ok: [localhost]

TASK [nginx : http service state] **********************************************
changed: [localhost]

TASK [swapspace : Set swap_file variable] **************************************
ok: [localhost]

TASK [swapspace : Check if swap file exists] ***********************************
ok: [localhost]

TASK [swapspace : Create swap file] ********************************************
changed: [localhost]

TASK [swapspace : Change swap file permissions] ********************************
changed: [localhost]

TASK [swapspace : Format swap file] ********************************************
changed: [localhost]

TASK [swapspace : Write swap entry in fstab] ***********************************
changed: [localhost]

TASK [swapspace : Turn on swap] ************************************************
changed: [localhost]

TASK [swapspace : Set swappiness] **********************************************
changed: [localhost]

RUNNING HANDLER [nginx : restart nginx] ****************************************
changed: [localhost]

PLAY RECAP *********************************************************************
localhost                  : ok=20   changed=14   unreachable=0    failed=0   

```






