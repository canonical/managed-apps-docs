Extensibility examples 
=======================

These examples illustrate how to extend and customize your Managed Kubeflow deployment.

Multi-subnet
------------

Add extra ``Microsoft.Network/virtualNetworks/subnets`` entries and point ``vnetSubnetID`` per pool to distinct subnets.


Additional Ingress
-------------------

Append security rules for ports (e.g. 80, 22 restricted to management CIDR, 9443 for API gateway).


GPU Pools
---------

Set ``poolXWorkerVmSize`` to GPU SKU (e.g. ``Standard_NC24ads_A100_v4``) and adjust taints ``accelerator=nvidia`` for scheduling.

Private Cluster
----------------

Enable AKS private API endpoint; add Azure Bastion or jump host for operator access.


Expanded Identity
-----------------

Attach extra user-assigned identities for pulling images from private ACR.


