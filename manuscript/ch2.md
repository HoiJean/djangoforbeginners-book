In this chapter we'll build a Django project that simply says "Hello, World" on the homepage. This is [the traditional way](https://en.wikipedia.org/wiki/%22Hello,_World!%22_program) to start a new programming language or framework. We'll also work with _git_ for the first time and deploy our code to Bitbucket.

If you need help configuring your local development environment, please refer to [Chapter 1: Initial Setup]({{ site.baseurl }}{% post_url book/2010-01-01-initial-setup %}).

Complete source code can be <a href="https://github.com/wsvincent/djangoforbeginners/tree/master/ch2-hello-world-app/" target="\_blank">found on Github</a>.

## Initial Setup

To start navigate to a new directory on your computer. For example, we can create a `helloworld` folder on the Desktop with the following commands.

```
$ cd ~/Desktop
$ mkdir helloworld
$ cd helloworld
```

Make sure you're not within an existing virtual environment at this point. If you see text in parentheses `()` before the dollar sign `$` then you are. To exit it, type `exit` and hit `Return`. The parentheses should disappear which means that virtual environment is no longer active.

We'll use `pipenv` to create a new virtual environment, install Django and then activate it.

```
$ pipenv install django
$ pipenv shell
```

You should see parentheses now at the beginning of your command line prompt in the form `(helloworld-XXX)` where `XXX` represents random characters. On my computer I see `(helloworld-415ivvZC)`. I'll display `(helloworld)` here in the text but you will see something slightly different on your computer.

Create a new Django project called `helloworld_project` making sure to include the period `.` at the end of the command so that it is installed in our current directory. Without the period Django creates a new directory _before_ our new Django directory.

```
(helloworld) $ django-admin startproject helloworld_project .
```

