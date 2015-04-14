# Python Quick Start Guide

This guide will walk you through deploying a Python application on Deis.

## Usage

```
$ deis create
Creating application... done, created timely-darkroom
Git remote deis added
$ git push deis master
Counting objects: 80, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (44/44), done.
Writing objects: 100% (80/80), 18.60 KiB | 0 bytes/s, done.
Total 80 (delta 29), reused 80 (delta 29)
-----> Python app detected
-----> No runtime.txt provided; assuming python-2.7.6.
-----> Preparing Python runtime (python-2.7.6)
-----> Installing Setuptools (2.1)
-----> Installing Pip (1.5.4)
-----> Installing dependencies using Pip (1.5.4)
       Downloading/unpacking Flask==0.9 (from -r requirements.txt (line 1))
       Running setup.py (path:/tmp/pip_build_root/Flask/setup.py) egg_info for package Flask

       warning: no files found matching '*' under directory 'tests'
       no previously-included directories found matching 'docs/_build'
       no previously-included directories found matching 'docs/_themes/.git'
       Downloading/unpacking Jinja2==2.6 (from -r requirements.txt (line 2))
       Running setup.py (path:/tmp/pip_build_root/Jinja2/setup.py) egg_info for package Jinja2

       Downloading/unpacking gunicorn==0.17.2 (from -r requirements.txt (line 3))
       Running setup.py (path:/tmp/pip_build_root/gunicorn/setup.py) egg_info for package gunicorn

       Downloading/unpacking Werkzeug>=0.7 (from Flask==0.9->-r requirements.txt (line 1))
       Running setup.py (path:/tmp/pip_build_root/Werkzeug/setup.py) egg_info for package Werkzeug

       warning: no files found matching '*' under directory 'werkzeug/debug/templates'
       warning: no files found matching '*' under directory 'tests'
       no previously-included directories found matching 'docs/_build'
       Installing collected packages: Flask, Jinja2, gunicorn, Werkzeug
       Running setup.py install for Flask

       warning: no files found matching '*' under directory 'tests'
       no previously-included directories found matching 'docs/_build'
       no previously-included directories found matching 'docs/_themes/.git'
       Running setup.py install for Jinja2

       Running setup.py install for gunicorn

       Installing gunicorn_paster script to /app/.heroku/python/bin
       Installing gunicorn script to /app/.heroku/python/bin
       Installing gunicorn_django script to /app/.heroku/python/bin
       Running setup.py install for Werkzeug

       warning: no files found matching '*' under directory 'werkzeug/debug/templates'
       warning: no files found matching '*' under directory 'tests'
       no previously-included directories found matching 'docs/_build'
       Successfully installed Flask Jinja2 gunicorn Werkzeug
       Cleaning up...
-----> Discovering process types
       Procfile declares types -> web
-----> Compiled slug size is 31M
remote: -----> Building Docker image
remote: Uploading context 31.75 MB
remote: Uploading context
remote: Step 0 : FROM deis/slugrunner
remote:  ---> 5567a808891d
remote: Step 1 : RUN mkdir -p /app
remote:  ---> Using cache
remote:  ---> 4096b5c0b838
remote: Step 2 : ADD slug.tgz /app
remote:  ---> c2c3aa045300
remote: Removing intermediate container bc85fb8c3b74
remote: Step 3 : ENTRYPOINT ["/runner/init"]
remote:  ---> Running in c919bc22157f
remote:  ---> d5bd87f455c8
remote: Removing intermediate container c919bc22157f
remote: Successfully built d5bd87f455c8
remote: -----> Pushing image to private registry
remote:
remote:        Launching... done, v2
remote:
remote: -----> timely-darkroom deployed to Deis
remote:        http://timely-darkroom.local.deisapp.com
remote:
remote:        To learn more, use `deis help` or visit http://deis.io
remote:
To ssh://git@local.deisapp.com:2222/timely-darkroom.git
 * [new branch]      master -> master
$ curl http://timely-darkroom.local.deisapp.com
Powered by Deis!
```

## Additional Resources

* [Get Deis](http://deis.io/get-deis/)
* [GitHub Project](https://github.com/deis/deis)
* [Documentation](http://docs.deis.io/)
* [Blog](http://deis.io/blog/)
