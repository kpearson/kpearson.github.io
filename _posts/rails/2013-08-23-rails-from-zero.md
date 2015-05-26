---
layout: post
category: article
title: Rails from Zero
---

*osx 10.10.1   |  Jan 14, 2015*

# Rails Up and Running

Prerequisites: RVM or rbenv, Bundler, ...

Create a new Rails app:

{% highlight bash %}
rails new <app_name> -T --database=postgresql --skip-turbolinks
{% endhighlight %}

This gives us a new Rails app with no test framwork (-T),
a postgrsql database(--database=postgresql),
and sans turbolinks (--skip-turbolinks).

If your using Rspec add `rspec-rails` gem to Gemfile and bundle.
Then run:
{% highlight bash %}
rails g rspec:install
{% endhighlight %}
*note: `-T `is equivalent to `--skip-test-unit`.
Both are only used on the `rails new` command.*

## Add gems to `Gemfile`:

__[Minitest]__

{% highlight ruby %}
group :test, :development do
  gem 'pg'
  gem 'minitest-rails'
  gem 'capybara'
  gem 'launchy'
  gem 'growl'
  gem 'guard'
  gem 'pry-rails'
  gem 'guard-minitest', require: false
end
{% endhighlight %}

__Or__

__[Rspec]__

{% highlight ruby %}
group :test, :development do
  gem 'pg'
  gem 'rspec-rails'
  gem 'capybara'
  gem 'launchy'
  gem 'growl'
  gem 'guard'
  gem 'pry-rails'
  gem 'guard-rspec', require: false
end
{% endhighlight %}

With test frame work chosen and gems in place run:
`bin/bundle`

## Testing Framework

Next (depending on the test-framework) run the installation generator:

__[Minitest]__

`rails generate minitest:install`
This will add the `test_helper.rb` file to the `test/` directory.

__Or__

__[Rspec]__

`rails generate rspec:install`
This adds the following files which are used for configuration:

- .rspec
- spec/spec_helper.rb
- spec/rails_helper.rb

[Guard] generator:__

`guard init rspec __or__ minitest`

***

## Postgres

Setup postgres in Production, Test and/or Development.
By editing the adapter argument in `database.yml` the `rake db:create` and
`rake db:migrate` take care of the rest.
Example: `config/database.yml`

{% highlight ruby %}
default: &default
adapter: postgresql
encoding: unicode
pool: 5
timeout: 5000

development:
<<: *default
database: dinner_dash_development

# Warning: The database defined as "test" will be erased and

# re-generated from your development database when you run "rake".

# Do not set this db to the same as development or production.
test:
<<: *default
database: dinner_dash_test

production:
adapter: postgresql
database: dinner_dash
{% endhighlight %}

Create a the Dev Database named in the database.yml file.
__`bin/rake db:create`__
Run:
__`bin/rake db:migrate`__
Creates the test Database named in the database.yml file and runs the migrations, creating the db tables in test and development databases.

***

Start Guard:
`bundle exec guard`
