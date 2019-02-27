# Django

**Django** is a Python web framework designed for enterprise-scale application. [Ion](../../services/ion/), [Director](../../services/director/), and [Othello](../../services/othello/) are all Django applications.

For more information about the framework itself, see the Django website at [https://www.djangoproject.com/](https://www.djangoproject.com/).

### Quickstart Guide

This guide is meant to be a simple explanation of how Django works, not really a complete one. You should really read the entire thing twice through because there's so much interconnected stuff. Really the thing to read is [https://docs.djangoproject.com/en/2.1/intro/tutorial01/](https://docs.djangoproject.com/en/2.1/intro/tutorial01/) but this is more of a reference guide so meh.

#### Making/Getting a project

If there is already a git repository containing a Django project, you just have to `git clone` it and go to the directory it lives in to start working.

Otherwise, if you are starting a new project, you must install Django globally first, then run the command:

```bash
django-admin startproject <project-name>
```

This will create a new directory `project-name/` which you can `git init` a repository in and start working on.

#### Installing Django

**In a virtual environment**

I like to use virtual environments to run Django code, keeping their packages separate from cluttering up my main Python environment.

First, to create a virtual environment, run the command:

```bash
pip install --upgrade virtualenv
python -m virtualenv <venv-folder>
```

in the directory you want to create a `venv-folder/` in.

Then, you must `source activate venv-folder/bin/activate` \(or, if you are on Windows, `venv-folder\Scripts\activate.bat`\) in order to activate the virtual environment.

**Which packages tho**

You are now ready to actually start installing Django packages in your virtual environment.

If you have cloned a repository from somewhere else, there will usually be a file called `requirements.txt` at the root. Run

```bash
pip install --upgrade -r requirements.txt
```

to install all the packages it specifies. This is better than reading the file yourself because pip will automatically take care of version control and whatnot.

A sample `requirements.txt` is listed below:

```text
daphne
django
requests
Twisted[tls,http2]
```

#### Django's Directory Layout

So apparently the creators of Django decided they didn't like how normal people organized they files, and also didn't think they should add any custom organization options, so here we are with a pretty strict way of doing Django projects.

Fortunately, it's also a pretty good way and teaches best practices for modularity so there's that.

For the rest of this section, we will assume your project is called `your_project` containing a single app called `the_app`.

**Main files**

Let's say you are in the root directory of your new Django project/git repository. You should see these files/folders:

* `manage.py`: the main helper script for developing with Django. Can start the server, run db operations, etc
* `your-project/`: Your main project folder, where all of your code goes. This will have a few sub-files, as denoted by sub-bullets \(sub-folders are discussed in a later section\)
  * `settings.py`: Where all of Django's settings are stored. This file is massive, an absolute unit. Specifics discussed later.
  * `urls.py`: Where Django looks for all the URLs it should handle. URLs can be manually specified, included from other files, and all correspond to Views defined in Apps \(next section\)
  * `wsgi.py`: A small file containing everything needed for a Django server to actually server your app. Doesn't really need to be edited

There can be more files in here, but they will be project-specific.

**Apps**

Now, let's move on to what actually makes Django cool: **Apps**! Apps are small, supposedly independent modules that take care of one task. Every app is a subfolder \(`the_app/`\) in the main project directory \(\) In the Othello Server, for example, there is one app each dedicated to:

* Authentication \(`auth`\)
* User sessions \(`users`\)
* Running games \(`games`, the biggest one\)

Because apps are what makes up the meat of a Django project, they contain the most sub-files. The automatically created ones are listed below, but apps can contain as many sub-files as they want to get the functionality they need.

**views.py**

This is where you write the code to handle what happens when someone visits a page. Views \(really just Python functions that return HTML with Django helpers\) defined in this file are referenced from the main `urls.py` file or, for more complicated apps, an app-specific `the-app/urls.py` file which in turn is included in bulk in the main `urls.py` file. Got it?

**models.py**

This file defines what database objects Django needs to create. Django is super cool in that you can basically write regular ol' Python data storage objects and Django will convert them to work with any database backend you use. If you make changes to this file, you must also update the database with `python manage.py makemigrations && python manage.py migrate`

**apps.py**

Some lame file you needs to have in order for Django to not complain. Example:

```python
# your_project/the_app/apps.py
from django.apps import AppConfig


class TheAppConfig(AppConfig):
    name = 'your_project.apps.the_app'
```

**admin.py and tests.py**

Unless you do stuff on Ion or Director these don't really matter. `admin.py` defines models accessible from django-admin, and `tests.py` defines testcases so you have a less chance of breaking your code by accident \(super lame\).

**Templates**

The folder `your_project/templates/` contains all the [Jinja2](http://jinja.pocoo.org/) templates for you Django project. Basically stores all the HTML/JS/CSS that should be dynamically rendered \(mostly HTML\).

If you want good text highlighting, you might want to name these ending in `.j2` instead of `.html`. We don't really do that yet though.

**Static files**

The folder `your_project/static/` contains all of the static files, like most of your JS/CSS and all of your images. Note that this folder only really has to be here for debugging purposes: in production, Django won't touch it.

#### `settings.py` layout

Some important variables:

* `DEBUG`: controls whether we are in debug mode or not, will enable static file hosting and automatic server reload when set to `True`
* `ALLOWED_HOSTS`: All the hostnames Django will allow itself to be accessed from. `"localhost"` or `"127.0.0.1"`should be in there if you are developing.
* `INSTALLED_APPS`: All the Apps that Django will load. If you want to add an App you made to the list, append `"your_project.apps.the_app"` to it.
* `MIDDLEWARE`: some hoodoo-vooodoo black-magic security wizardry, best not to touch it unless a library says you should add them to it.
* `ROOT_URLCONF`: The file Django loads urls from. Should be set to `your_project.urls` by default and stay that way.
* `TEMPLATES`: I don't even know, man. Best not to touch it.
* `LOGGING`: All the places for Django to log it's runtime messages. You should read the actual documentation for this.
* `WSGI_APPLICATION`: The file a Django server should hook into in order to start the server. Why it is called WSGI will be covered later.
* `DATABASES`: All the places Django should store data

There are a ton more variables, but most are simple enough to understand/are part of an external library that builds on Django. Most of the time, you will only be touching a very limited subset of variables when developing.

#### Adding a simple page

As an example to demonstrate the knowledge mountain that has been piled upon thee by the above sections, let's walk through adding a simple templated page to an existing project `your_project`

1. Add the file at `your_project/templates/sample-page.html`

   ```markup
    <html>
      <head>
        <title>A page!</title>
      </head>
      <body>
        <h1>
          Your random number is: {{ the_number }}
        </h1>
      </body>
    </html>
   ```

2. Add the app using `manage.py`

   ```bash
    python manage.py startapp the_app
   ```

3. Add the view to the app

    \`\`\`python

   **your\_project/apps/the\_app/views.py**

    from django.shortcuts import render

    from random import randint

```text
def sample_view(request):
  n = str(randint(1, 100))
  return render(request, "sample-page.html", {'the_number': n})
```
```

1. Add the view to `urls.py`

   ```python
    # your_project/urls.py
    """big django comment"""
    from django.conf.urls import url, include
    from django.contrib import admin

    from .apps.the_app import views as the_app_views

    urlpatterns = [
      url(r'^randnum$', the_app_views.sample_view, name="sample"),
    ]
   ```

2. Add the app to `settings.py`

   ```python
    # your_project/settings.py

    """
    ...
    A bunch of stuff
    ...
    """

    INSTALLED_APPS = [
      # All the existing apps
      "your_project",
      "your_project.apps.the_app",
    ]

    """
    ...
    A bunch more stuff
    ...
    """
   ```

3. You're done! Start the server using `python manage.py runserver 8001` and go to `http://localhost:8001/randnum` to \(hopefully\) see your random number!

#### Django Channels

This is required for Django to support. I highly recommend reading [https://channels.readthedocs.io/en/latest/](https://channels.readthedocs.io/en/latest/) because I can't explain it too well yet. TL;DR you need to do a hecka bunch more stuff.

#### Running the server

**In development**

First, make sure `DEBUG=True` is set in your `settings.py`, then you can run

```bash
python manage.py runserver {optional_port}
```

at the root of your Django project. If that fails for some reason, or some website functionality is limited, you might have forgotten to create the database. Run

```bash
python manage.py makemigrations && python manage.py migrate
```

to maybe fix that.

**In production**

In production, we need to have a few more things set up.

**Static files**

Django doesn't like to serve static files \(because it is Python and slow\), so it wants something else like [Nginx](nginx.md) to take care of that for it. How the whole proxying setup works is explained in the [Nginx](nginx.md) page.

**A better webserver**

There are a ton better, much more performant [WSGI](https://wsgi.readthedocs.io/en/latest/what.html)/[ASGI](https://github.com/django/asgiref/blob/master/specs/asgi.rst)-compatible webservers than the one Django comes with. Good WSGI-only ones include [Gunicorn](https://gunicorn.org/) and [uWSGI](https://uwsgi-docs.readthedocs.io/en/latest/), while ASGI \(websocket-compatible\) ones include [Daphne](https://github.com/django/daphne) and [Uvicorn](https://www.uvicorn.org/). Choose one and set it up in your virtual environment.

**A nice startup script**

Using `daphne` as an example production server, here's what a complete startup script would look like:

```bash
#!/bin/bash

cd $DJANGO_ROOT
source $VENV/bin/activate
daphne -b 0.0.0.0 -p 8001 django_project.asgi:application
```

Assuming you have everything set up right \(including static file proxying\), running this script should make the server accessible through the proxied port.

Now you can add `ExecStart=/usr/bin/bash your-script.sh` to a systemd unit to make it the server at boot if that's what you want.

### Trivia

* Django is very good and much better than Flask for large web projects
* You can remember how to pronounce "Django" by remembering that "**Jango** Fett died in Star Wars Episode II: Attack of the Clones"
  * You should never say "duh-Jango" because that makes you sound weird
* Django is pretty resource intensive and spawns a heck ton of threads, which can especially bog down older laptops so be careful.

