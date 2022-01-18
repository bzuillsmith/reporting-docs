---
title: How to Add Bookmarks to a Report group
page_title: How to Add Bookmarks to a Report group | for Telerik Reporting Documentation
description: How to Add Bookmarks to a Report group
slug: telerikreporting/designing-reports/adding-interactivity-to-reports/bookmarks/how-to-add-bookmarks-to-a-report-group
tags: how,to,add,bookmarks,to,a,report,group
published: True
position: 2
---

# How to Add Bookmarks to a Report group

Add bookmarks to a report when you want to provide a customized table of contents or      	to provide customized internal navigation in the report. Typically, you add bookmarks to locations in      	the report to which you want to direct users. You can create your own strings to use as bookmarks,      	or, for groups, you can set the bookmark to the group expression. After you create bookmarks,      	you can add report items that the user     	can click to go to each bookmark. These items are typically text boxes or images. 

## Adding a bookmark to a Report group

1. In __Design view__  , right click outside the report sections, select View and open up the [Group Explorer]({%slug telerikreporting/designing-reports/report-designer-tools/desktop-designers/tools/group-explorer%}). Select a report group to which you want to add a bookmark. The properties for the selected group appear in the __Properties__ pane.

1. In the [BookmarkId](/reporting/api/Telerik.Reporting.Group#Telerik_Reporting_Group_BookmarkId) property, 
	type a string that is the label for this bookmark. Alternatively, click
	the ellipsis to open the __Expression__  dialog box to specify an expression that evaluates to text. 
	Typically for a group, the expression you type should be the group expression.

>note The [BookmarkId](/reporting/api/Telerik.Reporting.Group#Telerik_Reporting_Group_BookmarkId) can be any string, but it must be unique in the report. If the __BookmarkID__ is not unique, an action to the bookmark finds the first matching bookmark.

# See Also

 * [Bookmarks Overview]({%slug telerikreporting/designing-reports/adding-interactivity-to-reports/bookmarks/overview%})
 
 * [How to Add Bookmarks to a Report item]({%slug telerikreporting/designing-reports/adding-interactivity-to-reports/bookmarks/how-to-add-bookmarks-to-a-report-item%})
  
 * [How to Add Bookmarks to a Table group]({%slug telerikreporting/designing-reports/adding-interactivity-to-reports/bookmarks/how-to-add-bookmarks-to-a-table-group%})