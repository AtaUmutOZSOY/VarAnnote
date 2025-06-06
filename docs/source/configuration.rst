Configuration Guide
===================

VarAnnote uses YAML-based configuration with environment variable support for flexible setup.

Configuration File
------------------

Default configuration file: ``config.yaml``

.. code-block:: yaml

   # Database Configuration
   databases:
     priority: ["clinvar", "gnomad", "dbsnp", "cosmic"]
     
   # API Configuration
   api:
     rate_limit: 100
     timeout: 30
     
   # Cache Configuration
   cache:
     enabled: true
     ttl: 3600
     
   # Output Configuration
   output:
     format: "json"
     include_metadata: true

Environment Variables
--------------------

Override configuration with environment variables:

.. code-block:: bash

   export VARANNOTE_API_KEY="your_api_key"
   export VARANNOTE_CACHE_TTL=7200
   export VARANNOTE_OUTPUT_FORMAT="csv"

Configuration Manager
---------------------

.. code-block:: python

   from varannote.utils.config_manager import ConfigManager
   
   # Load configuration
   config = ConfigManager()
   
   # Access settings
   api_key = config.get('api.key')
   cache_ttl = config.get('cache.ttl', default=3600)
   
   # Update settings
   config.set('output.format', 'excel')
   config.save() 