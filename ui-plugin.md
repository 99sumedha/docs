---
nav-title: User Interface Layouts in NativeScript
title: "UI: Layouts"
description: Learn the basic principles of designing and positioning the UI elements inside your apps.
position: 12
---

# Overview
This article covers the basics of creating a NativeScript User Interface plugin that integrates with the existing [cross-platform](https://github.com/NativeScript/NativeScript) modules. Although the cross-platform part of NativeScript is entirely written in [TypeScript](http://www.typescriptlang.org/), the provided code samples are in plain JavaScript and are created with Sublime Text 2 as the preferred IDE. You can achieve the same with any transpiler tool that produces valid ES5 JavaScript and an IDE of your choice. The widget to enable is [NumberPicker](http://developer.android.com/reference/android/widget/NumberPicker.html) / [UIStepper](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIStepper_Class/index.html#//apple_ref/occ/cl/UIStepper) as this component is currently not available in the NativeScript UI modules and (a very important note) - semantically similar widgets are available in both Android and iOS. The suggested way for building the plugin will follow the guidelines the NativeScript team uses when creating cross-platform modules and will emphasize on major concepts like observables, bindable properties, abstract View hierarchy (or Visual Tree) and CSS styling.

> The article assumes that you are already familiar with developing [applications with NativeScript](http://docs.nativescript.org/getting-started).

# Class Hierarchy
> Across the article you will often see the "Visual Tree" expression - it refers to the JavaScript abstraction available in the cross-platform modules.

Following is a brief overview of the class hierarchy in the Visual Tree:

```
Observable
├── DependencyObservable
│   ├── Bindable
│   │   ├── ProxyObject
│   │   │   ├── View
```

### [Observable](http://docs.nativescript.org/ApiReference/data/observable/Observable)
This is the class that implements the [Observer](https://en.wikipedia.org/wiki/Observer_pattern) design-pattern. Every node within the Visual Tree should support the addEventListener/removeEventListener routine, hence the base class.

### [DependencyObservable](http://docs.nativescript.org/ApiReference/ui/core/dependency-observable/DependencyObservable)
This class enables cascading property values - for example an effective property value may be the default one, inherited from some ancestor, coming from a style or set locally directly. You may consider it as an `Observable` with extended property backing mechanism.

### [Bindable](http://docs.nativescript.org/ApiReference/ui/core/bindable/Bindable)
This class enables data-binding, using the extended property backing mechanism of the `DependencyObservable` base class. 

### [ProxyObject](http://docs.nativescript.org/ApiReference/ui/core/proxy/ProxyObject)
Each concrete UI module within NativeScript internally creates and maintains a corresponding native UI instance. This class serves as a proxy between the JavaScript object and the wrapped native object. For example, when a property changes on the JavaScript side, the implementation takes care of delegating the change to the native representation and vice-versa.

### [View](http://docs.nativescript.org/ApiReference/ui/core/view/View)
At an abstract level, *View* describes an object that has visual representation on the screen. It participates in the life-cycle and layout passes and may be styled - either through CSS or by using the [View.style](http://docs.nativescript.org/ApiReference/ui/styling/Style.html) property.

### See Also
The NativeScript documentation portal has some great content to walk you through each of the above classes in greater details:

* [Events](http://docs.nativescript.org/events.html)
* [Properties](http://docs.nativescript.org/properties.html)
* [Data-binding](http://docs.nativescript.org/bindings.html)
  
# File Structure
Having in mind the class hierarchy, the obvious choice for the base class of the widget will be `View`. Following is the structure of a typical NativeScript [UI module](https://github.com/NativeScript/NativeScript/tree/master/ui/button):

### Definition File (number-picker.d.ts)
This is the TypeScript way to describe all the publicly available APIs within a module. It is our practice to start with the API first (or defining what a widget should do) and then move to the actual implementation. Taking a close look at [NumberPicker](http://developer.android.com/reference/android/widget/NumberPicker.html) and [UIStepper](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIStepper_Class/index.html#//apple_ref/occ/cl/UIStepper) you can easily come up with the API definition (we are looking for an API that is cross-platform and each method/property is available for each native platform). For the sake of simplicity the article will cover only one property (you can examine the sample Github repository [TODO: Link]package for the complete implementation):

> This file is not mandatory and if you do not use TypeScript you may simply skip this step. Still, defining the public API one way or another will be useful for the users of the plugin.

```javascript
declare module "number-picker" {
    import view = require("ui/core/view");
    import dependencyObservable = require("ui/core/dependency-observable");

    export class NumberPicker extends view.View {
    	// static (prototype) properties
        public static valueProperty: dependencyObservable.Property;

        // instance properties
        value: number;
        
        android: android.widget.NumberPicker;
        ios: UIStepper;
    }
} 
```

> In the NativeScript modules we are using TypeScript's [ambient modules](http://www.typescriptlang.org/Handbook#modules-working-with-other-javascript-libraries), which tell the language service that this module will be available at runtime and it is our responsibility to load it.

To enable features like data-binding and styling for the widget, you need to use dependency properties to back the instance properties as described [here](http://docs.nativescript.org/bindings.html).

### Common File (number-picker-common.ts)
Looking at the [Button](https://github.com/NativeScript/NativeScript/tree/master/ui/button) folder you will notice the `*-common` file. This is the file that holds the functionality which is the same regardless of the target platform. Such functionality for example is instance properties implementation. Here is how the common file looks like:

```javascript
var view = require("ui/core/view");
var dObservable = require("ui/core/dependency-observable");
var proxy = require("ui/core/proxy");

var NumberPicker = (function (_super) {
    global.__extends(NumberPicker, _super);
    function NumberPicker() {
        _super.call(this);
    }
    Object.defineProperty(NumberPicker.prototype, "value", {
        get: function () {
            return this._getValue(NumberPicker.valueProperty);
        },
        set: function (value) {
            this._setValue(NumberPicker.valueProperty, value);
        }
    });
    NumberPicker.valueProperty = new dObservable.Property("value", "NumberPicker", new proxy.PropertyMetadata(0, dObservable.PropertyMetadataSettings.AffectsLayout));
    return NumberPicker;
})(view.View);

exports.NumberPicker = NumberPicker;
```

>The global __extends function is provided by the NativeScript Runtime and it basically adds some functionality on top of the TypeScript's __extends function.

A new `NumberPicker` class is defined and a backing dependency property with the respective getter and setter functions for the instance property is created. An important note is how the the `getter` and `setter` functions are implemented. The `DependencyObservable` `_getValue` and `_setValue` methods, respectively, enable cascading values as well as change notifications and data-bindings. Now comes the other interesting part - creating the native widgets themselves and plugging them into the NativeScript framework.

### Android-Specific File (number-picker.android.ts)
The [NativeScript CLI](https://github.com/NativeScript/nativescript-cli#development-in-app) follows the convention of marking platform-specific files with the platform name. This tells the CLI that these files will be only available for the package targeting the specified platform. With that said, you will need the `number-picker.android.ts` file to specify the Android-specific part of the widget:

```javascript
var common = require("./number-picker-common");

function onValuePropertyChanged(data) {
    var picker = data.object;
    if (!picker.android) {
        return;
    }
    picker.android.setValue(data.newValue);
}

common.NumberPicker.valueProperty.metadata.onSetNativeValue = onValuePropertyChanged;
require("utils/module-merge").merge(common, module.exports);

var NumberPicker = (function (_super) {
    global.__extends(NumberPicker, _super);
    function NumberPicker() {
        _super.apply(this, arguments);
    }
    NumberPicker.prototype._createUI = function () {
        this._android = new android.widget.NumberPicker(this._context);
    };
    Object.defineProperty(NumberPicker.prototype, "android", {
        get: function () {
            return this._android;
        }
    });
    return NumberPicker;
})(common.NumberPicker);

exports.NumberPicker = NumberPicker;
```

What the above code does is:

* Require the common module.
* Define a PropertyChangedCallback that will update the value of the native `NumberPicker`. Since it is granted this code is available for Android only, you can safely go through the NativeScript Bridge and change the property of the native widget directly.
* Register the callback with the metadata of the `value` property so that the base implementation can call it. Here, the `Proxy` level of the class hierarchy provides the implementation that will execute this callback whenever the JavaScript `value` property changes. 
* Merge the exports of the common file with the exports of this file. This is very important since at runtime the loaded file is the `number-picker.android` and you need publicly exposed API from the common module to be available on the specific one.
* Create a new `NumberPicker` class that inherits the common one and its functionality.
* Override the `_createUI` method and instantiate the `_android` field.
* Define a public getter property for the native Android instance.

> The `iOS` property, as declared in the definition (API) file, remains `undefined` when running on Android.

The `_createUI` method is part of the Visual Tree instantiation pass for Android. Because every native Android UI widget requires a valid [Context](http://developer.android.com/reference/android/content/Context.html) as a constructor parameter, the UI is lazy initialized when we have such `Context` available. This happens when the main [Activity](http://developer.android.com/reference/android/app/Activity.html) is created. The following scheme briefly summarizes the steps during the UI initialization pass within the Visual Tree:

```
Android Runtime raises the getActivity callback
				 |
Application Module handles the callback
				 |
Frame Module onActivityRequested is called
				 |
Native Activity overrides are provided
				 |
Native onCreate method is called on the main Activity
				 |
Application is navigated to the main module
				 |
The XML is parsed and the Visual Tree is created
				 |
The Visual Tree is traversed and _onAttached(context) is called on each View
				 |
_onAttached(context) will update the _context property and will call _createUI

```

So, overriding the `_createUI` method is what is needed to plug the new widget within the Visual Tree.

### iOS-specific File (number-picker.ios.ts)
Here is the minimalistic iOS functionality implementation:

```javascript
var common = require("./number-picker-common");

function onValuePropertyChanged(data) {
    var picker = data.object;
    picker.ios.value = data.newValue;
}

common.NumberPicker.valueProperty.metadata.onSetNativeValue = onValuePropertyChanged;
require("utils/module-merge").merge(common, module.exports);

var NumberPicker = (function (_super) {
    global.__extends(NumberPicker, _super);
    function NumberPicker() {
        _super.apply(this, arguments);
        this._ios = new UIStepper();
    }

    Object.defineProperty(NumberPicker.prototype, "ios", {
        get: function () {
            return this._ios;
        }
    });
    return NumberPicker;
})(common.NumberPicker);

exports.NumberPicker = NumberPicker;
```

To some extent the code looks similar to the Android-specific one and uses the same concepts but when you look into the details you will see the differences. For example the `onValuePropertyChanged` function uses the iOS [UIStepper](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIStepper_Class/index.html#//apple_ref/occ/cl/UIStepper) API to update the native `value`. Or the absence of a `_createUI` method. In iOS the instantiation pass happens in the constructor of each UI widget since each native widget may be constructed at any time, without the need of additional parameter like Android's `Context`. Here are the steps of the iOS instantiation pass:


```
AppDelegate's applicationDidFinishLaunchingWithOptions notification comes
                 |
Application Module handles the notification
                 |
Application is navigated to the main module
                 |
The XML is parsed and the Visual Tree, including native widgets, is created
```

# Testing
The minimalistic implementation is ready and the widget is ready to be tested. You need to package the files as a valid plugin, as described in the [documentation](http://docs.nativescript.org/plugins). Here are the steps:

* Create a new sample project named *myApp*
```Shell
tns create myApp
```
* Navigate to the new folder
```Shell
cd myApp
```
* Create a new folder, named `number-picker`.
* Add the above described JavaScript files in `number-picker`
  * `number-picker-common.js`
  * `number-picker.android.js`
  * `number-picker.ios.js`
* Add a new `package.json` file in `number-picker` with the following content:

```json
{
    "name": "number-picker",
    "version": "0.0.1",
    "main": "number-picker.js",
    "nativescript": {
        "platforms": {
            "ios": "1.0.0",
            "android": "1.1.0"
        }
    }
}
```

* Run the following command
```Shell
tns plugin add number-picker
```
* Modify the `app/main-page.js` file to create our new widget:

```javascript
var vmModule = require("./main-view-model");
var pickerModule = require("number-picker");

function pageLoaded(args) {
    var page = args.object;
    page.bindingContext = vmModule.mainViewModel;

    var layout = page.content;
    var picker = new pickerModule.NumberPicker();
    layout.addChild(picker);
}

exports.pageLoaded = pageLoaded;
```
* Run the application
```Shell
 tns run android (or tns emulate ios)
```

The new widget should be successfully displayed on the page.

# Handling User Interaction
The widget is already successfully visualized but it is in a very basic state - for example it does not reflect changes, coming from the Native world when the user interacts with the widget. In other words, the `value` property on the JavaScript side will not be updated after user interaction.

### Android
The Android's general way of handling change notifications is via *Listeners* - in the current scenario this is the [OnValueChangeListener](http://developer.android.com/reference/android/widget/NumberPicker.OnValueChangeListener.html). What you need to do is to create a new interface implementation and register it on the native picker instance to receive updates coming from the Android world. Since this implementation is instance-related, we want to put it in the `_createUI` method:

```javascript
NumberPicker.prototype._createUI = function () {
    this._android = new android.widget.NumberPicker(this._context);

    var that = new WeakRef(this);
    var changeListener = new android.widget.NumberPicker.OnValueChangeListener({
        onValueChange: function(picker, oldVal, newVal){
            var instance = that.get();
            if(instance) {
                instance._onPropertyChangedFromNative(NumberPicker.valueProperty, newVal);
            }
        }
    });

    this._android.setOnValueChangedListener(changeListener);
};
```

>Note the `WeakRef` wrapper of the `this` argument. This is an important part of the listener implementation as it prevents circular references (resulting in a memory leak) between the JavaScript implementation Object literal, which is statically cached per `extend` call and the outer JavaScript instance.

Here the NativeScript's Android Bridge is used to create a new [interface implementation in JavaScript](http://docs.nativescript.org/runtimes/android/generator/extend-class-interface) and to handle the `onValueChange` method. When a notification from the native Picker is received, the JavaScript object, associated with the event is retrieved and the special method `_onPropertyChangedFromNative` is called on it. This is a method on the `Proxy` class and what it does is to synchronize properties from both JavaScript and Native in a way that prevents circular updates, which may result in a StackOverflow Exception.

### iOS
On iOS the generic event [UIControlEventValueChanged](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIControl_Class/index.html#//apple_ref/c/econst/UIControlEventValueChanged) raised by the `UIStepper` widget is used. So, the approach will be to extend the base `NSObject` class, to expose a handler method and register a new instance of the extended object using the [addTargetActionForControlEvents](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIControl_Class/#//apple_ref/occ/instm/UIControl/addTarget:action:forControlEvents:) method. According to the [Extending Classes in NativeScript for iOS](http://docs.nativescript.org/runtimes/ios/how-to/ObjC-Subclassing#calling-base-methods-Exposed) article, the following code enables value change notifications:

```javascript
// put this somewhere in the module implementation
var ListenerClass = NSObject.extend({
    valueChanged: function(sender) {
        if(this._owner) {
            this._owner._onPropertyChangedFromNative(common.NumberPicker.valueProperty, sender.value);
        }
    }
}, {
    name: "ListenerClass",
    exposedMethods: {
        "valueChanged": { returns: interop.types.void, params: [ UIStepper ] }
    }
});

////////////////////////////////////////

// inside the NumberPicker's constructor
function NumberPicker() {
    _super.apply(this, arguments);

    this._ios = new UIStepper();
    this._listener = new ListenerClass();
    this._listener._owner = this;
    this._ios.addTargetActionForControlEvents(this._listener, "valueChanged", UIControlEvents.UIControlEventValueChanged);
}
```

>Note the assignment of the `listener` object to the `this` argument (`this._listener = new ListenerClass()`) in the constructor. This is needed to prevent the native class deallocation since the `addTargetActionForControlEvents` method uses `Weak` references when adding listeners.

### Data-binding Ready
With gluing the Native-to-JavaScript and JavaScript-to-Native flow of changes the `value` property is completely [data-binding](http://docs.nativescript.org/bindings) ready and calling the [Bindable.bind](http://docs.nativescript.org/ApiReference/ui/core/bindable/Bindable) method on the widget will work as expected, both in one-way and two-way cases.

# XML-Ready
Making the component visible to the XML parser is as easy as adding a custom namespace at [Page level](http://docs.nativescript.org/ui-with-xml#custom-components). The following code illustrates this:

```xml
<Page
    xmlns="http://www.nativescript.org/tns.xsd"
    xmlns:numPicker="number-picker"
    loaded="pageLoaded">
  <StackLayout id="rootLayout">
    <Label text="{{ value }}" cssClass="title"/>
    <Button text="TAP" tap="{{ tapAction }}" />
    <Label text="{{ value }}" cssClass="message" textWrap="true"/>
    <numPicker:NumberPicker value="{{ value }}" horizontalAlignment="left"/>
  </StackLayout>
</Page>
```

> The CLI will copy the JavaScript part of the plugin within the tns_modules folder. The XML parser will automatically check for `tns_modules/number-picker` folder to load the widget.
