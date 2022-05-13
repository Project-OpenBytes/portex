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

***************
 numeric types
***************

There are four numeric types in Portex, they share the same parameters.

-  ``int32``: 32-bit signed integer.
-  ``int64``: 64-bit signed integer.
-  ``float32``: single precision (32-bit) IEEE 754 floating-point number.
-  ``float64``: double precision (64-bit) IEEE 754 floating-point number

**Examples**:

#. 32-bits signed integer:

   .. code:: yaml

      ---
      type: int32

#. single precision floating-point number:

   .. code:: yaml

      ---
      type: float32
