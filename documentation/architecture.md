# Architecture

## Glossary

There are a lot of potentially unfamiliar terminology used in this document so it might be helpful to briefly explain some of them.

**Django**: [Django](https://www.djangoproject.com/) is a Python web application framework that uses traditional [server-side scripting](https://en.wikipedia.org/wiki/Server-side_scripting). The framework follows [Model-view-controller (MVC)](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) design pattern.

**Docker**: [Docker](https://www.docker.com/) is a tool for bundling applications into *images* that contains all the libraries and resources that the application needs including a preferred Linux distribution. [Dockerfile](https://docs.docker.com/engine/reference/builder/) contains the instructions on how to create an image. These images can be used to created instances called [containers](https://www.docker.com/resources/what-container) that can be started, stopped and removed.

**Docker Compose**: [Docker Compose](https://docs.docker.com/compose/) is a tool for making the management of multiple Docker containers easier. The configuration is stored in a [docker-compose.yml](../docker-compose.yml) file.

**PostgreSQL**: [PostgreSQL](https://www.postgresql.org/) is a relational database management system.

## Environment
The full environment is set up using Docker Compose and consists of two Docker containers:
* **web** contains Django web server (nicknamed WebMark2)
* **db** contains PostgreSQL database

## Web server

The web server has been nicknamed WebMark2. Folder `/webmark2` contains project settings and `/qleader` contains the actual application. WebMark2 uses Django REST framework. More about its implementation is in [here](https://github.com/quantum-ohtu/WebMark2/blob/main/documentation/CreationNotes.md)

## Database

The database uses PostgreSQL and the schema is shown in the following diagram. User table has been automatically created by Django and most of the fields are not used. The relevant fields are username and password.

Here is the WebMark2's database diagram.

![WebMark's dbdiagram](https://github.com/quantum-ohtu/WebMark2/blob/main/documentation/dbdiagram.png)


