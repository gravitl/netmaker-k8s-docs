
.. image:: images/nm_plus_k8s_clear.png
   :width: 75%
   :alt: Netmaker WireGuard
   :align: center

.. role:: raw-html(raw)
    :format: html

:raw-html:`<br />`

=======================================
Welcome to the Netmaker K8S Guide
=======================================


Netmaker enhances Kubernetes clusters by providing flexible and secure networking for cross-environment scenarios, for instance:

- Create clusters that span environments
- Remotely access a cluster from an external source
- Remotely access an external source from a cluster
- Bridge cluster networks
- Use spot instances to cloud burst (temporary compute)
- Create a NAT Gateway for secure cluster internet access

This guide covers these use cases, in addition to explaining core concepts, how to deploy, and troubleshooting.

Core Concepts
---------------

Some fundamental challenges with Kubernetes networking, and why you might use Netmaker to solve these challenges.

.. toctree::
   :maxdepth: 2
   
   core-concepts

Deployment
---------------

A discussion of the various ways to deploy Netmaker and the Netclient.

.. toctree::
   :maxdepth: 1

   deployment


Distributed Cluster Guides
----------------------------

Detailed guides for running distributed Kubernetes clusters with Netmaker.

.. toctree::
   :maxdepth: 2
      
   distributed-guides

Additional Guides
---------------------

Detailed guides for solving various networking challenges on Kubernetes with Netmaker.

.. toctree::
   :maxdepth: 2
      
   additional-guides

Additional Resources
----------------------

Links to other resources that may be helpful.

.. toctree::
   :maxdepth: 2

   resources
