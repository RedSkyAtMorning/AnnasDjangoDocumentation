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


Make a directory for your project and use cd to move into that directory.
Run pipenv install django.
Run pipenv shell to enter the virtual environment where you now have Django installed.
To start your Django project, run django-admin startproject <project_name> . where <project_name> is replaced with your project name. Note the period after the project name. If you don't put that, another directory will be created, which you don't want.
