# Deploying Your First App

Please clone [tunr_rails_users](https://github.com/ga-wdi-exercises/tunr_rails_users):

```
$ git clone git@github.com:ga-wdi-exercises/tunr_rails_users.git
$ git checkout 5-added-assocations
```

We're going to use Heroku to deploy our app, because it has a free tier, and is
incredibly easy to get started with.

## Installing the Heroku Toolbelt

The Heroku toolbelt is a command line app that enables us to create new apps to
deploy, deploy code updates, and manage our server(s).

Follow the instructions on the
[Heroku Toolbelt site](https://toolbelt.heroku.com) to get it installed.

## Creating our App

**Note:** So far, you shouldn't have changed any of the code in `tunr_rails_users` that you cloned down at the start of class.

The first time we want to deploy a new app, we need to tell Heroku to create a
new server.

We'll also define the URL at which we want the app to be hosted. You can pick any name, but for this practice, let's go with `<YOUR-NAME>-tunr-rails.herokuapp.com`.

Note: If you try to name your app something with underscores in it, you'll get this error from Heroku:

```
Name must start with a letter and can only contain lowercase letters, numbers,
and dashes.
```

To create your app:

```bash
$ cd tunr_rails_users
$ heroku create <YOUR-NAME>-tunr-rails
```

When we run `heroku create`, heroku will automatically add a new git remote,
called `heroku`. This doesn't actually move your code anywhere -- it accomplishes just as much as `git remote add origin`.

> Note: If you type `heroku create` without a name afterward, it'll make up a name for you -- usually something goofy like `secure-wave-14641.herokuapp.com`. You can rename your app with `heroku apps:rename your-new-app-name`

## Deploying the App

To actually deploy our code onto the new server, we simply push to this new remote:

```bash
$ git push heroku master
```

> **Note** if you're trying to deploy from a branch other than master you want to use this syntax:

```
git push heroku <your-branch>:master
```

After doing this, you'll see a LOT of stuff printed out in your Terminal. You're watching Heroku run `bundle install` and run the commands necessary to set up a Rails app.

## Troubleshooting

Eventually, you'll get this message:

```
remote:  !     Push rejected, failed to compile Ruby app
remote:
remote: Verifying deploy....
remote:
remote: !	Push rejected to tunr-rails-users.
remote:
To https://git.heroku.com/tunr-rails-users.git
 ! [remote rejected] 5-added-associations -> master (pre-receive hook declined)
```

This means your deployment failed!

What happened? You can get a clue here, right before the above error message:

```
remote:        Tasks: TOP => assets:precompile
remote:        (See full trace by running task with --trace)
remote:  !
remote:  !     Precompiling assets failed.
```

When you go to the production environment from development, Rails tries to minify all of your CSS files and smush them into one file. (Tunr only has one CSS file, so Rails tries to minify that one.) This is called **precompiling**.

If there's any invalid CSS, Rails will fail to precompile.

> Side note: In the future, to check whether your CSS is OK without having to wait for Heroku to check it, you can run `rake assets:precompile`.

To fix this you'll do 3 things. You can see the results of them [in this pull request](https://github.com/ga-wdi-exercises/tunr_rails_users/pull/7):

### 1) Correct any errors in the CSS

If you scroll up through the error messages a bit, you'll see the message below, which references `font-inherit` and line `55`. The [CSS Validator](http://jigsaw.w3.org/css-validator) will be helpful.

```
remote:        Sass::SyntaxError: Invalid CSS after "   font-inherit": expected "{", was ";"
remote:        (sass):55
```

### 2) Add the `rails_12factor` gem to your app

[This gem](http://12factor.net/) helps manage your assets, and does some other things. (See the end of this lesson plan.) Not including it is a *very* common source of Heroku deployment errors.

Include this snippet in your `Gemfile`:

```
group :production do
  gem 'rails_12factor'
end
```

### 3) Push up to Heroku

Now, `bundle install`, `add`, `commit`, and `git push heroku master` again. **Any time you make changes** and want them to go onto Heroku, you'll need to run `git push heroku master`.

## Visiting Our Site

We could open our site manually by typing the URL into the browser, but Heroku
gives us a convenient tool to do this from our app's folder in the command line.

Simply run the following command to open the site in your default browser:

```bash
$ heroku open
```

You'll get an error page. No fear!

## Running Migrations on Heroku

The error is because Heroku creates a Postgres database for our app, but doesn't run
any migrations. To run our migrations on Heroku, we use the `heroku run`
command:

```bash
$ heroku run rake db:migrate
```

In general, the `heroku run` command will take the command immediately after it
and run it on your Heroku server, instead of locally.

After migrating, you should be able to successfully visit Tunr in your browser! The only downside is it will have no seed data.

## Seed your app

So: The usual command for *migrating* is `rake db:migrate`, and to migrate your Heroku database you type `heroku run rake db:migrate`.

Keeping that in mind, if the usual command for *seeding* is `rake db:seed`, how would you seed your Heroku database?

## Show logs

To be able to see your server log (what you normally see in Terminal when running a Rails app), use `heroku logs`. Some more information:

```bash
$ heroku logs             # print the most recent entries and quit
$ heroku logs -n 2000     # print the 2000 most recent entries and quit
$ heroku logs -t          # 'tail' - print the most recent entries and continue to print new ones until we quit using ctrl-c
```
