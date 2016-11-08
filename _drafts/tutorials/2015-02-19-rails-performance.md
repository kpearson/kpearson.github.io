---
title: Performance in Rails Applications
---
# Rails Performance

## Caching

## Backgraound workers

### Sidekiq

Create a backgroud worker class in `app/workers/`

```ruby
  class SequenceEmailWorker
    include Sidekiq::Worker

    def perform(email, sequence_length)
      SequenceMailer.sequence_email(email, sequence_length).deliver_now
    end
  end
```

Wrap the action in the method with the worker class (usualy controller acton)

```ruby
  def create
    SequenceEmailWorker.perform_async(params["email"], params["sequence_length"].to_i)
  end
```

## Configure Heroku

### Redis on heroku

`heroku addons:create redistogo`

Run `heroku config` to view the app config.  This let us confirm the new Redis config

We should now see:

```
REDISTOGO_URL:            redis://some-redis-host
```

Sidekiq by default looks at the ENV var `REDIS_PROVIDER` for which Redis to use.
We need to tell redis to use to one we just installed.
To do this run:

```
heroku config:set REDIS_PROVIDER=REDISTOGO_URL
```

### Mandrill Email

Set environmental valiables on heroku via the comandline interface:

```
  heroku config:set MANDRILL_USERNAME=<mandrill email> MANDRILL_APIKEY=<key>
```

In `app/config/environments/production.rb` add:

```ruby
  ActionMailer::Base.smtp_settings = {
    :port =>           '587',
    :address =>        'smtp.mandrillapp.com',
    :user_name =>      ENV['MANDRILL_USERNAME'],
    :password =>       ENV['MANDRILL_API_KEY'],
    :domain =>         'heroku.com',
    :authentication => :plain
  }
  ActionMailer::Base.delivery_method = :smtp
```

## Heroku managing prosess

The __Procfile__ is where we can setup how heroku configures the server and prosess.

```ruby
web: bundle exec rails server -p $PORT
worker: bundle exec sidekiq
```

We can view the current heroku prosess with `heroku ps`.

To spin-up workers: `heroku ps:scale worker=1`.

### Configure a seperate heroku instance to act as the worker ( for free )

First fork the heroku app:
`heroku fork --from app-one --to app-two`

Now we need to add the second app as a remote to the original.
To get the URL:
`heroku info -a app-two`

Take the Git URL and run: `git remote add <name> <Git URL>`

To run many heroku commands we will now need to use
`--app` to specify which app to act on.

#### Pointing the apps at Redis-to-go

Take the redis url form app one: `heroku config:get REDISTOGO_URL --app app-one`

and add it to app two:
`heroku config:set REDISTOGO_URL=redis://redistogo:some-redis-to-go-url --app app-two`

Finaly we need to tell the the second app not to run a
web prosess and to run the background worker.

```
heroku ps:scale web=0 worker=1 --app app-two
```
