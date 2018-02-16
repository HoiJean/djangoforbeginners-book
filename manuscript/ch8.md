# Chapter 8: Custom User Model

For the rest of the book we will build out a _social messaging_ app very similar to Twitter. It will allow users to write their own microposts and make comments on other user's posts. We'll implement a complete user authentication flow with custom signup, login, logout, password change, and password reset features. And we'll learn how to restrict site access so that only logged-in users can view most areas of the site.

To start we'll configure our `User` model but it's important to note that unlike our previous _blog_ app which used the default user model, we will instead use a **custom user model**. This is a Django best practice advocated <a href="https://docs.djangoproject.com/en/2.0/topics/auth/customizing/#using-a-custom-user-model-when-starting-a-project" target="\_blank">in the official documentation</a>, however many Django developers don't learn about it until it's too late. Don't make that mistake.

Our custom user model in this app will simply extend the existing `User` model. In other words, it will have identical functionality but because it is custom, and not the default, we have the flexibility to make changes later on as needed. For example maybe later on we want to add a "date of birth" field to `User`. Or maybe we want to have the option of using just a `name` field rather than the built-in `FirstName` and `LastName` fields.

If we use a custom user model from the beginning, we can future-proof our Django applications. **Always use a custom user model for new projects.**

<!-- For further context, Django core developer Russell Keith-Magee has an <a href="https://www.youtube.com/watch?v=458KmAKq0bQ" target="\_blank">excellent video talk</a> that dives into the details of `User` and some of the shortcomings of its legacy history. -->

<!-- And given Django's legacy history there are a few things about the built-in `User` model that are often changed in production websites. For example, Django uses the username/email/password pattern for both signup and login, however these days it is more common to simply use email/password. The username field is not case sensitive so "Will" and "will" are treated as distinct usernames. The email field is also not unique by default, so two different usernames can in theory have the same email address. Finally the `User` model has "first name" and "last name" fields on it but these are not friendly to international naming standards: in many cultures either one name or a string of names is used. -->

<!-- Django core developer Russell Keith-Magee has an <a href="https://www.youtube.com/watch?v=458KmAKq0bQ" target="\_blank">excellent video talk</a> that explores some of the legacy shortcomings of Django's default `User` model.

All that said, the Django `User` model still gets most things right and it manages all the passwords, security, and authorizations for us. For our *social messaging* app we will use a custom user model that extends the default `User` model but not make any changes to it. Let's get started. -->

Complete source code for this chapter can be found <a href="https://github.com/wsvincent/djangoforbeginners/tree/master/ch8-custom-user-model" target="\_blank">on Github</a>.

## Setup

The first step is to create a new Django project from the command line. We need to do several things:

* create a new directory for our code
* create a new virtual environment `msg`
* install Django
* make a new Django project `msg_project`
* make a new app `users`

We're calling our app for managing users `users` here but you'll also see it frequently called `accounts` in open source code. The actual name doesn't matter as long as you're consistent when referring to it throughout the project.

Here are the commands to run:

```
$ cd ~/Desktop
$ mkdir msg && cd msg
$ pipenv install django
$ pipenv shell
(msg) $ django-admin startproject msg_project .
(msg) $ python manage.py startapp users
(msg) $ python manage.py runserver
```

Note that we **did not** run `migrate` to configure our database. It's important to wait until **after** we've created our new custom user model before doing so given how tightly connected the user model is to the rest of Django.

