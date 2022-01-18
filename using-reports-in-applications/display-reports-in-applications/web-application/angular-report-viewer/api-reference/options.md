---
title: Options
page_title: Options | for Telerik Reporting Documentation
description: Options
slug: telerikreporting/using-reports-in-applications/display-reports-in-applications/web-application/angular-report-viewer/api-reference/options
tags: options
published: True
position: 0
---

# Options



Below is a list of all options available during initialization.       

## Options

| Parameter | Description |
| ------ | ------ |
| __id__ | *string*, *optional*; Sets the unique identifier of the ReportViewer instance. If not specified,                 the __id__ of the target HTML element will be used (if such is set). The id of the ReportViewer is used to identify the client                 session of the viewer when __persistSession__ is enabled (true);|
| __serviceUrl__ | *string*, *required if*; Sets the address of the Report REST Service;|
| __reportServer__ | *JSON*, *required if*; Sets the configuration details for Telerik Report Server. Available properties:<br/>* __url__ (*string*, *required*) - the URL to the Telerik Report Server instance.<br/>* __username__ (*string*, *optional*) - a registered username in Report Server that will be used to get access to the reports. If omitted, the Report Server built-in __Guest__ account will be used;<br/>* __password__ (*string*, *optional*) - the password for the provided username. Can be omitted only when connecting with the __Guest__ account.|
| __templateUrl__ | *string*, *optional*; Sets the address of the html page that contains the viewer templates; If omitted, the default template will be downloaded from the REST service.|
| __reportSource__ | *JSON*, *optional*; Sets the report and initial report parameter values for the viewer to be displayed. Available properties:<br/>* *report* - a string that represents a report on the server.                     On the server side this string will be converted to a   [ReportSource](/reporting/api/Telerik.Reporting.ReportSource)                      through an   [IReportResolver](/reporting/api/Telerik.Reporting.Service.IReportResolver).                     That said this string may contain everything as far as the [Telerik Reporting REST Services]({%slug telerikreporting/using-reports-in-applications/host-the-report-engine-remotely/telerik-reporting-rest-services/overview%})knows how to convert to a reports source that will be passed to the report engine for further processing.                     For example this may be an assembly qualified type name that can be converted to   [TypeReportSource](/reporting/api/Telerik.Reporting.TypeReportSource),                     a path to a report definition file (.trdp/.trdx) as in the   [UriReportSource](/reporting/api/Telerik.Reporting.UriReportSource),                     or even a report id that a dedicated IReportSourceResolver will use to read the report definition stored in a database                     in xml format and converted to a   [XmlReportSource](/reporting/api/Telerik.Reporting.XmlReportSource).<br/>* *parameters* - a JSON object with properties name/value equal to the report parameters’ names and values used in the report definition;|
| __sendEmail__ | *object*, *required to show the*;                 Available properties:<br/>* __enabled__ (*bool*, *required*) - Indicates whether to show the Send Mail Message toolbar button. Default value: false;<br/>* __from__ (*string*, *optional*) - E-mail address used for the MailMessage FROM value;<br/>* __to__ (*string*, *optional*) - E-mail address used for the MailMessage TO value;<br/>* __cc__ (*string*, *optional*) - E-mail address used for the MailMessage Carbon Copy value;<br/>* __subject__ (*string*, *optional*) - A string used for the MailMessage Subject value;<br/>* __body__ (*string*, *optional*) - Sentences used for the MailMessage Content value;<br/>* __format__ (*string*, *optional*) - The preselected report document format.|
| __scale__ | *number*, *optional*; Sets the scale factor for the report pages.                 The scale takes effect when __scaleMode__ is set to *“SPECIFIC”*. Default value is 1.0 (100%);|
| __scaleMode__ | *string*, *optional*; Sets how the report pages to be scaled. Available options are:<br/>* *“FIT_PAGE_WIDTH”* - the pages are scaled proportional to fit the entire width in the viewer’s view port;<br/>* *“FIT_PAGE”* - the pages are scaled proportional to fit the entire page in the view port;<br/>* *“SPECIFIC”* - the pages are scaled with the __scale value__;Default value is: *“FIT_PAGE”*.|
| __viewMode__ | *string*, *optional*;                 Sets if the report is displayed in interactive mode or in print preview. The available values are:<br/>* *“INTERACTIVE”* - enables drill-down interactivity, etc;<br/>* *“PRINT_PREVIEW”* - the report is paged according to the page settings;For more information please see [Interactive vs. Print Layout]({%slug telerikreporting/using-reports-in-applications/display-reports-in-applications/interactive-vs.-print-layout%}). Default value is: *'Interactive'*.|
| __pageMode__ | *string*, *optional*;                 Sets if the report is displayed in Single page or Continuous scroll mode. The available values are:<br/>* *“SINGLE_PAGE”* - only one page is loaded in the view port;<br/>* *“CONTINUOUS_SCROLL”* - more than one page could be loaded in the view port;Default value is: *'CONTINUOUS_SCROLL'*.|
| __persistSession__ | *boolean* | *optional*. Sets whether the viewer’s client session                 to be persisted between the page’s refreshes(ex. postback). The session is stored in the browser’s  [sessionStorage](https://developer.mozilla.org/en-US/docs/Web/Guide/API/DOM/Storage) and is available                 for the duration of the page session. A page session lasts for as long as the browser is open and survives over page reloads and restores.                 Opening a page in a new tab or window will cause a new session to be initiated.The viewer’s state is persisted in the global sessionStorage object under a key defined by the viewer’s __id__.                 In order to enable the correct session to be loaded on the next page reload please use the same __id__ as in the first load. This means that if you need to persist the client session between page reloads you should                 set the viewer’s __id__ (or the id of the target element) to a constant value that should not be changed dynamically                 during the page lifecycle.Default Value is: *false*;|
| __parameters__ | *object*, *optional*;                 Allows user to defined parameters options for the report parameters.|

 Option | Description |
| ------ | ------ |
| __editors__ | *object*, *optional*; Allows user to defined editors type for the report parameters. The available editors are:

* __singleSelect__ (*string*, *optional*) - defineds the editor type for the single select parameters. The available values are:

   + *“COMBO_BOX”* - uses [Kendo UI ComboBox](https://docs.telerik.com/kendo-ui/api/javascript/ui/combobox) widget as an editor;

   + *“LIST_VIEW”* - uses [Kendo UI ListView](https://docs.telerik.com/kendo-ui/api/javascript/ui/listview) widget as an editor;Default value is: *'LIST_VIEW'* 

* __multiSelect__ (*string*, *optional*) - defineds the editor type for the multi select parameters. The available values are:

   + *“COMBO_BOX”* - uses [Kendo UI MultiSelect](https://docs.telerik.com/kendo-ui/api/javascript/ui/multiselect) widget as an editor;

   + *“LIST_VIEW”* - uses [Kendo UI ListView](https://docs.telerik.com/kendo-ui/api/javascript/ui/listview) widget as an editor;Default value is: *'LIST_VIEW'* 

|   |   |
| ------ | ------ |
|
| __parameterEditors__ | *array*, *optional*;                 Allows user to defined custom editors for the report parameters.|
| __authenticationToken__ | *string*, *optional*; If provided, a *Bearer* token will be set in the *Authorization* header for every request to the REST service.|
| __printMode__ | *string*, *optional*.                 Specifies how the viewer should [print reports]({%slug telerikreporting/using-reports-in-applications/display-reports-in-applications/web-application/html5-report-viewer/printing-reports%}).                 The available values are:<br/>* *“AUTO_SELECT”* - specifies that the viewer should automatically decide                     which option for printing to use depending on browser's version and whether the PDF plug-in is available                     or not. This is the default value;<br/>* *“FORCE_PDF_FILE”* - specifies that the document for printing will be                     downloaded as a file (in PDF format with a special 'print' script enabled);<br/>* *“FORCE_PDF_PLUGIN”* - specifies that the viewer should always use the PDF plug-in;|
| __selector__ | *string*, *optional*.                 A selector used in conjunction with the data- attributes.                 Whenever a command is attached to an element outside of the report viewer via the data-attributes this selector must be provided.|
| __disabledButtonClass__ | *string*, *optional*.                 A class used in conjunction with the data- attributes.                 Whenever a command is in the disabled state this class will be added to the respective button.|
| __checkedButtonClass__ | *string*, *optional*.                 A class used in conjunction with the data- attributes.                 Whenever a command is in the checked state this class will be added to the respective button.|
| __enableAccessibility__ | *boolean*, *optional*.                 Determines whether the viewer should provide support for accessibility features. You can find more detailed information [here]({%slug telerikreporting/using-reports-in-applications/display-reports-in-applications/web-application/accessibility%}).Default value: *false*;|
| __parametersAreaVisible__ | *boolean* | *optional*. Determines whether the viewer's parameters area is displayed if any parameter editor exists.Default value: *true*;|
| __documentMapVisible__ | *boolean* | *optional*. Determines whether the viewer's document map is displayed if any bookmark is defined.Default value: *true*;|
| __parametersAreaPosition__ | *string*, *optional*.                 Specifies where the Parameters Area should be displayed                 The available values are:<br/>* *“RIGHT”* <br/>* *“TOP”* <br/>* *“LEFT”* <br/>* *“BOTTOM”* Default value: *RIGHT*;|
| __documentMapAreaPosition__ | *string*, *optional*.                 Specifies where the Document Map should be displayed                 The available values are:<br/>* *“RIGHT”* <br/>* *“LEFT”* Default value: *LEFT*;|
| __searchMetadataOnDemand__ | *boolean*, *optional*.                 Determines whether the search metadata will be delivered on demand __(true)__ or by default __(false)__.Default value: *false*;|
| __initialPageAreaImageUrl__ | *string*, *optional*.                 The image URL for the PageArea background image. Used only when the parameter values are missing or invalid.                 The image should be in __PNG__, __GIF__, or __JPG__ file format.|
| __keepClientAlive__ | *boolean* | *optional*. Determines whether the client will be kept alive. When set to true expiration of the client will                 be prevented by continually sending a request to the server, determined by the Reporting REST service's __ClientSessionTimeout__.Default Value is: *true*;|
