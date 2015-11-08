# systemd conference 2015 hacking #

Some hacking around systemd I did basing on [systemd 2015 conference](https://systemd.events/)

This repository consists mainly of ansible-playbooks used to spin some containers using different containers 
technologies ([docker](https://docker.io), [runc](http://runc.io/), 
[systemd-nspawn](http://www.freedesktop.org/software/systemd/man/systemd-nspawn.html), 
[rkt](https://github.com/coreos/rkt)). 

## Requirements: ##

- F22 minimal installation on VM w/installed
- ssh pubkey exported to **root** account of this VM (```ssh-copy-id root@<host_ip_addr>```)
- replace your VM's IP addr in **inventory/all** file
- installed **python** libs and **libselinux-python** on this VM 
  (```dnf install -y python libselinux-python python2-dnf```)
- Whole setup bases on static addresses on VMs:
    - 192.168.122.13 for vanilla-installation (minimal) of Fedora 22
    - 192.168.122.14 for docker VM
    - 192.168.122.15 for runc VM
    - 192.168.122.16 for systemd-nspawn VM
    - 192.168.122.17 for rkt VM
    - 192.168.122.12 for docker registry
- I also used my local Docker registry (as I do some work in areas where connectivity is rather poor, so it's easier
  this way). This registry might be created with **plays/docker_registry_deployer.yml**

## What we can do here? ##

- Prepare environment (like preparing vanila F22 installation; creating ansible user, configuring sudo etc): 
  ```ansible-playbook plays/prepare-env.yml```
- Prepare networking (switch from NetworkManager to systemd-networkd and systemd-resolved): 
  ```ansible-playbook plays/prepare-networking.yml```
- Create and deploy Docker registry:
  ```ansible-playbook plays/docker_registry_deployer.yml```
  
## Still todo ##

- Create basic docker images & containers for GoCD (server & agent). Use ansible internally to deploy apps.
- Pack above containers FS and create runc containers from it;
- Create systemd-nspawn containers and deploy GoCD using prepared earlier ansible-playbooks
- Run same apps on rkt
- maybe try to set this up using ovs L2 bridge?
- Use unprivileged containers