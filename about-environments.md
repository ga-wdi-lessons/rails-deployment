# Rails Environments

By default, a Rails app can be run in any of three different "environments". The
three default environments are:

* development
* test
* production

Each environment is basically a different set of configurations, things that
vary depending on where / why we're running our app. For example, configuration
settings might include:

* the name of the database
* the username/password to connect to the database
* API authentication keys (e.g. to connect to twitter API)
* whether or not to reload code on each request (for debugging vs performance)
* where to save log information (error logs, etc)

### development environment

Development is the default environment, and is what we are in when we run the
app locally.

In this mode:

* Rails will connect to your development database
* Rails will display informative error messages for any error
* code is reloaded on each request (so you don't have to restart the server)
* logs are written to `log/development.log`
* your CSS / JS will not be combined into one file

### test environment

* Rails will connect to your test database
* The DB will be wiped between each test
* Rails will display informative error messages for any error
* code is reloaded on each request (so you don't have to restart the server)
* logs are written to `log/test.log`

### production environment

* Rails will connect to your production database
* Rails will NOT display full error messages (just a generic 'error' page)
* code is NOT reloaded on each request (for performance)
* logs are written to `log/production.log`
* your CSS / JS *will* be combined into one file for performance


We usually think of deploying our app to mean 'deploying into production'. By
production, we mean the 'public' version of our site. The one with the important
data that all of our users are using.
