Customization options 
=====================

Customization focuses on scaling, networking, identity, and workload segregation while preserving core deployment primitives.

You may tailor network layout (CIDRs, multiple subnets), pool counts & sizes, AKS version, identity model, NSG rules, disk sizes. Maintain primary components: VNet, managed AKS cluster with at least one system pool, bootstrap VM + identity, and role assignments required for automation.

Some customization boundaries are described below.

Allowed changes 
----------------

These changes can be made by the customer directly (self-service) or with Canonical guidance:

*	Add/resize worker pools (e.g., GPU, spot instances)
*	Adjust AKS autoscaler parameters & node taints/labels
*	Integrate Azure services (Key Vault, Blob Storage, Event Hub) via managed identity bindings
*	Tuned Kubeflow components (pipelines concurrency, notebook images)


Guardrails / restricted changes 
--------------------------------

These changes are restricted and require Canonical review:

*	Direct modifications to core system pool or control plane policies
*	Disabling baseline security controls (NSG rules, RBAC, PodSecurity settings)
*	Unapproved admission controller changes impacting multi‑tenancy isolation
*	Arbitrary downgrade of AKS or Kubeflow component versions



