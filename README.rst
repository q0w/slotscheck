🎰 Slotscheck
=============

.. image:: https://img.shields.io/pypi/v/slotscheck.svg?color=blue
   :target: https://pypi.python.org/pypi/slotscheck

.. image:: https://img.shields.io/pypi/l/slotscheck.svg
   :target: https://pypi.python.org/pypi/slotscheck

.. image:: https://img.shields.io/pypi/pyversions/slotscheck.svg
   :target: https://pypi.python.org/pypi/slotscheck

.. image:: https://img.shields.io/readthedocs/slotscheck.svg
   :target: http://slotscheck.readthedocs.io/

.. image:: https://github.com/ariebovenberg/slotscheck/actions/workflows/build.yml/badge.svg
   :target: https://github.com/ariebovenberg/slotscheck/actions/workflows/build.yml

.. image:: https://img.shields.io/codecov/c/github/ariebovenberg/slotscheck.svg
   :target: https://codecov.io/gh/ariebovenberg/slotscheck

.. image:: https://img.shields.io/badge/code%20style-black-000000.svg
   :target: https://github.com/psf/black

Adding ``__slots__`` to a class in Python is a great way to reduce memory usage.
But to work properly, all base classes need to implement it — without overlap!
It's easy to get wrong, and what's worse: there is nothing warning you that you messed up.

✨ *Until now!* ✨

``slotscheck`` helps you validate your slots are working properly.
You can even use it to enforce the use of slots across (parts of) your codebase.

See my `blog post <https://dev.arie.bovenberg.net/blog/finding-broken-slots-in-popular-python-libraries/>`_
for the origin story behind ``slotscheck``.

Quickstart
----------

Usage is quick from the command line:

.. code-block:: bash

   python -m slotscheck [FILES]...
   # or
   slotscheck -m [MODULES]...

For example:

.. code-block:: bash

   $ slotscheck -m sanic
   ERROR: 'sanic.app:Sanic' defines overlapping slots.
   ERROR: 'sanic.response:HTTPResponse' has slots but superclass does not.
   Oh no, found some problems!
   Scanned 72 module(s), 111 class(es).

Now get to fixing —
and add ``slotscheck`` to your CI pipeline to prevent mistakes from creeping in again!
See `here <https://github.com/Instagram/LibCST/pull/615>`__ and
`here <https://github.com/dry-python/returns/pull/1233>`__ for examples.

Features
--------

- Detect broken slots inheritance
- Detect overlapping slots
- `Pre-commit <https://slotscheck.rtfd.io/en/latest/advanced.html#pre-commit-hook>`_ hook
- (Optionally) enforce the use of slots

See `the documentation <https://slotscheck.rtfd.io>`_ for more details
and configuration options.

Why not a flake8 plugin?
------------------------

Flake8 plugins need to work without running the code.
Many libraries use conditional imports, star imports, re-exports,
and define slots with decorators or metaclasses.
This all but requires running the code to determine the slots and class tree.

There's `an issue <https://github.com/ariebovenberg/slotscheck/issues/6>`_
to discuss the matter.

Notes
-----

- ``slotscheck`` will try to import all submodules of the given package.
  If there are scripts without ``if __name__ == "__main__":`` blocks,
  they may be executed.
- Even in the case that slots are not inherited properly,
  there may still be an advantage to using them
  (i.e. attribute access speed and *some* memory savings).
  However, in most cases this is unintentional.
  ``slotscheck`` allows you to ignore specific cases.
- Non pure-Python classes are currently assumed to have slots.
  This is not necessarily the case, but it is nontrivial to determine.

Installation
------------

It's available on PyPI.

.. code-block:: bash

  pip install slotscheck
