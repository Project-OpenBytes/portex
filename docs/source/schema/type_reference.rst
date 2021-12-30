################
 Type Reference
################

OpenBytes Schema supports Type Reference, which means the schema structure can be defined and shared
in the community.

The **package** is used to distribute a group of pre-defined schema structures. And the schema
structures can be referenced from the package.

.. tip::

   Just like programming language, take python as an example. Python package is used to distribute a
   set of functions which can be reused. OpenBytes schema also use package for distributing
   pre-defined schema structures.

The git repository is used as a carrier for a schema package. A schema package is distributed,
developed and referenced throuth a public git repository.

OpenBytes defines a set of standard formats for open datasets. These formats are put on a Github
repo and distributed as a schema package. Which url is https://github.com/Graviti-AI/standard

********************************
 How to build a schema package?
********************************

#. New a remote git repo;
#. Commit a file named ``standard.yaml`` to indicate the root path of the schema;
#. Commit the schema structure files which need to be reused into the git repo.

***************************************
 How to reference schema from package?
***************************************

#. Use :ref:`root-parameters` ``repo`` and ``version`` to indicate the repo url and version;
#. Put the schema structure name need to be referenced in the ``type`` field by :ref:`dot-grammar`.

.. _root-parameters:

Parameters
==========

|  Two parameters ``repo`` and ``version`` is provided for type reference:
|   - ``repo`` is used to indicate the url of the package repo.
|   - ``version`` is used to indicate the revision of the package repo.

These two parameters should be put on the top level of the schema definition file.

.. list-table::
   :header-rows: 1

   -  -  name
      -  type
      -  required
      -  default
      -  description

   -  -  ``repo``
      -  |  JSON
         |  string
      -  False
      -  "https://github.com/Graviti-AI/standard"
      -  |  The url of the package repo, and the schema
         |  structure will referenced from that package.

   -  -  ``version``

      -  |  JSON
         |  object

      -  True

      -  `-`

      -  |  The revision of the package repo, it is very important to
         |  point out the revision explicitly because the latest status
         |  of a git repo will change constantly. Point out a specific
         |  revision to make the schema unchanged.

.. _dot-grammar:

Dot Grammar
===========

The **doc grammar** is used for referencing pre-defined schema structures.

Dot grammar is:

#. Base on the file path of the schema structure file;
#. Use dot ``.`` to replace the file separator ( ``/`` for Linux and ``\`` for Windows);
#. Remove the file extension.

For example there are a schema repo with the following file structure:

.. code:: shell

   .
   ├── geometry
   │   ├── Vector2D.yaml
   │   └── Vector3D.yaml
   └── standard.yaml    # the standard.yaml file is used to indicate the root of the schema package.

The schema file ``geometry/Vector2D.yaml`` need to be changed to ``geometry.Vector2D`` for
referencing.

Example
=======

For example, a pre-defined ``Vector2D`` type needs to be referenced from a Github repo which url is
https://github.com/Graviti-AI/standard.

The repo file structure is:

.. code:: shell

   .
   ├── geometry
   │   └── Vector2D.yaml
   └── standard.yaml    # the standard.yaml file is used to indicate the root of the schema package.

Here is how the ``Vector2D`` type be referenced:

.. code:: yaml

   ---
   repo: https://github.com/Graviti-AI/standard  # Use "repo" parameter to indicate the repo url
   version: v1.0.0                               # Use "version" parameter to indicate the revision
   type: record
   fields:
     - name: point1
       type: geometry.Vector2D                   # Use "dot grammar" to reuse the pre-defined type

     - name: point2
       type: geometry.Vector2D
