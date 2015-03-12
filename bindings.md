---
nav-title: "NativeScript Data Binding"
title: "App: Data Binding"
description: "NativeScript Documentation: Data Binding"
position: 8
---

#Data Binding

##Overview

Data binding is the process of connecting application user interface (UI) to a data object (business model). With a correct data binding settings and if data object provides proper notifications then when data changes application UI will reflect changes accordingly (source to target). Depending on settings and requirements there is a possibility to update data from UI to data object (target to source).

> **source** is used as any business logic object, and **target** as any UI control (like TextField).

##Basic data binding concepts

Generally almost every UI control (since all controls are created with data binding in mind) could be bound to a data object. However there are few restrictions for data binding to work out of the box.

* Target object should be a successor of **Bindable** class.
* Target property should be a **dependency property** in order to use data binding from target to source (or two way data binding). A plain property could be used if there is no need of **twoWay** binding.
* Data (business) object should raise **propertyChange** event for every change in the value of the property.

##Direction of data flow

Part of the data binding settings is the way data (values) flows. NativeScript data binding supports following data transmissions.

* **OneWay** - this is a setting that indicates that a change in the source object will update target property, but a change in target property will not be propagated back to source (data object). Any update to the target property (except binding) will stop the binding connection. - this is the **default** option.

* **TwoWay** - this setting indicates that both changes in source object will be reflected to the target and changes from target will update the source object. Very useful in cases when user input should be handled (for example user writes a text - text is written within a TextField control and is updated to a underlying data property of the source object).
In order to use this option binding options should set **twoWay as true**. Following examples show how to do that.

##Creating a binding

* Creating binding in code.

In order to create a working binding first we should have a source object. Source object should raise **propertyChange** event for every change of any property. NativeScript has a built-in class that fulfills that requirement (Observable). Following is a code snippet that creates an observable object instance.

``` JavaScript
var observableModule = require("data/observable");
var source = new observableModule.Observable();
```
``` TypeScript
import observableModule = require("data/observable");
var source = new observableModule.Observable();
```

Creating a target object. For the sake of example we will also create a target object an instance of Bindable class (all UI controls derives from it).

``` JavaScript
var textFieldModule = require("ui/text-field");
var targetTextField = new textFieldModule.TextField();
```
``` TypeScript
import textFieldModule = require("ui/text-field");
var targetTextField = new textFieldModule.TextField();
```

Create a data binding.

``` JavaScript
var bindingOptions = {
	sourceProperty: "textSource",
	targetProperty: "text",
	twoWay: true
};
targetTextField.bind(bindingOptions, source);
source.set("textSource", "Text set via binding");
```
``` TypeScript
var bindingOptions = {
	sourceProperty: "textSource",
	targetProperty: "text",
	twoWay: true
};
targetTextField.bind(bindingOptions, source);
source.set("textSource", "Text set via binding");
```

This example will update **targetTextField.text** property with a *"Text set via binding"* value, **twoWay** option ensures that every change of the **targetTextField.text** property (via user input) will be stored within **source.text** property. The new value of the text property could be get via:

``` JavaScript
source.get("textSource");
```
``` TypeScript
source.get("textSource");
```

* Create a data binding in xml

	1. Create a source object. "source" object from the previous case (creating binding with code) will be used for following examples.

	2. Describe a binding within xml (using a mustache syntax).

``` XML
<Page>
	<StackLayout>{%raw%}
		<TextField text= {{ textSource }} />
{%endraw%}	</StackLayout>
</Page>
```

> Note: For an UI elements created with an xml declaration when data binding is set **twoWay** option is **true** by default.

With an xml declaration we set only properties names both for target (text) and source (textSource). The interesting thing here is that there is no source of the binding (actually it is not set directly).

##Binding source

The important part of the data binding is setting the source object. NativeScript data binding works with any object that emits a **propertyChange** event. On the process of creating binding source can be set as second parameter of the bind(bindingOptions, source) or could be omitted. In that case for source is used a special property named **bindingContext** of the Bindable class. The special about this property is that it is inheritable across the visual tree. This means that control can use the **bindingContext** (as source) of the first **parent** element with a explicitly set **bindingContext**. With the previous example **bindingContext** can be set either on Page instance or StackLayout instance and TextField will have a proper source for its "text" property binding.

``` JavaScript
page.bindingContext = source;
//or
stackLayout.bindingContext = source;
```
``` TypeScript
page.bindingContext = source;
//or
stackLayout.bindingContext = source;
```

* Create a data binding to an event in xml

There is an option to bind a function to execute on a specific event (MVVM command like). This option is available only through an xml declaration. The different part is that the source property should have an event handler function as value.

