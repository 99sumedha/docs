---
nav-title: "DatePicker How-To"
title: "How-To"
description: "Examples for using DatePicker"
---
# DatePicker
Using a DatePicker requires the "ui/date-picker" module.
``` JavaScript
var datePickerModule = require("ui/date-picker");
```
## Creating a DatePicker
``` JavaScript
var datePicker = new datePickerModule.DatePicker();
```
## Configuring a DatePicker
``` JavaScript
datePicker.year = 1980;
datePicker.month = 2;
datePicker.day = 9;
```
