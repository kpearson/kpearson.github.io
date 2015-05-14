Aral objects
(Active Recored Relation Object)
Arel quaries are lazy.  They do not run untill needed.


Find
```
User.find_by(first_name: 'Jimi')
User.where(first_name: 'Jimi').take
```

Where
```
Item.where(["release_date <= ? AND active = ? ",
Time.now, true])
```

Join
```
User.joins(:items).where({ items: { active: true } })
```

Order
```
Client.order(:created_at)
```

Limit and Offset
in this case order does not matter.
```
Client.limit(5).offset(30)
```

Find_each
```
User.find_each do |user|
  NewsMailer.weekly(user).delivery_now
end
```

Includes
```
users = User.limit(10)

users.each do |user|
  puts user.address.postcode
end

users = User.includes(:address.limit(10)
```

Indices (Indexing)

Improve perfomeance for lookups.
Has an increesed cost

```
.to_sql
.explane show the quarey to be generated
```

Scopes take lambdas

```
scope :complete

```

Default Scoop

```
class Item < ActiveRecord::Base
  default_scope(


```

.unscoped

```
