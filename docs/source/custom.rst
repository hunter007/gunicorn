.. _custom:

=========
自定义应用
=========

.. 支持的版本:: 从19.0开始

有时候，我们希望将 Gunicorn 集成到我们的 web 应用中，我们可以继承类 :class:`gunicorn.app.base.BaseApplication`。

以下是一个小例子。我们创建一个非常简单的 WSGI 应用：

.. code-block:: python

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-
    #
    # An example of a standalone application using the internal API of Gunicorn.
    #
    #   $ python standalone_app.py
    #
    # This file is part of gunicorn released under the MIT license.
    # See the NOTICE for more information.

    from __future__ import unicode_literals

    import multiprocessing

    import gunicorn.app.base

    from gunicorn.six import iteritems


    def number_of_workers():
        return (multiprocessing.cpu_count() * 2) + 1


    def handler_app(environ, start_response):
        response_body = b'Works fine'
        status = '200 OK'

        response_headers = [
            ('Content-Type', 'text/plain'),
        ]

        start_response(status, response_headers)

        return [response_body]


    class StandaloneApplication(gunicorn.app.base.BaseApplication):

        def __init__(self, app, options=None):
            self.options = options or {}
            self.application = app
            super(StandaloneApplication, self).__init__()

        def load_config(self):
            config = dict([(key, value) for key, value in iteritems(self.options)
                        if key in self.cfg.settings and value is not None])
            for key, value in iteritems(config):
                self.cfg.set(key.lower(), value)

        def load(self):
            return self.application


    if __name__ == '__main__':
        options = {
            'bind': '%s:%s' % ('127.0.0.1', '8080'),
            'workers': number_of_workers(),
        }
        StandaloneApplication(handler_app, options).run()

.. note::
    以上代码是example/standalone_app.py的内容。为了使读者方便，复制到此。