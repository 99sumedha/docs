---
nav-title: "Class ui/layouts/layout-base.LayoutBase"
title: "Class ui/layouts/layout-base.LayoutBase"
description: "Class ui/layouts/layout-base.LayoutBase"
---
## Class: "ui/layouts/layout-base".LayoutBase  
_Inherits:_ [_CustomLayoutView_](../../../ui/core/view/CustomLayoutView.md)  
Base class for all views that supports children positioning.

##### Static Properties
 - **clipToBoundsProperty** - [_Property_](../../../ui/core/dependency-observable/Property.md).

##### Instance Functions
 - **getChildrenCount()** _Number_  
     Returns the number of children in this Layout.
   - _**return**_ - _Number_
 - **getChildAt(** index _Number_ **)** [_View_](../../../ui/core/view/View.md)  
     Returns the view at the specified position.
   - **index** - _Number_  
     The position at which to get the child from.
   - _**return**_ - [_View_](../../../ui/core/view/View.md)
 - **getChildIndex(** child [_View_](../../../ui/core/view/View.md) **)** _Number_  
     Returns the position of the child view
   - **child** - [_View_](../../../ui/core/view/View.md)  
     The child view that we are looking for.
   - _**return**_ - _Number_
 - **addChild(** view [_View_](../../../ui/core/view/View.md) **)**  
     Adds the view to children array.
   - **view** - [_View_](../../../ui/core/view/View.md)  
     The view to be added to the end of the children array.
 - **insertChild(** child [_View_](../../../ui/core/view/View.md), atIndex _Number_ **)**  
     Inserts the view to children array at the specified index.
   - **child** - [_View_](../../../ui/core/view/View.md)
   - **atIndex** - _Number_  
     The insertion index.
 - **removeChild(** view [_View_](../../../ui/core/view/View.md) **)**  
     Removes the specified view from the children array.
   - **view** - [_View_](../../../ui/core/view/View.md)  
     The view to remove from the children array.
 - **removeChildren()**  
     Removes all views in this layout.
 - **_registerLayoutChild(** child [_View_](../../../ui/core/view/View.md) **)**  
     INTERNAL. Used by the layout system.
   - **child** - [_View_](../../../ui/core/view/View.md)
 - **_unregisterLayoutChild(** child [_View_](../../../ui/core/view/View.md) **)**  
     INTERNAL. Used by the layout system.
   - **child** - [_View_](../../../ui/core/view/View.md)
 - **eachLayoutChild(** callback _Function_... **)**  
     Calls the callback for each child that should be laid out.
   - **callback** - _Function_(child [_View_](../../../ui/core/view/View.md), isLast _Boolean_)  
     The callback