---
layout: guide
title: Best in Place | Ruby Gem | Ruby on Rails
---

This is a quick break down of a simple install of __Best in Place__
gem. I let the code snippets do most of the talking with the help of some
commtents indecating the relevent change.

This demo is updating detail and reviews for items.

There is a [companion Rails app on Github][demoapp] to demonstate the code in action.

## Setup

A few things that need to be in place before we go any further.

In the gemfile add:

```ruby
# Gemfile

gem 'best_in_place', '~> 3.0.1'
```

We a document ready Javascript function with css class we are going to be
executing the ajax call from.

```javascript
// app/assets/javascripts/application.js

//= require jquery
//= require jquery_ujs
//= require best_in_place                <-------------- Added
//= require jquery-ui                    <-------------- Added
//= require best_in_place.jquery-ui      <-------------- Added
//= require_tree .

$(document).ready(function() {                  // <---- Added
  /* Activating Best In Place */
  jQuery(".best_in_place").best_in_place();     // <---- Added
});                                             // <---- Added
```

## Implementation

This is a minimal implemintation to get up and running.

### Controller

The crushal thing to note here is the `respond_with_bip()` method.
We get this method from the gem and it needs to look just like this.

```ruby
def update
  @user = User.find params[:id]

  respond_to do |format|
    if @user.update_attributes(params[:user])
      format.html { redirect_to(@user, :notice => 'User was successfully updated.') }
      format.json { respond_with_bip(@user) }
    else
      format.html { render :action => "edit" }
      format.json { respond_with_bip(@user) }
    end
  end
end
```

### View

```erb
<!-- app/views/items/index.html.erb -->

<%= link_to "New Item", new_item_path %>
  <h1>All the items</h1>
<%= div_for @items do |item| %>
  <p><%= best_in_place item, :name, :as => :input %></p>
  <p><%= best_in_place item, :description, :as => :input  %></p>
<% end %>
```

The corresponding controller is the same as above.

### Nested Route

By nesting the `item_review` routes we get have accsess to the id of the item
without having to pass it in a param in the form.

```ruby
# config/routes.rb

Rails.application.routes.draw do
  root to: "items#index"

  resources :items do
    resources :item_reviews, only: [:new, :create, :update]
  end

...
```

... and then we modify our form with the `:url` option.
>The `:url` option gives us the ability to set the destination route for
the ajax call.

```erb
<!-- app/views/items/show.html.erb -->

<h1> <%= @item.name %></h1>
<p> <%= @item.description %></p>
<h2>Reviews</h2>
<%= div_for @item.item_reviews, class: "review" do |review| %>
  <p>
  <%= best_in_place review, :title,
    :url => item_item_review_path(@item, review),
    :as => :input %>
  </p>
  <p><%= review.body %></p>
<% end %>
<%= link_to "Write a Review", new_item_item_review_path(@item) %>
```

[demoapp]: https://github.com/kpearson/shop-a-bot/tree/best_in_place/
