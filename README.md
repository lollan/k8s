# k8s

# Disable firewalld on all nodes
ansible all -a "systemctl disable firewalld" -u root -i inventory/mycluster/hosts.ini 

# Test etcd health
/usr/local/bin/etcdctl --endpoints=https://10.170.5.81:2379,https://10.170.5.82:2379,https://10.170.5.83:2379 cluster-health | grep -q 'cluster is healthy'

# Install dependencies for NLT
yum install -y device-mapper-multipath sg3_utils iscsi-initiator-utils

# Install NLT in silent mode
./nlt_installer_2.3.1.8 --silent-mode --accept-eula --ncm --docker --flexvolume

# Add Nimble group
nltadm --group --add --ip-address 10.64.24.26 --username admin --password admin
