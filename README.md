Your own OSQA on OpenShift
============================

This git repository helps you get up and running quickly w/ a Django installation on OpenShift.  The Django project name used in this repo is 'osqa' but feel free to change it. 

Create the app on OpenShift
----------------------------

Create an account at http://openshift.redhat.com/ , don't forget to create a namespace and install client tools as well. And ensure you have 'django', 'mysql', 'mysql-devel', 'mysql-python' installed on your local machine.

Create a python application

    rhc app create -a osqa -t python-2.6

Add this upstream seambooking repo

    cd osqa
    git remote add upstream -m master git://github.com/lulinqing/OSQA-openshift-quickstart.git
    git pull -s recursive -X theirs upstream master

Add mysql database support for you app

    rhc app cartridge add -a osqa -c mysql-5.1

You will get some useful information from the output.

Make a little update and RUN!
----------------------------

Update the "wsgi/osqa/settings_local.py" config file with the information you just got

    cd wsgi/osqa
    vi settings_local.py

    Then update following parameters:
        DATABASE_NAME = 'osqa'        # usually the same name as your app
        DATABASE_PASSWORD = 'xxxxxxxxxx'    # replace it with your password
        DATABASE_HOST = '127.xx.xx.xx'       # replace it with IP of your
        APP_URL = 'http://osqa-$yournamespace.rhcloud.com'      # replace it with your app URL

    git commit -a -m ' update settings_local.py'

Forwarding remote mysql service port to your local machine, we will need this for step 7 (better use another terminal window)

    rhc-port-forward -a osqa

Prepare your mysql database for app

    ./manage.py syncdb --all

Choose 'no' when being asked whether create an account now.

    ./manage.py migrate forum --fake

You can push the repo now

    git push

That's it, you can now checkout your application at:

    http://osqa-$yournamespace.rhcloud.com

Here goes mine:

    http://osqa-linqing.rhcloud.com/
