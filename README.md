# mopinion-sdk-android
Mopinion Mobile SDK for android

## install

make `package.json` file in root of your project:

```javascript
{
  "name": "<YOUR PROJECT>",
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

In the main project gradle file:

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
	        url 'https://jitpack.io' 
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
    implementation "com.github.mopinion:mopinion-sdk-android:0.1.0"
}
```
### test code
```java
Mopinion M = new Mopinion(this, "3t5sg11dpm3vrpoquxirnsdvi4dff12y4vv");
M.event("click 1");
```