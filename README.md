# k8s

# Pre-requisites

# Disable firewalld on all nodes
ansible all -a "systemctl disable firewalld" -u root -i inventory/mycluster/hosts.ini 

# Test etcd health
/usr/local/bin/etcdctl --endpoints=https://10.170.5.81:2379,https://10.170.5.82:2379,https://10.170.5.83:2379 cluster-health | grep -q 'cluster is healthy'
