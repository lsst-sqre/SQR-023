.. _report-repository:

The GitHub repository of a report
=================================

Each notebook-based report has its own GitHub repository.
In the notebook-based report system, the GitHub repository for a report contains the source to generate a report instance.
The GitHub repository not contain the instances themselves, those are published and archived with LSST the Docs.

The GitHub repository for a report has several standardized files.
For illustration, the repository for a report with a handle ``DMQA-001`` is laid out like this:

.. code-block:: text

   DMQA-001/
   ├── cookiecutter.json
   ├── DMQA-001.ipynb
   ├── nbreport.yaml
   └── README.rst

The following sections describe each file.

cookiecutter.json
-----------------

The ``cookiecutter.json`` file, adopted from the Cookiecutter_ project, establishes the template context.
This file both defines the template variables that are expected in the report notebook and also defines default values.

A basic ``cookiecutter.json`` file that defines keys ``cookiecutter.a`` and ``cookiecutter.b`` with default values of ``0`` and ``1``, respectively, looks like this:

.. code-block:: json

   {
     "a": 0,
     "b": 1
   }

Then a cell in the ``DMQA-001.ipynb`` notebook file can use those template variables using standard Jinja syntax:

.. code-block:: jinja

   answer = {{ cookiecutter.a }} + {{ cookiecutter.b }}

That cell would be rendered, if using the default values, as:

.. code-block:: python

   answer = 0 + 1

DMQA-001.ipynb
--------------

This file, named after the report's handle, is a Jupyter notebook.
This notebook contains the code and prose that, when executed, becomes a report instance.

This ``ipynb`` file must be committed into the GitHub repository in an unexecuted state, without outputs.

The source of each cell in the ``ipynb`` file is treated as a Jinja template (see :ref:`notebook-templating`).

nbreport.yaml
-------------

This file provides configuration for the report within the LSST notebook-based report system.

For the DMQA-001 example report, this file looks like:

.. code-block:: yaml

   handle: DMQA-001
   ltd_product: dmqa-001
   repository: https://github.com/lsst/DMQA-001
   published_url: https://dmqa-001.lsst.io
   ipynb: DMQA-001.ipynb

README.rst
----------

The README file describes the report, for users on GitHub.

.. note::

   The report repository should contain a description that can published on the report's published homepage.
   This description could either come from the README or from the ``nbreport.yaml`` file.
