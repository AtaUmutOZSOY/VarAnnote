Output Formats
==============

VarAnnote supports multiple output formats to suit different analysis needs and downstream tools. This guide covers all available formats and customization options.

Supported Formats
-----------------

VarAnnote supports the following output formats:

* **JSON**: Structured data with full annotation details
* **CSV**: Comma-separated values for spreadsheet analysis
* **TSV**: Tab-separated values for Unix tools
* **VCF**: Annotated VCF with INFO fields
* **Excel**: Microsoft Excel format with multiple sheets
* **XML**: Structured XML format
* **Parquet**: Columnar format for big data analysis

JSON Format
-----------

JSON is the default and most comprehensive output format.

**Basic JSON Output**::

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
            "last_evaluated": "2019-12-01",
            "submitter": "ClinVar"
        },
        "gnomad": {
            "allele_frequency": 0.0001234,
            "allele_count": 15,
            "allele_number": 121456,
            "homozygote_count": 0,
            "populations": {
                "afr": 0.0002,
                "amr": 0.0001,
                "eas": 0.0000,
                "nfe": 0.0001,
                "sas": 0.0003
            }
        },
        "dbsnp": {
            "rs_id": "rs699",
            "variant_type": "SNV",
            "minor_allele": "G",
            "minor_allele_frequency": 0.0001
        },
        "annotation_metadata": {
            "timestamp": "2025-01-01T12:00:00Z",
            "version": "1.0.0",
            "databases_used": ["clinvar", "gnomad", "dbsnp"],
            "confidence_score": 0.85
        }
    }

**Pretty-Printed JSON**::

    from varannote import VarAnnote
    
    annotator = VarAnnote()
    result = annotator.annotate_variant("1", 230710048, "A", "G")
    
    # Pretty print JSON
    import json
    print(json.dumps(result, indent=2))

**JSON Configuration**::

    # In config.yaml
    output:
      default_format: "json"
      pretty_print: true
      include_metadata: true
      include_null_fields: false

CSV Format
----------

CSV format provides tabular data suitable for spreadsheet analysis.

**Basic CSV Output**::

    variant_id,chromosome,position,reference,alternate,gene_symbol,clinical_significance,allele_frequency
    1:230710048:A:G,1,230710048,A,G,AGT,Pathogenic,0.0001234
    17:43044295:G:A,17,43044295,G,A,BRCA1,Pathogenic,0.0000567

**Custom Field Selection**::

    from varannote import VarAnnote
    
    annotator = VarAnnote()
    
    # Specify custom fields for CSV output
    custom_fields = [
        "variant_id",
        "gene_symbol", 
        "clinvar.clinical_significance",
        "gnomad.allele_frequency",
        "dbsnp.rs_id"
    ]
    
    annotator.annotate_vcf(
        "input.vcf",
        "output.csv",
        format="csv",
        fields=custom_fields
    )

**CSV Configuration**::

    # In config.yaml
    output:
      csv:
        delimiter: ","
        quote_char: '"'
        include_header: true
        custom_fields:
          - "variant_id"
          - "gene_symbol"
          - "clinvar.clinical_significance"
          - "gnomad.allele_frequency"

TSV Format
----------

Tab-separated values format, ideal for Unix command-line tools.

**Basic TSV Output**::

    variant_id	chromosome	position	reference	alternate	gene_symbol	clinical_significance
    1:230710048:A:G	1	230710048	A	G	AGT	Pathogenic
    17:43044295:G:A	17	43044295	G	A	BRCA1	Pathogenic

**Command Line Usage**::

    # Generate TSV output
    varannote annotate variants.vcf --format tsv --output results.tsv
    
    # Use with Unix tools
    cut -f1,6,7 results.tsv | grep "Pathogenic"
    awk -F'\t' '$7=="Pathogenic" {print $1, $6}' results.tsv

VCF Format
----------

Annotated VCF format with annotations in INFO fields.

**Annotated VCF Output**::

    ##fileformat=VCFv4.2
    ##INFO=<ID=GENE,Number=1,Type=String,Description="Gene symbol">
    ##INFO=<ID=CLNSIG,Number=1,Type=String,Description="ClinVar clinical significance">
    ##INFO=<ID=AF,Number=1,Type=Float,Description="gnomAD allele frequency">
    ##INFO=<ID=RS,Number=1,Type=String,Description="dbSNP rs ID">
    #CHROM	POS	ID	REF	ALT	QUAL	FILTER	INFO
    1	230710048	.	A	G	.	.	GENE=AGT;CLNSIG=Pathogenic;AF=0.0001234;RS=rs699
    17	43044295	.	G	A	.	.	GENE=BRCA1;CLNSIG=Pathogenic;AF=0.0000567;RS=rs80357906

