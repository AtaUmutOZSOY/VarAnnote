Filtering Guide
===============

VarAnnote provides a powerful and flexible filtering system to help you identify variants of interest. This guide covers all filtering capabilities and best practices.

Overview
--------

The filtering system allows you to:

* Filter variants by quality scores, population frequencies, and clinical significance
* Create custom filter rules with multiple operators
* Combine filters with logical operations (AND/OR)
* Use predefined filter sets for common use cases
* Apply filters to annotation results efficiently

Quick Start
-----------

**Basic Filtering Example**::

    from varannote import VarAnnote
    from varannote.filters import FilterManager, FilterRule
    
    # Annotate variants
    annotator = VarAnnote()
    results = annotator.annotate_variants(my_variants)
    
    # Create filter manager
    filter_manager = FilterManager()
    
    # Filter for pathogenic variants
    pathogenic_filter = FilterRule(
        field="clinvar.clinical_significance",
        operator="equals",
        value="Pathogenic"
    )
    
    # Apply filter
    pathogenic_variants = filter_manager.apply_filter(results, pathogenic_filter)

Filter Operators
----------------

VarAnnote supports 13 different filter operators:

**Comparison Operators**:

* ``equals``: Exact match (==)
* ``not_equals``: Not equal (!=)
* ``greater_than``: Greater than (>)
* ``greater_equal``: Greater than or equal (>=)
* ``less_than``: Less than (<)
* ``less_equal``: Less than or equal (<=)

**String Operators**:

* ``contains``: String contains substring
* ``not_contains``: String does not contain substring
* ``starts_with``: String starts with prefix
* ``ends_with``: String ends with suffix
* ``regex``: Regular expression match

**List Operators**:

* ``in_list``: Value is in a list
* ``not_in_list``: Value is not in a list

Filter Rules
------------

Filter rules are the basic building blocks of the filtering system.

**Creating Filter Rules**::

    from varannote.filters import FilterRule
    
    # Quality score filter
    quality_filter = FilterRule(
        field="quality_score",
        operator="greater_than",
        value=20
    )
    
    # Population frequency filter
    freq_filter = FilterRule(
        field="gnomad.allele_frequency",
        operator="less_than",
        value=0.01
    )
    
    # Clinical significance filter
    clinical_filter = FilterRule(
        field="clinvar.clinical_significance",
        operator="in_list",
        value=["Pathogenic", "Likely pathogenic"]
    )

**Nested Field Access**::

    # Access nested fields with dot notation
    FilterRule("gnomad.allele_frequency", "less_than", 0.01)
    FilterRule("clinvar.review_status", "contains", "criteria provided")
    FilterRule("cosmic.mutation_description", "regex", r"missense")

Filter Sets
-----------

Filter sets allow you to combine multiple filter rules with logical operations.

**Creating Filter Sets**::

    from varannote.filters import FilterSet, FilterRule
    
    # Create individual rules
    quality_rule = FilterRule("quality_score", "greater_than", 20)
    freq_rule = FilterRule("gnomad.allele_frequency", "less_than", 0.01)
    clinical_rule = FilterRule("clinvar.clinical_significance", "not_equals", "Benign")
    
    # Combine with AND logic
    filter_set = FilterSet(
        rules=[quality_rule, freq_rule, clinical_rule],
        logic="AND"
    )
    
    # Combine with OR logic
    or_filter_set = FilterSet(
        rules=[clinical_rule, freq_rule],
        logic="OR"
    )

**Nested Filter Sets**::

    # Create complex nested filters
    clinical_filters = FilterSet([
        FilterRule("clinvar.clinical_significance", "equals", "Pathogenic"),
        FilterRule("clinvar.review_status", "contains", "criteria provided")
    ], logic="AND")
    
    frequency_filters = FilterSet([
        FilterRule("gnomad.allele_frequency", "less_than", 0.05),
        FilterRule("gnomad.homozygote_count", "less_than", 10)
    ], logic="AND")
    
    # Combine filter sets
    main_filter = FilterSet([clinical_filters, frequency_filters], logic="OR")

Predefined Filter Sets
----------------------

VarAnnote includes several predefined filter sets for common use cases.

**High Confidence Variants**::

    filter_manager = FilterManager()
    high_conf_filter = filter_manager.get_predefined_filter('high_confidence')
    
    # Filters for:
    # - Quality score > 30
    # - Clinical significance not "Uncertain significance"
    # - Review status contains "criteria provided"

**Rare Variants**::

    rare_filter = filter_manager.get_predefined_filter('rare_variants')
    
    # Filters for:
    # - gnomAD allele frequency < 0.01
    # - Homozygote count < 5
    # - Not common in any population

**Coding Variants**::

    coding_filter = filter_manager.get_predefined_filter('coding_variants')
    
    # Filters for:
    # - Consequence in coding regions
    # - Excludes synonymous variants
    # - Includes missense, nonsense, frameshift

