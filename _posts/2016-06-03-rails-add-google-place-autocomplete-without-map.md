---
layout: post
title:  "[Rails] Add google place autocomplete without google map to ActiveAdmin"
comments: true
date:   2016-06-03 15:07:33 +0700
categories: rails
---

Today, I attempted find the way to apply google place autocomplete for ActiveAdmin in my app.

ActiveAdmin really difficult to add third-party. I think :)

The normal way to add Google place autocomplete quite easy ( haha actually I only found this way ):

## Apply Google place

* Create file:

`app/assets/javascripts/admin/googlemap.js`

* Download content at:

`http://maps.googleapis.com/maps/api/js?key=YOUR_KEY_API&sensor=false&libraries=places`

* Copy content from page have just downloaded and paste this content to file `app/assets/javascripts/admin/googlemap.js` to make it like the file in the app.

* At the file `app/assets/javascripts/active_admin.js.coffee`

We require the js like:

We have to push `googlemap.js` above `autoplace.js.coffee`

```
# app/assets/javascripts/active_admin.js.coffee
#= require active_admin/base
#= require admin/googlemap.js
#= require admin/autoplace.js.coffee
```

## Apply Autocomplete get place

Create the file `app/assets/javascripts/admin/autoplace.js.coffee`

And add content to the page:

```
google.maps.event.addDomListener window, 'load', ->
  places = new (google.maps.places.Autocomplete)(document.getElementById('txtPlaces'))
  google.maps.event.addListener places, 'place_changed', ->
    place = places.getPlace()
    address = place.formatted_address
    document.getElementById("lat").value = place.geometry.location.lat()
    document.getElementById("lng").value = place.geometry.location.lng()
    return
  return
```

## Apply Autocomplete get place in the view of ActiveAdmin

We apply autocomplete get place at Edit action Address page

At file `app/admin/address.rb`

```
ActiveAdmin.register Address do
  # Edit form
  form do |f|
    f.inputs do
      f.input :address, input_html: { id: 'txtPlaces',
                                      type: 'text',
                                      placeholder: 'Enter a location' }
      f.input :latitude, input_html: { id: 'lat' }, as: :hidden
      f.input :longitude, input_html: { id: 'lng' }, as: :hidden
    end
    f.actions
  end

  # This is the way to override method update at ActiveAdmin
  controller do
    def update
      address.update_attributes!(address_params_permit)
      super
    end

    def address_params_permit
      params.require(:address).permit(:event, :title, :address, :latitude, :longitude, :description)
    end

    def address
      Address.find(params[:id])
    end
  end
end
```

### Alright Done, I think hehe ^_^
