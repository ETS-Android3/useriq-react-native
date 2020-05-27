# useriq-react-native

* [Installing react-native SDK](https://github.com/useriq-com/useriq-react-native#installing-react-native-sdk)
* [Linking the project](https://github.com/useriq-com/useriq-react-native#linking-the-project)
* [Usage](https://github.com/useriq-com/useriq-react-native#usage)
* [API & Usage](https://github.com/useriq-com/useriq-react-native#api--usage)
* [Support](https://github.com/useriq-com/useriq-react-native#support)
---

## Installing React-native SDK

`$ npm install @useriq/useriq-react-native --save`

> Note: Before `npm 5.0.0` the `--save` flag is required in this step. React Native will link modules based on dependencies in your package.json file.

Or in case you prefer yarn over npm, use the following command

`yarn add @useriq/useriq-react-native`

---
## Linking the project
### Autolinking
- **If using React Native 0.60+**


  [CLI autolink feature](https://github.com/react-native-community/cli/blob/master/docs/autolinking.md) links the module while building the app. 


- **If using React Native <= 0.59**


  ```bash
  $ react-native link @useriq/useriq-react-native
  ```

If you can't or don't want to use the CLI tool, you can also manually link the library using the instructions below

### Manual linking
<details>
<summary>Manually link the library on iOS</summary>

1. Install UserIQ framework via Cocoapods in the iOS folder of your app project
  
  ```ruby
  pod 'UserIQ'
  ```
  
2. Follow the [instructions in the React Native documentation](https://facebook.github.io/react-native/docs/linking-libraries-ios#manual-linking) to manually link the framework

</details>

<details>
<summary>Manually link the library on Android</summary>

1. Open up `android/app/src/main/java/[...]/MainApplication.java`

- Add `import com.useriq.rn.UserIQReactNativePackage;` to the imports at the top of the file
- Add `new UserIQReactNativePackage()` to the list returned by the `getPackages()` method

2. Append the following lines to `android/settings.gradle`:
   ```
   include ':@useriq_useriq-react-native'
   project(':@useriq_useriq-react-native').projectDir = new File(rootProject.projectDir, '../node_modules/@useriq/useriq-react-native/android')
   ```
3. Insert the following lines inside the dependencies block in `android/app/build.gradle`:
   ```
    implementation project(':@useriq_useriq-react-native')
   ```

</details>

---

## Usage

### 1. Initialization

Initialize `UserIQ` sdk as eary as possible. `UserIQ.init()` method needs to be called atleast once after the root component / application is mounted

```javascript
import React from 'react';
import UserIQ from '@useriq/useriq-react-native'

export class App extends React.Component {

  componentDidMount() {
    UserIQ.init('<YOUR_MOBILE_API_KEY>')
  }
  ...
}
```
> Note: The `<YOUR_MOBILE_API_KEY>` is available in the [mobile dashboard](https://mobile.useriq.com) under the `App Settings` page.

### 1a. Initialize for multiple platforms

If you have single code base for both iOS & Android & if you want to initialize SDK for both platforms, you can initialze using `Platform.select`. If not skip to step 2.

```javascript
import React from 'react';
import { Platform } from 'react-native';
import UserIQ from '@useriq/useriq-react-native'

export class App extends React.Component {

  componentDidMount() {
    Platform.select({
      ios: () => UserIQ.init('IOS_API_KEY'),
      android: () => UserIQ.init('ANDROID_API_KEY'),
    })()
  }
  ...
}
```

This will automatically choose the right `api_key` for the appropriate platform & initialize it. An anonymous user is set and SDK is initiated

> Note: The `IOS_API_KEY` & `ANDROID_API_KEY` is available in the [mobile dashboard](https://mobile.useriq.com) under the `App Settings` page.

### 2. Set loggedin user

SDK initialization itself doesnt send any data to UserIQ server until `setUser()` is called. So after user is sucessfully logged in, call `setUser()` with required params. `id`, `name`, `email`, `accountId`, `accountName` and `signUpDate` are mandatory parameters to be passed and values should be passed `String`

```javascript
import React from 'react'
import UserIQ from '@useriq/useriq-react-native'

class LoginComponent extends React.Component {
  onLoginSuccess(user) {
    UserIQ.setUser({
      id: user.id,
      name: user.name,
      email: user.email,
      accountId: user.accountId,
      accountName: user.accountName,
      signUpDate: user.signUpDate,
      custom_parameter_1: "custom_value_1",
      custom_parameter_2: "custom_value_2"
    })
  }
}
```

> Note: Ensure that `id`, `name`, `email`, `accountId`, `accountName` & `signUpDate` are sent as `string` 

### 3. Logout
If a user logs out, the user can be reset to anonymous user just by calling the `logout` API. Make sure this method is called when the user logs out, so that login screen tracking and other information not related to the user does not get linked to the user.

```javascript
  UserIQ.logOut()
```

# API & USAGE

For more details on API & usage, please refer to [wiki page](https://github.com/useriq-com/useriq-react-native/wiki)

### Support
If you face any other issues while integrating the UserIQ SDK [raise a ticket](https://support.useriq.com/hc/en-us/requests/new)

