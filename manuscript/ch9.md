# Chapter 9: Signup, Login, and Logout

Now that we have a working custom user model we can add the functionality every website needs: the ability to signup, login, and logout users. Django provides everything we need for login and logout but we will need to create our own form to sign up new users. We'll also build a basic homepage with links to all three features so we don't have to type in the URLs by hand all the time.

Complete source code can be <a href="https://github.com/wsvincent/djangoforbeginners/tree/master/ch9-signup-login-logout" target="\_blank">found on Github</a>.

## Templates

By default the Django template loader looks for templates in a nested structure within each app. So a `home.html` template in `users` would need to be located at `users/templates/users/home.html`. But a project-level `templates` folder approach is cleaner and scales better so that's what we'll use.

Let's create a new `templates` directory and within it a `registration` folder as that's where Django will look for the login template.

```
(msg) $ mkdir templates
(msg) $ mkdir templates/registration
```

Now we need to tell Django about this new directory by updating the configuration for `'DIRS'` in `settings.py`. This is a one-line change.

```python
# msg_project/settings.py
'DIRS': [os.path.join(BASE_DIR, 'templates')],
```

If you think about what happens when you login or logout of a site, you are immediately redirected to a subsequent page. We need to tell Django where to send users in each case. The `LOGIN_REDIRECT_URL` and `LOGOUT_REDIRECT_URL` settings do that. We'll configure both to redirect to our homepage which will have the named URL of `'home'`.

Remember that when we create our URL routes we have the option to add a `name` to each one. So when we make the homepage URL we'll make sure call it `'home'`.

Add these two lines at the bottom of the `settings.py` file.

```python
# msg_project/settings.py
LOGIN_REDIRECT_URL = 'home'
LOGOUT_REDIRECT_URL = 'home'
```

Now we can create four new templates:

```
(msg) $ touch templates/registration/login.html
(msg) $ touch templates/base.html
(msg) $ touch templates/home.html
(msg) $ touch templates/signup.html
```

Here's the HTML code for each file to use. The `base.html` will be inherited by every other template in our project. By using a block like {% raw %}`{% block content %}`{% endraw %} we can later override the content _just in this place_ in other templates.

```html
<!-- templates/base.html -->
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Social Messaging App</title>
</head>
<body>
  <main>
    {% raw %}{% block content %}
    {% endblock %}{% endraw %}
  </main>
</body>
</html>
```

```html
<!-- templates/home.html -->
{% raw %}{% extends 'base.html' %}

{% block title %}Home{% endblock %}

{% block content %}
{% if user.is_authenticated %}
  Hi {{ user.username }}!
  <p><a href="{% url 'logout' %}">logout</a></p>
{% else %}
  <p>You are not logged in</p>
  <a href="{% url 'login' %}">login</a> |
  <a href="{% url 'signup' %}">signup</a>
{% endif %}
{% endblock %}{% endraw %}
```

```html
<!-- templates/registration/login.html -->
{% raw %}{% extends 'base.html' %}

{% block title %}Login{% endblock %}

{% block content %}
<h2>Login</h2>
<form method="post">
  {% csrf_token %}
  {{ form.as_p }}
  <button type="submit">Login</button>
</form>
{% endblock %}{% endraw %}
```

```html
<!-- templates/signup.html -->
{% raw %}{% extends 'base.html' %}

{% block title %}Sign Up{% endblock %}

{% block content %}
  <h2>Sign up</h2>
  <form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Sign up</button>
  </form>
{% endblock %}{% endraw %}
```

Our templates are now all set. Still to go are our urls and views.

## URLs

Let's start with the url routes. In our project-level `urls.py` file we want to have our `home.html` template appear as the homepage. But we don't want to build a dedicated `pages` app just yet, so we can use the shortcut of importing `TemplateView` and setting the `template_name` right in our url pattern.

Next we want to "include" both the `users` app and the built-in `auth` app. So make sure to add `include` to our imports on the second line. We'll add url patterns for both apps at `/users/`.

