Development
===========


Philosophy
----------

1. First create something that work (to provide business value).
2. Then something that's beautiful (to lower maintenance costs).
3. Finally works on performance (to avoid wasting time on premature
   optimizations).


Stability policy
----------------

This project follows `Semantic Versioning <http://semver.org/>`_.

Which boils down to the following rules of thumb regarding stability:

* **Patch releases** (``0.x.n`` → ``0.x.(n+1)`` upgrades) are bug-fix only.
  These releases must not break anything and keeps backward-compatibility with
  ``0.x.*`` and ``0.(x-1).*`` series.

* **Minor releases** (``0.n.*`` → ``0.(n+1).0`` upgrades) includes any
  non-bugfix changes. These releases must be backward-compatible with any
  ``0.n.*`` version but are allowed to drop compatibility with the
  ``0.(n-1).*`` series and below.

* **Major releases** (``n.*.*`` → ``(n+1).0.0`` upgrades) are not planned yet,
  unless we introduce huge changes to the project.


Build status
------------

==============  ==================  ===================
Branch          |master-branch|__   |develop-branch|__
==============  ==================  ===================
Unittests       |build-stable|      |build-dev|
Coverage        |coverage-stable|   |coverage-dev|
Quality         |quality-stable|    |quality-dev|
Dependencies    |deps-stable|       |deps-dev|
Documentation   |docs-stable|       |docs-dev|
==============  ==================  ===================

.. |master-branch| replace::
   ``master``
__ https://github.com/kdeldycke/maildir-deduplicate/tree/master
.. |develop-branch| replace::
   ``develop``
__ https://github.com/kdeldycke/maildir-deduplicate/tree/develop

.. |build-stable| image:: https://travis-ci.org/kdeldycke/maildir-deduplicate.svg?branch=master
    :target: https://travis-ci.org/kdeldycke/maildir-deduplicate
    :alt: Unit-tests status
.. |build-dev| image:: https://travis-ci.org/kdeldycke/maildir-deduplicate.svg?branch=develop
    :target: https://travis-ci.org/kdeldycke/maildir-deduplicate
    :alt: Unit-tests status

.. |coverage-stable| image:: https://codecov.io/gh/kdeldycke/maildir-deduplicate/branch/master/graph/badge.svg
    :target: https://codecov.io/gh/kdeldycke/maildir-deduplicate/branch/master
    :alt: Coverage Status
.. |coverage-dev| image:: https://codecov.io/gh/kdeldycke/maildir-deduplicate/branch/develop/graph/badge.svg
    :target: https://codecov.io/gh/kdeldycke/maildir-deduplicate/branch/develop
    :alt: Coverage Status

.. |quality-stable| image:: https://scrutinizer-ci.com/g/kdeldycke/maildir-deduplicate/badges/quality-score.png?b=master
    :target: https://scrutinizer-ci.com/g/kdeldycke/maildir-deduplicate/?branch=master
    :alt: Code Quality
.. |quality-dev| image:: https://scrutinizer-ci.com/g/kdeldycke/maildir-deduplicate/badges/quality-score.png?b=develop
    :target: https://scrutinizer-ci.com/g/kdeldycke/maildir-deduplicate/?branch=develop
    :alt: Code Quality

.. |deps-stable| image:: https://requires.io/github/kdeldycke/maildir-deduplicate/requirements.svg?branch=master
    :target: https://requires.io/github/kdeldycke/maildir-deduplicate/requirements/?branch=master
    :alt: Requirements freshness
.. |deps-dev| image:: https://requires.io/github/kdeldycke/maildir-deduplicate/requirements.svg?branch=develop
    :target: https://requires.io/github/kdeldycke/maildir-deduplicate/requirements/?branch=develop
    :alt: Requirements freshness

.. |docs-stable| image:: https://readthedocs.org/projects/maildir-deduplicate/badge/?version=stable
    :target: http://maildir-deduplicate.readthedocs.io/en/stable/
    :alt: Documentation Status
