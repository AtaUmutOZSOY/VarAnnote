Configuration Guide
===================

VarAnnote uses a flexible configuration system that supports YAML files, environment variables, and user preferences. This guide covers all configuration options and best practices.

Configuration Overview
----------------------

VarAnnote loads configuration from multiple sources in this order of priority:

1. **Command line arguments** (highest priority)
2. **Environment variables**
3. **User configuration file** (`~/.varannote/config.yaml`)
4. **Project configuration file** (local `config.yaml`)
5. **Default configuration** (lowest priority)

Quick Configuration Setup
-------------------------

**Initialize Default Configuration**::

    # Create default config file
    varannote config init
    
    # Edit configuration
    varannote config edit
    
    # Validate configuration
    varannote config validate

**Configuration File Location**:

* Linux/macOS: `~/.varannote/config.yaml`
* Windows: `%USERPROFILE%\.varannote\config.yaml`

Complete Configuration Reference
--------------------------------

Here's a complete example configuration file with all available options:

.. code-block:: yaml

    # VarAnnote Configuration File v1.0.0
    
    # Database Configuration
    databases:
      # Database priority order (1 = highest priority)
      priorities:
        clinvar: 1
        gnomad: 2
        dbsnp: 3
        cosmic: 4
        pharmgkb: 5
        omim: 6
        gtex: 7
        encode: 8
        regulomedb: 9
      
      # API keys for databases that require authentication
      api_keys:
        cosmic: "your_cosmic_api_key_here"
        pharmgkb: "your_pharmgkb_api_key_here"
        omim: "your_omim_api_key_here"
      
      # Rate limiting (requests per second)
      rate_limits:
        clinvar: 10
        gnomad: 5
        dbsnp: 20
        cosmic: 3
        pharmgkb: 5
        omim: 2
        gtex: 10
        encode: 10
        regulomedb: 5
      
      # Timeout settings (seconds)
      timeouts:
        clinvar: 30
        gnomad: 45
        dbsnp: 30
        cosmic: 60
        pharmgkb: 30
        omim: 30
        gtex: 30
        encode: 30
        regulomedb: 30
    
    # Performance Configuration
    performance:
      # Number of parallel workers
      parallel_workers: 4
      
      # Batch size for processing variants
      batch_size: 50
      
      # Default timeout for API requests (seconds)
      timeout: 30
      
      # Number of retry attempts for failed requests
      retry_attempts: 3
      
      # Delay between retry attempts (seconds)
      retry_delay: 1.0
      
      # Maximum memory usage (MB)
      max_memory: 2048
      
      # Enable/disable parallel processing
      enable_parallel: true
    
    # Cache Configuration
    cache:
      # Enable/disable caching
      enabled: true
      
      # Cache directory
      directory: "~/.varannote/cache"
      
      # Cache time-to-live (seconds)
      ttl: 3600
      
      # Maximum cache size
      max_size: "1GB"
      
      # Cache compression
      compression: true
      
      # Cache cleanup interval (seconds)
      cleanup_interval: 86400
    
    # Output Configuration
    output:
      # Default output format
      default_format: "json"
      
      # Include metadata in output
      include_metadata: true
      
      # Pretty print JSON output
      pretty_print: true
      
      # Custom fields to include in output
      custom_fields:
        - "gene_symbol"
        - "clinical_significance"
        - "allele_frequency"
        - "consequence"
        - "transcript_id"
      
      # Field aliases
      field_aliases:
        "gene": "gene_symbol"
        "freq": "allele_frequency"
        "clinical": "clinical_significance"
      
      # Output file naming pattern
      file_naming: "{input_name}_annotated_{timestamp}"
    
    # Logging Configuration
    logging:
      # Logging level (DEBUG, INFO, WARNING, ERROR, CRITICAL)
      level: "INFO"
      
      # Log file path
      file: "varannote.log"
      
      # Log format
      format: "%(asctime)s - %(name)s - %(levelname)s - %(message)s"
      
      # Maximum log file size
      max_size: "10MB"
      
      # Number of backup log files
      backup_count: 5
      
      # Enable console logging
      console: true
      
      # Enable file logging
      file_logging: true
    
    # Quality Control Configuration
    quality:
      # Minimum quality score threshold
      min_quality_score: 20
      
      # Maximum allele frequency for rare variants
      max_allele_frequency: 0.01
      
      # Require clinical significance annotation
      require_clinical_significance: false
      
      # Exclude benign variants
      exclude_benign: true
      
      # Minimum read depth
      min_read_depth: 10
      
      # Quality filters
      filters:
        - field: "quality_score"
          operator: "greater_than"
          value: 20
        - field: "gnomad.allele_frequency"
          operator: "less_than"
          value: 0.05

Database-Specific Configuration
-------------------------------

ClinVar Configuration
~~~~~~~~~~~~~~~~~~~~~

.. code-block:: yaml

    databases:
      clinvar:
        priority: 1
        rate_limit: 10
        timeout: 30
        fields:
          - "clinical_significance"
          - "review_status"
          - "last_evaluated"
          - "submitter"
        filters:
          exclude_uncertain: true
          min_stars: 1

gnomAD Configuration
~~~~~~~~~~~~~~~~~~~~

.. code-block:: yaml

    databases:
      gnomad:
        priority: 2
        rate_limit: 5
        timeout: 45
        version: "v3.1.2"
        populations:
          - "AFR"  # African
          - "AMR"  # American
          - "ASJ"  # Ashkenazi Jewish
          - "EAS"  # East Asian
          - "FIN"  # Finnish
          - "NFE"  # Non-Finnish European
          - "SAS"  # South Asian
        fields:
          - "allele_frequency"
          - "allele_count"
          - "allele_number"
          - "homozygote_count"

