---
layout: post
title:  "[ReactNative] ReactNative app interact with end to end test (e2e)"
comments: true
date:   2018-03-12 15:07:33 +0700
categories: ReactNative
---

# ReactNative app interact with end to end test (e2e) 


## Generate a new react-native app call AwesomeProject

```
react-native init AwesomeProject
```

cd AwesomeProject

```
npm install react-native-paper

npm install react-native-vector-icons

react-native link react-native-vector-icons

yarn add @react-native-community/async-storage

react-native link @react-native-community/async-storage
```

**react navigation**

```
npm install --save react-navigation

yarn add react-native-gesture-handler

react-native link react-native-gesture-handler
```

## Install Detox

### 1.Brew update and install node

```
brew update && brew install node
```

### 2.Install applesimutils

```
brew tap wix/brew
brew install applesimutils
```

### 3.Install Detox command line tools (detox-cli)

```
npm install -g detox-cli
```

### 4.Add Detox to project

```
npm install -g detox-cli
```

### 5. Add Detox config to package.json

Change `AwesomeProject` to your actual project name.

<img width="645" alt="package_json_—_AwesomeProject" src="https://user-images.githubusercontent.com/6791942/58484235-412b2400-8194-11e9-8982-6d6fc03a0175.png">


```
"detox": {
    "configurations": {
      "ios.sim.debug": {
        "binaryPath": "ios/build/Build/Products/Debug-iphonesimulator/AwesomeProject.app",
        "build": "xcodebuild -project ios/AwesomeProject.xcodeproj -scheme AwesomeProject -configuration Debug -sdk iphonesimulator -derivedDataPath ios/build",
        "type": "ios.simulator",
        "name": "iPhone 7"
      }
    }
  }
```

### 6. Add jest 

Detox CLI supports Jest and Mocha out of the box. In this demo I'm choosing 'jest'

```
npm install jest --save-dev
```

### 7. Generate the test scaffolds

```
detox init -r jest
```

Ater generating the test scaffolds we should see the folder e2e

<img width="649" alt="README_md_—_AwesomeProject" src="https://user-images.githubusercontent.com/6791942/58485011-c95df900-8195-11e9-99c8-c59aac5c5d3d.png">

## If

If you see this error

`detox[18158] ERROR: [cli.js] Error: Command failed: xcodebuild -project ios/AwesomeProject.xcodeproj -scheme AwesomeProject -configuration Debug -sdk iphonesimulator -derivedDataPath ios/build`

<img width="1800" alt="1____Documents_projects_react-native_AwesomeProject__zsh_" src="https://user-images.githubusercontent.com/6791942/58485780-4342b200-8197-11e9-9add-859e550c9e5e.png">

Open new terminal tab run

```
react-native run-ios
```

If still see error let open X-code and follow steps bellow:

```
open ios/AwesomeProject.xcodeproj
```

From `File` choose `Project Setting`

<img width="1436" alt="Screenshot_28_5_19__10_56_PM" src="https://user-images.githubusercontent.com/6791942/58488443-f01f2e00-819b-11e9-8502-856e623fd163.png">

Change Build System to `Legacy Build System`

<img width="1451" alt="AwesomeProject_xcodeproj_and_New_Issue_·_mymai91_ReactNativeDetox" src="https://user-images.githubusercontent.com/6791942/58488561-2361bd00-819c-11e9-9780-e21bdc51d80c.png">

Try again

```
detox build
```

```
detox test
```

### eslint

If you are setting Eslint, at `.eslintrc.js` should add default in globals block 

```

globals: {
  fetch: false,
  detox: true,
  device: true,
  expect: true,
  waitFor: true,
  element: true,
  by: true,
}

```

### Demo

Requirment is: If you have not login yet you should see Login screen else should see Home screen

This is test case
```
describe('App', () => {
  beforeEach(async () => {
    await device.reloadReactNative()
  })

  describe('Have not login yet', () => {
    it('should show up Login screen', async () => {
      await expect(element(by.id('login_title'))).toBeVisible()
    })

    it('should redirect to Home screen when login successfully', async () => {
      await expect(element(by.id('login_userName'))).toBeVisible()
      await element(by.id('login_userName')).typeText('janymai')
      await element(by.id('login_password')).typeText('password')
      await element(by.id('login_button')).tap()
      await expect(element(by.id('home_title'))).toBeVisible()
    })
  })
})
```

**NOTE** If your test case fail in `.typeText` please make sure you **disable** connect hardware keyboard

Should turn on emulator then follow step bellow:

<img width="1222" alt="Screenshot_2_6_19__11_27_PM" src="https://user-images.githubusercontent.com/6791942/58763513-330f4600-858e-11e9-910b-dce00553fa90.png">

^___^

Jany Mai
