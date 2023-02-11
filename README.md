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
