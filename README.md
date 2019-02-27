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

Now we need to include our path and import include from django.urls. We're already importing django.urls so we can just add a comma after path.

	from django.urls import include	path, include

Now add the following path to the urls. Be sure to add a comma between paths.

	path('', include('dogs.urls'))

Now your dogs_api/urls.py file should now contain the following code.

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
	router.register('breeds', views.BreedView)

	urlpatterns = [
    	path('', include(router.urls))
	]

### Make Migrations And Migrate Models
From the command line, in the root directory copy the following code to make migrations with the model.

	python manage.py makemigrations

Now migrate the model.

	python manage.py migrate

### Run The Server
From the command line, type the following code to run the server

	python manage.py runserver

The server should now be running on port http://127.0.0.1:8000/. Click on the link and your Django Rest API should be up and running.

### Add Breeds of Dogs

### Create A Model With A One To Many Relationship
In the directory dogs/models.py

Copy the following code beneath your Breed model

	class Dog(models.Model):
    	name = models.CharField(max_length=50)
    	breed = models.ForeignKey(Breed, on_delete=models.CASCADE)

    	def __str__(self):
        	return self.name

Your dogs/models.py file should now contain the following code.

	from django.db import models

	class Breed(models.Model):
		name = models.CharField(max_length=100)

		def __str__(self):
			return self.name

	class Dog(models.Model):
    	name = models.CharField(max_length=100)
    	breed = models.ForeignKey(Breed, on_delete=models.CASCADE)

    	def __str__(self):
        	return self.name

###	Create Serializer For One To Many Relationship
In dog/serializers.py file, we need to import Dog from .models and create our Dog serializer. We're already importing from .models so we can just add a comma after Breed.

	from .models import Breed, Dog

Now create the Dog serializer.

	class DogSerializer(serializers.HyperlinkedModelSerializer):
    	class Meta:
        	model = Dog
        	fields = ('id', 'url', 'name', 'breed')

Your dog/serializers.py file should now contain the following code.

	from rest_framework import serializers
	from .models import Breed, Dog

	class BreedSerializer(serializers.HyperlinkedModelSerializer):
    	class Meta:
        	model = Breed
        	fields = ('id', 'url', 'name')

	class DogSerializer(serializers.HyperlinkedModelSerializer):
    	class Meta:
        	model = Dog
        	fields = ('id', 'url', 'name', 'breed')

### Create View For One To Many Model
In the dogs/views.py file, we need to import Dog from .models, DogSerializer from .seriailzers, and create a DogView.

	from .models import Breed, Dog
	from .serializers import BreedSerializer, DogSerializer

	class DogView(viewsets.ModelViewSet):
    	queryset = Dog.objects.all()
    	serializer_class = DogSerializer


The dogs/views.py file should now contain the following code.

	from rest_framework import viewsets
	from .models import Breed, Dog
	from .serializers import BreedSerializer, DogSerializer

	class BreedView(viewsets.ModelViewSet):
    	queryset = Breed.objects.all()
    	serializer_class = BreedSerializer

	class DogView(viewsets.ModelViewSet):
    	queryset = Dog.objects.all()
    	serializer_class = DogSerializer

### Create URL For One To Many View
In the dogs/urls.py file, add route for DogView

	router.register('dogs', views.DogView)

The dogs/urls.py file should now contain the following code.

		from django.urls import path, include
		from . import views
		from rest_framework import routers

		router = routers.DefaultRouter()
		router.register('breeds', views.BreedView)
		router.register('dogs', views.DogView)

		urlpatterns = [
    		path('', include(router.urls))
		]

### Make Migrations And Migrate Models
From the command line, in the root directory copy the following code to make migrations with the model. If the server is still running, stop it and type the following code.

	python manage.py makemigrations

Now migrate the model.

	python manage.py migrate

### Run The Server
From the command line, type the following code to run the server

	python manage.py runserver

The server should now be running on port http://127.0.0.1:8000/. Click on the link and your Django Rest API should be up and running.

### Add Dogs
### Create A Model, View, and Serializer With A Many To Many Relationship


