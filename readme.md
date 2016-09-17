# Containerized PHP Application

<a href="https://shippingdocker.com" title="learn how to use docker in dev and production">![Shipping Docker video series](https://cloud.githubusercontent.com/assets/467411/18037593/12321512-6d4e-11e6-8514-e8454f4fd286.jpg)</a>

## What is this?

This is an example of how you can quickly get up and running with Docker to develop a PHP application. This is a companion to the 🐳 [Shipping Docker](https://shippingdocker.com/) video series.

## Teach me how to use it!

I have a mini-course that uses this example to show you how to get up and running in Docker for development!

**Sign up here to get an email with a link to the mini course to see how this works!**

<a href="http://shippingdocker.com/#signup" title="see how to use Docker in development"><img src="https://cloud.githubusercontent.com/assets/467411/18333423/8ef22c66-7534-11e6-950d-850be40d9af0.png" alt="Shipping Docker mini-course" width="600" height="200" style="width: 600px; height: 200px;" /></a>

## How to use

### 1. Get the files and spin up containers

```bash
# Get shipping-docker files
git clone https://github.com/shipping-docker/php-app.git
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
docker run -it --rm \
    -v $(pwd):/opt \
    -w /opt \
    --network=phpapp_appnet \
    shippingdocker/php \
    composer create-project laravel/laravel application

docker run -it --rm \
    -v $(pwd)/application:/opt \
    -w /opt \
    --network=phpapp_appnet \
    shippingdocker/php \
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

**NOTE**: If you're not running Docker Mac/Windows (which run Docker in a small virtualized layer), you may need to set permissions on the shared directories that Laravel needs to write to. The following will let Laravel write the storage and bootstrap directories:

```bash
# From directory php-app
chmod -R o+rw application/bootstrap application/storage
```

### 3. (Optionally) Add Auth Scaffolding:

If you'd like, we can add Laravel's Auth scaffolding as well. To do that, we need to run some Artisan commands:

```bash
# Scaffold authentication views/routes
docker run -it --rm \
    -v $(pwd)/application:/opt \
    -w /opt \
    --network=phpapp_appnet \
    shippingdocker/php \
    php artisan make:auth

# Run migrations for auth scaffolding
docker run -it --rm \
    -v $(pwd)/application:/opt \
    -w /opt \
    --network=phpapp_appnet \
    shippingdocker/php \
    php artisan migrate
```

Now we can start using our application! Head to `http://localhost/register` to see your Laravel application with auth scaffolding in place.

![image](https://cloud.githubusercontent.com/assets/467411/18038743/6ac84008-6d61-11e6-8aa6-30a776b59aaa.png)
