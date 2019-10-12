# Linux-Server-Configuration

This project is the last project in Udacity full stack program

## Getting Started

* The IP address
```
188.166.169.139
```
* change ssh port
```
2200
```
* The complete URL to your hosted web application.
```
http://188.166.169.139.xip.io
```

* To login as Grader
```
ssh -p 2200 grader@188.166.169.139 -i your_private_key
```

## Summary of configurations made in server

* Update system packages
```
sudo apt-get update
sudo apt-get upgrade
```
* Adding new user and give sudo access 
```
adduser abdullah
usermod -aG sudo abdullah
```
* Ssh key generate in local computer
```
ssh-keygen
```
* Install SSH keys on server
```
ssh-copy-id abdullah@188.166.169.139
```
* Add ssh configration to /etc/ssh/sshd_config
```
Port 2200
PermitRootLogin no
```
* Restart ssh for changes to take effect
```
sudo service ssh restart
```
* Enable firewall and configure
```
sudo ufw default deny incoming
sudo ufw default allow outgoing

sudo ufw allow 2200
sudo ufw allow 80
sudo ufw allow 123

sudo ufw enable
sudo ufw status
```
* Adding grader user and give sudo access 
```
adduser grader
usermod -aG sudo grader
```
* Ssh key generate in local computer in folder:
```
ssh-copy-id -p 2200 -i ./id_rsa.pub grader@188.166.169.139
```
* Configure the local timezone to UTC
```
sudo timedatectl set-timezone UTC
timedatectl status
```
* Disable password authentication in /etc/ssh/sshd_config
```
PasswordAuthentication no
```

## A summary of software you installed, configuration changes made and list of any third-party resources

* Install apache server
```
sudo apt-get install apache2
service apache2 status
```
* Install python 2.7
```
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt-get update
sudo apt-get install python2.7
```
* Install wsgi
```
sudo apt-get install libapache2-mod-wsgi
```
* Point to WSGI application in /etc/apache2/sites-enabled/000-default.conf
```
WSGIScriptAlias / /var/www/html/project.wsgi
```
* Clone Catalog-App to var/www/html
```
sudo git clone https://github.com/IQAPS/Catalog-App.git
```
* Add project.wsgi with in the path /var/www/html/myapp.wsgi
```
import sys

sys.path.insert(0, '/var/www/html/Catalog-App')

from project import app as application
```
* Install PostgreSQL
```
sudo apt-get install postgresql
```
* Login to PostgreSQL
```
sudo -u postgres psql
```
* Create user catalog
```
CREATE USER catalog WITH PASSWORD 'catalog';
```
* Create the database
```
CREATE DATABASE catalog OWNER catalog;
```
* Update the following files in /var/www/html/Catalog-App/ (database_setup.py and add_dummy.py)
```
engine = create_engine("postgres://catalog:catalog@localhost/catalog")
```
* Install python modules 
```
pip install flask
pip install sqlalchemy
pip install flask_sqlalchemy
pip install psycopg2
pip install oauth2client
pip install requests
```
* Create the tables and adding the dummy data
```
python database_setup.py
python add_dummy.py
```
* Update the fowlloing in /var/www/html/Catalog-App/project.py
```
app.secret_key = "super secret key"
PATH = '/var/www/html/Catalog-App/'
CLIENT_ID = json.loads(open(PATH + "client_secrets.json", "r").read())["web"]["client_id"]
engine = create_engine("postgres://catalog:catalog@localhost/catalog")
if __name__ == "__main__":
    app.debug = True
    app.run(host="0.0.0.0", port=80)
```
* Restart ssh for changes to take effect
```
sudo apache2ctl restart
```

