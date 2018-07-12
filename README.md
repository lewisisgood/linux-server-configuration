# Linux Server Configuration
Information for Linux Server Configuration project for Udacity.

Secure an Ubuntu instance in Amazon Lightsail, and deploy an appication to the web via Heroku (in this case, the Item Catalog app).

## Accessing the App
The IP address and SSH port so your server can be accessed by the reviewer:
* IP 13.58.140.170
* Port 2200

The complete URL to your hosted web application:
* https://dashboard.heroku.com/apps/item-catalog-app-lew

## App Information
Software installed:
* apache2
* libapache2-mod-wsgi
* postgresql
* postgresql-contrib
* git
* finger

Python packages installed:
* python-pip
* sqlalchemy
* postgresql 
* flask
* requests
* python-psycopg2

Configuration changes made:
* Changes to mod_wsgi to allow Apache and Flask to work nicely, outlined [here](http://flask.pocoo.org/docs/1.0/deploying/mod_wsgi/)

A list of any third-party resources you made use of to complete this project:
* [Digital Ocean Tutorials](https://www.digitalocean.com/community/tutorials/)
* [Flask Documentation](http://flask.pocoo.org/docs/1.0/deploying/mod_wsgi/)

## Authors
* **Lewis King** - [Github](https://github.com/lewisisgood)
