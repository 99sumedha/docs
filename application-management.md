---
nav-title: Application Management in NativeScript
title: "App: Management"
description: Learn how to manage the life cycle of NativeScript applications from application start to storing user-defined settings.
position: 4
---

# Application Management

The `application` module lets you manage the life cycle of your NativeScript apps from starting the application to storing user-defined settings.

* [Start Application](#start-application)
* [Use Application Events](#use-application-events)
* [Android Activity Events](#android-activity-events)
* [Persist and Restore Application Settings](#persist-and-restore-application-settings)

## Start Application

You must call the **start** method of the application module after the module initialization. 

This method is required for iOS applications. 

### Example

``` JavaScript
/*
iOS calls UIApplication and triggers the application main event loop.
*/

var application = require("application");
application.mainModule = "main-page";
application.start();
```
``` TypeScript
/*
iOS calls UIApplication and triggers the application main event loop.
*/

import application = require("application");
application.mainModule = "main-page";
application.start();
```

## Use Application Events

NativeScript applications have the following life cycle events.

+ `launch`: This event is called when application launch.
+ `suspend`: This event is called when the application is suspended.
+ `resume`: This event is called when the application is resumed after it has been suspended.
+ `exit`: This event is called when the application is about to exit.
+ `lowMemory`: This event is called when the memory on the target device is low.
+ `uncaughtError`: This event is called when an uncaught application error is present.

### Example

``` JavaScript
var application = require("application");
application.mainModule = "main-page";

application.on(application.launchEvent, function (args) {
    if (args.android) {
        // For Android applications, args.android is an android.content.Intent class.
        console.log("Launched Android application with the following intent: " + args.android + ".");
    } else if (args.ios !== undefined) {
        // For iOS applications, args.ios is NSDictionary (launchOptions).
        console.log("Launched iOS application with options: " + args.ios);
    }
});

application.on(application.suspendEvent, function (args) {
    if (args.android) {
        // For Android applications, args.android is an android activity class.
        console.log("Activity: " + args.android);
    } else if (args.ios) {
        // For iOS applications, args.ios is UIApplication.
        console.log("UIApplication: " + args.ios);
    }
});

application.on(application.resumeEvent, function (args) {
    if (args.android) {
        // For Android applications, args.android is an android activity class.
        console.log("Activity: " + args.android);
    } else if (args.ios) {
        // For iOS applications, args.ios is UIApplication.
        console.log("UIApplication: " + args.ios);
    }
});

application.on(application.exitEvent, function (args) {
    if (args.android) {
        // For Android applications, args.android is an android activity class.
        console.log("Activity: " + args.android);
    } else if (args.ios) {
        // For iOS applications, args.ios is UIApplication.
        console.log("UIApplication: " + args.ios);
    }
});

application.on(application.lowMemoryEvent, function (args) {
    if (args.android) {
        // For Android applications, args.android is an android activity class.
        console.log("Activity: " + args.android);
    } else if (args.ios) {
        // For iOS applications, args.ios is UIApplication.
        console.log("UIApplication: " + args.ios);
    }
});

application.on(application.uncaughtErrorEvent, function (args) {
    if (args.android) {
        // For Android applications, args.android is an NativeScriptError.
        console.log("NativeScriptError: " + args.android);
    } else if (args.ios) {
        // For iOS applications, args.ios is NativeScriptError.
        console.log("NativeScriptError: " + args.ios);
    }
});

application.start();
```
``` TypeScript
import application = require("application");
application.mainModule = "main-page";
application.on(application.launchEvent, function (args: application.ApplicationEventData) {
    if (args.android) {
        // For Android applications, args.android is an android.content.Intent class.
        console.log("Launched Android application with the following intent: " + args.android + ".");
    } else if (args.ios !== undefined) {
        // For iOS applications, args.ios is NSDictionary (launchOptions).
        console.log("Launched iOS application with options: " + args.ios);
    }
});

application.on(application.suspendEvent, function (args: application.ApplicationEventData) {
    if (args.android) {
        // For Android applications, args.android is an android activity class.
        console.log("Activity: " + args.android);
    } else if (args.ios) {
        // For iOS applications, args.ios is UIApplication.
        console.log("UIApplication: " + args.ios);
    }
});

application.on(application.resumeEvent, function (args: application.ApplicationEventData) {
    if (args.android) {
        // For Android applications, args.android is an android activity class.
        console.log("Activity: " + args.android);
    } else if (args.ios) {
        // For iOS applications, args.ios is UIApplication.
        console.log("UIApplication: " + args.ios);
    }
});

application.on(application.exitEvent, function (args: application.ApplicationEventData) {
    if (args.android) {
        // For Android applications, args.android is an android activity class.
        console.log("Activity: " + args.android);
    } else if (args.ios) {
        // For iOS applications, args.ios is UIApplication.
        console.log("UIApplication: " + args.ios);
    }
});

application.on(application.lowMemoryEvent, function (args: application.ApplicationEventData) {
    if (args.android) {
        // For Android applications, args.android is an android activity class.
        console.log("Activity: " + args.android);
    } else if (args.ios) {
        // For iOS applications, args.ios is UIApplication.
        console.log("UIApplication: " + args.ios);
    }
});

application.on(application.uncaughtErrorEvent, function (args: application.ApplicationEventData) {
    if (args.android) {
        // For Android applications, args.android is an NativeScriptError.
        console.log("NativeScriptError: " + args.android);
    } else if (args.ios) {
        // For iOS applications, args.ios is NativeScriptError.
        console.log("NativeScriptError: " + args.ios);
    }
});
application.start();
```
## Android Activity Events

NativeScript applications have the following Android specific activity events:

+ `activityCreated`: This event is called when activity is created.
+ `activityDestroyed`: This event is called when activity is destroyed.
+ `activityStarted`: This event is called when activity is started.
+ `activityPaused`: This event is called when activity is paused.
+ `activityResumed`: This event is called when activity is resumed.
+ `activityStopped`: This event is called when activity is stopped.
+ `saveActivityState`: This event is called to retrieve per-instance state from an activity before being killed so that the state can be restored.
+ `activityResult`: This event is called when an activity you launched exits, giving you the requestCode you started it with, the resultCode it returned, and any additional data from it.

### Example
``` JavaScript
var application = require("application");

application.mainModule = "app/mainPage";

application.on(application.androidActivityCreatedEvent, function (args) {
    console.log("Event: " + args.eventName + ", Activity: " + args.activity + ", Bundle: " + args.bundle);
});

application.on(application.androidActivityDestroyedEvent, function (args) {
    console.log("Event: " + args.eventName + ", Activity: " + args.activity);
});

application.on(application.androidActivityPausedEvent, function (args) {
    console.log("Event: " + args.eventName + ", Activity: " + args.activity);
});

application.on(application.androidActivityResultEvent, function (args) {
    console.log("Event: " + args.eventName + ", Activity: " + args.activity +
        ", requestCode: " + args.requestCode + ", resultCode: " + args.resultCode + ", Intent: " + args.intent);
});

application.on(application.androidActivityResumedEvent, function (args) {
    console.log("Event: " + args.eventName + ", Activity: " + args.activity);
});

application.on(application.androidActivityStartedEvent, function (args) {
    console.log("Event: " + args.eventName + ", Activity: " + args.activity);
});

application.on(application.androidActivityStoppedEvent, function (args) {
    console.log("Event: " + args.eventName + ", Activity: " + args.activity);
});

application.on(application.androidSaveActivityStateEvent, function (args) {
    console.log("Event: " + args.eventName + ", Activity: " + args.activity + ", Bundle: " + args.bundle);
});

application.start();
```
``` TypeScript
import application = require("application");
application.mainModule = "app/mainPage";

// Android activity events
application.on(application.androidActivityCreatedEvent, function (args: application.AndroidActivityBundleEventData) {
    console.log("Event: " + args.eventName + ", Activity: " + args.activity + ", Bundle: " + args.bundle);
});

application.on(application.androidActivityDestroyedEvent, function (args: application.AndroidActivityEventData) {
    console.log("Event: " + args.eventName + ", Activity: " + args.activity);
});

application.on(application.androidActivityPausedEvent, function (args: application.AndroidActivityEventData) {
    console.log("Event: " + args.eventName + ", Activity: " + args.activity);
});

application.on(application.androidActivityResultEvent, function (args: application.AndroidActivityResultEventData) {
    console.log("Event: " + args.eventName + ", Activity: " + args.activity +
        ", requestCode: " + args.requestCode + ", resultCode: " + args.resultCode + ", Intent: " + args.intent);
});

application.on(application.androidActivityResumedEvent, function (args: application.AndroidActivityEventData) {
    console.log("Event: " + args.eventName + ", Activity: " + args.activity);
});

application.on(application.androidActivityStartedEvent, function (args: application.AndroidActivityEventData) {
    console.log("Event: " + args.eventName + ", Activity: " + args.activity);
});

application.on(application.androidActivityStoppedEvent, function (args: application.AndroidActivityEventData) {
    console.log("Event: " + args.eventName + ", Activity: " + args.activity);
});

application.on(application.androidSaveActivityStateEvent, function (args: application.AndroidActivityBundleEventData) {
    console.log("Event: " + args.eventName + ", Activity: " + args.activity + ", Bundle: " + args.bundle);
});

application.start();
```

## Persist and Restore Application Settings

To persist user-defined settings, you need to use the `application-settings` module. The `application-settings` module is a static singleton hash table that stores key-value pairs for the application. 

The getter methods have two parameters: a key and an optional default value to return if the specified key does not exist.
The setter methods have two required parameters: a key and value. 

### Example

``` JavaScript
var applicationSettings = require("application-settings");
// Event handler for Page "loaded" event attached in main-page.xml.
function pageLoaded(args) {
    applicationSettings.setString("Name", "John Doe");
    console.log(applicationSettings.getString("Name")); // Prints "John Doe".
    applicationSettings.setBoolean("Married", false);
    console.log(applicationSettings.getBoolean("Married")); // Prints false.
    applicationSettings.setNumber("Age", 42);
    console.log(applicationSettings.getNumber("Age")); // Prints 42.
    console.log(applicationSettings.hasKey("Name")); // Prints true.
    applicationSettings.remove("Name"); // Removes the Name entry.
    console.log(applicationSettings.hasKey("Name")); // Prints false.
}
exports.pageLoaded = pageLoaded;
```
``` TypeScript
import observable = require("data/observable");
import applicationSettings = require("application-settings");
// Event handler for Page "loaded" event attached in main-page.xml.
export function pageLoaded(args: observable.EventData) {
    applicationSettings.setString("Name", "John Doe");
    console.log(applicationSettings.getString("Name"));// Prints "John Doe".
    applicationSettings.setBoolean("Married", false);
    console.log(applicationSettings.getBoolean("Married"));// Prints false.
    applicationSettings.setNumber("Age", 42);
    console.log(applicationSettings.getNumber("Age"));// Prints 42.
    console.log(applicationSettings.hasKey("Name"));// Prints true.
    applicationSettings.remove("Name");// Removes the Name entry.
    console.log(applicationSettings.hasKey("Name"));// Prints false.
}
```
