CLI Usage Guide
===============

VarAnnote provides a comprehensive command-line interface for variant annotation.

Basic Commands
--------------

Annotate VCF File
~~~~~~~~~~~~~~~~~

.. code-block:: bash

   varannote annotate input.vcf --output results.json

Apply Filters
~~~~~~~~~~~~~

.. code-block:: bash

   varannote annotate input.vcf --filter high_confidence --output filtered.json

Multiple Filters
~~~~~~~~~~~~~~~~

.. code-block:: bash

   varannote annotate input.vcf --filter rare_variants,high_confidence --output results.json

Output Formats
~~~~~~~~~~~~~~

.. code-block:: bash

   # JSON output (default)
   varannote annotate input.vcf --output results.json
   
   # CSV output
   varannote annotate input.vcf --output results.csv --format csv
   
   # Excel output
   varannote annotate input.vcf --output results.xlsx --format excel

Advanced Options
----------------

Configuration File
~~~~~~~~~~~~~~~~~~

.. code-block:: bash

   varannote annotate input.vcf --config custom_config.yaml

Database Selection
~~~~~~~~~~~~~~~~~~

.. code-block:: bash

   varannote annotate input.vcf --databases clinvar,gnomad

Batch Processing
~~~~~~~~~~~~~~~~

.. code-block:: bash

   varannote batch-annotate *.vcf --output-dir results/

Help and Version
----------------

.. code-block:: bash

   # Show help
   varannote --help
   varannote annotate --help
   
   # Show version
   varannote --version 