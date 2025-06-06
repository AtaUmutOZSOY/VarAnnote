Output Formats Guide
====================

VarAnnote supports multiple output formats for different use cases.

Supported Formats
-----------------

JSON Format
~~~~~~~~~~~

Default format with full annotation data:

.. code-block:: python

   annotator.export_json(results, "output.json")

CSV Format
~~~~~~~~~~

Tabular format for spreadsheet applications:

.. code-block:: python

   annotator.export_csv(results, "output.csv")

TSV Format
~~~~~~~~~~

Tab-separated values:

.. code-block:: python

   annotator.export_tsv(results, "output.tsv")

Excel Format
~~~~~~~~~~~~

Microsoft Excel format with multiple sheets:

.. code-block:: python

   annotator.export_excel(results, "output.xlsx")

VCF Format
~~~~~~~~~~

Annotated VCF with INFO fields:

.. code-block:: python

   annotator.export_vcf(results, "output.vcf")

XML Format
~~~~~~~~~~

Structured XML output:

.. code-block:: python

   annotator.export_xml(results, "output.xml")

Parquet Format
~~~~~~~~~~~~~~

High-performance columnar format:

.. code-block:: python

   annotator.export_parquet(results, "output.parquet")

Format Options
--------------

Include Metadata
~~~~~~~~~~~~~~~~

.. code-block:: python

   annotator.export_json(results, "output.json", include_metadata=True)

Custom Fields
~~~~~~~~~~~~~

.. code-block:: python

   fields = ["chr", "pos", "ref", "alt", "clinvar.significance"]
   annotator.export_csv(results, "output.csv", fields=fields)

Compression
~~~~~~~~~~~

.. code-block:: python

   annotator.export_json(results, "output.json.gz", compress=True) 