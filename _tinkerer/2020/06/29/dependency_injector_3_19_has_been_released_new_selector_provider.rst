Dependency Injector 3.19 has been released - New Selector provider
==================================================================

.. image:: https://avatars0.githubusercontent.com/u/11329744?s=400&v=4
   :align: left
   :width: 200
   :target: /2020/06/14/dependency_injector_3_16_has_been_released_new_list_provider.html

I have released `Dependency Injector`_ 3.19.

This release adds ``Selector`` provider. Release also fixes minor bug in ``Configuration``
provider.

Release is available for downloads on `PyPI`_.

.. more::

|
|

Selector provider
~~~~~~~~~~~~~~~~~

``Selector`` provider selects provider based on the configuration value or other callable.

The idea of ``Selector`` provider was brought up at the issue
`#239 <https://github.com/ets-labs/python-dependency-injector/issues/239>`_. It was brought up
by `Markus Leuthold (githubkusi) <https://github.com/githubkusi>`_. Thanks to Markus for his
contribution.

.. code-block:: python

    from dependency_injector import providers


    class SomeClass:
        ...


    class SomeOtherClass:
        ...


    config = providers.Configuration()

    selector = providers.Selector(
        config.one_or_another,
        one=providers.Factory(SomeClass),
        another=providers.Factory(SomeOtherClass),
    )

    config.override({'one_or_another': 'one'})
    instance_1 = selector()
    assert isinstance(instance_1, SomeClass)

    config.override({'one_or_another': 'another'})
    instance_2 = selector()
    assert isinstance(instance_2, SomeOtherClass)

More
~~~~

- ⭐️ Star project `GitHub <https://github.com/ets-labs/python-dependency-injector>`_
- 📖 Read the `Docs <http://python-dependency-injector.ets-labs.org/providers/list.html>`_

Enjoy Python and Dependency Injector.

.. author:: default
.. categories:: none
.. tags:: none
.. comments::

.. _Dependency Injector: https://github.com/ets-labs/python-dependency-injector
.. _PyPI: https://pypi.org/project/dependency-injector/
