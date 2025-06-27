=======================
HumanBase documentation
=======================

This repository contains the documentation source files for `HumanBase <https://hb.flatironinstitute.org>`_. These files are Sphinx docs written with reStructuredText.

Build Status
------------

Check the Read the Docs build status and documentation: https://app.readthedocs.org/projects/humanbase/

The live documentation is available at: https://humanbase.readthedocs.io/

Quick Start
-----------

Prerequisites
~~~~~~~~~~~~~

* Python 3.8 or higher
* pip (Python package installer)
* Make (for building documentation)

Local Development Setup
~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

   # Clone the repository
   git clone https://github.com/aaronkw/humanbase-docs.git
   cd humanbase-docs

   # Create and activate a virtual environment
   python3 -m venv venv
   source venv/bin/activate

   # Install dependencies
   pip install -r docs/requirements.txt

Building Documentation Locally
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

   # Navigate to the docs directory
   cd docs

   # Clean previous builds (optional)
   make clean

   # Build HTML documentation
   make html

   # View the documentation
   open _build/html/index.html  # On macOS
   # Or: python -m http.server -d _build/html 8000  # Then visit http://localhost:8000

Documentation Structure
-----------------------

The documentation is organized as follows:

* ``docs/`` - Main documentation directory
  
  * ``index.rst`` - Main table of contents and entry point
  * ``conf.py`` - Sphinx configuration file
  * ``requirements.txt`` - Python dependencies for building docs
  * ``img/`` - Images and diagrams used in documentation
  * Tool-specific documentation:
    
    * ``sei.rst`` - Sei/DeepSEA sequence-based predictions
    * ``beluga.rst`` - DeepSEA (Beluga) chromatin profile predictions
    * ``expecto.rst`` - ExPecto expression predictions
    * ``clever.rst`` - CLEVER variant effect predictions
    * ``netwas.rst`` - NetWAS network-based association studies
    * ``in-silico-mutagenesis.rst`` - In silico mutagenesis analysis
    
  * Network documentation:
    
    * ``functional-networks.rst`` - Functional gene networks
    * ``tissue-networks.rst`` - Tissue-specific networks

* ``.readthedocs.yaml`` - Read the Docs build configuration
* ``README.rst`` - This file
