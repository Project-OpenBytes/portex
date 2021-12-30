#################
 Primitive Types
#################

OpenBytes Schema provides a set of primitive types:

*********
 boolean
*********

The ``boolean`` type represents a binary value, only two values are supported: ``true`` and
``false``

*******
 bytes
*******

The ``bytes`` type represents a sequence of 8-bit unsigned bytes.

********
 string
********

The ``string`` type represents a sequence of UTF-8 encoded character.


.. _numeric_types:

***************
 numeric types
***************

There are four numeric types in OpenBytes Schema, they share the same parameters.

-  ``int``: 32-bit signed integer.
-  ``long``: 64-bit signed integer.
-  ``float``: single precision (32-bit) IEEE 754 floating-point number.
-  ``double``: double precision (64-bit) IEEE 754 floating-point number

There are two parameters for numeric type: ``minimum`` and ``maximum``, they are used to indicate
the range of the number.

.. list-table::
   :header-rows: 1

   -  -  name
      -  type
      -  required
      -  default
      -  description

   -  -  ``minimum``
      -  JSON number
      -  False
      -  null
      -  Satisfies ``X >= minimum``

   -  -  ``maximum``
      -  JSON number
      -  False
      -  null
      -  Satisfies ``X <= maximum``

**Examples**:

#. 32-bits signed integer:

   .. code:: yaml

      ---
      type: int

#. 32-bits signed integer range from 0 to 100:

   .. code:: yaml

      ---
      type: int
      minimum: 0
      maximum: 0

#. single precision floating-point number greater than or equal to 0:

   .. code:: yaml

      ---
      type: float
      minimum: 0
