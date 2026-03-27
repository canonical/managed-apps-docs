Enable Microsoft providers
============================

This guide describes how to register your `Microsoft.Capacity`, `Microsoft.Storage`, and `Microsoft.Compute` providers required to set up a Canonical Managed application.

.. note::

   If you are already on the `Resource Providers` page, skip to step 4.

1. Go to the Azure `Subscriptions` page by searching on the top bar:

.. image:: ./enable-microsoft-providers-images/01-search-subscriptions.png
   :align: center

2. Click on the subscription you want to use for the setup:

.. image:: ./enable-microsoft-providers-images/02-choose-subscription.png
   :align: center

3. Search for `providers` on the menu search box and click on `Resource Providers`:

.. image:: ./enable-microsoft-providers-images/03-providers.png
   :align: center

4. Search for `capacity` using the `Filter by name` field:

.. image:: ./enable-microsoft-providers-images/04-capacity.png
   :align: center

5. You should see that `Microsoft.Capacity` status is `notRegistered` or `unregistered`:

.. image:: ./enable-microsoft-providers-images/05-capacity-result.png
   :align: center

6. Select it by checking the circle before the name and click `Register` on the top:

.. image:: ./enable-microsoft-providers-images/06-register.png
   :align: center

7. Wait until the provider status changes to `Registered`:

.. image:: ./enable-microsoft-providers-images/07-registered-status.png
   :align: center

8. Now do the same for `Microsoft.Compute`.

9. Search for it using the `Filter by name`.

10.  Select it by checking the circle before the name, and click the `Register` button.

11.  Wait until the provider changes to `Registered`:

.. image:: ./enable-microsoft-providers-images/08-registered-status-2.png
   :align: center

12. Now do the same for `Microsoft.Storage`.

13. Search for it using the `Filter by name`.

14.  Select it by checking the circle before the name, and click the `Register` button.

15.  Wait until the provider changes to `Registered`:

.. image:: ./enable-microsoft-providers-images/08-registered-status-2.png
   :align: center

16. Once all providers are registered, you need to restart your setup. The easiest way is to close the setup tab in your browser and restart it from the `Azure Marketplace <https://portal.azure.com/#create/canonical.managed-kubeflowkubeflow-metered>`_.