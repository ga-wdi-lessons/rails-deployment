# Common Errors

The most common pitfalls when deploying to Heroku are:

* not including the `rails_12factor` and `pg` gems
* not running `heroku run rails db:migrate`
* can't drop / reset your database
* saving user-uploaded data to the filesystem (instead of a service like AWS S3)
* checking in sensitive information into your public repository
* using a Git branch other than master
* having your Rails app in the wrong folder

## Not Using the Right Gems

Your app must include both the `rails_12factor` and `pg` gems if you are
deploying to production.

#### Make sure you create your app with `rails new my-app-name -d postgresql`.

This includes the `pg` gem for you. If you forget the `-d postgresql`, Rails will default to using SQLite3 for your database. This saves data in a file called `development.sqlite3` in your app's `db` folder.

Heroku rejects apps that use SQLite3: you can't upload them to Heroku. This is because Heroku erases your server whenever it goes to sleep. The only stuff that will survive the erasing is whatever is tracked with Git.

Presumably, your users are going to be saving lots of data to your database which is *not* going to be tracked by Git. Therefore, if you're using SQLite3, all of your data will periodically get erased -- which would make for a very poor app!

If you did forget `-d postgresql`, it's totally fixable -- just Google for the solution -- but a little annoying.

## Not Running Migrations on Heroku

Don't forget to run `heroku run rails db:migrate` after you include new
migrations, or your app won't work correctly.

## Can't Drop / Reset Your Database

On heroku, we can't run `heroku run rails db:drop`. Instead you need to run:

```bash
$ heroku pg:reset DATABASE
$ heroku run rails db:migrate
```

## Saving User Uploaded Data

Instead of saving user-uploaded data to the local filesystem, we need to store
those files somewhere permanent (heroku can wipe our filesystem at any time).

AWS S3 is a popular option if you're using heroku. For more info, see one of the
following links, depending on what gem you're using for file-upload.

* [CarrierWave + AWS](https://github.com/carrierwaveuploader/carrierwave#using-amazon-s3)
* [Paperclip + AWS](https://devcenter.heroku.com/articles/paperclip-s3)

## Avoiding Sensitive Info in Repos

Right now, we don't have any sensitive info in our repos, but if we use other
services / APIs in our app, we'll almost certainly need to configure our app
with API or other access keys.

IT IS VERY UNSAFE TO CHECK THESE KEYS INTO YOUR REPOSITORY!!!!!! (i.e. include
them in your code).

Instead, you should use environment variables in your code, and set the
environment variables on the server when you deploy.

The easiest way to do this is using one of the gems available to help you. We
suggest [figaro](https://github.com/laserlemon/figaro).

## Using a Git branch other than master

When you type `git push heroku master`, it tries to push your master branch to Heroku. If all your changes are in a branch other than master, this will be a problem!

Instead, enter this:

```
$ git push heroku your-branch-name:master
```

This says, "Push one specific branch to a specific *other* branch."

## Having your Rails app in the wrong folder

If, on trying to push to Heroku, you get an error saying "No Cedar app detected" or something similar, double-check whether your directory looks like this:

```
wdi/
  my-rails-app-folder/
    .git
    readme.md
    actual-rails-app/
      .gitignore
      app/
      bin/
      Gemfile
      Gemfile.lock (and so on)
```

If it does, the problem is that **your `.git` folder needs to be in the same folder as your Gemfile and the rest of your Rails app**.

That is, your directory should look like this:

```
wdi/
  my-rails-app-folder/
    .git
    .gitignore
    app/
    bin/
    Gemfile
    Gemfile.lock
    readme.md (and so on)
```

To fix this, you'll just move everything in your rails app up one folder, like so:

```bash
$ cd wdi/my-rails-app-folder
$ git mv actual-rails-app/* .
$ git add .
$ git commit -m "moved everything to root directory"
```

**Note** that dotfiles (files beginning with `.`) aren't moved with this command. You'll need to move those individually.

When you `git add`, there may be a TON of changes. This is because Git thinks you deleted a bunch of files, and then created a bunch of files. One way to mitigate this is to use the `git mv` command instead of just the `mv` command.

**If `git mv` doesn't work, however, just try `mv`.**

Once you've added and committed, if you push back up to `heroku master`, all should be well.

#### To avoid getting into this situation

Whenever you type `rails new myapp -d postgresql`, it creates a new folder called `myapp` *inside* your current folder. This results in the situation above.

To prevent this, instead type `rails new . -d postgresql`. This will create the Rails app inside the current folder, *instead* of inside a new folder.
