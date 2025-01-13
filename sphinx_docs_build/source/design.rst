Design Decisions
================

Range
-----

For the ``Range`` type, the constructor follows the `PostgreSQL range
constructor
functions <https://www.postgresql.org/docs/current/rangetypes.html#RANGETYPES-CONSTRUCT>`__
which makes [`closed,
open) <https://fhur.me/posts/always-use-closed-open-intervals>`__ the
easiest to express:

.. code:: python

   from pg8000.types import Range
   pg_range = Range(2, 6)