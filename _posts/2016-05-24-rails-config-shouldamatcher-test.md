---
layout: post
title:  "[Rails] Config shoulda-matchers use to test in Rails"
comments: true
date:   2016-05-26 15:07:33 +0700
categories: rails
---

## Hey!

Shoulda Matchers provides RSpec- and Minitest-compatible one-liners that test common Rails functionality. These tests would otherwise be much longer, more complex, and error-prone.

Take the look [Shoulda Matchers] [shoulda-matchers] to know detail

## Config

In Gemfile at folder group test add shoulda-matchers

```
group :test do
  gem 'shoulda-matchers', github: 'thoughtbot/shoulda-matchers'
end


In folder spec you create folder support

```
mkdir spec/support
```

We will touch to create shoulda-matcher.rb to setup shoulda matchers in this file

```
Shoulda::Matchers.configure do |config|
  config.integrate do |with|
    # Choose a test framework:
    with.test_framework :rspec

    # Choose the following
    with.library :rails
  end
end
```

### Alright Done, I think hehe ^_^

[shoulda-matchers]: https://github.com/thoughtbot/shoulda-matchers
