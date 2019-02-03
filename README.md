## Linux-Server-Configuration
By Prasanna Raj Mallipudi

### Server Details
Server IP Address 3.90.89.13

Hosted site Url http://3.90.89.13.xip.io/

#### Private Key for the `grader` user

```
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAv4FHuIMHLkunok/vJ23kJ6uiOr8ktKcwjvR4nfCG6hTMoJnurHDCEUB/isSL
+Vq40JNfcYA3he+3qsHxsXfn9Zpe3zU58x7LtUZ7XVj/KPI2+UxdOZAZ2vEkJbS54T05fP26WBuV
T0wJPb5/aAHucOjxyinkfSTOqpud0JG4tHNlzXeBcQmuAmXuNllhZIxXgITGOgP8WtIQFcTlajq9
UrYojnUGECRtJ3uM36zvXnOzz+tt3twT7DGe94t4MA/PPpC3F8ijow/tiHyRWs169ON0rYAb3BOk
K7n6UwzeIbU18X9QunB/fQHrB9ZJsmN64pGsjzrLEU0CGtbXoz2zWQIDAQABAoIBAAUU3h1URL2C
vmj9LABk1xgCYuSiK8wrX4t7izPNrRbLBH8CdqXWvB+PEZNSEdDFCoBmWLL/Tvnb2YjLaqll4B7R
QNE1bVs5JvihbSaV9PdWLgVBdHvRig4YNfhWKlSCtmLNrMMnwDeBcgFUa7cLzhLq63eM0wZLErzO
MkrVgwwus/QVWXgqFnAhfo+3k6vJi/THyl2BimB4p3l2oKJmmxhCUAraNOf/JrLXJcqNJM3xDsl5
IU+HnTaKo5SkhCsaOVoAoF2uTng++W672QMu053OWEinKrk/OWByRxt7VG6UH7lSIWWNGpAIMZo+
4/C6R7WJk1A8p4Om/ALA4dJ5nAECgYEA5MRmyG8qPEMhTmgz0P42tX4yBlwxApONhyXss1Xf6ARk
BuJ7kanogVjimdAD6T5wNLu1MLEYkkAxS7QNF9SwpSS+jUerI53FLOFMzfcwrdFKlNHmUqwIKXn4
V7DLL3xg1NQ3sLDma8/VpzIaDVu+esoP4gnNVdLbYKQBW7GeR9kCgYEA1k1Tn1EjOE1736npl486
JGPQcj4dL/NHfLN5FTjT6jrUOMJ2B9/4GghTqtMqhGtn7yz0nVuOshTWWiQfma2a3YSNXI42gPhF
pcGkr6iH7Uj+lYQ8/Mw+Uo3jQ92ErQo8C4TwE39huWqFrQavG0WuzJlGVvz3ucyTifLy13TfF4EC
gYBqw+acQJVbjpDqwOL99MafqYe7RCdzZl2051Qy1i8rX3E3bVfyQXPLeio3UseggY1obvo/AUfQ
abkX9a3uo0rVp/JuiTYr5yPwsC1iKq1DUdwW3Gc7yiH9CXmHo3EQdmHSskBTbBv90P+LimxyM5EQ
SWFkQuLb3Q6ZabFgk+Kk8QKBgGTxa5BB0cs1hEHocSV4y7RUZ91IMMtjVqsAhupeKEPOjiRmRN6U
uvLYNUDj0sUpKn3YuShd9isf8Y/oMBJdWfM5+rQ8MKQzTesz+XTcd5pnpbOb5HZXZovQVdqjv1ka
1wm+Bq/Icqm56VmFKno4KowpLp7O7YlRObXjAExIusUBAoGAP+Sq19AZeMWcQ4I9t6YcxDKm96zL
zBrfk4WOoui9f6r53ysdcM37/XYEjacB9HzexnnlishnW5qR0SG5pVDsSZEuEJ/cOX4KgvmP5iD/
CfC4WK5lByYiZi77vVvmZy8VX1c7Y3VmgtzyR+g/yOsU0HV7km8lm8LjQCzdiuKNj4g=
-----END RSA PRIVATE KEY-----

```
#### grader `password`
grader

### Steps

