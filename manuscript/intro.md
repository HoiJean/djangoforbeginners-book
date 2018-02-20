# Introduction

Welcome to _Django for Beginners_, a step-by-step approach to learning web development with [Python](https://www.python.org/) and [Django](https://djangoproject.com). In this book you will build and deploy five progressively more complex web applications, starting with a simple "Hello, World" app and progressing to both a blog app and a Twitter-clone. Throughout we'll be using best practices from the Django, Python, and web development communities, especially the thorough use of testing.

This book is regularly updated and features the latest versions of both Django (2.0) and Python (3.6x). It also uses [Pipenv](https://docs.pipenv.org/) which is now the officially recommended by [Python.org](https://packaging.python.org/tutorials/managing-dependencies/#managing-dependencies) for managing Python packages and virtual environments.

## Why Django

Django is a free, open source web framework that is unusually friendly to both beginners and advanced programmers. It is robust enough to be used by many of the largest websites in the world--Instagram, Pinterest, Bitbucket, Disqus--but also flexible enough to be a good choice for early-stage startups and prototyping personal projects.

Django inherited Python's "batteries-included" approach and includes out-of-the box support for common tasks in web development:

* user authentication
* templates, routes, and views
* admin interface
* robust security
* support for multiple database backends
* and much much more

This approach makes our job as web developers much, much easier. We can focus on what makes our web application unique rather than reinventing the wheel when it comes to standard web application functionality.

Even though Django is over 13 years old at this point--a grizzled veteran in software years--it shows no signs of slowing down. It remains under active development and is constantly adding new features and security improvements. You can find an [updated product timeline](https://www.djangoproject.com/download/#supported-versions) here.

## Why this book

I wrote this book because while Django is [extremely well documented](https://docs.djangoproject.com/en/2.0/) there is a distinct lack of beginner-friendly tutorials available. When I first learned Django years ago I struggled to complete the [official polls tutorial](https://docs.djangoproject.com/en/2.0/intro/tutorial01/). Why was this so hard I remember thinking?

With more experience I recognize now that the writers of the Django docs faced a difficult choice: they could emphasize Django's **ease-of-use** or its **depth**, but not both. They choose the latter and as a professional developer I appreciate the choice, but as a beginner I found it so...**frustrating!**

My goal is that this book fills in the gaps and showcases how beginner-friendly Django really is.

Even experienced web developers, however, will benefit from this book. Django best practices are constantly evolving and this book implements many of them including a modern local development setup with pipenv, project setup, and working with custom user models.

## Prerequisites

You don't need previous Python or web development experience to complete this book. It is intentionally written so that even a total beginner can follow along and feel the magic of writing their own web applications from scratch. However if you are serious about a career in web development, you will eventually need to invest the time to learn Python, HTML, and CSS properly. A list of recommended resources for further study is included in **the Conclusion**.

## Book Structure

In the **first chapter** we review how to properly configure our computer for Django development. We're using bleeding edge tools in this book: the most recent version of Django (2.0), Python (3.6), and [Pipenv](https://docs.pipenv.org/) to manage our virtual environments. We'll also introduce the command line and how to work with a modern text editor.

In **Chapter 2** we build our first project, a minimal \_hello world* application that demonstrates how to setup new Django projects. Because establishing good software practices is important, we'll also save our work with _git_ and upload a copy to a remote code repository on [Bitbucket](https://bitbucket.org/product).

In **Chapter 3** we make, test, and deploy a \_simple* app that introduces templates and class-based views. Templates are how Django allows for DRY (Don't Repeat Yourself) development with HTML and CSS while class-based views require a minimal amount of code to use and extend core functionality in Django. They're awesome as you'll soon see. We also add our first tests and deploy to [Heroku](https://www.heroku.com/) which has a free tier we'll use throughout this book. Using platform-as-a-service providers like Heroku transforms development from a painful, time-consuming process into something that takes just a few mouse clicks.

By **Chapter 4** we're ready for our first database-backed project, a \_message board* app. Django provides a powerful [ORM](https://en.wikipedia.org/wiki/Object-relational_mapping) that allows us to write concise Python for our database tables. We'll explore the built-in admin app which provides a graphical way to interact with our data and can be even used as a Content Management System (CMS) similar to Wordpress. Of course we also write tests for all our code, store a remote copy on Bitbucket, and deploy to Heroku.

Next up in **Chapters 5-7** is a robust blog application that demonstrates how to perform CRUD (Create-Read-Update-Delete) functionality in Django. We'll find that Django's generic class-based views mean we have to write only a small amount of actual code for this! Then we'll add forms and user authentication (login logout, signup).

Finally we're ready for our final project: a _social messaging_ app similar to Twitter. In **Chapter 8** we start our new project the right way with a custom user model and then implement signup, login, and logout functionality in **Chapter 9**. **Chapter 10** introduces [Bootstrap](https://getbootstrap.com/) for styling and we complete our user registration flow with password change and reset via email in **Chapter 11**. Our core social messaging app and layout is built in **Chapter 12**. In **Chapter 13** we add a \_comments* feature and introduces one-to-many foreign key relationships. Finally in **Chapter 14** we'll explore authorization by restricting parts of the app only to logged-in users.

**The conclusion** provides an overview of the major concepts introduced in the book and a list of recommended resources for further learning.

While you can pick and choose chapters to read, the book's structure is very deliberate. Each app/chapter introduces a new concept and reinforces past teachings. I **highly recommend** reading it in order, even if you're eager to skip ahead. Later chapters won't cover previous material in the same depth as earlier chapters.

By the end of this book you'll have an understanding of how Django works in practice, the ability to build apps on your own, and the background needed to fully take advantage of additional resources for learning intermediate and advanced Django techniques.

## Book layout

There are many code examples in this book, which are denoted as follows:

{title="Command Line",lang="text"}
~~~~~~~~
# This is Python code
print(Hello, World)
~~~~~~~~

For brevity we will use dots `...` to denote existing code that remains unchanged, for example in a function we are updating.

{title="Code",lang="python"}
~~~~~~~~
```python
def make_my_website:
    ...
    print("All done!")
~~~~~~~~

We will also use the command line console frequently (starting in **Chapter 1: Initial Setup** to execute commands, which take the form of a `$` prefix in traditional Unix style.

{title="Command Line",lang="text"}
~~~~~~~~
$ echo "hello, world"
~~~~~~~~

The result of this particular command is the next line will state:

{title="Command Line",lang="text"}
~~~~~~~~
"hello, world"
~~~~~~~~

We will typically combine a command and its output. The command will always be prefaced by a `$` and the output will not. For example, the command and result above will be represented as follows:

{title="Command Line",lang="text"}
~~~~~~~~
$ echo "hello, world"
hello, world
~~~~~~~~

Complete source code for all examples can be found in the [official Github repository](https://github.com/wsvincent/djangoforbeginners).

## Conclusion

Django is an excellent choice for any developer who wants to build modern, robust web applications with a minimal amount of code. In the next chapter we'll learn how to configure any computer for Django development.
