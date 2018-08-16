.. _cli:

nbreport command-line interface
===============================

nbreport provides a command-line interface that can be used directly, or through automated scripting.
nbreport uses the subcommand pattern so that several atomic commands are encapsulated in the same executable.
The CLI itself is implemented with Click_.
This section describes the basic design of this CLI.

.. _cli-clone:

nbreport clone
--------------

This command clones a report's repository from GitHub to the local filesystem.

Example:

.. code-block:: bash

   nbreport clone https://github.com/lsst/DMQA-001

.. _cli-init:

nbreport init
-------------

This command initializes a report instance.

Example:

.. code-block:: bash

   nbreport init DMQA-001

Alternative example that also clones the report repository:

.. code-block:: bash

   nbreport init https://github.com/lsst/DMQA-001

This command does the following:

1. **Reserves an instance ID for the report.**

   Instance IDs are managed by the ``api.lsst.codes/nbreport`` service.

2. **Creates a directory named after the report instance.**

   For example, if the report is ``DMQA-001`` and the reserved ID is ``1``, then the report instance is named ``DMQA-001-1``.
   This report instance directory (``DMQA-001-1``) is where all notebook computations are carried out.
   By giving each notebook an isolated directory, the system allows notebooks to create intermediate files in the current working directory without any concern of colliding with other report instances.

3. **Copies the nbreport.yaml file into the instance's directory.**

   In addition, a field named ``instance_id`` is added to the ``nbreport.yaml`` file.
   This allows the nbreport tool to concretely identify the report and its instance.

.. _cli-render:

nbreport render
---------------

This command renders the report template from the report repository into a Jupyter Notebook in the instance directory.

Example:

.. code-block:: bash

   nbreport render DMQA-001-1 -c dataRef=xyz -c paramX=2.9

The ``-c`` options are context overrides --- that is, values that replace the template variable defaults set in ``cookiecutter.json``.

.. _cli-compute:

nbreport compute
----------------

This command computes the Jupyter notebook in the report instance.
It does so in a "headless" manner, without opening a browser window.

.. code-block:: bash

   nbreport compute DMQA-001-1

.. _cli-upload:

nbreport upload
---------------

This command uploads the report instance to the ``api.lsst.codes/nbreport`` service, which then publishes the report with LSST the Docs.

.. code-block:: bash

   nbreport upload DMQA-001-1

Authentication for this command comes from a GitHub token.

.. _cli-issue:

nbreport issue
--------------

This all-in-one command renders, computes, and uploads a report instance.
This command is useful for automated environments.

Example:

.. code-block:: bash

   nbreport issue https://github.com/lsst/DMQA-001 -c dataRef=xyz

This command carries out the following steps:

1. Clones the report repository (like :ref:`cli-clone`).

2. Reserves the report instance number (like :ref:`cli-init`).

3. Renders the notebook given the provided context variables (like :ref:`cli-render`).

4. Computes the notebook (like :ref:`cli-compute`).

5. Uploads the computed notebook (like :ref:`cli-upload`).

.. _cli-test:

nbreport test
-------------

The ``nbreport test`` command provides a workflow for testing the executability of notebook templates during development.

This command works on an already-cloned report repository (the most common case in development).

Example:

.. code-block:: bash

   nbreport test DMQA-001

1. Creates a test instance directory (``DMQA-001-test``, by default), and clears pre-existing test instance.

2. Renders the notebook instance using default values from ``cookiecutter.json`` (by default).
   It is also possible to pass context overrides on the command line.

3. Executes and saves the notebook instance for inspection.

The advantage of this workflow for testing is that it creates a local test instance, rather than registering an instance with ``api.lsst.codes/nbreport``.

.. _cli-reproduce:

nbreport reproduce
------------------

The ``nbreport reproduce`` command is used to verify that a report is reproducible.
"Reproducible" in this context means that a published report instance can be re-generated, given the same template context variables, without meaningful variations in the output cells.

Example:

.. code-block:: bash

   nbreport reproduce https://dmqa-001.lsst.io/v/1

In this example, ``nbreport reproduce`` attempts to regenerate the ``DMQA-001-1`` report instance published at ``dmqa-001.lsst.io/v/1``.

.. note::

   This command could be implemented by adapting the nbval_ package.