#### 1: New Ubuntu Linux server instance on Amazon EC2

- Login to aws.amazon.com and login to default user (ubuntu)
- Choose EC2 and Launch Instance.
- Check for instance `IPv4 public IP - 3.90.89.13`
  we can download a `.pem File` and connect with following command
- `ssh -i linux_31.pem ubuntu@3.90.89.13`
- `22` is Port by Default,Later we need to Change `2200`.

#### 2: Update and upgrade installed packages
  - sudo apt-get update
  - sudo apt-get upgrade
  - sudo apt-get install unattended-upgrades
  - sudo apt-get dist-upgrade

#### 3. Change SSH port from 22 to 2200

 - Edit File: `sudo vi /etc/ssh/sshd_config`
 - Change Port number 22 to 2200
 - Save and exit using esc and confirm with :wq
 - Restart: `sudo service ssh restart`
 - Change inbound rules in Amazon EC2
 - Type : Custom TCP Rule as `2200`
 - To check port 2200: Working or Not
 - `ssh -i linux_31.pem -p 2200 ubuntu@3.90.89.13`
 
#### 4: Configure Firewall (UFW)

 - Configure the default firewall allow incoming connections for `SSH (port 2200)` , `HTTP (port 80)` and `NTP (port 123)`
 
   `sudo ufw status`                
   `sudo ufw default deny incoming`   
   `sudo ufw default allow outgoing`  
   `sudo ufw allow 2200/tcp`          
   `sudo ufw allow www`               
   `sudo ufw allow 123/udp`           
   `sudo ufw deny 22`
  
  - `sudo ufw enable`
  - After Proceed Option Y/N - Y
  - `sudo ufw status`
```  
Status: active

To                         Action      From
--                         ------      ----
2200/tcp                   ALLOW       Anywhere                  
80/tcp                     ALLOW       Anywhere                  
123/udp                    ALLOW       Anywhere                  
22                         DENY        Anywhere                  
2200/tcp (v6)              ALLOW       Anywhere (v6)             
80/tcp (v6)                ALLOW       Anywhere (v6)             
123/udp (v6)               ALLOW       Anywhere (v6)             
22 (v6)                    DENY        Anywhere (v6)
```

#### 5: Create a ``New User`` account named `grader`

 - While logged in as ubuntu, add user: sudo adduser grader
 - Enter a password
 - Edits the sudoers file: `sudo visudo`
   ``root    ALL=(ALL:ALL) ALL``
 
 - add a new line to give sudo privileges to grader user
   ```
   root    ALL=(ALL:ALL) ALL
   grader  ALL=(ALL:ALL) ALL
   ```
 - Save and exit using ctrl+x and confirm with Y.
 
 - Run su - grader, enter the password
 
#### 6: Create an SSH key pair for grader

- create .ssh folder by mkdir /home/grader/.ssh
- su grader
- RUN this command `sudo cp /home/ubuntu/.ssh/authorized_keys /home/grader/.ssh/authorized_keys`
- change Ownership ``chown grader.grader /home/grader/.ssh``
- `sudo su` and goto root directorie after next command
- sudo Group add `usermod -aG sudo grader`
- change permissions for .ssh folder ``chmod 0700 /home/grader/.ssh/``, for authorized_keys ``chmod 644 authorized_keys``
- Check in ``vi /etc/ssh/sshd_config`` file if `PermitRootLogin is set to No`
- Restart SSH: `sudo service ssh restart`
- grader account Working or Not by RUNNING this command : ``ssh -i linux_31.pem -p 2200 grader@3.90.89.13``
- Configure the local timezone to UTC Logged On grader Account
- TIME ZONE: ``sudo dpkg-reconfigure tzdata``. Choose time zone UTC

####  Deploying the project Steps
- logged in as grader, install Apache: `sudo apt-get install apache2`
- Enter `public IP` of the `Amazon EC2` instance into browser(Installation process sucess or not)
  --- If sucess DIsplay the APACHE PAGE
- My project is built with Python 3. So, I need to install the Python 3 mod_wsgi package:
   `sudo apt-get install libapache2-mod-wsgi-py3`
- Enable mod_wsgi using: `sudo a2enmod wsgi`

