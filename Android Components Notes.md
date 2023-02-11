# Android Components

Created: July 15, 2022 2:29 PM
Type: Architecture

- Android apps can be written using Kotlin, Java, and C++ languages. The Android SDK tools compile your code along with any data and resource files into an APK or an Android App Bundle.
- An *Android package*, which is an archive file with an `.apk` suffix, contains the contents of an Android app that are required at runtime and it is the file that Android-powered devices use to install the app.

## App components

- App components are the essential building blocks of an Android app. Each component is an entry point through which the system or a user can enter your app. Some components depend on others.
- There are four different types of app components:
    - Activities
    - Services
    - Broadcast receivers
    - Content providers

### Activities

- An *activity* is the entry point for interacting with the user. It represents a single screen with a user interface. For example, an email app might have one activity that shows a list of new emails, another activity to compose an email, and another activity for reading emails.
- An activity provides the window in which the app draws its UI. This window typically fills the screen, but may be smaller than the screen and float on top of other windows.
- A different app can start any one of these activities if the email app allows it. For example, a camera app can start the activity in the email app that composes new mail to allow the user to share a picture.
- Each activity needs to be declared in the Android Manifest with the following syntax:

```xml
<activity android:name="ActivityName">
</activity>
```

### Service

- A service is an application component that can perform long-running operations in the background. It does not provide a user interface. Once started, a service might continue running for some time, even after the user switches to another application.
- For example, a service might play music in the background while the user is in a different app, or it might fetch data over the network without blocking user interaction with an activity.
- Another component, such as an activity, can start the service and let it run or bind to it in order to interact with it and even perform inter-process communication (IPC).

<aside>
💡 You can ensure that your service is available to only your app by including the `android:exported` attribute and setting it to `false`. This effectively stops other apps from starting your service.

</aside>

- There are two types of services that tell the system how to manage an app: started services and bound services.
- Started services ****tell the system to keep them running until their work is completed. This could be to sync some data in the background or play music even after the user leaves the app. Syncing data in the background or playing music also represent two different types of started services that modify how the system handles them:
    - Music playback is something the user is directly aware of, so the app tells the system this by saying it wants to be foreground with a notification to tell the user about it; in this case the system knows that it should try really hard to keep that service's process running, because the user will be unhappy if it goes away.
    - A regular background service is not something the user is directly aware as running, so the system has more freedom in managing its process. It may allow it to be killed (and then restarting the service sometime later) if it needs RAM for things that are of more immediate concern to the user.
- To help protect user privacy, Android 11 (API level 30) introduces limitations to when a foreground service can access the device's location, camera, or microphone. When your app starts a foreground service while the app is running in the background, the foreground service has the following limitations:
    - Unless the user has granted the `ACCESS_BACKGROUND_LOCATION`permission to your app, the foreground service cannot access location.
    - The foreground service cannot access the microphone or camera.
- A service is *bound* when an application component binds to it by calling `[bindService(](https://developer.android.com/reference/android/content/Context#bindService(android.content.Intent,%20android.content.ServiceConnection,%20int))).` A bound service offers a client-server interface that allows components to interact with the service, send requests, receive results, and even do so across processes with inter-process communication (IPC).
- A bound service runs only as long as another application component is bound to it. Multiple components can bind to the service at once, but when all of them unbind, the service is destroyed.
- To declare your service, add a `[<service>](https://developer.android.com/guide/topics/manifest/service-element)` element as a child of the `[<application>](https://developer.android.com/guide/topics/manifest/application-element)` element. Here is an example:

```xml
<manifest ... >
  ...
  <application ... >
      <service android:name=".ExampleService" />
      ...
  </application>
</manifest>
```

### Content providers

![Untitled](Android%20Components%2005aff72e11f64e818d6954314d7c0336/Untitled.png)

- A *content provider* manages a shared set of app data that you can store in the file system, in a SQLite database, on the cloud, or on any other persistent storage location that your app can access.
- It helps manage & mediate access to shared data.
- Through the content provider, other apps can query or modify the data if the content provider allows it. Content providers offer all regular database operations: create, read, update, delete. That means that any app with proper rights in its manifest file can manipulate the data from other apps.
- Applications don’t access a Content provider directly, but instead use a Content Resolver.
- Content Resolver is proxy that directs client request to the appropriate Content Provider.
- To query a content provider, you specify the query string in the form of a URI: `<prefix>://<authority>/<data_type>/<id>`
    - **prefix :** This is always set to content://
    - **authority :** This specifies the name of the content provider, for example *contacts*, *browser* etc. For third-party content providers, it could be *com.example.statusprovider*
    - **data_type :** This indicates the type of data that this particular provider provides. For example, if you are getting all the contacts from the *Contacts* content provider, then the data path would be *people* and URI would look like this *content://contacts/people*
    - **id :** This specifies the specific record requested. For example, if you are looking for contact number 5 in the Contacts content provider then URI would look like this *content://contacts/people/5*

### Broadcasts overview