**VCF Configuration**::

    # In config.yaml
    output:
      vcf:
        info_fields:
          GENE: "gene_symbol"
          CLNSIG: "clinvar.clinical_significance"
          AF: "gnomad.allele_frequency"
          RS: "dbsnp.rs_id"
        include_original_info: true

Excel Format
------------

Microsoft Excel format with multiple sheets for different data types.

**Excel Output Structure**::

    # Sheet 1: Variants
    variant_id | gene_symbol | clinical_significance | allele_frequency
    
    # Sheet 2: Summary
    total_variants | pathogenic_count | benign_count | uncertain_count
    
    # Sheet 3: Database_Stats
    database | queries_made | success_rate | avg_response_time

**Generate Excel Output**::

    annotator.annotate_vcf(
        "variants.vcf",
        "results.xlsx",
        format="excel",
        include_summary=True,
        include_stats=True
    )

XML Format
----------

Structured XML format for integration with XML-based systems.

**XML Output Example**::

    <?xml version="1.0" encoding="UTF-8"?>
    <varannote_results>
        <metadata>
            <version>1.0.0</version>
            <timestamp>2025-01-01T12:00:00Z</timestamp>
        </metadata>
        <variants>
            <variant>
                <variant_id>1:230710048:A:G</variant_id>
                <chromosome>1</chromosome>
                <position>230710048</position>
                <reference>A</reference>
                <alternate>G</alternate>
                <gene_symbol>AGT</gene_symbol>
                <clinvar>
                    <clinical_significance>Pathogenic</clinical_significance>
                    <review_status>criteria provided</review_status>
                </clinvar>
                <gnomad>
                    <allele_frequency>0.0001234</allele_frequency>
                </gnomad>
            </variant>
        </variants>
    </varannote_results>

Parquet Format
--------------

Columnar format optimized for big data analysis and analytics.

**Generate Parquet Output**::

    annotator.annotate_vcf(
        "large_variants.vcf",
        "results.parquet",
        format="parquet",
        compression="snappy"
    )

**Read Parquet with Pandas**::

    import pandas as pd
    
    # Read parquet file
    df = pd.read_parquet("results.parquet")
    
    # Analyze data
    pathogenic_variants = df[df['clinvar.clinical_significance'] == 'Pathogenic']

Custom Output Fields
--------------------

**Field Selection**::

    # Select specific fields for output
    custom_fields = [
        "variant_id",
        "chromosome", 
        "position",
        "gene_symbol",
        "clinvar.clinical_significance",
        "clinvar.review_status",
        "gnomad.allele_frequency",
        "gnomad.homozygote_count",
        "dbsnp.rs_id",
        "cosmic.mutation_id"
    ]
    
    annotator.annotate_vcf(
        "input.vcf",
        "output.csv",
        format="csv",
        fields=custom_fields
    )

**Field Aliases**::

    # Use friendly field names
    field_aliases = {
        "clinvar.clinical_significance": "clinical_sig",
        "gnomad.allele_frequency": "population_freq",
        "dbsnp.rs_id": "rs_number"
    }
    
    annotator.annotate_vcf(
        "input.vcf",
        "output.csv",
        format="csv",
        field_aliases=field_aliases
    )

**Nested Field Flattening**::

    # Flatten nested structures
    flattened_fields = [
        "variant_id",
        "gene_symbol",
        "clinvar_clinical_significance",  # Flattened from clinvar.clinical_significance
        "clinvar_review_status",          # Flattened from clinvar.review_status
        "gnomad_allele_frequency",        # Flattened from gnomad.allele_frequency
        "gnomad_af_afr",                  # Flattened from gnomad.populations.afr
        "gnomad_af_eas"                   # Flattened from gnomad.populations.eas
    ]

Output Customization
--------------------

**Metadata Inclusion**::

    # Include annotation metadata
    annotator.annotate_vcf(
        "input.vcf",
        "output.json",
        include_metadata=True,
        metadata_fields=[
            "timestamp",
            "version", 
            "databases_used",
            "confidence_score",
            "processing_time"
        ]
    )

**Null Value Handling**::

    # Configure null value handling
    output_config = {
        "include_null_fields": False,
        "null_replacement": "N/A",
        "empty_string_replacement": "Unknown"
    }

**Data Type Formatting**::

    # Configure data type formatting
    format_config = {
        "float_precision": 6,
        "scientific_notation": False,
        "date_format": "%Y-%m-%d",
        "boolean_format": {"true": "Yes", "false": "No"}
    }

Batch Output Processing
-----------------------

**Multiple Format Output**::

    # Generate multiple formats simultaneously
    formats = ["json", "csv", "vcf"]
    
    for fmt in formats:
        output_file = f"results.{fmt}"
        annotator.annotate_vcf(
            "input.vcf",
            output_file,
            format=fmt
        )

**Chunked Output**::

    # Process large files in chunks
    chunk_size = 1000
    
    for i, chunk in enumerate(annotator.annotate_vcf_chunks("large.vcf", chunk_size)):
        output_file = f"results_chunk_{i}.json"
        annotator.save_results(chunk, output_file, format="json")

