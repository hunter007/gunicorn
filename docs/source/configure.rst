.. _配置:

======================
概述
======================

Gunicorn 从三个地方获取配置信息。

第一个位置是框架的配置文件，当前仅仅对 Paster 有效。

第二个位置是通过命令行指定的配置文件。命令行指定的配置文件的内容会覆盖掉框架的配置。

第三个位置是命令行参数。如果通过命令行指定一个参数，Gunicorn 最终会使用这个值。

也就是说，配置的优先级从低到高：

    1. 框架配置
    2. 配置文件
    3. 命令行指定


.. 注意::

    当使用命令行指定参数或者在命令行传入配置文件时，可以检测配置是否正确::

        $ gunicorn --check-config APP_MODULE

    这也让我们知道了应用是否能正常启动。


命令行指定
==========

如果在命令行指定了一个选项，这将覆盖应用中的配置和配置文件中的配置。但也不是 Gunicorn 的所有选项都可以在命令行指定。要看命令行支持的选项列表，可以执行::

    $ gunicorn -h


配置文件
========

配置文件就是一个 Python 源代码文件。只要从文件系统可读即可。比较特殊的是，不需要导入它。任何
Python代码都可以出现在这个文件中。每次 Gunicorn 启动的时候（包括给 Gunicorn 发送reload信号）
会执行一次这个配置文件。

要设置一个选项，给它赋值即可。没有特殊的语法，你给它什么值，Gunicorn 就用什么值。

比如::

    import multiprocessing

    bind = "127.0.0.1:8000"
    workers = multiprocessing.cpu_count() * 2 + 1

更多的选项在 :ref: `设置<settings>` 中。


框架配置
========
当前仅 Paster 应用支持框架配置。关于为 WSGI 应用提供配置，或者获取 Django 的 settings.py 信息，
如果你有任何想法，请打开一个 `issue`_，让我们知道。

.. _issue: http://github.com/benoitc/gunicorn/issues

Paster 应用
------------

在 INI 文件中，可以指定使用 Gunicorn 作为应用服务器：

.. code-block:: ini

    [server:main]
    use = egg:gunicorn#main
    host = 192.168.0.1
    port = 80
    workers = 2
    proc_name = brim

Gunicorn 能识别的参数会被自动地插入基本配置中。
请记住：这些选项可能会被 Gunicorn 配置文件或者命令行参数覆盖。
