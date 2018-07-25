---
layout: post 
title: How to create a calendar using Django
category: normal
---

End result:
<figure>
<img src="/../images/calendar_v1.0.png" alt="Image of calendar"/>
</figure>


### Setup 
Install virtualenv and django
```bash
virtualenv env
source env/bin/activate
pip3 install django

# this stores all the python packages you installed into a requirements.txt file
pip3 freeze > requirements.txt

# create django project 
django-admin startproject djangocalendar

# start server to check that our app is running at localhost:8000
python3 manage.py runserver
```

### Create the calendar app
```bash
python3 manage.py startapp cal
```

In cal/views.py, create a index view:
```python
from django.shortcuts import render
from django.http import HttpResponse

# Create your views here.

def index(request):
    return HttpResponse('hello')
```

and in cal/urls.py, add the view we just created:  
```python
from django.conf.urls import url
from . import views

app_name = 'cal'
urlpatterns = [
    url(r'^index/$', views.index, name='index'),
]
```

To make the cal app work, we need to include the urls specified there in the djangocalendar/urls.py file:
```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('cal.urls')),
]
```

Run migration and start server to check things are running fine:
```bash
python3 manage.py migrate
python3 manage.py runserver
```

Here comes the fun part! In a calendar app, we would want to see our events for the day, what time does it start, end and what the event is about. We can start by declaring the Event model in cal/models.py:
```python
from django.db import models

class Event(models.Model):
    title = models.CharField(max_length=200)
    description = models.TextField()
    start_time = models.DateTimeField()
    end_time = models.DateTimeField()
```

This means that an event needs to have a title (not more than 200 chars in length), a description field, start time and end time. We then register it in cal/admin.py so that we can add events through the admin interface:
```python
from django.contrib import admin
from cal.models import Event

admin.site.register(Event)
```

To access the admin interface, we need to create a superuser:
```bash
python manage.py createsuperuser
```

Follow the prompts and when you're done, head on to [http://localhost:8000/admin/](http://localhost:8000/admin/). Try adding an event to see what happens!  

Now that we've got the event model set up, let's jump into creating the calendar. We will be inheriting the HTMLCalendar class in Python. What this means is that our Calendar class will have all of HTMLCalendar's attributes and methods, but we can also override its methods if we want to.  

For our calendar class, we override the formatday, formatweek and formatmonth methods:
```python
from datetime import datetime, timedelta
from calendar import HTMLCalendar
from .models import Event

class Calendar(HTMLCalendar):
	def __init__(self, year=None, month=None):
		self.year = year
		self.month = month
		super(Calendar, self).__init__()

	# formats a day as a td
	# filter events by day
	def formatday(self, day, events):
		events_per_day = events.filter(start_time__day=day)
		d = ''
		for event in events_per_day:
			d += f'<li> {event.title} </li>'

		if day != 0:
			return f"<td><span class='date'>{day}</span><ul> {d} </ul></td>"
		return '<td></td>'

	# formats a week as a tr 
	def formatweek(self, theweek, events):
		week = ''
		for d, weekday in theweek:
			week += self.formatday(d, events)
		return f'<tr> {week} </tr>'

	# formats a month as a table
	# filter events by year and month
	def formatmonth(self, withyear=True):
		events = Event.objects.filter(start_time__year=self.year, start_time__month=self.month)

		cal = f'<table border="0" cellpadding="0" cellspacing="0" class="calendar">\n'
		cal += f'{self.formatmonthname(self.year, self.month, withyear=withyear)}\n'
		cal += f'{self.formatweekheader()}\n'
		for week in self.monthdays2calendar(self.year, self.month):
			cal += f'{self.formatweek(week, events)}\n'
		return cal
```

We can then use the Calendar class that we created in cal/views.py:
```python
from datetime import datetime
from django.shortcuts import render
from django.http import HttpResponse
from django.views import generic
from django.utils.safestring import mark_safe

from .models import *
from .utils import Calendar

class CalendarView(generic.ListView):
    model = Event
    template_name = 'cal/calendar.html'

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)

        # use today's date for the calendar
        d = get_date(self.request.GET.get('day', None))

        # Instantiate our calendar class with today's year and date
        cal = Calendar(d.year, d.month)

        # Call the formatmonth method, which returns our calendar as a table
        html_cal = cal.formatmonth(withyear=True)
        context['calendar'] = mark_safe(html_cal)
        return context

def get_date(req_day):
    if req_day:
        year, month = (int(x) for x in req_day.split('-'))
        return date(year, month, day=1)
    return datetime.today()
```

We can then use this calendar variables in our templates. Create a base template in cal/templates/cal/base.html:
```html
{'% load staticfiles %}
<!doctype html>
<html lang="en">

<head>
  <!-- Required meta tags -->
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
  <!-- Bootstrap CSS -->
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm"
    crossorigin="anonymous">
  <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.10/css/all.css" integrity="sha384-+d0P83n9kaQMCwj8F4RJB66tzIwOKmrdb46+porD/OvrJ+37WqIM7UoBtwHO6Nlg"
    crossorigin="anonymous">
  <link rel="stylesheet" type="text/css" href="{'% static 'cal/css/styles.css' %}">
  <title>Django Calendar App</title>
</head>
<body>

  {'% block content %}
  {'% endblock %}

  <!-- Optional JavaScript -->
  <!-- jQuery first, then Popper.js, then Bootstrap JS -->
  <script src="https://code.jquery.com/jquery-3.2.1.slim.min.js" integrity="sha384-KJ3o2DKtIkvYIK3UENzmM7KCkRr/rE9/Qpg6aAZGJwFDMVNA/GpGFF93hXpG5KkN"
    crossorigin="anonymous"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.12.9/umd/popper.min.js" integrity="sha384-ApNbgh9B+Y1QKtv3Rn7W3mgPxhU9K/ScQsAP7hUibX39j7fakFPskvXusvfa0b4Q"
    crossorigin="anonymous"></script>
  <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/js/bootstrap.min.js" integrity="sha384-JZR6Spejh4U02d8jOt6vLEHfe/JQGiRRSQQxSfFWpi1MquVdAyjUar5+76PVCmYl"
    crossorigin="anonymous"></script>

  {'% block script %}
  {'% endblock %}
</body>
</html>
```

and in cal/templates/cal/calendar.html, we just need to extend base.html and render the calendar variable:
```html
{'% extends 'cal/base.html' %}

{'% block content %}
{'{ calendar }}
{'% endblock %}
```

We then add this view to cal/urls.py:
```python
from django.conf.urls import url
from . import views

app_name = 'cal'
urlpatterns = [
    url(r'^index/$', views.index, name='index'),
    url(r'^calendar/$', views.CalendarView.as_view(), name='calendar'), # here
]
```

You should be able to view the calendar at [http://localhost:8000/calendar/](http://localhost:8000/calendar/) now! For the last part, we add a little bit of css to make it a full blown calendar:
```css
.calendar {
  width: 100%;
  font-size: 13px;
}

.month {
  font-size: 25px;
}

tr, td {
  border: 1px solid black;
}

th {
  padding: 10px;
  text-align: center;
  font-size: 18px;
}

td {
  width: 200px;
  height: 150px;
  padding: 20px 0px 0px 5px;
}

.date {
  font-size: 16px;
}

ul {
  height: 100%;
  padding: 0px 5px 0px 20px;
}

a {
  color: #17a2b8;
}
```

It should look something like this:
<figure>
<img src="/../images/calendar_v1.0.png" alt="Image of calendar"/>
</figure>

