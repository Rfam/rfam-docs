Rfam Documentation
=====================

|docs|

This repository is the source of `Rfam documentation <http://rfam.readthedocs.io/en/latest/>`_ hosted on Read The Docs.

Installation
============

.. code:: bash

  cd /path/to/rfam-docs

  # create a new virtual environment
  virtualenv `pwd`/env

  # activate virtual environment
  source env/bin/activate

  # install Python dependencies
  python -m pip install -r requirements.txt

  # Note - if you are using a newer version of Python (3.13 +) you may need to use updated dependencies instead
  python -m pip install -r requirements-local.txt

  # serve documentation locally at http://127.0.0.1:8000
  cd docs
  sphinx-autobuild source build

How to contribute
=================

Edit on GitHub
--------------

1. Fork the repo or create a new branch

2. Make changes `using GitHub interface <https://help.github.com/articles/editing-files-in-your-repository/>`_

3. Send a pull request

Edit locally
------------

1. Fork the repo or create a new branch

2. Install documentation

3. Build and view HTML output locally

4. Make changes

5. Commit and push to GitHub

6. Send a pull request

Contact
========

If you have any questions or feedback, feel free to `submit a new issue <https://github.com/Rfam/docs/issues>`_ or email us at *rfam-help@ebi.ac.uk*.

.. |docs| image:: https://readthedocs.org/projects/rfam/badge/?version=latest
    :alt: Documentation Status
    :scale: 100%
    :target: https://rfam.readthedocs.io/en/latest/?badge=latest
