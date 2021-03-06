DjangoCon 2015
============

DjangoCon 2015 is built on top of Pinax Symposion but may have customizations that
will may make things more difficult for you.

To get running locally
----------------------

This documentation assume you have the following installed::

- `pip >= 1.2.1 <http://www.pip-installer.org/>`_
- `virtualenv >= 1.11 <http://www.virtualenv.org/>`_
- `virtualenvwrapper >= 3.6 <http://pypi.python.org/pypi/virtualenvwrapper>`_

Create a new virtualenv and install the necessary requirements::

    mkvirtualenv --distribute djangocon --python=python2.7
    $VIRTUAL_ENV/bin/pip install -r $PWD/requirements/dev.txt

(For production, install -r requirements/base.txt).

Then create a local settings file and set your ``DJANGO_SETTINGS_MODULE`` to use it::

    cp djangocon/settings/local.py.example djangocon/settings/local.py
    echo "export DJANGO_SETTINGS_MODULE=djangocon.settings.local" >> $VIRTUAL_ENV/bin/postactivate
    echo "unset DJANGO_SETTINGS_MODULE" >> $VIRTUAL_ENV/bin/postdeactivate

Exit the virtualenv and reactivate it to activate the settings just changed::

    deactivate
    workon djangocon

Setup the postgres database and load fixtures::

    createdb djangocon2015
    python manage.py syncdb
    python manage.py migrate
    python manage.py loaddata fixtures/*

Create a user account::

    python manage.py createsuperuser


Run local server::

    python manage.py runserver

For Gondor
--------------

Deploying to Gondor is relatively straightforward. You will need to setup a
Gondor configuration file, if you have not already done so::

    vi ~/.gondor

Add the following content::

    [auth]
    username = gondor-username
    key = gondor-user-key

Change the permissions on the file::

    chmod 600 ~/.gondor

To deploy the develop branch to the staging instance::

    gondor deploy develop develop

To deploy the master branch to the production instance::

    gondor deploy primary master

To run tests
------------

::

    python manage.py test djangocon
