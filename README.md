### URL shortener

#### Using docker-compose file for non-production

Test trigger

test build auto jenkins

Another URL shoterner app.

This is an example of using [Sanic](https://github.com/channelcat/sanic) with [asyncpg](https://github.com/MagicStack/asyncpg) - a fast PostgreSQL client lib for asyncio.


If you want to try, use `docker-compose`:

```
$ docker-compose -f docker-compose-non-pro.yaml build
$ docker-compose up -d
```

#### Using docker-compose for Production

Build images:

```
docker build -t namptit307/sanic_url ./app
docker build -t namptit307/sanic_db ./sql/
docker build -t namptit307/sanic_nginx -f nginx.Dockerfile .
```

Start application via docker-compose

```
docker-compose -f docker-compose-pro.yaml up -d
```


Screenshot of the app

![Screenshot](doc/images/screenshot.png?raw=true)


### Development

Each component of this app has its own Dockerfile:

- PostgreSQL: `sql/Dockerfile`
- Nginx: `nginx.Dockerfile`
- App: `app/Dockerfile`


#### PostgreSQL

There is a dockerized PostgreSQL in `sql`, build your own PostgreSQL container:

```
$ cd sql
$ docker build -t postgres_local .
$ docker run --rm -it -v /tmp/pg:/var/lib/postgres/data -p 5432:5432 postgres_local
```

#### App

Only support Python 3 (tested on Python 3.6.4)

This app uses [pipenv](https://github.com/pypa/pipenv) to manage virtualenv. Go to `app` and install dependencies, then you can start working with this:

```
$ cd app
$ pipenv install
$ pipenv shell # run app inside virtualenv

```

Run the app

```
$ POSTGRES_USER=url POSTGRES_PASSWORD=secret DEBUG=True python app.py
```


