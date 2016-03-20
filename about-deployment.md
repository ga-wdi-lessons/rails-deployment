# About Deployment

### Requirements for Deploying

There are generally a few things we need for an app to be properly deployed:

* **server** - the server(s) must be on and connected to the internet
* **services** - the server must be running the correct services (web,
  database, email, etc)
* **dependencies** - the server(s) must have the proper dependencies installed
(e.g. ruby, our gems, postgres, etc)
* **code** - we must get our code onto the server and run it
* **config** - we must configure our running app with any configuration that is
unique to that environment

### Many ways to deploy

There are lots of ways to do each of these steps. For example, we can get our
code onto a server by:

* FTP'ing the files onto the server
* Adding a git remote and using `git push` to send the files over
* Putting the files on a flash drive, attaching it to a homing pigeon, and
having someone receive the pigeon and copying the files over

### Heroku

Today, we'll be using a service called Heroku to deploy our apps, because it
makes all the above steps easy. For example, Heroku automatically:

* starts up a new server when we run `heroku create`, and installs all the necessary services
* adds a new remote to our git repo, so we can just run `git push heroku master` to copy our code over
* automatically uses `bundle install` to install our app's dependancies, and starts our app.

And if we need to change configuration information, we can set configuration
variables using `heroku config`, e.g.
