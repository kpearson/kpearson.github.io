---
layout: post
keywords: [ruby, rails, rspec, minitest-rails]
---

*Ruby 2.1.4 | Rails 4.2.3 | Sep 2015*

When I begin a new Ruby on Rails project there are a few thing I always like to have in place from the start. This post cover my initial setup, go to gems and what they do. There is a [companion code base] on Github to everything I go over so you can see what the code looks in a working application. Just make sure on the brach which corresponds the example.

## Prerequisites

- RVM or rbenv
- [Bundler]

## Create a new Rails app

```bash
rails new <app_name> -T --database=postgresql --skip-turbolinks
```

This gives us a new Rails app with a few option in place that once I found I use ever time.

Option | Description
--- | ---
__`-T`__ | No test framework. Only use this if you're planing on using Rspec
__`--database=postgresql`__ | Setup for a postgrsql database
__`--skip-turbolinks`__ | Turbolinks are more trouble than they are worth

A more complete list of the available [command line options] can be found by entering the `rails -h` command.

>Note: All of the `rails new` options can be set as default by adding them to a `.railsrc` file in your home directory.

>```
># ~/.railsrc
>
>--skip-test-unit
>--database=postgresql
>--skip-turbolinks
>```

## Gems

My next step is to add a few gems I know I'm going to use in the initial setup. I'll go over them in more detail as we put them to use. I also remove all the comments and organize the Gemfile a little.

### Example Gemfile (Rspec)

```ruby
gem 'pry-rails'

group :test, :development do
  gem 'rspec-rails'
  gem 'capybara'
  gem 'launchy'
  gem 'database_cleaner', '~> 1.4.1'
end
```

### Example Gemfile (Minitest)

The following approach to setting up [Minitest] in a Rails application is specific to the `minitest-rails` gem. Rails comes with [Minitest] built in, however; setting up an app to make use of [Minitest] without the the gem requires slightly different wiring.

```ruby
# Gemfile

gem 'pry-rails'

group :test, :development do
  gem 'minitest-rails-capybara'
  gem 'launchy'
  gem 'database_cleaner', '~> 1.4.1'
end
```

>Note: The Minitest-rails gem is a dependency of `minitest-rails-capybara` so I don't need to list in the `Gemfile`. If want, you can include it. It doesn't hurt to include in the file.

### Bundle

With test framework chosen and gems in place run `bin/bundle`

## Testing Framework Setup

Now that we have the gems in place we need to do the initial setup.

### [Rspec] Setup

```bash
rails generate rspec:install
```

This will add a couple configuration files:

```
- .rspec
- - spec/spec_helper.rb
- - spec/rails_helper.rb
```

At this point all I do to the new files is remove the comments. Here is the [example Rspec] implementation on Github.

### [Minitest] Setup

The `minitest-rails` gem gives us a generator.

```bash
rails generate minitest:install
```

This will overwrite the current `/test/test_helper.rb` created by the `rails new ...` command. I remove the comments in the new file and I end up with something that looks like:

```ruby
ENV["RAILS_ENV"] = "test"
require File.expand_path("../../config/environment", __FILE__)
require "rails/test_help"
require "minitest/rails"

class ActiveSupport::TestCase
  ActiveRecord::Migration.check_pending!
  fixtures :all
end
```

### Unit testing

At this point we are all set up for unit testing. The test framework know about it's self and what directory to look in for test files. We can run the Rails generators and get the corresponding test files.

Next we'll get integration and feature testing up and running.

## [Capybara] Setup

The capybara gem is a very common way to implement integration/feature testing. In short it uses a kind of a browser called a _headless browser_ to navigate the app, This provides the ability to test the presence of elements on the page which change based on input. For example click the logout button we expect to the page  to display "Good bye {user name}".

To get Capybara up and running in our test framework we need to do two things. Add the gem(s) and make the gem available.

>One thing to note: Integration tests and feature tests are effectively the same thing. __Integration tests for Minitest land and feature for in Rspec world.__

### Rspec Capybara Setup

```ruby
# Gemfile

group :test, :development do
  gem 'capybara'
  gem 'launchy'
end
```

```ruby
# rails_helper.rb

require 'capybara/rails'

```

Capybara and Rspec play very nicely together. So nicely in fact, if you place your feature tests in the `spec/features` directory Rspec will automatically know what type of test to run and you won't need to specify this in your example groups.

>A word of caution. This convenience can trip you up if your not a ware of it. Naming the directory for your feature tests anything other than `spec/features` i.e. `spec/feature`, you will need to explicitly tag each example group with `:type => :feature` where you will be using Capybara.

>```ruby
>describe "the signin process", :type => :feature do
>  before :each do
>    User.make(:email => 'user@example.com', :password => 'password')
>  end

>  it "signs me in" do
>    visit '/sessions/new'
>    within("#session") do
>      fill_in 'Email', :with => 'user@example.com'
>      fill_in 'Password', :with => 'password'
>    end
>    click_button 'Sign in'
>    expect(page).to have_content 'Success'
>  end
>end
>```

### Minitest Capybara Setup

