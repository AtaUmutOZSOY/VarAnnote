VarAnnote Annotator
==================

The main VarAnnote class provides the core functionality for variant annotation.

.. automodule:: varannote.annotator
   :members:
   :undoc-members:
   :show-inheritance:

VarAnnote Class
---------------

.. autoclass:: varannote.annotator.VarAnnote
   :members:
   :undoc-members:
   :show-inheritance:
   :special-members: __init__

Key Methods
-----------

annotate_variant
~~~~~~~~~~~~~~~~

.. automethod:: varannote.annotator.VarAnnote.annotate_variant

annotate_variants
~~~~~~~~~~~~~~~~~

.. automethod:: varannote.annotator.VarAnnote.annotate_variants

annotate_vcf
~~~~~~~~~~~~

.. automethod:: varannote.annotator.VarAnnote.annotate_vcf

Usage Examples
--------------

Basic Annotation::

    from varannote import VarAnnote
    
    # Initialize annotator
    annotator = VarAnnote()
    
    # Annotate single variant
    result = annotator.annotate_variant("1", 230710048, "A", "G")
    
    # Annotate multiple variants
    variants = [
        ("1", 230710048, "A", "G"),
        ("17", 43044295, "G", "A"),
    ]
    results = annotator.annotate_variants(variants)

VCF File Annotation::

    # Annotate VCF file
    annotator.annotate_vcf("input.vcf", "output.json", format="json")
    
    # With custom configuration
    annotator = VarAnnote(config_file="config.yaml")
    annotator.annotate_vcf("input.vcf", "output.csv", format="csv")

Parallel Processing::

    # Enable parallel processing
    annotator = VarAnnote(parallel=True, max_workers=4)
    results = annotator.annotate_variants(large_variant_list) 