# Ansible scripts for a full HDP install

First provision machines for your cluster. You should expand the disks and do any other alterations with fdisk now if that has not already been done. For the latest HDP 8G RAM is preferable. Also, distribute SSH keys to enable passwordless login.

Next, on one of the machines, install Ansible (either from their git repository or do `yum install epel-release` and `yum install ansible` . If you run from the git repo you need to do `source ansible/hacking/env-setup` to prepare your environment.

If you are reading this, you should have unpacked the files in this TGZ. Go to the file `inventory/all` and enter the details for your machines. 

## Network details
Please note: at present there are two roles to distribute `/etc/hosts` files for your cluster machines. This allows your cluster nodes to find each other independent of a DNS which you may or may not have control of. You must choose whether to run `hostfile` or `hostfilefromip` in the file [playbooks/prereqs.yml](https://github.com/derwinmcgeary/hdp-ansible/blob/master/playbooks/prereqs.yml) depending on the following.

If your machines already have hostnames defined, you can simply list the hostnames and give them the role `hostfile`. If they do *not*, use `hostfilefromip` and IP addresses and hostnames of your machines should be specified in the format (if you want machine 128.2.2.67 to get the hostname "ambari"),

```
128.2.2.67 hn=ambari
```

All of the machines should be in the section `[nodes]` and you must pick one to be `[ambari]`. The other sections are just ignored for now.

Then run `ansible-playbook -i inventory/all playbooks/hdp-install.yml` and everything will be installed. Once it's finished, login to Ambari to select which services you want on your cluster. The nodes are already manually registered.
