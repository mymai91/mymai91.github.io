---
layout: post
title:  "[React Native] Get OTP code from firebase"
comments: true
date:   2019-07-02 15:07:33 +0700
categories: React Native
---

# **Get OTP code from firebase**

Please follow the stable version

https://rnfirebase.io/docs/v5.x.x/getting-started

### **Go to Firebase console**

https://console.firebase.google.com/ and add project

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58547411-e0eebd80-8239-11e9-9f3e-d79c37bac84d.png">

### **Fill the infomations and create**

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58547535-1b585a80-823a-11e9-8918-ff6d899a6607.png">

### **Click on iOS icon to get info**

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58547716-75f1b680-823a-11e9-9433-98f94173386b.png">

### **Add firebase to the app**

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58547838-b0f3ea00-823a-11e9-89c2-f63516f63aa6.png">

### **Enable Phone option**

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58607039-8b192480-82d0-11e9-8799-e6bc89eb69ff.png">

### **Download and drag GoogleService-Info.plist to Xcode**

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58547956-f9130c80-823a-11e9-9ea7-9bd32dfb5fcd.png">

### **Procced**

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58548616-3af08280-823c-11e9-8c37-6b76b915a4af.png">

### **PodFile**

### **Install CocoaPod**

```
sudo gem install cocoapods

```

### **Init Podfile**

```
cd ios

```

then

```
pod init

```

Open your Podfile and add ( Can delete the things unnecessary)

```
# Uncomment the next line to define a global platform for your project
platform :ios, '9.0'

target 'FirebaseDetox' do
  # Comment the next line if you don't want to use dynamic frameworks
  use_frameworks!

  # Pods for firebaseApp
  pod 'Firebase/Core'
  pod 'Firebase/Auth'

end

```

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58549135-6f187300-823d-11e9-9b57-e5caf594c0e3.png">

then run

```
pod install

```

Need to continue proceed click next step at Firebase console

### **Result**

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58549352-e221e980-823d-11e9-9337-07ac8d7cf665.png">

## **Step**

### **1. Install react-native-firebase**

```
yarn add react-native-firebase

```

**Check package.json** make sure verion 5.x.x

```
react-native link react-native-firebase

```

### **Podfile**

### **AppDelegate.m**

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58547034-32e31380-8239-11e9-8dc6-af01932045a5.png">

```
#import "AppDelegate.h"

#import <React/RCTBridge.h>
#import <React/RCTBundleURLProvider.h>
#import <React/RCTRootView.h>

@import Firebase;

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
  RCTBridge *bridge = [[RCTBridge alloc] initWithDelegate:self launchOptions:launchOptions];
  RCTRootView *rootView = [[RCTRootView alloc] initWithBridge:bridge
                                                   moduleName:@"FirebaseDetox"
                                            initialProperties:nil];

  rootView.backgroundColor = [[UIColor alloc] initWithRed:1.0f green:1.0f blue:1.0f alpha:1];

  self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
  UIViewController *rootViewController = [UIViewController new];
  rootViewController.view = rootView;
  self.window.rootViewController = rootViewController;
  [self.window makeKeyAndVisible];
  [FIRApp configure];
  return YES;
}

- (NSURL *)sourceURLForBridge:(RCTBridge *)bridge
{
#if DEBUG
  return [[RCTBundleURLProvider sharedSettings] jsBundleURLForBundleRoot:@"index" fallbackResource:nil];
#else
  return [[NSBundle mainBundle] URLForResource:@"main" withExtension:@"jsbundle"];
#endif
}

@end

```

### **Make sure**

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58608270-5fe50400-82d5-11e9-9145-009a6148871c.png">

### **Turn on Notification**

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58603043-cc560800-82c1-11e9-92ef-3bf5616e6286.png">

### **From console**

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58612423-24523600-82e5-11e9-9b2b-16d53a26f272.png">

### 

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58612520-77c48400-82e5-11e9-9ccd-5d5c03d44acf.png">

### 

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58612631-d427a380-82e5-11e9-8aa5-155cf90b5f83.png">

### 

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58612682-01745180-82e6-11e9-9a83-68068f2baddf.png">