If you use the `tree` command you can see what our Django project structure now looks like. (**Note**: If `tree` doesn't work for you, install it with Homebrew: `brew install tree`.)

```
(helloworld) $ tree
.
├── helloworld_project
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── manage.py

1 directory, 5 files
```

The `settings.py` file controls our project's settings, `urls.py` tells Django which pages to build in response to a browser or url request, and `wsgi.py`, which stands for [web server gateway interface](https://en.wikipedia.org/wiki/Web_Server_Gateway_Interface), helps Django serve our eventual web pages. The last file `manage.py` is used to execute various Django commands such as running the local web server or creating a new app.

Django comes with a built-in web server for local development purposes. We can start it with the `runserver` command.

```
(helloworld) $ python manage.py runserver
```

If you visit [http://127.0.0.1:8000/](http://127.0.0.1:8000/) you should see the following image:

![Django welcome page](/assets/images/book/02_django_welcome.png)

## Create an app

Django uses the concept of projects and apps to keep code clean and readable. A single Django project contains one or more apps within it that all work together to power a web application. For example a real-world Django e-commerce site might have one app for user authentication, another app for payments, and a third app to power item listing details.

We need to create our first app which we'll call `pages`. From the command line, quit the server with `Control+c`. Then use the `startapp` command.

```
(helloworld) $ python manage.py startapp pages
```

If you look again inside the directory with the `tree` command you'll see Django has created a `pages` directory with the
following files:

```
├── pages
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── migrations
│   │   └── __init__.py
│   ├── models.py
│   ├── tests.py
│   └── views.py
```

Let's review what each new `pages` app file does:

* `admin.py` is a configuration file for the built-in Django Admin app
* `apps.py` is a configuration file for the app itself
* `migrations/` keeps track of any changes to our `models.py` file so our database and `models.py` stay in sync
* `models.py` is where we define our database models, which Django automatically translates into database tables
* `tests.py` is for our app-specific tests
* `views.py` is where we handle the request/response logic for our Web app

Even though our new app exists within the Django project, Django doesn't "know" about it until we explicitly add it to the app. In your text editor (VS Code, Atom, or other) open the `settings.py` file and scroll down to `INSTALLED_APPS` where you'll see six built-in Django apps already there. Add our new `pages` app at the bottom:

```python
# helloworld_project/settings.py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'pages',
]
```

## Views and URLConfs

In Django, _Views_ determine **what** content is displayed on a given page while _URLConfs_ determine **where** that
content is going.

When a user requests a specific page, for example `/` which represents the home page, the URLConf uses a
[regular expression](https://en.wikipedia.org/wiki/Regular_expression) to map that request to the appropriate view
function which then returns the correct data.

In other words, our _view_ will output the text "Hello, World" while our _url_ will ensure that when the user visits `/` they are redirected to the correct view.

Let's start by updating the `views.py` file in our `pages` app to look as follows:

```python
# pages/views.py
from django.http import HttpResponse


def homePageView(request):
    return HttpResponse('Hello, World!')
```

Basically we're saying whenever the view function `homePageView` is called, return the text "Hello, World!" More specifically, we've imported the built-in
[HttpResponse](https://docs.djangoproject.com/en/2.0/ref/request-response/#django.http.HttpResponse) method so we can
return a response object to the user. We've created a function called `homePageView` that accepts the `request` object
and returns a response with the string `Hello, World!`.

Now we need to configure our urls. Within the `pages` app, create a new `urls.py` file.

```
(helloworld) $ touch pages/urls.py
```

Then update it with the following code:

```python
# pages/urls.py
from django.urls import path

from . import views

urlpatterns = [
    path('', views.homePageView, name='home')
]
```

On the top line we import `path` from Django to power our `urlpattern` and on the next line we import our views. Our urlpattern has three parts:

* a Python regular expression for the empty string `''`
* specify the view which is called `homePageView`
* add an optional url name of `'home'`

In other words, if the user requests a page at `/` then use the view called `homePageView`.

We're _almost_ done. The last step is to configure our project-level `urls.py` file too. Remember that it's common to have multiple apps within a single Django project, so they each need their own route.

Update the `helloworld_project/urls.py` file as follows:

```python
# helloworld_project/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('pages.urls')),
]
```

We've imported `include` on the second line next to `path` and then created a new urlpattern for our `pages` app. Now whenever a user visits the homepage at `/` they will first be routed to the `pages` app and then to the `homePageView` view.

## Hello, world!

We have all the code we need now! To confirm everything works as expected, restart our Django server:

```
(helloworld) $ python manage.py runserver
```

If you refresh the browser for [http://127.0.0.1:8000/](http://127.0.0.1:8000/) it now displays the text "Hello, world!"

![Hello world homepage](/assets/images/book/02_helloworld.png)

## Git

In the previous chapter we also installed **git** which is a version control system. Let's use it here. The first step is to initialize (or add) _git_ to our repository.

```
(helloworld) $ git init
```

If you then type `git status` you'll see a list of changes since the last git commit. Since this is our first commit, this list is all of our changes so far.

```
(helloworld) $ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        db.sqlite3
        helloworld_project/
        manage.py
        pages/
        Pipfile
        Pipfile.lock

nothing added to commit but untracked files present (use "git add" to track)
```

We next want to add _all_ changes by using the command `add -A` and then `commit` the changes along with a message describing what has changed.

```
(helloworld) $ git add -A
(helloworld) $ git commit -m 'initial commit'
```

<!-- That's it for now but in the coming chapters we'll use *git* to:

* store our code in a remote repository on [Github](https://github.com/)
* deploy our code using [Heroku](https://www.heroku.com/)
* run continuous integration tests with [CircleCI](circleci.com) -->

## Bitbucket

It's a good habit to create a remote repository of your code for each project. This way you have a backup in case anything happens to your computer and more importantly, it allows for collaboration with other software developers. The two most popular choices are [Bitbucket](https://bitbucket.org/) and [Github](https://github.com/).

In this book we will use Bitbucket because it allows private repositories **for free**. Github charges a fee. Public repositories are available for anyone on the internet to use; private repositories are not. When you're learning web development, it's best to stick to private repositories so you don't inadvertently post critical information such as passwords online.

To get started on Bitbucket, [sign up for a free account](https://bitbucket.org/account/signup/). After confirming your account via email you'll be redirected to the Bitbucket welcome page. Click on the link for "Create a repository".

![Bitbucket welcome page](/assets/images/book/02_bitbucket_welcome.png)

Then on the "Create Repo" page enter in the name of your repository: "hello-world". Then click the blue "Create repository button":

![Bitbucket create repo](/assets/images/book/02_bitbucket_create_repo.png)

On the next page, click on the link on the bottom for "I have an existing project" which will open a dropdown:

![Bitbucket existing project](/assets/images/book/02_bitbucket_existing_project.png)

We're already in the directory for our repo so skip Step 1. In Step 2, we'll use two commands to add our project to Bitbucket. Note that your command will differ from mine since you have a different username. The general format is the below where `<USER>` is your Bitbucket username.

```
(helloworld) $ git remote add origin https://<USER>@bitbucket.org/<USER>/hello-world.git
```

After running this command to configure git with this Bitbucket repository, we must "push" our code into it.

```
(helloworld) $ git push -u origin master
```

Now if you go back to your Bitbucket page and refresh it, you'll see the code is now online!

![Bitbucket overview](/assets/images/book/02_bitbucket_overview.png)

Since we're done, go ahead and exit our virtual environment with the `exit` command.

```
(helloworld) $ exit
```

You should now see no parentheses on your command line, indicating the virtual environment is no longer active.

## Conclusion

Congratulations! We've covered a lot of fundamental concepts in this chapter. We built our first Django application and learned about Django's project and app structure. We started to learn about views, urls, and the internal web server. And we worked with git to track our changes and pushed our code into a private repo on Bitbucket.

Continue on to [Chapter 3: A simple app]({{ site.baseurl }}{% post_url book/2010-01-01-simple %}) where we'll build a more complex Django application using templates and class-based views. <!--and deploy it live on the internet.-->
