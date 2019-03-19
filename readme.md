#High-Level Concepts
* Matron - members
* Bowl - collections of components
* Necklace - a decision
* Bead - suggested componensts
* Beat - events
* - financial transactions

#Convert all concepts into a url-to-template.html map

    ''    index
    'beads/'  'bead-list'
    'beads/<int:pk>'    'bead-detail'
    'necklaces/'  'necklace-list'
    'necklaces/<int:pk>'    'necklace-detail'
    'matrons/'   'necklace-list'
    'matrons/<int:pk>'   'necklace-detail'

# Install Django
- [ ] Make a directory for your project and use cd to move into that directory.
- [ ] Run 
'
    'pipenv install django'
- [ ] Run 
    'pipenv shell' 
to enter the virtual environment where you now have Django installed.
- [ ] To start your Django project, run 
    'django-admin startproject <project_name> . 
where <project_name> is replaced with your project name. Note the period after the project name. If you don't put that, another directory will be created, which you don't want.
to enter the virtual environment where you now have Django installed.
- [ ] Move a gitignore.io file into the project https://gitignore.io

# For any app
- [ ] 'python3 manage.py startapp core'
where core is <app>
- [ ] within <app>, create urls.py
    
# Back to project
- [ ] In settings.py
    INSTALLED_APPS = 'Core.apps.CoreConfig', 
   
    TIME_ZONE = 'America/New_York'
- [ ] In configuration URL.py, paste the following

```
from django.contrib import admin
from django.urls import path
from django.urls import include
from django.conf import settings
from django.conf.urls.static import static
from django.views.generic import RedirectView

urlpatterns = [
    path('', RedirectView.as_view(url='/core/', permanent=True)),
    path('blog/', include('core.urls')),
    path('admin/', admin.site.urls),
    ]   
   
urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```

- [ ] it's already time to migrate
'python3 manage.py makemigrations
'python3 manage.py migrate

# There will be errors with URLS, so let's work on that next.
- [ ] In your <app>, make a subdir <templates> and a subdir for it <app>
- [ ] Make the -list and -detail ..html files for all your key concepts in the <app> under the lowest <app> dir just made
- [ ] In the templates directory above, touch a base.htm. and a index.html
- [ ] For all these html files, put a hard string at the top to label it.

```
urlpatterns = [
    path('', TemplateView.as_view(template_name = 'index.html'), ),
    path('beads/', TemplateView.as_view(template_name='core/bead-list.html'), ),
    path('beads/<int:pk>', TemplateView.as_view(template_name='core/bead-detail.html'), ),
    path('matrons/', TemplateView.as_view(template_name='core/matron-list.html'), ),
    path('matrons/<int:pk>', TemplateView.as_view(template_name='core/matron-detail.html'), ),
    path('necklaces/', TemplateView.as_view(template_name='core/necklace-list.html'), ),
    path('necklaces/<int:pk>', TemplateView.as_view(template_name='core/necklaces-detail.html'), ),
    path('bowls/', TemplateView.as_view(template_name='core/bowl-list.html'), ),
    path('bowls/<int:pk>', TemplateView.as_view(template_name='core/bowl-detail,html'), ),
]
```
```
## This is catalog.urls

from django.contrib import admin
from django.urls import path
from django.views.generic import TemplateView

from . import views

urlpatterns = [
    path('', TemplateView.as_view(), name = 'index'),
    path('beads/', TemplateView.as_view(), name='beads-list'),
    path('beads/<int:pk>', TemplateView.as_view(), name='beads-detail'),
    path('matrons/', TemplateView.as_view(), name='matrons-list'),
    path('matrons/<int:pk>', TemplateView.as_view(), name='matrons-detail'),
    path('necklaces/', TemplateView.as_view(), name='necklaces-list'),
    path('necklaces/<int:pk>/', TemplateView.as_view(), name='necklaces-detail'),
    path('bowls/', TemplateView.as_view(), name='bowls-list'),
    path('bowls/<int:pk>/', TemplateView.as_view(), name='bowls-detail'),
]
```
- [ ] makemigrations
- [ ] migrate
- [ ] runserver
- [ ] 127.0.0.1:8000/core 
- [ ] and test all the simple sub-pages

# Now let's create the base template. Uh, can't. So, let's do simple models and simple views.


Header
```
from django.db import models
from django.contrib.auth.models import User
from django.urls import reverse
from datetime import date
import uuid

```
- [ ] Do this over and over for each core concept. A primary key <int:pk> is implied.


```
# Create your models here.
class Bead(models.Model):
    name = models.CharField(max_length=100)

    class Meta:
        ordering = ['name',]

    def get_absolute_url(self):
        return reverse('bead-detail', args=[str(self.pk)])
        #That first argument is supposed to be the URL, not the view

    def __str__(self):
        return self.name
```
- [ ] Go ahead an make migrations

