# Deploy Openstack Using the Deploy in ODL Integration/CSIT Code

##  Setup Info
### 1node  Setup (1 Openstack Control Node)
Requires 4 VM instances
* 1 for robot execution
* 1 for Control Node 
* 2 Compute Nodes

#### 1node setup Platform Requirements
* All Nodes should have this hardware support as minimum
     - 4 VCPU
     - 8192 MB RAM
  
### 3node Setup (3 Openstack Control Nodes for HA)
Requires 7 VM instances
* 1 for robot execution
* 3 Openstack Control Nodes 
* 1 HAProxy Node
* 2 Compute Nodes

#### 3node setup Platform Requirements
* All Control Nodes to have 
     - 4 VCPU
     - 12288 MB RAM
     
* All Other Nodes should have this hardware support as minimum
     - 4 VCPU
     - 8192 MB RAM

### Common Settings for all VM
* All VM's will run the latest CentOS (7) distro
  At the time of creating this text, latest can be found here
  [CentOS 7.4](http://mirrors.kernel.org/centos/7.4.1708/)
  
* Ensure that one common sudo capable username/password can login to all VM's. i.e login with the same username/password combination
  is possible in all VM's, preferrably have the same root password in all VM's

* Ensure that SELINUX is disabled in all nodes.
   ```
   Edit the file /etc/selinux/config and ensure the line
   SELINUX=disabled 
   ```
* Ensure the instances can reach internet to download and install packages

* If you have Proxy for internet, please configure the proxy details to yum.conf
   [Ref:Add proxy settings to yum.conf](https://www.centos.org/docs/5/html/yum/sn-yum-proxy-server.html)


##  Topology

### 1node Topology

        -------------
	|	     |---------------------|		|
	|  Robot VM  |---------|           |            |
	|            |---------|-----------|------------|
	-------------          |           |		|
	-------------          |           |  		|
	|	     |---------|-----------|		|
	| Controller |---------|           |		|
	|            |---------|-----------|------------|
	 -------------         |           |		|
	-------------          |           |		|
	|	     |---------|-----------|		|
	|  Compute1  |---------|           |		|
	|            |---------|-----------|------------|
	-------------          |           |		|
	-------------          |           |		|
	|	     |---------|-----------|		|
	|  Compute2  |---------|           |		|
	|            |---------|-----------|------------|
	-------------          |           |		|
	                       |           |		|
	                       |           |		|
      			     Data      External      External
                                      Network1	   Network2
                                      
                                 
 ### 3node Topology
 
        -------------
	|	     |---------------------|	        |
	|  Robot VM  |---------|           |            |
	|            |---------|-----------|------------|
	-------------          |           |		|
	-------------          |           |  		|
	|	     |---------|-----------|		|
	|Controller1 |---------|           |		|
	|            |---------|-----------|------------|
	-------------          |           |		|
	-------------          |           |		|
	|	     |---------|-----------|		|
	|Controller2 |---------|           |		|
	|            |---------|-----------|------------|
	-------------          |           |		|
	-------------          |           |		|
	|	     |---------|-----------|	        |
	|Controller3 |---------|           |		|
	|            |---------|-----------|------------|
	-------------          |           |		|
	-------------          |           |		|
	|	     |         |           |		|
	|  haproxy   |---------|           |		|
	|            |         |           |            |
	-------------          |           |	        |  
	-------------          |           |		|
	|	     |---------|-----------|		|
	|  Compute1  |---------|           |		|
	|            |---------|-----------|------------|
	-------------          |           |		|  
                               |           |		|
        -------------          |           |		|
	|	     |---------|-----------|		|
	|  Compute2  |---------|           |		|
	|            |---------|-----------|------------|
	-------------          |           |		|  
                               |           |		|   
                               |           |		|  
      			   Data      External      External
                                      Network1	   Network2
 
						   


