########
 record
########

The ``record`` type represents a labeled array capable of holding any data type, just like the
struct in C++ or the Series in pandas.

The parameter ``fields`` is provided for the ``record`` to indicate the label name and the
corresponding type.

.. list-table::
   :header-rows: 1
   :widths: auto

   -  -  name
      -  type
      -  required
      -  description

   -  -  ``fields``
      -  JSON array
      -  True
      -  |  Each element in the array represents a field of the record,
         |  and the element order indicates the order of the fields.

   -  -  ``fields.<index>``
      -  JSON object
      -  True
      -  |  A JSON object to represent the label name and the type
         |  of a record field.

   -  -  ``fields.<index>.name``
      -  JSON string
      -  True
      -  Represent the label name of a record field.

   -  -  ``fields.<index>.type``
      -  JSON string
      -  True
      -  Represent the type of a record field.

   -  -  ``fields.<index>.<type-param>``
      -  `-`
      -  False
      -  Represent the type parameter of a record field.

**Examples**:

#. a 2D point which uses ``x`` and ``y`` to represent its coordinate:

   .. code:: yaml

      ---
      type: record
      fields:
        - name: x
          type: int

        - name: y
          type: int

   it can be visually represented in table structure:

   +----------------+----------------+
   | x              | y              |
   +================+================+
   | <x coordinate> | <y coordinate> |
   +----------------+----------------+

#. a student record which contains the information of a student:

   .. code:: yaml

      ---
      type: record
      fields:
        - name: name
          type: string

        - name: gender
          type: enum
          values: [male, female, other]

        - name: age
          type: int
          minimum: 0

        - name: student number
          type: string

   it can be visually represented in table structure:

   +----------------+------------------+---------------+------------------+
   | name           | gender           | age           | student number   |
   +================+==================+===============+==================+
   | <student name> | <student gender> | <student age> | <student number> |
   +----------------+------------------+---------------+------------------+

#. a 2D line which is represented by two 2D point coordinates:

   .. code:: yaml

      ---
      type: record
      fields:
        - name: point1
          type: record
          fields:
            - name: x
              type: int

            - name: y
              type: int

        - name: point2
          type: record
          fields:
            - name: x
              type: int

            - name: y
              type: int

   it can be visually represented in table structure:

   +----------------+----------------+----------------+-----------------+
   | point1                          | point2                           |
   +----------------+----------------+----------------+-----------------+
   | x              | y              | x              | y               |
   +================+================+================+=================+
   | <x coordinate> | <y coordinate> | <x coordinate> | <y coordinate>  |
   +----------------+----------------+----------------+-----------------+

   This example shows the record can be nested, it can be used to support the multi-indexing feature
   in table structure.
