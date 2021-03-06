#### Installation

To get 3bot up and running, make sure you have [pip](https://github.com/pypa/pip) and [virtualenv](https://github.com/pypa/virtualenv) installed on your system. Now switch to your shell or Terminal and enter the following commands.

    # first cd to your directory where you want to install 3bot
    # then create a new python virtualenv
    virtualenv --no-site-packages -p python 3bot && cd 3bot

    # activate the created virtualenv
    source ./bin/activate

    # install the required packages
    pip install django==1.7
    pip install django-ar-organizations
    pip install threebot

    # start a new django project
    django-admin startproject djangothreebot
    cd djangothreebot


After the packages were installed successfully we make some changes in the `settings.py` file. The file is located in your new django project we just created.

Add `sekizai`, `threebot` and `organizations` to your `INSTALLED_APPS`. The section should look like this:

    INSTALLED_APPS = (
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
        'django.contrib.sites',
        
        'sekizai',
        'threebot',
        'organizations',
    )


Add `sekizai.context_processors.sekizai`, `request` and `auth` to `TEMPLATE_CONTEXT_PROCESSORS` in your project settings. The section should look like this:

    TEMPLATE_CONTEXT_PROCESSORS = (
        "django.contrib.auth.context_processors.auth",
        "django.core.context_processors.request",
        "django.core.context_processors.debug",
        "django.core.context_processors.i18n",
        "django.core.context_processors.media",
        "django.core.context_processors.static",
        "django.core.context_processors.tz",
        "django.contrib.messages.context_processors.messages",
        "sekizai.context_processors.sekizai",
    )


3bot provides a login form, to use it properly, add the following lines to your `settings.py` file
    
    LOGIN_URL = '/login/'
    LOGIN_REDIRECT_URL = '/'


Next to `settings.py` you find `urls.py`. Add the patterns for `teams` and `threebot`.

    urlpatterns = patterns('',
        url(r'^admin/', include(admin.site.urls)),
        url(r'^teams/', include('organizations.urls')),
        url(r'^', include('threebot.urls')),
    )


After this modifications, we have to sync the database. Therefor run the following commands in your shell or Terminal.

    python manage.py syncdb
    python manage.py migrate


Start the development server and enjoy your fresh 3bot instance running under [http://127.0.0.1:8000](http://127.0.0.1:8000).

    python manage.py runserver


To use your new 3bot application you need to set up a worker. A worker is a computer program that runs as a background process on a machine. This could be a server, an embedded systems or your laptop. The worker execute the tasks of a workflow - they do perform. 
For testing purposes we suggest to set up your first worker on localhost. Therefor you first create new Worker at [http://127.0.0.1:8000/worker/add/](http://127.0.0.1:8000/worker/add/). Choose `127.0.0.1` as IP-Address, and `55556` as port. Save your new worker head over to [https://gist.github.com/walterrenner/4d8863043404bec01d0f](https://gist.github.com/walterrenner/4d8863043404bec01d0f) for further instructions.

