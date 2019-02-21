# GETTING STARTED WITH DJANGO REST FRAMEWORK

### 1. Install Django Rest Framework
	pip install djangorestframework

### 2. Start Django Project
	django-admin startproject dogs_api

### 3. 	Go Into Project Directory
	cd dogs_api

### 4. 	Migrate Existing Models
	python manage.py migrate

### 5.	Create The App
	python manage.py startapp dogs

### 6. 	Open In Text Editor
	code .

### 7.	Add Apps to Installed Apps in settings
In dogs_api/settings.py scroll down until you see the following code

	INSTALLED_APPS = [
    	'django.contrib.admin',
    	'django.contrib.auth',
    	'django.contrib.contenttypes',
    	'django.contrib.sessions',
    	'django.contrib.messages',
    	'django.contrib.staticfiles',
	]

Now add rest_framework and your apps with the following code. Be sure to add a comma between apps

	'rest_framework',
	'dogs'

The Installed Apps should now look like this

	INSTALLED_APPS = [
    	'django.contrib.admin',
    	'django.contrib.auth',
    	'django.contrib.contenttypes',
    	'django.contrib.sessions',
    	'django.contrib.messages',
    	'django.contrib.staticfiles',
		'rest_framework',
		'dogs'
	]

### 8.	Create A Model
In the directory dogs/models.py, you'll see

	from django.db import models

Copy the following code beneath

	class Breed(models.Model):
		name = models.CharField(max_length=100)

		def __str__(self):
			return self.name

### 9.	Create A Serializer For Your Model
First we need to make serializer file. From your command line cd into the dogs directory.

	cd dogs

Create the serializer file.

	touch serializer.py

Now we need to import serializers, our model, and create a class serlializer from our model. In your text editor,copy the following code to your dog/serializer.py file.

	from rest_framework import serializers
	from .models import Breed

	class BreedSerializer(serializers.HyperlinkedModelSerializer):
    	class Meta:
        	model = Breed
        	fields = ('id', 'url', 'name')

### 10.	Create A View For Model
We're using viewsets for this example so we need to import viewsets along with our model and serializer, and then create our the view. In the directory dogs/views.py, you'll see the following code.

	from django.shortcuts import render

Copy the following code beneath

	from rest_framework import viewsets
	from .models import Breed
	from .serializers import BreedSerializer

	class BreedView(viewsets.ModelViewSet):
    	queryset = Breed.objects.all()
    	serializer_class = BreedSerializer

### 11.	Create A URL And Router For The View
First we need to make urls.py file. From your command line cd into the dogs directory.

	cd dogs

Create the urls file.

	touch urls.py

Now we need to import our views, routers, path, and include.
In your text editor, go to dogs/urls.py and copy the following code.

	from django.urls import path, include
	from . import views
	from rest_framework import routers

	router = routers.DefaultRouter()
	router.register('pets', views.BreedView)

	urlpatterns = [
    	path('', include(router.urls))
	]

### 12.	Make Migrations And Migrate Models
From the command line, in the root directory copy the following code to make migrations with the model.

	python manage.py makemigrations

Now migrate the model.

	python manage.py migrate

### 13.	Runserver
From the command line, type the following code to run the server

	python manage.py runserver

