**Procedure to Execute Openstack-ODL CSIT in local Environment**

# CSIT Environment

## Details  
  - An environment in which the CSIT automated tests can be executed
  - Opendaylight Jenkins Infrastructure uses the service from Rackspace who in turn use Openstack
     to create slave instances.
  - A Verify/Merge job will create CentOs slaves with maven and Java installed for buinding Code
  - A CSIT test job will create a  CentOS slave with Java for executing ODL and other slaves for 
	other components like Mininet/Openstack etc.
  - The test team creates scripts to deploy various components to the slaves for testing with Opendaylight.  
  
## System Requirements

### ODL 1 node Test

 - 5 VMs
   - 1  Robot VM
   - 1  VM with ODL running
   - 3  Openstack Nodes (1 Control + 2 Compute)
   
### ODL 3node test

 - 8 VM's (3 for ODL, 1 for HA Proxy, 3 for Openstack Nodes) + 1 for Executing Robot Tests
   - 1  Robot VM
   - 3  VM's with ODL running and configured for Clustering
   - 1  HA Proxy VM
   - 3  Openstack Nodes (1 Control + 2 Computes)
  
## Pre Requisites

### Install ODL and configure karaf features

	- In the ODL VM, Please download the required ODL distribution
	- Untar the distro
	- Add the feature "odl-netvirt-openstack" to the featuresBoot in the file etc/org.apache.karaf.features.cfg.
	- To enable other netvirt features like conntract for security group and Conntrack for SNAT please use the scripts 
	   - [set snat mode](https://github.com/opendaylight/integration-test/blob/master/csit/scripts/set_snat_mode.sh)
	   - [set sg implementation mode](https://github.com/opendaylight/integration-test/blob/master/csit/scripts/set_sg_mode.sh)
	   - [set odl as dhcp server](https://github.com/opendaylight/integration-test/blob/master/csit/scripts/set_dhcp_mode.sh)		
	- Start ODL using bin/start in the distro

#### Clustering
   - Execute the script bin/configure_cluster.sh in every ODL node to configure the ODL nodes as cluster
   - [More details](http://docs.opendaylight.org/en/latest/getting-started-guide/common-features/clustering.html)

### Install Openstack 

        -  Openstack can be installed either using devstack or any other method
	-  You can refer to this [ODL-Openstack Integration](https://docs.opendaylight.org/en/stable-carbon/submodules/netvirt/docs/openstack-guide/index.html)  for installing the Openstack Nodes.
	- You can also refer to the [script based on install guide](https://gist.github.com/gvrangan/440afc433b6029244b3eb68e877a6456) to install control node
	-  and [script to deploy compute node](https://gist.github.com/gvrangan/ecb4798267d7e8c83d08829d5596d75d)
	
_Note: The Installation of Openstack is beyond the scope_

### Test the Setup (Optional)

   - Perform some operations like creating Network and ensure flow entries are getting installed
   - Create slave instances and ensuring conenctivity could be more time saving instead of debugging later.

### Setup Robot VM

    -  Using pip install "pip install robotframework" 
 
 
## Executing Tests

