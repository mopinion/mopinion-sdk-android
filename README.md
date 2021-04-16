# Mopinion Mobile SDK for android
The Mopinion Mobile SDK can be used to collect feedback from Android apps based on events.
To use Mopinion mobile feedback forms in your app you can include the SDK as a Library in your Android Studio project.

There is also a Mopinion Mobile SDK for iOS available [here](https://github.com/mopinion/mopinion-sdk-ios).

## Release notes for version 0.4.0
### New features
- Migrated to AndroidX (breaking change).
- Migrated to react-native 0.61.5 (breaking change). 
- Thumbnail and preview of screenshot.

### Improvements
- New random distribution algorithm for the proactive trigger condition: percentage of users.
- Autoclose for web forms.
- Margins for thank-you page.

## install

The Mopinion Mobile SDK Library can be installed by adding it `build.gradle` file of your project.
The SDK is partly built with [React Native](https://facebook.github.io/react-native/), it needs the React Native Library to function.

You can see what your mobile forms will look like in your app by downloading our [Mopinion Forms](https://play.google.com/store/apps/details?id=com.mopinion.news) preview app in the Google Play Store.

### Prepare folder structure

If you have a default Android Studio folder structure for your project, then move all your files from the folder containing the `app` folder into a new folder `android`:

```
$ cd your-project
$ mkdir android
$ mv app android/app
$ mv build.gradle settings.gradle android/
$ # etc.
```

### npm

[Install Node.js/npm](https://www.npmjs.com/get-npm)

make a `package.json` file in the root of your project:

```javascript
{
  "name": "YourAppNameHere",
  "version": "0.1.0",
  "dependencies": {
    "react": "16.9.0",
    "react-native": "0.61.5",
    "@react-native-community/async-storage": "^1.12.1",
	"react-native-webview": "^11.4.0"
  }
}
```

`$ npm install`

Your project folder should now at least contain the following:

```
./android/app/
./node_modules/
./package.json
```

### dependencies

- [Google Volley](https://github.com/google/volley), installation is already included in our instructions below.

### Android Studio

In the main project `android/build.gradle` file, add the following:

```gradle
allprojects {
    repositories {
        google()
        jcenter()
        maven {
            // All of React Native (JS, Android binaries) is installed from npm
            url "$rootDir/../node_modules/react-native/android"
        }
        maven {
            // Android JSC is installed from npm
            url("$rootDir/../node_modules/jsc-android/dist")
        }
        maven {
            url  "https://dl.bintray.com/mopinion/MopinionSDK"
        }
    }
}
```

The `urls "$rootDir/../node_modules/*"` assumes the `node_modules` folder is in the folder above the your app folder.

#### android/app/build.gradle

In the `build.gradle` file of your main component, set the `minSdkVersion` to `19` and add the React Native and Mopinion SDK Libraries :

```gradle
...
project.ext.react = [
    enableHermes: false,  // clean and rebuild if changing
]

def enableHermes = project.ext.react.get("enableHermes", false);
def jscFlavor = 'org.webkit:android-jsc:+'
...
android {
	...
	defaultConfig {
		...
		minSdkVersion 19
		...
		ndk {
		        abiFilters "armeabi", "armeabi-v7a", "arm64-v8a", "x86", "x86_64"
	    }
	}
}
...
dependencies {
    ...
    implementation "com.facebook.react:react-native:0.61.5"    
    implementation project(':react-native-webview')
    implementation project(':@react-native-community_async-storage')
    implementation "com.mopinion.mopinionsdk:mopinionsdk:0.4.0"
    implementation "com.android.volley:volley:1.2.0"
}
```

#### android/settings.gradle

In the `settings.gradle` file of your main component, comment out the `rootProject.name` line and add the required React Native Projects:

```gradle
include ':app'
//rootProject.name = "My Application"
...
apply from: file("../node_modules/@react-native-community/cli-platform-android/native_modules.gradle"); applyNativeModulesSettingsGradle(settings)

include ':react-native-webview'
project(':react-native-webview').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-webview/android')
include ':@react-native-community_async-storage'
project(':@react-native-community_async-storage').projectDir = new File(rootProject.projectDir, '../node_modules/@react-native-community/async-storage/android')

```

Again, the above assumes the `node_modules/` folder is in the folder above your app folder. 

#### android/app/src/main/AndroidManifest.xml

The SDK needs to connect to the Mopinion servers so the internet permission should be added to your `AndroidManifest.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.app">
    
		<uses-permission android:name="android.permission.INTERNET" />
		
		<application
		...
```

## Implement the SDK
For instance, in your MainActivity.java:

```java
Mopinion M = new Mopinion(Context context, String key, boolean log);
M.event(String event);
```

* The `context` is the Activity context from which you would like to activate the feedback form, normally `this`.
* The `key` should be replaced with your specific deployment key. This key can be found in your Mopinion account at the `Feedback forms` section under `Deployments`.
* The `log` flag can be set to `true` while developing the app to see Logcat messages in Android Studio from the Mopinion SDK library. (The default is `false` if not given.)
* The `event` is a specific event that can be connected to a feedback form action in the Mopinion system.  
* The default `_button` event triggers the form, but an unlimited number of custom events can also be added.

### example:
```java
Mopinion M = new Mopinion(this, "12345abcde");
M.event("_button");
```

## extra data

From version `0.3.0` it's also possible to send extra data from the app to your form. 
This can be done by adding a key and a value to the `data()` method.
The data should be added before the `event()` method is called if you want to include the data in the form that comes up for that event.

```java
M.data(String key, String value);
```

Example:
```java
import com.mopinion.mopinionsdk.*;
...
Mopinion M = new Mopinion(this, "12345abcde");
...
M.data("first name", "Andy");
M.data("last name", "Rubin");
...
M.event("_button");
```

## clear extra data

From version `0.3.3` it's possible to remove all or a single key-value pair from the extra data previously supplied with the `data(key,value)` method. To remove a single key-value pair use this method:

```java
M.removeData(String key)
```

Example:

```java
M.removeData("first name")
```

To remove all supplied extra data use this method without arguments:

```java
M.removeData()
```

Example:

```java
M.removeData()
```

## Edit triggers
In the Mopinion system you can define events and triggers that will work with the SDK events you created in your app.
Login to your Mopinion account and go to the form builder to use this functionality.

The custom defined events can be used in combination with rules:

* trigger: `passive` or `proactive`. A passive form always shows when the event is triggered. A proactive form only shows once, you can set the refresh time after which the form should show again.  
* percentage (proactive trigger): % of users that should see the form  
* date: only show the form at, after or before a specific date or date range
* time: only show the form at, after or before a specific time or time range  
* target: the OS the form should show (iOS or Android)
