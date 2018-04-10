# mopinion-sdk-android
Mopinion Mobile SDK for android

## install

### npm

[Install Node.js/npm](https://www.npmjs.com/get-npm)

make `package.json` in the root of your project:

```javascript
{
  "name": "MopinionSDK",
  "version": "0.1.0",
  "dependencies": {
    "react": "16.0.0",
    "react-native": "0.51.0"
  }
}
```

`$ npm install`

### Android Studio

In the main project `build.gradle` file:

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

In the main module gradle file:

```gradle
dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation 'com.android.support:appcompat-v7:26.1.0'
    ...
    implementation "com.facebook.react:react-native:+"    
    implementation "com.mopinion.mopinionsdk:mopinionsdk:0.1.0"
}
```

Put internet permissions in the `AndroidManifest.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.app">
    
		<uses-permission android:name="android.permission.INTERNET" />
		
		<application
		...
```

### test code
```java
Mopinion M = new Mopinion(Context context, String key, boolean log);
M.event(String event);
```

### example:
```java
Mopinion M = new Mopinion(this, "12345abcde", true);
M.event("button_1");
```