**Pharmacogenomics Variants**::

    pharmaco_filter = filter_manager.get_predefined_filter('pharmacogenomics')
    
    # Filters for:
    # - PharmGKB annotations present
    # - Drug-gene interactions
    # - Clinical annotations

**Pathogenic Variants**::

    pathogenic_filter = filter_manager.get_predefined_filter('pathogenic_variants')
    
    # Filters for:
    # - Clinical significance: Pathogenic or Likely pathogenic
    # - Excludes conflicting interpretations
    # - High confidence annotations

Applying Filters
----------------

**Basic Filter Application**::

    from varannote.filters import FilterManager
    
    filter_manager = FilterManager()
    
    # Apply single filter rule
    filtered_results = filter_manager.apply_filter(results, filter_rule)
    
    # Apply filter set
    filtered_results = filter_manager.apply_filter(results, filter_set)
    
    # Apply predefined filter
    rare_variants = filter_manager.apply_filter(results, 'rare_variants')

**Filter with Statistics**::

    # Get filtering statistics
    filtered_results, stats = filter_manager.apply_filter_with_stats(
        results, filter_set
    )
    
    print(f"Total variants: {stats['total']}")
    print(f"Filtered variants: {stats['filtered']}")
    print(f"Pass rate: {stats['pass_rate']:.2%}")
    print(f"Filter time: {stats['filter_time']:.3f}s")

**Parallel Filtering**::

    # For large datasets, use parallel filtering
    filtered_results = filter_manager.apply_filter_parallel(
        results, filter_set, workers=4
    )

Advanced Filtering Examples
---------------------------

**Gene-Based Filtering**::

    # Filter by specific genes
    gene_filter = FilterRule(
        field="gene_symbol",
        operator="in_list",
        value=["BRCA1", "BRCA2", "TP53", "EGFR", "KRAS"]
    )
    
    # Filter by gene region
    exon_filter = FilterRule(
        field="consequence",
        operator="regex",
        value=r"(exon|coding)"
    )

**Quality-Based Filtering**::

    # Multi-level quality filtering
    quality_filters = FilterSet([
        FilterRule("quality_score", "greater_than", 30),
        FilterRule("read_depth", "greater_than", 10),
        FilterRule("allele_balance", "greater_than", 0.2),
        FilterRule("strand_bias", "less_than", 0.1)
    ], logic="AND")

**Population Frequency Filtering**::

    # Rare in all populations
    pop_filters = FilterSet([
        FilterRule("gnomad.af_afr", "less_than", 0.01),
        FilterRule("gnomad.af_amr", "less_than", 0.01),
        FilterRule("gnomad.af_eas", "less_than", 0.01),
        FilterRule("gnomad.af_nfe", "less_than", 0.01),
        FilterRule("gnomad.af_sas", "less_than", 0.01)
    ], logic="AND")

**Clinical Significance Filtering**::

    # High-confidence pathogenic
    clinical_filters = FilterSet([
        FilterRule("clinvar.clinical_significance", "in_list", 
                  ["Pathogenic", "Likely pathogenic"]),
        FilterRule("clinvar.review_status", "regex", 
                  r"(criteria provided|reviewed by expert panel)"),
        FilterRule("clinvar.conflicting_interpretations", "equals", False)
    ], logic="AND")

**Consequence-Based Filtering**::

    # High-impact variants
    impact_filters = FilterSet([
        FilterRule("consequence", "in_list", [
            "stop_gained", "stop_lost", "start_lost",
            "frameshift_variant", "splice_donor_variant",
            "splice_acceptor_variant"
        ])
    ])
    
    # Missense variants with predictions
    missense_filters = FilterSet([
        FilterRule("consequence", "equals", "missense_variant"),
        FilterRule("sift_prediction", "equals", "deleterious"),
        FilterRule("polyphen_prediction", "equals", "probably_damaging")
    ], logic="AND")

Regular Expression Filtering
----------------------------

**Pattern Matching**::

    # Filter by variant ID pattern
    rs_filter = FilterRule(
        field="dbsnp.rs_id",
        operator="regex",
        value=r"^rs\d+$"
    )
    
    # Filter by gene name pattern
    gene_pattern = FilterRule(
        field="gene_symbol",
        operator="regex",
        value=r"^(BRCA|TP53|EGFR)"
    )
    
    # Filter by mutation description
    mutation_filter = FilterRule(
        field="cosmic.mutation_description",
        operator="regex",
        value=r"(missense|nonsense|frameshift)"
    )

**Complex Patterns**::

    # Filter by genomic coordinates
    coord_filter = FilterRule(
        field="variant_id",
        operator="regex",
        value=r"^(chr)?[1-9XY]\d*:\d+:[ATCG]+:[ATCG]+$"
    )
    
    # Filter by HGVS notation
    hgvs_filter = FilterRule(
        field="hgvs_notation",
        operator="regex",
        value=r"^[NM_]+\d+\.\d+:c\.\d+"
    )

