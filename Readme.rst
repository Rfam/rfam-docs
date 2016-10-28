Rfam Documentation
=====================

|docs|

This repository is the source of `Rfam documentation <http://rfam.readthedocs.io/en/latest/>`_ hosted on Read The Docs.

Installation
============

.. code:: bash

  # create a new virtual environment
  virtualenv /path/to/new/rfam-docs-virtualenv

  # activate virtual environment
  source /path/to/new/rfam-docs-virtualenv/bin/activate

  # install Python dependencies
  cd /path/to/rfam-docs
  pip install -r requirements.txt

Editing workflow
================

1. Install documentation

2. Build and view HTML output locally

    .. code:: bash

      cd docs
      sphinx-autobuild source build

    The documentation will be available at http://127.0.0.1:8000.

3. Make changes

4. Commit your changes in a new branch or a fork

5. Send a pull request on GitHub

Contact
========

If you have any questions or feedback, feel free to `submit a new issue <https://github.com/Rfam/docs/issues>`_ or email us at *rfam-help@ebi.ac.uk*.

.. |docs| image:: https://readthedocs.org/projects/rfam/badge/?version=latest
    :alt: Documentation Status
    :scale: 100%
    :target: https://rfam.readthedocs.io/en/latest/?badge=latest
