==============================
Distributed Cluster Guidance 
==============================

The following are sets of guidance and best practices for setting up various use cases on Kubernetes. These use cases assume you have deployed Netmaker and created a Network. If you have not done so, please first complete those steps (the Deployment section of the documentation can provide guidance).

Cluster Install
==================

The following scenarios require you to set up your cluster with Netmaker from the beginning. Before you install your cluster you must:

1) Deploy Netmaker, typically in a standalone environment (dedicated cluster, server, or set of servers)
2) Create a Network
3) Deploy Netclient on each Kubernetes host
4) Test network connectivity

Once those four steps are completed, you can continue with your cluster install. The primary concern while installing the cluster will be to ensure the CNI is attached to the network interface created by Netmaker.

Wide Nodes
==================

Cloud Bursting
===================

Edge-Based Clusters
=====================
