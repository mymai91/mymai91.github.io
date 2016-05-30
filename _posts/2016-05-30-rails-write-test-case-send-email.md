---
layout: post
title:  "[Rails] Write test case send email"
comments: true
date:   2016-05-30 15:07:33 +0700
categories: rails
---

To write test we will use a gem called [email_spec][emailspec]

```
group :test do
  gem "email_spec"
end
```

To install

```
bundle install
```

### Config

At `spec/spec_helper.rb` add code below:

```
require "email_spec"

RSpec.configure do |config|
.
.
.
config.include(EmailSpec::Helpers)
config.include(EmailSpec::Matchers)
.
.
.
end
```

### Test
In pre post [Config send email with MailChimp][sendmail] we created `order_mailer` Hence, I will write test case for `order_mailer`. Ah, of course you can use this way to test for send mailer default from Rails

Create a file:
```
touch spec/mailers/order_mailer.rb
```

I will test for cases:
  * Email deliver_from
  * Email deliver_to
  * Email subject

**You can see detail at [email_spec][emailspec] to know more case to test. Because in this example I send email via MailChimp, so the content email it not exist in our app. So We can't touch on it to test**

```
require 'spec_helper'

describe OrderMailer do
  describe 'send welcome email' do
    before do
      @order = FactoryGirl.create :order
      @order_mailer = OrderMailer.welcome(@order.id)
    end

    it 'should be set to be send from noreply@mymaith.com' do
      expect(@order_mailer).to deliver_from('MyMai <noreply@cosplaywon.com>')
    end

    it 'should be set to be send to order email' do
      expect(@order_mailer).to deliver_to(@order.email)
    end

    it 'should have the correct subject' do
      expect(@order_mailer).to have_subject(/Welcome to our awesome app!/)
    end
  end
end

```

### Run test case

Just invoke on terminal:

```
# To run all test case
rspec spec

# To run test case for order_mailer
rpsec spec/mailers/order_mailer.rb
```

### Alright Done, I think hehe ^_^

[mandrill]: https://mandrillapp.com/login/?referrer=%2F
[sendmail]: /rails/2016/05/30/rails-config-send-email-with-mail-chimp.html
