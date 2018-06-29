Python 3.7 has been released
============================

Really exciting news - Python 3.7 has been released!

So what's inside?

.. image:: https://www.python.org/static/community_logos/python-logo-master-v3-TM.png
   :align: left
   :width: 200

As it stated on python.org:

Python 3.7.0 is the newest major release of the Python language, and it
contains many new features and optimizations.

.. more::

Data classes
------------

Most interesting feature for myself is `data classes <https://docs.python.org/3/library/dataclasses.html>`_:

.. code-block:: python

    @dataclass
    class Point:
        x: float
        y: float
        z: float = 0.0

    p = Point(1.5, 2.5)
    print(p)   # produces "Point(x=1.5, y=2.5, z=0.0)"

I feel curious and happy at the same time because some time ago I had an idea
to open source sort of similar thing -
`Domain models <https://github.com/ets-labs/python-domain-models>`_.

.. code-block:: python

    from domain_models import models, fields


    class Photo(models.DomainModel):
        id = fields.Int()
        storage_path = fields.String()


    class Profile(models.DomainModel):
        id = fields.Int()
        name = fields.String()
        main_photo = fields.Model(Photo)
        photos = fields.Collection(Photo)
        birth_date = fields.Date()

History **Domain models** of is quite regular - it was extracted from one of
production projects that I had to work for. Unfortunately, nor
`Sergii [boonya] Buinytskyi <https://github.com/boonya>`_ (who actively
contributed in this project) neither mine efforts were not enough to give it
proper publicity.

Context variables
-----------------

Another quite useful new feature is context variables -
`contextvars <https://docs.python.org/3/library/contextvars.html>`_.

What I find the most beneficial is that this primitive is integrated
with `asyncio <https://docs.python.org/3/library/asyncio.html>`_ that slightly
improve management of contexts in asynchronous programming:


.. code-block:: python

    import asyncio
    import contextvars

    client_addr_var = contextvars.ContextVar('client_addr')

    def render_goodbye():
        # The address of the currently handled client can be accessed
        # without passing it explicitly to this function.

        client_addr = client_addr_var.get()
        return f'Good bye, client @ {client_addr}\n'.encode()

    async def handle_request(reader, writer):
        addr = writer.transport.get_extra_info('socket').getpeername()
        client_addr_var.set(addr)

        # In any code that we call is now possible to get
        # client's address by calling 'client_addr_var.get()'.

        while True:
            line = await reader.readline()
            print(line)
            if not line.strip():
                break
            writer.write(line)

        writer.write(render_goodbye())
        writer.close()

    async def main():
        srv = await asyncio.start_server(
            handle_request, '127.0.0.1', 8081)

        async with srv:
            await srv.serve_forever()

    asyncio.run(main())

    # To test it you can use telnet:
    #     telnet 127.0.0.1 8081


Other features
--------------

Besides of what declared above, there are lot more cool things done in this
release.

Full list of features: https://www.python.org/downloads/release/python-370/

Enjoy Python!

.. author:: default
.. categories:: none
.. tags:: none
.. comments::
