<img src="https://readme-typing-svg.herokuapp.com/?font=ubuntu&color=%23B335F7&size=22&vCenter=true&height=40&lines=Android+hacking+101;">
Get ready to enter the wild world of Android security, where bugs are bountiful and the fun never ends! Buckle up, bug hunters, this repository is about to take you on a ride.


#### Disclaimer
This is not intended to be a comprehensive guide to all Android hacking resources or a guarantee that it will make you an expert in this field. However, it can provide a useful starting point for those interested in bug bounties, as all the resources mentioned have personally helped the me in getting into this field. It should be noted that some of the videos referenced may not reflect current best practices, so it is advisable to also use the regularly updated Android developer documentation.

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

## Android Hacking
- [Getting started with Android App testing with genymotion](https://youtu.be/_HRpLPrlg1U)
- [Basic Android Pentest](https://youtu.be/watch?v=NrxTBcjAL8A)
- [Hacking Android Apps with Frida](https://youtu.be/watch?v=iMNs8YAy6pk)
- [frida-boot 👢 - a binary instrumentation workshop, using Frida, for beginners](https://youtu.be/watch?v=CLpW1tZCblo)
- [Overview of common Android app vulnerabilities - LevelUp 0x05](https://youtu.be/watch?v=51S8PeuzlmI)

#### Simple Static Analysis
- [ANDROID APP SECURITY BASICS (Static analysis)](https://youtu.be/watch?v=qS5PkC-37io)
- [Static Analysis with apktool + gf + jadx](https://youtu.be/watch?v=6-M_7O3A8AI)

#### SSL Unpinning
- [SSL & It's Unpinning - Sniffing Android '10' HTTPs traffic - Part - 01](https://youtu.be/watch?v=xyCaPU0Vz20)
- [Physical Vs Emulator - Sniffing Android '10' HTTPs traffic - Part - 02](https://youtu.be/watch?v=0L_bziZiYRE)

#### Webview and Deeplinks
- [HACKING ANDROID WebViews](https://youtu.be/watch?v=qS5PkC-37io)
- [Hacking Android Deeplink Issues | Insecure URL Validation](https://youtu.be/watch?v=jn2qkLH_wjU)
- [Android Weak Host Validation](https://youtu.be/watch?v=VfyuZIvLX8Y)
- [Exploiting Android deep links and exported components - Ekoparty Mobile Hacking Space Talk](https://yewtu.be/watch?v=lg1sN8njSYs)

#### Issues with Intent
- [Pending Intents: A Pentester’s view](https://valsamaras.medium.com/pending-intents-a-pentesters-view-92f305960f03)
- [Intent Redirection (Access to Protected Components)](https://youtu.be/watch?v=Vw7I99AR-Iw)
- [Access to app protected components](https://blog.oversecured.com/Android-Access-to-app-protected-components/)


#### Mobile API
- [Finding Bugs in Mobile APIs](https://youtu.be/watch?v=N9YODrMUk5A)

## Enough theory let's do some work!
- [Android App Reverse Engineering 101](https://www.ragingrock.com/AndroidAppRE/)
- [InjuredAndroid - CTF](https://github.com/B3nac/InjuredAndroid)
- [Insecureshop - An Intentionally Vulnerable Android Application](https://github.com/hax0rgb/InsecureShop)
- [hpAndro1337 Android Application Security](https://github.com/RavikumarRamesh/hpAndro1337)
- I have placed the APKs I created for the CTF in the "challenges" folder.

## Tools that will make your life easy
- [Mobile-Security-Framework MobSF](https://github.com/MobSF/Mobile-Security-Framework-MobSF)
- [Drozer](https://github.com/FSecureLABS/drozer)
- [Objection - Runtime Mobile Exploration toolkit, powered by Frida](https://github.com/sensepost/objection)
- [Apktool:A tool for reverse engineering Android apk files](https://ibotpeaches.github.io/Apktool/)

#### Online Tools
- [Bevigil](https://bevigil.com/)
- [Oversecured](https://oversecured.com/)

## Some pretty good talks & Blogs
#### Talks
- [Advanced Android Bug Bounty skills - Ben Actis, Bugcrowd's LevelUp 2017](https://youtu.be/watch?v=OLgmPxTHLuY)
- [DEF CON Safe Mode Red Team Village - Kyle Benac - Android Application Exploitation](https://youtu.be/watch?v=UbT1KRxq9GQ)
- [Securing the System: A Deep Dive into Reversing Android Pre-Installed Apps](https://youtu.be/watch?v=U6qTcpCfuFc)
- [Maddie Stone: Whatsup with WhatsApp: A Detailed Walk Through of Reverse Engineering CVE-2019-3568](https://vimeo.com/377181218)
- [Android Exploits 101 Workshop](https://youtu.be/watch?v=squuwVQiPgg)
- [Maddie Stone - Exploiting Samsung: Analysis of an in-the-wild Samsung Exploit Chain - Ekoparty 2022](https://youtu.be/watch?v=hIRKYwgcT54)
- [OffensiveCon22 - Maddie Stone -Real World 0-days ](https://youtu.be/watch?v=8SV5l-Bxj_U)
- [Vulnerabilities of mobile OAuth 2.0 by Nikita Stupin, Mail.ru](https://yewtu.be/watch?v=vjCF_O6aZIg)

#### Blogs
- [Penetrate the Protected Component in Android Part -1](https://payatu.com/blog/amit/Penetrate_the_protected_component_in_android_Part-0)
- [Penetrate the Protected Component in Android Part -2](https://payatu.com/blog/amit/Penetrate_the_protected_component_in_android_Part-2)
- [Two weeks of securing Samsung devices: Part 1](https://blog.oversecured.com/Two-weeks-of-securing-Samsung-devices-Part-1/)
- [Two weeks of securing Samsung devices: Part 2](https://blog.oversecured.com/Two-weeks-of-securing-Samsung-devices-Part-2/)
- [Android: Gaining access to arbitrary* Content Providers](https://blog.oversecured.com/Gaining-access-to-arbitrary-Content-Providers/)
- [Reversing an Android sample which uses Flutter](https://cryptax.medium.com/reversing-an-android-sample-which-uses-flutter-23c3ff04b847)
- [How to exploit insecure WebResourceResponse configurations + an example of the vulnerability in Amazon apps](https://blog.oversecured.com/Android-Exploring-vulnerabilities-in-WebResourceResponse)
- [Sharpening your FRIDA scripting skills with Frida Tool](https://blog.securelayer7.net/sharpening-your-frida-scripting-skills-with-frida-tool/)

## Checklist
- [OWASP Mobile Security Testing Guide (MSTG)](https://github.com/OWASP/owasp-mstg/tree/master/Checklists)
- [OWASP Mobile Application Security Verification Standard (MASVS)](https://github.com/OWASP/owasp-masvs)
- [Android security checklist: WebView](https://blog.oversecured.com/Android-security-checklist-webview/)

# Disclosed Bounty Report 
- [List of Android Hackerone disclosed reports](https://github.com/B3nac/Android-Reports-and-Resources)

<a href="https://www.buymeacoffee.com/devangsolankii" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/default-orange.png" alt="Buy Me A Coffee" height="41" width="174" /></a>
