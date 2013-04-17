---
layout: post
title: "Basic Ruby/Rails Commands"
category: Ruby
tags: [tutorial, Ruby]
---
{% include JB/setup %}

#### See which version of Rails you're using
    > rails -v

#### See which version of Ruby you're using
    > ruby -v

#### See which version of Ruby Gems you're using
    > gem -v

#### See all the Rake commands
    > rake -T

#### See all FIXME, TODO, OPTIMIZE notes in code comments
    > rake notes:fixme      # shows all #FIXME comments
    > rake notes:todo       # shows all #TODO comments
    > rake notes:optimize   # shows all #OPTIMIZE comments

#### Testing
    > rake test
    > COVERAGE=true rake test # Test coverage report can be found at /coverage/index.html

#### Use bundle exec rake instead of plain rake
Use `bundle exec rake` instead of plain `rake` so that the gems specified in your Gemfile are used
instead of the gems installed system-wide.