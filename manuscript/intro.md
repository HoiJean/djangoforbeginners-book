Welcome to _Django for Beginners_, a step-by-step approach to learning web development with [Python](https://www.python.org/) and [Django](https://djangoproject.com). In this book you will learn how to create, test, and deploy five progressively more complex web applications in Django.

I wrote this book because while Django is [extremely well documented](https://docs.djangoproject.com/en/2.0/) there is a distinct lack of beginner-friendly tutorials available. Most content is aimed at experienced developers already comfortable with web development rather than beginners trying to make sense of Django, Python, and web development--often all at the same time.

A good example is the [official polls tutorial](https://docs.djangoproject.com/en/2.0/intro/tutorial01/) which showcases the depth of Django, but not its ease-of-use. If you've tried--and failed--to complete the polls tutorial, then this book is for you. And even if you have previous web development experience, there are a number of Django best practices covered here that are worth exploring including proper local development setup, working with virtual environments, and implementing a custom user model.

## Prerequisites

You don't need to have previous Python or web development experience to complete this book. It is written so that even a total beginner can follow along and feel the magic of writing their own web applications from scratch. However if you are serious about a career in web development, you will eventually need to invest the time to learn Python, HTML, and CSS properly. A list of recommended resources for further study is included in [the Conclusion]({{ site.baseurl }}{% post_url book/2010-01-01-conclusion %}).

## Book Structure

In the [first chapter]({{ site.baseurl }}{% post_url book/2010-01-01-initial-setup %}) we review how to properly configure our computer for Django development. We're using bleeding edge tools in this book: the most recent version of Django (2.0), Python (3.6), and [Pipenv](https://docs.pipenv.org/) to manage our virtual environments. We'll also introduce the command line and how to work with a modern text editor.

In [Chapter 2](({{ site.baseurl }}{% post_url book/2010-01-01-hello-world %})) we build our first project, a minimal _hello world_ application that demonstrates how to setup new Django projects. Because establishing good software practices is important, we'll also save our work with _git_ and upload a copy to a remote code repository on [Bitbucket](https://bitbucket.org/product).

In [Chapter 3]({{ site.baseurl }}{% post_url book/2010-01-01-simple %}) we make, test, and deploy a _simple_ app that introduces templates and class-based views. Templates are how Django allows for DRY (Don't Repeat Yourself) development with HTML and CSS while class-based views require a minimal amount of code to use and extend core functionality in Django. They're awesome as you'll soon see. We also add our first tests and deploy to [Heroku](https://www.heroku.com/) which has a free tier we'll use throughout this book. Using platform-as-a-service providers like Heroku transforms development from a painful, time-consuming process into something that takes just a few mouse clicks.

By [Chapter 4]({{ site.baseurl }}{% post_url book/2010-01-01-message-board %}) we're ready for our first database-backed project, a _message board_ app. Django provides a powerful [ORM](https://en.wikipedia.org/wiki/Object-relational_mapping) that allows us to write concise Python for our database tables. We'll explore the built-in admin app which provides a graphical way to interact with our data and can be even used as a Content Management System (CMS) similar to Wordpress. Of course we also write tests for all our code, store a remote copy on Bitbucket, and deploy to Heroku.

Next up in [Chapters 5-7]({{ site.baseurl }}{% post_url book/2010-01-01-blog %}) is a robust blog application that demonstrates how to perform CRUD (Create-Read-Update-Delete) functionality in Django. We'll find that Django's generic class-based views mean we have to write only a small amount of actual code for this! Then we'll add forms and user authentication (login logout, signup).

Finally we're ready for our final project: a _social messaging_ app similar to Twitter. In [Chapter 8]({{ site.baseurl }}{% post_url book/2010-01-01-custom-user-model %}) we start our new project the right way with a custom user model and then implement signup, login, and logout functionality in [Chapter 9]({{ site.baseurl }}{% post_url book/2010-01-01-signup-login-logout %}). [Chapter 10]({{ site.baseurl }}{% post_url book/2010-01-01-bootstrap %}) introduces [Bootstrap](https://getbootstrap.com/) for styling and we complete our user registration flow with password change and reset via email in [Chapter 11]({{ site.baseurl }}{% post_url book/2010-01-01-password-change-reset %}). Our core social messaging app and layout is built in [Chapter 12]({{ site.baseurl }}{% post_url book/2010-01-01-social-messaging-app %}). In [Chapter 13]({{ site.baseurl }}{% post_url book/2010-01-01-comments-app %}) we add a _comments_ feature and introduces one-to-many foreign key relationships. Finally in [Chapter 14]({{ site.baseurl }}{% post_url book/2010-01-01-authorization %}) we'll explore authorization by restricting parts of the app only to logged-in users.

[The conclusion]({{ site.baseurl }}{% post_url book/2010-01-01-conclusion %}) provides an overview of the major concepts introduced in the book and a list of recommended resources for further learning.

While you can pick and choose chapters to read, the book's structure is very deliberate. Each app/chapter introduces a new concept and reinforces past teachings. I **highly recommend** reading it in order, even if you're eager to skip ahead. Later chapters won't cover previous material in the same depth as earlier chapters.

By the end of this book you'll have an understanding of how Django works in practice, the ability to build apps on your own, and the background needed to fully take advantage of additional resources for learning intermediate and advanced Django techniques.

There are several "bonus" chapters on advanced Django concepts that will be covered in a follow-up book, <a href="http://intermediatedjango.com/" target="\_blank">Intermediate Django</a>. They are included here to showcase the rich Django ecosystem available. The topics include:

* [adding social authentication]({{ site.baseurl }}{% post_url book/2010-01-01-django-allauth %}) via 3rd party services like Gmail and Github
* using [Docker and PostgreSQL]({{ site.baseurl }}{% post_url book/2010-01-01-docker-postgresql %}) to mimic a production environment locally
* an [introduction to Django Rest Framework]({{ site.baseurl }}{% post_url book/2010-01-01-django-rest-framework %})
* how to build a [Django Rest Framework todo list API]({{ site.baseurl }}{% post_url book/2010-01-01-todo-list-api %}) consumed by a [React](https://reactjs.org/) frontend

## Book layout

There are many code examples in this book, which are denoted as follows:

```python
# This is Python code
print(Hello, World)
```

For brevity we will use dots `...` to denote existing code that remains unchanged, for example in a function we are updating.

```python
def make_my_website:
    ...
    print("All done!")
```

We will also use the command line console frequently (starting in [Chapter 1: Initial Setup]({{ site.baseurl }}{% post_url book/2010-01-01-initial-setup %})) to execute commands, which take the form of a `$` prefix in traditional Unix style.

```
$ echo "hello, world"
```

The result of this particular command is the next line will state:

```
"hello, world"
```

We will typically combine a command and its output. The command will always be prefaced by a `$` and the output will not. For example, the command and result above will be represented as follows:

```
$ echo "hello, world"
hello, world
```

Complete source code for all examples can be found on Github in the [Django for Beginners repository](https://github.com/wsvincent/djangoforbeginners).

## Conclusion

Django is an excellent choice for any developer who wants to build modern, robust web applications with a minimal amount of code. In the next chapter we'll learn how to configure any computer for Django development.

Continue on to [Chapter 1: Getting Started]({{ site.baseurl }}{% post_url book/2010-01-01-initial-setup %}).
