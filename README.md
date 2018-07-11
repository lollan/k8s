# k8s

# Disable firewalld on all nodes
ansible all -a "systemctl disable firewalld" -u root -i inventory/mycluster/hosts.ini 

# Create inventory file
declare -a IPS=(10.170.5.81 10.170.5.82 10.170.5.83 10.170.5.84 10.170.5.9110.170.5.92 10.170.5.93 10.170.5.94 10.170.5.95 10.170.5.96 10.170.5.97 10.170.5.98 10.170.5.99 10.170.5.100)
CONFIG_FILE=inventory/mycluster/hosts.ini python3 contrib/inventory_builder/inventory.py ${IPS[@]}

# Run kubespray deployment playbook
ansible-playbook -i inventory/mycluster/hosts.ini cluster.yml

# Test etcd health
/usr/local/bin/etcdctl --endpoints=https://10.170.5.81:2379,https://10.170.5.82:2379,https://10.170.5.83:2379 cluster-health | grep -q 'cluster is healthy'

# Test cluster health
kubectl get cs
NAME                 STATUS    MESSAGE              ERROR
scheduler            Healthy   ok                   
controller-manager   Healthy   ok                   
etcd-2               Healthy   {"health": "true"}   
etcd-1               Healthy   {"health": "true"}   
etcd-0               Healthy   {"health": "true"}   

# Install dependencies for NLT
yum install -y device-mapper-multipath sg3_utils iscsi-initiator-utils

# Install NLT in silent mode
./nlt_installer_2.3.1.8 --silent-mode --accept-eula --ncm --docker --flexvolume

# Add Nimble group
nltadm --group --add --ip-address 10.64.24.26 --username admin --password admin
