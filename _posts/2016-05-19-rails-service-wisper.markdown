---
layout: post
title:  "[Rails] Write service object in Ruby use Wisper"
comments: true
date:   2016-05-19 15:07:33 +0700
categories: jekyll update
---

## Hey! Tell me know What is "[Wisper][wisper_link]"?

Base on the theory "[Wisper][wisper_link]" said on site. This is a _A micro library providing Ruby objects with Publish-Subscribe capabilities_

Basically, It easy to approach. Imagine that, we have a service it will broadcast event and the app can listen this event.

I guess you are thinking that What the hell with that? I don't understand what you said.
So, Don't hasty. Read continue you to know detail.

<!-- Image here -->

## Usage

We have to install this gem first

**Installation**

Add `gem 'wisper', '2.0.0.rc1'` to your app's Gemfile

### Broadcast event
**Implement**
To implement `broadcast event` at any where you wanna broadcast include module

`include Wisper::Publisher`

Look example, I will explain detail about it:

```
module Food
  class Service
    include Wisper::Publisher

    # Execute food_detail service
    # food not nil
    # @broadcast event find_post_by_id_successful
    # food is nil
    # @broadcast event find_post_by_id_failed
    def execute(post_id)
      food = find_food_by_id(food_id)
      if food.nil?
        broadcast(:find_food_by_id_failed)
      else
        broadcast(:find_food_by_id_successful, food)
      end
    end

    # Find food by id
    # @return food_object
    def find_food_by_id(id)
      Food.find_by_id(id)
    end
  end
end
```

When I add `include Wisper::Publisher` at **module Food** It will tell that:
I will broadcast event about Food at here.

We find a food. There are 2 cases:

  * Found a food => food object != nil
  * Not found a food => food object == nil

Base on the case it will broadcast event:

  * If food != nil the service will broadcast event `find_food_by_id_successful`
  * If food == nil the service will broadcast event `find_food_by_id_failed`

Food service is created. And now, we can use it any time & any where you want

### Listening event is broadcast

At FoodsController we wanna listen the event broadcast at method `show`
We will init the service at FoodsController by this way `Food::Service.new`

```
class FoodsController < ApplicationController

  def show
    food_service = Food::Service.new

    food_service.on(:find_food_by_id_successful) do |result|
      render json: result, status: 200
    end

    food_service.on(:find_food_by_id_failed) do
      render json: { message: 'not found'}, status: 200
    end

    # Call method excute in FoodService
    food_service.execute(params[:id])
  end
end
```

We listen even `broadcast` via `.on`. When service broadcast `broadcast(:find_food_by_id_failed)`
The controller will listen event by this way`on(:find_food_by_id_failed)`

### How about test for Wisper

We will use "[wisper_rspec][wisper_rspec]" to write test case?

Which case we will define to test? For me, we just need test service broadcast an event, the rest `listen` event
we will test at where you `listen` event. In this case we listen at `controller`.

**Test for service**

```
require 'wisper/rspec/stub_wisper_publisher'

describe PostDetail::Service do
  describe 'broadcast matcher' do
    before do
      @post_detail_object = FactoryGirl.create :post
    end

    let(:post_detail_service) { PostDetail::Service.new }

    it 'broadcast #find_post_by_id_successful' do
      expect do
        post_detail_service.execute(@post_detail_object.id)
      end.to broadcast(:find_post_by_id_successful, @post_detail_object)
    end
  end
end
```

Done!

[wisper_rspec]: https://github.com/krisleech/wisper-rspec
[wisper_link]: https://github.com/krisleech/wisper
<!-- You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: http://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/ -->
