#############
 Type Import
#############

Portex supports Type Import, which means the schema structure can be defined and shared in the
community.

A **package** is used to distribute a group of pre-defined types. And these types can be imported
from the package.

.. tip::

   Just like a programming language, Portex also uses packages for distributing pre-defined types.
   Take python as an example. Python package is used to distribute a set of functions which can be
   reused.

The git repository is used as a carrier for a schema package. A schema package is distributed,
developed, and imported through a public git repository.

OpenBytes defines a set of standard formats for open datasets. These formats are put on a Github
repo and distributed as a schema package whose url is
https://github.com/Project-OpenBytes/portex-standard.

********************************
 How to build a schema package?
********************************

#. Create a remote git repo;
#. Commit a file named ``ROOT.yaml`` to indicate the root path of the schema;
#. Commit the schema structure files which need to be reused into the git repo.

*************************************
 How to import types from a package?
*************************************

#. Use :ref:`root-parameters` ``imports`` to indicate what types needs to be imported and which
   package these types come from;
#. Put the schema structure name or alias which needs to be referenced in the ``type`` field.

.. _root-parameters:

Parameters
==========

The parameter ``imports`` is provided for type importing, and it should be put on the top level of
the schema definition file.

.. list-table::
   :header-rows: 1
   :widths: auto

   -  -  name
      -  type
      -  required
      -  description

   -  -  ``imports``
      -  |  JSON
         |  array
      -  False
      -  |  A JSON object which indicates what types needs to
         |  be imported and which package these types come from.

   -  -  ``imports.<index>``
      -  |  JSON
         |  object
      -  True
      -  |  Each item in the ``imports`` array indicates a group
         |  of imported types which come from a same package.

   -  -  ``imports.<index>.repo``
      -  |  JSON
         |  string
      -  True
      -  |  The url and the revision of the schema package,
         |  which follows the following format: "<url>@<rev>".

   -  -  ``imports.<index>.types``
      -  |  JSON
         |  array
      -  True
      -  |  A JSON array to indicate the types needs to be
         |  imported from the package to this file.

   -  -  ``imports.<index>.types.<index>``
      -  |  JSON
         |  object
      -  True
      -  |  Each item in the ``imports.<index>.types``
         |  array indicates one imported type.

   -  -  ``imports.<index>.types.<index>.name``
      -  |  JSON
         |  string
      -  True
      -  |  The name of the imported type which follows
         |  the :ref:`dot-syntax`

   -  -  ``imports.<index>.types.<index>.alias``

      -  |  JSON
         |  string

      -  False

      -  |  The alias of the imported type. If this field is given,
         |  it will replace the ``imports.types.<index>.name`` as
         |  the unique identifier of the imported type. This field
         |  is useful for solving the type name conflicts in
         |  different packages.

.. _dot-syntax:

Dot Syntax
==========

The **doc syntax** is used for referencing pre-defined type.

Dot syntax is:

#. Based on the file path of the schema structure file;
#. Use dot ``.`` to replace the file separator ( ``/`` for Linux and ``\`` for Windows);
#. Remove the file extension.

For example, there is a schema repo with the following file structure:

.. code:: shell

   .
   ├── geometry
   │   ├── Vector2D.yaml
   │   └── Vector3D.yaml
   └── ROOT.yaml    # the ROOT.yaml file is used to indicate the root of the schema package.

The schema file ``geometry/Vector2D.yaml`` needs to be written as ``geometry.Vector2D`` for
referencing.

Example
=======

For example, two pre-defined types ``Vector2D`` and ``Vector3D`` need to be imported from a Github
repo, whose url is https://github.com/Project-OpenBytes/portex-standard and the tag is ``v1.0.0``.

The repo file structure is:

.. code:: shell

   .
   ├── geometry
   │   ├── Vector2D.yaml
   │   └── Vector3D.yaml
   └── ROOT.yaml    # the ROOT.yaml file is used to indicate the root of the schema package.

Here is how the ``Vector2D`` and ``Vector3D`` are imported:

.. code:: yaml

   ---
   imports:
     - repo: https://github.com/Project-OpenBytes/portex-standard@v1.0.0
                                               # Use "<url>@<rev>" format to # point out where the
                                               # source code comes from.
       types:
         - name: geometry.Vector2D             # Use "dot syntax" to point out the type defined in
                                               # "geometry/Vector2D.yaml" that needs to be imported
                                               # to this file.
         - name: geometry.Vector3D
           alias: Vector3D                     # Use "alias" field to rename the imported type.
                                               # "alias" will replace the origin name as the unique
                                               # identifier. Which means "geometry.Vector3D" will
                                               # be treated as illegal name. Only "Vector3D" can be
                                               # used for referencing the imported type.

   type: record
   fields:
     - name: point2d
       type: geometry.Vector2D       # Use the "name" defined in the "imports" field to reuse
                                     # the pre-defined type.
     - name: point3d
       type: Vector3D                # Use the "alias" defined in the "imports" field to reuse
                                     # the pre-defined type.