.. |docs-dev| image:: https://readthedocs.org/projects/maildir-deduplicate/badge/?version=develop
    :target: http://maildir-deduplicate.readthedocs.io/en/develop/
    :alt: Documentation Status


Setup a development environment
-------------------------------

Check out latest development branch:

.. code-block:: bash

    $ git clone git@github.com:kdeldycke/maildir-deduplicate.git
    $ cd ./maildir-deduplicate
    $ git checkout develop

Install package in editable mode with all development dependencies:

.. code-block:: bash

    $ pip install -e .[develop]

Now you're ready to hack and abuse git!


Unit-tests
----------

Install test dependencies and run unit-tests:

.. code-block:: bash

    $ pip install -e .[tests]
    $ nosetests


Coding style
------------

Run `isort <https://github.com/timothycrosley/isort>`_ utility to sort Python
imports:

.. code-block:: bash

    $ pip install -e .[develop]
    $ isort --apply

Then run `pycodestyle <https://pycodestyle.readthedocs.io>`_ and `Pylint
<http://docs.pylint.org>`_ code style checks:

.. code-block:: bash

    $ pip install -e .[tests]
    $ pycodestyle maildir_deduplicate
    $ pylint --rcfile=setup.cfg maildir_deduplicate


Build documentation
-------------------

The documentation you're currently reading can be built locally with `Sphinx
<http://www.sphinx-doc.org>`_:

.. code-block:: bash

    $ pip install -e .[docs]
    $ sphinx-apidoc -f -o ./docs .
    $ sphinx-build -b html ./docs ./docs/html

For a smooth release, you also need to validate the rendering of package's long
description on PyPi, as well as metadata:

.. code-block:: bash

    $ pip install -e .[develop]
    $ ./setup.py check -m -r -s


Release process
---------------

Start from the ``develop`` branch:

.. code-block:: bash

    $ git clone git@github.com:kdeldycke/maildir-deduplicate.git
    $ cd ./maildir-deduplicate
    $ git checkout develop

Install development dependencies:

.. code-block:: bash

    $ pip install -e .[develop]

Revision should already be set to the next version, so we just need to set the
released date in the changelog:

.. code-block:: bash

    $ vi ./CHANGES.rst

Create a release commit, tag it and merge it back to ``master`` branch:

.. code-block:: bash

    $ git add ./maildir_deduplicate/__init__.py ./CHANGES.rst
    $ git commit -m "Release vX.Y.Z"
    $ git tag "vX.Y.Z"
    $ git push
    $ git push --tags
    $ git checkout master
    $ git pull
    $ git merge "vX.Y.Z"
    $ git push

Push packaging to the `test cheeseshop
<https://wiki.python.org/moin/TestPyPI>`_:

.. code-block:: bash

    $ ./setup.py register -r testpypi
    $ ./setup.py clean --all
    $ ./setup.py sdist bdist_egg bdist_wheel upload -r testpypi

Publish packaging to `PyPi <https://pypi.python.org>`_:

.. code-block:: bash

    $ ./setup.py register -r pypi
    $ ./setup.py clean --all
    $ ./setup.py sdist bdist_egg bdist_wheel upload -r pypi

Update revision with `bumpversion <https://github.com/peritus/bumpversion>`_
and set it back to development state by increasing the ``patch`` level.

.. code-block:: bash

    $ git checkout develop
    $ bumpversion --verbose patch
    $ git add ./maildir_deduplicate/__init__.py ./CHANGES.rst
    $ git commit -m "Post release version bump."
    $ git push

Now if the next revision is no longer bug-fix only, bump the ``minor``
revision level instead:

.. code-block:: bash

    $ bumpversion --verbose minor
    $ git add ./maildir_deduplicate/__init__.py ./CHANGES.rst
    $ git commit -m "Next release no longer bug-fix only. Bump revision."
    $ git push
