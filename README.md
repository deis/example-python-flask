Python Quick Start Guide
========================

This guide will walk you through deploying a Python application on AWS using OpDemand.

Prerequisites
--------------
* A [free OpDemand account](https://app.opdemand.com/signup) with
  * Valid AWS credentials
  * Linked GitHub account
* The OpDemand Command Line Interface
* A Python application that is **hosted on GitHub**

Clone your Application
----------------------
The simplest way to get started is by forking OpDemand's sample application located at:
<https://github.com/opdemand/example-python-flask>

After forking the project, clone it to your local workstation using the SSH-style URL:

    $ git clone git@github.com:gabrtv/example-python-flask.git example-python-flask
    $ cd example-python-flask

If you want to use an existing application, no problem -- just make sure you've cloned it from GitHub.

Prepare your Application
------------------------
To use a Python application with OpDemand, you will need to conform to 3 basic requirements:

 * Use **virtualenv and pip** to manage dependencies
 * Use **Foreman** to manage processes
 * Use **Environment Variables** to manage configuration

If you're deploying the example application, it already conforms to these requirements.  If you're in a rush, skip to [Create a Platform](#create).

### Use virtualenv and pip to manage dependencies

On every deploy action, OpDemand will run an `source venv/bin/activate` followed by a `pip install -r requirements.txt` on all application workers to ensure dependencies are up to date.  

To setup and activate virtualenv on your local workstation:

    $ virtualenv venv --distribute
    $ source venv/bin/activate

The last command will activate virtualenv for your current shell session.  To ensure you're using virtualenv and local dependencies, you'll need to re-run the `source venv/bin/activate` command for each new shell session.  With virtualenv active, you can safely install external dependencies for your application:

    $ pip install flask gunicorn

When you're done installing dependencies, use `pip freeze` to write out a new `requirements.txt` file that contains dependency information:

    $ pip freeze > requirements.txt

### Use Foreman to manage processes

OpDemand uses a Foreman Procfile to manage the processes that serve up your application.  The `Procfile` is how you define the command(s) used to run your application.  Here is an example `Procfile` that uses gunicorn:

	web: gunicorn -b 0.0.0.0:$APPLICATION_PORT -k gevent app:app

This tells OpDemand to run one web process using `gunicorn`.  You can test this out locally by running setting the `APPLICATION_PORT` environment variable and calling `foreman start`.

    $ export APPLICATION_PORT=8080
	$ foreman start
    12:45:57 web.1     | started with pid 26809
    12:45:58 web.1     | 2012-05-10 12:45:57 [26809] [INFO] Starting gunicorn 0.14.2
    12:45:58 web.1     | 2012-05-10 12:45:58 [26809] [INFO] Listening at: http://0.0.0.0:8080 (26809)
    12:45:58 web.1     | 2012-05-10 12:45:58 [26809] [INFO] Using worker: gevent
    12:45:58 web.1     | 2012-05-10 12:45:58 [26811] [INFO] Booting worker with pid: 26811
    
### Use Environment Variables to manage configuration

OpDemand uses environment variables to manage your application's configuration.  For example, the application listener must use the value of the `APPLICATION_PORT` environment variable.  The following code snippets demonstrates how this can work inside your application:

	port = os.environ.get('APPLICATION_PORT', 8080)    # fallback to 8080

The same is true for external services like databases, caches and queues.  Here is an example in that shows how to connect to a MongoDB database using the `DATABASE_HOST` and `DATABASE_PORT` environment variables:

    import pymongo
    database_host = os.environ.get('DATABASE_HOST', 'localhost')
    database_port = os.environ.get('DATABASE_PORT', 27017)
    connection = pymongo.Connection(database_host, database_port)

Create a new Platform
---------------------
Use the `opdemand list` command to list the available infrastructure templates:

	$ opdemand list | grep python
    app/python/1node: Python Application (1-node)
    app/python/2node: Python Application (2-node with ELB)
    app/python/4node: Python Application (4-node with ELB)
    app/python/Nnode: Python Application (Auto Scaling)

Use the `opdemand create` command to create a new platform based on one of the templates listed.  To create an `app/python/1node` platform with `app` as its handle/nickname.

	$ opdemand create app --template=app/python/1node

Configure the Platform
----------------------
To quickly configure a platform from the command-line use `opdemand config [handle] --repository=detect`.  This will attempt to detect and install repository configuration including:

* Detecting your GitHub repository URL, project and username
* Generating and installing a secure SSH Deploy Key

More detailed configuration can be done using:

	$ opdemand config app					   # the entire config wizard (all sections)
	$ opdemand config app --section=provider   # only the "provider" section

Detailed configuration changes are best done via the web console, which exposes additional helpers, drop-downs and overrides.

Start the Platform
------------------
To start your platform use the `opdemand start` command:

	$ opdemand start app
	
You will see real-time streaming log output as OpDemand orchestrates the platform's infrastructure and triggers the necessary SSH deployments.  Once the platform has finished starting you can access its services using an `opdemand show`.

    $ opdemand show app

	Application URL (URL used to access this application)
	http://ec2-23-20-231-188.compute-1.amazonaws.com

Open the URL and you should see "Powered by OpDemand" in your browser.  To check on the status of your platforms, use the `opdemand status` command:

	$ opdemand status
	app: Python Application (1-node) (status: running)

Deploy the Platform
----------------------
As you make changes to your application code, push those to GitHub as you would normally.  When you're ready to deploy those changes, use the `opdemand deploy` command:

	$ opdemand deploy app

This will trigger an OpDemand deploy action which will -- among other things -- update configuration settings, pull down the latest source code, install new dependencies and restart services where necessary.


Additional Resources
====================
* <http://www.opdemand.com>

