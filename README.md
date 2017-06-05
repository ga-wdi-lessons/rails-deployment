# Deployment

## Learning Objectives

- Define 'deployment', and contrast different methods of deploying an application
- Describe the difference between development, test, and production environments
- Deploy a rails application using Heroku
- Run migrations on Heroku
- Debug errors on Heroku (using logs)
- Describe the major points of a `12-factor` application as applied to deployment
- Use environment variables to keep sensitive data out of code
- List common pitfalls and their solutions when deploying to Heroku
- Describe the role of the asset pipeline in rails

## Framing

Deployment is the act of putting an app up on one or more internet-connected servers that allow users to access and use the app.

## About Deployment

### Requirements for Deploying

There are generally a few things we need for an app to be properly deployed:

* **server** - the server(s) must be on and connected to the internet
* **services** - the server must be running the correct services (web, database, email, etc)
* **dependencies** - the server(s) must have the proper dependencies installed (e.g. ruby, our gems, postgres, etc)
* **executed code** - we must get our code onto the server and run it
* **configuration** - we must configure our running app with respect to its deployment environment

### Many ways to deploy

There are lots of ways to do each of these steps. For example, we can get our code onto a server by...

  * Using FTP to transfer the files onto the server
  * Adding a `git` remote and using `git push` to transmit files (like with GH pages)
  * Putting the files on a flash drive, fastening it to a homing pigeon's leg, then having an operator receive the pigeon and copy the files over to the server

### Heroku

Today, we'll be using a service called Heroku to deploy our apps, because it makes all the above steps easy. For example, Heroku automatically does the following...

  * starts up a new server when we run `heroku create`, and installs all the necessary services
  * adds a new remote to our git repo, so we can just run `git push heroku master` to copy our code over
  * detects our database
  * automatically uses `bundle install` to install our app's dependancies, and starts our app.

If we need to change configuration information, we can set configuration variables using `heroku config`, e.g.

## [Read: Rails Environments](about-environments.md)

## [Read: Deployment](about-deployment.md)

## [You Do: Deploying to Heroku](deploying-your-first-app.md)

> We'll use Heroku to deploy our app, since it has a "free" pricing tier, and a ton of nice features that simplify and expedite deployment.

## [Reference: Common Errors](common-errors.md)

## [Reference: Deployment Cheat Sheet](cheat-sheet.md)

## Resources
  - [Rails Asset Pipeline](asset-pipeline.md)
  - [Getting Started Deploying Rails on Heroku](https://devcenter.heroku.com/articles/getting-started-with-rails5)
  - [The Twelve-Factor App](http://12factor.net)

## Deployment Screencasts
  - WDI7
    - [Part 1](https://youtu.be/8NZsSxFSFLM)
    - [Part 2](https://youtu.be/EFDy2sAHFCw)
    - [Part 3](https://youtu.be/nx1gAA9tyog)
  - WDI8
    - [Part 1](https://youtu.be/7izx6kOOOGI)
    - [Part 2](https://youtu.be/_LiJBimguak)
    - [Part 3](https://youtu.be/ZGDVBwtsurk)