COSMIC Configuration
~~~~~~~~~~~~~~~~~~~~

.. code-block:: yaml

    databases:
      cosmic:
        priority: 4
        api_key: "your_cosmic_api_key"
        rate_limit: 3
        timeout: 60
        version: "v95"
        fields:
          - "mutation_id"
          - "gene_name"
          - "mutation_description"
          - "primary_site"
          - "histology"

Environment Variables
---------------------

All configuration options can be overridden using environment variables with the prefix `VARANNOTE_`:

**Database API Keys**::

    export VARANNOTE_COSMIC_API_KEY="your_cosmic_key"
    export VARANNOTE_PHARMGKB_API_KEY="your_pharmgkb_key"
    export VARANNOTE_OMIM_API_KEY="your_omim_key"

**Performance Settings**::

    export VARANNOTE_PARALLEL_WORKERS=8
    export VARANNOTE_BATCH_SIZE=100
    export VARANNOTE_TIMEOUT=60

**Cache Settings**::

    export VARANNOTE_CACHE_ENABLED=true
    export VARANNOTE_CACHE_DIRECTORY="/tmp/varannote_cache"
    export VARANNOTE_CACHE_TTL=7200

**Output Settings**::

    export VARANNOTE_DEFAULT_FORMAT="csv"
    export VARANNOTE_PRETTY_PRINT=false

**Logging Settings**::

    export VARANNOTE_LOG_LEVEL="DEBUG"
    export VARANNOTE_LOG_FILE="/var/log/varannote.log"

Configuration Management
------------------------

**Programmatic Configuration**::

    from varannote.config_manager import ConfigManager
    
    # Load configuration
    config = ConfigManager("config.yaml")
    
    # Update settings
    config.update_database_priority("clinvar", 1)
    config.update_performance_setting("parallel_workers", 8)
    
    # Save changes
    config.save_to_file("updated_config.yaml")

**Runtime Configuration Updates**::

    from varannote import VarAnnote
    
    # Initialize with config
    annotator = VarAnnote(config_file="config.yaml")
    
    # Update configuration at runtime
    annotator.config.performance.parallel_workers = 8
    annotator.config.cache.enabled = True
    
    # Apply changes
    annotator.reload_config()

**Configuration Validation**::

    # Validate configuration file
    is_valid, errors = config.validate()
    if not is_valid:
        for error in errors:
            print(f"Configuration error: {error}")

**Configuration Templates**::

    # Generate configuration for specific use cases
    varannote config template research > research_config.yaml
    varannote config template clinical > clinical_config.yaml
    varannote config template population > population_config.yaml

User Preferences
----------------

**Setting User Preferences**::

    # Set default preferences
    varannote config set default_format json
    varannote config set parallel_workers 4
    varannote config set cache_enabled true
    
    # View current preferences
    varannote config show
    
    # Reset to defaults
    varannote config reset

**Preference Categories**:

* **Output preferences**: Default format, fields, pretty printing
* **Performance preferences**: Workers, batch size, timeouts
* **Database preferences**: Priorities, API keys, rate limits
* **Cache preferences**: Enable/disable, directory, TTL
* **Logging preferences**: Level, file location, format

Configuration Profiles
----------------------

**Creating Profiles**::

    # Create different profiles for different use cases
    varannote config profile create research
    varannote config profile create clinical
    varannote config profile create population
    
    # Switch between profiles
    varannote config profile use research
    
    # List available profiles
    varannote config profile list

**Profile Examples**:

**Research Profile**::

    # Focus on comprehensive annotation
    databases:
      priorities:
        clinvar: 1
        gnomad: 2
        cosmic: 3
        pharmgkb: 4
    performance:
      parallel_workers: 8
      batch_size: 100
    quality:
      min_quality_score: 10
      exclude_benign: false

**Clinical Profile**::

    # Focus on clinical significance
    databases:
      priorities:
        clinvar: 1
        omim: 2
        pharmgkb: 3
    quality:
      min_quality_score: 30
      require_clinical_significance: true
      exclude_benign: true

**Population Profile**::

    # Focus on population genetics
    databases:
      priorities:
        gnomad: 1
        dbsnp: 2
        gtex: 3
    quality:
      max_allele_frequency: 0.05
      min_read_depth: 20

Best Practices
--------------

**Security**:

* Store API keys in environment variables, not config files
* Use restrictive file permissions for config files (600)
* Regularly rotate API keys
* Don't commit config files with secrets to version control

**Performance**:

* Adjust parallel workers based on your system capabilities
* Use appropriate batch sizes (50-100 for most cases)
* Enable caching for repeated analyses
* Set reasonable timeouts for your network conditions

**Reliability**:

* Set appropriate retry attempts and delays
* Use database priorities to handle failures gracefully
* Monitor rate limits to avoid being blocked
* Validate configuration before running large analyses

**Organization**:

* Use different config files for different projects
* Document your configuration choices
* Use profiles for different analysis types
* Keep backup copies of working configurations

Troubleshooting Configuration
-----------------------------

**Common Issues**:

**Configuration Not Loading**::

    # Check file location
    varannote config locate
    
    # Validate syntax
    varannote config validate
    
    # Check permissions
    ls -la ~/.varannote/config.yaml

**API Key Issues**::

    # Test API keys
    varannote config test-api-keys
    
    # Check environment variables
    env | grep VARANNOTE

**Performance Issues**::

    # Check current settings
    varannote config show performance
    
    # Test with different settings
    varannote config set parallel_workers 2
    varannote config set batch_size 25

**Cache Issues**::

    # Clear cache
    varannote cache clear
    
    # Check cache status
    varannote cache status
    
    # Disable cache temporarily
    varannote config set cache_enabled false

For more advanced configuration options, see the :doc:`api/config_manager` API reference. 