# Do the Views file
Header
'''
from django.shortcuts import render
from core.models import Bead, Matron, Necklace, Bowl
from django.views import generic
from django.views.generic.edit import CreateView
from django.views.generic import ListView, DetailView
'''
Index
```
def index(request):
    request.session['num_visits'] = num_visits+1

    return render(
        request,
        'index.html',
        context={'num_visits': num_visits},
    )
```
For each core concept

```
class BeadListView(generic.ListView):
    model = Bead
    
class BeadDetailView(generic.DetailView):
    model = Bead
    
```

```
urlpatterns = [
    path('', views.index, name = 'index'),
    path('beads/', views.BeadListView.as_view(), name='bead-list'),
    path('beads/<int:pk>', views.BeadDetailView.as_view(), name='bead-detail'),
    path('matrons/', views.MatronListView.as_view(), name='matron-list'),
    path('matrons/<int:pk>', views.MatronDetailView.as_view(), name='matron-detail'),
    path('necklaces/', views.NecklaceListView.as_view(), name='necklace-list'),
    path('necklaces/<int:pk>', views.NecklaceDetailView.as_view(), name='necklace-detail'),
    path('bowls/', views.BowlListView.as_view(), name='bowl-list'),
    path('bowls/<int:pk>', views.BowlDetailView.as_view(), name='bowl-detail'),
]
```
- [ ] making migrations, migrate, is a great way to check for errors

Can we test this? 
Or do we need data first?
'''
<p>This is the base template</p>

{% block menu %}
        <ul>
          <li><a href="{% url 'index' %}">Home</a></li>
          <li><a href="{% url 'bead-list' %}">Beads</a></li>
          <li><a href="{% url 'matron-list' %}">Matrons</a></li>
          <li><a href="{% url 'necklace-list' %}">Necklaces</a></li>
          <li><a href="{% url 'bowl-list' %}">Bowls</a></li>
        </ul>
{% endblock %}
'''
**Changes the dashes in my HTML templates into underscores? WAIT WHY DO I HAVE TO PUT UNDERSCORES IN ALL MY FILES? Will this have repercussions???
Yes, I had to do that, but no BREAKS so far

# It's time to add a super user.
```
python3 manage.py createsuperuser
```
add the essential information to your essential table
|Superuser | SuperuserPassword | SuperUserEmail|
|-----|-----|-----|
| .    | .    | .    |

# WAIT A MINUTE, WHY DID NOT MY MODELS MIGRATE TO THE ADMIN SITE?
Because I did not register them in admin.py

```
from django.contrib import admin
from core.models import Bowl, Necklace, Bead, Matron


# Pattern 1
admin.site.register(Bowl)
admin.site.register(Bead)
admin.site.register(Necklace)
admin.site.register(Matron)
```
# Now, add some scant data using the admin interface:
Beads, Necklace, and Bowls. Just a few for each. 
Not Matrons, because they are going to rest on users.

# Now, we want to learn the trick of getting a list to show up on a page.

So, you add this code to all the list_templates. Funny how that content block has to be called content block. 

```
{% extends "base.html" %}

{% block content %}
  <h1>Necklace List</h1>
  {% if necklace_list %}
  <ul>
    {% for necklace in necklace_list %}
      <li>
         {{necklace.name}}
      </li>
      
    {% endfor %}
  </ul>
  {% else %}
    <p>There are none in this project.</p>
  {% endif %}       
{% endblock %}

```

# Now let's do the trick of getting detail pages to show up. 

- [ ] first we will add a description line to each class. 
'''description = models.TextField(max_length=1000,null=True, blank=True)'''
- [ ] make migrations, migrate
- [ ] use the admin panel to add in a few descriptions
- [ ] this is the code that we paste inton all the detail.html pages
```
{% extends "base.html" %}

{% block content%}

    <h1>Title: {{ Bead.name }}</h1>

    <p><strong>Description:</strong><br>
        {{ Bead.description }}</p>

{% endblock %}
```
# But the list pages aren't linking to them, so something needs to change.
- [ ] so we add or expand this code for the innermost loop on the list pages:
```
<a href="{{ bead.get_absolute_url }}">{{ bead.name }}</a><br>
```
# Here are the important fixes that I did:
- [ ] Note: i changed my URLS in in url file, my html files, my base template - everywhere there was a dash, I changed to underscore. And probably the reverse URL in the model.
- [ ] just made urls with <int:pk> singular.
- [ ] No / after <int:pk> on urls.py

