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

'''
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
'''

- [ ] it's already time to migrate
'python3 manage.py makemigrations
'python3 manage.py migrate

# There will be errors with URLS, so let's work on that next.
- [ ] In your <app>, make a subdir <templates> and a subdir for it <app>
- [ ] Make the -list and -detail ..html files for all your key concepts in the <app> under the lowest <app> dir just made
- [ ] In the templates directory above, touch a base.htm. and a index.html
- [ ] For all these html files, put a hard string at the top to label it.

'''

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
'''


# Create the base template
The templates in django-registration-redux assume you have a base.html template in your project’s template directory. This base template should include a title, meta, and content block. The title block should allow customization of the <title> tag. The meta block should appear within the <head> tag to allow for custom <meta tags for security reasons. The content block should be within the <body> tag.

# Now let's study some redux pre-requesites.
https://django-registration-redux.readthedocs.io/en/latest/index.html

- [ ]
'pipenv install django-registration-redux
- [ ]
'python setup.py install
- [ ] Begin by adding 
'registration 
to the INSTALLED_APPS setting of your project, 
- [ ] and specifying one additional setting:
'ACCOUNT_ACTIVATION_DAYS = 7
'REGISTRATION_DEFAULT_FROM_EMAIL = "This string is from settings.py REGISTRATION_DEFAULT_FROM_EMAIL"
- [ ] Open project-level URLConf (urls.py) and add
ulr += path(r'^accounts/', include('registration.backends.default.urls')),