### **Turn off Recapcha**

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58603137-3078cc00-82c2-11e9-925d-f22fb2c3fdee.png">

### **Config GoogleService-plist.info**

1. Open GoogleService-Info.plist and Select REVERSED_CLIENT_ID
2. Open FirebaseDetox.xcworkspace
3. Double Click on FirebaseDetox
4. At TARGETS : Select FirebaseDetox
5. Choose Info Tab
6. Select URL Type
7. Paste REVERSED_CLIENT_ID

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58606639-eb0ecb80-82ce-11e9-8fa2-484823508068.png">

### 

```
cd ios

```

```
open FirebaseDetox.xcworkspace

```

If you see any error

Delete Pods folder and re try

```
rm -rf Pods
rm Podfile.lock
rm -rf FirebaseDetox.xcworkspace

```

Re install Pod

```
pod install

pod update

```

### **CODE**

Update code for App.js

```
import React, { Component } from 'react'
import { View, Button, Text, TextInput, Image } from 'react-native'

import firebase from 'react-native-firebase'

const successImageUri =
  'https://cdn.pixabay.com/photo/2015/06/09/16/12/icon-803718_1280.png'

export default class PhoneAuthTest extends Component {
  constructor(props) {
    super(props)
    this.unsubscribe = null
    this.state = {
      user: null,
      message: '',
      codeInput: '',
      phoneNumber: '+6582309048',
      confirmResult: null
    }
  }

  componentDidMount() {
    this.unsubscribe = firebase.auth().onAuthStateChanged(user => {
      if (user) {
        this.setState({ user: user.toJSON() })
      } else {
        // User has been signed out, reset the state
        this.setState({
          user: null,
          message: '',
          codeInput: '',
          phoneNumber: '+6582309048',
          confirmResult: null
        })
      }
    })
  }

  componentWillUnmount() {
    if (this.unsubscribe) this.unsubscribe()
  }

  signIn = () => {
    const { phoneNumber } = this.state
    this.setState({ message: 'Sending code ...' })

    firebase
      .auth()
      .signInWithPhoneNumber(phoneNumber)
      .then(confirmResult =>
        this.setState({ confirmResult, message: 'Code has been sent!' })
      )
      .catch(error =>
        this.setState({
          message: `Sign In With Phone Number Error: ${error.message}`
        })
      )
  }

  confirmCode = () => {
    const { codeInput, confirmResult } = this.state

    if (confirmResult && codeInput.length) {
      confirmResult
        .confirm(codeInput)
        .then(user => {
          this.setState({ message: 'Code Confirmed!' })
        })
        .catch(error =>
          this.setState({ message: `Code Confirm Error: ${error.message}` })
        )
    }
  }

  signOut = () => {
    firebase.auth().signOut()
  }

  renderPhoneNumberInput() {
    const { phoneNumber } = this.state

    return (
      <View style={{ padding: 25 }}>
        <Text testID="welcome">Enter phone number:</Text>
        <TextInput
          autoFocus
          style={{ height: 40, marginTop: 15, marginBottom: 15 }}
          onChangeText={value => this.setState({ phoneNumber: value })}
          placeholder={'Phone number ... '}
          value={phoneNumber}
        />
        <Button title="Sign In" color="green" onPress={this.signIn} />
      </View>
    )
  }

  renderMessage() {
    const { message } = this.state

    if (!message.length) return null

    return (
      <Text style={{ padding: 5, backgroundColor: '#000', color: '#fff' }}>
        {message}
      </Text>
    )
  }

  renderVerificationCodeInput() {
    const { codeInput } = this.state

    return (
      <View style={{ marginTop: 25, padding: 25 }}>
        <Text>Enter verification code below:</Text>
        <TextInput
          autoFocus
          style={{ height: 40, marginTop: 15, marginBottom: 15 }}
          onChangeText={value => this.setState({ codeInput: value })}
          placeholder={'Code ... '}
          value={codeInput}
        />
        <Button
          title="Confirm Code"
          color="#841584"
          onPress={this.confirmCode}
        />
      </View>
    )
  }

  render() {
    const { user, confirmResult } = this.state
    return (
      <View style={{ flex: 1 }}>
        {!user && !confirmResult && this.renderPhoneNumberInput()}

        {this.renderMessage()}

        {!user && confirmResult && this.renderVerificationCodeInput()}

        {user && (
          <View
            style={{
              padding: 15,
              justifyContent: 'center',
              alignItems: 'center',
              backgroundColor: '#77dd77',
              flex: 1
            }}
          >
            <Image
              source={{ uri: successImageUri }}
              style={{ width: 100, height: 100, marginBottom: 25 }}
            />
            <Text style={{ fontSize: 25 }}>Signed In!</Text>
            <Text>{JSON.stringify(user)}</Text>
            <Button title="Sign Out" color="red" onPress={this.signOut} />
          </View>
        )}
      </View>
    )
  }
}

```

