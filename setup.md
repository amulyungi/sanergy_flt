# Odoo Developer Setup

The idea behind this is to get developers setup and get to developing custom Odoo addons as quickly as possible without having to spend a tedious amount of time going through the documentation to find the relevant content.

The steps below should get you setup and start coding in no time.

## OS

Ubuntu 20 lts

# Setup Odoo

Clone the community repository

`git clone git@github.com:odoo/odoo.git`

Checkout the most recent version

`git checkout 14.0`

## Install postgres

`sudo apt install postgresql postgresql-client`

## Setup user

Odoo prevents use of default postgres user so we need to setup the current user as a postgres user

`sudo -u postgres createuser -s $USER`

## Install required libraries

`sudo apt install python3-dev libxml2-dev libxslt1-dev libldap2-dev libsasl2-dev libtiff5-dev libjpeg8-dev libopenjp2-7-dev zlib1g-dev libfreetype6-dev liblcms2-dev libwebp-dev libharfbuzz-dev libfribidi-dev libxcb1-dev libpq-dev`

## Setup virtualenv and install requirements

### Install virtualenvwrapper

`sudo apt install python3-virtualenvwrapper`

Documentation can be found [here](https://virtualenvwrapper.readthedocs.io/en/latest/)

### Setup your environment

`virtualenv odoodev`

`source odoodev/bin/activate`

### Install dependancies

`pip3 install setuptools wheel`

`pip3 install -r requirements.txt`

### Setup config file

`vim .odoorc`

Add lines below

```
{
  [options]

  db_user=odoo

  dbfilter=odoo

  without_demo = False
}
```

The without_demo = False tells Odoo to include demo data when you intizialize the database. Set this to True if you don't need demo data.

Save and exit your editor.

### Add-ons path

Find and edit odoo.conf and add the addons path

`addons_path = /mnt/c/odoo/addons`

### Additional python libraries

Documentation doesn't mention some additional python libraries needed

`pip3 install passlib pypdf2 pdfminer`

### Install wkhtmltopdf

wkhtmltopdf is a library to render HTML into PDF. Odoo uses it to create PDF reports. wkhtmltopdf is not installed through pip and must be installed manually.

```
{
  sudo apt update -y

  wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox_0.12.6-1.focal_amd64.deb

  sudo apt install ./wkhtmltox_0.12.6-1.focal_amd64.deb
}
```

Verify that installation was successful

`wkhtmltopdf --version`

# Starting up

## Switch to virtual environment

`source odoodev/bin/activate`

## Start and initialize db odoo

The -i flag initializes Odoo core database files. You only need to start Odoo like this the first time only.

The -d flag takes the name of the database you have created in this case the db is named odoo.

`python3 odoo-bin -d odoo -i base`

For subsequent starts use

`python3 odoo-bin -d odoo`

## Start with addons path for custom modules

`python3 odoo-bin --addons-path="addons/" -d odoo`

## Default user & pswd

admin/admin

# Developer mode

Activate developer mode under settings module

## Scaffolding modules

`odoo-bin scaffold my_module add-ons/`

This creates a module named my_module under the add_ons directory

The module structure looks something like this

```
  📦my_module
  ┣ 📂controllers
  ┃ ┣ 📜controllers.py
  ┃ ┗ 📜__init__.py
  ┣ 📂demo
  ┃ ┗ 📜demo.xml
  ┣ 📂models
  ┃ ┣ 📜model.py
  ┃ ┗ 📜__init__.py
  ┣ 📂security
  ┃ ┗ 📜ir.model.access.csv
  ┣ 📂views
  ┃ ┣ 📜templates.xml
  ┃ ┗ 📜views.xml
  ┣ 📜__init__.py
  ┗ 📜__manifest__.py
```

In your favorite editor make changes as needed to module which will be found on your odoo installation path under the addons folder.

## Installation & Testing

Update modules list under settings

Install your module.

If the module is not listed under Apps, check the addons path is setup and search while having removed the 'Apps' filter

## Module Upgrade

## Happy coding!
