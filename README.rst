=====================================================================
 Aipo: Simple AsyncIO task/job queue execution framework
=====================================================================

:Version: 0.1.0
:Download: http://pypi.org/project/aipo
:Source: http://github.com/CaiqueReinhold/aipo
:Keywords: asyncio, task, job, queue

What is Aipo?
=============

Aipo is a minimal framework for dispatching and running background tasks using asynchronous python.

Using the task decorator you can turn your python functions into tasks that can be dispatched to a queue and executed in the aipo server.

Setting up your aipo app::

    app = Aipo(config='aipo.yaml')

    @app.task
    async def my_task():
        await get_stuff()
        await comunicate_stuff()


Once you execute you task it will be dispatched to the queue and executed in the aipo server::

    await my_task()

Run your Aipo server using the command line::

    python -m aipo --app my_app:app [--loop uvloop]


Configuration
==============
You can configure your Aipo app either by passing a dictionary or a yaml file::

    app = Aipo(config={
        'broker_url': 'redis://localhost:6379/0',
        'broker_params': {
            'username': 'username',
            'password': 'password'
        },
    })

    app = Aipo(config='aipo.yaml')


Configuration parameters:
+++++++++++++++++++++++++

- ``broker_url``: The url of the broker you want to use. Currently only redis is supported.
- ``broker_params``: A dictionary of parameters to pass to the broker. See broker specific documentation for more information.
- ``broker_class``: The class of the broker you want to use. Defaults to ``aipo.backends.redis.RedisBroker``.
- ``logging``: Dictionary of logging configuration. See python logging documentation for more information.
- ``default_queue``: Default queue used to exchanged messages. For tasks that don't specify a queue this will be used.
- ``max_concurrent_tasks``: Maximum number of concurrent tasks that can be executed. Defaults to 100.
- ``serializer_class``: The class of the serializer you want to use. Defaults to ``aipo.serializers.JSONSerializer``.

Redis broker parameters:
++++++++++++++++++++++++

- ``username``: [str] Username to authenticate with redis. Optional.
- ``password``: [str] Password to authenticate with redis. Optional.
- ``max_connections``: [int] Maximum number of connections to redis in the connection pool. Optional.
- ``socket_timeout``: [float] Timeout for socket operations. Optional.
- ``socket_connect_timeout``: [float] Timeout for socket connection. Optional.
- ``socket_keepalive``: [bool] Enable TCP keepalive on sockets. Optional.
- ``socket_keepalive_options``: [dict] TCP keepalive options. Optional.
- ``health_check_interval``: [float] How often to check the health of connections in the pool. Optional.


Installation
=============

You can install Aipo from the Python Package Index (PyPI).

To install using `pip`::

    $ pip install aipo


Currently supported Aipo backends
==================================

- Redis


Currently supported event loops
================================

- asyncio
- uvloop
