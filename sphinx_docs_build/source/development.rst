Development Guide
=================

How to Generate the Sphinx Documentation
----------------------------------------

You may generate the documentation as follows:

.. code:: bash

    # Build the documentation into static HTML pages
    cd sphinx_docs_build
    python3 -m pip install -r requirements.txt
    make html

Tests
-----

-  Install `tox <http://testrun.org/tox/latest/>`__: ``pip install tox``

-  Enable the PostgreSQL hstore extension by running the SQL command:
   ``create extension hstore;``

-  Add a line to ``pg_hba.conf`` for the various authentication options:

::

   host    pg8000_md5           all        127.0.0.1/32            md5
   host    pg8000_gss           all        127.0.0.1/32            gss
   host    pg8000_password      all        127.0.0.1/32            password
   host    pg8000_scram_sha_256 all        127.0.0.1/32            scram-sha-256
   host    all                  all        127.0.0.1/32            trust

-  Set password encryption to ``scram-sha-256`` in ``postgresql.conf``:
   ``password_encryption = 'scram-sha-256'``

-  Set the password for the postgres user:
   ``ALTER USER postgresql WITH PASSWORD 'pw';``

-  Run ``tox`` from the ``pg8000`` directory: ``tox``

This will run the tests against the Python version of the virtual
environment, on the machine, and the installed PostgreSQL version
listening on port 5432, or the ``PGPORT`` environment variable if set.

Benchmarks are run as part of the test suite at
``tests/test_benchmarks.py``.

Doing A Release Of pg8000
-------------------------

Run ``tox`` to make sure all tests pass, then update the release notes,
then do:

::

   git tag -a x.y.z -m "version x.y.z"
   rm -r dist
   python -m build
   twine upload dist/*