My approach with [Minitest] is a little different from the Rspec example. I used the `minitest-rails-capybare` gem to bring in `minitest-rails` gem in as a dependency, so we have nothing to change in the `Gemfile` at this point.

In the `test/test_helper.rb` we need to add `require "minitest/rails/capybara"`.

After all the dust settles I end up with a `test_helper.eb` that looks like this:

```ruby
ENV["RAILS_ENV"] = "test"
require File.expand_path("../../config/environment", __FILE__)
require "rails/test_help"
require "minitest/rails"
require "minitest/rails/capybara"

class ActiveSupport::TestCase
  fixtures :all
end
```

Here is the [example Minitest] implementation on Github.

Minitest integration tests live in `test/integration`. This will have been automatically created with `rails new ...` if you don't include the `-T` option.

### Launchy

When using either Minitest or Rspec the __Launchy gem__ is something I alway have in place with Capybara. It is not necessary for Capybara to run but it is very useful for debugging. To learn more about why it's to useful do search for [`save_and_open_page`].

## Database Cleaner

To have consistent tests, it's important to remove any data added to the test database after each test.
I do this with the Database_cleaner gem.

Add a file to `spec/support/database_cleaner.rb`

```ruby
# spec/support/database_cleaner.rb

require 'database_cleaner'

RSpec.configure do |config|

  config.before(:suite) do
    DatabaseCleaner.strategy = :transaction
    DatabaseCleaner.clean_with(:truncation)
  end

  config.around(:each) do |example|
    DatabaseCleaner.cleaning do
      example.run
    end
  end

end
```

## [Pry]

[Pry] is an enhanced version of Ruby's IRB. The [Pry-rails] gem bring Pry to the rails console and interactive debugger environments. Pry is packed with additional features not available in IRB or the Rails Console. While I love it. Your milage may very. Season to taste.

## [Minitest-Reporters]

When I'm using Minitest I also add the `minitest/reporters` gem. I like the enhanced test output. Along with adding the gem it requires a little setup. Add the `Minitest::Reporters.use!` method to the `test_helper.rb` file.

```ruby
# Gemfile

  gem 'pry-rails'

group :test, :development do
  gem 'minitest-rails-capybara'
  gem 'minitest-reporters'
  gem 'launchy'
  gem 'database_cleaner', '~> 1.4.1'
end
```

```ruby
# test_helper.rb

ENV["RAILS_ENV"] = "test"
require File.expand_path("../../config/environment", __FILE__)
require "rails/test_help"
require "minitest/rails"
require "minitest/rails/capybara"

require "minitest/reporters"
Minitest::Reporters.use!(
  Minitest::Reporters::SpecReporter.new,
  ENV,
  Minitest.backtrace_filter
)

class ActiveSupport::TestCase
  fixtures :all
end
```

## Wrapping up

I try to keep this article update, reflecting my current first few steps in creating a Rails application. I'd love to know what you think. Was this helpful to to you? If you got stuck on any pice or have a question leave a comment down below. I want you to be honest and will do my best to update the article to reflect inaccuracies. As much as I value and respect opinions I will remove any comment that are unkind to anyone.

[Bundler]: http://bundler.io/
[Minitest]: https://github.com/blowmage/minitest-rails
[Rspec]: https://github.com/rspec/rspec-rails
[Capybara]: https://github.com/jnicklas/capybara
[pry]: http://pryrepl.org/
[pry-rails]: https://github.com/rweng/pry-rails
[command line options]: https://github.com/guard/guard/wiki/Command-line-options-for-Guard
[save_and_open_page]: http://www.google.com/search?q=save_and_open_page

[companion code base]: https://github.com/kpearson/shop-a-bot
[example Minitest]: https://github.com/kpearson/shop-a-bot/commit/6eba263d985280f7ef1874515f02dc495cb5e5b8
[example Rspec]: https://github.com/kpearson/shop-a-bot/commit/62b7e328935f44cc8a6732421c62dbbc51fb77eb
[example Minitest Capybara]: https://github.com/kpearson/shop-a-bot/commit/6eba263d985280f7ef1874515f02dc495cb5e5b8#diff-f14ca9959ff06f3ca2ee6b9564866954R5
[example Rspec Capybara]: https://github.com/kpearson/shop-a-bot/commit/daa3b225d5aa8188977feb9c0074f59d8e1cb7a5

[partials]: http://guides.rubyonrails.org/layouts_and_rendering.html#using-partials
[form helpers]: http://guides.rubyonrails.org/form_helpers.html
[Routing]: http://edgeguides.rubyonrails.org/routing.html
[factory_girl]: https://github.com/thoughtbot/factory_girl
[Scopes]: http://guides.rubyonrails.org/active_record_querying.html#scopes
[Rspec feature tests]: https://github.com/rspec/rspec-rails#feature-specs
[rails_12factor]: https://github.com/heroku/rails_12factor
[ActionView helpers]: http://api.rubyonrails.org/classes/ActionView/Helpers/TextHelper.html

[Growl]: http://growl.info/downloads
[growl_gem]: https://rubygems.org/gems/growl
[Guard]: https://github.com/guard/guard
[guard-rspec]: https://github.com/guard/guard-rspec
[guard-minitest]: https://github.com/guard/guard-minitest
