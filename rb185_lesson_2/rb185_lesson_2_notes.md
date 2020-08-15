##### RB185 Database Applications

---

## Lesson 2: Database-backed Web Applications

### What to Focus On

* What schema does an application require?
* Project setup is secondary.

---

### Designing a Schema

* Create a `schema.sql` file.

* Write the SQL code and save it to the file.

* Create a new database; then load the file into the database with `psql`

  ```sql
  createdb db_name
  psql -d db_name < schema.sql
  ```

---

### Deploying PG Applications to Heroku

1. Make sure your code is working and committed to your local Git repository.

2. Follow these steps to prepare your application for deployment to Heroku:

   * Specify a version of Ruby in the `Gemfile`.

     ```ruby
     ruby "2.4.2"
     ```

   * Add `puma` to the `Gemfile`.

     ```ruby
     group :production do
       gem "puma"
     end
     ```

   * Run `bundle install`.

   * Add a `Procfile`. Insert the following line of code in the `Procfile`:

     ```
     web: bundle exec puma -t 5:5 -p ${PORT:-3000} -e ${RACK_ENV:-development}
     ```

   * Create a `config.ru` file with the following code:

     ```ruby
     require './file_name'
     run Sinatra::Application
     ```

   * Verify that everything is working using `heroku local`.

   * Make sure that any development-specific code, such as `sinatra/reloader`, won't run in production.

     ```ruby
     require 'sinatra/reloader' if development?
     
     # or
     
     configure(:development) do
       require "sinatra/reloader"
     end
     ```

   * For more info, see [Configuring an Application for Deployment](https://launchschool.com/lessons/26c18317/assignments/ab12b730) and [Deploying PG Applications to Heroku](https://launchschool.com/lessons/421e2d1e/assignments/54681a23).

3. Update your code to open the database like this:

   ```ruby
   @db = if Sinatra::Base.production?
   				PG.connect(ENV['DATABASE_URL'])
   			else
   				PG.connect(dbname: "db_name")
   			end
   ```

   This code uses the `DATABASE_URL` environment variable to determine the database name when running on Heroku.

4. Create an application on Heroku by running `heroku apps:create` within the project's directory:

   ```
   $ heroku apps:create xyzzy-rb180-todos
   Creating ⬢ xyzzy-rb180-todos... done
   https://xyzzy-rb180-todos.herokuapp.com/ | https://git.heroku.com/xyzzy-rb180-todos.git
   ```

   Use your own Heroku login name or any unique string in place of `xyzzy`, and use an appropriate app name for the `rb180-todos` part.

5. Enable PostgreSQL for your application on Heroku:

   ```
   $ heroku addons:create heroku-postgresql:hobby-dev -a xyzzy-rb180-todos
   Creating heroku-postgresql:hobby-dev on ⬢ xyzzy-rb180-todos... free
   Database has been created and is available
    ! This database is empty. If upgrading, you can transfer
    ! data from another database with pg:copy
   Created postgresql-asymmetrical-27376 as DATABASE_URL
   Use heroku addons:docs heroku-postgresql to view documentation
   ```

6. If you have some code to create your database schema in `schema.sql`, you can try running the following command to do so:

   ```
   $ heroku pg:psql -a postgresql-asymmetrical-27376 < schema.sql
   - -> Connecting to postgresql-asymmetrical-27376
   Pager usage is off.
   ...
   ```

   Make sure you're using the correct application name after the `-a` option. If, after confirming that you're using the correct name, you get the message `Couldn't find that app`, try running it without the application name:

   ```
   $ heroku pg:psql < schema.sql
   --> Connecting to postgresql-asymmetrical-27376
   Pager usage is off.
   ...
   ```

   If you need to set up your database schema manually, you can log into a psql shell on Heroku with:

   ```
   $ heroku pg:psql -a postgresql-asymmetrical-27376
   ```

   As before, you can try running this command without the application name if it complains that it `Couldn't find that app`:

   ```
   $ heroku pg:psql
   ```

7. We're using Heroku's free hobby-dev PostgreSQL database plan, which only allows for a maximum of 20 open database connections at once. If we exceed this limit, then our application will throw an error. Add the following code into your application to ensure that you don't exceed that 20 connection limit.

   ```ruby
   after do
     @storage.disconnect
   end
   ```

   ```ruby
   def disconnect
     @db.close
   end
   ```

8. Commit any changes you made above.

9. Deploy the application to Heroku using `git push heroku master` (if you're working on a branch other than master, you'll need to use its name in this command instead).

Now if you visit your app's URL, your app should appear as a brand new app that is ready for you to track your todos permanently.



