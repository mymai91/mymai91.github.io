---
layout: post
title:  "[Rails] Customizing menu item at active admin"
comments: true
date:   2016-06-02 15:07:33 +0700
categories: rails
---

If you want so a page manage the Post in the app (mean that manage Post model) we will create a resource

```
rails g active_admin:resource Post
```

The ActiveAdmin will build for app a file `app/admin/post.rb`

```
ActiveAdmin.register Post do
  # This is the where you define any things you want for post admin
end
```

Post model including fields:

* id
* title
* photo
* address
* description
* latitude
* longitue
* total_view
* total_comment
* total_like
* create_at
* updated_at

The view active_admin after build will like:

![screen shot 2016-06-02 at 10 38 19 am](https://cloud.githubusercontent.com/assets/6791942/15733040/68f8ae1a-28ac-11e6-9fdc-63509fb4752b.png)

So for now client require that they just need to manage fields:

* id
* title
* photo
* address
* description
* total_view
* total_comment
* total_like
* create_at
* updated_at

How we can remove 2 fields... hmmmhhhh

At file `app/admin/post.rb` we will update block `ActiveAdmin.register Post`

We limit field by the way define fields in **index** block

```
ActiveAdmin.register Post do
  index do
    column :id
    column :title
    column :photo
    column :address
    column :description
    column :total_view
    column :total_comment
    column :total_like
    column :create_at
    column :updated_at
  end
end
```

Look the picture below:

![screen shot 2016-06-02 at 10 39 19 am](https://cloud.githubusercontent.com/assets/6791942/15733240/33dba514-28ae-11e6-8f35-c4df1762d7e4.png)

You can see that it is removed

#### If you wanna add action View - Edit - Delete. How you can do it?

Like above just add one more line is : 'actions'

```
ActiveAdmin.register Post do
  index do
    column :id
    column :title
    column :photo
    column :address
    column :description
    column :total_view
    column :total_comment
    column :total_like
    column :create_at
    column :updated_at
    actions
  end
end
```

You can see actions  View - Edit - Delete are added

![screen shot 2016-06-02 at 10 40 19 am](https://cloud.githubusercontent.com/assets/6791942/15733360/779a8260-28af-11e6-9e43-34ff9cf97460.png)

### Alright Done, I think hehe ^_^

[sendmail]: /rails/2016/05/30/rails-config-send-email-with-mail-chimp.html
