# Building Images

This project uses two custom Docker images, hosted at [https://hub.docker.com/r/shippingdocker/](https://hub.docker.com/r/shippingdocker/).

These images are built here ahead of time using the `build` bash script found in this directory.

## Usage

The TL;DR usage of the `build` script is:

```bash
# Log into the shippingdocker Docker Hub user
$ docker login

# Build a new version
$ ./build 1.1
```

## Explanation

This build script pushes up to the `shippingdocker` repository, so only I can actually push to it.

However it's useful to see how you might use it to build your own images (simply replace the `shippingdocker` namespace with your own).

Let's see an example, with comments to explain what we're doing:

```bash
# Log into your Docker Hub account
$ docker login

# Find our build files
$ cd php-app/build

# Build an Nginx image with your own namespace
# We tag is 1.0 and latest - you can tag it any version
#  in addition to tagging it the "latest"
$ docker build -t your-namespace/nginx:1.0 -t your-namespace/nginx:latest

# Push both images up
$ docker push your-namespace/nginx:1.0
$ docker push your-namespace/nginx:latest
```


