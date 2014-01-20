Heroku buildpack: Heroku Toolbelt
=========================

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) that
allows one to run Heroku Toolbelt in a dyno alongside application code.
It is meant to be used in conjunction with at least the
[minimal ruby](https://github.com/dpiddy/heroku-buildpack-ruby-minimal) as well
as other buildpacks as part of a
[multi-buildpack](https://github.com/ddollar/heroku-buildpack-multi).
The heroku binary is added to the dyno's $PATH.

Usage
-----

Example usage:

    $ ls -a
    .buildpacks  Gemfile  Gemfile.lock  Procfile  config/  config.ru

    $ heroku config:add BUILDPACK_URL=https://github.com/ddollar/heroku-buildpack-multi.git

    $ heroku config:add HEROKU_TOOLBELT_API_EMAIL=my-fake-email@gmail.com

    $ heroku config:add HEROKU_TOOLBELT_API_PASSWORD=`heroku auth:token`

    $ cat .buildpacks
    https://github.com/gregburek/heroku-buildpack-toolbelt.git
    https://github.com/heroku/heroku-buildpack-ruby.git

    $ git push heroku master
    ...
    -----> Fetching custom git buildpack... done
    -----> Multipack app detected
    =====> Downloading Buildpack: https://github.com/gregburek/heroku-buildpack-toolbelt
    =====> Detected Framework: heroku-toolbelt
    -----> Fetching and vendoring Heroku Toolbelt into slug
    -----> Moving the netrc generation script into app/.profile.d
    -----> heroku toolbelt installation done
    =====> Downloading Buildpack: https://github.com/heroku/heroku-buildpack-ruby.git
    =====> Detected Framework: Ruby/Rack
    -----> Using Ruby version: ruby-1.9.3
    -----> Installing dependencies using Bundler version 1.3.2
    ...

    $ heroku run 'heroku auth:token'
    Running `heroku auth:token` attached to terminal... up, run.3706
    abcdef0123456789abcdef0123456789abcdef01

    $ heroku run 'heroku pgbackups:capture SILVER -a myapp'
    Running `heroku pgbackups:capture SILVER -a myapp` attached to terminal... up, run.9532

    HEROKU_POSTGRESQL_SILVER_URL  ----backup--->  b003

    Capturing... done
    Storing... done
