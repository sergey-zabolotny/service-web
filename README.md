# Apache Docker images for Docksal

This image(s) is part of the [Docksal](http://docksal.io) image library.

## Versions

- apache2.2 (based on http:2.2-alpine)
- apache2.4 (based on http:2.4-alpine)

## Features

- SSL enabled (self-signed cer)
- HTTP Basic Authentication
- PHP handling via FastCGI (`mod_proxy_fcgi`) (both 2.2 and 2.4)

## FastCGI server endpoint

These images are set up to work with a FastCGI server and will not start without one.  
The FastCGI endpoint can be set via `APACHE_FCGI_HOST_PORT` environment variable (defaults to `cli:9000`).

## HTTP Basic Authentication

Use `APACHE_BASIC_AUTH_USER` and `APACHE_BASIC_AUTH_PASS` environment variables to set username and password.

Example with Docker Compose

```yaml
  ...
  environment:
    - APACHE_BASIC_AUTH_USER=user
    - APACHE_BASIC_AUTH_PASS=password
  ...
```
