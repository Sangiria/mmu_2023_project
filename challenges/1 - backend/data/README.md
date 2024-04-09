# Пака для файлов результатов

//обновленный список приложений
INSTALLED_APPS = [
    'response',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]

//urls адреса в главном проекте
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('response.urls'))
]

//связка urls адреса с приложением response
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index)
]

//views.py в папке response
from django.shortcuts import render
from django.http import HttpResponse
from django.http.response import JsonResponse
from rest_framework.decorators import api_view
from rest_framework.parsers import JSONParser
from django.core.files import File


@api_view(['GET', 'POST', 'DELETE'])
def index(request):
    state = 'off'
    if request.method == 'GET':
        with open("switch.state.txt", "r+") as f:
            state = f.read()
            if state == '':
                f.write('off')
        return JsonResponse({'state': state})
    if request.method == 'POST':
        state = JSONParser().parse(request)
        with open("switch.state.txt", "w+") as f:
            f.write(state)
        return JsonResponse({'state': state})
