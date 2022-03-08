#######
 array
#######

The ``array`` type represents a sequence of elements which have the same type.

|  Type ``array`` has two parameters ``items`` and ``length``:
|   - ``items`` is used to indicate the type of the items in the array.
|   - ``length`` is used to indicate the length of the array.

.. list-table::
   :header-rows: 1
   :widths: auto

   -  -  name
      -  type
      -  required
      -  default
      -  description

   -  -  ``items``
      -  JSON object
      -  True
      -  `-`
      -  `-`

   -  -  ``items.type``
      -  JSON string
      -  True
      -  `-`
      -  Represent the type of the items in the array.

   -  -  ``items.<type-param>``
      -  `-`
      -  False
      -  `-`
      -  Represent the type parameter of the items in the array.

   -  -  ``length``
      -  JSON integer
      -  False
      -  null
      -  |  Represent the length of the array,
         |  used to define an array with a fixed length.

**Examples**:

#. a natural number array with unlimited length:

   .. code:: yaml

      ---
      type: array
      items:
        type: int32
        minimum: 0

#. an integer array with fixed length:

   .. code:: yaml

      ---
      type: array
      items:
        type: int32
      length: 2

#. a polygon represented by its vertex coordinates:

   .. code:: yaml

      ---
      type: array
      items:
        type: record
        fields:
          - name: x
            type: int32

          - name: y
            type: int32

   when the item type is ``record``, the behavior of an ``array`` will change to a table:

   .. note::

      ``array`` + ``record`` = table

   A ``record`` can be understood as a row in the table, then ``array`` put many rows together to
   get a table.

   So the polygon array can be visually represented in table structure:

   +----------------+----------------+
   | x              | y              |
   +================+================+
   | <x coordinate> | <y coordinate> |
   +----------------+----------------+
   | <x coordinate> | <y coordinate> |
   +----------------+----------------+
   | <x coordinate> | <y coordinate> |
   +----------------+----------------+
   | <x coordinate> | <y coordinate> |
   +----------------+----------------+
   | <x coordinate> | <y coordinate> |
   +----------------+----------------+
   | `...`          | `...`          |
   +----------------+----------------+
