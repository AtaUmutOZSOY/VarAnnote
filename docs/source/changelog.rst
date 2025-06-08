Changelog
=========

Version 1.0.6 (2025-01-06)
---------------------------

**PATH Configuration Improvements**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* Automatic PATH setup for Windows users
* Eliminated need for manual PATH configuration
* Improved CLI accessibility after pip install
* Enhanced user experience with zero-configuration setup

**Entry Points Optimization**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* Fixed entry points conflicts between setup.py and pyproject.toml
* Streamlined console scripts configuration
* Improved package installation reliability

**Documentation Updates**
~~~~~~~~~~~~~~~~~~~~~~~~~
* Updated all documentation to reflect v1.0.6
* Enhanced installation instructions
* Improved CLI usage examples

Version 1.0.0 (2025-01-05)
---------------------------

**Major Release - Production Ready**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* Upgraded from Beta (v0.2.0) to Production/Stable status
* Comprehensive test coverage improvement (16% → 82%)
* 394+ tests with 99.5% success rate

**New Features**
~~~~~~~~~~~~~~~~
* **Advanced Filtering System**: 13 filter operators with predefined filter sets
* **Configuration Management**: YAML-based configuration with environment variables
* **Enhanced Database Integration**: Improved support for ClinVar, gnomAD, dbSNP, COSMIC
* **Multiple Output Formats**: JSON, CSV, TSV, VCF, Excel, XML, Parquet support

**Code Quality Improvements**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* Fixed critical test import errors
* Resolved performance test division by zero errors
* Updated pytest configuration for custom markers
* Comprehensive error handling and validation

**Documentation**
~~~~~~~~~~~~~~~~~
* Complete Sphinx documentation with RTD theme
* API documentation for all modules
* User guides (installation, quickstart, configuration)
* CLI usage guide with examples
* Filtering system documentation
* Output formats guide

**Academic Infrastructure**
~~~~~~~~~~~~~~~~~~~~~~~~~~~
* DOI assignment through Zenodo (10.5281/zenodo.15615370)
* CITATION.cff file for GitHub citations
* Multiple citation formats (APA, BibTeX, IEEE)
* Academic contact information

**Development Infrastructure**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* GitHub repository made public
* ReadTheDocs integration
* PyPI package deployment
* Automated testing and CI/CD ready

Version 0.2.0 Beta (Previous)
------------------------------

**Initial Features**
~~~~~~~~~~~~~~~~~~~~
* Basic variant annotation functionality
* CLI interface implementation
* Database integrations (9 databases)
* PyPI package publication
* 16% test coverage
* Beta release status 