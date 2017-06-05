# Deployment Cheat Sheet

## Table of Contents

  1. [Deployment Steps](#deployment-steps)
  2. [Debugging](#debugging)
  3. [Pushing Changes](#pushing-changes)
  4. [Changing Migrations](#changing-steps)
  5. [Switching Heroku Environments](#switching-heroku-environments)
  6. [Deleting Heroku Apps](#deleting-heroku-apps)

## Deployment Steps

1. *__After__ switching to the branch you'd like to deploy*, create a new branch for deployment...

```bash
 $ git checkout -b deployment
```

2. Add the following line to the bottom of your Gemfile to add the `rails_12factor` dependency to the `production` environment...

```rb
gem 'rails_12factor', group: :production
```

```bash
 $ bundle install
```

3. Commit your changes.

```bash
 $ git add .
 $ git commit -m "added 12factor"
```

4. Create a Heroku app using the Heroku CLI tools...

```bash
 $ heroku create my-sweet-app
# wait...
 $ git push heroku deployment:master
 # wait some more...
```

The first command will both create a new app on your Heroku account and add a **remote** on your repository named `heroku`, which points at your deployed Heroku app.

The second command will push from the local branch `deployment` to the remote branch `master` on the remote named `heroku`.

5. Run migrations and seed on Heroku...

```bash
 $ heroku run rails db:migrate
 $ heroku run rails db:seed
 $ heroku open
```

Heroku automatically detects the database and creates it for you. It is up to you to finish building your application's database, however.

## Debugging

Errors associated with deployment will appear here. To view your app's server log...

```bash
 $ heroku logs
```

You can also monitor the log file in your terminal session by passing the following option flag...

```bash
 $ heroku logs -t
```

> You can also view the logs in the browser via Heroku's dashboard by visiting `https://dashboard.heroku.com/apps/your-app-name/logs`

## Pushing Changes

```bash
 $ git add .
 $ git commit -m "your message"
 $ git push heroku master
```

Note that this will *not* update Github. If you want to push your changes to Github as well, you need to run `git push origin master` as usual.

## Changing Migrations

Do *not* edit an existing migration file. Instead...

```bash
 $ rails g migration yourMigrationName
# Edit the new migration file
 $ rails db:migrate
 $ git add .
 $ git commit -m "added migration"
 $ git push heroku master
 $ heroku run rails db:migrate
```

## Switching Heroku Environments

```bash
 $ heroku config:set RAILS_ENV=development
```

To change it back...

```bash
 $ heroku config:set RAILS_ENV=production
```

## Deleting Heroku Apps

```sh
 $ heroku apps:delete --confirm my-sweet-app
```

You're likely to end up with a bunch of Heroku apps. To delete all of them at once, you can add this function to your `.bash_profile`...

```sh
function happ(){
  for app in $(heroku apps)
    do heroku apps:delete --confirm $app
  done
}
```

...and then in bash, run `happ` from anywhere on your computer; the working directory doesn't matter.
