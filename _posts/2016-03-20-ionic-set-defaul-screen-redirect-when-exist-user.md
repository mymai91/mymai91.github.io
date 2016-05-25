---
layout: post
title:  "[Ionic] Set default screen redirect when exist user"
comments: true
date:   2016-03-20 15:07:33 +0700
categories: ionic
---
We will have 2 cases

If the exist user the screen is  will `home screen`

If is not exist user the screen will `login screen`

## How we can check user is existing?

When we sign up a user. We will set user object to localstorage.

**Example:**

```
user_object = {
              email: "example@gmail.com",
              name: "mymai",
              token: "aksoenziekd14mxd"
            }
            
localStorage.setItem("user", user_object)
```

Currently, The user exist in the app. We have to check the case to display screen

## Detect the case

We will the localstorage, if it contain the user it won't redirect to `login screen` but will redirect to `home screen`

At the app.js file in `config` block we will check exist user or not

```
  if (window.localStorage['user']) {
    $urlRouterProvider.otherwise('/app/home');
  } else {
    $urlRouterProvider.otherwise('/app/login');
  }
```
Detail code below

```
// Ionic burgerboy App

angular.module('burgerboy', [
  'ionic',
  'ngCordova',
  'login.module',
  'home.module'
])

.run(function($ionicPlatform, $ionicNavBarDelegate, $state) {
  $ionicPlatform.ready(function() {
    // Hide the accessory bar by default (remove this to show the accessory bar above the keyboard
    // for form inputs)
    if (window.cordova && window.cordova.plugins.Keyboard) {
      cordova.plugins.Keyboard.hideKeyboardAccessoryBar(true);
      cordova.plugins.Keyboard.disableScroll(true);
    }
    if (window.StatusBar) {
      // org.apache.cordova.statusbar required
      StatusBar.styleDefault();
    }
  });
})
.config(function($stateProvider,
  $urlRouterProvider,
  RestangularProvider,
  $httpProvider,
  $compileProvider
) {

  $stateProvider
  .state('app', {
    url: '/app',
    abstract: true,
    templateUrl: 'templates/menu.html',
    controller: 'AppCtrl'
  });

  if (window.localStorage['user']) {
    RestangularProvider.setDefaultHeaders({Authorization: JSON.parse(localStorage.token).token});
    $urlRouterProvider.otherwise('/app/home');
  } else {
    $urlRouterProvider.otherwise('/app/login');
  }
});

```
Done