#### Setting up your Google Oauth2
  Login to your developer console and select your project and edit OAuth details(Configuration) as following
```
Javascript origin http://ip.xip.io
                  
redirect URI

http://ip.xip.io/login

http://ip.xip.io/gconnect

http://ip.xip.io/callback

xip.io is a free DNS which will be the same as using IP address
```
- After download the client_secrets.json file.If you usese the data copy and paste

#### Install and configure PostgreSQL
```
- sudo apt-get install libpq-dev python-dev
- sudo apt-get install postgresql postgresql-contrib
- sudo su - postgres
- psql
- CREATE USER catalog WITH PASSWORD 'catalog';
- ALTER USER catalog CREATEDB;
- CREATE DATABASE catalog WITH OWNER catalog;
- \c catalog
- REVOKE ALL ON SCHEMA public FROM public;
- GRANT ALL ON SCHEMA public TO catalog;
- \q
- exit
- Switch back to the grader user: exit
```
- Logged in as `grader`, install git: `sudo apt-get install git`

- Clone and setup the Item Catalog project from the GitHub repository
```
- While logged in as grader,
- From the /var/www directory, Clone the catalog project:
- sudo git clone `https://github.com/username/catalog.git`
- Change the ownership of the catalog directory to grader using: sudo chown -R grader:grader catalog/.
- Change to the `/var/www/catalog/catalog` directory.
- Rename the mainpage.py file to __init__.py using: mv mainpage.py __init__.py.
- We need to change `sqlite` to `postgresql` create_engine in __init__.py,database_setup.py and sample_database.py
- Reffered from stack overflow
```
``
 #engine = create_engine("sqlite:///catalog.db")
 engine = create_engine('postgresql://catalog:catalog@localhost/catalog')
``
#### Configure and Enable a New Virtual Host

sudo nano /etc/apache2/sites-available/FlaskApp.conf

```
<VirtualHost *:80>
    ServerName 54.161.86.157.xip.io
    ServerAlias ec2-54-161-86-157.compute-1.amazonaws.com
    ServerAdmin ubuntu@54.210.140.47
    WSGIDaemonProcess catalog python-path=/var/www/catalog:/var/www/catalog/catalog/venv3/lib/python3.6/site-packages
    WSGIProcessGroup catalog
    WSGIScriptAlias / /var/www/catalog/catalog.wsgi
    <Directory /var/www/catalog/catalog/>
        Order allow,deny
        Allow from all
    </Directory>
    Alias /static /var/www/catalog/catalog/static
    <Directory /var/www/catalog/catalog/static/>
        Order allow,deny
        Allow from all
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    LogLevel warn
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

```
- Enable the virtual host `sudo a2ensite catalog`
- Enabling site `catalog`. To activate the new configuration
- You need to run: `service apache2 reload`
#### Set up the Flask application
- Create `/var/www/catalog/catalog.wsgi` file add the following lines:
```
  import sys
  import logging
  logging.basicConfig(stream=sys.stderr)
  sys.path.insert(0, "/var/www/catalog/")
  from catalog import app as application
  application.secret_key = 'supersecretkey'
```
- Restart Apache: sudo service apache2 restart.

- From the `/var/www/catalog/catalog/` directory
- Activate the virtual environment: `. venv3/bin/activate`

`Run`: `python db_setup.py`

- Deactivate the virtual environment: `deactivate`

#### Disable the default Apache site
- Disable the default Apache site: `sudo a2dissite 000-default.conf`

The following prompt will be returned:

`Site 000-default disabled`
- To activate the new configuration, you need to run:
  `service apache2 reload`
- Reload Apache: `sudo service apache2 reload`


#### Final Step
- Security Updates and package updates Try this commands
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
```

#### Launch the Web Application
 
 Open your browser to : (http://3.90.89.13.xip.io)
 Open your browser to : (http://ec2-3-90-89-13.compute-1.amazonaws.com)
 
`Special Thanks`::-  [SkBadulla] and  [SriramTalasila]  for a very helpful READMEs in Linux Server Configuration...... :) 
`Very Special Thanku`::Online Resources(Udacity,Stack Overflow,Google,Youtube,..........)
 
