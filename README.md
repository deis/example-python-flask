# Python Quick Start Guide

This guide will walk you through deploying a Python / Flask application on [Deis Workflow][].

## Usage

```console
$ git clone https://github.com/deis/example-python-flask.git
$ cd example-python-flask
$ deis create
Creating Application... done, created queens-airfield
Git remote deis added
remote available at ssh://git@deis-builder.deis.rocks:2222/queens-airfield.git
$ git push deis master
Counting objects: 108, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (95/95), done.
Writing objects: 100% (108/108), 23.81 KiB | 0 bytes/s, done.
Total 108 (delta 41), reused 0 (delta 0)
Starting build... but first, coffee!
-----> Python app detected
-----> Installing python-2.7.11
       $ pip install -r requirements.txt
       Collecting Flask==0.11 (from -r requirements.txt (line 1))
       Downloading Flask-0.11-py2.py3-none-any.whl (80kB)
       Collecting Jinja2==2.8 (from -r requirements.txt (line 2))
       Downloading Jinja2-2.8-py2.py3-none-any.whl (263kB)
       Collecting gunicorn==19.6.0 (from -r requirements.txt (line 3))
       Downloading gunicorn-19.6.0-py2.py3-none-any.whl (114kB)
       Collecting click>=2.0 (from Flask==0.11->-r requirements.txt (line 1))
       Downloading click-6.6.tar.gz (283kB)
       Collecting itsdangerous>=0.21 (from Flask==0.11->-r requirements.txt (line 1))
       Downloading itsdangerous-0.24.tar.gz (46kB)
       Collecting Werkzeug>=0.7 (from Flask==0.11->-r requirements.txt (line 1))
       Downloading Werkzeug-0.11.10-py2.py3-none-any.whl (306kB)
       Collecting MarkupSafe (from Jinja2==2.8->-r requirements.txt (line 2))
       Downloading MarkupSafe-0.23.tar.gz
       Installing collected packages: click, itsdangerous, Werkzeug, MarkupSafe, Jinja2, Flask, gunicorn
       Running setup.py install for click: started
       Running setup.py install for click: finished with status 'done'
       Running setup.py install for itsdangerous: started
       Running setup.py install for itsdangerous: finished with status 'done'
       Running setup.py install for MarkupSafe: started
       Running setup.py install for MarkupSafe: finished with status 'done'
       Successfully installed Flask-0.11 Jinja2-2.8 MarkupSafe-0.23 Werkzeug-0.11.10 click-6.6 gunicorn-19.6.0 itsdangerous-0.24

-----> Discovering process types
       Procfile declares types -> web
-----> Compiled slug size is 37M
Build complete.
Launching App...
Done, queens-airfield:v2 deployed to Deis

Use 'deis open' to view this application in your browser

To learn more, use 'deis help' or visit https://deis.com/

To ssh://git@deis-builder.deis.rocks:2222/queens-airfield.git
 * [new branch]      master -> master
$ curl http://queens-airfield.deis.rocks
Powered by Deis
```

## Additional Resources

* [GitHub Project](https://github.com/deis/workflow)
* [Documentation](https://deis.com/docs/workflow/)
* [Blog](https://deis.com/blog/)

[Deis Workflow]: https://github.com/deis/workflow#readme
