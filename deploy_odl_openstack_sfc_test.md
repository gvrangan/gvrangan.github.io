# Deploy Openstack with ODL and Test Service Chaining

##  Setup Details
a. 1 Openstack Control Node + 1 Compute Nodes
b. Requires 2 VM instances with the below spec

Configuration | Requirement | Remarks
 --- | --- | ---
 OS | Centos 7 | Latest is Preferrable
 RAM |  > 8GB | 12 GB is preferrable
 Memory | > 50 GB | Needed to handle more logs
 
## Proxy Setup
If you are behind a corporate, you might have to configure proxy information for applications.

### yum Proxy
1. edit /etc/yum.conf and add proxy as follows

```
proxy=http://<PROXY_URL>:<PROXY_PORT>
proxy_username=<PROXY_USERNAME>
proxy_password=<PROXY_PASSWORD>
```

2. Add Proxy for wget/curl and other downloads aka (HTTP_PROXY)

```
edit /etc/environment and add entries like this
export http_proxy=http://<username>:<password>@<proxy_url>:<PROXY_Port>
export https_proxy=http://<username>:<password>@<proxy_url>:<PROXY_Port>
export noproxy="localhost,127.0.0.1,<CONTROl_NODE_IP>,<COMPUTE_NODE_IP>"
```
 
## Compile and Install OVS 2.9.2 with NSH
a. Requires OVS version 2.9.2 which is not available as rpm
b. It has to be compiled from source yay!!!
c. Install git and other pre-requisites

...Note: In Both the VM's
```
sudo yum install -y git 
sudo yum install -y centos-release yum-utils
K_VERSION=$(uname -r)
sudo yum install -y kernel-devel-${K_VER} kernel-headers-${K_VER} kernel-tools-${K_VER}
sudo yum groupinstall -y "Development Tools"
```
d. Get OVS source
```
git clone https://github.com/openvswitch/ovs.git
cd ovs
git checkout v2.9.2
```

e. Build OVS (Assuming you are in ovs directory)
```
sed -i "/BuildRequires:.*sphinx.*/d" rhel/openvswitch-fedora.spec.in
sed -e 's/@VERSION@/0.0.1/' rhel/openvswitch-fedora.spec.in > /tmp/ovs.spec
sed -e 's/@VERSION@/0.0.1/' rhel/openvswitch-kmod-fedora.spec.in > /tmp/ovs-kmod.spec
sed -e 's/@VERSION@/0.0.1/' rhel/openvswitch-dkms.spec.in > /tmp/ovs-dkms.spec
sudo yum-builddep /tmp/ovs.spec /tmp/ovs-kmod.spec /tmp/ovs-dkms.spec -y
./boot.sh
./configure
make rpm-fedora RPMBUILD_OPT="--without check"
rpmbuild -D "_topdir $(pwd)/rpm/rpmbuild" -bb --without check rhel/openvswitch-dkms.spec
```

f. Install OpenVswitch
```
cd rpm/rpmbuild/RPMS/
sudo yum install -y epel-release yum-plugin-versionlock
sudo yum install -y x86_64/openvswitch-2.9.2-1.el7.x86_64.rpm x86_64/openvswitch-dkms-2.9.2-1.el7.x86_64.rpm noarch/openvswitch-selinux-policy-2.9.2-1.el7.noarch.rpm
sudo systemctl start openvswitch 
...Note: Ensure there are no errors.
sudo yum versionlock openvswitch*
```


