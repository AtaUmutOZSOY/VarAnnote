Quick Start Guide
=================

5-Minute Setup
--------------

1. **Install VarAnnote**

.. code-block:: bash

   pip install varannote

2. **Basic Usage**

.. code-block:: python

   from varannote.core.annotator import VariantAnnotator
   
   # Initialize annotator
   annotator = VariantAnnotator()
   
   # Annotate a VCF file
   results = annotator.annotate_vcf("your_variants.vcf")
   
   # Save results
   annotator.save_results(results, "annotated_variants.json")

3. **CLI Usage**

.. code-block:: bash

   # Annotate VCF file
   varannote annotate input.vcf --output results.json
   
   # Apply filters
   varannote annotate input.vcf --filter high_confidence --output filtered.json

Example Workflow
----------------

.. code-block:: python

   from varannote.core.annotator import VariantAnnotator
   from varannote.utils.filters import FilterSet
   
   # Create annotator
   annotator = VariantAnnotator()
   
   # Create filter for rare, high-confidence variants
   filter_set = FilterSet()
   filter_set.add_predefined("rare_variants")
   filter_set.add_predefined("high_confidence")
   
   # Annotate and filter
   results = annotator.annotate_vcf("variants.vcf", filters=filter_set)
   
   # Export to multiple formats
   annotator.export_csv(results, "results.csv")
   annotator.export_excel(results, "results.xlsx") 