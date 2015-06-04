---
layout: post
category: article
title: Rails from Zero
---

*osx 10.10.1 | Jan 14, 2015*

## Prerequisites

- RVM or rbenv
- Bundler

## Create a new Rails app

```bash
rails new <app_name> -T --database=postgresql --skip-turbolinks
```

This gives us a new Rails app with no test framwork (-T),
a postgrsql database(--database=postgresql),
and no turbolinks (--skip-turbolinks).

## Gems

```ruby
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
```

If Minitest is your thing swap out the rspec spsific gem with:

```ruby
group :test, :development do
  gem 'minitest-rails'
  gem 'guard-minitest', require: false
end
```

With test frame work chosen and gems in place run:

```bash
bin/bundle
```

## Testing Framework

Next depending on the test-framework run the installation generator:

### [Minitest]

```bash
rails generate minitest:install
```

This will add the `test_helper.rb` file to the `test/` directory.

### [Rspec]

```bash
rails generate rspec:install
```

This adds the following files which are used for configuration:

```
- .rspec
- - spec/spec_helper.rb
- - spec/rails_helper.rb
```

### [Guard]

```bash
guard init rspec __or__ minitest
```

[rails_12factor]: https://github.com/heroku/rails_12factor
[ActionView helpers]: http://api.rubyonrails.org/classes/ActionView/Helpers/TextHelper.html
[form helpers]: http://guides.rubyonrails.org/form_helpers.html
[Routing]: http://edgeguides.rubyonrails.org/routing.html
[factory_girl]: https://github.com/thoughtbot/factory_girl
[Guard]: https://github.com/guard/guard
[guard-rspec]: https://github.com/guard/guard-rspec
[guard-minitest]: https://github.com/guard/guard-minitest
[Growl]: http://growl.info/downloads
[growl_gem]: https://rubygems.org/gems/growl
[Bundler]: http://bundler.io/
[Minitest]: https://github.com/blowmage/minitest-rails
[Rspec]: https://github.com/rspec/rspec-rails
[Command line options]: https://github.com/guard/guard/wiki/Command-line-options-for-Guard
[pry]: http://pryrepl.org/
[partials]: http://guides.rubyonrails.org/layouts_and_rendering.html#using-partials
[Scopes]: http://guides.rubyonrails.org/active_record_querying.html#scopes
[Rspec feature tests]: https://github.com/rspec/rspec-rails#feature-specs
[Capybara]: https://github.com/jnicklas/capybara

