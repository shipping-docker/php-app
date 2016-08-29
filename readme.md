# Containerized PHP Application

<a href="https://shippingdocker.com" title="learn how to use docker in dev and production">![Shipping Docker video series](https://cloud.githubusercontent.com/assets/467411/18037593/12321512-6d4e-11e6-8514-e8454f4fd286.jpg)</a>

## What is this?

This is an example of how you can quicklyget up and running with Docker to develop a PHP application. This is a companion to the 🐳 [Shipping Docker](https://shippingdocker.com/) video series.


## How to use

```bash
git clone git@github.com:shipping-docker/php-app.git
cd php-app

# Start the app, run containers
# in the background
docker-compose up -d

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

Then start using it! Head to `http://localhost/register` to see your Laravel application with auth scaffolding in place.

![image](https://cloud.githubusercontent.com/assets/467411/18038743/6ac84008-6d61-11e6-8aa6-30a776b59aaa.png)
