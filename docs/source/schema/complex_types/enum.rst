######
 enum
######

The ``enum`` type represents a value which is restricted in a fixed set of values.

The parameter ``values`` is provided for ``enum`` to indicate the set of values. It is a JSON array
with at least one element, where each element is unique.

.. list-table::
   :header-rows: 1

   -  -  name
      -  type
      -  required
      -  description

   -  -  ``values``
      -  JSON array
      -  True
      -  Contains at least one element, and each element is unique.

**Examples**:

#. enum to represent colors

   .. code:: yaml

      ---
      type: enum
      values: [red, yellow, blue]

#. enum to represent animals

   .. code:: yaml

      ---
      type: enum
      values: [dog, cat, bird]
