#################
 Primitive Types
#################

Portex provides a set of primitive types:

*********
 boolean
*********

The ``boolean`` type represents a binary value, only two values are supported: ``true`` and
``false``

********
 binary
********

The ``binary`` type represents a sequence of 8-bit unsigned binary.

********
 string
********

The ``string`` type represents a sequence of UTF-8 encoded characters.


.. _numeric_types:

***************
 numeric types
***************

There are four numeric types in Portex, they share the same parameters.

-  ``int32``: 32-bit signed integer.
-  ``int64``: 64-bit signed integer.
-  ``float32``: single precision (32-bit) IEEE 754 floating-point number.
-  ``float64``: double precision (64-bit) IEEE 754 floating-point number

There are two parameters for numeric type: ``minimum`` and ``maximum``, they are used to indicate
the range of the number.

.. list-table::
   :header-rows: 1
   :widths: auto

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
      type: int32

#. 32-bits signed integer range from 0 to 100:

   .. code:: yaml

      ---
      type: int32
      minimum: 0
      maximum: 100

#. single precision floating-point number no less than 0:

   .. code:: yaml

      ---
      type: float32
      minimum: 0