# **Setup Detox**

Follow the link to install Detox

https://github.com/wix/Detox/blob/master/docs/Introduction.GettingStarted.md

```
npm install detox --save-dev
npm install jest --save-dev

```

### **Add Detox config to package.json**

We are using Pod so we have build with .xcworkspace file

`"xcodebuild -workspace ios/FirebaseDetox.xcworkspace -scheme FirebaseDetox -configuration Debug -sdk iphonesimulator -derivedDataPath ios/build"`

**NOTE**

Check list simulator

`xcrun simctl list`

Detail

```
"detox": {
    "configurations": {
      "ios.sim.debug": {
        "binaryPath": "ios/build/Build/Products/Debug-iphonesimulator/FirebaseDetox.app",
        "build": "xcodebuild -workspace ios/FirebaseDetox.xcworkspace -scheme FirebaseDetox -configuration Debug -sdk iphonesimulator -derivedDataPath ios/build",
        "type": "ios.simulator",
        "name": "iPhone 8"
      }
    },
    "test-runner": "jest"
  }

```

### **Generate e2e test folder**

```
detox init -r jest

```

### **Open firstTest.spec.js and update test case**

```
describe('Example', () => {
  beforeEach(async () => {
    await device.reloadReactNative()
  })

  it('should have welcome screen', async () => {
    await expect(element(by.id('welcome'))).toBeVisible()
  })
})

```

### **Open App.js and update**

Add renderPhoneNumberInput() function add

```
renderPhoneNumberInput() {
    const { phoneNumber } = this.state

    return (
      <View style={{ padding: 25 }}>
        <Text testID="welcome">Enter phone number:</Text>
        <TextInput
          autoFocus
          style={{ height: 40, marginTop: 15, marginBottom: 15 }}
          onChangeText={value => this.setState({ phoneNumber: value })}
          placeholder={'Phone number ... '}
          value={phoneNumber}
        />
        <Button title="Sign In" color="green" onPress={this.signIn} />
      </View>
    )
  }

```

### **Run**

```
react-native start

```

```
react-native run-ios

```

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58608326-a8042680-82d5-11e9-8a43-86a1e0048ede.png">

If you catch this error

open .xcworkspace and update build system to Legacy Build System

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58608527-8b1c2300-82d6-11e9-88a1-60b5a88c4d06.png">

Open 1 terminal tab run

```
yarn start -- --reset-cache

```

```
detox build

```

```
detox test

```

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58609242-7f7e2b80-82d9-11e9-93a2-007ac7416001.png">

# **Android**

### **1. Add Android**

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58620600-0e02a500-82fa-11e9-94b3-b3519f3fbeda.png">

### **2. Fill up the form**

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58620693-4efab980-82fa-11e9-9f38-b13ec8b75634.png">

**Follow this link** https://facebook.github.io/react-native/docs/signed-apk-android to generate SHA

```
/usr/libexec/java_home

```

you can get the link `/Library/Java/JavaVirtualMachines/jdk-XX.X.X.jdk/Contents/Home`

assume your link is `cd /Library/Java/JavaVirtualMachines/jdk-11.0.2.jdk/Contents/Home`

```
cd /Library/Java/JavaVirtualMachines/jdk-11.0.2.jdk/Contents/Home

```

generate key

```
sudo keytool -genkey -v -keystore my-release-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000

```

