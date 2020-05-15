Update Wifi Country
Update Locale
Update Timezone
Update Keyboard Layout
sudo apt install libtiff5 libopenjp2-7-dev python3-venv git python3-dev libpq-dev postgresql postgresql-contrib nginx curl

# Database Setup
sudo -u postgres psql

CREATE DATABASE database_name;
CREATE USER username WITH PASSWORD 'password';
ALTER ROLE username SET client_encoding TO 'utf8';
ALTER ROLE username SET default_transaction_isolation TO 'read committed';
ALTER ROLE username SET timezone TO 'UTC';
GRANT ALL PRIVILEGES ON DATABASE database_name TO username;
\q


# django setup
python3 -m venv .env
source .env/bin/activate
pip install django gunicorn psycopg2-binary

django-admin startproject project_name

settings.py:
	DATABASES = {
	    'default': {
	        'ENGINE': 'django.db.backends.postgresql_psycopg2',
	        'NAME': 'myproject',
	        'USER': 'myprojectuser',
	        'PASSWORD': 'password',
	        'HOST': 'localhost',
	        'PORT': '',
	    }
	}

# test gunicorn
gunicorn --bind 0.0.0.0:8000 myproject.wsgi

# setup gunicorn
sudo nano /etc/systemd/service/gunicorn.socket
[Unit]
Description=gunicorn socket

[Socket]
ListenStream=/run/gunicorn.sock

[Install]
WantedBy=sockets.target

sudo nano /etc/systemd/system/gunicorn.service
[Unit]
Description=gunicorn daemon
Requires=gunicorn.socket
After=network.target

[Service]
User=pi
Group=www-data
WorkingDirectory=/home/pi/www
ExecStart=/home/pi/www/.env/bin/gunicorn \
		--access-logfile - \
		--workers 3 \
		--bind unix:/run/gunicorn.sock \
		myproject.wsgi:application

[Install]
WantedBy=multi-user.target

sudo systemctl start gunicorn.socket
sudo systemctl enable gunicorn.socket

# Troubleshooting gunicorn
file /run/gunicorn.socket -> check for socket file
sudo systemctl status gunicorn.socket
sudo journalctl -u gunicorn.socket

# Testing socket activation
sudo systemctl status gunicorn -> Should show inactive

curl --unix-socket /run/gunicorn.sock localhost

sudo systemctl status gunicorn -> Should show active

sudo journalctl -u gunicorn

# reload
sudo systemctl daemon-reload

# configure nginx
