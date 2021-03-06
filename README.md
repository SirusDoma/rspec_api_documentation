[![Build Status](https://travis-ci.org/zipmark/rspec_api_documentation.svg?branch=master)](https://travis-ci.org/zipmark/rspec_api_documentation)
[![Code Climate](https://codeclimate.com/github/zipmark/rspec_api_documentation/badges/gpa.svg)](https://codeclimate.com/github/zipmark/rspec_api_documentation)
[![Inline docs](https://inch-ci.org/github/zipmark/rspec_api_documentation.svg?branch=master)](https://inch-ci.org/github/zipmark/rspec_api_documentation)
[![Gem Version](https://badge.fury.io/rb/rspec_api_documentation.svg)](https://badge.fury.io/rb/rspec_api_documentation)

# RSpec API Doc Generator

Generate pretty API docs for your Rails APIs.

Check out a [sample](http://rad-example.herokuapp.com).

## Fork Note

This fork focus on [API Blueprint](https://apiblueprint.org) format to fix and improve api blueprint generator.  
Refer to [original repository](https://github.com/zipmark/rspec_api_documentation) for instruction for other formats.

You can use original functions for `:api_blueprint` that inherited from original implementation.  
In addition, you can also put comment on your route and action section via `route_summary` and `action_summary`:

* `route`: APIB groups URLs together and then below them are HTTP verbs.

  ```ruby
  route '/orders', 'Orders Collection' do
    route_summary 'Collection of orders API, put your summary here if neccessary!'

    get 'Returns all orders' do
      action_summary 'Be aware of something that might unexpected, explain here!'
      # ...
    end

    delete 'Deletes all orders' do
      # ...
    end
  end
  ```

* `attribute`: APIB has attributes besides parameters. Use attributes exactly
  like you'd use `parameter` (see documentation below).

You can also use http verb function directly to instead of using `route` now, which seems doesn't work in original implementation.  

Additionally, you may specify route name and action name altogether. The preceeding tests will inherit route name as long as it share same route, so you don't have to repeat route name setting.  

For `parameter`, you can define inside the action, it will rendered inside the action section instead of resource section

```ruby
get '/orders', route: 'Orders Collection', action: 'Returns all orders'  do
  # Note that, only first occurence `route_summary` that will take into account
  route_summary 'Collection of orders API, put your summary here if neccessary!'
  action_summary 'Be aware of something that might unexpected, explain here!'

  # ...
end

# `route: 'Orders Collection'` is not needed unless you want to assign it to another route name
delete '/orders', action: 'Deletes all orders' do
  # `route_summary` will also inherited and it's NOT overridable

  # ...
end

patch '/orders/:id', route: 'Order detail', action: 'Update an order' do
  route_summary 'API to manipulate specific order, put your summary here if neccessary!'
  parameter :id, 'The id of order that will be updated', required: true, type: :integer
  
  # ...
end

# The route will be set to 'Order detail'
delete '/orders/:id', action: 'Delete an order' do
  # Original implementation has problem where parameters is placed inside resource section which cause second call have no effect
  # Now this will be rendered inside action section instead so you can put unique comment and options for each action
  parameter :id, 'The id of order that will be deleted', required: true, type: :integer

  # ...
end
```
