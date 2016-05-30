---
layout: post
title:  "[Rails] Config send email with MailChimp"
comments: true
date:   2016-05-30 15:07:33 +0700
categories: rails
---

### Config send email with MailChimp

**Use MailChimp like a service use to send email**

In the past to send email with MailChimp we have to use [Mandrill][mandrill] like a service to send. But now rely on quote on mandrillapp.com "On April 27, Mandrill became a paid MailChimp add-on."
For now to send email we will create MailChimp account and use MailChimp account login Mandrill

### Create template
After login, we will access to templates and touch on create templates button

After create templates successful. Send templates to mandrill

### Verify account

After login Mandrill by MailChimp account. Choose Domain to verify account

### Setup to send email

In Gemfile we will add `mandrill-api` to help send email via mandrill ``

```
# Send email
gem 'mandrill-api'
```

In the file manage `ENV` variables we will config ENV variables

```
SMTP_ADDRESS=smtp.mandrillapp.com

SMTP_DOMAIN=yourdomain

SMTP_PASSWORD=mandrill_api_key

SMTP_USERNAME=mandrill_username
```

In the file `config/environments/development.rb` or `config/environments/production.rb`

```
Rails.application.configure do

  config.action_mailer.smtp_settings = {
    address: ENV.fetch("SMTP_ADDRESS"),
    authentication: :plain
    domain: ENV.fetch("SMTP_DOMAIN"),
    enable_starttls_auto: true,
    password: ENV.fetch("SMTP_PASSWORD"),
    port: "587",
    user_name: ENV.fetch("SMTP_USERNAME")
  }
  config.action_mailer.default_url_options = { host: ENV["SMTP_DOMAIN"] }
end
```

### Create mailer at Rails app

I assume I will send email when in case order something. I will create order_mailer

```
$ rails g mailer order_mailer
```

Rails will auto build mailers folder then we will config:

In `app/mailers/application_mailer.rb`:

```
class ApplicationMailer < ActionMailer::Base
  default from: 'noreply@mymaith.com'
  layout 'mailer'
end
```

Create file `app/mailers/base_mandrill_mailer.rb` and then in this file

```
require 'mandrill'

class BaseMandrillMailer < ActionMailer::Base
  default(
    from: 'MyMai <noreply@mymaith.com>'
  )

  private

  def send_mail(email, subject, body)
    mail(to: email, subject: subject, body: body, content_type: 'text/html')
  end

  def mandrill_template(template_name, attributes)
    mandrill = Mandrill::API.new(ENV['SMTP_PASSWORD'])

    merge_vars = attributes.map do |key, value|
      { name: key, content: value }
    end

    mandrill.templates.render(template_name, [], merge_vars)['html']
  end
end

```
Let me introduce the way to bind variables to template email

```
merge_vars = {
  'ORDER_URL' => "#{ENV['CONFIRMATION_LINK']}?confirmation_token=#{order.confirmation_token}"
}
```
Example:

This is your template email

```
<p>Your order <a href=“*|ORDER_URL|*" target="_blank">here</a></p>
```

In the file `app/mailers/order_mailer.rb`

```
class OrderMailer < BaseMandrillMailer
  def welcome(order_id)
    order = Order.find(order_id)
    subject = 'Welcome to our awesome app!'
    merge_vars = {
      'ORDER_URL' => "#{ENV['CONFIRMATION_LINK']}?confirmation_token=#{order.confirmation_token}"
    }
    body = mandrill_template('template_demo', merge_vars)
    send_mail(order.email, subject, body)
  end
end
```

### Add layout at Rails app
Here is the part is equally important

Create this file `app/views/layouts/mailer.html.erb`
Add content for mailer layout

```
<%= yield %>
```

### Send mail

In the controller you wanna apply send email

Just invoke

```
OrderMailer.welcome(order_id).deliver
```

### Alright Done, I think hehe ^_^

[mandrill]: https://mandrillapp.com/login/?referrer=%2F
