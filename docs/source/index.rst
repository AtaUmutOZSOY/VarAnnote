.. VarAnnote documentation master file, created by
   sphinx-quickstart on Fri Jun  6 21:45:36 2025.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

VarAnnote v1.0.7 Documentation
===============================

**VarAnnote** is a comprehensive variant analysis and annotation suite designed for genomic research and clinical applications. This production-ready tool provides advanced filtering, configuration management, and integration with multiple genomic databases.

🚀 **Key Features**
-------------------

* **Advanced Filtering System** - 13 filter operators with predefined filter sets
* **Configuration Management** - YAML-based configuration with environment variable support  
* **Database Integration** - Support for ClinVar, gnomAD, dbSNP, COSMIC, and more
* **Async API System** - High-performance API calls with rate limiting
* **Multiple Output Formats** - JSON, CSV, TSV, VCF, Excel, XML, Parquet
* **Professional CLI** - Comprehensive command-line interface
* **High Test Coverage** - 82% test coverage with 394+ tests

📦 **Quick Installation**
-------------------------

.. code-block:: bash

   pip install varannote

🔧 **Basic Usage**
------------------

.. code-block:: python

   from varannote.core.annotator import VariantAnnotator
   from varannote.utils.config_manager import ConfigManager
   
   # Load configuration
   config = ConfigManager()
   
   # Initialize annotator
   annotator = VariantAnnotator(config)
   
   # Annotate variants
   results = annotator.annotate_vcf("variants.vcf")

📚 **Documentation Sections**
------------------------------

.. toctree::
   :maxdepth: 2
   :caption: User Guide:
   
   installation
   quickstart
   configuration
   cli_usage
   filtering
   output_formats
   changelog

.. toctree::
   :maxdepth: 2
   :caption: API Reference:
   
   api/annotator
   api/databases
   api/config_manager
   api/filters

🔗 **Links**
------------

* **GitHub Repository**: https://github.com/AtaUmutOZSOY/VarAnnote
* **PyPI Package**: https://pypi.org/project/varannote/
* **Issue Tracker**: https://github.com/AtaUmutOZSOY/VarAnnote/issues

📄 **License**
--------------

This project is licensed under the MIT License.

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

