# GETTING STARTED WITH DJANGO REST FRAMEWORK

### 1. Install Django Rest Framework
	pip install djangorestframework

### 2. Start Django Project
	django-admin startproject beer_api

### 3. 	Go Into Project Directory
	cd beer_api

### 4. 	Migrate Existing Models
	python manage.py migrate

### 5.	Create The App
	python manage.py startapp beer

### 6. 	Open In Text Editor
	code .

### 7.	Add Apps to Installed Apps in settings
In beer_api/settings.py scroll down until you see the following code

	INSTALLED_APPS = [
    	'django.contrib.admin',
    	'django.contrib.auth',
    	'django.contrib.contenttypes',
    	'django.contrib.sessions',
    	'django.contrib.messages',
    	'django.contrib.staticfiles',
	]

Now add rest_framework and your apps with the following code

	'rest_framework',
	'beer'

	INSTALLED_APPS = [
    	'django.contrib.admin',
    	'django.contrib.auth',
    	'django.contrib.contenttypes',
    	'django.contrib.sessions',
    	'django.contrib.messages',
    	'django.contrib.staticfiles',
		'rest_framework',
		'beer'
	]


