# Python Quick Start Guide

This guide will walk you through deploying a Python application on Deis.

## Prerequisites

* A [User Account](http://docs.deis.io/en/latest/client/register/) on a [Deis Controller](http://docs.deis.io/en/latest/terms/controller/).
* A [Deis Formation](http://docs.deis.io/en/latest/gettingstarted/concepts/#formations) that is ready to host applications

If you do not yet have a controller or a Deis formation, please review the [Deis installation](http://docs.deis.io/en/latest/gettingstarted/installation/) instructions.

## Setup your workstation

* Install [RubyGems](http://rubygems.org/pages/download) to get the `gem` command on your workstation
* Install [Foreman](http://ddollar.github.com/foreman/) with `gem install foreman`
* Install [Python](http://www.python.org/getit/) (we still recommend 2.7.x for library compatibility)

## Clone your Application

If you want to use an existing application, no problem.  You can also use the Deis sample application located at <https://github.com/bengrunfeld/example-python-flask>.  Clone the example application to your local workstation:

    $ git clone https://github.com/bengrunfeld/example-python-flask.git
    $ cd example-python-flask

## Prepare your Application

To use a Python application with Deis, you will need to conform to 3 basic requirements:

 1. Use [Pip](http://pypi.python.org/pypi/pip) to manage dependencies
 2. Use [Foreman](http://ddollar.github.com/foreman/) to manage processes
 3. Use [Environment Variables](https://help.ubuntu.com/community/EnvironmentVariables) to manage configuration inside your application

If you're deploying the example application, it already conforms to these requirements.

#### 1. Use Pip to manage dependencies

Pip requires that you explicitly declare your dependencies using a [requirements.txt](http://www.pip-installer.org/en/latest/requirements.html) file. Here is a very basic example:

    Flask==0.9
    Jinja2==2.6
    gunicorn==0.17.2

We highly recommend isolating your dependencies inside a Python [virtualenv](https://python-guide.readthedocs.org/en/latest/dev/virtualenvs/):

    $ virtualenv venv                        # create the virtualenv
    New python executable in venv/bin/python
    Installing setuptools............done.
    Installing pip...............done.
    $ source venv/bin/activate               # activate the virtualenv
    
You can then install dependencies on your local workstation with `pip install -r requirements.txt`:

    (venv)$ pip install -r requirements.txt 
    Downloading/unpacking Flask==0.9 (from -r requirements.txt (line 1))
    Downloading Flask-0.9.tar.gz (481kB): 481kB downloaded
    Running setup.py egg_info for package Flask
    ...
    Successfully installed Flask gunicorn Werkzeug
    Cleaning up...

#### 2. Use Foreman to manage processes

Deis relies on a [Foreman](http://ddollar.github.com/foreman/) `Procfile` that lives in the root of your repository.  This is where you define the command(s) used to run your application.  Here is an example `Procfile`:

    web: gunicorn -b 0.0.0.0:$PORT app:app

This tells Deis to run `web` workers using the command `gunicorn -b 0.0.0.0:$PORT app:app`. You can test this locally by running `foreman start`.

    (venv)$ foreman start
    15:05:12 web.1  | started with pid 88517
    15:05:12 web.1  | 2013-04-06 15:05:12 [88517] [INFO] Starting gunicorn 0.17.2
    15:05:12 web.1  | 2013-04-06 15:05:12 [88517] [INFO] Listening at: http://0.0.0.0:5000 (88517)
    15:05:12 web.1  | 2013-04-06 15:05:12 [88517] [INFO] Using worker: sync
    15:05:12 web.1  | 2013-04-06 15:05:12 [88520] [INFO] Booting worker with pid: 88520

You should now be able to access your application locally at <http://localhost:5000>.

#### 3. Use Environment Variables to manage configuration

Deis uses environment variables to manage your application's configuration. For example, your application listener must use the value of the `PORT` environment variable. The following code snippet demonstrates how this can work inside your application:

    port = os.environ.get('PORT', 5000)    # fallback to 5000

## Create a new Application

Per the prerequisites, we assume you have access to an existing Deis formation. If not, please review the Deis [installation instuctions](http://docs.deis.io/en/latest/gettingstarted/installation/).

Use the following command to create an application on an existing Deis formation.

    $ deis create --formation=<formationName> --id=<appName>
    <show output>
    Creating application... done, created pythonApp
    Git remote deis added
    
If an ID is not provided, one will be auto-generated for you.

## Deploy your Application

Use `git push` to deploy your application.

    $ git push deis master
    <show output>
    Counting objects: 62, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (37/37), done.
    Writing objects: 100% (62/62), 12.98 KiB, done.
    Total 62 (delta 18), reused 62 (delta 18)
gi         Python app detected


Once your application has been deployed, use `deis open` to view it in a browser. To find out more info about your application, use `deis info`.

## Scale your Application

To scale your application's [Docker](http://docker.io) containers, use `deis scale`.

    $ deis scale web=8
    <show output>
    Scaling containers... but first, coffee!
    done in 16s
    
    === elfish-radiator Containers
    
    --- web: `gunicorn -b 0.0.0.0:$PORT app:app`
    web.1 up 2013-10-23T19:02:35.666Z (pythonFormation-runtime-1)
    web.2 up 2013-10-23T19:05:33.067Z (pythonFormation-runtime-1)
    web.3 up 2013-10-23T19:05:33.082Z (pythonFormation-runtime-1)
    web.4 up 2013-10-23T19:05:33.095Z (pythonFormation-runtime-1)
    web.5 up 2013-10-23T19:05:33.109Z (pythonFormation-runtime-1)
    web.6 up 2013-10-23T19:05:33.125Z (pythonFormation-runtime-1)
    web.7 up 2013-10-23T19:05:33.143Z (pythonFormation-runtime-1)
    web.8 up 2013-10-23T19:05:33.162Z (pythonFormation-runtime-1)


## Configure your Application

Deis applications are configured using environment variables. The example application includes a special `POWERED_BY` variable to help demonstrate how you would provide application-level configuration. 

    $ curl -s http://yourapp.com
    Powered by Deis
    $ deis config:set POWERED_BY=Python
    <show output>
    === elfish-radiator
    POWERED_BY: Python
    $ curl -s http://yourapp.com
    Powered by Python

This method is also how you connect your application to backing services like databases, queues and caches.

To experiment in your application environment, use `deis run` to execute one-off commands against your application.

    $ deis run ls -la
    <show output>
    total 60
    drwxr-xr-x  4 root root 4096 Oct 23 19:12 .
    drwxr-xr-x 57 root root 4096 Oct 23 19:14 ..
    -rw-r--r--  1 root root  237 Oct 23 19:01 .gitignore
    drwxr-xr-x  3 root root 4096 Oct 23 19:01 .heroku
    drwxr-xr-x  2 root root 4096 Oct 23 19:02 .profile.d
    -rw-r--r--  1 root root   18 Oct 23 19:02 .release
    -rw-r--r--  1 root root  553 Oct 23 19:02 LICENSE
    -rw-r--r--  1 root root   39 Oct 23 19:02 Procfile
    -rw-r--r--  1 root root 9424 Oct 23 19:02 README.md
    -rw-r--r--  1 root root  330 Oct 23 19:02 app.py
    -rw-r--r--  1 root root  602 Oct 23 19:12 app.pyc
    -rw-r--r--  1 root root   40 Oct 23 19:02 requirements.txt
    -rw-r--r--  1 root root   13 Oct 23 19:02 runtime.txt

## Troubleshoot your Application

To view your application's log output, including any errors or stack traces, use `deis logs`.

    $ deis logs
    <show output>

## Additional Resources

* [Get Deis](http://deis.io/get-deis/)
* [GitHub Project](https://github.com/opdemand/deis)
* [Documentation](http://docs.deis.io/)
* [Blog](http://deis.io/blog/)
