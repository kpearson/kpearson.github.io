---
layout: post
title: Rails Sessions, params and flashs, cookies 
excerpt: "http session thru the Ruby on Rails conventions."
tags: [intro, beginner, rails, ruby, ruby on rails, params]
---
Rails Sessions, params alerts travers Rails application via the http requests.

## Sessions

- We can only access the `session` from the controller and the view. *Not from the model*
- Sessions act like a hash. ie.`session[:thing]` but is in fact an object.
  _When stored in a cookie it is a one long string._
- Sessions in rails by default are stored a cookie.  
- The rails session cookie stores a reference to the session.
- Session saved in a cookie are encrypted. (serialized)

## Cookies

- are stored in the browser.
- data is always strings.
- are inherently __NOT__ secure.

## Carts
A cart should be a hash with the key set to item_id and the value is the quantity.

### Cart

`rails generate cart_items controllerr`
Controller has one action.  

```ruby
class CartItemsController < ApplicationController
  def create
    @cart[params[:item_id]] ||= 0
    @cart[params[:item_id]]  += 1

    session[:cart] = @cart

    redirect_to root_path
  end
end
```
The create action has params hash with item_id.
An alternative method raps the hash in an object.

```ruby
class Cart
  attr_reader :data

  def initialize(cart_data)
    @data = cart_data || Hash.new
  end

  def add_item(item_id)
    @data[item_id] ||= 0
    @data[item_id]  += 1
  end

  def count
    @data.values.inject(:+)
  end
```

To have access to the cart in all the controller methods:

```ruby
ApplicationController < ActionController:Base
  ...
  private

  def set_cart
    @cart = session[:cart] || Hash.new
  end
  before_action :set_cart
end
```
set_ methods 

View

```ruby
<%= button_to "link name", cart_items_path(item_id: item)%
```

###Order
The second major component of a cart is the Order.

```ruby
rails generate controller order
```

```ruby
class OrderController < ApplicationController
  def create
    @cart.data
  end
end
```

Order model with a join table named Order_Items connecting Orders and Items.



