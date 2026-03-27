Documentation for Canonical's Managed Kubeflow offering
=======================================================

This is the documentation repository for Managed Kubeflow. Currently it only includes content for Managed Kubeflow on Azure. But the repository is structured to include content for other clouds as well.

The documentation for each cloud will be published on a separate URL. 


How to work with this documentation
-----------------------------------

Download and install
~~~~~~~~~~~~~~~~~~~~
Fork and clone this repository.

Once cloned, run::

	cd docs
	make install

This invokes the ``install`` command from the ``Makefile``, which creates a
virtual environment (``.sphinx/venv``) and installs all the dependencies specified in
``docs/requirements.txt``.


Build and serve the documentation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The ``make run`` command can be used to start the ``sphinx-autobuild`` documentation server.
Since each cloud has it's own separate documentation set, you have to specify the required cloud name as a command line parameter. For example the command below will build and serve the documentation for Azure cloud::

	PROJECT=azure make run

The different projects that can be specfied are 'all-clouds', 'aws', 'azure', 'gcp', and 'on-prem'. Currently, only 'azure' can be used since that is the only project which has content. Also, 

The documentation will be available at `127.0.0.1:8000 <http://127.0.0.1:8000>`_ or equivalently at `localhost:8000 <http://localhost:8000>`_.

The command:

* activates the virtual environment and serves the documentation
* rebuilds the documentation each time you save a file
* sends a reload page signal to the browser when the documentation is rebuilt

(This is the most convenient way to work on the documentation, but you can still use
the more standard ``make html``. For instance, ``PROJECT=azure make html`` will create the 
azure related html pages in _build/azure.)


Check spelling
~~~~~~~~~~~~~~

Run a spell check::

	PROJECT=azure make spelling

If new words are to be added to the allowed list, update ``.custom_wordlist.txt`` accordingly.


Check links
~~~~~~~~~~~

Run a check for broken links::

	PROJECT=azure make linkcheck

