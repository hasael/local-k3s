
# Install k3s through Ansible

Clone rancher k3s
```
https://github.com/k3s-io/k3s-ansible
```

Copy the sample to configure your own cluster, and change the hosts on host.ini
```
cd k3s-ansible
cp -R inventory/sample inventory/my-cluster
nano inventory/my-cluster/hosts.ini
```
Example
```
[master]
192.16.35.12

[node]
192.16.35.10:11

[k3s_cluster:children]
master
node
```

Also change the config params under /inventory/my-cluster/group_vars/all.yml
Set the default ansible user to 'pi'

provision the cluster by running
```bash
ansible-playbook site.yml -i inventory/my-cluster/hosts.ini
```

Setup kubectl by copying the config from the master node

```
scp pi@192.168.0.68:~/.kube/config ~/.kube/config
```

Verify with
```
kubectl get nodes
```

---
#raspberrypi #k8s 
[ansible github](https://github.com/k3s-io/k3s-ansible) 