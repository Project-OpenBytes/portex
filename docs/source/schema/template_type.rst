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
|     - ``parameters`` is used to indicate the parameters.
|     - ``declaration`` is used to indicate how the parameters take effect.

.. list-table::
   :header-rows: 1
   :widths: auto

   -  -  name
      -  type
      -  required
      -  description

   -  -  ``parameters``
      -  |  JSON
         |  array
      -  False
      -  Indicate all the parameters for this template.

   -  -  ``parameters.<index>``
      -  |  JSON
         |  object
      -  True
      -  |  Each element in ``parameters`` defines a parameter.

   -  -  ``parameters.<index>.name``
      -  |  JSON
         |  string
      -  True
      -  The name of the parameter.

   -  -  ``parameters.<index>.default``
      -  `-`
      -  False
      -  | The default value of the parameter.
         | The default value is set -> This is a optional parameter.
         | The default value is not set -> This is a required parameter.

   -  -  ``parameters.<index>.options``
      -  |  JSON
         |  array
      -  False
      -  |  This parameter uses an array to list all possible values, if the input parameter value
         |  is not listed in the array, it will not be accepted.

   -  -  ``declaration``
      -  |  JSON
         |  object
      -  True
      -  |  The declaration of template, use ``$<name>`` to indicate how different
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
      parameters:
        - name: labels                 # "labels" is a required parameter

      declaration:
        type: record
        fields:
          - name: x
            type: int32

          - name: y
            type: int32

          - name: label
            type: enum
            values: $labels             # the values of enums depends on the input "labels"

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

.. error::

   Setting the type name as a parameter, as shown in the following example, is not allowed in
   Portex.

   .. code:: yaml

      # geometry/Point.yaml
      ---
      type: template
      parameters:
        - name: coords
          default: int32          # $coords represent the name of the type

      declaration:
        type: record
        fields:
          - name: x
            type: $coords         # The type name should be put after keyword "type:"
                                  # set the type name as parameter is not allowed in Portex

          - name: y
            type: $coords

.. note::

   Check the :ref:`object unpack <object_unpack>` grammar for creating a template type with
   configurable internal types.

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
      -  required
      -  default
      -  description

   -  -  ``declaration.fields.<index>.exist_if``
      -  False
      -  True
      -  |  The field exists if the value of ``exist_if`` is not ``null``,
         |  otherwise it does not exist.

**Examples**:

a Point type with or without a enum label:

   .. code:: yaml

      # geometry/Point.yaml
      ---
      type: template
      parameters:
        - name: labels
          default: null

      declaration:
        type: record
        fields:
          - name: x
            type: int32

          - name: y
            type: int32

          - name: label
            exist_if: $labels              # When "labels" is not "null", the "label" field exists,
            type: enum
            values: $labels

   after definition, this ``Point`` type can be referenced with a parameter ``labels``:

   .. code:: yaml

      ---
      type: record
      fields:
        - name: point
          type: geometry.Point

        - name: labeled_point
          type: geometry.Point
          labels: ["visble", "occluded"]

   it can be visually represented in table structure:

   +---------------+---------------+---------------+----------------+---------------------------+
   | point                         | labeled_point                                              |
   +---------------+---------------+---------------+----------------+---------------------------+
   | x             | y             | x             | y              | label                     |
   +===============+===============+===============+================+===========================+
   | <int32 value> | <int32 value> | <int32 value> | <int32 value>  | <"visble" or "occluded">  |
   +---------------+---------------+---------------+----------------+---------------------------+

****************
 Unpack Grammar
****************

Portex provides unpack grammar for JSON object and JSON array in template type.

.. _object_unpack:

Object unpack
=============

Portex use ``+`` symbol for object unpack, it is used to unpack the JSON object parameter and merge
it into another JSON object.

This grammar is used to create the template type whose internal type is configurable. Just like the
builtin :doc:`/schema/complex_types/array` type, the type of the array elements can be configured by
its ``items`` parameter

.. note::

   Portex object unpack is similar with `YAML merge grammar`_.

.. _yaml merge grammar: https://yaml.org/type/merge.html

**Examples**:

#. A 2D point type with configurable coordinate type:

   .. code:: yaml

      # geometry/Point.yaml
      ---
      type: template
      parameters:
        - name: coords
          default:                    # "coords" is not a required parameter
            type: int32               # the default value of "coords" is '{"type": "int32"}'

      declaration:
        type: record
        fields:
          - name: x
            +: $coords               # use object unpack symbol "+" to unpack $coords
                                     # which makes the coordinate type configurable
                                     # $coords should be a JSON object

          - name: y
            +: $coords

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

Array unpack
============

Portex also use ``+`` symbol for array unpack. The grammar ``+$<name>`` is used to unpack the
JSON array parameter and merge it into another JSON array.

This grammar can be used to extend the record fields.

**Examples**:

#. A 2D point type with extensible fields:

   .. code:: yaml

      # geometry/Point.yaml
      ---
      type: template
      parameters:
        - name: extra
          default: []        # the default value is an empty array, which means add no fields

      declaration:
        type: record
        fields:
          - name: x
            type: int32

          - name: y
            type: int32

          - +$extra          # use "+$<name>" grammar to unpack the parameter "extra"
                             # which makes the record fields extensible
                             # $extra should be a JSON array

   after definition, this ``Point`` type can be referenced with a parameter ``extra``:

   .. code:: yaml

      ---
      type: record
      fields:
        - name: point1
          type: geometry.Point
          extra:
            - name: label         # set "label" as a extra field
              type: enum
              values: ["visble", "occluded"]

        - name: point2
          type: geometry.Point    # the default behavior is no extra field

   it can be visually represented in table structure:

   +---------------+---------------+--------------------------+---------------+---------------+
   | point1                                                   | point2                        |
   +---------------+---------------+--------------------------+---------------+---------------+
   | x             | y             | label                    | x             | y             |
   +===============+===============+==========================+===============+===============+
   | <int32 value> | <int32 value> | <"visble" or "occluded"> | <int32 value> | <int32 value> |
   +---------------+---------------+--------------------------+---------------+---------------+
