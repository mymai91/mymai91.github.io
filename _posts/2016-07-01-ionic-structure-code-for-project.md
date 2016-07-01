---
layout: post
title:  "[Ionic] Structure code for Ionic Project"
comments: true
date:   2016-07-01 15:07:33 +0700
categories: ionic
---

When you run `$ ionic start myApp sidemenu` Ionic will help build the app structure code:

But you will see it really difficult to realize a real project. Why? why?

Every code controller include in `controller.js` file. Imaging that you have 20 controller and you have to apply more behavior in each controller. How to you control your code?

This is a reason you have to find another way to build code structure better.

If you check file `app.js` you will see that:

```
.
.
.
.config(function($stateProvider, $urlRouterProvider) {
  .
  .
  .
  .state('app.search', {
    url: '/search',
    views: {
      'menuContent': {
        templateUrl: 'templates/search.html'
      }
    }
  })
.
.
.
```

Comment out the config for `app.search`

```
.
.
.
.config(function($stateProvider, $urlRouterProvider) {
  .
  .
  .
  // .state('app.search', {
  //   url: '/search',
  //   views: {
  //     'menuContent': {
  //       templateUrl: 'templates/search.html'
  //     }
  //   }
  // })
.
.
.
```
Instead we will create a new folder with the name `Search` include files:

* controller/search.js
* config.js
* search.module.js

In config.js

```
'use strict';

angular
  .module('search.module')
  .config(function config($stateProvider) {
    $stateProvider
      .state('app.search', {
        url: '/search',
        views: {
          'menuContent': {
            templateUrl: 'templates/search.html',
            controller: 'SearchCtrl'
          }
        }
      })
  });
```

In `search.module.js`

```
'use strict';

angular.module('search.module', [
  'ui.router'
]);
```

I define a `console.log` to demonstrate that this way work really good. y'all can define app behavior at controller

```
'use strict';

angular
  .module('search.module')
  .controller('SearchCtrl', function() {
    console.log('SearchCtrl');
  });
```

### How to the app know that, the search module is existing:

**First** Include the `Search/*.js` files in `index.html`

```
.
.
.
<!-- module js -->
<script src="js/Search/search.module.js"></script>
<script src="js/Search/config.js"></script>
<script src="js/Search/controller/search.js"></script>
.
.
.
```

**Second** Inject Search module to `app.js`

```
angular.module('starter', [
  'ionic',
  'starter.controllers',
  'search.module'
])
```

Run `ionic serve` to check result

![screen shot 2016-07-01 at 3 30 08 pm](https://cloud.githubusercontent.com/assets/6791942/16516210/f40d419a-3fa0-11e6-823f-7b1dcd087ea1.png)


## Okay, Done. Above that is the way to you can config your code structure when build Ionic project

**Find my code at** [codedemo][mymaiApp]

### Alright DONE

[mymaiApp]: https://github.com/mymai91/mymaiApp
