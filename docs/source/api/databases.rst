Database Modules
================

VarAnnote integrates with multiple genetic databases to provide comprehensive variant annotation.

Database Manager
----------------

.. automodule:: varannote.database_manager
   :members:
   :undoc-members:
   :show-inheritance:

ClinVar Database
----------------

.. automodule:: varannote.databases.clinvar
   :members:
   :undoc-members:
   :show-inheritance:

dbSNP Database
--------------

.. automodule:: varannote.databases.dbsnp
   :members:
   :undoc-members:
   :show-inheritance:

gnomAD Database
---------------

.. automodule:: varannote.databases.gnomad
   :members:
   :undoc-members:
   :show-inheritance:

COSMIC Database
---------------

.. automodule:: varannote.databases.cosmic
   :members:
   :undoc-members:
   :show-inheritance:

PharmGKB Database
-----------------

.. automodule:: varannote.databases.pharmgkb
   :members:
   :undoc-members:
   :show-inheritance:

OMIM Database
-------------

.. automodule:: varannote.databases.omim
   :members:
   :undoc-members:
   :show-inheritance:

GTEx Database
-------------

.. automodule:: varannote.databases.gtex
   :members:
   :undoc-members:
   :show-inheritance:

ENCODE Database
---------------

.. automodule:: varannote.databases.encode
   :members:
   :undoc-members:
   :show-inheritance:

RegulomeDB Database
-------------------

.. automodule:: varannote.databases.regulomedb
   :members:
   :undoc-members:
   :show-inheritance:

Database Integration Examples
-----------------------------

Using Individual Databases::

    from varannote.databases import clinvar, dbsnp, gnomad
    
    # Query ClinVar
    clinvar_data = clinvar.query_variant("1", 230710048, "A", "G")
    
    # Query dbSNP
    dbsnp_data = dbsnp.query_variant("1", 230710048, "A", "G")
    
    # Query gnomAD
    gnomad_data = gnomad.query_variant("1", 230710048, "A", "G")

Database Manager Usage::

    from varannote.database_manager import DatabaseManager
    
    # Initialize with specific databases
    db_manager = DatabaseManager(databases=['clinvar', 'dbsnp', 'gnomad'])
    
    # Query all databases
    results = db_manager.query_all("1", 230710048, "A", "G")
    
    # Query with priorities
    db_manager.set_priorities({'clinvar': 1, 'gnomad': 2, 'dbsnp': 3})
    prioritized_results = db_manager.query_prioritized("1", 230710048, "A", "G")

Custom Database Configuration::

    # Configure API keys
    db_manager.configure_database('cosmic', api_key='your_api_key')
    
    # Set rate limits
    db_manager.set_rate_limit('clinvar', requests_per_second=10)
    
    # Enable caching
    db_manager.enable_cache(cache_dir='/path/to/cache', ttl=3600) 