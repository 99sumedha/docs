---
title: View modes
page_title: RadCalendar view modes | Progress NativeScript UI Documentation
description: This article explains how to set the RadCalendar's view mode with Vue
slug: calendar-view-modes-vue
tags: calendar, vue, view, modes, nativescript, professional, ui
position: 5
publish: true
---

# RadCalendar View Modes
{% typedoc_link classes:RadCalendar %} supports the view modes exposed by the {% typedoc_link enums:CalendarViewMode %} enum. The following view modes are supported:

- {% typedoc_link enums:CalendarViewMode,member:Week %}
- {% typedoc_link enums:CalendarViewMode,member:Month %}
- {% typedoc_link enums:CalendarViewMode,member:MonthNames %}
- {% typedoc_link enums:CalendarViewMode,member:Year %}
- {% typedoc_link enums:CalendarViewMode,member:Day %}

To change the view mode of {% typedoc_link classes:RadCalendar %} you should use its {% typedoc_link classes:RadCalendar,member:viewMode %} property and set it to one of the aforementioned values.

```
import { CalendarViewMode } from 'nativescript-ui-calendar';

export default {
  name: 'ViewModes',
  description: description,
  template: `
  <Page>
    <StackLayout>
      <RadCalendar
        :viewMode="viewMode">
      </RadCalendar>
    </StackLayout>
  </Page>
  `,
  data () {
    return {
      viewMode: CalendarViewMode.Month,
    };
  },
  methods: {
    onWeekTap() {
      this.viewMode = CalendarViewMode.Week;
    },
    onMonthTap() {
      this.viewMode = CalendarViewMode.Month;
    },
    onMonthNamesTap() {
      this.viewMode = CalendarViewMode.MonthNames;
    },
    onYearTap() {
      this.viewMode = CalendarViewMode.Year;
    },
    onDayTap() {
      this.viewMode = CalendarViewMode.Day;
    },
  },
};
```
