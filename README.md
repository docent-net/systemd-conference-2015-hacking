# systemd conference 2015 hacking #

Some hacking around systemd I did basing on [systemd 2015 conference](https://systemd.events/)

This repository consists mainly of ansible-playbooks used to spin some containers using different containers 
technologies ([docker](https://docker.io), [runc](http://runc.io/), 
[systemd-nspawn](http://www.freedesktop.org/software/systemd/man/systemd-nspawn.html), 
[rkt](https://github.com/coreos/rkt)). 

## Requirements: ##

- F22 minimal installation on VM w/installed
- ssh pubkey exported to **root** account of this VM (```ssh-copy-id root@<host_ip_addr>```)
- replace your VM's IP addr in **inventory** file
- installed **python** libs and **libselinux-python** on this VM 
  (```dnf install -y python libselinux-python python2-dnf```)

## What we can do here? ##

- Prepare environment (like preapring vanila F22 installation; creating ansible user, configuring sudo etc): 
  ```ansible-playbook plays/prepare-env.yml```
- Prepare networking (switch from NetworkManager to systemd-networkd and systemd-resolved, install openvswitch and 
  configure L2 bridge): 
  ```ansible-playbook plays/prepare-networking.yml```
  
## Still todo ##

- Create openvswitch bridge
- Prepare separate VMs for runc, systemd-nspawn and rkt
- Create basic docker images & containers for GoCD (server & agent). Use ansible internally to deploy apps.
- Switch docker to openvswitch bridge and repeat above
- Pack above containers FS and create runc containers from it; use OVS bridge for networking
- Create systemd-nspawn containers and deploy GoCD using prepared earlier ansible-playbooks
- Run same apps on rkt