``` JavaScript
source.set("onTap", function(eventData) {
		console.log("button is tapped!");
});
page.bindingContext = source;
```
``` TypeScript
source.set("onTap", function(eventData) {
	console.log("button is tapped!");
	});
page.bindingContext = source;
```

and how xml will look like:

``` XML
<Page>
	<StackLayout>{%raw%}
		<Button text="Test Button For Binding" tap="{{ onTap }}" />
{%endraw%}	</StackLayout>
</Page>
```

> Note: Be aware that if there is an event handler function **onTap** within the page code behind ([more info about xml declarations](./ui-with-xml.md)), and **onTap** function within the **bindingContext** object then there will be 2 event handlers hooked for that button and both will be executed on tap event.

##Using expressions for bindings

A great way of using bindings is the option to create a custom expression (that will be evaluated every time when the source property is changed). Custom expression could help in cases when a certain logic should be applied to the UI while keeping the underlying business data and logic clear. To be more clear lets see a basic binding expression example.

``` XML
<Page>
	<StackLayout>{%raw%}
		<TextField text="{{ sourceProperty, sourceProperty + ' some static text' }}" />
{%endraw%}	</StackLayout>
</Page>
```

As seen from the example adding an expression extends a binding syntax a little bit. Actually it is a full binding syntax - first parameter is the source property (which will be listened for changes), second parameter is the expression that will be evaluated, there is one more (third) parameter which states if the binding is twoWay or not (as mentioned earlier by default xml declaration creates a `twoWay` binding). The result of the upper example is a TextField element that will display the value of the `sourceProperty` followed by " some static text" string.
Speaking of a `twoWay` binding there is a common problem that origins in the different way of storing and displaying data. For example date objects are generally stored as number or a sequence of numbers and this is not the best possible option for displaying date to the end user. Also there is another problem end user could type a date (in a way convenient to its culture for example using a `DD.MM.YYYY` format) and underlying data should convert it to a correct value. Lets see how this could be handled with NativeScript binding.

``` XML
<Page>
	<StackLayout>{%raw%}
		<TextField text="{{ testDate, testDate | dateConverter('DD.MM.YYYY') }}" />
{%endraw%}	</StackLayout>
</Page>
```
``` JavaScript
var dateConverter = {
	toView: function (value, format) {
		var result = format;
		var day = value.getDate();
		result = result.replace("DD", month < 10 ? "0" + day : day);
		var month = value.getMonth() + 1;
		result = result.replace("MM", month < 10 ? "0" + month : month);
		result = result.replace("YYYY", value.getFullYear());
		return result;
	},
	toModel: function (value, format) {
		var ddIndex = format.indexOf("DD");
		var day = parseInt(value.substr(ddIndex, 2));
		var mmIndex = format.indexOf("MM");
		var month = parseInt(value.substr(mmIndex, 2));
		var yyyyIndex = format.indexOf("YYYY");
		var year = parseInt(value.substr(yyyyIndex, 4));
		var result = new Date(year, month - 1, day);
		return result;
	}
}

source.set("dateConverter", dateConverter);
source.set("testDate", new Date());
page.bindingContext = source;
```

The above code snippet (both XML and JavaScript part) will display a date in a `DD.MM.YYYY` format (`toView` function), and when a new date is entered with the same format is converted to a valid `Date` object (`toModel` function). `Converter` object should have one or two functions (`toView` and `toModel`) executed every time when a data should be converted. `toView` function is called when data will be displayed to the end user as value of any UI view, and `toModel` function will be called when we have an editable element (like TextField) and user enters a new value. In case of one way binding `Converter` object could have only `toView` function or to be a function. All convert functions have an array of parameters where the first parameter is the value which will be converted and all other parameters are custom parameters defined in the converter definition.

> Remarks: Any run-time error within the converter methods (`toView` and `toModel`) will be handled automatically and application will not break, but data in view model will not be altered (in case of error) and an error message with more information will be logged to the console. Date converter is simplified just for the sake of the example and it is not supposed to be used as a fully functional converter from date to string and vice versa.

Converter can accept not only static custom parameters, but any value from the `bindingContext`. For example:

``` XML
<Page>
	<StackLayout>{%raw%}
		<TextField text="{{ testDate, testDate | dateConverter(dateFormat) }}" />
{%endraw%}	</StackLayout>
</Page>
```
``` JavaScript
...
source.set("dateFormat", "DD.MM.YYYY");
page.bindingContext = source;
```

##Stop binding

Generally there is no need to stop binding explicitly, since Binding object uses weak references which prevents any memory leaks. However there are some scenarios (business logic) where binding must be stopped. In order to stop existing data binding just call **unbind** method with target property name as argument.

``` JavaScript
targetTextField.unbind("text");
```
``` TypeScript
targetTextField.unbind("text");
```
More information about binding can be found in [API-Ref](./ApiReference/ui/core/bindable/Bindable.md).
