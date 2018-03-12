---
layout: post
title:  "[ReactNative] How to set StackNavigator initialRouteName by AsyncStorage?"
comments: true
date:   2018-03-12 15:07:33 +0700
categories: reactnative
---

## React Native yeah >___<

<b>How to set StackNavigator initialRouteName by AsyncStorage?</b>

Let me talk about the scenario here:

1) If this is the first time login => default screen is Login

2) User already login in the App => default screen is Dashboard

### Logic

To realize user account logged in or the user hasn't logged in to the app. We will define a variable called <b>token</b> to check. 
The variable will be saved in App till user log out or password expires (assume you know the way to save token in AsyncStorage)

When the app is loaded, it will check the <b>token</b>

- **If** token => default screen is Dashboard

  **Else** => default screen is Login

### Okay! Let's code this case

At the _**App.js**_ 

```
const AppNavigator = StackNavigator({
  dashboard: { screen: Dashboard },
  login: { screen: Login },
  initScreen: { screen: InitScreen }
}, {
  // Default config for all screens
  initialRouteName: 'initScreen',
});
```

#### Why define **initScreen**?

Look over this image to imagine initScreen we use for what 

<div style="text-align: center">
  <img src="https://user-images.githubusercontent.com/6791942/37290471-e64d81f6-263e-11e8-9ecd-eee8e493c9d0.jpeg" alt="reactnative">
</div>

<br/>

* This is the screen stand bettween Login screen and Dashboard screen.
* We will define the method to check condition:

  
   **If** token => default screen is Dashboard

   **Else** => default screen is Login
  

#### Code

We will check status of this case in componentWillMount (the meaning like before the page loading it will check the condition)
I assume you know the way to save and get data by use AsyncStorage.

```
componentWillMount() {
    AsyncStorageData.getToken().then((token) => {
      const mainPage = !!token ? 'dashboard' : 'authentication';
      const resetAction = NavigationActions.reset({
        index: 0,
        actions: [
          NavigationActions.navigate({ routeName: mainPage })
        ]
      });
      this.props.navigation.dispatch(resetAction);
    });
  }
```

### Alright, Done.

*Read comment code to understand more*

^___^

Jany Mai
