
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

## Unicorn
//TODO: Unicorn in development.

## Rspec

### Feature test ([Capybara])
Add a the named spec file in `spec/feature` directory.
In the top of the new spec file:

{% highlight ruby %}
require 'spec_helper'
{% endhighlight %}

[Rspec feature tests] docs

## Capybara

With `gem  'capybara'` in the `Gemfile` and bundled.
Add `require 'capybara/rails'` to
- if Rspec `spec/rails_helper.rb`
- if Minitest `test/test_helper.rb`
Add `include Capybara::DSL` to the given *integration* test file.

*Overview of DSL*

- have_content '__'

## Heroku

#### Commands

From with in the project dir.
`Heroku create <app_name>`

Rename command:
`Heroku apps:rename <new_name>`

Create and Migrate DB:
`heroku rake db:create`

`heroku rake db:migrate`

prep app for Heroku
`rake assets:precompile`

Undo precompile
`rake assets:clobber`

#### Utilities

[rails_12factor] gem
12factor is gem created by [Heroku] to override any setting nesesarry to host an on Heroku.

{% highlight ruby %}
gem 12factor
{% endhighlight %}

#### Confiraration

{% highlight ruby %}

# Disable serving static files from the `/public` folder by default since

## Apache or NGINX already handles this.
config.serve_static_files = ENV['RAILS_SERVE_STATIC_FILES'].present?

# Compress JavaScripts and CSS.
config.assets.js_compressor = :uglifier
# config.assets.css_compressor = :sass

# Do not fallback to assets pipeline if a precompiled asset is missed.
config.assets.compile = false
{% endhighlight %}



## seccret environmental veriable
`rake TODO

## dotenv
With `gem 'dotenv'` in the `Gemfile` and bundled.
Create a `.env` file in the application root.
Add secrets to `.env` file.

```ruby
MAPS_KEY=<key>
```

In the `secrets.yml` file
add the secret to each development environment.

Example:

```ruby
development:
secret_key_base: 51c002fa4674c4ac46cfdb2fb5360bbf535f5737b03c6c496baf0d7d86e088acf8f44f114bb1dcab58d6dc5e0e7b789c966ad8018dd20724deb23570fc0ef66b
maps_key: <%= ENV["MAPS_KEY"] %>

test:
secret_key_base: 674d62375c0a9548dea1b5b5608cfdf56cb400fe9d536f1f501eb9313c072eba3bfab22769514920ef6f7ed458f0f07a629530709e55df06e94fb970835b5874
maps_key: <%= ENV["MAPS_KEY"] %>

production:
secret_key_base: <%= ENV["SECRET_KEY_BASE"] %>
maps_key: <%= ENV["MAPS_KEY"] %>
```

## Controllers
CRUD standards and best practices.
### before_actions

## Models

### Validations
Model validation syntax:

```ruby
class Article < ActiveRecord::Base
  belongs_to :articles
  validates  :title, presence: true,	length: { minimum: 5 }
end
```
## Sessions
What is where and what things look like.
A session is lives in the browsers momory when parts of it ar esaved they are saved in a cookie

Cookies can not be shard

saving a shopping cart in a cookie would intail storing the item id and and user id.

Rookie stored carts are more fraginle but more consistantly stored. db carts require db interactions.

### Authentication
TODO:

### Authorization
TODO:

#### Cancancan
_Often over kill. If two or fewer logedin user rolels are called
 for a hand crafted soultion is probably the way to go._

Generate the Ability class with:
{% highlight ruby %}
rails g cancan:ability
{% endhighlight %}

or just create the class by hand:
_class based authorization_
{% highlight ruby %}
cass Ability
  include CanCan::Ability

  def initialize(user)
    if user.class == Admin
      can [:read, :update, :destroy], Admin, id: user.id # user of class Admin can only view and update her self
      can [:new, :create, :destroy], Admin    		       # user of class Admin can create and destroy
      can :manage, Article                               # user of class Admin can perform any action on the article
    elsif user.class == User
      can :read, User, id: user.id                       # user of class User can view their own profile
  end
end
{% endhighlight %}

_role based authorization_
{% highlight ruby %}
class Ability
  include CanCan::Ability

  def initialize(user)
    user ||= User.new # guest user (not logged in)

      can :manage, :all
    else
      can :read, :all
    end
  end
end
{% endhighlight %}

Implement authorization in the controller action
{% highlight ruby %}
def show
  @article = Article.find(params[:id])
  authorize! :read, @article
end
{% endhighlight %}

Implement view conditional
{% highlight ruby %}
<% if can? :update, @article %>
  <%= link_to "Edit", edit_article_path(@article) %>
<% end %>
{% endhighlight %}


## [Routing]
  Standards and options.
## Params

  /TODO


## Views
    dont do calculations in the view.
    dont quary the db in the view.

### Scopes

					[Rails guides scopes][Scopes]

					Scopes are like class methods.

{% highlight ruby %}
class Article < ActiveRecord::Base
scopre :current, -> { where(current: true) }
end
{% endhighlight %}

### Partials

[Rails guides partials][partials]

{% highlight ruby %}
<%= render partial: "form" %>
{% endhighlight %}

##### Local Variables
Passing Local Variables into partials:

{% highlight ruby %}
<%= render partial: "form", locals: {zone: @zone} %>
{% endhighlight %}

A partial can use its own layout file, just as a view can use a layout. For example, you might call a partial like this:

{% highlight ruby %}
<%= render partial: "link_area", layout: "graybar" %>
{% endhighlight %}

This would look for a partial named _link_area.html.erb and render it using the layout _graybar.html.erb. Note that layouts for partials follow the same leading-underscore naming as regular partials, and are placed in the same folder with the partial that they belong to (not in the master layouts folder).

You can also pass in arbitrary local variables to any partial you are rendering with the locals: {} option:

{% highlight ruby %}
<%= render partial: "product", collection: @products,
	as: :item, locals: {title: "Products Page"} %>
{% endhighlight %}

##### Shared Partials

{% highlight ruby %}
<%= render "shared/menu" %>
{% endhighlight %}

That code will pull in the partial from app/views/shared/_menu.html.erb.

###Forms


{% highlight ruby %}
<%= form_for :article do |f| %>
<p>
<%= f.label :title %><br>
<%= f.text_field :title %>
</p>

<p>
<%= f.label :text %><br>
<%= f.text_area :text %>
</p>

<p>
<%= f.submit %>
</p>
<% end %>
{% endhighlight %}

###Helper Methods

[ActionView helpers]

[form helpers]

##### Custom Helper
Custome live in the helpers dir in app root.

To use:
{% highlight ruby %}
class ArticlesController < ApplicationController
include ArticlesHelper
end
{% endhighlight %}

###Flash Messages


//TODO

### Links

{% highlight ruby %}
<%= link_to "GOOGLE", "http://www.google.com", target: "_blank", class: "btn btn-large btn-primary" %
{% endhighlight %}


`button_to`


### View assets path
from with in the console run:
`y Rails.application.config.assets.paths`

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
