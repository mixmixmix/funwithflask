# funwithflask: Mixflask
A bit of exercising with python flask (based on http://flask.pocoo.org/docs/0.12/quickstart/)

## Install:

We are using pipenv which seem to be the nicest thing for python in a while...
```
sudo apt install python3-pip python3-dev
pip install pipenv
```
Install virtual environment in the folder (if you do not create `.venv` the development setup will work but uwsgi/nginx setup won't):
```
mkdir .venv
pipenv install
```

## Running the app in development
```
pipenv shell
python mixflask/run.py
```

## Running the app in production
### Installation
Make sure you have nginx, and python3 tools installed. The following instructions are for an debian-like system

Clone the files into a convenient location:
```
mkdir /www
sudo chown user www
git clone https://github.com/mixmixmix/funwithflask.git
```

Create place for uwsgi logs:
```
sudo mkdir /var/log/uwsgi
```
Make sure correct ownership is setup if running from a user account:
```
sudo chown -R user:user /var/log/uwsgi
```
(note: I assume that you clone this repo in `/www/funwithflask` as the services (`/setup/mixflask.service`) requires absolute paths.)

One change you might need to do is to change server_name in nginx config file `/setup/mixflask`. Then following:

```
sudo cp setup/mixflask.service /etc/systemd/system/
sudo systemctl start mixflask.service
sudo systemctl enable mixflask.service
sudo systemctl status mixflask.service
sudo cp setup/mixflask /etc/nginx/sites-available/mixflask
sudo rm /etc/nginx/sites-enabled/mixflask
sudo ln -s /etc/nginx/sites-available/mixflask /etc/nginx/sites-enabled/
sudo systemctl restart nginx.service
```

To apply changes in flask:
```
sudo systemctl restart mixflask.service
```
### Debugging

However to get logs of the system process (if it fails before starting uwsgi for instance) run
`journalctl -u mixflask.service` and if it fails later in nginx `sudo cat /var/log/nginx/error.log`)

## Tutorials:
- Basic flask tutorial: http://flask.pocoo.org/docs/0.12/quickstart/
- A bit more complex flaks tutorial: http://flask.pocoo.org/docs/0.12/tutorial/
- Jinja2 documentation (fairly accessible): http://jinja.pocoo.org/docs/2.9/
- Another Flask and jinja tutorial: https://code.tutsplus.com/tutorials/templating-with-jinja2-in-flask-essentials--cms-25571

- The greatests tutorial on scientific web-app with flask using MVC: http://hplgit.github.io/web4sciapps/doc/pub/web4sa_flask.html
