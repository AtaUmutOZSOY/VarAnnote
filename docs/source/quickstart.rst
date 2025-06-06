Quick Start Guide
=================

This guide will get you up and running with VarAnnote in just a few minutes. We'll cover basic installation, configuration, and your first variant annotation.

5-Minute Setup
--------------

**Step 1: Install VarAnnote**::

    pip install varannote

**Step 2: Verify Installation**::

    varannote --version

**Step 3: Your First Annotation**::

    python -c "
    from varannote import VarAnnote
    annotator = VarAnnote()
    result = annotator.annotate_variant('1', 230710048, 'A', 'G')
    print(result)
    "

That's it! You've successfully annotated your first variant.

Basic Usage Examples
--------------------

Python API
~~~~~~~~~~

**Single Variant Annotation**::

    from varannote import VarAnnote
    
    # Initialize the annotator
    annotator = VarAnnote()
    
    # Annotate a variant (chromosome, position, ref, alt)
    result = annotator.annotate_variant("1", 230710048, "A", "G")
    
    # Print results
    print(f"Gene: {result.get('gene_symbol', 'Unknown')}")
    print(f"Clinical Significance: {result.get('clinvar', {}).get('clinical_significance', 'Unknown')}")
    print(f"Population Frequency: {result.get('gnomad', {}).get('allele_frequency', 'Unknown')}")

**Multiple Variants**::

    # List of variants to annotate
    variants = [
        ("1", 230710048, "A", "G"),    # AGT gene variant
        ("17", 43044295, "G", "A"),    # BRCA1 gene variant
        ("13", 32315474, "G", "T"),    # BRCA2 gene variant
    ]
    
    # Annotate all variants
    results = annotator.annotate_variants(variants)
    
    # Process results
    for i, result in enumerate(results):
        variant = variants[i]
        print(f"Variant {i+1}: {variant[0]}:{variant[1]} {variant[2]}>{variant[3]}")
        print(f"  Gene: {result.get('gene_symbol', 'Unknown')}")
        print(f"  Clinical: {result.get('clinvar', {}).get('clinical_significance', 'Unknown')}")
        print()

Command Line Interface
~~~~~~~~~~~~~~~~~~~~~~

**Basic VCF Annotation**::

    # Annotate a VCF file
    varannote annotate input.vcf --output results.json
    
    # Specify output format
    varannote annotate input.vcf --output results.csv --format csv
    
    # Use parallel processing
    varannote annotate input.vcf --output results.json --parallel 4

**Single Variant from Command Line**::

    # Annotate one variant
    varannote variant 1:230710048:A:G
    
    # With specific output format
    varannote variant 1:230710048:A:G --format json --pretty

Working with Different Input Formats
-------------------------------------

VCF Files
~~~~~~~~~

**Basic VCF Processing**::

    from varannote import VarAnnote
    
    annotator = VarAnnote()
    
    # Annotate VCF file
    annotator.annotate_vcf(
        input_file="variants.vcf",
        output_file="annotated_variants.json",
        format="json"
    )

**VCF with Custom Fields**::

    # Specify which fields to include in output
    annotator.annotate_vcf(
        input_file="variants.vcf",
        output_file="annotated_variants.csv",
        format="csv",
        fields=["gene_symbol", "clinical_significance", "allele_frequency"]
    )

CSV/TSV Files
~~~~~~~~~~~~~

**From Pandas DataFrame**::

    import pandas as pd
    from varannote import VarAnnote
    
    # Read variant data
    df = pd.read_csv("variants.csv")
    
    # Prepare variant list
    variants = list(zip(df['chromosome'], df['position'], df['ref'], df['alt']))
    
    # Annotate
    annotator = VarAnnote()
    results = annotator.annotate_variants(variants)
    
    # Add results to dataframe
    df['annotation'] = results
    df.to_csv("annotated_variants.csv", index=False)

Understanding the Output
------------------------

**Sample Output Structure**::

    {
        "variant_id": "1:230710048:A:G",
        "chromosome": "1",
        "position": 230710048,
        "reference": "A",
        "alternate": "G",
        "gene_symbol": "AGT",
        "gene_id": "ENSG00000135744",
        "transcript_id": "ENST00000366667",
        "consequence": "missense_variant",
        "clinvar": {
            "variant_id": "17853",
            "clinical_significance": "Pathogenic",
            "review_status": "criteria provided, multiple submitters, no conflicts",
            "last_evaluated": "2019-12-01"
        },
        "gnomad": {
            "allele_frequency": 0.0001234,
            "allele_count": 15,
            "allele_number": 121456,
            "homozygote_count": 0
        },
        "dbsnp": {
            "rs_id": "rs699",
            "variant_type": "SNV",
            "minor_allele": "G",
            "minor_allele_frequency": 0.0001
        }
    }

