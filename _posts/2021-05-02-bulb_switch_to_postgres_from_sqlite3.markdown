---
layout: post
title:      "💡 Switch to Postgres from SQLite3"
date:       2021-05-02 17:56:50 -0400
permalink:  bulb_switch_to_postgres_from_sqlite3
---


As a part of my development journey at Flatiron School, I developed several capstone projects the utilized SQLite3 databases. These all worked great for me, but when it came time to get my project up on Heroku I hit a wall!

You will need to change your database to Postgres if you want to go the Heroku route, and here is how I did it.

## 📖 Useful resources

This links were very helpful for me making this database change as well as getting everything working on Heroku.

[SQLite3 to Postgres](https://medium.com/@thorntonbrenden/ruby-on-rails-switch-from-sqlite3-to-postgres-590009645c25)

[Postgres can't connect to server](https://stackoverflow.com/questions/31645550/postgresql-why-psql-cant-connect-to-server)

[Deploying Rails API React App to Heroku](https://dev.to/caicindy87/deploying-rails-api-backend-react-frontend-app-to-heroku-5a25)

## 💎 Update your Gemfile

You will want to remove SQLite3 from your Gemfile and add Postgres.

Remove SQLite3:
`gem 'sqlite3'`

Add Postgres:
`gem 'pg'`

Run `bundle install` to update your Gemfile.lock.

## 📒 Update your database.yml file

Your database.yml may look something like this:

    # SQLite. Versions 3.8.0 and up are supported.
    #   gem install sqlite3
    #
    #   Ensure the SQLite 3 gem is defined in your Gemfile
    #   gem 'sqlite3'
    #
    default: &default
    adapter: sqlite3
    pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
    timeout: 5000

    development:
    <<: *default
    database: db/development.sqlite3

    # Warning: The database defined as "test" will be erased and
    # re-generated from your development database when you run "rake".
    # Do not set this db to the same as development or production.
    test:
    <<: *default
    database: db/test.sqlite3

    production:
    <<: *default
    database: db/production.sqlite3

Replace it with something like this:

    # PostgreSQL. Versions 9.3 and up are supported.
    default: &default
    adapter: postgresql
    encoding: unicode
    pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>

    development:
    <<: *default
    database: farout_development
    test:
    <<: *default
    database: farout_test

    production:
    <<: *default
    database: farout_production
    username: farout
    password: <%= ENV['MYAPP_DATABASE_PASSWORD'] %>
		# PostgreSQL. Versions 9.3 and up are supported.
    default: &default
    adapter: postgresql
    encoding: unicode
    pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
    
    development:
    <<: *default
    database: farout_development
    test:
    <<: *default
    database: farout_test
    
    production:
    <<: *default
    database: farout_production
    username: farout
    password: <%= ENV['MYAPP_DATABASE_PASSWORD'] %>

## 🌳 [optional] Set your environment variable

I believe that you can leave the password field blank in your database.yml file, but I have opted to have a password that is stored in my `.env` file in the root directory.

In `.env`, add:

`MYAPP_DATABASE_PASSWORD={{your password here}}`

## ☁️ Start your Postgres Server

I have done this several ways.

1st way:
`sudo service postgresql start`

2nd way can be found [here](https://stackoverflow.com/questions/31645550/postgresql-why-psql-cant-connect-to-server)

## 🚗 Create and Migrate your database

`rake db:setup`

`rake db:migrate`

## ✔️ Check your work

`rails s`

then go to `http://localhost:3000`

If you see the Rails startup page, then congrats, IT WORKED!

