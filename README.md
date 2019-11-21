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
Type in "arc" in the search box at the top centre of the Azure portal.
![alt text](https://github.com/jometzg/arc-demo/blob/master/find-arc.png "Find Arc feature")
Selecting this will bring up a useful summary.
![alt text](https://github.com/jometzg/arc-demo/blob/master/arc-summary.png "Arc summary")
This is the details page where there is a list of VMs that Azure Arc already knows about. The first time this is used, this list will be empty, but below is one I have previously created.
![alt text](https://github.com/jometzg/arc-demo/blob/master/arc-list.png "Arc list")

### Add a new VM in Azure Arc
Hit the "+" symbol to add a new VM to Arc.
![alt text](https://github.com/jometzg/arc-demo/blob/master/arc-add1.png "Add a VM")
Here I am going to select a Linux target machine.
![alt text](https://github.com/jometzg/arc-demo/blob/master/arc-add2.png "Add a VM")
As can be seen below, these are the details of the machine together with a shell script, which may be downloaded.
![alt text](https://github.com/jometzg/arc-demo/blob/master/arc-add3.png "Add a VM")

This generates a shell script for the target Linux machine
```
 # Download the installation package
wget https://aka.ms/azcmagent -O ~/install_linux_azcmagent.sh

# Install the hybrid agent
bash ~/install_linux_azcmagent.sh

# Run connect command
azcmagent connect --resource-group "traget-resource-group" --tenant-id "YOUR-AZURE-AD-TENANT-ID" --location "target-location" --subscription-id "YOUR-SUBSCRIPTION-ID"
```
This will need to be run on the target Linux machine

### Provisioning a target VM
As Azure already knows about its VMs, this demonstration will use a different target. This can easily be done on a Windows PC, but it is more interesting to use another cloud vendor and a Linux target.

This is the part that requires an AWS account (only for these test purposes, of course).

In the AWS console, here is the list of VMs. I have a couple already:
![alt text](https://github.com/jometzg/arc-demo/blob/master/aws-vm-summary.png "AWS VM instance list")

Now press the "Lauch" button.
![alt text](https://github.com/jometzg/arc-demo/blob/master/aws-launch1.png "AWS Launch")

You are now presented with a list of VM types.

**It is important for this demonstration that you select Ubuntu, as the agent at Arc preview time will only run on this flavour of Linux**

![alt text](https://github.com/jometzg/arc-demo/blob/master/aws-select-ubuntu.png "AWS select Ubuntu")

and go for the cheapest tier for testing purposes:

![alt text](https://github.com/jometzg/arc-demo/blob/master/aws-select-size.png "AWS select smallest size")

Now hit "Review and Launch"
![alt text](https://github.com/jometzg/arc-demo/blob/master/aws-review-and-launch.png "AWS review and launch")

Now you will be presented with a summary of what has been chosen

![alt text](https://github.com/jometzg/arc-demo/blob/master/aws-launch.png "AWS launch summary")

The VM will now start provisioning. So some waiting is required.

### Connecting to the target Ubuntu VM
Once the VM has provisioned, you can connect to the VM or rather get its connection credentials.
Hit "Connect", as shown below:
![alt text](https://github.com/jometzg/arc-demo/blob/master/aws-connect.png "AWS connect")

This will then display the connection details to allow you to open an ssh session to the VM. I have redacted mine.

![alt text](https://github.com/jometzg/arc-demo/blob/master/aws-connect2.png "AWS connect summary")

### Installing the Arc agent on the VM

