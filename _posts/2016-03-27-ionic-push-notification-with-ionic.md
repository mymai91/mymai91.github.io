---
layout: post
title:  "[Ionic] Push notification"
comments: true
date:   2016-03-27 15:07:33 +0700
categories: ionic
---

To push notification with Ionic we will use `cordovaPushV5` It quite easy to handle:

## Logic to push notification

**Android device**

## Set up
The first, we should create an for app at https://developers.google.com/cloud-messaging/gcm#arch

When we have an app at google developers, we will get an `sender_id`

Use `sender_id` to install phonegap plugin push

```
phonegap plugin add phonegap-plugin-push --variable SENDER_ID="XXXXXXX"
```
Then, we will use ` $cordovaPushV5` to help push notification via phonegap plugin

## Where we put the code to get `registrationID` use with android and `deviceToken` use with ios

We will put the code at `login.js`:

```
'use strict';

angular
  .module('login.module')
  .controller('LoginCtrl', function($scope, $rootScope, $ionicPlatform, $cordovaPushV5) {

    var config = null;
    var deviceToken = "";
    $ionicPlatform.ready(function() {
      config =  {
        android: {
          senderID: "551489489489"
        },
        ios: {
          alert: 'true',
          badge: true,
          sound: 'false',
          clearBadge: true
        }
      };

      $cordovaPushV5.initialize(config).then(function(result) {
        $cordovaPushV5.onNotification();
        $cordovaPushV5.onError();
        $cordovaPushV5.register().then(function(registrationID) {
          deviceToken = registrationID;
        }, function(err) {
          alert(err);
        });
      }, function(err) {
        alert(err);
      });
    });
  });
```


## How we display notification on screen

To display the notification on screen we will use [$cordovaToast][cordovaToast]

See the way how **codovaToast** apply:

```
module.controller('MyCtrl', function($cordovaToast) {

  $cordovaToast
    .show('Here is a message', 'long', 'center')
    .then(function(success) {
      // success
    }, function (error) {
      // error
    });

  $cordovaToast.showShortTop('Here is a message').then(function(success) {
    // success
  }, function (error) {
    // error
  });

  $cordovaToast.showLongBottom('Here is a message').then(function(success) {
    // success
  }, function (error) {
    // error
  });

});
```
And we will apply cordovaToast in next part

## Where we listen notification fire to device

Cause we will get notification at in every where of the app, our app so we will listen notification fire at `app.js`

```
// Ionic stater App

angular.module('stater', [
  'ionic',
  'ngCordova',
  'login.module',
])
.
.
.
.run(function($cordovaPushV5, $rootScope, $location, $state, $cordovaToast, Session) {

  // Auto logout app base on expire time
  if (window.localStorage['user']) {
    if (Date.now() >= JSON.parse(localStorage["token"]).expire) {
      Session.logout();
      $location.path('/');
    }
  }

  // Push notification
  $rootScope.$on('$cordovaPushV5:notificationReceived', function(event, data) {
    if (data.additionalData.foreground === false) {
      // For the case in app
      $cordovaToast
        .show(data.message, 'long', 'top')
        .then(function(success) {
          // $location.path('/app/notification');
          // $rootScope.$apply();
        }, function (error) {
          alert(error);
        });
    } else {
      // For the case out app
      $cordovaToast
        .show(data.message, 'long', 'center')
        .then(function(success) {
          // $state.go('app.notification');
        }, function (error) {
          alert(error);
        });
    }

    if (Device.isOniOS()) {
      if (data.additionalData.badge) {
        $cordovaPushV5.setBadgeNumber(NewNumber).then(function (result) {
          alert(result);
        }, function (err) {
          alert(err);
        });
      }
    }
  });

  $rootScope.$on('$cordovaPushV5:errorOccurred', function(event, error) {
    alert(error);
  });
})
.
.
.
});
```

In the next I will write another post how to fire notification from backend (Rails)

### Alright DONE


[cordovaToast]: http://ngcordova.com/docs/plugins/toast/
