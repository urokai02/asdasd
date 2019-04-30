# DSB SDK Implementation 

<!--ts-->
   * [Description](#desc)
      * [Technical Information](#tech-desc)
   * [Considerations to implement](#considerations)
      * [Swift](#swift)
      * [Kotlin](#kotlin)
      * [Xamarin](#xamarin)
      * [Ionic](#ionic)
   * [License](#license)
<!--te-->

<a name="desc"></a>
# Description

The objetive of this Demo Implementation is understand the properly way to implement the protection in your apps in differents development environments, those project follow the same UI and the same functionality, in this repository: Protect the Login. However, you can implement in any workflow that your consider sensitive to the businnes logic. Also if the device is secure according to isSecureCertificate and isSecureByRiskRules methods, the next activity that you can uses is the main methods of the SDK in order to understand the functionality that the SDK offer for you requeriments.

### Login Activity 

![Login UI](readme_assets/loginUI.png)

As we mentioned previously, the login data used the following method:

 1. Initialization Method: Download the main information for the SDK as License State, Risk Controller Configuration, Pins for isSecureCertificate Validation and send the initialization events (Install and Daily Run when is necessary).
 2. isSecureByRiskRules: When the Initialization method end succesfully, the SDK had the last Risk Rules Configuration, with this information validate the devices's state and report if any rules was broke.
 3. isSecureByCertificate: Validates the secure connection with the business's server, the new method with callbacks move the process in other thread and return the result in the main thread to update the UI according to the result.
 4. sendLoginData: If the previous validations (2 and 3) was successful and didn't found any risk, the SDK follow to the next activity and send the login data with user ID captured in the text box.

### Main Activity

![Main UI](readme_assets/mainUI.png)

This activity you are free to used the most important methods that the SDK offers:

 1. isSecureCertificate.
 2. isSecureByRiskRules.
 3. getRiskRulesStatus.
 4. getDeviceID.
 4. isDeviceHostsFileInfected.
 5. getDeviceHostsFileInfections.
 6. deviceHasJailbreak.
 7. Secure Storage Implementation. 

#### Note
>The projects must be have the artifacts, please contact with [Cyxtera][cyxtera] in order to get those libraries for Android and iOS.

<a name="tech-desc"></a>
## Technical Information

This repository implements the DSB SDK version 5.0.0, this major release offers the following features:
 - Easy to implement: We reduce the methods to uses the SDK, the initialization method 
 - Exceptions: The most important methods will have exceptions in case if some process is wrong, you can know what is the error and if is important according to the business logic that you implemented. 
 - Main thread secure: New methods signature in order to used as Callbacks, the idea is improve the concurrency in the main thread: The SDK create your owns thread and return the result in the main thread: **Ready to update your UI**.
 - You don't lose any event of the users: If the SDK detected any incident and is imposible to send to the DSB servers, the SDK created a queue, when the SDK have internet and execute the initialization method or detected any incidented adicionally, the SDK will send all the events in the queue (Max: 30 events in the queue).
 - Improved the method isSecureCertificate, preventing false positives.

### iOS

- Base SDK compiled: iOS 12.2.
- OS versions compatibility: From 8 to 12.
- Programing Language: Objective - C.


### Android

- API level SDK compiled: 28.
- API level version compatibility: From 16 to 28.
- Programing Language: Java.
- Dependency: Gson Library.

> For more information please use use the Developer guided.

<a name="considerations"></a>
# Considerations to implement

Something (TODO)

<a name="swift"></a>
# Swift
**Bridging-Header**

Import our framework in the Bridging-Header:

#import <dsb_protector_sdk_iOS/dsb_protector_sdk_iOS.h>

**Capabilities**

The project needs a Access Wifi Information in capabilities with a bundle identifier that suport this capabilitie and its certificate installed in the xcode and activate this capabilitie like show in the next picture

![ScreenShot](readme_assets/ScreenShotCapabilities.png)

in the info.plist put the Location When In Use Usage Description permission and Location Usage Description permission in source code 

![ScreenShot](readme_assets/ScreenShotInfoPlist.png)

in the propertie list shows:

![ScreenShot](readme_assets/ScreenShotPropertyList.png)

will need import the framework in the project and in the Bridging-Header set this #import

![ScreenShot](readme_assets/importBridging.png)

and can implment our methods like this image

![ScreenShot](readme_assets/ScreenShotImplementation.png)

<a name="kotlin"></a>
# Kotlin

## IMPORT LIBRARY 
import .aar and add dependency in build.gradle 
```sh
 implementation project(':dsb_protector_sdk_v5.0.0_android_2.3.3') 
 ``` 
You can use it in Kotlin language as usual.

<a name="xamarin"></a>
# Xamarin 

## Creating the wrapper

For Xamarin, use Xamarin studio or Visual studio, the first step is prepare the dll wrapper for will start the implementation, for this press new project and create a multiplatform library. 

![ScreenShot](readme_assets/choseProject.png)

### iOS:

for iOS, you need make two files, this files are generate with shapie, you need the sharpie version 3.3.0, use the command in terminal:

sharpie bind --output={nameOutput} --namespace={nameSpace} --sdk={SupportSdk} {nameFramework}.framework/Headers/*.h

**the namespace could be the same name than the project**, sharpie generates two files

![ScreenShot](readme_assets/FilesGenerated.png)

copy the generated files content to the project files: ApiDefinitions' content to ApiDefinition.cs and StructsAndEnums to Structs.cs 

![ScreenShot](readme_assets/csWrapper.png)

#### **Structs**

in this file you should change the variable's type: nuint to Long and nint to Int or Long 

#### **ApiDefinition**

in this file you could doing some changes:
* delete every verify ![ScreenShot](readme_assets/deleteVerify.png)
* delete the first letter to the delegates ![ScreenShot](readme_assets/deleteADDelegate.png) 
* unify the const in one interface ![ScreenShot](readme_assets/unifyConstants.png) 

#### **generate dll**

build the proyect and find in the proyect path, inside to iOS/bin/Debug the {namespace}.dll

### Android:

in the jar folder inside the Android proyect, you need add your proyect's jar and the dependencies jars in the SDK, after to add the jars select the jars and open the properties and change the build action to embeddedJar 
![ScreenShot](readme_assets/jarCompilation.png)

#### **generate dll**

build the proyect and find in the proyect path, inside to Android/bin/Debug the {namespace}.dll

### **APPLICATION:**

click in new solution for create a native app 
![ScreenShot](readme_assets/choseApp.png)

### iOS:

in Android open the edit references, and in .Net Assembly click in browse and select your {namespace}.dll in the previous directory and reference this dll. after open the iOS proyect option and go to iOS build:
* check the enable devices-specifics builds
* in the additional mtouch arguments field add the next command:
--framework:{pathAlocatedFramework}/{nameFramework}.framework

in the activity import the sdk 

### ANDROID:

in Android open the edit references, and in .Net Assembly click in browse and select your {namespace}.dll in the previous directory and reference this dll.

in the activity import the sdk 

<a name="ionic"></a>
### Ionic

The following steps describe how the SDK works in the App Development framework Ionic, in this repository the SDK already works, so this steps is just for academic purpose.

### Creating the Ionic Native Wrapper

This wrapper was created from the DSB SDK Cordova plugin and these docs are for apps built with Ionic Framework 4.0.0. 

First install the plugin as a Cordova dependency:

```sh
ionic cordova plugin add [path DSB SDK Cordova Plugin]
```

Second you need install or download the [Ionic Native][ionic_native_repo].

```sh
npm install @ionic-native/core --save
```

Then create the plugin with the .ts File:

![Plugin Ionic DSB](readme_assets/pluginIonic.png)

Then write the wrapper in TypeSript, this is the Example for the current SDK:

```sh

import { Injectable } from '@angular/core';
import { Plugin, Cordova, CordovaProperty, CordovaInstance, InstanceProperty, IonicNativePlugin } from '@ionic-native/core';

@Plugin({
  pluginName: 'DSBSDKCordovaPlugin',
  plugin: 'net.easysol.dsb.DSBSDKCordovaPlugin',
  pluginRef: 'window.DSBSDKCordovaPlugin', 
  platforms: ['Android', 'iOS']
})

@Injectable({
  providedIn: 'root'
})
export class DSBSDKCordovaPlugin extends IonicNativePlugin {
  @Cordova({
    callbackOrder: 'reverse'
  })
  init(licenseKey: string, domain?: string): Promise<any> {
    return;
  }

  @Cordova({
    callbackOrder: 'reverse'
  })
  sendLoginData(args: {}): Promise<any> {
    return;
  }

  @Cordova({
    callbackOrder: 'reverse'
  })
  getDeviceID(): Promise<any> {
    return;
  }
  
  @Cordova({
    callbackOrder: 'reverse'
  })
  isDeviceRooted(): Promise<any> {
    return;
  }

  .
  .
  .
  .
}

```

Then you need that ionic-native create the wrapper in ordert to call the Cordova Plugin from Ionic:

```sh
npm run build
```

When you execute the previous line, you have a ready wrapper in the follwing path:
![Plugin Ionic DSB](readme_assets/resultIonicWrapper.png)

Then move the folder dsbsdk-plugin in the modules of the Ionic App project that you have been developed: `node_modules/@ionic-native`:

![Wrapper in Project](readme_assets/wrapperInProject.png)

Then you can import the module in each class that you need:

![Using SDK TS](readme_assets/usingSDK.png)

1. Import the module in the class.
2. Inject the library in the constructor.
3. Use the SDK as promise (Ex: Init method).

Finally build the project for each platform:
```sh
ionic cordova build [platform]
```

> For more information:
> [Ionic Native Developer Guide][ionic_native_dev]



<a name="license"></a>
## License
----

[//]: #
   [cyxtera]: <https://www.cyxtera.com>
   [ionic_native_repo]: <https://github.com/ionic-team/ionic-native>
   [ionic_native_dev]: <https://github.com/ionic-team/ionic-native/blob/master/DEVELOPER.md>
