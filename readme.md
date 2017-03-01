# Deployment

## Learning Objectives

- Define 'deployment', and contrast different methods of deploying an application
- Describe the difference between development, test, and production environments
- Deploy a rails application using heroku
- Run migrations on heroku
- Debug errors on heroku (using logs)
- Describe the major points of a `12-factor` application as applied to deployment
- Use environment variables to keep sensitive data out of code
- List common pitfalls and their solutions when deploying to heroku
- Describe the role of the asset pipeline in rails

## Framing

Deployment is the act of putting our app up on one or more servers connected to
the internet, such that people can use our app.

## [Read: Rails Environments](about-environments.md)

## [Read: Deployment](about-deployment.md)

## [You Do: Deploying to Heroku](deploying-your-first-app.md)

We're going to use Heroku to deploy our app, because it has a "free" pricing tier, and is very easy to get started with.

## [Reference: Common Errors](common-errors.md)

# Resources
- [Rails Asset Pipeline](asset-pipeline.md)
- [Getting Started Deploying Rails on Heroku](ttps://devcenter.heroku.com/articles/getting-started-with-rails5)
- [The Twelve-Factor App](http://12factor.net)
- Screencasts
  - WDI7
    - [Part 1](https://youtu.be/8NZsSxFSFLM)
    - [Part 2](https://youtu.be/EFDy2sAHFCw)
    - [Part 3](https://youtu.be/nx1gAA9tyog)
  - WDI8
    - [Part 1](https://youtu.be/7izx6kOOOGI)
    - [Part 2](https://youtu.be/_LiJBimguak)
    - [Part 3](https://youtu.be/ZGDVBwtsurk)

# Cheat Sheet

## Deployment Steps

The whole series of commands for deploying to Heroku is:

Add to the bottom of your Gemfile:

```rb
gem "rails_12factor", group: :production
```

```bash
$ bundle install
$ git add .
$ git commit -m "added 12factor"
$ heroku create my-sweet-app
# wait...
$ git push heroku master
# wait...
$ heroku run rails db:migrate
$ heroku run rails db:seed
$ heroku open
```

Then, to view your app's server log:

```bash
$ heroku logs -t
```

## To push changes to your app

```bash
$ git add .
$ git commit -m "your message"
$ git push heroku master
```

Note that this will *not* update Github. If you want to push your changes to Github as well, you need to run `git push origin master` as usual.

## To change your migrations

Do *not* edit an existing migration file. Instead:

```bash
$ rails g migration yourMigrationName
# Edit the new migration file
$ rails db:migrate
$ git add .
$ git commit -m "added migration"
$ git push heroku master
$ heroku run rails db:migrate
```

## To switch your Heroku app's environment to development

```bash
$ heroku config:set RAILS_ENV=development
```

...and to change it back:

```bash
$ heroku config:set RAILS_ENV=production
```

## Deleting apps

```sh
$ heroku apps:delete --confirm my-sweet-app
```

You're likely to end up with a bunch of Heroku apps. To delete all of them at once, you can add this function to your `.bash_profile`:

```sh
function happ(){
  for app in $(heroku apps)
    do heroku apps:delete --confirm $app
  done
}
```

...and then run `happ` from anywhere on your computer.
