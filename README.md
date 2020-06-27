# Play with Ansible using Vagrant

## Setup your Vagrantfile
The file is provided above. 
Make sure you have **Vagrant Installed on your System**

Head over to this website to know more [Download Vagrant](https://www.vagrantup.com/downloads.html)

## Get into your newly created vagrant virtual machines and setup your ansible.
> This Vagrant file is using centos/7 image

```
sudo yum install epel-release
```
```
sudo yum install net-tools
```
```
sudo yum update -y
```
### Installing Ansible on Centos/7
```
sudo yum install ansible
```

> This is our Setup

We've created 2 VMs.
1. MainNode
2. WorkerNode

MainNode - This is our MainNode which will be used to push the configurations onto our **WorkerNode**

> Since, Ansible uses the push configuration and hence it does not require any agent on the target machine.

## Network Setup 

We've provided a private network to both the VMs via which both will communicate.

1. MainNode - 192.168.33.10
2. WorkerNode - 192.168.33.11

## SSH Setup

Ansible requires ssh key authentication to communicate with target hosts securely. To make that possible, we need to setup a public key onto a target hosts, we'll do it by generating a new public key on our **MainNode** and will copy it to our **WorkerNode**

1. Generate ssh key
```
ssh-keygen
```
Hit Enter without typing anything until the key is generated. You can provide the passphrase for more security (optional)

2. Copy it Over
```
ssh-copy-id -i /home/vagrant/.ssh/id_rsa.pub vagrant@<vm ipv4>
```
While using this we came across with an error **Permission Denied**

> To resolve this issue, we've to make some changes to the **Target Hosts** sshd_config file.

3. Open up your Target VM (which in our case is WorkerNode)
Head over to ssh config file.
```
sudo vim /etc/ssh/sshd_config
```
Do 2 things here
- Allow **PasswordAuthentication** to **yes**
- Allow **PubkeyAuthentication** to **yes**

4. Switch over to your MainNode run step 2 again, you'll get prompted with (yes/no) type **yes** then you'll be asked for a password **The default password for vagrant is vagrant**
Type **vagrant** when prompted for a password hit enter and now your ssh-key should have been copied sucessfully.

# Let's run our first Ansible Command

Ansible uses push configuration so target host does not require any angent to be installed like chef or puppet. 

Ansible needs to know with whom it is interacting with. For this, ansible have something called Hosts or Inventory where it stores all the devices which it'll be interacting with.
Ansible does need some kind of address, right? Yes, we've given a private ip address to our VM.

Now, head over to Hosts file where it stores information about it's target hosts.

```
sudo vim /etc/ansible/hosts
```
Go to the end of file and type your WorkerNode Ip address. Now, this can be done in 2 ways.
- Make a Group
```
[worker]
<WorkerNode Ip Address>
```

In ansible, it can also store addresses under a group name and call them with a single point of contact that is group name. 

Imagine if you've multiple VMs to manage then you can store multiple VMs under different groups to make things easy. Use [square brackets] to create a group.

- Just type your Target's Ip as it is at the end of the file.

This is a way to define your target hosts but if you're dealing with multiple VMs then grouping them would be a better solution.


2. Let's run our Ad-hoc command.

```
ansible -m ping worker
```
**Breakdown**

`-m:` This indicates we're using ansible modules. In ansible world, modules are everything. These are prebuilt configurations that are used with some user parameters that is required by the modules to do the work.

`ping:` This is the module name. **-m** is flag to tell ansible that we're passing a module.

`worker:` This is the group name from our **hosts** file. We're telling ansible that we need to perform module ping onto our workerNode.

> The output will give you some 'ping: pong'. which means you've successfully communicated with your target node.

Now, Install some packages to Target Node.
```
ansible -m yum -a "name=nano state=latest" worker
```

`yum:` Module name.
`-a:` Telling ansible that we're passing an argument.
Yum package requires 2 things minimum.
- name and state
`name:` what package you want to install
`state:` what version or state you want your package to be.

That's it. Learn some more about from [Ansible Docs](https://docs.ansible.com/ansible/latest/index.html)

Ansible has one of the best documentation available on the Internet.

