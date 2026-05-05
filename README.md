# Android-Tools-ANE
AndroidToolsANE is an Adobe AIR Native Extension that exposes Android system features including calls (dial/direct), SMS, email, USSD with results, WhatsApp, Telegram, Messenger, Signal, contacts picker, battery, network type, SIM info, device info, installed apps, and app availability via a simple AS3 API.

---

## ✨ Features

* 📞 Make phone calls (Dial / Direct Call)
* 💬 Send SMS messages
* 📧 Send emails
* 📶 Execute USSD codes and receive responses
* 💚 Open WhatsApp chats with text support
* ✈️ Open Telegram chats by username
* 💬 Open Messenger chats
* 🔒 Open Signal chats
* 📇 Pick contacts from device
* 🔋 Get battery information
* 📡 Get network type (WiFi / Mobile / etc.)
* 📱 Get SIM information
* 📊 Get device information
* 📦 List installed apps
* 🔍 Check if an app is installed

---

# 📘 AndroidToolsANE – Usage Guide

AndroidToolsANE is an Adobe AIR Native Extension that provides access to Android system features through a simple ActionScript 3 API.

---

# 📌 Initialization

```actionscript id="init1"
import com.mx.android.tools.AndroidTools;
import flash.events.StatusEvent;
```
```actionscript id="init1"
var tools:AndroidTools = new AndroidTools();
```

---

# 📞 Phone Calls

## Open dialer (no permission required)

```actionscript id="dial1"
tools.dial("0555123456");
```

## Direct call (requires CALL_PHONE permission)

```actionscript id="call1"
tools.call("0555123456");
```

---

# 💬 SMS

```actionscript id="sms1"
tools.sms("0555123456", "Hello from AndroidToolsANE");
```

---

# 📧 Email

```actionscript id="email1"
tools.email(
    "test@gmail.com",
    "Subject",
    "Message body"
);
```

---

# 📶 USSD

## Simple USSD request

```actionscript id="ussd1"
tools.ussd("*222#");
```

## USSD with response handling

```actionscript id="ussd2"
tools.ussdResult("*222#");
```

### Handle response

```actionscript id="ussd3"
tools.addEventListener(StatusEvent.STATUS, onStatus);

function onStatus(e:StatusEvent):void
{
    if (e.code == "USSD_SUCCESS")
    {
        trace("USSD Result: " + e.level);
    }
}
```

---

# 💚 WhatsApp

```actionscript id="wa1"
tools.whatsapp("0555123456", "Hello WhatsApp");
```

---

# ✈️ Telegram

```actionscript id="tg1"
tools.telegram("username");
```

---

# 💬 Messenger

```actionscript id="ms1"
tools.messenger("user_id");
```

---

# 🔒 Signal

```actionscript id="sg1"
tools.signal("0555123456");
```

---

# 📇 Contact Picker

```actionscript id="cp1"
tools.pickContact();
```

---

# 🔋 Battery Information

```actionscript id="bat1"
var battery:Object = tools.getBatteryInfo();

trace(battery.level);
trace(battery.isCharging);
```

---

# 📡 Network Type

```actionscript id="net1"
trace(tools.getNetworkType());
```

### Possible values:

* WIFI
* MOBILE
* ETHERNET
* UNKNOWN

---

# 📱 SIM Information

```actionscript id="sim1"
var sim:Object = tools.getSIMInfo();

trace(sim.carrier);
trace(sim.country);
```

---

# 📊 Device Information

```actionscript id="dev1"
var device:Object = tools.getDeviceInfo();

trace(device.model);
trace(device.manufacturer);
trace(device.androidVersion);
```

---

# 📦 Installed Apps

```actionscript id="apps1"
var apps:Array = tools.getInstalledApps();

trace(apps.length);
```

---

# 🔍 Check App Availability

```actionscript id="check1"
var installed:Boolean =
    tools.isAppInstalled("com.whatsapp");

trace(installed);
```

---

### Android Manifest Configuration

```xml
<android>
    <manifestAdditions><![CDATA[
<manifest xmlns:android="http://schemas.android.com/apk/res/android">

    <!-- SDK -->
    <uses-sdk
        android:minSdkVersion="23"
        android:targetSdkVersion="35"/>

    <!-- Permissions -->
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
    <uses-permission android:name="android.permission.CALL_PHONE"/>
    <uses-permission android:name="android.permission.READ_PHONE_STATE"/>
    <uses-permission android:name="android.permission.SEND_SMS"/>
    <uses-permission android:name="android.permission.RECEIVE_SMS"/>
    <uses-permission android:name="android.permission.READ_SMS"/>
    <uses-permission android:name="android.permission.READ_CONTACTS"/>
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>

    <!-- Android 11+ package visibility -->
    <queries>

        <intent>
            <action android:name="android.intent.action.DIAL"/>
        </intent>

        <intent>
            <action android:name="android.intent.action.CALL"/>
        </intent>

        <intent>
            <action android:name="android.intent.action.SENDTO"/>
            <data android:scheme="smsto"/>
        </intent>

        <intent>
            <action android:name="android.intent.action.SENDTO"/>
            <data android:scheme="mailto"/>
        </intent>

        <package android:name="com.whatsapp"/>
        <package android:name="com.whatsapp.w4b"/>

        <package android:name="org.telegram.messenger"/>
        <package android:name="com.facebook.orca"/>
        <package android:name="org.thoughtcrime.securesms"/>

    </queries>

</manifest>
    ]]></manifestAdditions>
</android>



---

# ⚠️ Notes

### ✔ USSD

* Some carriers do not return responses
* Behavior depends on Android device implementation

### ✔ Direct Call

* Requires runtime permission: `CALL_PHONE`

### ✔ Installed Apps (Android 11+)

* Requires `<queries>` in Android manifest
