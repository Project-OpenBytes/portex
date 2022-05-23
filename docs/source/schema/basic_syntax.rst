##############
 Basic Syntax
##############

Here is the basic syntax of Portex:

.. code:: yaml

   ---
   type: <type>
   <type-param 1>: <value 1>
   <type-param 2>: <value 2>
   ...: ...
   ...: ...

Portex provides the basic key ``type``. Its value means the type of data and presented by a JSON
string. The bulitin supported types can be found in :doc:`/schema/primitive_types` and
:doc:`/schema/complex_types/index`.

The most important feature of Portex is that the type is configurable, different types has different
parameters.

For example, the :doc:`/schema/complex_types/enum` type has parameters ``values`` to indicate the
possible values of the enum.

So an enum of ``"dog"`` and ``"cat"`` can be defined:

.. code:: yaml

   ---
   type: enum
   values: ["dog", "cat"]

Besides the builtin types, the customized types can also be configurable, check
:doc:`/schema/template_type` for more details.

###############
 Nullable Type
###############

Portex provides a common parameter ``nullable`` for all types to indicate whether the value can be
null.

.. list-table::
   :header-rows: 1
   :widths: auto

   -  -  name
      -  type
      -  required
      -  default
      -  description

   -  -  ``nullable``
      -  JSON boolean
      -  False
      -  ``False``
      -  |  Default to ``False``, which means all types are not nullable by default.
         |  Setting to ``True`` allows the stored value to be null.

**Examples**:

Nullable 32-bits signed integer:

   .. code:: yaml

      ---
      type: int32
      nullable: true
