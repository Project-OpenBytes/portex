###############
 Template Type
###############

One of the most important features in Portex is configurable type, different types provide different
parameters to adjust their behaviors.

Such as ``numeric types`` provide ``maximum`` and ``minimum``, ``record`` type provides ``fields``
etc.

************
 Parameters
************

Portex provides ``template`` type to define customized configurable types.

|  Two parameters are provided in ``template`` type:
|     - ``params`` is used to indicate the parameters.
|     - ``declaration`` is used to indicate how the parameters take effect.

.. list-table::
   :header-rows: 1
   :widths: auto

   -  -  name
      -  type
      -  required
      -  description

   -  -  ``params``
      -  |  JSON
         |  object
      -  True
      -  Indicate all the parameters for this template.

   -  -  ``params.<name>``
      -  |  JSON
         |  object
      -  True
      -  |  Every element in ``params`` defines a parameter:
         |  - <key> is name of the parameter
         |  - <value> is the basic info of the parameter

   -  -  ``params.<name>.required``
      -  |  JSON
         |  string
      -  True
      -  Whether this parameter is required.

   -  -  ``params.<name>.default``
      -  `-`
      -  False
      -  The default value of this parameter. It is meaningless when this parameter is required.

   -  -  ``params.<name>.options``
      -  |  JSON
         |  array
      -  False
      -  |  This parameter uses an array to list all possible values, if the input parameter value
         |  is not listed in the array, it will not be accepted.

   -  -  ``declaration``
      -  |  JSON
         |  object
      -  True
      -  |  The declaration of template, use ``$params.<name>`` to indicate how different
         |  parameters take effect in the template.

   -  -  ``declaration.type``
      -  |  JSON
         |  string
      -  True
      -  The actual type of the template.

   -  -  ``declaration.<type-param>``
      -  `-`
      -  True
      -  The parameters of the actual type.

**Examples**:

#. A 2D point type:

   .. code:: yaml

      # geometry/Point.yaml
      ---
      type: template
      declaration:
        type: record
        fields:
          - name: x
            type: int32

          - name: y
            type: int32

   after definition, this ``Point`` type can be referenced:

   .. code:: yaml

      ---
      type: record
      fields:
        - name: point1
          type: geometry.Point

        - name: point2
          type: geometry.Point

   it can be visually represented in table structure:

   +---------------+---------------+---------------+---------------+
   | point1                        | point2                        |
   +---------------+---------------+---------------+---------------+
   | x             | y             | x             | y             |
   +===============+===============+===============+===============+
   | <int32 value> | <int32 value> | <int32 value> | <int32 value> |
   +---------------+---------------+---------------+---------------+

#. A 2D point type with configurable label:

   .. code:: yaml

      # geometry/LabeledPoint.yaml
      ---
      type: template
      params:
        labels:
          required: true               # "labels" is a required parameter

      declaration:
        type: record
        fields:
          - name: x
            type: int32

          - name: y
            type: int32

          - name: label
            type: enum
            values: $params.labels     # the values of enums depends on the input "labels"

   after definition, this ``LabeledPoint`` type can be referenced:

   .. code:: yaml

      ---
      type: record
      fields:
        - name: labeled_point
          type: geometry.LabeledPoint
          values: ["visble", "occluded"]

   it can be visually represented in table structure:

   +---------------+---------------+--------------------------+
   | labeled_point                                            |
   +---------------+---------------+--------------------------+
   | x             | y             | label                    |
   +===============+===============+==========================+
   | <int32 value> | <int32 value> | <"visble" or "occluded"> |
   +---------------+---------------+--------------------------+

****************
 Unpack Grammar
****************

Portex provides unpack grammar for JSON object and JSON array in template type.

Object unpack
=============

Portex use ``+`` symbol for object unpack, it is used to unpack the JSON object parameter and merge
it into another JSON object.

This grammar is used to create the template type whose internal type is configurable. Just like the
builtin ``array`` type, the type of the array elements can be configured by its ``items`` parameter

.. note::

   Portex object unpack is similar with `YAML merge grammar`_.

.. _yaml merge grammar: https://yaml.org/type/merge.html

**Examples**:

#. A 2D point type with configurable coordinate type:

   .. code:: yaml

      # geometry/Point.yaml
      ---
      type: template
      params:
        coords:
          required: false             # "coords" is not a required parameter
          default:
            type: int32               # the default value of "coords" is '{"type": "int32"}'

      declaration:
        type: record
        fields:
          - name: x
            +: $params.coords          # use object unpack symbol "+" to unpack $params.coords
                                       # which makes the coordinate type configurable
                                       # params.coords should be a JSON object

          - name: y
            +: $params.coords

   after definition, this ``Point`` type can be referenced with a parameter ``coords``:

   .. code:: yaml

      ---
      type: record
      fields:
        - name: point1
          type: geometry.Point
          coords:
            type: float32         # set the coordinate type to "float32"

        - name: point2
          type: geometry.Point    # use the default type "int32"

   it can be visually represented in table structure:

   +-----------------+-----------------+---------------+---------------+
   | point1                            | point2                        |
   +-----------------+-----------------+---------------+---------------+
   | x               | y               | x             | y             |
   +=================+=================+===============+===============+
   | <float32 value> | <float32 value> | <int32 value> | <int32 value> |
   +-----------------+-----------------+---------------+---------------+

************
 Expression
************

Variable and Constant
=====================

-  **Variable**: The symbol ``$`` is used to indicate variables, use ``$params.<name>`` to expand
   the input parameter in the template.
-  **Constant**: The JSON value is used to indicate constants. For example, use ``0``, ``20.5`` to
   represent numbers, use "cat", "dog" to represent strings.

Conditional Statement
=====================

Portex provides the following basic conditional binary operators:

-  ``==``
-  ``!=``
-  ``>=``
-  ``<=``
-  ``>``
-  ``<``

Put variables or constants on the left and right side of these operators to get a conditional
statement which returns a bool value.

**Examples**:

-  ``$params.length < 100``
-  ``$params.name == "cat"``

**********************
 Parameter "exist_if"
**********************

Portex provides a special parameter ``exist_if`` to control whether a field in ``record`` exists.

When ``declaration.type`` is ``record``, the parameter ``declaration.fields.<index>.exist_if`` can
be used to control whether the field exists.

.. list-table::
   :header-rows: 1
   :widths: auto

   -  -  name
      -  type
      -  required
      -  default
      -  description

   -  -  ``declaration.fields.<index>.exist_if``
      -  JSON boolean
      -  False
      -  True
      -  The field exists if ``exist_if`` is True, otherwise it does not exist.

**Examples**:

a Point type which can be configured to be 2D or 3D:

   .. code:: yaml

      # geometry/Point.yaml
      ---
      type: template
      params:
        dimension:
          required: true
          options: [2D, 3D]

      declaration:
        type: record
        fields:
          - name: x
            type: int32

          - name: y
            type: int32

          - name: z
            exist_if: $params.dimension == "3D" # When "dimension" is "3D", the "z" field exists,
                                                # this record represent a 3D point with 3 fields: x, y, z
                                                # When "dimension" is "2D", the "z" field does not exist,
                                                # this record represent a 2D point with 2 fields: x, y
            type: int32

   after definition, this ``Point`` type can be referenced with a parameter ``dimension``:

   .. code:: yaml

      ---
      type: record
      fields:
        - name: point2D
          type: geometry.Point
          dimension: 2D

        - name: point3D
          type: geometry.Point
          dimension: 3D

   it can be visually represented in table structure:

   +----------------+----------------+----------------+-----------------+-----------------+
   | point2D                         | point3D                                            |
   +----------------+----------------+----------------+-----------------+-----------------+
   | x              | y              | x              | y               | z               |
   +================+================+================+=================+=================+
   | <x coordinate> | <y coordinate> | <x coordinate> | <y coordinate>  | <z coordinate>  |
   +----------------+----------------+----------------+-----------------+-----------------+

**************
 If Statement
**************

Portex provides ``if-then-else`` for if statement.

Grammar:

.. code:: yaml

   if: <expression>
   then:
     <the branch when the expression is True>
   else:
     <the branch when the expression is False>

**Examples**:

an animal enum type which the letter case of the value is configuable:

   .. code:: yaml

      # Animal.yaml
      ---
      type: template
      params:
        upperCase:
          required: true
          options: [true, false]

        declaration:
          type: enum
          values:
            if: $params.upperCase == true
            then: [CAT, DOG, BIRD]
            else: [cat, dog, bird]

   after definition, this ``Animal`` type can be referenced with a parameter ``upperCase``:

   .. code:: yaml

      ---
      type: record
      fields:
        - name: upperCaseAnimal
          type: Animal
          upperCase: true

        - name: lowerCaseAnimal
          type: Animal
          upperCase: false