Filter Validation and Testing
-----------------------------

**Validate Filter Syntax**::

    # Validate filter rules
    is_valid, errors = filter_manager.validate_filter(filter_rule)
    if not is_valid:
        for error in errors:
            print(f"Filter error: {error}")

**Test Filters on Sample Data**::

    # Test filter on sample data
    sample_data = [
        {"quality_score": 25, "clinvar": {"clinical_significance": "Pathogenic"}},
        {"quality_score": 15, "clinvar": {"clinical_significance": "Benign"}}
    ]
    
    test_results = filter_manager.test_filter(filter_set, sample_data)
    print(f"Filter would match {len(test_results)} variants")

**Filter Performance Analysis**::

    # Analyze filter performance
    performance = filter_manager.analyze_filter_performance(
        filter_set, large_dataset
    )
    
    print(f"Filter selectivity: {performance['selectivity']:.2%}")
    print(f"Average filter time: {performance['avg_time']:.3f}s")

Filter Optimization
-------------------

**Order Optimization**::

    # Optimize filter order for performance
    optimized_filter = filter_manager.optimize_filter_order(filter_set)
    
    # Most selective filters are applied first
    # Reduces processing time for large datasets

**Caching**::

    # Enable filter result caching
    filter_manager.enable_caching(cache_size=1000)
    
    # Subsequent applications of the same filter are faster

**Batch Processing**::

    # Process filters in batches for memory efficiency
    batch_size = 1000
    for batch in filter_manager.filter_in_batches(
        large_results, filter_set, batch_size
    ):
        process_batch(batch)

Custom Filter Functions
-----------------------

**Creating Custom Operators**::

    def custom_distance_operator(value, threshold, genomic_pos):
        """Custom operator for genomic distance filtering"""
        return abs(value - genomic_pos) <= threshold
    
    # Register custom operator
    filter_manager.register_operator("within_distance", custom_distance_operator)
    
    # Use custom operator
    distance_filter = FilterRule(
        field="position",
        operator="within_distance",
        value={"threshold": 1000, "genomic_pos": 43044295}
    )

**Complex Custom Filters**::

    def compound_heterozygous_filter(variant_data):
        """Custom filter for compound heterozygous variants"""
        # Custom logic for identifying compound heterozygous variants
        gene_variants = {}
        for variant in variant_data:
            gene = variant.get("gene_symbol")
            if gene:
                if gene not in gene_variants:
                    gene_variants[gene] = []
                gene_variants[gene].append(variant)
        
        # Return genes with multiple variants
        return [v for variants in gene_variants.values() 
                if len(variants) >= 2 for v in variants]

Integration with Analysis Pipelines
-----------------------------------

**With Pandas**::

    import pandas as pd
    
    # Convert to DataFrame for analysis
    df = pd.DataFrame(filtered_results)
    
    # Additional pandas filtering
    high_impact = df[df['consequence'].isin([
        'stop_gained', 'frameshift_variant'
    ])]

**With Jupyter Notebooks**::

    # Interactive filtering in notebooks
    from ipywidgets import interact, FloatSlider
    
    @interact(freq_threshold=FloatSlider(min=0, max=0.1, step=0.001, value=0.01))
    def filter_by_frequency(freq_threshold):
        freq_filter = FilterRule("gnomad.allele_frequency", "less_than", freq_threshold)
        filtered = filter_manager.apply_filter(results, freq_filter)
        print(f"Found {len(filtered)} variants with AF < {freq_threshold}")

**Export Filtered Results**::

    # Export to various formats
    filter_manager.export_filtered_results(
        filtered_results, "pathogenic_variants.csv", format="csv"
    )
    
    filter_manager.export_filtered_results(
        filtered_results, "pathogenic_variants.json", format="json"
    )

Best Practices
--------------

1. **Start with predefined filters** for common use cases
2. **Combine multiple criteria** for more specific filtering
3. **Use appropriate operators** for different data types
4. **Test filters on sample data** before applying to large datasets
5. **Optimize filter order** for better performance
6. **Cache filter results** for repeated analyses
7. **Validate filter syntax** before applying
8. **Document custom filters** for reproducibility
9. **Use parallel processing** for large datasets
10. **Monitor filter performance** and optimize as needed

Troubleshooting
---------------

**Common Issues**:

* **Field not found**: Check field names and nested access
* **Type mismatch**: Ensure value types match field types
* **Regex errors**: Validate regular expression syntax
* **Performance issues**: Optimize filter order and use caching
* **Memory errors**: Process in batches or reduce dataset size

For more advanced filtering examples, see the :doc:`examples/advanced_filtering` section. 