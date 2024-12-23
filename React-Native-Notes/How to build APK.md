this post addresses following question which i also faced with my team while building app:

how do i create APK file from my react native project?
what is eas build?
what is fastlane?
what to use eas or fastlane?
how do i automate building and publishing of my app?
how do i maintain and publish versions of app? 

so just trying to write about them so i can share with others, feedback is always welcome!

>Note:
>i am assuming you have this basic things already installed, Nodejs, JDK, Android Studio

If you are using Expo and haven't ejected your app then jump to [[#what is eas build? | how do i build my expo App]]
## how do i create APK file from my react native project?

straight forward without discussing or throwing any new jargons is below:

go to android/app folder:
```bash
cd android/app
```
>[!info]
>if you are using expo then eject your project and you have any native dependency 

```bash
npx expo prebuild
```


You will have to create a key to sign the apk. Use below to create your key:

```bash
keytool -genkey -v -keystore my-app-key.keystore -alias my-app-alias -keyalg RSA -keysize 2048 -validity 10000
```

```bash
cd android && ./gradlew assembleRelease
```

## what is eas build?