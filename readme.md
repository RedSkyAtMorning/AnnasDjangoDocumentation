#High-Level Concepts
* Matron
* Bowl
* Necklace
* Bead
* Beat

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
'pipenv install django'
- [ ] Run 
'pipenv shell' 
to enter the virtual environment where you now have Django installed.
- [ ] To start your Django project, run 
'django-admin startproject <project_name> . 
where <project_name> is replaced with your project name. Note the period after the project name. If you don't put that, another directory will be created, which you don't want.


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
