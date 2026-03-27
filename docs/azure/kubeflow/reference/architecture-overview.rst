Architectural overview of Managed Kubeflow
===========================================

The Managed Kubeflow design delivers an opinionated, minimal set of Azure resources (network, identities, bootstrap VM, AKS with structured node pools), with application complexity being deferred to a GitOps Juju-based bootstrap layer.

High Level Design
-----------------

Jumphost
~~~~~~~~

This will be used for CLI clients to the rest of the components like the different Kubernetes, Juju, Azure CLI and others. Additionally, the Secrets will be stored in the jumphost file system. The secrets folder will not be added to the git repository.

Juju Controllers
~~~~~~~~~~~~~~~~

Juju Controllers in HA cluster on top of Azure VMs. These controllers manage the following models:

* controller: Juju Controllers (VM) on top of Azure VMs
* cos: Canonical Observability Stack on top AKS COS
* kubeflow: Charmed Kubeflow and Charmed MLflow on top AKS Prod

Physical distribution of VMs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

VMs of each machine type should be distributed in different availability zones.

Software Versions
------------------

Using the latest stable version of most software. Except for `this compatibility matrix <https://documentation.ubuntu.com/charmed-kubeflow/reference/supported-versions/>`_.

Versions of sub-components, for instance Juju Bundles of COS, Charmed Kubeflow and MLflow will be recorded in the Git repository of the deployment.


.. list-table::
   :widths: 35 30 35
   :header-rows: 1

   * - **Software**
     - **Format**
     - **Version**
   * - Ubuntu Server
     - VM OS - AMI
     - Noble 24.04
   * - Juju CLI
     - snap
     - 3.6/stable LTS
   * - Juju Controller
     - Juju Charm (VM)
     - Same as Juju CLI version
   * - Azure Kubernetes (AKS)
     - Public Cloud Service
     - 1.32/stable Kubernetes API Latest release compatible with Kubeflow
   * - NVIDIA GPU Operator
     - Helm Chart (K8s)
     - latest
   * - NVIDIA GPU Drivers
     - Deb
     - latest
   * - Canonical Observability Stack (COS) Lite
     - Juju Bundle (K8s)
     - latest/edge
   * - Charmed Kubeflow
     - Juju Bundle (K8s)
     - 1.10/stable
   * - Charmed MLflow
     - Juju Bundle (K8s)
     - 2.2/stable


GPUs
----

GPUs on the Kubernetes layer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

GPUs can be accessed on pods using NVIDIA GPU Operator, making each GPU only available on one pod at the same time.

Storage
-------

Azure
~~~~~~

Standard_SSD type of block storage will be used for all VM disks as well as all PVCs for pods. 

Azure CSI is integrated with Azure Kubernetes. All persistent PVCs will have a retain storage policy to prevent deletion of the volume in case the pod is deleted.

ZRS type of storage will be used for multi AZ components like Kubeflow control plane. LRS type of storage will be used for single AZ components like COS and Kubeflow workers.

Region
-------

Canonical will deploy the infrastructure in the provided subscription and chosen region by the user deploying from the Marketplace.

Networking
-----------

In the network design above, both Kubeflow clusters will have access to the Infrastructure network (Jumphost, Juju controllers, Canonical Observability Stack).

No proxy in the environment and default Azure DNS server configurations.

Load Balancing
---------------

External load balancers from Azure will be used to expose:
* Kubeflow & MLflow GUIs
* COS (Canonical Observability Stack)

AKS node autoscaler
--------------------

We will use default autoscaler options if not specified below. Here is a `list of default options <https://learn.microsoft.com/en-us/azure/aks/cluster-autoscaler?tabs=azure-cli#cluster-autoscaler-profile-settings>`_


.. list-table::
   :header-rows: 1

   * - **Configuration**
     - **Value**
   * - **expander**
     - priority, least-waste
   * - **ignore-daemonsets-utilization**
     - true
   * - **scale-down-utilization-threshold**
     - 0.2
   * - **scale-down-unneeded-time**
     - 30 minutes


AKS node taints and labels
--------------------------

The labels are added based on the flavors for the instance sizes chosen during the deployment phase on top of the labels created by AKS services and nvidia-operator.


User management
----------------

Kubeflow users will be mapped from EntraID integration with Dex through OIDC protocol. integration with Dex. Default mapping is to auto create a Kubernetes/Kubeflow namespace per user. The customer wants to be able to have Kubernetes/Kubeflow namespaces per project, each including multiple users, this will be managed by the customer to create namespaces and invite users to the namespaces.

Data isolation
~~~~~~~~~~~~~~

Kubeflow pipelines data isolation between users will be achieved by not using S3 for passing data across steps but rather using the same PVC for different steps of a pipeline. 

Furthermore, for MLflow there will not be isolation between users, but isolation between internal users and external users. Secret credentials will only be given on a case by case to specific users manually, so the Juju relation between Charmed MLflow and Charmed Kubeflow will not be added to avoid all users getting MLflow credentials. This is important for the customer to avoid external users having access to MLflow data.    
