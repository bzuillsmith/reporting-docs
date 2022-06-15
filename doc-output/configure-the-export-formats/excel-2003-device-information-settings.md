---
title: Excel 2003 Device Information Settings
page_title: Excel 2003 Device Information Settings 
description: Excel 2003 Device Information Settings
slug: telerikreporting/using-reports-in-applications/export-and-configure/configure-the-export-formats/excel-2003-device-information-settings
tags: excel,2003,device,information,settings
published: True
position: 5
---
<style>
table th:first-of-type {
    width: 15%;
}
table th:nth-of-type(2) {
    width: 10%;
}
table th:nth-of-type(3) {
    width: 75%;
}
</style>

# Excel 2003 Device Information Settings

The following table lists the device information settings for rendering in Excel 97-2003 (XLS) format.

## Excel Device Information Settings

|__Name__|__Type__|__Description__|
| ------ | ------ | ------ |
|UseNativePageHeader|Boolean|Specifies whether to render the report’s page header in the native Excel page header section. When __false__ (default value) the page header is rendered as a sequence of frozen rows at the top of the worksheet.|
|UseNativePageFooter|Boolean|Specifies whether to render the report’s page footer in the native Excel page footer section. When false, the footer is rendered as sequence of excel cells. Default value is __true__.|
|ShowGridlines|Boolean|Specifies whether Excel Gridlines are visible. Default value is __false__.|
|UseExtendedFormatting|Boolean|Format strings used throughout the report will be hard-coded with "Custom Format" in Excel which includes the locale information of the server. This way the formatting from the report definition is preserved and does not depend on the client machine locale where the document is opened. The custom format information can be omitted by setting __UseExtendedFormatting = false__. Default value is __true__.|

For an example of how to set up the settings for a rendering extension, see [extensions Element]({%slug telerikreporting/using-reports-in-applications/export-and-configure/configure-the-report-engine/extensions-element%}). 

## See Also

* [Device Information Settings]({%slug telerikreporting/using-reports-in-applications/export-and-configure/configure-the-export-formats/overview%})

* [Export Formats]({%slug telerikreporting/using-reports-in-applications/export-and-configure/export-formats%})