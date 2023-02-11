# android-hacking-101

## Android Architecture
#### Overview of Android Architecture
- [Overview of Android Layers (Part 1)](https://youtu.be/Thl_0ycg-i8)
- [Overview of Android Layers (Part 2)](https://youtu.be/-XsdClm9yAM)

#### Overview of Android Components
- Android apps can be written using Kotlin, Java, and C++ languages. The Android SDK tools compile your code along with any data and resource files into an APK or an Android App Bundle.
- An *Android package*, which is an archive file with an `.apk` suffix, contains the contents of an Android app that are required at runtime and it is the file that Android-powered devices use to install the app.

- App components are the essential building blocks of an Android app. Each component is an entry point through which the system or a user can enter your app. Some components depend on others.
- There are four different types of app components:
    - [Activities](https://youtu.be/watch?v=Wg-pZAQAYHA)
    - [Services](https://youtu.be/watch?v=BKvwuZ5ib1M)
    - [Broadcast receivers](https://youtu.be/watch?v=GZmM_x-PJBY)
    - [Content providers](https://youtu.be/watch?v=b3m9UIeSpBY)

### Android Manifest Overview

- Every app has an Android Manifest file, which embeds content in binary XML format. The standard name of this file is `AndroidManifest.xml`. It is located in the root directory of the app’s Android Package Kit (APK) file.
- The manifest file is required to declare the components of the app, which include all activities, services, broadcast receivers, and content providers. Each component must define basic properties such as the name of its Kotlin or Java class. It can also declare capabilities such as which device configurations it can handle, and intent filters that describe how the component can be started.
- The manifest does a number of things in addition to declaring the app's components,
such as the following:
    - Identifies any user permissions the app requires, such as Internet access or read-access to the user's contacts.
    - Declares the minimum API Level required by the app, based on which APIs the app uses.
    - Declares hardware and software features used or required by the app, such as a camera, Bluetooth services, or a multi-touch screen.
    - Declares API libraries the app needs to be linked against (other than the Android framework APIs), such as the  Google Maps library.

#### App components

- For each app component that you create in your app, you must declare a corresponding XML element in the manifest file:
    - `<activity>` for each subclass of `Activity`.
    - `<service>` for each subclass of `Service`.
    - `<receiver>` for each subclass of `BroadcastReceiver`.
    - `<provider>` for each subclass of `ContentProvider`.
- Activities, services, and content providers that you include in your source but do not declare in the manifest are not visible to the system and, consequently, can never run.  However, broadcast receivers can be either declared in the manifest or created dynamically in code as `BroadcastReceiver` objects and registered with the system by calling `registerReceiver()`.
- Services and Activities can also be exported, which allows other processes on the device to start the service or launch the activity. The components are exported by setting an element in he manifest like below. By default, `android:exported="false"` unless this element is set to true in the manifest or intent-filters are defined for the Activity or Service.

```xml
<service android:name=".ExampleExportedService" android:exported="true"/>
<activity android:name=".ExampleExportedActivity" android:exported="true"/>
```

- The name of your subclass must be specified with the `name` attribute, using the full package designation. For example, an `Activity` subclass can be declared as follows:
    
    ```xml
    <manifest ... >
        <application ... >
            <activity android:name="com.example.myapp.MainActivity" ... >
            </activity>
        </application>
    </manifest>
    ```
    
- However, if the first character in the `name` value is a period, the app's namespace (from the module-level `build.gradle` file's `namespace` property) is prefixed to the name. For example, if the namespace is "com.example.myapp" the following activity name is resolved to "com.example.myapp.MainActivity"`:

```xml
<manifest ... >
    <application ... >
        <activity android:name=".MainActivity" ... >
            ...
        </activity>
    </application>
</manifest>
```

- The following link provides links to reference documents for all valid elements in the `AndroidManifest.xml` file.

[](https://developer.android.com/guide/topics/manifest/manifest-intro.html#reference)

- The XML below is a simple example `AndroidManifest.xml` that declares two activities for the app.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:versionCode="1"
    android:versionName="1.0">

    <!-- Beware that these values are overridden by the build.gradle file -->
    <uses-sdk android:minSdkVersion="15" android:targetSdkVersion="26" />

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">

        <!-- This name is resolved to com.example.myapp.MainActivity
             based upon the namespace property in the `build.gradle` file -->
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <activity
            android:name=".DisplayMessageActivity"
            android:parentActivityName=".MainActivity" />
    </application>
</manifest>
```

#### Intents
- Three of the four component types—activities, services, and broadcast receivers—are activated by an asynchronous message called an *intent*.
- Intents bind individual components to each other at runtime. You can think of them as the messengers that request an action from other components, whether the component belongs to your app or another.
- This framework allows both point-to-point and publish-subscribe messaging.
- For activities and services, an intent defines the action to perform (for example, to *view* or *send* something) and may specify the URI of the data to act on, among other things that the component being started might need to know.
- For broadcast receivers, the intent simply defines the announcement being broadcast.
    - For example, a broadcast to indicate the device battery is low includes only a known action string that indicates *battery is low*.

**More about this in** [Overview of Android Components: Intents](https://yewtu.be/watch?v=ptt7uo7lyhs)
#### Android Startup
- [Digging Into Android Startup](https://youtu.be/watch?v=5SQP0qfUDjI) 
