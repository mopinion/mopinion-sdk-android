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
  "private": true,
  "scripts": {
    "start": "node node_modules/react-native/local-cli/cli.js start",
	"postinstall": "sed -i '' 's\/#import <RCTAnimation\\/RCTValueAnimatedNode.h>\/#import \"RCTValueAnimatedNode.h\"\/' ./node_modules/react-native/Libraries/NativeAnimation/RCTNativeAnimatedNodesManager.h && sed -i '' 's#<fishhook/fishhook.h>#\"fishhook.h\"#g' ./node_modules/react-native/Libraries/WebSocket/RCTReconnectingWebSocket.m"
  },
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
### test code
```java
Mopinion M = new Mopinion(Context context, String key);
M.event(String event);
```