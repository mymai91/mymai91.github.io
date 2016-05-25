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

## Where we listen notification fire to device

## How we display notification on screen





