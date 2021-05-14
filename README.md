## This playbook for make cluster of any number of etcd (key/value store)

first of all you should setup **inventory.ini** file for target servers in **etcd** group like this :

```
[etcd]
etcd1 ansible_host=ip_etcd_server_1 ansible_user=root ansible_ssh_pass=pass
etcd2 ansible_host=ip_etcd_server_2 ansible_user=root ansible_ssh_pass=pass
etcd3 ansible_host=ip_etcd_server_3 ansible_user=root ansible_ssh_pass=pass
...
```

then make sure your target server network crad be **eth0**, if it's different , change it in this dir **roles/create-cluster/templates/etcd.conf.yaml.j2**.


> by default this playbook use **v3.4.10** version 

you can install any version of etcd cluster , just change environment variables in this dir *roles/install-etcd/vars/main.yml***

```
ETCD_VER: v3.4.10
```

in the end , run this commmad for install etcd cluster :):
```
ansible-playbook -i inventory.ini etcd-cluster.yml
```

**i am gonna add tls soon as possible , don't worry about it ;)**
