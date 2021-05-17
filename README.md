# Ansible playbook for setup etcd cluster

## Get Started :

first of all you should configure **inventory.ini** file for your etcd servers in **etcd** group like this :

```
[etcd]
etcd1 ansible_host=ip_etcd_server_1 ansible_user=root ansible_ssh_pass=pass
etcd2 ansible_host=ip_etcd_server_2 ansible_user=root ansible_ssh_pass=pass
etcd3 ansible_host=ip_etcd_server_3 ansible_user=root ansible_ssh_pass=pass
...
```

then make sure the name of **NIC** in target server be **eth0**, if it's different , change it in this directory **etcd/roles/create-cluster-tls/templates/etcd.conf.yaml.j2**.


> by default , this playbook use **v3.4.10** etcd version 

you can install any version of etcd , if you need another version , just change this
**ETCD_VER** variable in this directory **etcd/group_vars/etcd.yml**

```
ETCD_VER: v3.4.10
```

## Install etcd cluster with tls

by default etcd cluster install with tls , just run this command :

```
ansible-playbook -i inventory.ini etcd-manager.yml
```


## Install etcd cluster without tls :

by default etcd cluster install with tls but for install etcd cluster without tls you should change two files like this:

etcd-manager.yml :
```
---
#- import_playbook: private-ip-etcd/private-ip-etcd.yml
#- import_playbook: generate-ca-tls/tls.yml
- import_playbook: etcd/etcd-cluster.yml
```
and

etcd/roles/etcd-cluster.yml :

```
---
- name: etcd cluster
  strategy: linear
  hosts: etcd
  roles:
    - etcd-install
    - etcd-cluster
    # - etcd-cluster-tls
    # - etcd-destroy
```

and then run

```
ansible-playbook -i inventory.ini etcd-manager.yml
```

## Destory etcd cluster

for remove etcd cluster already has been provisioned , you should change two files like this:

etcd-manager.yml :
```
---
#- import_playbook: private-ip-etcd/private-ip-etcd.yml
#- import_playbook: generate-ca-tls/tls.yml
- import_playbook: etcd/etcd-cluster.yml
```
and

etcd/roles/etcd-cluster.yml :

```
---
- name: etcd cluster
  strategy: linear
  hosts: etcd
  roles:
    # - etcd-install
    # - etcd-cluster
    # - etcd-cluster-tls
    - etcd-destroy
```

and then run

```
ansible-playbook -i inventory.ini etcd-manager.yml
```

### Check etcd cluster member list

> for get information about non tls etcd cluster, just run command without certification flags

ssh in one of your cluster node and run this command for get information about etcd cluster:

```
  etcdctl \
    --endpoints=https://127.0.0.1:2379 \
    --cacert=/etc/etcd/ssl/ca.crt \
    --cert=/etc/etcd/ssl/server.crt \
    --key=/etc/etcd/ssl/server.key \
  member list
```

it's gonna be like this:

```
7a469b777ccae99, started, etcd1, https://10.12.90.72:2380, https://10.12.90.72:2379, false
8844f9e4d13ad0d7, started, etcd3, https://10.12.90.163:2380, https://10.12.90.163:2379, false
d86274b6fdbb0f6e, started, etcd2, https://10.12.90.33:2380, https://10.12.90.33:2379, false
```
