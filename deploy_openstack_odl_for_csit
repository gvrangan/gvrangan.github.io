# Deploy Openstack Using the Deploy in ODL Integration/CSIT Code

##  Setup
### 1node  Setup (1 Openstack Control Node)
Requires 4 VM instances
* 1 for robot execution
* 1 for Control Node 
* 2 Compute Nodes

### 3node Setup (3 Openstack Control Nodes for HA)
Requires 7 VM instances
* 1 for robot execution
* 3 Openstack Control Nodes 
* 1 HAProxy Node
* 2 Compute Nodes

### Setting for all VM
1. Ensure that one common sudo capable username/password can login to all VM's.

2. Ensure that SELINUX is disabled in all nodes.
   ```
   Please modify the file /etc/selinux/config and ensure the line
   SELINUX=disabled 
   ```
3. Ensure the instances can reach internet to download and install packages

4. If you have Proxy for internet, please configure the proxy details to yum.conf
   [REf:Add proxy settings to yum.conf](https://www.centos.org/docs/5/html/yum/sn-yum-proxy-server.html)

5. 
