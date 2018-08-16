.. _notebook-templating:

Templating of the report notebook
=================================

The Jupyter notebook in a report's GitHub repository is templated so that it can be customized on-demand when the report is generated.

Use of Cookiecutter and Jinja
-----------------------------

Jinja_ is the templating format.
The notebook-based report system uses Cookiecutter_ as a convenient wrapper around Jinja.
Although the notebook-based report system does not use Cookiecutter_ for its true purpose of instantiating entire file trees, Cookiecutter_ has these capabilities that can be adapted into the notebook-based report system:

- The :file:`cookiecutter.json` file is useful for defining the full set of template variables, along with their types, and defaults.
  :file:`cookiecutter.json` is also useful for creating secondary variables based on prior variables.

- Cookiecutter has a mechanism for running `pre- and postprocessing hooks <https://cookiecutter.readthedocs.io/en/latest/advanced/hooks.html>`_, if necessary.

- Cookiecutter has useful way of `registering Jinja extensions <https://cookiecutter.readthedocs.io/en/latest/advanced/template_extensions.html>`__.

Cookiecutter's own command line interface is not used by the notebook-based report system.
Instead, cookiecutter's Python APIs are invoked by the nbreport command-line client.
Doing so enables cell-wise templating, as described next.
This usage pattern is already used by LSST for the `lsst/templates`_ project.

Cell source templating
----------------------

Rather than interpreting the entire notebook file as a Jinja template, the notebook-based report system is designed so that the source of individual cells is processed as a Jinja_ template.
This distinction is key because it ensures that the notebook file (``ipynb`` format) can always be opened, displayed, and authored in the Jupyter notebook viewer or JupyterLab.
Notebook authors simply mark up the Markdown and Python cells with Jinja formatting.
