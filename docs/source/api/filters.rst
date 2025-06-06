Filtering System
================

VarAnnote provides a comprehensive filtering system for variant annotation results with support for quality, clinical significance, population frequency, and custom filters.

.. automodule:: varannote.filters
   :members:
   :undoc-members:
   :show-inheritance:

Filter Classes
--------------

FilterRule
~~~~~~~~~~

.. autoclass:: varannote.filters.FilterRule
   :members:
   :undoc-members:
   :show-inheritance:
   :special-members: __init__

FilterSet
~~~~~~~~~

.. autoclass:: varannote.filters.FilterSet
   :members:
   :undoc-members:
   :show-inheritance:
   :special-members: __init__

FilterManager
~~~~~~~~~~~~~

.. autoclass:: varannote.filters.FilterManager
   :members:
   :undoc-members:
   :show-inheritance:
   :special-members: __init__

Filter Operators
----------------

The filtering system supports the following operators:

* **equals**: Exact match (==)
* **not_equals**: Not equal (!=)
* **greater_than**: Greater than (>)
* **greater_equal**: Greater than or equal (>=)
* **less_than**: Less than (<)
* **less_equal**: Less than or equal (<=)
* **contains**: String contains substring
* **not_contains**: String does not contain substring
* **starts_with**: String starts with prefix
* **ends_with**: String ends with suffix
* **regex**: Regular expression match
* **in_list**: Value is in a list
* **not_in_list**: Value is not in a list

Predefined Filter Sets
----------------------

High Confidence Variants
~~~~~~~~~~~~~~~~~~~~~~~~~

Filters for variants with high confidence annotations::

    from varannote.filters import FilterManager
    
    filter_manager = FilterManager()
    high_conf_filter = filter_manager.get_predefined_filter('high_confidence')
    
    # Apply to results
    filtered_results = filter_manager.apply_filter(results, high_conf_filter)

Rare Variants
~~~~~~~~~~~~~

Filters for rare variants (low population frequency)::

    rare_filter = filter_manager.get_predefined_filter('rare_variants')
    rare_results = filter_manager.apply_filter(results, rare_filter)

Coding Variants
~~~~~~~~~~~~~~~

Filters for variants in coding regions::

    coding_filter = filter_manager.get_predefined_filter('coding_variants')
    coding_results = filter_manager.apply_filter(results, coding_filter)

Pharmacogenomics Variants
~~~~~~~~~~~~~~~~~~~~~~~~~

Filters for pharmacogenomics-relevant variants::

    pharmaco_filter = filter_manager.get_predefined_filter('pharmacogenomics')
    pharmaco_results = filter_manager.apply_filter(results, pharmaco_filter)

Custom Filter Examples
----------------------

Basic Filtering::

    from varannote.filters import FilterRule, FilterSet, FilterManager
    
    # Create individual filter rules
    quality_filter = FilterRule(
        field="quality_score",
        operator="greater_than",
        value=20
    )
    
    frequency_filter = FilterRule(
        field="gnomad.allele_frequency",
        operator="less_than",
        value=0.01
    )
    
    # Combine filters
    filter_set = FilterSet(
        rules=[quality_filter, frequency_filter],
        logic="AND"
    )
    
    # Apply filters
    filter_manager = FilterManager()
    filtered_results = filter_manager.apply_filter(results, filter_set)

Clinical Significance Filtering::

    # Filter for pathogenic variants
    pathogenic_filter = FilterRule(
        field="clinvar.clinical_significance",
        operator="in_list",
        value=["Pathogenic", "Likely pathogenic"]
    )
    
    # Filter out benign variants
    not_benign_filter = FilterRule(
        field="clinvar.clinical_significance",
        operator="not_in_list",
        value=["Benign", "Likely benign"]
    )

Gene-Based Filtering::

    # Filter by specific genes
    gene_filter = FilterRule(
        field="gene_symbol",
        operator="in_list",
        value=["BRCA1", "BRCA2", "TP53", "EGFR"]
    )
    
    # Filter by gene region
    exon_filter = FilterRule(
        field="consequence",
        operator="contains",
        value="exon"
    )

Complex Filtering::

    # Create nested filter sets
    clinical_filters = FilterSet(
        rules=[
            FilterRule("clinvar.clinical_significance", "not_equals", "Uncertain significance"),
            FilterRule("clinvar.review_status", "contains", "criteria provided")
        ],
        logic="AND"
    )
    
    frequency_filters = FilterSet(
        rules=[
            FilterRule("gnomad.allele_frequency", "less_than", 0.05),
            FilterRule("gnomad.homozygote_count", "less_than", 10)
        ],
        logic="AND"
    )
    
    # Combine filter sets
    main_filter = FilterSet(
        rules=[clinical_filters, frequency_filters],
        logic="OR"
    )

Regular Expression Filtering::

    # Filter by variant ID pattern
    variant_id_filter = FilterRule(
        field="variant_id",
        operator="regex",
        value=r"rs\d+"
    )
    
    # Filter by gene name pattern
    gene_pattern_filter = FilterRule(
        field="gene_symbol",
        operator="regex",
        value=r"^(BRCA|TP53|EGFR)"
    )

Filter Statistics and Performance
---------------------------------

Getting Filter Statistics::

    # Apply filter and get statistics
    filtered_results, stats = filter_manager.apply_filter_with_stats(
        results, filter_set
    )
    
    print(f"Total variants: {stats['total']}")
    print(f"Filtered variants: {stats['filtered']}")
    print(f"Pass rate: {stats['pass_rate']:.2%}")
    print(f"Filter time: {stats['filter_time']:.3f}s")

Performance Optimization::

    # Enable filter caching
    filter_manager.enable_caching(cache_size=1000)
    
    # Optimize filter order (most selective first)
    optimized_filter = filter_manager.optimize_filter_order(filter_set)
    
    # Parallel filtering for large datasets
    filtered_results = filter_manager.apply_filter_parallel(
        results, filter_set, workers=4
    )

Filter Validation::

    # Validate filter syntax
    is_valid, errors = filter_manager.validate_filter(filter_set)
    if not is_valid:
        for error in errors:
            print(f"Filter error: {error}")
    
    # Test filter on sample data
    test_results = filter_manager.test_filter(filter_set, sample_data)
    print(f"Filter would match {len(test_results)} variants") 