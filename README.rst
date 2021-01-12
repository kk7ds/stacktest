===========================================================
 stacktest.el - in buffer testing of projects in OpenStack
===========================================================

OpenStack projects have a relatively well defined testing interface
that uses ``tox`` and ``testrepository``.

This is a mode to make testing using those be easily available inside
of emacs. This reuses the compile infrastructure in emacs and is based
upon nosemacs that does this same functionality for nose.

Installation
============

Add the following to your .emacs configuration::

  (require 'stacktest)
  ;; optionally enable for all python files
  (add-hook 'python-mode-hook 'stacktest-mode)

Usage
=====

From within a python file you can run any of the following:

- M-x stacktest-one : runs the current test function your cursor is in
- M-x stacktest-debug-one : runs the current test function your cursor
  is in with OS_DEBUG=True (turns on debug logging)
- M-x stacktest-pdb-one : runs the current test function your cursor
  is in pdb
- M-x stacktest-module : runs all the tests in the current file
- M-x stacktest-all : runs a full ``tox`` run in an emacs buffer
- M-x stacktest-target : prompts for and runs a specific ``tox``
  environment
- M-x stacktest-again : rerun the last test run
- M-x stacktest-pep8 : run the ``pep8`` tox target
- M-x stacktest-pep8-module : run the ``pep8`` check command on
  the current file

You can bind these to more convenient keys. The various modes are
extremely useful when iterating on test development. The compilation
buffer is annotated so that failure points are clickable back into the
code in question.

This also works over tramp if you are using ssh as your transport,
through the magic of awesomeness.

Screenshots
===========

A successful stacktest-one run (M-x stacktest-one from the shown
cursor possition)

.. image:: images/stacktest-success.png
           :alt: stacktest successful

A failed stacktest-one run. The filenames listed in the traceback are
linked elements that will open those files at those lines.

.. image:: images/stacktest-fail.png
           :alt: stacktest failing


Assumptions
===========

When running in -one or -module mode the testing skips tox / testr
entirely and just uses subunit.run (if subunit-trace is available) or
testtools.run from the .tox/py27 directory by default. This assumes
that tox has been run at least once before to generate the venv.

By default, if test files are structured as ``tests/unit/..`` and
``tests/functional/..``, the ``py27`` and ``functional`` virtualenvs
will be used for executing subunit-trace or testtools. The fallback is
always ``py27`` and additional rules can be defined by setting
``stacktest-venv-pattern``.

Futures
=======

Things that would be nice to have (PRs welcomed)

- further enhance the compilation buffer regexp to make it easier to
  understand at a glance
- menu available when stacktest-mode enable
