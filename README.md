# Linux Server Configuration
Secure an Ubuntu instance in Amazon Lightsail, and deploy an appication to the web (in this case, my [Item Catalog](https://github.com/lewisisgood/item-catalog) app).

## Accessing the App
The IP address and SSH port so your server can be accessed by the reviewer:
* IP 13.58.140.170
* Port 2200

The complete URL to your hosted web application:
* http://13.58.140.170.xip.io

## Software Installed
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

## Configuration Changes to Ubuntu
Update all currently installed packages:
```
sudo apt-get update        # Fetches the list of available updates
sudo apt-get upgrade       # Strictly upgrades the current packages
```

Configure SSH port and setup firewall:
```
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh
sudo ufw allow 2200/tcp
sudo ufw allow www
sudo ufw allow ntp
sudo ufw enable
```

Change these config within the sshd_configs file and restart sshd:

```
sudo nano /etc/ssh/sshd_config
// Change the Port at the top from 20 to 2200
// Set the PermitRootLogin property from prohibit-password to no
// Set the PasswordAuthentication property to no
sudo systmctl restart sshd
sudo systemctl status sshd
```

Change the timezone to UTC:
```
sudo timedatectl set-timezone Etc/UTC
```

Install and configure PostgreSQL:
```
sudo apt-get install postgresql postgresql-contrib
sudo su postgres
psql
CREATE DATABASE catalog;
CREATE USER catalog;
GRANT ALL PRIVILEGES ON DATABASE catalog TO catalog;
```

Clone the item catalog project:
```
mkdir /var/www/catalog/
git clone https://github.com/lewisisgood/item-catalog.git
cd item-catalog
```

Replace the "engine" line to use Postgres instead of SQLite in project.py, database_setup.py, lotsofitems.py with the following line:
```
engine = create_engine('postgresql://catalog@localhost:5432/catalog')
```

Install and configure Apache to serve a Python mod_wsgi application:
```
sudo apt-get install apache2
sudo apt-get install libapache2-mod-wsgi
Create a new configuration file, by editing a copy the default configuration file:
cd /etc/apache2/sites-available
sudo cp 000-default.conf 100-itemcatalog.conf
sudo nano 100-itemcatalog.conf
```

Update DocumentRoot property to /var/www/catalog/item-catalog/static by adding these lines before </VirtualHost> closing tag:
```
WSGIScriptAlias / /var/www/catalog/item-catalog/static/itemcatalog.wsgi
	<Directory /var/www/catalog/item-catalog/static>
	Order deny,allow
	Allow from all
</Directory>
```
```
ln -s ../sites-available/100-itemcatalog.conf 100-itemcatalog.conf        
sudo apache2ctl restart
```

Create a WSGI file, containing the code mod_wsgi executes on startup to get the application object:
```
cd /var/www/catalog/item-catalog/static
touch itemcatalog.wsgi
sudo nano itemcatalog.wsgi
```
Paste these lines into file:
```
import sys
sys.path.insert(0, '/var/www/catalog/item-catalog')
from project import app as application
```

The Ubuntu instance is now ready to serve the item catalog app.


## Acknowledgements
* [Digital Ocean Tutorials](https://www.digitalocean.com/community/tutorials/)
* [Flask Documentation](http://flask.pocoo.org/docs/1.0/deploying/mod_wsgi/)

## Author
* **Lewis King** - [Github](https://github.com/lewisisgood)
