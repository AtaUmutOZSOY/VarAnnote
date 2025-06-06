.. VarAnnote documentation master file, created by
   sphinx-quickstart on Fri Jun  6 21:45:36 2025.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

VarAnnote v1.0.0 Documentation
==============================

**VarAnnote** is a comprehensive Python tool for genetic variant annotation that integrates multiple databases to provide detailed information about genetic variants. It supports parallel processing, advanced filtering, and multiple output formats.

.. image:: https://img.shields.io/badge/version-1.0.0-blue.svg
   :target: https://github.com/varannote/varannote
   :alt: Version

.. image:: https://img.shields.io/badge/python-3.8+-blue.svg
   :target: https://www.python.org/downloads/
   :alt: Python Version

.. image:: https://img.shields.io/badge/license-MIT-green.svg
   :target: https://opensource.org/licenses/MIT
   :alt: License

Key Features
------------

* **Multi-Database Integration**: ClinVar, dbSNP, gnomAD, COSMIC, PharmGKB, and more
* **Parallel Processing**: Efficient annotation of large variant sets
* **Advanced Filtering**: Quality-based, population frequency, and clinical significance filters
* **Multiple Output Formats**: JSON, CSV, TSV, VCF with customizable fields
* **Configuration Management**: YAML-based configuration with user preferences
* **Production Ready**: Comprehensive error handling, logging, and monitoring

Quick Start
-----------

Installation::

    pip install varannote

Basic Usage::

    from varannote import VarAnnote
    
    # Initialize annotator
    annotator = VarAnnote()
    
    # Annotate a variant
    result = annotator.annotate_variant("1", 230710048, "A", "G")
    print(result)

Command Line Interface::

    # Annotate variants from VCF file
    varannote annotate input.vcf --output results.json --format json
    
    # Use configuration file
    varannote annotate input.vcf --config config.yaml --parallel 4

.. toctree::
   :maxdepth: 2
   :caption: User Guide:
   
   installation
   quickstart
   configuration
   cli_usage
   filtering
   output_formats

.. toctree::
   :maxdepth: 2
   :caption: API Reference:
   
   api/annotator
   api/databases
   api/config_manager
   api/filters
   api/utils

.. toctree::
   :maxdepth: 2
   :caption: Examples:
   
   examples/basic_usage
   examples/advanced_filtering
   examples/batch_processing
   examples/custom_configuration

.. toctree::
   :maxdepth: 2
   :caption: Development:
   
   contributing
   testing
   performance
   changelog

.. toctree::
   :maxdepth: 1
   :caption: About:
   
   license
   authors
   acknowledgments

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

