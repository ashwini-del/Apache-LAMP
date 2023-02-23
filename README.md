vim /etc/ssh/sshd_config
vim .ssh/authorized_keys
systemctl restart sshd


## Install LAMP Stack on Debian / Redhat with Ansible
sudo ansible -m ping -i hosts web
sudo ansible-playbook -i hosts main.yml 
sudo ansible-playbook -i hosts main.yml --extra-vars "install_firewall=no"

do we need to install firewall on ubuntu 
for apache2

In this Task I am going to setup multiple client servers quickly with LAMP . The client servers are like  Debian and Redhat . For setting up LAMP I have used ansible roles. Playbook is made as such it is easily customizable and deployable on mutiple clients(Debian and Redhat) with task specific to each client will be executed using import_tasks module and when condition. 

do we need to install firewall on linux 
for apache2

Yes, it is highly recommended to install a firewall on Linux servers that run Apache2. This provides an extra layer of security and helps prevent unauthorized access to the server. A firewall can help block malicious traffic, control incoming and outgoing traffic, and allow only the necessary services and protocols to be accessible.

There are several firewall options available for Linux, including iptables, ufw, and firewalld. These firewalls can be easily installed and configured to protect your Apache2 server.

## Pre-requisites

- Used Ansible Master server as Ubuntu and Client servers on Amazon Linux
- Ansible Master server is installed with Ansible2.
## Resources Used
- Amazon Linux 2 
- Ubuntu
- Mysql
- PHP
- Ansible2
## Included roles
- Devops Internal Ansible - Setup lamp on all  client systems.

## Actions Performed
- Installs Apache, PHP, mysql
- Starts and enables httpd, mysql 
- Setup virtualhost in both client systems
- Copies test file content to documentroot
- Handler used to notify and restart httpd service
## Role Hirarachy
```
/lamp-linux/files = include following files
apache.conf.j2, info.php.j2, test.html , test.php.
```
```
/lamp-linux/handler = it include main.yml file it includes code for restart services
```
```
/lamp-linux/task = it include following files main.yml file is targeting to other file as per OS family
 Debian.yml  RedHat.yml  main.yml
```
```
/lamp-linux/templates - it includes following templates.
 apache.conf.j2 , httpd.conf.tmpl , info.php.j2 ,vhost.conf.tmpl
```
```
/lamp-linux/vars = this include following files this file contain variable accourding to OS family
Debian.yml and RedHat.yml
```
```
/lamp-linux/test = it inludes inventory file and main file which calling to role
```
## How to use?

To begin with this project, first ensure that ansible is installed on your server.

First create your Inventory file. You can work entirely in this repository. Name your Inventory file and setup all required connections there.

Example Inventory file:
```
[example-host]
xxx.xxx.xxx.xxx

[example-host]
xxx.xxx.xxx.xxx 
```

Next setup your playbook. Here I have setup my playbook main-role.yml and included roles for Devops Internal Ansible  on both the clients. For the execution of the playbook, first check the connection status to your client servser via:

```sh
ansible  -m ping [server ip]
```
Once you have established the connection then check for any syntax error in the playbook:

```sh
ansible-playbook  YOUR_PLAYBOOK_FILE.yml --syntax-check
```

if all checks have pass then you are good to execute the ansible-playbook:

```sh
ansible-playbook  YOUR_PLAYBOOK_FILE.yml
```
After Ansible deployment, login to the client servers and check if everything is as good as it was planned.

