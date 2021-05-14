## Ansible playbook for setup etcd cluster

first of all you should configure **inventory.ini** file for your etcd servers in **etcd** group like this :

```
[etcd]
etcd1 ansible_host=ip_etcd_server_1 ansible_user=root ansible_ssh_pass=pass
etcd2 ansible_host=ip_etcd_server_2 ansible_user=root ansible_ssh_pass=pass
etcd3 ansible_host=ip_etcd_server_3 ansible_user=root ansible_ssh_pass=pass
...
```

then make sure the name of **NIC** in target server be **eth0**, if it's different , change it in this directory **roles/create-cluster/templates/etcd.conf.yaml.j2**.


> by default , this playbook use **v3.4.10** etcd version 

you can install any version of etcd , if you need another version , just change this
**ETCD_VER** variable in this directory **roles/install-etcd/vars/main.yml**

```
ETCD_VER: v3.4.10
```

in the end , run this commmad for install etcd cluster :):
```
ansible-playbook -i inventory.ini etcd-cluster.yml
```

**i am gonna add tls soon as possible , don't worry about it ;)**
