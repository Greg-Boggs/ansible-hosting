## Automated Ansible Hosting on Digital Ocean

Ansible Hosting is an Ansible-based script that purchases a Droplet from Digital Ocean and automatically installs a fully-
configured web server on the new Droplet. To run this script, you need anAnsible control server which could either
be your laptop or a Droplet on Digital Ocean. Using this script requires a Digital Ocean account, and it will cost money
because it creates a new Droplet for you.

This script is the foundation for open-source web hosting. This script is made possible by the work of many
amazing folks and it's given away in the same spirit. Use it if you like. If you do, please consider sharing
your code back to the project. A link to http://www.greboggs.com would be nice too. But, that is, of course, optional.

### Requirements

This script requires a paid Digital Ocean account with a Digital Ocean [Personal Access Token](https://www.digitalocean.com/community/tutorials/how-to-use-the-digitalocean-api-v2).

### Using this Script

Once you have setup a control server, this script allows you to automatically create and remove fully-configured web servers.

#### Create a Control Server

This guide will walk you through [installing an Ansible Control Server](https://www.digitalocean.com/community/tutorials/how-to-use-the-digitalocean-api-v2-with-ansible-2-0-on-ubuntu-16-04). 

Set up a [Ubuntu Server](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04).
Enable [automatic security updates](https://help.ubuntu.com/community/AutomaticSecurityUpdates). I like the unattended-upgrades 
 method.
 
#### Add Your Digital Ocean API Key

The script requires a Digital Ocean API Key to create a Droplet. If you remove it after testing, you will incur only a
small fee. If you don't want to be billed, you 

    echo 'export DO_API_KEY=<YOURKEY>' >> ~/.bashrc
    source ~/.bashrc
    
#### Install Ansible

    sudo apt-add-repository ppa:ansible/ansible
    sudo apt-get update
    sudo apt-get install ansible

#### Configure Ansible

    sudo apt-get install python-pip
    sudo pip install dopy
    git clone git@github.com:Greg-Boggs/ansible-hosting.git
    cd ansible-hosting
    
#### Add Ansible Galaxy Roles

This script relies on Ansible Galaxy Roles on your control server. So, before running the script, install them!

    sudo ansible-galaxy install geerlingguy.firewall
    sudo ansible-galaxy install geerlingguy.nginx
    sudo ansible-galaxy install geerlingguy.mysql
    sudo ansible-galaxy install geerlingguy.php
    
#### Run the Script

Now, run the script and you have a fully configured web server ready to go!

    ansible-playbook digitalocean.yml

#### Remove the new Droplet

This command will delete your new server and prevent you from incuring any charges if you are just testing this script.

    ansible-playbook remove.yml

#### Future Plans

I plan to add support for Linode and Amazon in the future. I chose Digital Ocean for the starting point because they currently
have better written Ansible documentation. 