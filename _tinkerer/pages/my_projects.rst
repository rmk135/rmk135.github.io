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
container from one of the real-life applications. This example demonstrates
usage of *Dependency Injector* inversion of control container & providers
for specifying all application components and their dependencies between
each other in one module. Besides other listed above advantages, it gives a
great opportunity to control & manage application's structure in one place.

.. code-block:: python

    """Example of dependency injection in Python."""

    import logging
    import sqlite3

    import boto3

    from dependency_injector import containers, providers
    from example import services, main


    class IocContainer(containers.DeclarativeContainer):
        """Application IoC container."""

        config = providers.Configuration('config')
        logger = providers.Singleton(logging.Logger, name='example')

        # Gateways

        database_client = providers.Singleton(sqlite3.connect, config.database.dsn)

        s3_client = providers.Singleton(
            boto3.client, 's3',
            aws_access_key_id=config.aws.access_key_id,
            aws_secret_access_key=config.aws.secret_access_key,
        )

        # Services

        users_service = providers.Factory(
            services.UsersService,
            db=database_client,
            logger=logger,
        )

        auth_service = providers.Factory(
            services.AuthService,
            token_ttl=config.auth.token_ttl,
            db=database_client,
            logger=logger,
        )

        photos_service = providers.Factory(
            services.PhotosService,
            db=database_client,
            s3=s3_client,
            logger=logger,
        )

        # Misc

        main = providers.Callable(
            main.main,
            users_service=users_service,
            auth_service=auth_service,
            photos_service=photos_service,
        )

Next example demonstrates run of example application defined above:

.. code-block:: python

    """Run example of dependency injection in Python."""

    import sys
    import logging

    from container import IocContainer


    if __name__ == '__main__':
        # Configure container:
        container = IocContainer(
            config={
                'database': {
                    'dsn': ':memory:',
                },
                'aws': {
                    'access_key_id': 'KEY',
                    'secret_access_key': 'SECRET',
                },
                'auth': {
                    'token_ttl': 3600,
                },
            }
        )
        container.logger().addHandler(logging.StreamHandler(sys.stdout))

        # Run application:
        container.main(*sys.argv[1:])
More extensive description of this example could be found
`here <http://python-dependency-injector.ets-labs.org/examples/index.html>`_.

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
