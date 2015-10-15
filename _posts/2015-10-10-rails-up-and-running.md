---
layout: post
keywords: [ruby, rails, rspec, minitest-rails]
---

*Ruby 2.1.4 | Rails 4.2.3 | Sep 2015*

When I begin a new Ruby on Rails project, there are a few thing I always like to have in place from the start. This post covers my initial setup, go-to gems, and what they do. I've created a [companion code base] on Github to share everything that is in this article, so you can play with the code in a working application. Just make sure to match up the branch which corresponds to the example.

## Prerequisites

- [RVM] \(Ruby Version Manager) or [rbenv] \(simple Ruby version management) runtime environments
- [Bundler] a gem/extension manager

## Create a new Rails app

```bash
rails new <app_name> -T --database=postgresql --skip-turbolinks
```

This gives us a new Rails app with a few options in place that I find useful for every application.

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

My next step is to add a few gems I know I'm going to use in the initial setup. I'll go over them in more detail later, as we put them to use. I also remove all the comments and organize the Gemfile a little.

### Example Gemfile ([Rspec])

```ruby
gem 'pry-rails'

group :test, :development do
  gem 'rspec-rails'
  gem 'capybara'
  gem 'launchy'
  gem 'database_cleaner', '~> 1.4.1'
end
```

### Example Gemfile ([Minitest])

This is how I approach setting up [Minitest] in a Rails application.  This is specific to the `minitest-rails` gem. Rails comes with Minitest built in, however setting up an app to make use of Minitest without the the gem requires slightly different code.

```ruby
# Gemfile

gem 'pry-rails'

group :test, :development do
  gem 'minitest-rails-capybara'
  gem 'launchy'
  gem 'database_cleaner', '~> 1.4.1'
end
```

>Note: The Minitest-rails gem is a dependency of `minitest-rails-capybara` so I don't need to list it in the `Gemfile`. If you prefer, you can include it to remind you that it's included.

Run `bin/bundle` with test framework chosen, and gems in place.

## Testing Framework Setup

Now that we have the gems in place, we need to do the initial setup.

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

At this point I remove the comments. Here is the [example Rspec] implementation on Github.

### [Minitest] Setup

The `minitest-rails` gem gives us a generator.

```bash
rails generate minitest:install
```

This will overwrite the current `/test/test_helper.rb` file created by the `rails new ...` command. I remove the comments in the new file and I end up with something that looks like:

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

### Unit Testing

At this point we are all set up for unit testing. The test framework knows about itself and knows which directory to look in for test files. That will allow us to run the Rails generators and get the corresponding test files.

Next we'll get integration and feature testing up and running.

## [Capybara] Setup

The capybara gem is a very common way to implement integration/feature testing. By using a _headless browser_ to navigate the app, Capybara allows testing of elements on the page that change based on the input. So when you click the logout button, you ras expert the page to display "Good bye {user name}".

To get Capybara up and running in our test framework we need to add the gem(s) and make the gem available.

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

Capybara and Rspec play very nicely together. If you place your feature tests in the `spec/features` directory, Rspec will automatically know what type of test to run and you won't need to specify the type.

>A word of caution. This convenience can trip you up if your not aware of it. When you name the feature test directory anything other than `spec/features` (i.e. `spec/feature`), Capybara requires you to explicitly declare the type for each example group with `:type => :feature`

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

I approach [Minitest] a little differently from the Rspec example. I use the `minitest-rails-capybara` gem to bring in the `minitest-rails` gem as a dependency, so we have nothing to change in the `Gemfile` at this point.

In the `test/test_helper.rb` we need to add `require "minitest/rails/capybara"`.

At this point I have a `test_helper.eb` that looks like this:

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

Minitest integration tests live in `test/integration`. This folder is automatically created with `rails new ...` when you don't include the `-T` option.

### Launchy

When using either Minitest or Rspec the __Launchy gem__ is something I always use in conjunction with Capybara. It is not required for Capybara to run, but it is very useful for debugging. To learn more about why it's to useful do a search for [`save_and_open_page`].

## Database Cleaner

I use the Database_cleaner gem to help maintain a clean database after each test:

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

[Pry] is an enhanced version of Ruby's IRB (a Ruby shell). The [Pry-rails] gem brings Pry to the Rails console and interactive debugger environments. Pry is packed with additional features not available in IRB or the Rails Console. While I love it, your milage may vary. Season to taste.

## [Minitest-Reporters]

When I'm using Minitest I also add the `minitest/reporters` gem. I like the enhanced test output. Along with adding the gem, it requires a little more setup than the other gems we've added so far. Add the `Minitest::Reporters.use!` method to the `test_helper.rb` file.

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

I try to keep this article updated, to reflect my current Rails application set-up. I'd love to know what you think and what you use.

Was this article helpful to to you? If you got stuck on any piece or have a question, leave a comment down below.

I want you to be honest and will do my best to update this article to keep it fresh and free of bugs.

And, hey!  Keep it kind.  As much as I value and respect opinions, I will remove any comment that are unkind to anyone.

[RVM]: https://rvm.io
[rbenv]: http://rbenv.org
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
