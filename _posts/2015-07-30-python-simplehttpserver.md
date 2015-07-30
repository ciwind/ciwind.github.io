---
layout: post
title: "Python, SimpleHTTPServer"
description: "python快速搭建简单http服务"
category: python
tags: [python, SimpleHttpServer]
---
{% include JB/setup %}

通过以下代码，快速搭建简单http服务。enjoy~

<pre class="code prettyprint linenums lang-python">
#!/usr/bin/python

"""
Save this file as server.py
>>> python server.py 0.0.0.0 8001
serving on 0.0.0.0:8001

or simply

>>> python server.py
Serving on localhost:8000

You can use this to test GET and POST methods.
"""

import SimpleHTTPServer
import SocketServer
import logging
import cgi
import sys

if len(sys.argv) > 2:
    PORT = int(sys.argv[2])
    I = sys.argv[1]
elif len(sys.argv) > 1:
    PORT = int(sys.argv[1])
    I = ""
else:
    PORT = 11223
    I = ""

class ServerHandler(SimpleHTTPServer.SimpleHTTPRequestHandler):

    def do_something(self, v):
        pass

    def do_GET(self):
        logging.warning("======= GET STARTED =======")
        logging.warning(self.headers)

        SimpleHTTPServer.SimpleHTTPRequestHandler.do_GET(self)

    def do_POST(self):
        logging.warning("======= POST STARTED =======")
        logging.warning(self.headers)

        # 获取参数
        form = cgi.FieldStorage(
            fp=self.rfile,
            headers=self.headers,
            environ={'REQUEST_METHOD':'POST',
                     'CONTENT_TYPE':self.headers['Content-Type'],
                     })

        v = ''
        if form.has_key('k'):
            v = form['k'].value

        self.do_something(v)

Handler = ServerHandler

httpd = SocketServer.TCPServer(("", PORT), Handler)

print "@rochacbruno Python http server version 0.1 (for testing purposes only)"
print "Serving at: http://%(interface)s:%(port)s" % dict(interface=I or "localhost", port=PORT)
httpd.serve_forever()
</pre>