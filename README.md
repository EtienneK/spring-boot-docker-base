# Spring Boot Docker Base Image

[![](https://img.shields.io/docker/stars/etiennek/spring-boot.svg)](https://hub.docker.com/r/etiennek/spring-boot/ 'Docker hub')
[![](https://img.shields.io/docker/pulls/etiennek/spring-boot.svg)](https://hub.docker.com/r/etiennek/spring-boot/ 'Docker hub')

## What is this?

A Docker base image, following Docker image
[best-practices](http://http://www.projectatomic.io/docs/docker-image-author-guidance/),
for Spring Boot applications.

Some features:

  - The Spring Boot application runs as a non-root user inside 
    the container
  - Injection of JVM- and application arguments at runtime
  - Wrapper script that will forward Unix signals (example: 
    `SIGINT`) to the Spring Boot application

## How do I use it?

Using this base image to create an image for your Spring
Boot application is easy:

  1. Create a new Dockerfile and use this image as the 
     `FROM` image
  1. Make sure the Spring Boot application jar file is in 
  the same directory as the Dockerfile you've created above
  1. Build your Dockerfile

Here's an example on how you would use it
(Replace `/path/to/spring/boot/application.jar`
to the actual path of your Spring Boot application jar file):

```bash
mkdir bootapp-docker && cd bootapp-docker
echo "FROM etiennek/spring-boot" > Dockerfile
cp /path/to/spring/boot/application.jar ./app.jar
docker build -t bootapp-docker .
docker run -p 8007:8007 bootapp-docker
```

Then open up your browser to
[http://localhost:8007/](http://localhost:8007/) and you
should see your Spring Boot application's index page.

## Injecting JVM- and application arguments

To inject JVM- and application arguments during runtime,
simply override the `BOOTAPP_JAVA_OPTS` (for JVM) and 
`BOOTAPP_OPTS` (for application) environment variables.
Example:

```bash
docker run -p 8007:8007 \
  -e BOOTAPP_JAVA_OPTS="-Xms512m -Xmx1024m" \
  -e BOOTAPP_OPTS="--spring.profiles.active=test --spring.main.banner-mode=off" \
  bootapp-docker
```
