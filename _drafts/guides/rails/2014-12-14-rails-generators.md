---
layout: guide
category: guides
---

*osx 10.10.1 | Jan 15, 2015*
## Rails Generators Refference.
A quick refference to syntax, pluralization and options for Rails generators.

## [Migrations]

### Creating Tables
If the migration name is of the form "CreateXXX" and is followed by a list of column names and types then a migration creating the table XXX with the columns listed will be generated. For example:
`bin/rails generate migration CreateProducts name:string part_number:string`
generates:
```ruby
class CreateProducts < ActiveRecord::Migration
  def change
    create_table :products do |t|
      t.string :name
      t.string :part_number
    end
  end
end
```

### Changing / Modifing Tables

#### Adding Columns
Generate an empty but corectly named migration file:
`bin/rails generate migration AddPartNumberToProducts`

Include a list of column names and types:
`bin/rails generate migration AddPartNumberToProducts part_number:string`

will generate:
```ruby
class AddPartNumberToProducts < ActiveRecord::Migration
  def change
    add_column :products, :part_number, :string
  end
end
```

Optionaly add an index on the new column, you can do that as well:
`bin/rails generate migration AddPartNumberToProducts part_number:string:index`
will generate:
```ruby
class AddPartNumberToProducts < ActiveRecord::Migration
  def change
    add_column :products, :part_number, :string
    add_index :products, :part_number
  end
end
```

#### Removing Columns
Generate a migration for removing a column:
`bin/rails generate migration RemovePartNumberFromProducts part_number:string`
generates:
```ruby
class RemovePartNumberFromProducts < ActiveRecord::Migration
  def change
    remove_column :products, :part_number, :string
  end
end
```

## Models

Capitolize, Singular model name.

```ruby
bin/rails generate model Article title:string text:text
```

Now with an assosiation. Notice the __`article:references`__.

```ruby
bin/rails generate model Comment commenter:string body:text article:references
```

Generates:

- Migration file (To create the corresponding database table.)
- Model file (The Comment model with the `belongs_to :article` line.)
- Corisponding Test file

## Controllers

>Note: I usualy build controllers by hand. But for the times do use the generateor.

Capitolize and (almost always)pluralize controller name.

```shell
bin/rails g Users
```

The controller generater takes two types of agruments.
first, controller actions like index, new, and create. For each of these the
Rails generator give us a corosponding action, route and view.
I nearly always rebuild the route using the `resource` syntax.

Options list:
`--no-` followed by `optionname`:
`--no-helper`
`--no-assets`
`--np-controller-specs`
`--no-view-specs`


Example with actions:

```shell
bin/rails g Users index new create
```

```ruby
# app/controllers/users.rb

class UsersController < ApplicationController
  def index
  end

  def new
  end

  def create
  end
end
```

and

```ruby
# config/routes.rb

Rails.application.routes.draw do
  get 'users/index'

  get 'users/new'

  get 'users/create'

end
```

[Migrations]: http://edgeguides.rubyonrails.org/active_record_migrations.html
