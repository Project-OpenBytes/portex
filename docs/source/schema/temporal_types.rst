################
 Temporal Types
################

Portex provides a set of temporal types:

******
 date
******

The ``date`` type represents a date in a calendar without timezone or time of day.

The storage type of ``date`` is ``int32``. It represents the days since UNIX epoch ``1970-01-01``.

**Examples**:

A date object:

   .. code:: yaml

      ---
      type: date

******
 time
******

The ``time`` type represents a time of day, independent of any particular calendar, timezone or
date.

The parameter ``unit`` is provided for ``time`` to indicate time resolution.

.. list-table::
   :header-rows: 1
   :widths: auto

   -  -  name
      -  type
      -  required
      -  description

   -  -  ``unit``
      -  JSON string
      -  True
      -  |  The time resolution, support ``s``, ``ms``, ``us`` and ``ms``:
         |  - ``s`` for second
         |  - ``ms`` for millisecond
         |  - ``us`` for microsecond
         |  - ``ns`` for nanosecond

The ``s`` and ``ms`` time will be stored as ``int32`` and ``us`` and ``ns`` time will be stored as
``int64``. And it represents an offset from ``00:00:00`` with the giving unit.

**Examples**:

A time with millisecond resolution:

   .. code:: yaml

      ---
      type: time
      unit: ms

***********
 timestamp
***********

The ``timestamp`` type represents a time of day with date.

|  Type ``timestamp`` has two parameters ``unit`` and ``tz``:
|     - ``unit`` is used to indicate time resolution.
|     - ``tz`` is used to indicate the timezone info.

.. list-table::
   :header-rows: 1
   :widths: auto

   -  -  name
      -  type
      -  required
      -  description

   -  -  ``unit``
      -  JSON string
      -  True
      -  |  The time resolution, support ``s``, ``ms``, ``us`` and ``ms``:
         |  - ``s`` for second
         |  - ``ms`` for millisecond
         |  - ``us`` for microsecond
         |  - ``ns`` for nanosecond

   -  -  ``tz``
      -  JSON string
      -  False
      -  |  The timezone info, default to naive timestamp.
         |  Supported timezone list: TZ_LIST_

.. _tz_list: https://en.wikipedia.org/wiki/List_of_tz_database_time_zones

The storage type of ``timestamp`` is ``int64``. It represents an offset from ``1970-01-01T00:00:00``
with the giving unit.

**Examples**:

#. A naive timestamp with millisecond resolution:

   .. code:: yaml

      ---
      type: timestamp
      unit: ms

#. A aware timestamp with microsecond resolution and timezone info is Asia/Shanghai:

   .. code:: yaml

      ---
      type: timestamp
      unit: us
      tz: Asia/Shanghai

***********
 timedelta
***********

The ``timedelta`` type represents a time duratiion, the difference between two dates or times.

The parameter ``unit`` is provided for ``timedelta`` to indicate time resolution.

.. list-table::
   :header-rows: 1
   :widths: auto

   -  -  name
      -  type
      -  required
      -  description

   -  -  ``unit``
      -  JSON string
      -  True
      -  |  The time resolution, support ``s``, ``ms``, ``us`` and ``ms``:
         |  - ``s`` for second
         |  - ``ms`` for millisecond
         |  - ``us`` for microsecond
         |  - ``ns`` for nanosecond

The storage type of ``timedelta`` is ``int64``. It represents an time offset with the giving unit.

**Examples**:

A timedelta with millisecond resolution:

   .. code:: yaml

      ---
      type: timedelta
      unit: ms
