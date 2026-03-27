Deployment and operations lifecycle
====================================


Provisioning
------------
An ARM template deploys networking, identity, VM, AKS cluster and assigns roles.


Bootstraping
------------
A VM script extension pulls portal bootstrap content, configures Juju operators and sets up Kubeflow components.


Operation
---------
Canonical monitors cluster health, node autoscaling, workload stability, storage, and security patches.


Metering
--------
An exporter gathers instance/tenant metrics that are fed to Prometheus, which in turn feeds data to the portal billing engine.


Change management
------------------
Controlled updates of AKS and Kubeflow components are executed via runbooks which include a rollback strategy.


Support events
--------------
Severity-1 incidents trigger Firefighting, where an engineer is engaged within minutes (max 1 hour).
