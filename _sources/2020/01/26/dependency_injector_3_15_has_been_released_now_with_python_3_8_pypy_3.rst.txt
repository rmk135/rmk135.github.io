Dependency Injector 3.15 has been released - Now with Python 3.8 & PyPy 3
=========================================================================

.. image:: https://avatars0.githubusercontent.com/u/11329744?s=400&v=4
   :align: left
   :width: 200
   :target: /2020/01/26/dependency_injector_3_15_has_been_released_now_with_python_3_8_pypy_3.html

`Dependency Injector`_ 3.15 has been released. This release adds a support of Python 3.8 & PyPy 3,
as well as widens the list of compatible versions of ``six`` library with its 1.13.0 and 1.14.0
releases.

This release also is a beginning of soft and gentle wiping out of Python 2. In particular, all
examples were rewritten to stop doing Python 2-ish inheritance from ``object`` and start calling
parent methods in Python 3-ish way.

.. more::

In addition to the already mentioned features some kind of general clean up was done - fixing of
typos on the README and documentation pages, updating links, images, etc...

New release is available on PyPi. Community is welcome to install, upgrade and update requirement
files:

.. code-block:: bash

    pip install dependency-injector>=3.15

    pip install --upgrade dependency-injector>=3.15

    # requirements.txt
    dependency-injector>=3.15

Full changelog of 3.15 release:

- Add Python 3.8 support.
- Add PyPy 3.6 support.
- Add support of six 1.14.0.
- Add support of six 1.13.0.
- Regenerate C sources using Cython 0.29.14.
- Remove Python 2-ish inheritance from ``object`` in example modules.
- Replace Python 2-ish ``super(class, self).__init__()`` calls with Python 3-ish
  ``super().__init__()`` in example modules.
- Fix doc block errors in example modules, including related to PEP257-compliance.
- Clean up tox.ini file.
- Fix a couple of typos in the README.
- Fix a couple of types in the diagram of "Engines-Cars" example.
- Fix a typo in the installation instructions on the README page and in the documentation.
- Fix a typo in the link to the PyPi on the "Dependency Injection in Python" documentation page.
- Fix a couple of typos in the list of key features on the "Key Features" and index documentation
  pages.
- Update a link to the PyPi page on a couple of documentation pages.
- Update a link to the PyPi page on the README page.

More:

- https://github.com/ets-labs/python-dependency-injector
- http://python-dependency-injector.ets-labs.org

.. author:: default
.. categories:: none
.. tags:: none
.. comments::

.. _Dependency Injector: https://github.com/ets-labs/python-dependency-injector