**Streaming Output**::

    # Stream results for memory efficiency
    with annotator.stream_annotation("input.vcf") as stream:
        with open("output.jsonl", "w") as f:
            for variant in stream:
                f.write(json.dumps(variant) + "\n")

Format-Specific Options
-----------------------

**JSON Options**::

    json_options = {
        "indent": 2,
        "sort_keys": True,
        "ensure_ascii": False,
        "separators": (",", ": ")
    }

**CSV Options**::

    csv_options = {
        "delimiter": ",",
        "quotechar": '"',
        "quoting": "minimal",
        "lineterminator": "\n",
        "escapechar": "\\"
    }

**VCF Options**::

    vcf_options = {
        "include_header": True,
        "info_prefix": "VA_",
        "format_fields": True,
        "preserve_original": True
    }

**Excel Options**::

    excel_options = {
        "sheet_names": {
            "variants": "Variant_Annotations",
            "summary": "Analysis_Summary", 
            "stats": "Database_Statistics"
        },
        "freeze_panes": (1, 0),
        "auto_filter": True,
        "column_width": "auto"
    }

Command Line Usage
------------------

**Basic Format Selection**::

    # JSON output (default)
    varannote annotate variants.vcf --output results.json
    
    # CSV output
    varannote annotate variants.vcf --format csv --output results.csv
    
    # TSV output
    varannote annotate variants.vcf --format tsv --output results.tsv
    
    # VCF output
    varannote annotate variants.vcf --format vcf --output annotated.vcf

**Custom Field Selection**::

    # Specify custom fields
    varannote annotate variants.vcf \
        --format csv \
        --fields variant_id gene_symbol clinvar.clinical_significance \
        --output custom_results.csv

**Format Options**::

    # JSON with pretty printing
    varannote annotate variants.vcf \
        --format json \
        --pretty \
        --output results.json
    
    # CSV with custom delimiter
    varannote annotate variants.vcf \
        --format csv \
        --delimiter ";" \
        --output results.csv

Integration Examples
--------------------

**With Pandas**::

    import pandas as pd
    
    # Read different formats
    json_df = pd.read_json("results.json")
    csv_df = pd.read_csv("results.csv")
    parquet_df = pd.read_parquet("results.parquet")
    
    # Analyze data
    summary = csv_df.groupby('clinvar.clinical_significance').size()

**With R**::

    # Read CSV in R
    library(readr)
    variants <- read_csv("results.csv")
    
    # Read JSON in R
    library(jsonlite)
    variants_json <- fromJSON("results.json")

**With Spark**::

    from pyspark.sql import SparkSession
    
    spark = SparkSession.builder.appName("VarAnnote").getOrCreate()
    
    # Read Parquet with Spark
    df = spark.read.parquet("results.parquet")
    df.show()

**With Databases**::

    import sqlite3
    import pandas as pd
    
    # Load CSV into SQLite
    df = pd.read_csv("results.csv")
    conn = sqlite3.connect("variants.db")
    df.to_sql("variants", conn, if_exists="replace")

Performance Considerations
--------------------------

**Format Performance Comparison**:

* **JSON**: Moderate write speed, excellent for complex data
* **CSV**: Fast write speed, good for simple tabular data
* **TSV**: Fastest write speed, minimal overhead
* **VCF**: Moderate speed, standard genomics format
* **Parquet**: Slower write, excellent for analytics
* **Excel**: Slowest write, good for presentations

**Memory Usage**:

* **Streaming formats**: JSON Lines, CSV, TSV
* **Memory-efficient**: Parquet, compressed JSON
* **Memory-intensive**: Excel, XML

**Compression Options**::

    # Enable compression for large outputs
    annotator.annotate_vcf(
        "input.vcf",
        "output.json.gz",
        format="json",
        compression="gzip"
    )
    
    # Parquet with compression
    annotator.annotate_vcf(
        "input.vcf", 
        "output.parquet",
        format="parquet",
        compression="snappy"
    )

Best Practices
--------------

1. **Choose appropriate formats** for your use case
2. **Use field selection** to reduce output size
3. **Enable compression** for large datasets
4. **Use streaming** for memory-constrained environments
5. **Validate output** before downstream processing
6. **Document field mappings** for reproducibility
7. **Consider downstream tools** when selecting formats
8. **Use batch processing** for large-scale analyses
9. **Monitor performance** and optimize as needed
10. **Keep format configurations** under version control

Troubleshooting
---------------

**Common Issues**:

* **Large file sizes**: Use field selection and compression
* **Memory errors**: Use streaming or chunked processing
* **Format compatibility**: Check downstream tool requirements
* **Encoding issues**: Specify UTF-8 encoding
* **Performance problems**: Use appropriate format for use case

For more output examples, see the :doc:`examples/basic_usage` section. 