# Python Quick Start Guide

This guide will walk you through deploying a Python / Flask application on [Deis Workflow][].

## Usage

```console
$ git clone https://github.com/deis/example-python-flask.git
$ cd example-python-flask
$ deis create
Creating application... done, created padded-gemstone
Git remote deis added
$ git push deis master
Counting objects: 96, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (86/86), done.
Writing objects: 100% (96/96), 20.88 KiB | 0 bytes/s, done.
Total 96 (delta 38), reused 0 (delta 0)
-----> Python app detected
-----> Installing runtime (python-2.7.9)
-----> Installing dependencies with pip
       Collecting Flask==0.10.1 (from -r requirements.txt (line 1))
       Downloading Flask-0.10.1.tar.gz (544kB)
       Collecting Jinja2==2.7.3 (from -r requirements.txt (line 2))
       Downloading Jinja2-2.7.3.tar.gz (378kB)
       Collecting gunicorn==19.3.0 (from -r requirements.txt (line 3))
       Downloading gunicorn-19.3.0-py2.py3-none-any.whl (110kB)
       Collecting Werkzeug>=0.7 (from Flask==0.10.1->-r requirements.txt (line 1))
       Downloading Werkzeug-0.10.4-py2.py3-none-any.whl (293kB)
       Collecting itsdangerous>=0.21 (from Flask==0.10.1->-r requirements.txt (line 1))
       Downloading itsdangerous-0.24.tar.gz (46kB)
       Collecting markupsafe (from Jinja2==2.7.3->-r requirements.txt (line 2))
       Downloading MarkupSafe-0.23.tar.gz
       Installing collected packages: Werkzeug, markupsafe, Jinja2, itsdangerous, Flask, gunicorn
       Running setup.py install for markupsafe
       Running setup.py install for Jinja2
       Running setup.py install for itsdangerous
       Running setup.py install for Flask
       Successfully installed Flask-0.10.1 Jinja2-2.7.3 Werkzeug-0.10.4 gunicorn-19.3.0 itsdangerous-0.24 markupsafe-0.23

-----> Discovering process types
       Procfile declares types -> web
-----> Compiled slug size is 38M

-----> Building Docker image
remote: Sending build context to Docker daemon 39.45 MB
remote: build context to Docker daemon
Step 0 : FROM deis/slugrunner
 ---> 385e8129fbdf
Step 1 : RUN mkdir -p /app
 ---> Using cache
 ---> 877ce2869c1c
Step 2 : WORKDIR /app
 ---> Using cache
 ---> 40b847b64730
Step 3 : ENTRYPOINT /runner/init
 ---> Using cache
 ---> fe7cf39f55ff
Step 4 : ADD slug.tgz /app
 ---> 0dab16cd6994
Removing intermediate container 2121d50679e5
Step 5 : ENV GIT_SHA 91e48e7f071dd70676dd3d050421aaab52687ee8
 ---> Running in eb00fd106d3b
 ---> c66474feaa32
Removing intermediate container eb00fd106d3b
Successfully built c66474feaa32
-----> Pushing image to private registry

-----> Launching...
       done, padded-gemstone:v2 deployed to Deis

       http://padded-gemstone.local3.deisapp.com

       To learn more, use `deis help` or visit http://deis.io

To ssh://git@deis.local3.deisapp.com:2222/padded-gemstone.git
 * [new branch]      master -> master
$ curl http://padded-gemstone.local3.deisapp.com
Powered by Deis
$ deis config:set POWERED_BY="Engine Yard"
Creating config... done, v3

=== padded-gemstone
DEIS_APP: padded-gemstone
POWERED_BY: Engine Yard
$ curl http://padded-gemstone.local3.deisapp.com
Powered by Engine Yard
```

## Additional Resources

* [GitHub Project](https://github.com/deis/workflow)
* [Documentation](https://deis.com/docs/workflow/)
* [Blog](https://deis.com/blog/)

[Deis Workflow]: https://github.com/deis/workflow#readme
