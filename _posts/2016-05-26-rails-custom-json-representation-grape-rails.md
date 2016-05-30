---
layout: post
title:  "[Rails] Custom json representation grape rails"
comments: true
date:   2016-05-24 15:07:33 +0700
categories: rails
---

## Hey!

Currently, you are using grape to write APIs

```
gem 'grape'
gem 'grape-roar'
gem 'grape-swagger-rails'
```

Create an API with method GET

```
get '/products' do
  present Product.all, with: ProductsRepresenter
end
```

In this case easy, when it success it will return **list products**

But if you don't want return **list products** you only want
  * return a message success when it **successful**
  * return a message fail when it **failed**

### How you custom it?

I will show you the way:

To a case **successful** you will present the message

```
get '/products' do
  present :success, 'Get list products success'
end
```

To a case **failed** you will custom the error:

```
get '/products' do
  error!({ error: 'This is wrong behavior' }, 422)
end
```

### Alright Done, I think hehe ^_^
