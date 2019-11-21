# Azure Arc Demonstration
This is a brief demostration on how to use Azure Arc to monitor AWS Linux instances.

## Prerequisites
1. An Azure account (of course)
2. A test AWS account (can be a free trial one)
3. Some basic bash and command line skills

## What is Azure Arc?
The [documentation](https://azure.microsoft.com/en-gb/services/azure-arc/) gives the following summary:

>For customers who want to simplify complex and distributed environments across on-premises, edge and multicloud, Azure Arc enables >deployment of Azure services anywhere and extends Azure management to any infrastructure
![alt text](https://github.com/jometzg/arc-demo/blob/master/overview.png "Arc overview")

**Organize and govern across environments** - Get databases, Kubernetes clusters, and servers sprawling across on-premises, edge and multicloud environments under control by centrally organizing and governing from a single place.

**Manage Kubernetes Apps at scale** - Deploy and manage Kubernetes applications across environments using DevOps techniques. Ensure that applications are deployed and configured from source control consistently.

**Run data services anywhere** - Get automated patching, upgrades, security and scale on-demand across on-premises, edge and multicloud environments for your data estate.

## This demonstration Scope
This demonstration is only focussed around the first feature which is to allow governance across environments. 

## Steps
### Find the Arc feature in the portal
![alt text](https://github.com/jometzg/arc-demo/blob/master/find-arc.png "Find Arc feature")
![alt text](https://github.com/jometzg/arc-demo/blob/master/arc-summary.png "Arc summary")
![alt text](https://github.com/jometzg/arc-demo/blob/master/arc-list.png "Arc list")

### Add a new VM
![alt text](https://github.com/jometzg/arc-demo/blob/master/arc-add1.png "Add a VM")
![alt text](https://github.com/jometzg/arc-demo/blob/master/arc-add2.png "Add a VM")
![alt text](https://github.com/jometzg/arc-demo/blob/master/arc-add3.png "Add a VM")

This generates a shell script for the target Linux machine
```
 # Download the installation package
wget https://aka.ms/azcmagent -O ~/install_linux_azcmagent.sh

# Install the hybrid agent
bash ~/install_linux_azcmagent.sh

# Run connect command
azcmagent connect --resource-group "traget-resource-group" --tenant-id "YOUR-AZURE-AD-TENANT-ID" --location "target-location" --subscription-id "YOUR-SUBSCRIPTION-ID"
``
This will need to be run on the target Linux machine

### Provisioning a target VM
As Azure already knows about its VMs, this demonstration will use a different target. This can easily be done on a Windows PC, but it is more interesting to use another cloud vendor.
