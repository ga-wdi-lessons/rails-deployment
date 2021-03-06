# Rails Environments

Application environments are an important part of the context in which an applications run. Specifically, environments store data and configurations that pertains to the phase of the application's development. Each environment is configured to support a certain usage of the application.

By default, a Rails app can be run in any of three different ***environments***. The three default environments are:

  1. development
  2. test
  3. production

Development is the default environment, and is what we are in when we run the app locally.

Deploying an app often entails deploying into a production environment. By production, we mean the public, published version of our site. This published version of the app would have important data generated by user activity, such as user accounts and user-submitted data.

Each environment has a different set of configurations, things that vary depending on how we're running or using our app. We could be testing, developing, or deploying our apps. Configuration settings often include...

  * the name of the database
  * the username/password to connect to the database
  * API authentication keys (e.g. to connect to twitter API)
  * whether or not to reload code on each request (for debugging vs performance)
  * where to save log information (error logs, etc)

## Environmental Differences

|Environment|  Database |    Log Filename   |Descriptive Errors?|Asset Precompilation|Code reloaded on request|
|-----------|-----------|-------------------|-------------------|--------------------|------------------------|
|Development|development|log/development.log|        Yes        |   Not by default   |           Yes          |
|Testing    |test       |   log/test.log    |        Yes        |   Not by default   |           Yes          |
|Production |production |log/production.log |        No         |         Yes        |           No           |

<details>
  <summary><strong>Listed Environment Differences</strong></summary>

  <h3> development environment </h3>
  <ul>
    <li> Rails will connect to your development database</li>
    <li> Rails will display informative error messages for any</li> error
    <li> code is reloaded on each request (so you don't have to</li> restart the server)
    <li> logs are written to `log/development.log`</li>
    <li> by default, your CSS / JS will not be combined into</li> one file (no default asset precompilation)
  </ul>

  <h3> test environment </h3>
  <ul>
    <li> Rails will connect to your test database</li>
    <li> The DB will be wiped between each test</li>
    <li> Rails will display informative error messages for any</li> error
    <li> code is reloaded on each request (so you don't have to</li> restart the server)
    <li> by default, your CSS / JS will not be combined into</li> one file (no default asset precompilation)
    <li> logs are written to `log/test.log`</li>
  </ul>

  <h3> production environment </h3>
  <ul>
    <li> Rails will connect to your production database</li>
    <li> Rails will NOT display full error messages (just a</li> generic 'error' page)
    <li> code is NOT reloaded on each request (for performance)</li>
    <li> logs are written to `log/production.log`</li>
    <li> your CSS / JS *will* be combined into one file for  performance</li>
  </ul>
</details>

### 12 Factor

12 Factor is a set of strong guidelines designed by Heroku for building apps that deploy and scale more easily and predictably. Spend 10 minutes reading through [this explanation of what characteristics a 12 Factor app has](http://www.clearlytech.com/2014/01/04/12-factor-apps-plain-english/).
