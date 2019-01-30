---
title: Getting started
page_title: Getting Started with Progress NativeScript UI for Vue | Progress NativeScript UI Documentation
description: This is a getting started guide for using NativeScript UI with Vue
slug: getting-started-vue
tags: getting started, vue, ui, nativescript
position: 2
publish: true
---
# Getting Started
The following article assumes that you already have the latest version of NativeScript and have [created a NativeScript application]({% slug introduction %}).

## Installation

- Open a console window and go to the root directory of your NativeScript application.
- Install the package that contains the component that you want to use by typing nativescript command that adds a plugin. For example, if you want to use the chart, type  
````
$ tns plugin add nativescript-ui-chart
````
- Then import/require the installed component in your app

```TypeScript
import * as chartModule from "nativescript-ui-chart";
```
```JavaScript
var chartModule = require("nativescript-ui-chart");
```

## Getting Started with Vue
To use a plugin which is part from Progress NativeScript UI with Vue you need to first install the corresponding plugin by executing `tns plugin add nativescript-ui-<plugin-name>`, for example `tns plugin add nativescript-ui-chart`

After installing a plugin simply request its `nativescript-ui-chart/vue` module:

```
const ChartVue = require('nativescript-ui-chart/vue');
```

Once the module is required inside the NativeScript + Vue application you can take advantage of its contents by utilizing the RadCartesianChart:

```
<RadCartesianChart>
    <LineSeries v-tkCartesianSeries :items="categoricalSource" categoryProperty="Country" valueProperty="Amount" />
    <CategoricalAxis v-tkCartesianHorizontalAxis />
    <LinearAxis v-tkCartesianVerticalAxis />
</RadCartesianChart>
```
