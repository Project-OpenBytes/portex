..
   schema documentation master file, created by
   sphinx-quickstart on Mon Dec  6 15:06:05 2021.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

##########
 Overview
##########

This documentation defines Portex. It is a table-structed data definition language designed for both
structed-data and unstructed-data.

##############
 Data Storage
##############

Portex is designed for a table-structed data storage system, which has the following features:

-  Strongly typed: The type of the data in each column MUST be the same
-  Support storing the binary file
-  Support storing nested table

########
 Portex
########

Portex is a language for describing the data structure of the objects stored in the table. It
defines the name and the type of each table column. It also tells the data user how to access the
data in the table.

Portex is defined with JSON_, this doc uses ``yaml`` to represent JSON objects for better
legibility.

.. _json: https://www.json.org/json-en.html

.. toctree::
   :maxdepth: 2

   schema/basic_grammar
   schema/primitive_types
   schema/complex_types/index
   schema/type_reference
   schema/template_type

..
   Indices and tables
   ==================

   * :ref:`genindex`
   * :ref:`modindex`
   * :ref:`search`