```python
# msg_project/urls.py
from django.contrib import admin
from django.urls import path, include
from django.views.generic.base import TemplateView

urlpatterns = [
    path('', TemplateView.as_view(template_name='home.html'), name='home'),
    path('admin/', admin.site.urls),
    path('users/', include('users.urls')),
    path('users/', include('django.contrib.auth.urls')),
]
```

Now create a `urls.py` file in the `users` app.

```
(msg) $ touch users/urls.py
```

We don't need to include urls or views for login and logout since they're already provided by Django for us in the built-in `auth` app. We just need to configure a route for our signup page.

Update `users/urls.py` with the following code:

```python
# users/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('signup/', views.SignUp.as_view(), name='signup'),
]
```

The last step is our `views.py` file which will contain the logic for our signup form. We're using Django's generic `CreateView` here and telling it to use our custom `form_class`, to redirect to `login` once a user signs up successfully, and that our template is named `signup.html`.

```python
# users/views.py
from django.urls import reverse_lazy
from django.views import generic

from .forms import CustomUserCreationForm

class SignUp(generic.CreateView):
    form_class = CustomUserCreationForm
    success_url = reverse_lazy('login')
    template_name = 'signup.html'
```

Ok, phew! We're done. Let's test things out.

Start up the server with `python manage.py runserver` and go to the homepage at [http://127.0.0.1:8000/](http://127.0.0.1:8000/).

![Home page logged in]({{site.url}}/assets/images/book/signup-login-logout/home_loggedin.png)

We logged in to the admin in the previous chapter so you should see a personalized greeting here. Click on the _logout_ link.

![Home page logged out]({{site.url}}/assets/images/book/signup-login-logout/home_loggedout.png)

Now we're on the logged out homepage. Go ahead and click on _login_ link and use your **superuser** credentials.

![Login]({{site.url}}/assets/images/book/signup-login-logout/login.png)

Upon successfully logging in you'll be redirected back to the homepage and see a personalized greeting. It works!

![Home page logged in]({{site.url}}/assets/images/book/signup-login-logout/home_loggedin.png)

Now use the _logout_ link and then click on _signup_.

![Signup page]({{site.url}}/assets/images/book/signup-login-logout/signup.png)

Create a new user. Mine is called `testuser`. After successfully submitting the form you'll be redirected to the login page. Login with your new user and you'll again be redirected to the homepage with a personalized greeting for the new user.

![Home page for testuser]({{site.url}}/assets/images/book/signup-login-logout/testuser.png)

Everything works as expected.

## Admin

Let's also log in to the admin to view our two user accounts. Navigate to [http://127.0.0.1:8000/admin](http://127.0.0.1:8000/admin) and ...

![Admin login wrong]({{site.url}}/assets/images/book/signup-login-logout/admin_login_wrong.png)

What's this! Why can't we log in?

Well we're logged in with our new `testuser` account not our superuser account. Only a superuser account has the permissions to log in to the admin! So use your superuser account to log in.

After you've done that you should see the normal admin homepage.

![Admin homepage]({{site.url}}/assets/images/book/signup-login-logout/admin_homepage.png)

Click on `Users` and you can see our two users: the one we just created and your previous superuser name (mine is `wsv`).

![Users in the Admin]({{site.url}}/assets/images/book/signup-login-logout/admin.png)

So everything works as expected.

<!-- ## Tests
Since we've added new functionality it's time for tests. We want to ensure that our homepage exists and the signup, login, and logout links work correctly.

XXXXXXXXXXXXXXXXXXXXXXX -->

## Conclusion

Our signup, login, and logout functionality is now wired up. But you may have noticed our site doesn't look very good. In the next chapter we'll add [Bootstrap](https://getbootstrap.com/) for styling and create a dedicated `pages` app.

Continue on to [Chapter 10: Bootstrap]({{ site.baseurl }}{% post_url book/2010-01-01-bootstrap %}).
