
### Usage
Declare config variables in Gradle, under android/app/build.gradle:
```javascript
android {
    defaultConfig {
      buildConfigField "String",  "API_URL",     '"https://myapi.com"'
      buildConfigField "Boolean", "SHOW_ERRORS", "true"
      ...
```
Then access those from javascript:

```javascript
var Config = require('react-native-android-config');
 
Config.API_URL     // "https://myapi.com" 
Config.SHOW_ERRORS // true 
```

### More on Gradle and configs
n case you're wondering how to keep secrets outside your source: create android/config.properties:

```javascript
API_URL="https://:secret@myapi.com"
```
Then load it from build.gradle like:
```javascript
defaultConfig {
    def props = new Properties()
    props.load(new FileInputStream(rootProject.file('config.properties')))
    props.each { key, val -> buildConfigField "String", key, val }
}
```
You can do something similar under release in buildTypes to read a different set of credentials from another file.

### Setup

Include this module in android/settings.gradle:
```javascript
include ':react-native-android-config'
include ':app'
 
project(':react-native-android-config').projectDir = new File(rootProject.projectDir,
  '../node_modules/react-native-android-config/android')
```
Add a dependency to your app build in android/app/build.gradle:
```javascript
dependencies {
    ...
    compile project(':react-native-android-config')
}
```
Change your main activity to add a new package, in android/app/src/main/.../MainActivity.java:
```javascript
import com.lugg.ReactConfig.ReactConfigPackage; // add import 
 
public class MainActivity extends Activity implements DefaultHardwareBackBtnHandler {
 
    private ReactInstanceManager mReactInstanceManager;
    private ReactRootView mReactRootView;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        mReactRootView = new ReactRootView(this);
 
        // initialize our PhotoRequest handler 
        mPhotoRequest = new ReactPhotoRequestPackage(this);
 
        mReactInstanceManager = ReactInstanceManager.builder()
                .setApplication(getApplication())
                .setBundleAssetName("index.android.bundle")
                .setJSMainModuleName("index.android")
                .addPackage(new MainReactPackage())
                .addPackage(new ReactConfigPackage()) // add the package here 
```

see [react-native-android-config](https://www.npmjs.com/package/react-native-android-config) for usage.