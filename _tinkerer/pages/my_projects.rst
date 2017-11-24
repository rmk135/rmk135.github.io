===========
My Projects
===========

I have several public open-source projects that I work on when I have some 
spare time. This pages is intended to keep very short summary about each of 
mine projects and contain links to the resources associated with them.

Dependency Injector
-------------------

.. image:: https://img.shields.io/github/watchers/ets-labs/python-dependency-injector.svg?style=social&label=Watch
   :target: https://github.com/ets-labs/python-dependency-injector
   :alt: Github watchers
.. image:: https://img.shields.io/github/stars/ets-labs/python-dependency-injector.svg?style=social&label=Star
   :target: https://github.com/ets-labs/python-dependency-injector
   :alt: Github stargazers
.. image:: https://img.shields.io/github/forks/ets-labs/python-dependency-injector.svg?style=social&label=Fork
   :target: https://github.com/ets-labs/python-dependency-injector
   :alt: Github forks

`Dependency Injector`_ is a dependency injection microframework for Python. 
It was designed to be unified, developer-friendly tool that helps to 
implement dependency injection design pattern in formal, pretty, Pythonic way.

Dependency Injector framework key features are:

+ Easy, smart, pythonic style.
+ Obvious, clear structure.
+ Extensibility and flexibility.
+ High performance.
+ Memory efficiency.
+ Thread safety.
+ Documentation.
+ Semantic versioning.

Dependency Injector containers and providers are implemented as C extension
types using Cython.

Brief example below is a simplified version of inversion of control 
containters from one of the real-life applications. This example demonstrates
usage of `Dependency Injector`_ inversion of control containers & providers
for specifying all application components and their dependencies beetween
each other in one module. Besides other listed above advantages, it gives a
great opportunity to control & manage application's structure in one place.

.. code-block:: python

    """Example of dependency injection in Python."""

    import logging
    import sqlite3

    import boto3

    import example.main
    import example.services

    import dependency_injector.containers as containers
    import dependency_injector.providers as providers


    class Core(containers.DeclarativeContainer):
        """IoC container of core component providers."""

        config = providers.Configuration('config')

        logger = providers.Singleton(logging.Logger, name='example')


    class Gateways(containers.DeclarativeContainer):
        """IoC container of gateway (API clients to remote services) providers."""

        database = providers.Singleton(sqlite3.connect, Core.config.database.dsn)

        s3 = providers.Singleton(
            boto3.client, 's3',
            aws_access_key_id=Core.config.aws.access_key_id,
            aws_secret_access_key=Core.config.aws.secret_access_key)


    class Services(containers.DeclarativeContainer):
        """IoC container of business service providers."""

        users = providers.Factory(example.services.UsersService,
                                  db=Gateways.database,
                                  logger=Core.logger)

        auth = providers.Factory(example.services.AuthService,
                                 db=Gateways.database,
                                 logger=Core.logger,
                                 token_ttl=Core.config.auth.token_ttl)

        photos = providers.Factory(example.services.PhotosService,
                                   db=Gateways.database,
                                   s3=Gateways.s3,
                                   logger=Core.logger)


    class Application(containers.DeclarativeContainer):
        """IoC container of application component providers."""

        main = providers.Callable(example.main.main,
                                  users_service=Services.users,
                                  auth_service=Services.auth,
                                  photos_service=Services.photos)

Next example demonstrates run of example application defined above:

.. code-block:: python

    """Run example application."""

    import sys
    import logging

    from containers import Core, Application


    if __name__ == '__main__':
        # Configure platform:
        Core.config.update({'database': {'dsn': ':memory:'},
                            'aws': {'access_key_id': 'KEY',
                                    'secret_access_key': 'SECRET'},
                            'auth': {'token_ttl': 3600}})
        Core.logger().addHandler(logging.StreamHandler(sys.stdout))

        # Run application:
        Application.main(uid=sys.argv[1],
                         password=sys.argv[2],
                         photo=sys.argv[3])

More extensive description of this example could be found 
`here <http://python-dependency-injector.ets-labs.org/examples/services_miniapp.html>`_.

Links:

+ GitHub - https://github.com/ets-labs/python-dependency-injector
+ PyPI - https://pypi.python.org/pypi/dependency_injector/
+ Documentation - http://python-dependency-injector.ets-labs.org/

Python VIMRC
------------

.. image:: https://img.shields.io/github/watchers/ets-labs/python-vimrc.svg?style=social&label=Watch
   :target: https://github.com/ets-labs/python-vimrc
   :alt: Github watchers
.. image:: https://img.shields.io/github/stars/ets-labs/python-vimrc.svg?style=social&label=Star
   :target: https://github.com/ets-labs/python-vimrc
   :alt: Github stargazers
.. image:: https://img.shields.io/github/forks/ets-labs/python-vimrc.svg?style=social&label=Fork
   :target: https://github.com/ets-labs/python-vimrc
   :alt: Github forks

`Python VIMRC`_ - VIM Configuration for Python / Cython / C Development.

One screenshot is better than hundred of words:

.. image:: /_static/images/python-vimrc-screenshot.png
   :scale: 50%

Keep calm and use VIM!

Links:

+ GitHub - https://github.com/ets-labs/python-vimrc
+ YouTube - https://www.youtube.com/watch?v=7o9yiHO7gHM


.. _Dependency Injector: https://github.com/ets-labs/python-dependency-injector
.. _Python VIMRC: https://github.com/ets-labs/python-vimrc
