Kubeflow on Azure
=================

**What is it?**

Canonical Managed Kubeflow on Azure is a production-ready, fully supported
Kubeflow deployment engineered on Azure Kubernetes Service (AKS). It bundles
opinionated networking, secure identity, lifecycle automation, and integrated
portal & metering to give data science and platform teams a fast, governed
path from prototype to production. Canonical operates, monitors, and
continuously improves the stack under an SLA so you can focus on delivering
ML value.

**Key outcomes**

* Faster onboarding of ML workloads onto AKS with pre-validated architecture
* Built-in multi-tenant isolation and metering for chargeback/showback
* Reduced operational risk via Canonical 24/7 support & Firefighting response
* Production reliability (patching, scaling, security baselining) handled for you
* Alignment with upstream Kubeflow and Ubuntu LTS security updates

**Why Canonical & Kubeflow on Azure?**

* `Upstream fidelity`: Minimal drift from upstream Kubeflow; avoids lock-in.
* `Secure base`: Ubuntu Pro provides extended (10+ years) security maintenance for OS & open source components.
* `Cloud-native integration`: Leverages AKS autoscaling, managed identities, Azure networking constructs, and optional Azure GPU node pools.
* `Observability & billing`: Prometheus exporter with tenant labels for granular usage metrics; integrates with portal webhook for lifecycle events.
* `Proven operations`: Canonical engineers apply well-established runbooks spanning Kubernetes, Kubeflow, networking, storage, and upgrades.

---------

In this documentation
---------------------

.. grid:: 1 1 2 2

    .. grid-item-card:: Get started
        :link: get-started
        :link-type: doc

        **Start here** - use Canonical's Managed Kubeflow on Azure to get supported operation management, deployment and dedicated customer service.

    .. grid-item-card:: How-to guides
        :link: how-to/index
        :link-type: doc

        **Step-by-step guides** - learn about risks and mitigation strategies.

.. grid:: 1 1 2 2

    .. grid-item-card:: Reference
        :link: reference/index
        :link-type: doc

        **Technical information** - review the resources provisioned and customization boundaries.

    .. grid-item-card:: Explanation
        :link: explanation/index
        :link-type: doc

        **Concepts** - understand the design and architecture of Managed Kubeflow on Azure.

---------

Get in touch
------------

* `Talk to us about Managed Kubeflow on Azure <https://ubuntu.com/engage/managed-kubeflow-preview>`_

.. toctree::
   :hidden:
   :maxdepth: 1
   :caption: Get started

   Get started <get-started>

.. toctree::
   :hidden:
   :maxdepth: 1
   :caption: How-to guides

   How-to guides <how-to/index>

.. toctree::
   :hidden:
   :maxdepth: 1
   :caption: Reference

   Reference <reference/index>

.. toctree::
   :hidden:
   :maxdepth: 1
   :caption: Explanation

   Explanation <explanation/index>
