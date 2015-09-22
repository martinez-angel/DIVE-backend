DIVE Backend
=================================================
The Data Integration and Visualization Engine (DIVE) is a platform for semi-automatically generating web-based, interactive visualizations of structured data sets. Data visualization is a useful method for understanding complex phenomena, communicating information, and informing inquiry. However, available tools for data visualization are difficult to learn and use, require a priori knowledge of what visualizations to create. See [dive.media.mit.edu](http://dive.media.mit.edu) for more information.


Install System Dependencies (Linux / apt)
---------
```bash
$ sudo apt-get install -y postgres git python2.7 python-pip build-essential python-dev python-dev libffi-dev liblapack-dev gfortran rabbitmq-server
$ sudo su postgres
$ createuser -D -P -R -S dive
$ createdb -E utf8 -O dive -T template0 spendb
```

Install System Dependencies (Mac / brew)
---------
Install [Homebrew](http://brew.sh/) if you don't already have it. Then, run the following code:
```
brew install Caskroom/cask/xquartz
brew install cairo
```

Install and get into a virtual environment
---------
1. Installation: See [this fine tutorial](http://simononsoftware.com/virtualenv-tutorial/).
2. Freezing virtual env packages: `pip freeze > requirements.txt`.
3. Starting virtual env: `source venv/bin/activate`.

Install Python Dependencies
---------
Within a virtual environment, install dependencies in `requirements.txt`. But due to a dependency issue in numexpr, we need to install numpy first.
```
pip install numpy
pip install -r requirements.txt
```

Run API
---------
1. Load virtual environment.
2. To run development Flask server, run `python run.py`.
3. To run production Gunicorn server, run `./run.sh`.

Start RabbitMQ
---------
1. Create a RabbitMQ user and virtual host:
```
sudo rabbitmqctl add_user admin password
sudo rabbitmqctl add_vhost dive
sudo rabbitmqctl set_permissions -p dive admin ".*" ".*" ".*"
```
2. Run the server as a background process
`sudo rabbitmq-server -detached`

Database Migrations
--------
Follow [the docs](https://flask-migrate.readthedocs.org/en/latest/). The first time, run the migration script.
```bash
python migrate.py db init
```

Then, review and edit the migration script. Finally, each time models ar echanged, run the following:
```
python migrate.py db migrate
python migrate.py db upgrade
```
