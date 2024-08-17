# Deploying django project on render

## Steps to follow for deployment on render

### 1. Create postgresql on render.
- dashboard -> new -> postgresql

### 2. Next we need to connect this database to our repo.
- ```pip install dj-database-url```
- Add below code in settings.py file
```py
import dj_database_url
DATABASES['default']=dj_database_url.parse("postgresql://postgresql_01_wrrk_user:fNzUx4GYEC1X42jZqlNyPhJcR8V0vyP9@dpg-cr07v1bv2p9s73a32ogg-a.oregon-postgres.render.com/postgresql_01_wrrk")
```
- Now we will install postgres. ```pip install psycopg2-binary```
- Runserver to check if it's working.
- Make migration ```python manage.py migrate```
- create superuser ```python manage.py createsuperuser```

### 3. Create new web service
- dashboard -> new -> web service

### 4. Securing our sensitive information
- Here we will create two files ```.env``` and ```.gitignore```.
- Install the python-decouple ```pip install python-decouple```.
- .gitignore
```
.env
```
- .env
```
SECRET_KEY=django-insecure-kddwx*-46!5ow-xb5%c-5)%f3y2-ci_s=t5o_82_kk**ok94%d
DEBUG=True
ALLOWED_HOSTS=127.0.0.1,
DATABASE_URL=postgresql://postgresql_01_wrrk_user:fNzUx4GYEC1X42jZqlNyPhJcR8V0vyP9@dpg-cr07v1bv2p9s73a32ogg-a.oregon-postgres.render.com/postgresql_01_wrrk
```
- In ```.env``` file we are only removing the sensitive information for the settings.py and add to the ```.env``` file.
- Now we will use ```.env``` file to add sesitive information in ```settings.py``` file.
```py
from decouple import config
SECRET_KEY = config('SECRET_KEY')
DEBUG = config('DEBUG', cast=bool)
ALLOWED_HOSTS = config("ALLOWED_HOSTS").split(",")
DATABASES['default']=dj_database_url.parse(config('DATABASE_URL'))
```