create the base template
The templates in django-registration-redux assume you have a base.html template in your project’s template directory. This base template should include a title, meta, and content block. The title block should allow customization of the <title> tag. The meta block should appear within the <head> tag to allow for custom <meta tags for security reasons. The content block should be within the <body> tag.

# Now let's study some redux pre-requesites.
https://django-registration-redux.readthedocs.io/en/latest/index.html

To meet those requirements, put the following in the base.html:
```
<p>This is the base template</p>

{% load staticfiles %}
<!DOCTYPE html>
<html>

<head>
  <meta charset="UTF-8">
  <title>Outer Title Hidden
      {% block title %}This is for the block labeled title{% endblock %}
  </title>
{% block meta %}This is for the block labeled meta{% endblock %}
</head>
<br>

<body>Start of body
<br>
{% block menu %}Block labled menu
        <ul>
          <li><a href="{% url 'index' %}">Home</a></li>
          <li><a href="{% url 'bead_list' %}">Beads</a></li>
          <li><a href="{% url 'matron_list' %}">Matrons</a></li>
          <li><a href="{% url 'necklace_list' %}">Necklaces</a></li>
          <li><a href="{% url 'bowl_list' %}">Bowls</a></li>
        </ul>
End Menu{% endblock %}
<br>

{% block content %}This is for the block labeled content{% endblock %}
</body>

```

# That got a little out of order.
Let's step back and look at some of this Django template language.
https://docs.djangoproject.com/en/2.1/ref/templates/language/

# We just did a push, so let's try the registration stuff.



- [ ]
'pipenv install django-registration-redux

- [ ] Begin by adding AT THE TOP OF THE LIST
'registration 
to the INSTALLED_APPS setting of your project, 
- [ ] and specifying one additional setting:
'ACCOUNT_ACTIVATION_DAYS = 7
'REGISTRATION_DEFAULT_FROM_EMAIL = "This string is from settings.py REGISTRATION_DEFAULT_FROM_EMAIL"
- [ ] Open project-level URLConf (urls.py) and NEAR THE BEGINNING
```urlpatterns += path('accounts/', include('registration.backends.default.urls')),```
Now that we've done that, let's go check it out in admin, where we learn that registration files have a user and a key.

now try the site with /accounts/login/
...

# stuck

eventually, I worked with settings.py and made a number of changes. The problem, though, is that I don't have a separate email server. Many settings are commented about. I am not able in registew new users. At least I can log in and log out of admin. I think the last step in all of this is to reset the admin password.

AJ taught me to look for a confirmation link in my server terminal window.
Also, I needed to add something to the top of my views.py file:
```
from django.shortcuts import render, get_object_or_404, redirect
```
Probably the redirect was the winner.

Notes ONLY NOTES not implemented
```
EMAIL_BACKEND= "django.core.mail.backends.smtp.EmailBackend"
EMAIL_HOST= "smtp.gmail.com"
EMAIL_HOST_USER= "abc@gmail.com"
EMAIL_HOST_PASSWORD= "password"
EMAIL_PORT= 587
EMAIL_USE_TLS= True
DEFAULT_FROM_EMAIL= "abc@gmail.com"
```

# So let's read up on leveraging Users
https://docs.djangoproject.com/en/2.1/topics/auth/default/#user-objects

add this to Matron in models.py
```userid = models.OneToOneField(User, on_delete=models.PROTECT, default=1)```


Model field reference:
https://django.readthedocs.io/en/2.1.x/ref/models/fields.html

PSQL NOTES



Change stuff in Settings > Databases



(w6-apile-team-channa) bash-3.2$ createuser -d apile
(w6-apile-team-channa) bash-3.2$ createdb -U apile apile
(w6-apile-team-channa) bash-3.2$ psql
psql: FATAL:  database "jayspain" does not exist
(w6-apile-team-channa) bash-3.2$ psql apile
psql (11.2)
Type "help" for help.

apile=# \l
                                  List of databases
   Name    |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges
-----------+----------+----------+-------------+-------------+-----------------------
 apile     | apile    | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
 postgres  | jayspain | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
 template0 | jayspain | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/jayspain          +
           |          |          |             |             | jayspain=CTc/jayspain
 template1 | jayspain | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/jayspain          +
           |          |          |             |             | jayspain=CTc/jayspain
(4 rows)

apile=# t
apile-# \q
(w6-apile-team-channa) bash-3.2$ ls
Pipfile		README.md	core		noun-stack.png
Pipfile.lock	apile		manage.py	teamthoughts.md
(w6-apile-team-channa) bash-3.2$ cd apile
(w6-apile-team-channa) bash-3.2$ ls
__init__.py	settings.py	urls.py		wsgi.py
(w6-apile-team-channa) bash-3.2$ open settings.py
(w6-apile-team-channa) bash-3.2$

