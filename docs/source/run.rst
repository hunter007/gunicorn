================
运行 Gunicorn
================

.. highlight:: bash

可以通过命令行运行 Gunicorn，也可以和 Django 或者 Paster 集成。
要在生产环境中部署 Gunicorn，请查看 :doc: `部署`。

命令行
========

安装完成 Gunicorn 之后，可以通过命令行执行 gunicorn。直接执行``gnuicorn`` 即可。

.. _gunicorn-cmd:

gunicorn
--------

基本用法::

    $ gunicorn [OPTIONS] APP_MODULE

``APP_MODULE`` 的格式为： ``$(MODULE_NAME):$(VARIABLE_NAME)``。
模块名是用“.”分隔的完整路径。变量名则必须在这个模块内可找到，引用一个 WSGI 可调用对象。

用以下代码作为例子:

.. code-block:: python

    def app(environ, start_response):
        """Simplest possible application object"""
        data = 'Hello, World!\n'
        status = '200 OK'
        response_headers = [
            ('Content-type','text/plain'),
            ('Content-Length', str(len(data)))
        ]
        start_response(status, response_headers)
        return iter([data])

可以用以下命令运行例子应用::

    $ gunicorn --workers=2 test:app


常用参数
^^^^^^^^^

* ``-c CONFIG, --config=CONFIG`` - 指定配置文件。支持的格式：
  ``$(PATH)``, ``file:$(PATH)``, or ``python:$(MODULE_NAME)``.
* ``-b BIND, --bind=BIND`` - 指定 Server 的绑定信息. Server sockets支持以下格式：
  ``$(HOST)``, ``$(HOST):$(PORT)``, 或者 ``unix:$(PATH)``. IP 也可用于 ``$(HOST)``。
* ``-w WORKERS, --workers=WORKERS`` - worker 进程数. 这个值通常是 2-4个进程/核。
  查看 :ref:`faq`以获得如何调优此参数的信息。
* ``-k WORKERCLASS, --worker-class=WORKERCLASS`` - 要运行的 worker 进程类型。
   You'll definitely want to read the production page for the
  implications of this parameter. 可用的值有： ``sync``, ``eventlet``, ``gevent``, 或者
  ``tornado``, ``gthread``, ``gaiohttp``。 ``sync`` 是默认值。
* ``-n APP_NAME, --name=APP_NAME`` - 如果安装了 setproctitle_ ，你可以设置此进程在
  系统进程表中显示的名字。这会影响一些显示进程的工具，如 ``ps`` 和 ``top``。

设置项可以通过环境变量指定。请参考 :ref:`GUNICORN_CMD_ARGS <settings>`。

查看 :ref:`配置` 和 :ref:`设置` 可以了解更详细的信息。

.. _setproctitle: http://pypi.python.org/pypi/setproctitle/

集成
====

目前支持和 Django 和 Paster 的集成。

Django
------

没有指定名称的情况下，Gunicorn 会去查找一个叫做 ``application`` 的 WSGI 可调用对象。
所以，对典型的 Django 项目来说，像这样调用 Gunicorn::

    $ gunicorn myproject.wsgi

.. note::

    前提是你的项目在 Python 路径上。最简单的确定方式是：在和 ``manage.py`` 同目录下运行这个命令。

你可以用
`--env <http://docs.gunicorn.org/en/latest/settings.html#raw-env>`_ 选项来设置
加载 Django 的 settings 的路径。当你需要的时候，可以用
`--pythonpath <http://docs.gunicorn.org/en/latest/settings.html#pythonpath>`_
把应用的路径添加到 ``PYTHONPATH`` 中::

    $ gunicorn --env DJANGO_SETTINGS_MODULE=myproject.settings myproject.wsgi

Paste
-----

如果你是一个兼容 paster 的框架/应用（Pyramid, Pylons 和 Turbogears）的用户/开发者，可以用
选项 `--paste <http://docs.gunicorn.org/en/latest/settings.html#paste>`_ 运行应用。
例如::

    $ gunicorn --paste development.ini -b :8080 --chdir /path/to/project

或者用另一个应用::

    $ gunicorn --paste development.ini#admin -b :8080 --chdir /path/to/project

这就是全部了。没有配置文件和额外的 Python 模块要写！
