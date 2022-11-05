STEP 1: INITIATING PROJECT

1. virtualenv env

2. env\Scripts\activate

3. pip install -r requirements.txt with following requirements
﻿asgiref==3.5.2
Django==4.1.3
sqlparse==0.4.3
tzdata==2022.6
gunicorn==20.1.0
psycopg2==2.9.3
dj-database-url==0.5.0

4. django-admin startproject codingmstrproject

5. python manage.py startapp codingmstrapp

6. python manage.py runserver

7. Create a frontend folder

8. cd frontend

9. npx create-react-app ./

10. Go to Django path



STEP 2: SETTING DJANGO

11. TEMPLATES         
	'DIRS': [
            os.path.join(BASE_DIR, 'frontend/build')
        ],

12. Add Inside urls.py   
from django.contrib import admin
from django.urls import path, include, re_path
from django.conf import settings
from django.views.static import serve

urlpatterns = [
    path('', include('attendance.urls')),
    path('admin/', admin.site.urls),

    re_path(r'^media/(?P<path>.*)$', serve,{'document_root':settings.MEDIA_ROOT}), 
    re_path(r'^static/(?P<path>.*)$', serve,{'document_root':settings.STATIC_ROOT}), 
]

12. Add Inside views.py
def index(request):
    return render(request, 'index.html')



13. Make Other urls.py inside codingmstrapp directory and add these lines
from django.urls import path
from . import views
from django.conf import settings
from django.urls import path, include
from django.conf.urls.static import static


urlpatterns = [
    path('', views.index, name='index'),
] 


14. Add Inside Setting.py
import os
from urllib.parse import urlparse
from django.core.management.utils import get_random_secret_key
import sys
import dj_database_url


SECRET_KEY = os.getenv("DJANGO_SECRET_KEY", get_random_secret_key())

DEBUG = os.getenv("DEBUG", "False") == "True"

ALLOWED_HOSTS = os.getenv("DJANGO_ALLOWED_HOSTS", "127.0.0.1, localhost").split(",")

Add codingmstrapp in Installed Apps


# For Server
DEVELOPMENT_MODE = os.getenv("DEVELOPMENT_MODE", "False") == "True"

if DEVELOPMENT_MODE is True:
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.postgresql',
            'NAME': 'cdotdb',
            'USER': 'postgres',
            'PASSWORD': '1234',
            'HOST': 'localhost', 
        }
    }
elif len(sys.argv) > 0 and sys.argv[1] != 'collectstatic':
    if os.getenv("DATABASE_URL", None) is None:
        raise Exception("DATABASE_URL environment variable not defined")
    DATABASES = {
        "default": dj_database_url.parse(os.environ.get("DATABASE_URL")),
    }


STATIC_URL = 'static/'
STATIC_ROOT = os.path.join(BASE_DIR, "staticfiles") 

# Extra places for collectstatic to find static files.
STATICFILES_DIRS = (
    os.path.join(BASE_DIR, 'frontend/build/static'),


15. go to frontend by typing 'cd frontend' in cmd and 

16. write command 'npm run build' to generate build file inside frontend

16. Create migrations by typing 'python manage.py makemigrations' and 

17. then type 'python manage.py migrate'

18. Type 'python manage.py createsuperuser'  and enter details like username, email and password


STEP 3: SETUP GIT ACCOUNT

19. git init

20. git remote add origin git@github.com:{name}/{name}

21. git branch -M branch

22. git add *

23. git commit -m 'first'

19. git push -u origin main


STEP 4: SET DIGITALOCEAN APP

20 Environment Variables

DJANGO_ALLOWED_HOSTS  ${APP_DOMAIN}

DEBUG  True 

DJANGO_SECRET_KEY   94875uwegsdkjlg (Encrypted)

DEVELOPMENT_MODE False


21. Add Run Command
gunicorn --worker-tmp-dir /dev/shm codingmstrproject.wsgi