- Android apps can send or receive broadcast messages from the Android system and other Android apps, similar to the publish-subscribe design pattern.
- These broadcasts are sent when an event of interest occurs.
    - For example, the Android system sends broadcasts when various system events occur, such as when the system boots up or the device starts charging.
    - Apps can also send custom broadcasts, for example, to notify other apps of something that they might be interested in (for example, some new data has been downloaded).
- Apps can register to receive specific broadcasts. When a broadcast is sent, the system automatically routes broadcasts to apps that have subscribed to receive that particular type of broadcast.
- The broadcast message itself is wrapped in an `Intent` object whose action string identifies the event that occurred (for example `android.intent.action.AIRPLANE_MODE`). The intent may also include additional information bundled into its extra field. For example, the airplane mode intent includes a boolean extra that indicates whether or not Airplane Mode is on.
- There are two ways to make a Broadcast Receiver known to the system.
    - One way is to declare it in the Android Manifest file. The manifest should specify an association between the Broadcast Receiver and an intent filter to indicate the actions the receiver is meant to listen for.
    
    ```xml
    <receiver android:name=".MyReceiver" >
        <intent-filter>
            <action android:name="com.owasp.myapplication.MY_ACTION" />
        </intent-filter>
    </receiver>
    ```
    
    - The other way is through context-registered receivers that dynamically register it via `Context.registerReceiver()`.
    
    ```kotlin
    // Define a broadcast receiver
    val myReceiver: BroadcastReceiver = object : BroadcastReceiver() {
        override fun onReceive(context: Context, intent: Intent) {
            Log.d(FragmentActivity.TAG, "Intent received by myReceiver")
        }
    }
    // Define an intent filter with actions that the broadcast receiver listens for
    val intentFilter = IntentFilter()
    intentFilter.addAction("com.owasp.myapplication.MY_ACTION")
    // To register the broadcast receiver
    registerReceiver(myReceiver, intentFilter)
    // To un-register the broadcast receiver
    unregisterReceiver(myReceiver)
    ```
    
- Android provides three ways for apps to send broadcast:
    - The `sendOrderedBroadcast(Intent, String` method sends broadcasts to one receiver at a time. As each receiver executes in turn, it can propagate a result to the next receiver, or it can completely abort the broadcast so that it won't be passed to other receivers. The order receivers run in can be controlled with the android:priority attribute of the matching intent-filter; receivers with the same priority will be run in an arbitrary order.
    - The `sendBroadcast(Intent)` method sends broadcasts to all receivers in an undefined order. This is called a Normal Broadcast. This is more efficient, but means that receivers cannot read results from other receivers, propagate data received from the broadcast, or
    abort the broadcast.
    - The `LocalBroadcastManager.sendBroadcast` method sends broadcasts to receivers that are in the same app as the sender. If you don't need to send broadcasts across apps, use local broadcasts. The implementation is much more efficient (no inter-process communication needed) and you don't need to worry about any security issues related to other apps being able to receive or send your broadcasts.

## Intents

- Three of the four component types—activities, services, and broadcast receivers—are activated by an asynchronous message called an *intent*.
- Intents bind individual components to each other at runtime. You can think of them as the messengers that request an action from other components, whether the component belongs to your app or another.
- This framework allows both point-to-point and publish-subscribe messaging.
- For activities and services, an intent defines the action to perform (for example, to *view* or *send* something) and may specify the URI of the data to act on, among other things that the component being started might need to know.
- For broadcast receivers, the intent simply defines the announcement being broadcast.
    - For example, a broadcast to indicate the device battery is low includes only a known action string that indicates *battery is low*.
- Unlike activities, services, and broadcast receivers, content providers are not activated by intents. Rather, they are activated when targeted by a request from a `ContentResolver`.
- The content resolver handles all direct transactions with the content provider so that the component that's performing transactions with the provider doesn't need to and instead calls methods on the `ContentResolver`object. This leaves a layer of abstraction between the content provider and the component requesting information (for security).
- There are separate methods for activating each type of component:
    - You can start an activity or give it something new to do by passing an `Intent` to `startActivity()` or `startActivityForResult()`(when you want the activity to return a result).
    - With Android 5.0 (API level 21) and later, you can use the `JobScheduler` class to schedule actions. For earlier Android versions, you can start a service (or give new instructions to an ongoing service) by passing an `Intent` to `[startService()](https://developer.android.com/reference/android/content/Context#startService(android.content.Intent))`. You can bind to the service by passing an `Intent` to `bindService()`.
    - You can initiate a broadcast by passing an `Intent` to methods such as `sendBroadcast()`, `sendOrderedBroadcast()`, or `sendStickyBroadcast()`.
    - You can perform a query to a content provider by calling `query()` on a `ContentResolver`.

### **Intent types**

- **Explicit intents** specify which application will satisfy the intent, by supplying either the target app's package name or a fully-qualified component class name. You'll typically use an explicit intent to start a component in your own app, because you know the class name of the activity or service you want to start.
    - For example, you might start a new activity within your app in response to a user action, or start a service to download a file in the background.