assume key name is `firebase-detox`

```
sudo keytool -genkeypair -v -keystore firebase-detox.keystore -alias firebase-detox -keyalg RSA -keysize 2048 -validity 10000

```

Enter password. Do not forget it (password **firebasedetox**)

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58621622-6aff5a80-82fc-11e9-8f7f-11fd7d8e7d64.png">

### **Setting up gradle variables**

**Move keystore to your android/app**

```
sudo mv firebase-detox.keystore ~/workspace/FirebaseDetox/android/app/firebase-detox.keystore

```

open file `android/gradle.properties`. Follow this structure

```
MYAPP_RELEASE_STORE_FILE=my-release-key.keystore
MYAPP_RELEASE_KEY_ALIAS=my-key-alias
MYAPP_RELEASE_STORE_PASSWORD=*****
MYAPP_RELEASE_KEY_PASSWORD=*****

```

- `*****` is your keystore password when you generate the keystore. Example my keystore password is **firebasedetox**

```
MYAPP_RELEASE_STORE_FILE=firebase-detox.keystore
MYAPP_RELEASE_KEY_ALIAS=firebase-detox
MYAPP_RELEASE_STORE_PASSWORD=firebasedetox
MYAPP_RELEASE_KEY_PASSWORD=firebasedetox

```

### **Open android/app/build.gradle**

Add code

```
...
android {
    ...
    defaultConfig { ... }
    signingConfigs {
        release {
            if (project.hasProperty('MYAPP_RELEASE_STORE_FILE')) {
                storeFile file(MYAPP_RELEASE_STORE_FILE)
                storePassword MYAPP_RELEASE_STORE_PASSWORD
                keyAlias MYAPP_RELEASE_KEY_ALIAS
                keyPassword MYAPP_RELEASE_KEY_PASSWORD
            }
        }
    }
    buildTypes {
        release {
            ...
            signingConfig signingConfigs.release
        }
    }
}
...

```

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58622553-6e93e100-82fe-11e9-9cb0-74968e7d2fd2.png">

Then follow the document and setup Android Installation step by step

https://rnfirebase.io/docs/v5.x.x/installation/android

https://rnfirebase.io/docs/v5.x.x/auth/android

Follow the image to get all the SHA 1

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58622940-68eacb00-82ff-11e9-9f17-375cf9be9aaa.png">

Put the SHA 1

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58623062-abaca300-82ff-11e9-990b-f24de335b079.png">

Download `google-service.json`

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58623305-460ce680-8300-11e9-88cc-44cd9224596d.png">

Drag `google-service.json` to app folder

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58623447-984e0780-8300-11e9-9183-d7d09815b490.png">

### 

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58623573-dfd49380-8300-11e9-928a-77c2b6dc95aa.png">

### **Add Firebase SDK**

**Project-level build.gradle (/build.gradle):**

```
buildscript {
  dependencies {
    // Add this line
    classpath 'com.google.gms:google-services:4.2.0'
  }
}

```

App-level build.gradle (//build.gradle):

```
dependencies {
  // Add this line
  implementation 'com.google.firebase:firebase-core:16.0.9'
}
...
// Add to the bottom of the file
apply plugin: 'com.google.gms.google-services'

```

### **After update need to sync gradle file**

<img width="919" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58624080-ff1ff080-8301-11e9-983a-147b3cdc8c03.png">

### **Go to android/app/src/main/java/com/[AppName]/MainApplication.java**

```
import io.invertase.firebase.auth.RNFirebaseAuthPackage;

```

```
@Override
protected List<ReactPackage> getPackages() {

    ...

    packages.add(new RNFirebaseAuthPackage()); // here
    return packages;
}

```

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/67454858-52526100-f65e-11e9-9524-e85abc395f53.png">

### 

<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/31947720/58624177-3bebe780-8302-11e9-8ae1-ac75df5e0e2a.png">

run `react-native run-android` concurent to verify installation step

### **Follow step by step**

```
https://rnfirebase.io/docs/v5.x.x/installation/android

```

```
https://rnfirebase.io/docs/v5.x.x/auth/android

```

Yay !!! Done
