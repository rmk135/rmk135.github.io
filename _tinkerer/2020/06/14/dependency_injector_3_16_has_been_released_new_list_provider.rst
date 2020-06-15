Dependency Injector 3.16 has been released - New ``List`` provider
==================================================================

.. image:: https://avatars0.githubusercontent.com/u/11329744?s=400&v=4
   :align: left
   :width: 200
   :target: /2020/06/14/dependency_injector_3_16_has_been_released_new_list_provider.html

I have released `Dependency Injector`_ 3.16.

Main feature of this release is a new ``List`` provider. Release also contains documentation
fixes and adding support of ``six 1.15.0``.

Release is available for downloads on `PyPI`_.

.. more::

|
|

List provider
~~~~~~~~~~~~~

``List`` provider is needed for injecting a list of dependencies. Now it is as simple as this:

.. code-block:: python

    dispatcher_factory = providers.Factory(
        Dispatcher,
        modules=providers.List(
            providers.Factory(Module, name='m1'),
            providers.Factory(Module, name='m2'),
        ),
    )

The call to ``dispatcher_factory`` is equivalent to:

.. code-block:: python

    dispatcher = Dispatcher(
        modules=[
            Module(name='m1'),
            Module(name='m2'),
        ],
    )

Visit docs for `Full Example <http://python-dependency-injector.ets-labs.org/providers/list.html>`_.

Changelog
~~~~~~~~~

- Add ``List`` provider
  `issue #243 <https://github.com/ets-labs/python-dependency-injector/issues/243>`_,
  `PR #251 <https://github.com/ets-labs/python-dependency-injector/pull/251>`_.
- Fix a few typos in docs (thanks to `Bruno P. Kinoshita <https://github.com/kinow>`_,
  `issue #249 <https://github.com/ets-labs/python-dependency-injector/issues/249>`_,
  `PR #250 <https://github.com/ets-labs/python-dependency-injector/pull/250>`_).
- Add support of six 1.15.0.
- Regenerate C sources using Cython 0.29.20.

More
~~~~

- https://github.com/ets-labs/python-dependency-injector
- http://python-dependency-injector.ets-labs.org

Enjoy Python and Dependency Injection.

.. author:: default
.. categories:: none
.. tags:: none
.. comments::

.. _Dependency Injector: https://github.com/ets-labs/python-dependency-injector
.. _PyPI: https://pypi.org/project/dependency-injector/
