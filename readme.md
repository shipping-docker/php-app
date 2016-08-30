# Containerized PHP Application

<a href="https://shippingdocker.com" title="learn how to use docker in dev and production">![Shipping Docker video series](https://cloud.githubusercontent.com/assets/467411/18037593/12321512-6d4e-11e6-8514-e8454f4fd286.jpg)</a>

## What is this?

This is an example of how you can quickly get up and running with Docker to develop a PHP application. This is a companion to the 🐳 [Shipping Docker](https://shippingdocker.com/) video series.


## How to use

### 1. Get the files and spin up containers

```bash
# Get shipping-docker files
git clone git@github.com:shipping-docker/php-app.git
cd php-app

# Start the app, run containers
#   in the background
# This will download and build the images
#   the first time you run this
docker-compose up -d
```

At this point, we've created containers and have them up and running. However, we didn't create a Laravel application to serve yet. We waited because we wanted a PHP image to get created so we can re-use it and run `composer` commands.

### 2. Create a new Laravel application

```bash
# From directory "php-app"
# Create a Laravel application
rm -r application

docker run -it --rm \
    -v $(pwd):/opt \
    -w /opt \
    --network=phpapp_appnet \
    phpapp_php \
    composer create-project laravel/laravel application

docker run -it --rm \
    -v $(pwd)/application:/opt \
    -w /opt \
    --network=phpapp_appnet \
    phpapp_php \
    composer require predis/predis

# Restart required to ensure
# app files shares correctly
docker-compose restart
```

Edit the `application/.env` file to have correct settings for our containers. Adjust the following as necessary:

```
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=homestead
DB_USERNAME=homestead
DB_PASSWORD=secret

BROADCAST_DRIVER=log
CACHE_DRIVER=redis
SESSION_DRIVER=redis
QUEUE_DRIVER=sync

REDIS_HOST=redis
REDIS_PASSWORD=null
REDIS_PORT=6379
```

> If you already have an application, you can move it to the `application` directory here. Else, you can adjust the shared volume file paths within the `docker-compose.yml` file.
> 
> If you edit the `docker-compose.yml` file, run `docker-compose down; docker-compose up -d` to suck in the new Volume settings.

### 3. (Optionally) Add Auth Scaffolding:

If you'd like, we can add Laravel's Auth scaffolding as well. To do that, we need to run some Artisan commands:

```bash
# Scaffold authentication views/routes
docker run -it --rm \
    -v $(pwd)/application:/opt \
    -w /opt \
    --network=phpapp_appnet \
    phpapp_php \
    php artisan make:auth

# Run migrations for auth scaffolding
docker run -it --rm \
    -v $(pwd)/application:/opt \
    -w /opt \
    --network=phpapp_appnet \
    phpapp_php \
    php artisan migrate
```

Now we can start using our application! Head to `http://localhost/register` to see your Laravel application with auth scaffolding in place.

![image](https://cloud.githubusercontent.com/assets/467411/18038743/6ac84008-6d61-11e6-8aa6-30a776b59aaa.png)
