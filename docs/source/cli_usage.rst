Command Line Interface
======================

VarAnnote provides a powerful command-line interface for variant annotation. This guide covers all CLI commands and options.

Overview
--------

The VarAnnote CLI provides several commands for different annotation tasks:

* **annotate**: Annotate variants from VCF files
* **variant**: Annotate a single variant
* **config**: Manage configuration settings
* **cache**: Manage cache operations
* **test**: Test database connections

Basic Usage
-----------

**Get Help**::

    varannote --help
    varannote annotate --help
    varannote variant --help

**Check Version**::

    varannote --version

Annotate Command
----------------

The main command for annotating variants from VCF files.

**Basic Syntax**::

    varannote annotate INPUT_FILE [OPTIONS]

**Examples**::

    # Basic annotation
    varannote annotate variants.vcf
    
    # Specify output file and format
    varannote annotate variants.vcf --output results.json --format json
    
    # Use parallel processing
    varannote annotate variants.vcf --parallel 4
    
    # Use specific databases
    varannote annotate variants.vcf --databases clinvar gnomad dbsnp
    
    # Use configuration file
    varannote annotate variants.vcf --config my_config.yaml

**Options**:

* ``--output, -o``: Output file path (default: auto-generated)
* ``--format, -f``: Output format (json, csv, tsv, vcf) (default: json)
* ``--databases, -d``: Specific databases to use (default: all)
* ``--parallel, -p``: Number of parallel workers (default: 4)
* ``--config, -c``: Configuration file path
* ``--cache/--no-cache``: Enable/disable caching (default: enabled)
* ``--verbose, -v``: Verbose output
* ``--quiet, -q``: Quiet mode (minimal output)

**Output Formats**:

* **JSON**: Structured data with full annotation details
* **CSV**: Comma-separated values for spreadsheet analysis
* **TSV**: Tab-separated values
* **VCF**: Annotated VCF with INFO fields

Variant Command
---------------

Annotate a single variant directly from the command line.

**Basic Syntax**::

    varannote variant CHROMOSOME:POSITION:REF:ALT [OPTIONS]

**Examples**::

    # Basic variant annotation
    varannote variant 1:230710048:A:G
    
    # With specific output format
    varannote variant 1:230710048:A:G --format json --pretty
    
    # Use specific databases
    varannote variant 1:230710048:A:G --databases clinvar gnomad
    
    # Save to file
    varannote variant 1:230710048:A:G --output variant_result.json

**Options**:

* ``--output, -o``: Output file path (default: stdout)
* ``--format, -f``: Output format (json, csv, tsv) (default: json)
* ``--databases, -d``: Specific databases to use
* ``--pretty``: Pretty-print JSON output
* ``--config, -c``: Configuration file path

Config Command
--------------

Manage VarAnnote configuration settings.

**Subcommands**:

* ``init``: Create default configuration file
* ``show``: Display current configuration
* ``edit``: Open configuration file in editor
* ``validate``: Validate configuration file
* ``set``: Set configuration values
* ``get``: Get configuration values
* ``reset``: Reset to default configuration

**Examples**::

    # Initialize configuration
    varannote config init
    
    # Show current configuration
    varannote config show
    
    # Edit configuration
    varannote config edit
    
    # Validate configuration
    varannote config validate
    
    # Set specific values
    varannote config set parallel_workers 8
    varannote config set default_format csv
    
    # Get specific values
    varannote config get parallel_workers
    
    # Set API keys
    varannote config set-api-key cosmic YOUR_API_KEY
    
    # Reset configuration
    varannote config reset

Cache Command
-------------

Manage VarAnnote cache operations.

**Subcommands**:

* ``status``: Show cache status and statistics
* ``clear``: Clear cache entries
* ``clean``: Clean expired cache entries
* ``info``: Show detailed cache information

**Examples**::

    # Show cache status
    varannote cache status
    
    # Clear all cache
    varannote cache clear
    
    # Clear specific database cache
    varannote cache clear --database clinvar
    
    # Clean expired entries
    varannote cache clean
    
    # Show cache info
    varannote cache info

Test Command
------------

Test database connections and functionality.

