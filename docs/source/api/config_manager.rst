Configuration Manager
=====================

The ConfigManager handles all configuration aspects of VarAnnote, including YAML files, environment variables, and user preferences.

.. automodule:: varannote.config_manager
   :members:
   :undoc-members:
   :show-inheritance:

ConfigManager Class
-------------------

.. autoclass:: varannote.config_manager.ConfigManager
   :members:
   :undoc-members:
   :show-inheritance:
   :special-members: __init__

Configuration Classes
---------------------

DatabaseConfig
~~~~~~~~~~~~~~

.. autoclass:: varannote.config_manager.DatabaseConfig
   :members:
   :undoc-members:
   :show-inheritance:

PerformanceConfig
~~~~~~~~~~~~~~~~~

.. autoclass:: varannote.config_manager.PerformanceConfig
   :members:
   :undoc-members:
   :show-inheritance:

CacheConfig
~~~~~~~~~~~

.. autoclass:: varannote.config_manager.CacheConfig
   :members:
   :undoc-members:
   :show-inheritance:

OutputConfig
~~~~~~~~~~~~

.. autoclass:: varannote.config_manager.OutputConfig
   :members:
   :undoc-members:
   :show-inheritance:

LoggingConfig
~~~~~~~~~~~~~

.. autoclass:: varannote.config_manager.LoggingConfig
   :members:
   :undoc-members:
   :show-inheritance:

QualityConfig
~~~~~~~~~~~~~

.. autoclass:: varannote.config_manager.QualityConfig
   :members:
   :undoc-members:
   :show-inheritance:

Configuration Examples
----------------------

Basic Configuration::

    from varannote.config_manager import ConfigManager
    
    # Load default configuration
    config = ConfigManager()
    
    # Load from file
    config = ConfigManager("config.yaml")
    
    # Access configuration values
    print(config.databases.priorities)
    print(config.performance.parallel_workers)

Environment Variables::

    import os
    
    # Set environment variables
    os.environ['VARANNOTE_CLINVAR_API_KEY'] = 'your_api_key'
    os.environ['VARANNOTE_PARALLEL_WORKERS'] = '8'
    
    # Load configuration (env vars override file values)
    config = ConfigManager("config.yaml")

User Preferences::

    # Set user preferences
    config.set_user_preference('default_output_format', 'json')
    config.set_user_preference('cache_enabled', True)
    
    # Get user preferences
    format_pref = config.get_user_preference('default_output_format')
    
    # Save preferences
    config.save_user_preferences()

Configuration Validation::

    # Validate configuration
    is_valid, errors = config.validate()
    if not is_valid:
        for error in errors:
            print(f"Configuration error: {error}")
    
    # Get configuration summary
    summary = config.get_summary()
    print(summary)

Dynamic Configuration Updates::

    # Update database priorities
    config.update_database_priority('clinvar', 1)
    config.update_database_priority('gnomad', 2)
    
    # Update performance settings
    config.update_performance_setting('parallel_workers', 6)
    config.update_performance_setting('batch_size', 100)
    
    # Save updated configuration
    config.save_to_file("updated_config.yaml")

Sample Configuration File
-------------------------

Here's a complete example of a YAML configuration file::

    # VarAnnote Configuration File
    databases:
      priorities:
        clinvar: 1
        gnomad: 2
        dbsnp: 3
        cosmic: 4
        pharmgkb: 5
      
      api_keys:
        cosmic: "your_cosmic_api_key"
        pharmgkb: "your_pharmgkb_api_key"
      
      rate_limits:
        clinvar: 10
        gnomad: 5
        dbsnp: 20
    
    performance:
      parallel_workers: 4
      batch_size: 50
      timeout: 30
      retry_attempts: 3
      retry_delay: 1.0
    
    cache:
      enabled: true
      directory: "~/.varannote/cache"
      ttl: 3600
      max_size: "1GB"
    
    output:
      default_format: "json"
      include_metadata: true
      pretty_print: true
      custom_fields:
        - "gene_symbol"
        - "clinical_significance"
        - "allele_frequency"
    
    logging:
      level: "INFO"
      file: "varannote.log"
      format: "%(asctime)s - %(name)s - %(levelname)s - %(message)s"
      max_size: "10MB"
      backup_count: 5
    
    quality:
      min_quality_score: 20
      max_allele_frequency: 0.01
      require_clinical_significance: false
      exclude_benign: true 