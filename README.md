Heroku buildpack: Heroku Toolbelt
=========================

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) that
allows one to run Heroku Toolbelt in a dyno alongside application code.
It is meant to be used in conjunction with at least the
[minimal ruby](https://github.com/dpiddy/heroku-buildpack-ruby-minimal) as well
as other buildpacks as part of a
[multi-buildpack](https://github.com/ddollar/heroku-buildpack-multi).


Usage
-----

Example usage:

    $ ls -a
    .buildpacks  Gemfile  Gemfile.lock  Procfile  config/  config.ru

    $ heroku config:add BUILDPACK_URL=https://github.com/ddollar/heroku-buildpack-multi.git

    $ heroku config:add HEROKU_API_KEY=`heroku auth:token`
    $ heroku config:unset HEROKU_HOST

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

    $ heroku run 'vendor/heroku-toolbelt/bin/heroku auth:token'
    Running `vendor/heroku-toolbelt/bin/heroku auth:token` attached to terminal... up, run.3706
    abcdef0123456789abcdef0123456789abcdef01

    $ heroku run 'vendor/heroku-toolbelt/bin/heroku pgbackups:capture SILVER -a myapp'
    Running `vendor/heroku-toolbelt/bin/heroku pgbackups:capture SILVER -a myapp` attached to terminal... up, run.9532

    HEROKU_POSTGRESQL_SILVER_URL  ----backup--->  b003

    Capturing... done
    Storing... done


Notes
------

WARNING: Your API token may change at any time for many reasons. If you are
getting authentication failures, try:

```
$ heroku config:set HEROKU_API_KEY=`heroku auth:token`
```

Some PATH manipulation may be needed to successfully use the toolbelt,
especially if you are using the heroku-buildpack-ruby-minimal solely to provide
a ruby execution environment for the heroku cli gem, which will conflict with
the toolbelt. Try:

```
$ heroku run 'env PATH="/app/bin" vendor/heroku-toolbelt/bin/heroku apps'
```
