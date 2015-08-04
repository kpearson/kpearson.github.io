---
layout: post
title: Components
category: article
---
We are fortunate to be building app in a time of relatively mature battle
tested frameworks. While amazingly powerful and easy to use, the building blocks
that make up these frameworks can become increasingly difficult to reason about
as complexity grows and requirement of the app push the capabilities of the
framework. __One of the key skills to building software within a framework is
understanding the role of its components and how they interact.__ Having a few
tools and techniques to break apart the layers and look at the components in
isolation with as few dependencies as possible is can make all the difference
to understanding what they do and what they offer.

In the case of the Ruby on Rails ecosystem, the libraries that make up Rails engines and Ruby gems used in a Rails application are called components. A rails engines is like a mini Rails app, with models, views and controllers, rake tasks etc. Component dependencies a defined in `.gemspec` file, test suite will test it in isolation and confirm its dependency structure.

## Example time

When consuming an api we need two things: An external api to hit and to employ some kind of http request adapter. Let’s start by taking a look at the Github api. Github api requires us to have a site to make requests. Github's api is well built and follows REST patterns so uri's return what you expect.

Before we move on we are going to need to have a few gems installed.
First the Hurley gem: `gem install hurley`.
Next, my tool of choice for looking at what’s happening inside a ruby method is the Pry gem: `gem install pry`.
and finally the JSON gem: `gem install json`

Create a new file (outside of a project directory). at the top of the file require in the gems we just installed. Then based on the Hurly documentation, create a `Hurley::Client` object with the url of the api. With that in place we create a block with the RESTful url and any addition params we need. With Github we need at minimum a app_key and app_secret.

```ruby
<code snippet>
```

Running this with in the terminal with the command `ruby file_name.rb` we can learn several things. How Hurley needs to be stup and what the api call looks like. As I put this file together I had a general idea what the api should respond to from the docs and how to implement Hurley but I found things need some tuning.

The way I go about this is to drop in a `binding.pry` to halte execution of the file.

```ruby
<!--code-->
```

Once in the pry session we can do all sorts of amazing things. Notice where I place the pry statement, after the `get` block also assigning the result of the get request to variable let us play around with it to find out how to extract the data we need. The puts is there because pry can't be the last thing evaluated. If Github wasn't responding the as expected we could place it before or inside the get request and build up the request making sure Github is getting the needed params. The important thing here is rapid experimentation. By eliminating all but the essentials pieces we can reason about the gem and its characteristics very rapidly without questions about how Rails may be affecting the outcome.

## Testing
Clear understanding of the components in an app leads to a better mode strategic testing. Knowing what is needed and not needed for a component to work as expected tells us how and what to stub and most importantly what to require. Any time we can leave out rails when testing is going to give us faster tests, in the case with Hurley making api calls we learned all we need to require is the Hurley and json.

Inevitably with any new gem or api there are bound to be surprises. By keeping track the things I learn along the way I gain a much better sense of the edge cases to test for.

## Putting it all together
