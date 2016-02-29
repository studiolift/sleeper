# Sleeper

Basic commands for a Ruby-on-Rails development environment on Docker.

> **sleeper** [_noun_, _British_]:
> a wooden or concrete beam laid transversely under railway track to support it

## Setup

You must have an accessible docker environment. If you are on OSX, try [dream](https://github.com/studiolift/dream) or [Docker Toolbox](https://www.docker.com/products/docker-toolbox). Untested on Linux but should work... PRs welcome if not!

Add `sleeper` to your `$PATH`.

## Usage

Given a simple rails project with a `Dockerfile`:

```
FROM ruby:2.1.2

RUN apt-get update -qq && apt-get install -y build-essential libpq-dev
RUN mkdir /app
WORKDIR /app

# Add gems - image must be rebuilt if this changes
ADD Gemfile /app/Gemfile
ADD Gemfile.lock /app/Gemfile.lock
RUN bundle install

ADD . /app
```

And `docker-compose.yml` file:

````
db:
  image: postgres
app:
  build: .
  command: bundle exec rails s -p 3000 -b '0.0.0.0'
  volumes:
    - .:/app
  ports:
    - "8001:3000"
  links:
    - db
````

When in the project directory you can setup and run the application without having to remember all the `docker-compose` commands.

First `sleeper setup` to build the images and setup the database (create, migrate and seed).

Then `sleeper start` to spin up the containers and get the application going

Finally in a new terminal window run `sleeper open` to open the application in your browser, without having to check for IP addresses and port numbers.

When you are done, `Ctrl+C` in the `sleeper start` terminal to close.

For more commands: `sleeper help`
