======================
Gunicorn - WSGI server
======================

.. image:: _static/gunicorn.png

:网站: http://gunicorn.org
:源代码: https://github.com/benoitc/gunicorn
:问题跟踪: https://github.com/benoitc/gunicorn/issues
:IRC: ``#gunicorn`` on Freenode
:邮件列表: http://lists.gunicorn.org/user/

Gunicorn（Green Unicorn）是一个 WSGI 服务器。它从 Ruby 的 Unicorn 项目移植过来，使用 prefork
工作模型。Gunicorn 服务器广泛地兼容大量web框架，实现简单，在占用服务器资源上是轻量级的，并且相当快！

特性
----

* 原生支持 WSGI、Django 和 Paster
* worker 进程自动管理
* 使用 Python 进行配置，并且简单
* 支持多个 worker 配置
* 扩展性：支持各种服务器钩子
* Python兼容性： >=2.6 或者 >= 3.2


目录
--------

.. toctree::
    :maxdepth: 2

    安装
    运行
    配置
    设置
    仪表
    部署
    信号
    自定义
    设计
    常见问题
    社区
    新闻
