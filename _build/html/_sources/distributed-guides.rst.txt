==============================
Distributed Cluster Guidance 
==============================

The following are sets of guidance and best practices for creating distributed Kubernetes clusters, along with specific guidance for certain common scenarios.

Cluster Install
==================

Prerequisites
---------------

To create a distributed cluster with Netmaker, you must first create a Netmaker network on the hosts. Before you install your cluster you must:

1) Deploy Netmaker, typically in a standalone environment (dedicated cluster, server, or set of servers)
2) Create a Network
3) Deploy Netclient on each Kubernetes host
4) Test network connectivity (ensure all nodes are ping-able from each other over private addresses)

The Deployment section of the documentation can provide guidance on these steps.

Note: Depending on the scenario, and the separation between control plane and worker nodes, you may only need to set up Netmaker as the network for the worker node subnet.

Once these four steps are completed, you can continue with your cluster install. The primary concern while installing the cluster will be to ensure the CNI is attached to the network interface created by Netmaker.

CNI
--------

The choice of CNI should not matter significantly for your installation, so long as that CNI can be used with a Layer 3 network. Netmaker has been tested against:

- Calico
- Flannel
- Canal

When installing your cluster, you must configure your chosen CNI to use the Netmaker interface (e.x: nm-k8s).

Install
---------

How to specify the appropriate interfaces depends on what Kubernetes distribution you are using. For instance, on K3S you just need to specify a few extra parameters:
- --node-ip <private address>
- --node-external-ip <private address> 
- --flannel-iface <netmaker network interface>

On MicroK8S, it can be done by running "microk8s join" using the private address of the server.

Primarily, you need to determine how to:

a) Set the node IP / external IP to the private address
b) Tell the CNI to use the correct network interface

Once installed, your clusters traffic should all be routed over the private Netmaker network.

Test


The netmaker repo contains some helpful manifests for testing your installation. In particular, the "pingtest" deployment can deploy pods onto all nodes. You may then exec into those pods and see if you can "ping" pods on other nodes: https://raw.githubusercontent.com/gravitl/netmaker/master/kube/example/pingtest.yaml

kubectl exec -it pingtest-xxx -n pingtest -- sh

You can also try to reach a service on the network. For instance, the examples contain an nginx deployment/service. This can be used to test service connectivity via wget: https://raw.githubusercontent.com/gravitl/netmaker/master/kube/example/nginx-example.yaml

wget nginx.nginx.svc.cluster.local

If these tests succeed, it is safe to assume that network connectivity has been established for the pod and service networks of your cluster.

The following are discussions around particular use cases, though the setup is fairly trivial beyond the initial setup completed in this section.

Wide Nodes
==================

This scenario assumes you have a single cluster mostly deployed into one environment (e.g. Data Center, Cloud Provider). You may need to extend your cluster into a separate environment:

- a different cloud region
- a different cloud provider
- a different network in the same location
- an edge location

There could be many reasons for this:

- Compliance requires deploying an application in a particular region
- You require specialized hardware for an application (GPU's, TPU's, sensors), which is not available in the original cluster location
- You have logical network separations in your data center, but wish to run one cluster between them.
- You have extra hardware resources in a different location you wish to utilize

In all of these situations, there is really no additional setup required over the Cluster Install steps. Just simply repeat the process to deploy a new node in the new location.

Cloud Bursting
===================

In this scenario, you need temporary compute to perform a task, typically something compute/data intensive, which runs for a set period of time. Most likely, you do not want to own the hardware long-term, you just want to utilize the cloud resources when you need them. 

Running this scenario is largely dependent on the k8s distribution being used, and the cloud provider. For instance, we have developed templates for AWS and K3S, which can be used to cloud burst a K3S cluster using AWS Spot Instances. But the same templates will not work for other distros/clouds.

The main considerations here are that you need to:

1. Create a high-use access key
2. Create a script for installing the netclient and the k8s worker node
3. Have a cleanup method for deleting the k8s node and netmaker node when complete

Edge-Based Clusters
=====================

This scenario takes the "wide nodes" scenario a bit further. In the "wide nodes" scenario, we wish to deploy some extra nodes in a different location. In this scenario, we have a cluster which is almost entirely composed of "edge" locations, which may be geographically separated.

Primarily, we would see this in IoT, or at a branch-based business (retail, offices, etc) where there are many locations.

One consideration to keep in mind here is that the control plane should be centrally located. The control plane nodes must maintain small geographic separation to maintain functionality. 

One other note is that in some scenarios, many nodes may be behind NAT or CGNAT. You may wish to incorporate Relay functionality to make these nodes accessible.

The Relay is a feature which can be set in the UI. Simply designate a globally accessible node (such as the Netmaker server or control plane nodes) to act as the relay for any nodes which will be tough to reach.


