# GETTING STARTED WITH DJANGO REST FRAMEWORK

### Install Django Rest Framework
	pip install djangorestframework

### Start Django Project
	django-admin startproject dogs_api

### Go Into Project Directory
	cd dogs_api

### Migrate Existing Models
	python manage.py migrate

### Create The App
	python manage.py startapp dogs

### Open In Text Editor
	code .

### Add Apps to Installed Apps in Project Settings
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

### Edit URLS In Project
In dogs_api/urls.py, you'll see the following code.

	from django.contrib import admin
	from django.urls import path

	urlpatterns = [
    	path('admin/', admin.site.urls),
	]

Now we need to include our path and import include. We're already importing django.urls so we can just add a comma after path.

	from django.urls import include	path, include

Now add the following path to the urls. Be sure to add a comma between paths.

	path('', include('dogs.urls'))

Your dogs_api/urls.py file should now contain the following code.

	from django.contrib import admin
	from django.urls import path, include

	urlpatterns = [
    	path('admin/', admin.site.urls),
		path('', include('dogs.urls'))
	]

### Create A Model
In the directory dogs/models.py, you'll see

	from django.db import models

Copy the following code beneath

	class Breed(models.Model):
		name = models.CharField(max_length=100)

		def __str__(self):
			return self.name

### Create A Serializer For Your Model
First we need to make serializers file. From your command line cd into the dogs directory.

	cd dogs

Create the serializers file.

	touch serializers.py

Now we need to import serializers, our model, and create a class serlializer from our model. In your text editor,copy the following code to your dog/serializer.py file.

	from rest_framework import serializers
	from .models import Breed

	class BreedSerializer(serializers.HyperlinkedModelSerializer):
    	class Meta:
        	model = Breed
        	fields = ('id', 'url', 'name')

### Create A View For Model
We're using viewsets for this example so we need to import viewsets along with our model and serializer, and then create our the view. In the directory dogs/views.py, you'll see the following code.

	from django.shortcuts import render

Copy the following code beneath

	from rest_framework import viewsets
	from .models import Breed
	from .serializers import BreedSerializer

	class BreedView(viewsets.ModelViewSet):
    	queryset = Breed.objects.all()
    	serializer_class = BreedSerializer

### Create A URL And Router For The View
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

### Make Migrations And Migrate Models
From the command line, in the root directory copy the following code to make migrations with the model.

	python manage.py makemigrations

Now migrate the model.

	python manage.py migrate

### Runserver
From the command line, type the following code to run the server

	python manage.py runserver

