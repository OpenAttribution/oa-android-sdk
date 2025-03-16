# OpenAttribution Android SDK

🏗️Under construction.🏗️ 
If you'd like to help feel free to send a PR or join the [OpenAttribution Discord](https://discord.gg/Z5ueYE3Ct3).


More info on [OpenAttribution.dev](https://openattribution.dev).


## MVP Goal

To have a fully functional SDK which can be used to track and attribute installs, events and revenue for iOS back to an OpenAttribution server.
### Event tracking and details


## MVP feature road map

- [x] Library installable via Maven
- [x] user input server endpoint ie `https://demo.openattribution.dev`
- [x] Events:
	- [x] app_open tracking and attributing
	- [x] Basic event tracking
	- [x] Basic revenue tracking
- [ ] Documentation for how to use and next steps

## Install

You can get the latest version of OpenAttribution's (Android SDK from Maven Central)[https://central.sonatype.com/artifact/dev.openattribution/open-attribution-sdk]

`build.gradle.kts`
```kotlin 
dependencies {
    implementation("dev.openattribution:open-attribution-sdk:{LATEST_VERSION}")
    // rest of your dependencies
}
```

## Tracking App Open

`MyApplication.kts`
```kotlin

import android.app.Application

import dev.openattribution.sdk.OpenAttribution

class MyApplication : Application() {

    override fun onCreate() {
        super.onCreate()
        // Initialize the OpenAttribution SDK, replace with your domain
        OpenAttribution.initialize(this, "https://demo.openattribution.dev")
    }
}

```

Add to `AndroidManfiest.xml` if doesn't exist yet:

```xml

//OpeenAttribution reequires internet connection
<uses-permission android:name="android.permission.INTERNET" />

<application
        // MyApplicaiton is the name of your Application class above
        android:name=".MyApplication"
        ...

```

## Tracking Events

Use `trackEvent` to log custom events in your app.

| Parameter   | Type      | Description                                               |
| ----------- | --------- | --------------------------------------------------------- |
| `context`   | `Context` | Android context (e.g., Activity or Application).          |
| `eventName` | `String`  | Name of the custom event to track (e.g., "button_click"). |


Example use:
```kotlin
    import dev.openattribution.sdk.OpenAttribution

    ...
	
    OpenAttribution.trackEvent(context, "my_event_name")

```



## Track Purchases

Use `trackPurchase` to track revenue for In App Purchases in your app.

| Parameter       | Type     | Description                                      | Default          |
|-----------------|----------|--------------------------------------------------|------------------|
| `context`       | `Context`| Android context (e.g., Activity or Application). | -                |
| `revenueAmount` | `Double` | Purchase amount (e.g., 12.34).                  | -                |
| `currency`      | `String` | ISO 4217 currency code (e.g., "USD").           | -                |
| `eventName`     | `String` | Event name for tracking.                        | `"iap_purchase"` |


```kotlin
    OpenAttribution.trackPurchase(context, 12.34, "USD")
```


## Local Development:

If you're interested in contributing to OpenAttribution Android SDK please don't hesitate to reach out [on Discord](https://discord.gg/Z5ueYE3Ct3)  or with a PR. 

The OpenAttribution postback API will expect or allow the values as defined here:
https://github.com/OpenAttribution/Open-Attribution/blob/main/apps/postback-api/config/dimensions.py

#### Sample post back payload
This is a sample of the minimum required postback by the OpenAttribution server. The empty IFA.

```http
POST https://demo.openattribution.dev/collect/events/com.example.app HTTP/1.1
Content-Type: application/json

{
  "ifa": "00000000-0000-0000-0000-000000000000",
  "event_time": 1732003510046,
  "event_uid": "5730a99e-b009-41da-9d52-1315e26941c1",
  "event_id": "app_open",
  "oa_uid": "3bd9e091-fa6e-4b91-8dd1-503f8d4fe8f2"
}

