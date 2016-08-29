# Containerized PHP Application

<a href="https://shippingdocker.com" title="learn how to use docker in dev and production">![Shipping Docker video series](https://cloud.githubusercontent.com/assets/467411/18037593/12321512-6d4e-11e6-8514-e8454f4fd286.jpg)</a>

## What is this?

This is an example of how you can quicklyget up and running with Docker to develop a PHP application. This is a companion to the 🐳 [Shipping Docker](https://shippingdocker.com/) video series.


## How to use

```bash
# Get shipping-docker files
git clone git@github.com:shipping-docker/php-app.git
cd php-app

# Start the app, run containers
# in the background
docker-compose up -d
```

At this point, we've created containers and have them up and running. However, we didn't create a Laravel application to serve yet. We waited because we wanted a PHP image to get created so we can re-use it and run `composer` commands.

Let's see that in action:

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

Now we can run some `artisan` commands to get Authentication scaffolded (or really, whatever you want):

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