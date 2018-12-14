# Docker setup for PHP (php 7.3 + mysql + composer etc.)
> This is my Docker PHP development environment. I use it mostly for developing Symfony 4+ apps but sometimes also for vanilla and Laravel 5 projects. Updated for php 7.3.

## Table of contents
* [Features](#Features)
* [Setup](#Setup)
* [Run](#Run)
* [Example setup & running for Symfony 4 project ](#example)
* [How connect to database via PHP Storm?](#how)
* [Issues](#Issues)
* [Status](#Status)
* [Contact](#contact)

## Features
* PHP 7.3
* Mysql 5.7
* Composer
* php.ini file under your control
* environment variables in config file (database access data)
* Local storage for database (data does not disappear after shutdown containers)

The most used PHP extensions come from official php docker image but it has some extra extensions like:
* pdo
* zip
* apcu

## Setup

1. Configure database connection of your application

File: ./docker/env/docker.env 

`DB_CONNECTION=mysql` // default service name in docker setup - 'mysql'

`DB_HOST=mysql` // default service name in docker setup - 'mysql'

`DB_PORT=3306` // port, default is 3306

`DB_DATABASE=database_name` // database name

`DB_USERNAME=someuser` // database username

`DB_PASSWORD=123qwe` // database password

2. Configure database data for mysql service

File: docker-compose.yml

```sh
      MYSQL_DATABASE: database_name
      MYSQL_ROOT_PASSWORD: 123qwe
      MYSQL_USER: someuser
      MYSQL_PASSWORD: 123qwe
```
3. Define volume (local path to storage data)

File: docker-compose.yml

```sh
    volumes:
    - mysql:/var/dockervolumes
```

4. Setting your application

Docker creates couple of env's with database acces data. You must enter those envs in your config apps.

Example (Symfony 4): .env file will look like this:

```sh
DATABASE_URL=$DB_CONNECTION://$DB_USERNAME:$DB_PASSWORD@$DB_HOST:$DB_PORT/$DB_DATABASE
```

> Hostname ($DB_CONNECTION) = docker service name (mysql)

## Run

Build:

```sh
$ docker-compose build
```

Run:

```sh
$ docker-compose up
```

Or build & run

```sh
docker-compose up --build
```

Go to http://localhost:8000/ and have fun!

## <a name="example"></a> Example setup & running for Symfony 4 project
1. Copy files to your root Symfony 4 application directory
2. Configure docker files (see 'setup' section)
3. In /.env file change default doctrine DATABASE_URL env to:

```sh
DATABASE_URL=$DB_CONNECTION://$DB_USERNAME:$DB_PASSWORD@$DB_HOST:$DB_PORT/$DB_DATABASE
```
4. Build & run docker

```sh
docker-compose up --build
```
5. Go to http://localhost:8000/public/ (you also need apache bundle to run s4 app without build-in server feature).


## <a name="how"></a> How connect to database in docker via PHP Storm?
1. Click on tab 'database'
2. Click on '+' button and select Data source -> Mysql
3. Fill in the fields:
- Host: localhost
- Port: 13306 (if you have not changed the default one)
- Database: database name (from .docker/env/docker.env file)
- User: user name (from .docker/env/docker.env file)
- Password: password (from .docker/env/docker.env file)
- URL: jdbc:mysql://localhost:13306/your_database_name
 

## Issues
* When you using maker bundle in symfony project in the container, it creates at first time new catalogs (like Entity, Repositories etc.) with root owner permissions in your local app volume. So if you are not root user on your local machine - you can't edit files in catalogs. It can be easy fixed. Just change permissions:

```bash
sudo chown -R $USER Entity Repositories etc...
```

## Status
Project is: in progress

## Contact
Created by [@sebastianSkurnog](https://www.skurnog.com/) - feel free to contact me!
