---
layout: post
title: Carrierwave Gem
---

Add the `uploaders` directiory to the asset pipe line loadpath in config/application:
```ruby
#config/application.rb

config.autoload_paths += "#{Rails.root}/app/uploaders"
```