If you navigate to [http://127.0.0.1:8000](http://127.0.0.1:8000) you’ll see the familiar Django welcome screen.

![Welcome  page]({{site.url}}/assets/images/book/django-custom-user-model/welcome.png)

Creating our custom user model requires four steps:

* update `settings.py`
* create a new User model and manager
* create new forms for `UserCreation` and `UserChangeForm`
* update the admin

We'll go through each in detail.

## Settings

We've created a new app `users` so we need to add it to our `INSTALLED_APPS` setting. Within `settings.py` add it at the bottom of the list.

```python
# msg_project/settings.py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'users', # new
]
```

Then in the same `settings.py` file we need to swap the default user model for our custom user model. We can do this with the setting [AUTH_USER_MODEL](https://docs.djangoproject.com/en/2.0/ref/settings/#std:setting-AUTH_USER_MODEL).

At the bottom of the `settings.py` file add the following line:

```python
# msg_project/settings.py
AUTH_USER_MODEL = 'users.CustomUser'
```

This line says tells Django that the user auth model to use is in our `users` app and called `CustomUser`. Now we need to create it.

## Models and Managers

Django models abstract away much of the complexity required to build database tables by hand. We've already used them to create our _message board_ and _blog_ models in just a few lines of code.

Under-the-hood in Django [model managers](https://docs.djangoproject.com/en/2.0/topics/db/managers/) are the actual interface for translating models into actual database queries. They handle the heavy lifting so we can just write simple Python in our _model_ classes.

**Every model has a corresponding model manager.** Thus to make our own `CustomUser` we'll need to create both a new model and manager.

We can create our own `CustomUserManager` and `CustomUser` by extending the model and manager used by Django for the default `User`. The existing model is called [AbstractUser](https://docs.djangoproject.com/en/2.0/topics/auth/customizing/#django.contrib.auth.models.AbstractUser) and its corresponding model manager is [UserManager](https://docs.djangoproject.com/en/2.0/ref/contrib/auth/#django.contrib.auth.models.UserManager).

Update `users/models.py` as follows.

```python
# users/models.py
from django.contrib.auth.models import AbstractUser, UserManager


class CustomUserManager(UserManager):
    pass

class CustomUser(AbstractUser):
    objects = CustomUserManager()
```

Our `CustomUserManager` literally does nothing at this point except extend `UserManager`, which means it contains all of `UserManager`'s code which we can modify later, if needed.

Our `CustomUser` extends `AbstractUser` and specifies that all its `objects` come from the `CustomUserManager`. That's it!

It's natural at this point to feel a little confused. Model managers are an advanced Django concept not normally introduced in tutorials for beginners, however we need to use them to create our custom user model. They provide a glimpse at all the hard work of the Django core team over the years to abstract away much of the complexity inherent in any web application.

So do your best to understand how models and managers work together but don't worry if it doesn't quite click. That usually takes some time.

## Forms

Whenever we sign up for a new account or login we're dealing with forms. Django comes with two forms for the default `User` model that we'll need to customize: [UserCreationForm](https://docs.djangoproject.com/en/2.0/topics/auth/default/#django.contrib.auth.forms.UserCreationForm) and [UserChangeForm](https://docs.djangoproject.com/en/2.0/topics/auth/default/#django.contrib.auth.forms.UserChangeForm).

Stop the local server with `Control+c` and create a new file in the `users` app called `forms.py`.

```
(msg) $ touch users/forms.py
```

We'll update it with the following code to largely subclass the existing forms.

```python
# users/forms.py
from django import forms
from django.contrib.auth.forms import UserCreationForm, UserChangeForm
from .models import CustomUser


class CustomUserCreationForm(UserCreationForm):

    class Meta(UserCreationForm.Meta):
        model = CustomUser
        fields = UserCreationForm.Meta.fields


class CustomUserChangeForm(UserChangeForm):

    class Meta:
        model = CustomUser
        fields = UserChangeForm.Meta.fields
```

At the top we're importing the existing forms which we'll extend as well as our own newly-created `CustomUser` model. We create a new class called `CustomUserCreationForm` that extends the existing `UserCreationForm`. Notably we specify that the model to use is `CustomUser` not the default user model.

In `CustomUserChangeForm` we're again subclassing an existing form `UserChangeForm` and specifying it use our new `CustomUser` model.

That's it! Now our forms will work correctly.

## Admin

The final step is to update our `admin.py` file. The default `User` model is tightly coupled to the Django admin. After all we can view and modify users within the admin so it makes sense we need to tell the admin about our new `CustomUser` model.

To do that we will update the existing [UserAdmin](https://docs.djangoproject.com/en/2.0/topics/auth/customizing/#extending-the-existing-user-model) class.

There's a bit of code in this one although ultimately all we're doing is extending `UserAdmin` to use our new `CustomUser` model and our two new forms.

```python
# users/admin.py
from django.contrib import admin
from django.contrib.auth.admin import UserAdmin

from .forms import CustomUserCreationForm, CustomUserChangeForm
from .models import CustomUser


class CustomUserAdmin(UserAdmin):
    model = CustomUser
    add_form = CustomUserCreationForm
    form = CustomUserChangeForm

admin.site.register(CustomUser, CustomUserAdmin)
```

At the top we import the existing `UserAdmin` as well as our new forms and user model. Then we create a new class `CustomUserAdmin` and specify it should use the new model and forms. Finally we "register" our changes with the admin, just as we do with our apps.

And we're done!

Go ahead and run `makemigrations` and `migrate` for the first time to create a new database that uses the custom user model.

```
(msg) $ python manage.py makemigrations
(msg) $ python manage.py migrate
```

## Superuser

Let's create a superuser account to confirm that everything is working as expected.

On the command line type the following command and go through the prompts.

```
(msg) $ python manage.py createsuperuser
```

The fact that this works is the first proof our custom user model works as expected. Let's view things in the admin too to be extra sure.

Start up the web server.

```
(msg) $ python manage.py runserver
```

Then navigate to the admin at [http://127.0.0.1:8000/admin](http://127.0.0.1:8000/admin).

![Admin  page]({{site.url}}/assets/images/book/django-custom-user-model/admin.png)

If you click on the link for "Users" you should see your superuser account.

![Admin  one user]({{site.url}}/assets/images/book/django-custom-user-model/admin_one_user.png)

## Conclusion

With our custom user model complete, we can now focus on building out the rest of our _social messaging_ app. In next chapter we'll configure and customize signup, login, and logout pages.

Continue on to [Chapter 9: Signup, Login, and Logout]({{ site.baseurl }}{% post_url book/2010-01-01-signup-login-logout %}).
