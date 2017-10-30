---
layout: post 
title: Setting up Django with React
category: normal
---

This post is a tutorial to start a project using React and Django. We'll work on getting Django started first, followed by React. 

Steps

1. Download Virtualenv 
```
$ pip install virtualenv
```
2. Create a virtual env, install Django and Django REST Framework
```
$ virtualenv env
$ source env/bin/activate
$ pip3 install django djangorestframework django-filter
$ pip3 freeze > requirements.txt
```
Note: pip freeze outputs installed packages in requirements format
I had to do a `alias python='python3' to set my python default to version 3 as it was using python2.7 and gave me some errors.

3. Now we want to set up a database with Django. They use SQLite as default, but we want to use PostgreSQL. So in our virtual env, we can install brew and use brew to install postgresql:
```
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
$ brew install postgresql
```
After installation is done, you'll see this at the end:
```
To have launchd start postgresql now and restart at login:
  brew services start postgresql
```
Follow it and do `brew services start postgresql` to start postgresql. Next, you want to create a db just to make sure postgres is installed properly:
```
$ createdb your_db_name
$ psql your_db_name
```
To use postgresql with Django, we need to install psycopg2:
```
pip3 install psycopg2
```
Once we're done with that, we want to change our Django settings to use the db we just created. In settings.py:
```
DATABASES = {  
    'default': {
    'ENGINE': 'django.db.backends.postgresql',
    'NAME': 'your_database_name',
    'USER': 'your_osx_username',
    'PASSWORD': '',
    'HOST': 'localhost',
    'PORT': '5432',
    }
}
```
and now, we can do a migration!! 
```
$ python manage.py check
$ python maange.py migrate
```

4. Now we can move on to configure the Django Rest Framework for our app. Following the [django-rest website](http://www.django-rest-framework.org/#installation)
```
pip3 install markdown       # Markdown support for the browsable API.
```
Add 'rest_framework' to INSTALLED_APPS:
```
INSTALLED_APPS = (
    ...
    'rest_framework',
)
```
Adding REST framework's login and logout views to root `urls.py` file:
```
urlpatterns = [
    ...
    url(r'^api-auth/', include('rest_framework.urls', namespace='rest_framework'))
]
```
Also, a good way to test if you've configured the Django Rest Framework correctly:

```
from django.conf.urls import url, include
from django.contrib.auth.models import User
from rest_framework import routers, serializers, viewsets
from django.contrib import admin

# Serializers define the API representation.
class UserSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = User
        fields = ('url', 'username', 'email', 'is_staff')

# ViewSets define the view behavior.
class UserViewSet(viewsets.ModelViewSet):
    queryset = User.objects.all()
    serializer_class = UserSerializer

# Routers provide an easy way of automatically determining the URL conf.
router = routers.DefaultRouter()
router.register(r'users', UserViewSet)

# Wire up our API using automatic URL routing.
# Additionally, we include login URLs for the browsable API.
urlpatterns = [
    url(r'^', include(router.urls)),
    url(r'^admin/', admin.site.urls),
    url(r'^api-auth/', include('rest_framework.urls', namespace='rest_framework')),
]
```
Once you've added that in, we can start our server and go to http://localhost:8000/ and view our new 'users' API. Cool!! :) 
```
$ python3 manage.py runserver 0.0.0.0:8000
```

We'll work on token authentication and adding React to our project next :)

--- TBC --- 

Credits / helpful links:
- [Create website using React and Django REST framework](https://hackernoon.com/creating-websites-using-react-and-django-rest-framework-b14c066087c7)
- [Setting up PostgreSQL and Django on OS X](https://goonan.io/setting-up-postgresql-on-os-x-2/)
- [Install and Configure PostgreSQL](http://www.marinamele.com/taskbuster-django-tutorial/install-and-configure-posgresql-for-django)
- [Django REST framework docs](http://www.django-rest-framework.org/#installation)
