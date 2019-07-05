# openstack.rocky

### 10
# SSH 
ssh-copy-id -i ./id_rsa.pub root@10.211.55.22

### 20
SELINUX
/etc/selinux/conf/  -> disabled

### 30
vi /etc/profile
/etc/environment

echo "export PATH=/usr/local/bin:$PATH" >> /etc/profile
echo "LANG=en_US.utf-8" >> /etc/environment
echo "LC_ALL=en_US.utf-8" >> /etc/environment

systemctl stop postfix firewalld NetworkManager
systemctl disable postfix firewalld NetworkManager
systemctl mask NetworkManager
yum remove postfix NetworkManager NetworkManager-libnm -y


### 70
time yum upgrade -y
yum install https://rdoproject.org/repos/openstack-rocky/rdo-release-rocky.rpm -y
yum install tcpdump tmux git ntp ntpdate openssh-server python-devel sudo '@Development Tools' -y

### 80
git clone https://git.openstack.org/openstack/openstack-ansible /opt/openstack-ansible

or
git clone -b master https://github.com/openstack/openstack-ansible.git /opt/openstack-ansible

### 90
cd /opt/openstack-ansible
git branch -a

git checkout stable/rocky
time ./scripts/bootstrap-ansible.sh

which ansible

### 110

export SCENARIO='aio_heat'
time ./scripts/bootstrap-aio.sh


### 125
vi /etc/openstack_deploy/user_variables.yml
heat_heat_conf_overrides:

 clients_keystone:
  insecure: True

cd /opt/openstack-ansible/playbooks/
echo "time openstack-ansible /opt/openstack-ansible/playbooks/setup-hosts.yml" > o1.sh
echo "time openstack-ansible /opt/openstack-ansible/playbooks/setup-infrastructure.yml" > o2.sh
echo "time openstack-ansible /opt/openstack-ansible/playbooks/setup-openstack.yml" > o3.sh


### 126

sh o1.sh
sh o2.sh
sh o3.sh
