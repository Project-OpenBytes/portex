########
 tensor
########

The ``tensor`` type represents an N-dimensional array in which all elements have the same type.

|  Type ``tensor`` has two parameters: ``shape`` and ``dtype``:
|   - ``shape`` is used to indicate the shape of the tensor.
|   - ``dtype`` is used to indicate the type of the elements in the tensor.

.. list-table::
   :header-rows: 1
   :widths: auto

   -  -  name
      -  type
      -  required
      -  description

   -  -  ``shape``
      -  |  JSON
         |  array
      -  True
      -  |  Represent the shape of the tensor, the presentation logic is the same as ``numpy``,
         |  and ``null`` can be used to represent a dimension with unlimited length.

   -  -  ``dtype``
      -  |  JSON
         |  string
      -  True
      -  Represent the type of the tensor elements.

**Examples**:

#. a 3x3 integer matrix:

   .. code:: yaml

      ---
      type: tensor
      shape: [3, 3]
      dtype: int32

#. a 640x480 image tensor with 3 channels:

   .. code:: yaml

      ---
      type: tensor
      shape: [640, 480, 3]
      dtype: int32

#. an integer matrix with unlimited shape:

   .. code:: yaml

      ---
      type: tensor
      shape: [null, null]
      dtype: int32
