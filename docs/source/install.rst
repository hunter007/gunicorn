============
安装
============

.. highlight:: bash

:需求: **Python 2.x >= 2.6** 或者 **Python 3.x >= 3.2**

安装最新版本::

  $ pip install gunicorn

源代码安装
===========

执行下面的命令从源代码库安装::

    $ pip install git+https://github.com/benoitc/gunicorn.git

要拉取最新代码的话，执行::

    $ pip install -U git+https://github.com/benoitc/gunicorn.git


异步 workers
=============

如果希望在请求处理期间暂停代码执行，你可能会安装 Eventlet_ 或者 Gevent_。当考虑这两种
worker 类型之一时，看看 `设计`_ 可以了解更多信息。

::

    $ pip install greenlet  # 两者都需要
    $ pip install eventlet  # eventlet workers需要
    $ pip install gevent    # gevent workers需要

.. note::
    如果安装 ``greenlet`` 失败，你可能需要安装 Python 头文件。这在多数包管理系统中都可用。
    在 Ubuntu 的 ``apt-get`` 中叫做 ``python-dev``。

    Gevent_ 也依赖 ``libevent`` 1.4.x 或者 2.0.4。这可能是在包管理器中最新的版本。如果
    Gevent_ 构建失败，既使安装了 libevent_ ，这是最可能的原因。


Debian GNU/Linux
================

如果你在使用 Debian GNU/Linux，如非想在 virtualenv 中使用不同版本的 Gunicorn，建议你使用
系统的包管理器安装 Gunicorn。这有几个好处：

* 安装简单：根据在 ``/etc/gunicorn.d`` 的配置，自动启动多个 Gunicorn 实例。

* 合理的日志默认位置（``/var/log/gunicorn``）。通过 ``logrotate`` ，日志被自动循环和压缩。

* 增强的安全性：可以容易地为每个 Gunicorn 实例分配不同的用户/组。

* 合理的升级路径：升级到新版本需要更少的停机时间，用配置项处理冲突，不兼容时快速回滚。也可以在几秒内就从系统中清除掉 Gunicorn。

稳定版 ("jessie")
-----------------

Debian_ 的稳定版中，Gunicorn 的版本是 19.0(2014年6月)。你可以使用下面命令安装::

    $ sudo apt-get install gunicorn

也可以通过 `Debian Backports`_ 使用最新版本。首先，复制下面这行代码到
``/etc/apt/siources.list``::

    deb http://backports.debian.org/debian-backports jessie-backports main

然后更新本地包列表::

    $ sudo apt-get update

使用下面命令安装最新版本::

    $ sudo apt-get -t jessie-backports install gunicorn

老版本 ("wheezy")
--------------------

Debian_ 老的稳定版中，Gunicorn 版本是 0.14.5（2012年6月）。要安装则执行下面的命令::

    $ sudo apt-get install gunicorn

你还可以通过 `Debian Backports`_ 安装最新版本。首先，复制下面代码到 ``/etc/apt/sources.list``::

    deb http://backports.debian.org/debian-backports wheezy-backports main

然后更新本地包列表::

    $ sudo apt-get update

使用下面命令安装最新版本::

    $ sudo apt-get -t wheezy-backports install gunicorn

测试版 ("stretch") / 非稳定版 ("sid")
--------------------------------------

"stretch" 和 "sid" 包含 Gunicorn 的最新版，你可以用之前的方法安装::

    $ sudo apt-get install gunicorn


Ubuntu
======

Ubuntu_ 12.04 (trusty) 和之后的版本默认包含 Gunicorn，所以安装很简单::

    $ sudo apt-get update
    $ sudo apt-get install gunicorn


.. _`设计`: design.html
.. _Eventlet: http://eventlet.net
.. _Gevent: http://gevent.org
.. _libevent: http://monkey.org/~provos/libevent
.. _Debian: http://www.debian.org/
.. _`Debian Backports`: http://backports.debian.org/
.. _Ubuntu: http://www.ubuntu.com/