**Key Fields Explained**:

* **variant_id**: Unique identifier for the variant
* **gene_symbol**: HGNC gene symbol
* **consequence**: Predicted effect on protein
* **clinvar**: Clinical significance and review status
* **gnomad**: Population frequency data
* **dbsnp**: Reference SNP information

Configuration Basics
---------------------

**Create Configuration File**::

    varannote config init

This creates a default configuration file at `~/.varannote/config.yaml`.

**Basic Configuration**::

    # ~/.varannote/config.yaml
    databases:
      priorities:
        clinvar: 1      # Highest priority
        gnomad: 2
        dbsnp: 3
    
    performance:
      parallel_workers: 4
      batch_size: 50
    
    output:
      default_format: "json"
      include_metadata: true

**Using Configuration**::

    from varannote import VarAnnote
    
    # Load with custom config
    annotator = VarAnnote(config_file="my_config.yaml")
    
    # Or use default config
    annotator = VarAnnote()  # Automatically loads ~/.varannote/config.yaml

Common Use Cases
----------------

**Research Analysis**::

    from varannote import VarAnnote
    from varannote.filters import FilterManager
    
    # Initialize
    annotator = VarAnnote()
    filter_manager = FilterManager()
    
    # Annotate variants
    results = annotator.annotate_variants(my_variants)
    
    # Filter for pathogenic variants
    pathogenic_filter = filter_manager.get_predefined_filter('pathogenic_variants')
    pathogenic_results = filter_manager.apply_filter(results, pathogenic_filter)
    
    print(f"Found {len(pathogenic_results)} pathogenic variants")

**Clinical Screening**::

    # Focus on high-confidence clinical variants
    clinical_filter = filter_manager.get_predefined_filter('high_confidence')
    clinical_results = filter_manager.apply_filter(results, clinical_filter)
    
    # Export for clinical review
    annotator.export_results(clinical_results, "clinical_variants.xlsx", format="excel")

**Population Studies**::

    # Filter for rare variants
    rare_filter = filter_manager.get_predefined_filter('rare_variants')
    rare_results = filter_manager.apply_filter(results, rare_filter)
    
    # Generate summary statistics
    stats = annotator.generate_statistics(rare_results)
    print(stats)

Performance Tips
----------------

**For Large Datasets**::

    # Enable parallel processing
    annotator = VarAnnote(parallel=True, max_workers=8)
    
    # Use batch processing
    annotator.batch_size = 100
    
    # Enable caching
    annotator.enable_cache(cache_dir="/tmp/varannote_cache")

**Memory Optimization**::

    # Process in chunks for very large files
    chunk_size = 1000
    for chunk in annotator.annotate_vcf_chunks("large_file.vcf", chunk_size):
        # Process each chunk
        process_chunk(chunk)

**Network Optimization**::

    # Adjust timeout and retry settings
    annotator.set_network_config(
        timeout=60,
        retry_attempts=5,
        retry_delay=2.0
    )

Next Steps
----------

Now that you've got the basics down, explore these advanced features:

1. **Advanced Filtering**: Learn to create custom filters for your specific needs
   
   * See: :doc:`filtering`

2. **Configuration Management**: Set up database priorities and API keys
   
   * See: :doc:`configuration`

3. **Output Formats**: Explore different output options and customization
   
   * See: :doc:`output_formats`

4. **API Reference**: Dive deep into all available methods and options
   
   * See: :doc:`api/annotator`

5. **Examples**: Check out real-world usage examples
   
   * See: :doc:`examples/basic_usage`

Getting Help
------------

If you run into issues:

1. Check the error message - VarAnnote provides detailed error information
2. Verify your input format matches the expected format
3. Check your internet connection for database queries
4. Review the :doc:`installation` guide for common issues
5. Search or create an issue on `GitHub <https://github.com/varannote/varannote/issues>`_

**Common First-Time Issues**:

* **No results returned**: Check chromosome format (use "1" not "chr1")
* **Slow performance**: Enable parallel processing and caching
* **API errors**: Some databases require API keys (see :doc:`configuration`)
* **Memory issues**: Reduce batch size or process files in chunks

Happy annotating! 🧬 