**Examples**::

    # Test all database connections
    varannote test
    
    # Test specific databases
    varannote test --databases clinvar gnomad
    
    # Test with verbose output
    varannote test --verbose
    
    # Test and save results
    varannote test --output test_results.json

Advanced Usage
--------------

**Batch Processing**::

    # Process multiple VCF files
    for file in *.vcf; do
        varannote annotate "$file" --output "${file%.vcf}_annotated.json"
    done
    
    # Using GNU parallel
    parallel varannote annotate {} --output {.}_annotated.json ::: *.vcf

**Custom Filtering**::

    # Annotate and filter in pipeline
    varannote annotate variants.vcf --format json | \
        jq '.[] | select(.clinvar.clinical_significance == "Pathogenic")'

**Configuration Profiles**::

    # Use different configs for different projects
    varannote annotate variants.vcf --config research_config.yaml
    varannote annotate variants.vcf --config clinical_config.yaml

**Performance Tuning**::

    # High-performance annotation
    varannote annotate large_file.vcf \
        --parallel 8 \
        --config high_performance.yaml \
        --format csv \
        --output results.csv

Environment Variables
---------------------

VarAnnote respects several environment variables:

**API Keys**::

    export VARANNOTE_COSMIC_API_KEY="your_cosmic_key"
    export VARANNOTE_PHARMGKB_API_KEY="your_pharmgkb_key"
    export VARANNOTE_OMIM_API_KEY="your_omim_key"

**Configuration**::

    export VARANNOTE_CONFIG_FILE="/path/to/config.yaml"
    export VARANNOTE_CACHE_DIR="/path/to/cache"
    export VARANNOTE_PARALLEL_WORKERS=8

**Output Settings**::

    export VARANNOTE_DEFAULT_FORMAT="csv"
    export VARANNOTE_OUTPUT_DIR="/path/to/output"

Exit Codes
----------

VarAnnote uses standard exit codes:

* **0**: Success
* **1**: General error
* **2**: Invalid command line arguments
* **3**: Configuration error
* **4**: Input file error
* **5**: Database connection error
* **6**: Output file error

Error Handling
--------------

**Common Errors and Solutions**:

**File Not Found**::

    Error: Input file 'variants.vcf' not found
    Solution: Check file path and permissions

**Invalid VCF Format**::

    Error: Invalid VCF format at line 42
    Solution: Validate VCF file with vcf-validator

**Database Connection Error**::

    Error: Failed to connect to ClinVar
    Solution: Check internet connection and API keys

**Configuration Error**::

    Error: Invalid configuration file
    Solution: Run 'varannote config validate' to check syntax

**Memory Error**::

    Error: Out of memory
    Solution: Reduce parallel workers or process in smaller batches

Logging and Debugging
---------------------

**Enable Verbose Logging**::

    varannote annotate variants.vcf --verbose
    
    # Or set log level
    export VARANNOTE_LOG_LEVEL=DEBUG
    varannote annotate variants.vcf

**Log to File**::

    varannote annotate variants.vcf --log-file annotation.log

**Debug Mode**::

    varannote annotate variants.vcf --debug

Integration Examples
--------------------

**With Nextflow**::

    process VARANNOTE {
        input:
        path vcf
        
        output:
        path "*.json"
        
        script:
        """
        varannote annotate ${vcf} --output ${vcf.baseName}_annotated.json
        """
    }

**With Snakemake**::

    rule annotate_variants:
        input:
            vcf="variants/{sample}.vcf"
        output:
            json="annotated/{sample}_annotated.json"
        shell:
            "varannote annotate {input.vcf} --output {output.json}"

**With Docker**::

    docker run -v $(pwd):/data varannote/varannote:latest \
        annotate /data/variants.vcf --output /data/results.json

Tips and Best Practices
------------------------

1. **Use configuration files** for consistent settings across runs
2. **Enable caching** for repeated analyses of similar variants
3. **Adjust parallel workers** based on your system capabilities
4. **Use appropriate output formats** for your downstream analysis
5. **Set API keys** as environment variables for security
6. **Monitor memory usage** for large VCF files
7. **Validate input files** before annotation
8. **Use verbose mode** for troubleshooting
9. **Keep configuration files** under version control
10. **Test database connections** before large batch jobs 