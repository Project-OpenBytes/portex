###############
 Template Type
###############

One of the most important features in Portex is configurable type, different types provide
different parameters to adjust their behaviors.

Such as ``numeric types`` provide ``maximum`` and ``minimum``, ``record`` type provides ``fields``
etc.

************
 Parameters
************

Portex provides ``template`` type to define customized configurable types.

|  Two parameters are provided in ``template`` type:
|   - ``params`` is used to indicate the parameters.
|   - ``declaration`` is used to indicate how the parameters take effect.

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

a Point type with its coordinates data type configurable:

   .. code:: yaml

      # geometry/Point.yaml
      ---
      type: template
      params:
        dtype:
          required: false          # "dtype" is not a required parameter
          default: float           # the default value of "dtype" is "float"
          options: [float, double] # the possible values of "dtype" is "float" and "double"

      declaration:
        type: record
        fields:
          - name: x
            type: $params.dtype    # the coordinate data type depends on the input "dtype"

          - name: y
            type: $params.dtype

   after definition, this ``Point`` type can be referenced with a parameter ``dtype``:

   .. code:: yaml

      ---
      version: v1.0.0
      type: record
      fields:
        - name: point1
          type: geometry.Point
          dtype: double

        - name: point2
          type: geometry.Point
          dtype: double

   it can be visually represented in table structure:

   +----------------+----------------+----------------+-----------------+
   | point1                          | point2                           |
   +----------------+----------------+----------------+-----------------+
   | x              | y              | x              | y               |
   +================+================+================+=================+
   | <double value> | <double value> | <double value> | <double value>  |
   +----------------+----------------+----------------+-----------------+

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

*********************
 Parameter "existIf"
*********************

Portex provides a special parameter ``existIf`` to control whether a field in ``record`` exists.

When ``declaration.type`` is ``record``, the parameter ``declaration.fields.<index>.existIf`` can be
used to control whether the field exists.

.. list-table::
   :header-rows: 1
   :widths: auto

   -  -  name
      -  type
      -  required
      -  default
      -  description

   -  -  ``declaration.fields.<index>.existIf``
      -  JSON boolean
      -  False
      -  True
      -  The field exists if ``existIf`` is True, otherwise it does not exist.

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
            type: int

          - name: y
            type: int

          - name: z
            existIf: $params.dimension == 3D # When "dimension" is "3D", the "z" field exists,
                                             # this record represent a 3D point with 3 fields: x, y, z
                                             # When "dimension" is "2D", the "z" field does not exist,
                                             # this record represent a 2D point with 2 fields: x, y
            type: int

   after definition, this ``Point`` type can be referenced with a parameter ``dimension``:

   .. code:: yaml

      ---
      version: v1.0.0
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
      version: v1.0.0
      type: record
      fields:
        - name: upperCaseAnimal
          type: Animal
          upperCase: true

        - name: lowerCaseAnimal
          type: Animal
          upperCase: false
