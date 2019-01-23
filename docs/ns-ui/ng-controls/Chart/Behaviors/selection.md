---
title: Selection
page_title: Chart Selection behavior | Progress NativeScript UI Documentation
description: A page describing the selection behavior in Telerik Chart for NativeScript
slug: chart-selection-angular
tags: chart, behavior, selection, series, datapoint, angular
position: 1
publish: true
---

# Selection

You can make your charts more interactive by enabling selection. 
When selection is enabled you can select, deselect and handle the selection events of either the data points or the series in [RadCartesianChart]({% slug chart-types-cartesian %} "Read more about RadCartesianChart") and 
[RadPieChart]({% slug chart-types-pie %} "Read more about RadPieChart"). 

The selection can be set on the whole chart with the {% typedoc_link classes:RadChartBase,member:seriesSelectionMode %} and {% typedoc_link classes:RadChartBase,member:pointSelectionMode %} properties. The values for these properties can be:
* *None* - disable selection
* *Single* - single selection
* *Multiple* - multiple selection

For finer tuning of the selection behavior you can also set the {% typedoc_link classes:ChartSeries,member:selectionMode %} property of each series. The individual series selection mode enables you to specify multiple data point selection for one series, single data point selection for another series and even disable selection for a third series all the same time. With the combination of the chart selection properties and series selectionMode property, any selection scenario can be implemented. The series selectionMode property can have the following values:
* *None* - disable selection for the series
* *NotSet* - fall back to the global chart selection properties
* *DataPoint* -  single data point selection
* *DataPointMultiple* - multiple data point selection
* *Series* - whole series selection

## Handling selection events

To notify you when the selection state of an item is changed, **RadChartView** exposes the following events:
- `seriesSelected` - fired after a series is selected. 
The event data argument provides information about the event name, the chart that the series belongs to and the *ChartSeries* class instance for the selected series
- `seriesDeselected` - fired after a series is deselected. 
The event data argument provides information about the event name, the chart that the series belongs to and the *ChartSeries* class instance for the selected series
- `pointSelected` - fired after a data point is selected. 
The event data argument provides information about the event name, the chart that the data point belongs to, the point index and the native data point class instances(TKChartData for iOS, DataPoint for Android)
- `pointDeselected` - fired after a data point is deselected. 
The event data argument provides information about the event name, the chart that the data point belongs to, the point index and the native data point class instances(TKChartData for iOS, DataPoint for Android)

## Example 
The following example demonstrates how to enable data point selection 
<snippet id='angular-datapoint'/>

## Example 
The following example demonstrates how to enable series selection 
<snippet id='chart-angular-series-selection'/>


