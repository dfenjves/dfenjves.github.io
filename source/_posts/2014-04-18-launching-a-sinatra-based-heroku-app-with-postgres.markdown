---
layout: post
title: "Launching a Sinatra-based Heroku App with Postgres"
date: 2014-04-18 13:54:43 -0400
comments: true
categories: 
---

Friday was equally one of the most frustrating and satisfying days I’ve had since I’ve started at the Flatrion School. We spent the day working on our pending labs/personal projects and I picked up where I’d left off on my venture to scrape yelp and display museum opening and closing hours dynamically. Once I had my css where I felt like it was somewhat presentable, I decided to set up my application on Heroku, something I imagined would be a simple and painless process. Was I wrong. What followed was five hours of dealing with obstacle after obstacle to get my app launched - so much so that at one point I had four TAs gathered around trying to help me get through the myriad of issues I was facing. Anyway, I’m going to try to explain what I did to get my app up and running, hopefully this will be useful to other people trying to do the same thing.

**Part 1:  Moving from SQLite3 to Postgres**

Heroku doesn’t work with SQLite 3, so the first challenge was to move is-it-open-nyc to postgres. Here are the steps to make this happen:

+Remove sqlite3 from your gemfile and add pg (postgres).
+Change your database connection in your environment.rb file:
```
ActiveRecord::Base.establish_connection('postgres://localhost/project')
```

+Initialize a new postgres server:
```
initdb -D projectdb
```

+Start up the Postgres server in your project directory. This server needs to be running locally when you test your postgres-connected app:
```
postgres -D projectdb
```
+Leave the server running and in a separate terminal window create a new database within this database cluster. Make sure to give the database the same name as the connection you set up in your Activerecord::Base connection in your environmencreatedb -D project
Check and see if your database is set up (and play around with data) by running the cli for psql (more on psql here):
```
psql project
```
Once your database is set up, run your migrations in exactly the same way you've been doing for sqlite3. The activerecord adaptor takes care of the connection to the database, so all you should be doing is running rake db:migrate. Seed your data, fire up your database, cross your fingers, and hope that there's something there!

**Part deux: Pushing to Heroku**
```
heroku init
git push heroku master
```

+That's pretty much it, in terms of getting the app on to Heroku (More reading here), but you're not done. You need to set up your database on Heroku itself.
+Install the Postgres buildpack for Heroku:
```
heroku addons | grep POSTGRES
```
+Connect heroku to your postgres db:
```
heroku addons:add heroku-postgresql:dev
```
+When this is done, you'll be assigned a  HEROKU_POSTGRESQL_COLOR_URL whose URL will serve as the heroku database for your app. To get the url run:
```
heroku config | grep HEROKU_POSTGRESQL
```
+The url will look something like this: 
```
HEROKU_POSTGRESQL_RED_URL: postgres://user3123:passkja83kd8@ec2-117-21-174-214.compute-1.amazonaws.com:6212/db982398
```
+Jump back in to your project and replace the ActiveRecord::Base connector url  ('postgres://localhost/project') with this new url. Add, commit and push again to heroku.
+From here, your last step is to run your migrations and seed your database in Heroku. To run files, you have to do the same thing your would locally, but add in 'heroku run':
```
heroku run rake db:migrate
heroku run rake db:seed
```
That's it. There are a LOT of steps to go through, but if you've done it once and understand what's going on, future attempts should be much easier.

Here it is, up on Heroku:

is-it-open-nyc.herokuapp.com