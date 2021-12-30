###############
 Basic Grammar
###############

Here is the basic grammar of OpenBytes Schema:

.. code:: yaml

   ---
   type: <type>
   <type-param 1>: <value 1>
   <type-param 2>: <value 2>
   ...: ...
   ...: ...

Schema provides the basic key ``type``. Its value means the type of the data and presented by a JSON
string. The bulitin supported types can be found in :doc:`/schema/primitive_types` and
:doc:`/schema/complex_types/index`.

The most important feature of OpenBytes schema is that the type is configuable, different types has
different parameters.

For example, the :ref:`numeric types <numeric_types>` ``int`` has parameters ``minimum`` and
``maximum`` which limits the range of the ``int`` number.

So a integer in range 0~100 can be defined:

.. code:: yaml

   ---
   type: int
   minimum: 0
   maximum: 100

Besides the builtin types, the customized types can also be configuable, check
:doc:`/schema/template_type` for more details.
