# Challenge 3: Introduction To Kubernetes

[< Previous Challenge](./02-acr.md) - **[Home](../README.md)** - [Next Challenge >](./04-k8sdeployment.md)

## Introduction

Now it is time to introduce the container orchestrator we all came for: Kubernetes!

## Description

In this challenge we will be provisioning our first Kubernetes cluster using the Azure Kubernetes Service (AKS). This will give us an opportunity to learn how to use the `kubectl` kubernetes command line tool, as well as using the Azure CLI to issue commands to AKS.

- Confirm that you've installed the Kubernetes command line tool (`kubectl`).
	- **Hint:** This can be done easily with the Azure CLI.
- Use the Azure CLI to create a new, multi-node AKS cluster with the following specifications:
	- Use the default Kubernetes version used by AKS.
	- The cluster will use Azure CNI networking.  
	- The cluster will use a managed identity
	- The cluster will use the maximum number of Availability Zones for improved worker node reliability.
	- The cluster should attach to your ACR created in Challenge 2
		- **NOTE:** Attaching an ACR requires you to have Owner or Azure account administrator role on the Azure subscription. If this is not possible then someone who is an Owner can do the attach for you after you create the cluster.
- Use kubectl to prove that the cluster is a multi-node cluster and is working.
- Use kubectl to examine which availability zone each node is in.  
- **Optional:** Bring up the AKS "Workloads" screen in the Azure portal.

## Success Criteria

1. The kubectl CLI is installed.
1. Show that a new, multi-node AKS kubernetes cluster exists.
1. Show that its nodes are running in multiple availability zones.
1. Show that it is using CNI networking