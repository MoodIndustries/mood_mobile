# Setup

First setup your react-native platform. 

### Android
Follow the official [Facebook] (https://facebook.github.io/react-native/docs/0.58/getting-started.html) doc. Make sure you use `Google APIs Intel x86 Atom System Image` for your simulator for 0.58.

Not covered above, but you will also need to install `gradle` to build the app for release. I recommend installing with [ASDF](https://asdf-vm.com/#/) ([Gradle plugin I used for asdf](https://github.com/rfrancis/asdf-gradle))

> npx jetify (which is run as a postInstall script) will no longer be needed once the upgrade to react native 0.60 is done

#### Troubleshooting
* Sometimes a new emulator will not see the packager. If you've followed the Facebook guide and pulled from the repo recently, try opening the dev menu in your emulator (CMD+M on mac), clicking on `Dev Settings`, clicking on `Debug server host & port for device`, and typing in `localhost:8081` (or whatever port your metro bundler is running on) as the port.

### iOS
Follow the official [Facebook] (https://facebook.github.io/react-native/docs/0.58/getting-started.html) doc for 0.58.

Make sure you are on XCode 10.2

Make sure you run `cd ios && pod install`

> You must install cocoapods if you don't already have them, `sudo gem install cocoapods`

### Fastlane
You must install fastlane if you want to deploy new iOS builds

https://docs.fastlane.tools/getting-started/ios/setup/#rubygems-macoslinuxwindows

## Running App 
1. `yarn`
2. [iOS only] `cd ios && pod install && cd ..`
3. `react-native link`
4. `react-native run-ios` or `react-native run-android`

## General Troubleshooting
Luis wrote a small [`react-native-clean` script](https://gist.github.com/whoislewys/18942ac40edb68460c709fe2ed74dee4). Kill your Metro Bundler, run `react-native-clean`, and run `react-native-run-*` again to get a fresh, artisinal, hand-crafted, non-GMO, fully vaccinated, grass-fed build.

> Sometimes, a JS refresh is not enough to update code, especially if you recently added functionality that utilizes native code.

> Also, please run this before merging a new branch to ensure that your changes still result in a clean build. Slack him if you need it. 

## Deloyment (Fastlane)

#### Pushing new builds
**IOS**
`fastlane ios beta` for new Testflight build

For new apple App Store build, open `ios/info.plist`. Increment the field `CFBundleShortVersionString` and `CFBundleVersion`

**ANDROID** 
1. Open `/android/app/build.gradle` and manually increment the versionCode field.
2. `fastlane android beta` for new Android build. (Alternatively, `cd android && gradle clean assembleRelease`)
3. Go into `android/app/build/outputs/apk/release`, rename the APK to `Mood.dev.apk` and upload it to Google Drive. Rename the same APK to `Mood.apk`, log into Play Console, and use it to create a new release there.

> Key was generated by google support suggested command `keytool -genkeypair -alias upload -keyalg RSA -keysize 2048 -validity 9125 -keystore keystore.jks` and pem file exp with `keytool -export -rfc -alias upload -file upload_certificate.pem -keystore keystore.jks`


#### Generating Screenshots
**For iOS**
In the root dir,
`fastlane snapshot`
`cd fastlane/ios_screenshots`
`fastlane frameit black`
`cd ..`
`fastlane deliver`

**For Android**
Go to https://developer.android.com/distribute/marketing-tools/device-art-generator
Drag each screenshot over to the Pixel 2 icon
Save image as
> TODO: Find a way to automate these with fastlane screengrab + an imagemagick script? similar to what frameit does but for android