# Mopinion Mobile SDK for android
The Mopinion Mobile SDK can be used to collect feedback from Android apps based on events.
To use Mopinion mobile feedback forms in your app you can include the SDK as a Library in your Android Studio project.

There is also a Mopinion Mobile SDK for iOS available [here](https://github.com/mopinion/mopinion-sdk-ios).

## install

The Mopinion Mobile SDK Library can be installed by adding it `build.gradle` file of your project.
The SDK is partly built with [React Native](https://facebook.github.io/react-native/), it needs the React Native Library to function.

You can see what your mobile forms will look like in your app by downloading our [Mopinion Forms](https://play.google.com/store/apps/details?id=com.mopinion.news) preview app in the Google Play Store.

### npm

[Install Node.js/npm](https://www.npmjs.com/get-npm)

make a `package.json` file in the root of your project:

```javascript
{
  "name": "MopinionSDK",
  "version": "0.1.0",
  "dependencies": {
    "react": "16.8.0",
    "react-native": "^0.59.4"
  }
}
```

`$ npm install`

### dependencies

- [Google Volley](https://github.com/google/volley)

### Android Studio

In the main project `build.gradle` file add the following:

```gradle
allprojects {
    repositories {
        google()
        jcenter()
        maven {
            // All of React Native (JS, Android binaries) is installed from npm
            url "$rootDir/node_modules/react-native/android"
        }
        maven {
            url  "https://dl.bintray.com/mopinion/MopinionSDK"
        }
    }
}
```

The `url "$rootDir/node_modules/react-native/android"` assumes the `node_modules` folder is in the root of your project. Otherwise you can change this location to  
`url "$rootDir/../node_modules/react-native/android"`, for example.

In the `build.gradle` file of your main component add the React Native and Mopinion SDK Libraries:

```gradle
android {
	...
	defaultConfig {
		...
		ndk {
		        abiFilters "armeabi", "armeabi-v7a", "x86", "x86_64", "mips", "mips64"
	    }
	    packagingOptions {
			 exclude "lib/arm64-v8a/libgnustl_shared.so"
	    }
	}
}
...
dependencies {
    ...
    implementation "com.facebook.react:react-native:+"    
    implementation "com.mopinion.mopinionsdk:mopinionsdk:0.3.1"
    implementation "com.android.volley:volley:1.1.1"
}
```

The SDK needs to connect to the Mopinion servers so the internet permission should be added to your `AndroidManifest.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.app">
    
		<uses-permission android:name="android.permission.INTERNET" />
		
		<application
		...
```

### implement the SDK
```java
Mopinion M = new Mopinion(Context context, String key, boolean log);
M.event(String event);
```

The `context` is the Activity context from which you would like to activate the feedback form, normally `this`.
The `key` should be replaced with your specific deployment key. This key can be found in your Mopinion account at the `Feedback forms` section under `Deployments`.
The `log` flag can be set to `true` while developing the app to see Logcat messages in Android Studio from the Mopinion SDK library. (The default is `false` if not given.)
The `event` is a specific event that can be connected to a feedback form action in the Mopinion system.  
The default `_button` event triggers the form, but an unlimited number of custom events can also be added.

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
import com.mopinion.mopinionsdkweb.*;
...
Mopinion M = new Mopinion(this, "12345abcde");
...
M.data("first name", "Andy");
M.data("last name", "Rubin");
...
M.event("_button");
```

## Edit triggers
In the Mopinion system you can define events and triggers that will work with the SDK events you created in your app.
Login to your Mopinion account and go to the form builder to use this functionality.

The custom defined events can be used in combination with rules:

* trigger: `passive` or `proactive`. A passive form always shows when the event is triggered. A proactive form only shows once, you can set the refresh time after which the form should show again.  
* percentage (proactive trigger): % of users that should see the form  
* date: only show the form at at, after or before a specific date or date range  
* time: only show the form at at, after or before a specific time or time range  
* target: the OS the form should show (iOS or Android) 