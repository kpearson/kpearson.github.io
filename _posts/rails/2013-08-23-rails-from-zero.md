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
*note: `-T` is equivalent to `--skip-test-unit`.
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

