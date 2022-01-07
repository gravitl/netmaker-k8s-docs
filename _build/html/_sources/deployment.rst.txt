============
Deployment
============

This page covers the general guidance (regardless of use case) for the 3 stages of a Netmaker deployment: Server Installation, Network Creation, and Client Installation

1) Server Installation
====================================

Considerations
----------------

You may think that for a Kubernetes scenario, you would want to deploy the Netmaker server onto the cluster you are managing. However, this is usually not recommended.

While typically reliable if correctly configured, consider if the Netmaker network stops working. Typically, you will need to debug the server. However, if Netmaker is deployed on a K8S cluster where it also manages the network, this means the container network is also now non-functional, meaning the netmaker server is unreachable! This can cause a huge headache and be very difficult to debug.

For this reason, it is typically recommend to deploy the Netmaker server either on its own VM, or on a dedicated "management" cluster. In either case, the Netmaker server should not run inside a cluster where it will also be configuring cluster networking.

Except in special circumstances, the best place for the Netmaker server is a dedicated, globally accessible location.

Installation
---------------

With the above guidance in mind, there are several ways to install the server, based on the environment and how you wish to maintain the server.

`For Trials, PoC's, and Experimenting <https://github.com/gravitl/netmaker/tree/master#get-started-in-5-minutes>`_ - Use this if you are looking to just get up and running as quickly as possible and try out a scenario. 


`For Small Deployments (Single Server, Non-HA) <https://docs.netmaker.org/quick-start.html>`_ - Use this if you are deploying for yourself or a small team, but want the server to run long term.

`For a Highly Available Server on Kubernetes <https://docs.netmaker.org/server-installation.html#highly-available-installation-kubernetes>`_ - Use this if deploying on K8S (again, we recommend a dedicated cluster for this).

`For a Highly Available Server (non-K8S) <https://docs.netmaker.org/server-installation.html#highly-available-installation-vms-bare-metal>`_ - Use this if you're looking for HA but don't want to deploy it on Kubernetes.

2) Network Creation
=====================

After deploying your Netmaker server via Step #1, you will need to navigate to your dashboard, and will be directed to create a user.

After creating a user and logging in, you will then need to create a network in order to set up your Kubernetes use case.

The Primary consideration here is that typically, UDP Hole Punching is not necessary. Why? Because Kubernetes nodes tend to be statically defined servers with dedicated IPs. We are not expecting them to move frequently, change addresses or be behind separate NAT's. Because of this, we recommend keeping UDP Hole Punching switched off. 

The other necessary consideration is address range. Make sure to choose an address range that will not conflict with:

a) The Kubernetes host subnet
b) The Kubernetes service network
c) The Kubernetes pod network

Outside of this, any (private) address range should work.

3) Client Installation
======================================

Once a network has been created, it's time to add nodes to the network.

Considerations
---------------

For Kubernetes scenarios, the client can be installed:

1) Directly on the host

2) As a container on the cluster

#1 should be performed only when building distributed clusters. This is necessary because the virtual network must be created before the cluster is installed (so the client can't be deployed to a container on the cluster yet). The interface created by the client is then used by the CNI for cluster networking.

For all other scenarios (remote access, multi-cluster networking, etc), we recommend deploying the client via containers. In some situations this will be a daemonset, and in others, it will be a standalone container on a dedicated host (usually for gateways).

a) Directly on Host (pre-K8S):
---------------------------------

Regular client install instructions can be followed here: https://docs.netmaker.org/getting-started.html#deploy-nodes 

More detailed client install instructions can be found here: https://docs.netmaker.org/client-installation.html

b) In Container (post-K8S)
-------------------------------

To deploy in a container, you will need a manifest. Examples can be found here: https://github.com/gravitl/netmak8s

If deploying a daemonset to encompass all (or most) nodes, use netclient-daemonset.yml as the basis. You will need to insert a key (ACCESS_TOKEN), specify a network (NETWORK), and change to the appropriate version.

If deploying a gateway, use netclient-gateway.yml as the basis. You will need to insert a key (ACCESS_TOKEN), specify a network (NETWORK), and change to the appropriate version.