- **Implicit intents** do not name a specific component, but instead declare a general action to perform, which allows a component from another app to handle it. For example, if you want to show the user a location on a map, you can use an implicit intent to request that another capable app show a specified location on a map.
    - When you use an implicit intent, the Android system finds the appropriate component to start by comparing the contents of the intent to the *intent filters* declared in the manifest file of other apps on the device. If the intent matches an intent filter, the system starts that component and delivers it the `Intent`object. If multiple intent filters are compatible, the system displays a dialog so the user can pick which app to use.
    - When there is more than one app that responds to your implicit intent, the user can select which app to use and make that app the default choice for the action.

![**[1]** *Activity A* creates an `Intent` with an action description and passes it to `startActivity()` **[2]** The Android System searches all apps for an intent filter that matches the intent. When a match is found, **[3]** the system starts the matching activity (*Activity B*) by invoking its `onCreate()` method and passing it the `Intent`.](Android%20Components%2005aff72e11f64e818d6954314d7c0336/Untitled%201.png)

**[1]** *Activity A* creates an `Intent` with an action description and passes it to `startActivity()` **[2]** The Android System searches all apps for an intent filter that matches the intent. When a match is found, **[3]** the system starts the matching activity (*Activity B*) by invoking its `onCreate()` method and passing it the `Intent`.

- An intent filter is an expression in an app's manifest file that specifies the type of intents that the component would like to receive. For instance, by declaring an intent filter for an activity, you make it possible for other apps to directly start your activity with a certain kind of intent. Likewise, if you do *not* declare any intent filters for an activity, then it can be started only with an explicit intent.

```xml
<activity android:name="MainActivity" android:exported="true">
    <!-- This activity is the main entry, should appear in app launcher -->
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>

<activity android:name="ShareActivity" android:exported="false">
    <!-- This activity handles "SEND" actions with text data -->
    <intent-filter>
        <action android:name="android.intent.action.SEND"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <data android:mimeType="text/plain"/>
    </intent-filter>
    <!-- This activity also handles "SEND" and "SEND_MULTIPLE" with media data -->
    <intent-filter>
        <action android:name="android.intent.action.SEND"/>
        <action android:name="android.intent.action.SEND_MULTIPLE"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <data android:mimeType="application/vnd.google.panorama360+jpg"/>
        <data android:mimeType="image/*"/>
        <data android:mimeType="video/*"/>
    </intent-filter>
</activity>
```

<aside>
💡 **Caution:** To ensure that your app is secure, always use an explicit intent when starting a `Service` and do not declare intent filters for your services. Using an implicit intent to start a service is a security hazard because you can't be certain what service will respond to the intent, and the user can't see which service starts. Beginning with Android 5.0 (API level 21), the system throws an exception if you call `bindService()` with an implicit intent.

</aside>

<aside>
💡 **Note:** An explicit intent is always delivered to its target, regardless of any intent filters the component declares.

</aside>

### Intent Structure

- An `Intent` object carries information that the Android system uses to determine which component to start (such as the exact component name or component category that should receive the intent), plus information that the recipient component uses in order to properly perform the action (such as the action to take and the data to act upon).
- The primary information contained in an `Intent` is the following:
    - **Component name :** The name of the component to start. This critical piece of information that makes an intent *explicit*, meaning that the intent should be delivered only to the app component defined by the component name. Without a component name, the intent is *implicit* and the system decides which component should receive the intent based on the other intent information. This field of the `Intent` is a `ComponentName` object, which you can specify using a fully qualified class name of the target component, including the package name of the app, for example, `com.example.ExampleActivit`
    
    <aside>
    💡 **Note:** When starting a `Service`, *always specify the component name*. Otherwise, you cannot be certain what service will respond to the intent, and the user cannot see which service starts.
    
    </aside>
    
    - **Action :** A string that specifies the generic action to perform (such as *view* or *pick*).
    - **Data :** The URI (a `Uri` object) that references the data to be acted on and/or the MIME type of that data. The type of data supplied is generally dictated by the intent's action. For example, if the action is `ACTION_EDIT`, the data should contain the URI of the document to edit.
    - **Category :** A string containing additional information about the kind of component that should handle the intent.  Any number of category descriptions can be placed in an intent, but most intents do not require a category. For example, `CATEGORY_BROWSABLE` indicates that the target activity allows itself to be started by a web browser to display data referenced by a link, such as an image or an e-mail message.
    - **Extras :** Key-value pairs that carry additional information required to accomplish the requested action. Just as some actions use particular kinds of data URIs, some actions also use particular extras. For example, when creating an intent to send an email with `ACTION_SEND`, you can specify the *to* recipient with the `EXTRA_EMAIL` key, and specify the *subject* with the `EXTRA_SUBJECT` key.
    - **Flags :** Flags are defined in the `Intent` class that function as metadata for the intent. It specify how Android should handle this Intent. For example, `FLAG_DEBUG_LOG_RESOLUTION`  flag causes extra logging info to be printed when intent is processed.