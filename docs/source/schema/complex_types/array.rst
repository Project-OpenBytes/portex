#######
 array
#######

The ``array`` type represents a sequence of elements which have the same type.

|  Type ``array`` has two parameters ``length`` and ``items``:
|   - ``length`` is used to indicate the length of the array.
|   - ``items`` is used to indicate the type of items in the array.

.. list-table::
   :header-rows: 1
   :widths: auto

   -  -  name
      -  type
      -  required
      -  default
      -  description

   -  -  ``length``
      -  JSON integer
      -  False
      -  null
      -  |  Represent the length of the array,
         |  used to define an array with a fixed length.

   -  -  ``items``
      -  JSON object
      -  True
      -  `-`
      -  `-`

   -  -  ``items.type``
      -  JSON string
      -  True
      -  `-`
      -  Represent the type of items in the array.

   -  -  ``items.<type-param>``
      -  `-`
      -  False
      -  `-`
      -  Represent the type parameter of items in the array.

**Examples**:

#. a natural number array with unlimited length:

   .. code:: yaml

      ---
      type: array
      items:
        type: int
        minimum: 0

#. an integer array with fixed length

   .. code:: yaml

      ---
      type: array
      length: 2
      items:
        type: int

#. a polygon represented by its vertex coordinates:

   .. code:: yaml

      ---
      type: array
      items:
        type: record
        fields:
          - name: x
            type: int

          - name: y
            type: int

   when the item type is ``record``, the behavior of an ``array`` will change to a table:

   .. note::

      ``array`` + ``record`` = ``table``

   a ``record`` can be understood as a row in the table, then ``array`` put many rows together to
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
