# Azure Arc Demonstration
This is a brief demostration on how to use Azure Arc to monitor AWS Linux instances.

## What is Azure Arc?
The [documentation](https://azure.microsoft.com/en-gb/services/azure-arc/) gives the following summary:

>For customers who want to simplify complex and distributed environments across on-premises, edge and multicloud, Azure Arc enables >deployment of Azure services anywhere and extends Azure management to any infrastructure


![alt text](https://github.com/jometzg/arc-demo/blob/master/overview.png "Arc overview")

**Organize and govern across environments** - Get databases, Kubernetes clusters, and servers sprawling across on-premises, edge and multicloud environments under control by centrally organizing and governing from a single place.

**Manage Kubernetes Apps at scale** - Deploy and manage Kubernetes applications across environments using DevOps techniques. Ensure that applications are deployed and configured from source control consistently.

**Run data services anywhere** - Get automated patching, upgrades, security and scale on-demand across on-premises, edge and multicloud environments for your data estate.

## This demonstration Scope
This demonstration is only focussed around the first feature which is to allow governance across environments. It illustrates how a remote VM can be managed in Azure via Arc.

## Prerequisites
1. An Azure account (of course)
2. A test AWS account (can be a free trial one)
3. Some basic bash and command line skills

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
azcmagent connect --resource-group "target-resource-group" --tenant-id "YOUR-AZURE-AD-TENANT-ID" --location "target-location" --subscription-id "YOUR-SUBSCRIPTION-ID"
```
This will need to be run on the target Linux machine

### Provisioning a target VM
As Azure already knows about its VMs, this demonstration will use a different target. This can easily be done on a Windows PC, but it is more interesting to use another cloud vendor and a Linux target.

This is the part that requires an AWS account (only for these test purposes, of course).

In the AWS console, here is the list of VMs. I have a couple already:
![alt text](https://github.com/jometzg/arc-demo/blob/master/aws-vm-summary.png "AWS VM instance list")

Now press the "Launch Instance" button.
![alt text](https://github.com/jometzg/arc-demo/blob/master/aws-launch1.png "AWS Launch")

You are now presented with a list of VM types.

**It is important for this demonstration that you select Ubuntu, as the agent at Arc preview time will only run on this flavour of Linux**

![alt text](https://github.com/jometzg/arc-demo/blob/master/aws-select-ubuntu.png "AWS select Ubuntu")

and go for the cheapest tier for testing purposes:

![alt text](https://github.com/jometzg/arc-demo/blob/master/aws-select-size.png "AWS select smallest size")

Now hit the "Review and Launch" button.

![alt text](https://github.com/jometzg/arc-demo/blob/master/aws-review-and-launch.png "AWS review and launch")

Now you will be presented with a summary of what has been chosen. Then press the "Launch" button.

![alt text](https://github.com/jometzg/arc-demo/blob/master/aws-launch.png "AWS launch summary")

The VM will now start provisioning. So some waiting is required.

### Connecting to the target Ubuntu VM
Once the VM has provisioned, you can connect to the VM or rather get its connection credentials.
Hit "Connect", as shown below:
![alt text](https://github.com/jometzg/arc-demo/blob/master/aws-connect.png "AWS connect")

This will then display the connection details to allow you to open an ssh session to the VM. I have redacted mine.

![alt text](https://github.com/jometzg/arc-demo/blob/master/aws-connect2.png "AWS connect summary")


**Note that AWS uses a private key file (PEM) as a means to authenticate to the VM. The PEM will need to be downloaded the first time a connection is attempted and stored on your PC. This is explained in the AWS documentation. Once you have a PEM downloaded, you can use this for multiple VMs - though this would not be recommended for anything production.**

```
ssh -i "firstvm.pem" ubuntu@ec2-3-17-XX-XX.us-east-2.compute.amazonaws.com
```
![alt text](https://github.com/jometzg/arc-demo/blob/master/aws-vm-shell.png "AWS VM shell")

### Installing the Arc agent on the VM
Once you have a shell onto the target machine, it is "just" a matter of running the Arc generated script on the Linux machine.

I have noticed a couple of things:
1. The commands really need to be preceeded with a "sudo"
2. As you have connected to the VM using a certificate you don't know the password.

This can be remedied by:
```
sudo passwd
```
This will prompt you twice for a root password.

You can then execute the script's 3 steps. This will download the client and install it. The final step connects the VM to Azure.

Phew! Almost there! So now switch contexts back to the Azure portal.

### Looking at the remote VM in Azure
Switching back to the Azure portal, the remote VM now appears in in the list of VMs in the Arc section of the portal:
![alt text](https://github.com/jometzg/arc-demo/blob/master/arc-vm-appears-in-arc.png "AWS VM in Arc")

Clicking on the Vm here, reveals the VM details, just like and Azure VM.

![alt text](https://github.com/jometzg/arc-demo/blob/master/arc-vm-details.png "AWS VM details")

If you noticed from the script on that gets executed on the VM, an Azure resource group was mentioned. This is to allow the VM to appear in your chosen resource group - just like any other Azure resources. In this demonstration I chose the resource group "arc-rg". So let's look at its contents.

![alt text](https://github.com/jometzg/arc-demo/blob/master/arc-vm-in-rg.png "AWS VM in Azure resource group")

This is then the start of the management journey for that VM. This VM can then be managed like other VMs. You can apply Azure policies to this VM, just like any other Azure VM.

## Summary
This demonstration shows that external resources (in this case VMs) can be managed in Azure. This is the first step of capabilities that Arc will bring to Azure.


