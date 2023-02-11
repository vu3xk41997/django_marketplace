# Django_marketplace
Project to build a simple online marketplace using django

## Initialize project
1. Create a folder for project
2. Change directory to folder
3. Run following lines in terminal:
```
python3 -m venv env
source env/bin/activate
pip install django
django-admin startproject marketplace
python manage.py runserver
```
4. Check if server is running on browser

## First app
1. Create new app
* Create a new app folder ```python manage.py startapp core```
* Add core to INSTALLED_APPS in marketplace/settings.py
2. Create first view (index.html)
* Create index method in core/views.py
* Create templates folder in core, and create templates/core/index.html so view can refer to it
* Set up index.html
* Import index from core.views in marketplace/urls.py and add index as path
* Add base.html in templates/core as a foundation for reusing later
* index.html extends from base.html and add code block to show content
* Add title block to in html files to display title
3. Create contact view (contact.html)
* Create contact method in core/views.py
* Create contact.html in template/core
* Import contact in marketplace/urls.py and add contact as path
4. Implement header and footer in base.html
* Use url block to connect to another view ```{% url '{view}' %}```

## Items
1. Create new item app
* Create new item folder ```python manage.py startapp item```
* Register item to INSTALLED_APPS in marketplace/settings.py
2. Create Category class
* Create database model in item/models.py by create Category class
* Update database in terminal:
```
python manage.py makemigrations
python manage.py migrate
```
* item/migrations/0001_initial.py should update and show both id and name
3. Create admin user
* Create admin user
* Run server and go to {address}/admin
* Category model is not shown even though the database has it already
4. Display Category in admin site
* Import Category to item/admin.py and show category in admin site
* Add categories into item/category on admin site
* Correct misspelling and category name on admin site in Category class in item/models.py
5. Create Item class
* Create and implement Items class in item/models.py and import User from django.contrib.auth.models
* Install Pillow to use imageField ```python -m pip install Pillow```
* Update the database again in terminal:
```
python manage.py makemigrations
python manage.py migrate
```
6. Display Item in admin site
* Import Item to item/admin.py and show item in admin site
* Correct item name on admin site in Item class in item/models.py
7. Configurate for images
* Add MEDIA_URL and MEDIA_ROOT in marketplace/settings.py to create media folder in project root folder
* Uploaded item images will be stored in media/item_images folder
8. Display Item and Category on front page
* Import Category and Item in core/views.py
* Also add category and item objects in index method
* Implement index.html to show each item
* Images won't be shown since django doesn't handle image for us
* (Only in development) Import settings and static to marketplace/urls.py and concat static(MEDIA_URL,MEDIA_ROOT) to urlpatterns
* Implement category secion in index/html

## Detail page
1. Add item detail in item/views.py using detail method
* Create templates/item/detail.html in item folder
* Implement item/detail.html to displace detail content
2. Seperate detail urls from main urls by creating item/urls.py
* Allow item urls to have primary key as integer in the path
* Include item urls in main marketplace/urls.py after item path
3. Add related items section beneath item detail page
* Add related item objects (items in same category) in item/views.py
* Excluding current item and only shows 3 items
* Append related items to render in item/views.py
* Implement related items section in detail.html (same is items section in index.html)

## Signing Up
1. Move index and contact paths from marketplace/urls.py to core/urls.py
* Create core/urls.py and create path for views.index and views.contact
* Remove contact path and include core.urls in marketplace/urls.py
* Allows urls first loop through index and contact from core/urls.py before other urls when running marketplace/urls.py
* This cause an error in footer since ```{% url 'contact' %}``` doesn't exist anymore
* Replace ```{% url 'contact' %}``` with ```{% url 'core:contact' %}``` in core/base.html
2. Create signup form page
* Create core/forms.py for creating user, validation, etc.
* Implement core/forms.py
* Create view for signup form in core/views.py using core/form.html
* Create core/form.html display information in signup form page
* Add a path for signup form in core/urls.py
* Change the herf of signup button in core/base.html to ```{% url 'core:signup' %}```
3. Style signup form
* Add each label and field in core/signup.html, show error when field is not filled
* In core/views.py signup(request), check if signup form has been submitted correctly. If true, redirect to login; else, stay on the same page and show error

## Logging in
1. Create login form page
* Add Authentication form in core/forms.py
* Use login view imported from django in core/urls.py
* Create core/login.html, copy from core/signup.html and implement to fit login 
* Create path for login in core/urls.py
2. Handle error after login
* User will be redirect to ./accounts/profile/, which is default django redirection after log in
* Add ```LOGIN_URL = '/login/'\nLOGIN_REDIRECT_URL = '/'\nLOGOUT_REDIRECT_URL = '/'``` in marketplace/settings.py to change rediction urls
3. Hide "sign up" and "log in" buttons in header after log in and add "inbox" and "dashboard" buttons instead
* Add if-else statement in core/base.html to check if user is authenticated or not
* If user is authenticated, show "inbox" and "dashboard" buttons
* If user is not authenticated, show "sign up" and "log in" buttons