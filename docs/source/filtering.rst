Filtering Guide
===============

VarAnnote provides a powerful filtering system with 13 operators and predefined filter sets.

Filter Operators
----------------

Basic Operators
~~~~~~~~~~~~~~~

* ``equals`` - Exact match
* ``not_equals`` - Not equal
* ``contains`` - Contains substring
* ``not_contains`` - Does not contain
* ``starts_with`` - Starts with string
* ``ends_with`` - Ends with string

Numeric Operators
~~~~~~~~~~~~~~~~~

* ``greater_than`` - Greater than value
* ``less_than`` - Less than value
* ``greater_equal`` - Greater than or equal
* ``less_equal`` - Less than or equal

Advanced Operators
~~~~~~~~~~~~~~~~~~

* ``regex`` - Regular expression match
* ``in_list`` - Value in list
* ``range`` - Value in range

Predefined Filter Sets
----------------------

High Confidence Variants
~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

   from varannote.utils.filters import FilterSet
   
   filter_set = FilterSet()
   filter_set.add_predefined("high_confidence")

Rare Variants
~~~~~~~~~~~~~

.. code-block:: python

   filter_set.add_predefined("rare_variants")

Coding Variants
~~~~~~~~~~~~~~~

.. code-block:: python

   filter_set.add_predefined("coding_variants")

Custom Filters
--------------

Single Filter
~~~~~~~~~~~~~

.. code-block:: python

   from varannote.utils.filters import FilterRule
   
   # Filter for pathogenic variants
   pathogenic_filter = FilterRule(
       field="clinvar.significance",
       operator="equals",
       value="Pathogenic"
   )

Multiple Filters
~~~~~~~~~~~~~~~~

.. code-block:: python

   from varannote.utils.filters import FilterSet
   
   filter_set = FilterSet()
   filter_set.add_rule("clinvar.significance", "equals", "Pathogenic")
   filter_set.add_rule("gnomad.af", "less_than", 0.01)

Filter Combinations
~~~~~~~~~~~~~~~~~~~

.. code-block:: python

   # AND combination (default)
   filter_set = FilterSet(logic="AND")
   
   # OR combination
   filter_set = FilterSet(logic="OR") 