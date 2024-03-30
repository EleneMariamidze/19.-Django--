# 19.-Django-Django-ს თემფლეითების ენა

python manage.py startapp book

from django.db import models

class Book(models.Model):
    title = models.CharField(max_length=50)
    author = models.CharField(max_length=100)
    publication_year = models.IntegerField()
    genre = models.CharField(max_length=50)
    available = models.BooleanField(default=True)

    def __str__(self):
        return self.title

python manage.py startapp reader

from django.db import models

class Reader(models.Model):
    name = models.CharField(max_length=100)
    surname = models.CharField(max_length=100)
    email = models.EmailField()

    def __str__(self):
        return f"{self.name} {self.surname}"

#book/admin.py 
from django.contrib import admin
from .models import Book

admin.site.register(Book)

#reader/admin.py
from django.contrib import admin
from .models import Reader

admin.site.register(Reader)

python manage.py makemigrations
python manage.py migrate
python manage.py runserver

#book.view.py
from django.shortcuts import render
from .models import Book

def book_detail(request, book_id):
    book = Book.objects.get(pk=book_id)
    return render(request, 'book_detail.html', {'book': book})

#book.urls.py
from django.urls import path
from .views import book_detail

urlpatterns = [
    path('<int:book_id>/', book_detail, name='book_detail'),
]

 #book/templates/book_detail.html
 <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>{{ book.title }}</title>
</head>
<body>
    <h1>{{ book.title }}</h1>
    <p>Author: {{ book.author }}</p>
    <p>Publication Year: {{ book.publication_year }}</p>
    <p>Genre: {{ book.genre }}</p>
    <p>Release Date: {{ book.release_date }}</p>
    {% if book.available %}
        <p>This book is available.</p>
    {% else %}
        <p>This book is not available.</p>
    {% endif %}
</body>
</html>
