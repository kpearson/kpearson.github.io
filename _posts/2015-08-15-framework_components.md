---
title: Framework Components
---

We are fortunate to be building apps in a time of relatively mature, battle tested frameworks. While amazingly powerful and easy to use, the building blocks that make up these frameworks become increasingly difficult to reason about as an app's complexity grows. __One of the key skills to building software within a framework is understanding the role of its components and how they interact__. Here are a few of my go-to tools and techniques for looking at the components of a Ruby on Rails app on their own, with as few dependencies as possible.

## Dealing with Dependencies
When lighting up the individual components of a framework one of the biggest things we can learn is what its dependencies are, i.e., what libraries it relies on to function. At the same time, we also can understand what parts of the framework are not necessary. __It can be very nice to know when it's possible not to require the entire stack and just make available the necessary code.__

## Practical Example
In the case of the Ruby on Rails ecosystem, the libraries that make up Rails engines and Ruby gems used in a Rails application are called components. A Rails engine is like a mini Rails app, with models, views and controllers, rake tasks, etc. Component dependencies are defined in a `.gemspec` file. The test suite will test it in isolation and confirm its dependency structure.

Let's look at consuming a third party API with the Hurley gem. For our purposes the Github API makes for a very clean example. It follows the REST pattern so URLs return what we would expect.

For those of you following along, before we move on, we are going to need to take care of a few things. First, Github requires us to have an [access token](github_api_docs) to make requests of its API. Next, we need a couple gems. The [Hurley] gem so we can explore the API, and Pry, my tool of choice for looking at what's happening inside a Ruby method. Because we are not going to be using a gem file they will need to installed manually.

From the terminal run:

- `gem install hurley`
- `gem install pry`

With the gems installed, let's create a new file (outside of a project directory) with require statements for the gem we just installed at the top.

```ruby
require 'hurley'
require 'pry'

```

Next, we need to create a `Hurley::Client` object with the URL of the API endpoint and assign it to a variable.

```ruby
require 'hurley'
require 'pry'

client = Hurley::Client.new('https://api.github.com/')
client.header[:accept] = 'application/json'

```

With that in place, we can build out different requests in the form of a block, with an API endpoint, the URL, and any additional params we need. In the case of Github, we need at minimum an __app key__ and an __app secret__.

>Never commit app keys or any credentials to version control.

My favorite gem for handling things you want to keep out of version control is [dotenv].

```ruby
require 'hurley'
require 'pry'

client = Hurley::Client.new('https://api.github.com/')
client.header[:accept] = 'application/json'

response = client.get('users/kpearson') do |req|

response = client.get('users/kpearson') do |req|
  req.query['app_key'] = APP_KEY
  req.query['secret']  = APP_SECRET
  req.options.timeout  = 3
end

```

By running this file in the terminal with `ruby file_name.rb` command we start to learn about what to expect from Hurley and the Github API. We confirm Hurley setup and what the API call response looks like.

Sometimes, it may be necessary to fine-tune the request. I find it nice to be able to do this in the terminal without having to change the file and re-run it every time. The way I go about this is to drop in a `binding.pry` to halt execution of the code. Once in, I can quickly try different requests until I have what I need.

```ruby
require "hurley"
require 'pry'

client = Hurley::Client.new("https://api.github.com/")
client.header[:accept] = "application/json"

response = client.get("users/kpearson") do |req|
  req.query["app_key"] = APP_KEY
  req.query["secret"]  = APP_SECRET
  req.options.timeout  = 3
end

binding.pry      # <-------------------
puts response
```

>When using [Pry], the `binding.pry` statement can not be the last thing evaluated. So to get around this in the example above I added a `puts` so the `binding.pry` would do what we expect.

Assigning the request response to a variable and placing the Pry statement after the `get` block is executed lets us play with what is returned and find out how to extract the data we need.

If the Github API wasn't responding as expected, we could place the `binding.pry` before or inside the `get` request block and build up the request by hand. This gives us a chance to make sure we're providing all the params Github needs.

The important thing here is rapid experimentation. By eliminating all but the essential pieces, we can quickly reason about the gem's behavior without wondering how other libraries may be affecting the outcome.

Putting this file together, I had a general idea what the API call should respond to and how to implement Hurley, but I found I learned a lot about the specific needs of both.

## Testing
Clear understanding of the components in an app leads to better, more strategic testing. Knowing what is needed and not needed for a component to work as expected, tells us how and what to stub, and most importantly, what to require. Any time we can leave out Rails when testing, we're going to get faster tests. In this case, with Hurley making API calls, we learned all we need to require is Hurley.

With any new component, gem, or API there are bound to be surprises. The ability to rapidly try things and experiment gives us useful information. By doing this, we gain a much better sense of the edge cases to test for.

## Summing up
Learning to understand the components of our applications outside of their context, gives us useful insight into what they need and how they behave. This helps us to write more artful tests and be more selective about how we manage our dependencies.

[github_api_docs]: https://help.github.com/articles/creating-an-access-token-for-command-line-use/
[dotenv]: https://github.com/bkeepers/dotenv
[Pry]: http://pryrepl.org/
[Hurley]: https://github.com/lostisland/hurley
