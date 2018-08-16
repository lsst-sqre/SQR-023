Requirements
============

The notebook-based report system is driven by these informally-defined design requirements:

- A document handle corresponds to a templated Jupyter Notebook.

- Notebook instances a generated from templates.

- Each notebook instance has a unique serial number so that a notebook instance can be identified universally from a combination of document handle and instance serial number.

- Once a notebook instance is generated, it is runnable.
  Notebooks may only be runnable in certain environments where data is available, such as the LSST Science Platform.

- Generated notebook instances are published to the web with LSST the Docs.

- The process of creating a notebook instance, running the notebook, and publishing the notebook instance is completely automated.
  Manual intervention is only required to develop the notebook template for the document and to set up the automations to run the report generation workflow.

- The notebook-based report system should share as much infrastructure as possible with notebook-based technical notes.
