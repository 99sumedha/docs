---
nav-title: "ActionBar How-To"
title: "How-To"
description: "Examples for using ActionBar"
---
# ActionBar
Using a ActionBar requires the action-bar module.
``` JavaScript
var actionBarModule = require("ui/action-bar");
```

## Setting Title and Icon
```XML
<Page>
  <Page.actionBar>
    {%raw%}<ActionBar title="{{ title }}" android.icon="res://ic_test"/>{%endraw%}
  </Page.actionBar>
  ...
</Page>
```
The icon can only be set in Android platform. Following the design guides it is automatically hidden in Lollipop versions (API level >= 20). You explicitly control its visibility with the `android.iconVisibility' property.


## Setting Custom Title View 
```XML
<Page loaded="pageLoaded">
  <Page.actionBar>
    <ActionBar title="Title">
      <ActionBar.titleView>
        <StackLayout orientation="horizontal">
          <Button text="1st" />
          <Button text="2nd" />
          <Button text="3rd" />
        </StackLayout>
      </ActionBar.titleView>
    </ActionBar>
  </Page.actionBar>
  ...
</Page>
```

## Setting Action Items
```XML
<Page>
  <Page.actionBar>
    <ActionBar title="Title">
      <ActionBar.actionItems>
        <ActionItem text="left"  ios.position="left"/>
        <ActionItem text="right" ios.position="right"/>
        <ActionItem text="pop"   ios.position="right"  android.position="popup"/>
      </ActionBar.actionItems>
    </ActionBar>
  </Page.actionBar>
  ...
</Page>
```

The position option is platform specific. The available values are as follows:
* **Android** - `actionBar`, `actionBarIfRoom` and `popup`. The default is `actionBar`.
* **iOS** - `left` and `right`. The default is `left`.

## Setting Navigation Button
```XML
<Page>
  <Page.actionBar>
    <ActionBar title="Title">
      <NavigationButton text="go back"/>
    </ActionBar>
  ...
</Page>
```

## Displaying Platform-Specific System Icons on Action Items
```XML
<Page>
  <Page.actionBar>
    <ActionBar>
      <ActionBar.actionItems>
         <ActionItem ios.systemIcon="12" android.systemIcon = "ic_menu_search" />
         <ActionItem ios.systemIcon="15" android.systemIcon = "ic_menu_camera" />
         <ActionItem ios.systemIcon="16" android.systemIcon = "ic_menu_delete" />
      </ActionBar.actionItems>
    </ActionBar>
  </Page.actionBar>
  ...
</Page>
```

### iOS
Set `ios.systemIcon` to a number representing the iOS system item to be displayed.
Use this property instead of `ActionItemBase.icon` if you want to diplsay a built-in iOS system icon.
The value should be a number from the `UIBarButtonSystemItem` enumeration
(https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIBarButtonItem_Class/#//apple_ref/c/tdef/UIBarButtonSystemItem)
0: Done
1: Cancel
2: Edit
3: Save
4: Add
5: FlexibleSpace
6: FixedSpace
7: Compose
8: Reply
9: Action
10: Organize
11: Bookmarks
12: Search
13: Refresh
14: Stop
15: Camera
16: Trash
17: Play
18: Pause
19: Rewind
20: FastForward
21: Undo
22: Redo
23: PageCurl

### Android
Set `android.systemIcon` the name of the system drawable resource to be displayed.
Use this property instead of `ActionItemBase.icon` if you want to diplsay a built-in Android system icon.
The value should be a string such as 'ic_menu_search' if you want to display the built-in Android Menu Search icon for example.
For a full list of Android drawable names, please visit http://androiddrawables.com

