Operational considerations
===========================

NSG rule 
---------

The Network Security Group (NSG) rule is kept minimal by design. Production should restrict source ranges.


Autoscaler profile
------------------

The AutoScaler profile is tuned for cost efficiency. It is set to ``least-waste`` and can be changed to ``balanced`` if faster scale-out is preferred.


OS upgrade channel
------------------

The OS upgrade channel is set to `Unmanaged`. Planned patching cycles should be integrated through the CI pipeline.


Logging / Monitoring
--------------------

After bootstrapping, integrate with Azure Monitor / Container Insights.
