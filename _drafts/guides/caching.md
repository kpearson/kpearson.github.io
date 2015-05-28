# Caching Rails

Storing results in memeory using key value store.
Common options Redis and Memcash.

Key based / Digetes Caching
  (Fragment Caching)

Srtategies for how and when to invalidate the cache.

* Manule expieration / On update

  ```ruby
    Rails.cache.delete("thing")
  ```

* Time based

  ```ruby
    Rails.cache.write("key", "val",
                       expires_in: 10.minuets)
  ```

* Digest based

  ```ruby
    "thing_#{t.id} {r.updated_at}"
  ```

## Rails

What to considder

* What am I caching?
* How am I invalidateing it?

`Redis.keys` # Can tests a system.













