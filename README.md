# Cordova plugin for [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/)
[![NPM version][npm-version]][npm-url] [![NPM downloads][npm-downloads]][npm-url] [![NPM total downloads][npm-total-downloads]][npm-url]

| [![Donate](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)] | Your help is appreciated. Create a PR, submit a bug or just grab me :beer: |
|-|-|

[npm-url]: https://www.npmjs.com/package/cordova-plugin-firebase-messaging
[npm-version]: https://img.shields.io/npm/v/cordova-plugin-firebase-messaging.svg
[npm-downloads]: https://img.shields.io/npm/dm/cordova-plugin-firebase-messaging.svg
[npm-total-downloads]: https://img.shields.io/npm/dt/cordova-plugin-firebase-messaging.svg?label=total+downloads

## Supported platforms

- iOS
- Android

## Installation

    $ cordova plugin add firebase-messaging-with-cordovaPlugin

If you get an error about CocoaPods being unable to find compatible versions, run
    
    $ pod repo update

Use variables `IOS_FIREBASE_POD_VERSION` and `ANDROID_FIREBASE_BOM_VERSION` to override dependency versions on Android:

    $ cordova plugin add cordova-plugin-firebase-messaging \
        --variable IOS_FIREBASE_POD_VERSION="9.3.0" \
        --variable ANDROID_FIREBASE_BOM_VERSION="30.3.1"

## Adding configuration files

Cordova supports `resource-file` tag for easy copying resources files. Firebase SDK requires `google-services.json` on Android and `GoogleService-Info.plist` on iOS platforms.

1. Put `google-services.json` and/or `GoogleService-Info.plist` into the root directory of your Cordova project
2. Add new tag for Android platform

```xml
<platform name="android">
    ...
    <resource-file src="google-services.json" target="app/google-services.json" />
</platform>
...
<platform name="ios">
    ...
    <resource-file src="GoogleService-Info.plist" />
</platform>
```

This way config files will be copied on `cordova prepare` step.

### Set custom default notification icon (Android only)
Setting a custom default icon allows you to specify what icon is used for notification messages if no icon is set in the notification payload. Also use the custom default icon to set the icon used by notification messages sent from the Firebase console. If no custom default icon is set and no icon is set in the notification payload, the application icon (rendered in white) is used.
```xml
<platform name="android">
    ...
    <config-file parent="/manifest/application" target="app/src/main/AndroidManifest.xml">
        <meta-data
            android:name="com.google.firebase.messaging.default_notification_icon"
            android:resource="@drawable/my_custom_icon_id"/>
    </config-file>
</platform>
```

**`Example`**

```ts
cordova.plugins.firebase.messaging.getToken().then(function(token) {
    console.log("Got device token: ", token);
});
```
### onBackgroundMessage

**onBackgroundMessage**(`callback`, `errorCallback?`): `void`

Registers background push notification callback.

**`Example`**

```ts
cordova.plugins.firebase.messaging.onBackgroundMessage(function(payload) {
    console.log("New background message: ", payload);
});
```

#### Parameters

| Name | Type | Description |
| :------ | :------ | :------ |
| `callback` | (`payload`: [`PushPayload`](#pushpayload)) => `void` | Callback function |
| `errorCallback?` | (`error`: `string`) => `void` | Error callback function |

#### Returns

`void`

___

### onMessage

**onMessage**(`callback`, `errorCallback?`): `void`

Registers foreground push notification callback.

**`Example`**

```ts
cordova.plugins.firebase.messaging.onMessage(function(payload) {
    console.log("New message: ", payload);
});
```