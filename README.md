# k8s-in-vagrant-with-ansible
Provision local virtual machine infrastructure using vagrant and configure a multi-node Kubernetes cluster using ansible.

## Requirements
* Virtualbox installed: https://www.virtualbox.org/wiki/Downloads
* Hashcorp Vagrant installed: https://www.vagrantup.com/downloads

### Provision instructions

1. Download and install **Vagrant** from https://www.vagrantup.com/downloads

2. Clone the git repo
```
git clone https://github.com/devopshouse/k8s-in-vagrant-with-ansible.git
```

3.  Enter repo dir
```
cd k8s-in-vagrant-with-ansible
```

4. Adjust network configuration in Vagrantfile
```
- NETWORK_PREFIX: network prefix for the node network, change it only if 172.19.69 conflits with your local network"
- NODES: number of desired worker nodes
```

5. Provision the required infrastructure with Vagrant
```
vagrant up
```

### Usage
1. SSH into k8s master node
```
vagrant ssh master
```

3. Use kubectl commands
```
kubectl get nodes
```

Sample output
```
NAME     STATUS   ROLES                  AGE     VERSION
master   Ready    control-plane,master   9m55s   v1.22.2
node1    Ready    <none>                 6m53s   v1.22.2
node2    Ready    <none>                 4m7s    v1.22.2
```