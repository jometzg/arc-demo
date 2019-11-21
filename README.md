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

