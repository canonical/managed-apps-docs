Resource provisioning
=====================

Managed deployment of Kubeflow on Azure uses opinionated ARM templates and post-provision bootstrap automation via a GitOps portal.


Azure resources provisioned
-----------------------------

The core Azure resources provisioned by the Managed Kubeflow include:

#. Virtual Network + subnet (CIDRs parameterized)
#. Network Security Group with curated ingress/egress rules
#. Public IP + NIC for the bootstrap VM (transient or retained as needed)
#. Managed Identity with role assignments (access to required Azure APIs)
#. Bootstrap VM (Ubuntu) running custom script extension to initialize juju
#. AKS cluster (agentPoolProfiles including system + optional GPU/worker pools)
#. Optional additional node pools (workerPools parameter) with taints/labels for workload segregation
#. Portal integration: notify & bootstrap endpoints invoked during deployment
#. Metering: Prometheus exporter (tenant‑aware) for usage aggregation and billing insights

Design rationale
-----------------

* 	Single-subnet default simplifies initial marketplace deployment; easily extended for multi-subnet (e.g. ``172.18.1.0/24`` GPU, ``172.18.2.0/24`` data plane).
* 	Managed Identity provides Azure-native auth for subsequent automation (portal script logs in with identity loop until ready).
* 	Discrete pools allow workload isolation via taints/labels; avoids noisy-neighbour between system components and user jobs.
* 	Custom Script Extension centralizes bootstrap logic without baking AMI-like customization.


GitOps & Bootstrap
-------------------

A portal call (``curl -d cloud=azure -d app=<app> ... /gitops/api/bootstrap.sh``) fetches a dynamic composite script, built from numbered fragments and Jinja context.

Juju controllers and charms deploy application components such as Charmed Kubeflow onto AKS. The marketplace template intentionally defers app logic to the bootstrap layer for upgradability.



Representative ARM template
---------------------------

The provisioning is done using opinionated ARM templates. A representative template (``mainTemplate.json``) specifies:

* Virtual Network (single address space, one subnet) with Network Security Group (NSG) inbound rule for 443 (extendable for additional ports)
* Static Public IP for bootstrap VM (+ DNS label ``bootstrap-<project>``)
* Network Interface bound to subnet & public IP
* User Assigned Managed Identity (attached to VM) + System Assigned identity for AKS
* Ubuntu 24.04 LTS Bootstrap Virtual Machine (custom script extension pulls ``bootstrap.sh`` from git and runs Juju + application deployment)
* Role Assignments: Owner / Network Contributor for VM, AKS, Managed Identity (supports delegated managed identity if enabled)
* Azure Kubernetes Service (AKS) cluster:

  - System pool components (fixed size 3 nodes) for control plane workloads
  - Up to 4 optional user pools (pool1..pool4) each with:

    * Autoscaling (min/max) and taints sku=<poolName>:NoSchedule
    *	Node labels pool=<poolName>
    * Shared subnet reference (single-subnet design)

  - AutoScaler profile tuned (least-waste, skip system/local storage nodes)
  - Explicit kubernetes version (e.g. 1.32) and unmanaged OS upgrade channel

* Custom Script Extension on VM writes OIDC configuration and triggers bootstrap portal

Parameters & Customization
---------------------------

The common adjustable parameters in the templates are:

* 	``virtualNetAddressCIDR``, ``virtualSubnetAddressCIDR``: default single-subnet (/24). You can expand to multiple subnets for separation (e.g. system vs GPU pools) by adding more subnet resources & assigning their IDs per pool.
* 	``componentsVmSize``, ``poolXWorkerVmSize``: scale vertically; use GPU SKUs for ML workloads.
* 	``poolXWorkerMinCount`` / ``poolXWorkerMaxCount``: horizontal autoscale boundaries.
* 	``numPools``: activate subset of defined pools (1-4). Additional pools can be added following existing pattern.
* 	``securityGroupSources``: tighten ingress from 0.0.0.0/0 to corporate CIDRs.
* 	OIDC parameters (``oidcIssuer``, ``oidcClientId``, ``oidcClientSecret``) for workload identity / Kubeflow UI login.
* 	``enableDelegatedManagedIdentity``: toggles delegated identity usage in role assignments.
