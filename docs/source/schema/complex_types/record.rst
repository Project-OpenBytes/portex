########
 record
########

The ``record`` type is where we can definte complext data stuctures by grouping related variables togather in the same place. It is simiilar to 
``struct`` in C++ or the ``Series`` in pandas. It is preferred to use `record` to hold the grouped data in each row, and a column, in portex, is a series of records of the same type.

The parameter ``fields`` is used in the ``record`` to define the member vairables. Each field should have a ``name`` and a ``type``. The ``fields`` is defined in a one dimentional array manner, so it can easily be expanded into a multi-column row.

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
      -  It is a one dimentional array. Each element in the array represents a member variable of the record. The member variables are ordered.

   -  -  ``fields.<index>``
      -  JSON object
      -  True
      -  One element in the array, which represents a member variable of the record.

   -  -  ``fields.<index>.name``
      -  JSON string
      -  True
      -  The name of the member variable.

   -  -  ``fields.<index>.type``
      -  JSON string
      -  True
      -  The type of the member variable. It does not have to be a primitive type. It could be any type defined in the context.

   -  -  ``fields.<index>.<type-param>``
      -  `-`
      -  False
      -  Type related parameters.

**Examples**:

#. a 2D point which uses ``x`` and ``y`` to represent its coordinates:

   .. code:: yaml

      ---
      type: record
      fields:
        - name: x
          type: int32

        - name: y
          type: int32

   In a tabular view:

   +----------------+----------------+
   | x              | y              |
   +================+================+
   | <x coordinate> | <y coordinate> |
   +----------------+----------------+

#. a student record which contains the basic information of a student:

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
          type: int32

        - name: student number
          type: string

   In a tabular view:

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
              type: int32

            - name: y
              type: int32

        - name: point2
          type: record
          fields:
            - name: x
              type: int32

            - name: y
              type: int32

   In a tabular view:

   +----------------+----------------+----------------+-----------------+
   | point1                          | point2                           |
   +----------------+----------------+----------------+-----------------+
   | x              | y              | x              | y               |
   +================+================+================+=================+
   | <x coordinate> | <y coordinate> | <x coordinate> | <y coordinate>  |
   +----------------+----------------+----------------+-----------------+

   This example shows the record can be nested, it can be used to support the multi-indexing feature
   in a columnar store.
