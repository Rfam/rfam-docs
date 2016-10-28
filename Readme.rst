Rfam Documentation
=====================

|docs|

This repository is the source of `Rfam documentation <http://rfam.readthedocs.io/en/latest/>`_ hosted on Read The Docs.

Installation instructions
==========================

**First time setup**

.. code:: bash

  # create a new virtual environment
  virtualenv /path/to/new/rfam-docs-virtualenv

  # activate virtual environment
  source /path/to/new/rfam-docs-virtualenv/bin/activate

  # install Python dependencies
  cd /path/to/rfam-docs
  pip install -r requirements.txt

**Workflow**

.. code:: bash

  cd docs
  sphinx-autobuild source build

The documentation will be served at http://127.0.0.1:8000.

**Generate documentation**

.. code:: bash

  cd docs
  make html

The output will be in the ``docs/build`` folder.

Contact
========

If you have any questions or feedback, feel free to `submit a GitHub issue <https://github.com/Rfam/docs/issues>`_ or email us at *rfam-help@ebi.ac.uk*.

.. |docs| image:: https://readthedocs.org/projects/rfam/badge/?version=latest
    :alt: Documentation Status
    :scale: 100%
    :target: https://rfam.readthedocs.io/en/latest/?badge=latest
