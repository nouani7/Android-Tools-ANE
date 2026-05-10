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

## 📘 AndroidToolsANE – Usage Guide

AndroidToolsANE is an Adobe AIR Native Extension that provides access to Android system features through a simple ActionScript 3 API.

---

## 📌 Initialization

```actionscript id="init1"
import com.mx.android.tools.AndroidTools;
import com.mx.android.tools.events.USSDResultEvent;
import com.mx.android.tools.events.ContactEvent;

```
```actionscript id="init1"
var tools:AndroidTools = new AndroidTools();
```

---

## 📞 Phone Calls

### Open dialer (no permission required)

```actionscript id="dial1"
tools.dial("0555123456");
```

### Direct call (requires CALL_PHONE permission)

```actionscript id="call1"
tools.call("0555123456");
```

---

## 💬 SMS

```actionscript id="sms1"
tools.sms("0555123456", "Hello from AndroidToolsANE");
```

---

## 📧 Email

```actionscript id="email1"
tools.email(
    "test@gmail.com",
    "Subject",
    "Message body"
);
```

---

## 📶 USSD

### Simple USSD request

```actionscript id="ussd1"
tools.ussd("*222#");
```

### USSD with response handling

```actionscript id="ussd2"
tools.ussdResult("*222#");
```

### Handle response

```actionscript id="ussd3"
tools.addEventListener(USSDResultEvent.RESULT, onUSSD);

function onUSSD(e: USSDResultEvent): void
{
	if (e.isSuccess)
	{
		 trace("USSD Result: " + e.result);
	}
	else
	{
		trace("ERORR:" + e.result);
	}
}

```

---

## 💚 WhatsApp

```actionscript id="wa1"
tools.whatsapp("0555123456", "Hello WhatsApp");
```

---

## ✈️ Telegram

```actionscript id="tg1"
tools.telegram("username");
```

---

## 💬 Messenger

```actionscript id="ms1"
tools.messenger("user_id");
```

---

## 🔒 Signal

```actionscript id="sg1"
tools.signal("0555123456");
```

---

## 📇 Contact Picker

```actionscript id="cp1"

tools.addEventListener(ContactEvent.SELECTED, onContact);
tools.addEventListener(ContactEvent.CANCELLED, onCancelled);
tools.addEventListener(ContactEvent.ERROR, onContactErr);

tools.pickContact();

function onContact(e: ContactEvent): void
{
	trace(e.name, e.phone); 
}

function onCancelled(e: ContactEvent): void
{
	trace("User cancelled pick Contact");
}
function onContactErr(e: ContactEvent): void
{
	trace("ERORR: " + e.phone);
}

```

---

## 🔋 Battery Information

```actionscript id="bat1"
var battery:Object = tools.getBatteryInfo();

trace(battery.level);
trace(battery.isCharging);
```

---

## 📡 Network Type

```actionscript id="net1"
trace(tools.getNetworkType()); // "WIFI" | "MOBILE" | "ETHERNET" | "BLUETOOTH" | "NONE" | "UNKNOWN"

```
---

## 📱 SIM Information

```actionscript id="sim1"
var sim:Object = tools.getSIMInfo();

trace(sim.carrier);
trace(sim.country);
```

---

## 📊 Device Information

```actionscript id="dev1"
var dev:Object = tools.getDeviceInfo();
trace(dev.brand);       
trace(dev.model);        
trace(dev.manufacturer); 
trace(dev.android);   
```

---

## 📦 Installed Apps

```actionscript id="apps1"
var apps:Array = tools.getInstalledApps();

for (var i:int = 0; i < apps.length; i++)
{
    var app:Object = apps[i];

    var name:String =
        app.name ? app.name :
        app.appName ? app.appName :
        app.label ? app.label :
        "Unknown";

    var pkg:String =
        app.packageName ? app.packageName :
        app.package ? app.package :
        "Unknown";

    trace("App " + i);
    trace("Name: " + name);
    trace("Package: " + pkg);
    trace("----------------");
}

trace(apps.length);

```
Example: Display Installed Apps List

---

## 🔍 Check App Availability

```actionscript id="check1"
var installed:Boolean =
    tools.isAppInstalled("com.whatsapp");

trace(installed);
```
---

## ✅ Android Manifest Configuration

```xml id="m7q2kp"
  <android>
    <manifestAdditions><![CDATA[
<manifest>

    <!-- ===================== -->
    <!-- SDK -->
    <!-- ===================== -->
    <uses-sdk
        android:minSdkVersion="23"
        android:targetSdkVersion="35"/>

    <!-- ===================== -->
    <!-- BASIC PERMISSIONS -->
    <!-- ===================== -->
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>

    <!-- ===================== -->
    <!-- CALL / PHONE -->
    <!-- ===================== -->
    <uses-permission android:name="android.permission.CALL_PHONE"/>
    <uses-permission android:name="android.permission.READ_PHONE_STATE"/>

    <!-- ===================== -->
    <!-- SMS -->
    <!-- ===================== -->
    <uses-permission android:name="android.permission.SEND_SMS"/>
    <uses-permission android:name="android.permission.RECEIVE_SMS"/>
    <uses-permission android:name="android.permission.READ_SMS"/>

    <!-- ===================== -->
    <!-- CONTACTS (if pickContact) -->
    <!-- ===================== -->
    <uses-permission android:name="android.permission.READ_CONTACTS"/>

    <!-- ===================== -->
    <!-- DEVICE INFO -->
    <!-- ===================== -->
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>

    <!-- ===================== -->
    <!-- ANDROID 11+ PACKAGE VISIBILITY -->
    <!-- مهم جدًا لـ WhatsApp / Telegram / Messenger -->
    <!-- ===================== -->
    <queries>

        <!-- Dial / Call -->
        <intent>
            <action android:name="android.intent.action.DIAL"/>
        </intent>

        <intent>
            <action android:name="android.intent.action.CALL"/>
        </intent>

        <!-- SMS -->
        <intent>
            <action android:name="android.intent.action.SENDTO"/>
            <data android:scheme="smsto"/>
        </intent>

        <!-- Email -->
        <intent>
            <action android:name="android.intent.action.SENDTO"/>
            <data android:scheme="mailto"/>
        </intent>

        <!-- WhatsApp -->
        <package android:name="com.whatsapp"/>
        <package android:name="com.whatsapp.w4b"/>

        <!-- Telegram -->
        <package android:name="org.telegram.messenger"/>
        <package android:name="org.telegram.messenger.web"/>

        <!-- Messenger -->
        <package android:name="com.facebook.orca"/>

        <!-- Signal -->
        <package android:name="org.thoughtcrime.securesms"/>

    </queries>
	
	<!-- ===================== -->
    <!-- ACTIVITIES -->
    <!-- ===================== -->

    <application>
        <!-- ContactPickerActivity — required For pickContact -->
        <activity
            android:name="com.mx.android.tools.ContactPickerActivity"
            android:theme="@android:style/Theme.Translucent.NoTitleBar"
            android:exported="false"/>
    </application>
	
</manifest>
    ]]></manifestAdditions>
  </android>
```
---

### ⚠️ Notes

#### ✔ USSD

* Some carriers do not return responses
* Behavior depends on Android device implementation

#### ✔ Direct Call

* Requires runtime permission: `CALL_PHONE`

#### ✔ Installed Apps (Android 11+)

* Requires `<queries>` in Android manifest
