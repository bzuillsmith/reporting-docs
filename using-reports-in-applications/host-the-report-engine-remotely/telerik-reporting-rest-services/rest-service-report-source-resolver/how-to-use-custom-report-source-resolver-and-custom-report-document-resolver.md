---
title: How to use Custom Report Source Resolver and Custom Report Document Resolver
page_title: How to use Custom Report Source Resolver and Custom Report Document Resolver | for Telerik Reporting Documentation
description: How to use Custom Report Source Resolver and Custom Report Document Resolver
slug: telerikreporting/using-reports-in-applications/host-the-report-engine-remotely/telerik-reporting-rest-services/rest-service-report-source-resolver/how-to-use-custom-report-source-resolver-and-custom-report-document-resolver
tags: how,to,use,custom,report,source,resolver,and,custom,report,document,resolver
published: True
position: 2
---

# How to use Custom Report Source Resolver and Custom Report Document Resolver



Most of [Telerik Report Viewers]({%slug telerikreporting/using-reports-in-applications/display-reports-in-applications/overview%}), utilize         [Telerik Reporting REST Service]({%slug telerikreporting/using-reports-in-applications/host-the-report-engine-remotely/telerik-reporting-rest-services/overview%}). They rely entirely on the latter to prepare and send the         requested report documents. The Reporting engine is exposed by the REST Service and all the work related to report processing and rendering is done by the engine.         The purpose of this article is to elaborate on how the report source information sent by the viewer is resolved to an object that can be processed by the Reporting engine,         and where and how the developer may customize this process.       

> We recommend using the built-in tools for report customization that Telerik Reporting offers. These are         
* [Expressions]({%slug telerikreporting/designing-reports/connecting-to-data/expressions/overview%})
* [Bindings]({%slug telerikreporting/designing-reports/connecting-to-data/expressions/using-expressions/bindings%})
* [Conditional Formattings]({%slug telerikreporting/designing-reports/styling-reports/conditional-formatting%})>The above tools allow you to use a variety of [Global Objects]({%slug telerikreporting/designing-reports/connecting-to-data/expressions/expressions-reference/global-objects%}) inside the report definition           to modify its appearance in run-time. For example, based on the incoming data, [Report Parameter]({%slug telerikreporting/designing-reports/connecting-to-data/report-parameters/overview%}) values           and logged-in user. In most of the reporting scenarios these tools should let you achieve your goals without custom code. One of the major benefits of this approach           is the much easier upgrade and maintenance of the Reporting part of your application. Our [Upgrade Wizard]({%slug telerikreporting/designing-reports/report-designer-tools/desktop-designers/visual-studio-report-designer/upgrade-wizard%})           takes care of all the code auto-generated by our report designers. The approach discussed in this article requires custom code that should be maintained manually.         

Let's start with some theory before showing two basic examples.       

## How does the REST Service know which report definition to use and what parameter values to apply?

The Report Viewer sends a [client-side reportSource]({%slug telerikreporting/using-reports-in-applications/display-reports-in-applications/web-application/html5-report-viewer/api-reference/reportviewer/methods/reportsource()%}) to the REST Service. It is an object with two properties           described next :         

* The __string__ property __report__ This is the report identifier. Server-side it will be used by the REST Service to identify the correct report definition to be used as a report template.             

* The __parameters__ dictionary with __string__ keys and __object__ values             The keys are the Report Parameter names, and the values are the corresponding values that need to be applied. The Reporting engine matches the parameter names               specified in the report definition with the keys in the __parameters__ dictionary when applying the values.             

The Reporting engine requires a [server-side ReportSource]({%slug telerikreporting/designing-reports/report-sources/overview%}) to generate a report document.           How does it get it from the client-side reportSource sent to the REST Service?         

The answer is in the [ReportSource resolver]({%slug telerikreporting/using-reports-in-applications/host-the-report-engine-remotely/telerik-reporting-rest-services/rest-service-report-source-resolver/overview%}) that implements the interface            [IReportSourceResolver](/reporting/api/Telerik.Reporting.Services.IReportSourceResolver). Its method __Resolve()__ is invoked with the           __reportSource.report__ passed as first argument in order to return the correct server-side ReportSource supposed to be used by the Reporting engine.         

The rest of the arguments of the __Resolve()__ method contain important information that may be used effectively. Let's clarify the meaning and           the intended usage of these additional arguments :         

* The  [OperationOrigin](/reporting/api/Telerik.Reporting.Services.OperationOrigin)  type __operationOrigin__             shows what is the purpose of the current call to the resolver.             The viewer makes a number of requests to the REST Service in order to let the user preview and select the desired parameter values for the report rendering,               and then to display the requested report document. The first request that requires report resolving is               [Get Report Parameters]({%slug telerikreporting/using-reports-in-applications/host-the-report-engine-remotely/telerik-reporting-rest-services/rest-api-reference/report-parameters-api/get-report-parameters%}). Its purpose is to check what parameters does the report definition have               and display the visible ones with their preset or default values. During this call to the Resolver, the __operationOrigin__ is set to               __ResolveReportParameters__.             The second call to the resolver is in response to the [Resolve Report Instance]({%slug telerikreporting/using-reports-in-applications/host-the-report-engine-remotely/telerik-reporting-rest-services/rest-api-reference/report-instances-api/resolve-report-instance%}) request from the viewer.               When successful, it results in instantiation of the report definition. The __operationOrigin__ is set to               __CreateReportInstance__.             The third call is associated with __operationOrigin__ equal to __GenerateReportDocument__ and is in response to the               [Resolve Document]({%slug telerikreporting/using-reports-in-applications/host-the-report-engine-remotely/telerik-reporting-rest-services/rest-api-reference/documents-api/resolve-document%}) request from the viewer. This is when the rendering begins               if the requested document is not found in the REST Service storage. Generally, each report document gets saved in the storage. If the client requests the same report with               the same parameter values, the report would be taken from the storage rather than being rendered again.             The __GetPageLayout__ value of the __operationOrigin__ is related to the PageSettings dialog of the viewers and is for internal use.             

* The __currentParameterValues__ dictionary contains the parameter values that would be applied when rendering the report.             This is a dictionary with the values of the report parameters. You may access the values set on the client with all __operationOrigin__ calls.               When opening the report for the first time with __operationOrigin__ equal to __OperationOrigin.ResolveReportParameters__,               the __currentParameterValues__ returns only the parameter values set from the client/viewer. At this point, the report hasn't been resolved yet               and the default parameter values haven't been read from the report definition.             Let's elaborate on handling the Report Parameter values. Generally, they may be set on three different places.             

   1. In the report definition, when you prepare or modify it. These are the default values.                 

   1. On the client-side reportSource of the viewer.                 

   1. On the server-side ReportSource returned by the REST Service.                 There are two appoaches for modifying the parameter values in the ReportSource resolver:             

   + You may edit directly the dictionary __currentParameterValues__ during __OperationOrigin.ResolveReportParameters__ The changes will override all the setting, including the values passed from the viewer. If you add to the dictionary parameters that don't exist in the report definition                   during this call, they will be ignored later. The same is valid for the non-existing parameters added from the viewer.                   The reason is that when the report definition is identified/resolved, the Reporting engine matches the parameters passed with                   the ReportSource with the parameters in the report definition by name. Those that don't match with report definition parameters get ignored.                 The dictionary should not be changed during the rest of the calls as this may lead to inconsistency of what is displayed in the viewer and the actual parameter values                    used during report processing, or even a failure in report generation.                 

   + Here is the priority of applying parameter values if you don't modify the __currentParameterValues__ dictionary directly.                 If you pass any parameter values from the viewer, the Reporting engine will use them with the highest priority. For the values that are not provided, it will utilize                   the default parameter values from the report definition. In this case, any parameter values set in the ReportSource returned by the resolver will be ignored. For example,                   if the report has two parameters, 'Category' and 'Id', and you set only the 'Id' from the viewer, the 'Category' value would be taken from the report definition.                   If the default 'Category' value is not set in the definition, the viewer will display an exception message even if you set this value in the ReportSource returnded                   by the resolver.                 If you don't pass any parameter values from the viewer, the Reporting engine will use the values assigned to the server-side ReportSource Parameters collection                   set in the resolver during __OperationOrigin.ResolveReportParameters__ pass. If you provide them later, they will be ignored and                   the values from the report definition will be used instead.                 

After this short introduction in the ReportSource resolver concept, we may demonstrate it in action with some sample implementations.         

## How to assign data source dynamically to a report and one of its data items

A common scenario that requires a custom ReportSource resolver is when you need to assign a data source dynamically to the report or other           [data items]({%slug telerikreporting/designing-reports/connecting-to-data/data-items/overview%}).           In this case, it is necessary to instantiate the report definition, set the data source to the report (and/or other data item) and return the modified report definition instance           wrapped in an  [InstanceReportSource](/reporting/api/Telerik.Reporting.InstanceReportSource). Here is sample code that demonstrates the approach:         

    
````c#
using System.Collections.Generic;
using System.IO;
using Telerik.Reporting;
using Telerik.Reporting.Services;
namespace MyReportSourceResolverDemo
{
    public class MyReportSourceResolver : IReportSourceResolver
    {
        public string ReportsPath { get; set; }
        public MyReportSourceResolver(string reportsPath)
        {
            this.ReportsPath = reportsPath;
        }
        public ReportSource Resolve(string reportId, OperationOrigin operationOrigin, IDictionary<string, object> currentParameterValues)
        {
            string reportPath = Path.Combine(this.ReportsPath, reportId);
            var reportPackager = new ReportPackager();
            Report report = null;
            using (var sourceStream = System.IO.File.OpenRead(reportPath))
            {
                report = (Report)reportPackager.UnpackageDocument(sourceStream);
            }
            if (operationOrigin == OperationOrigin.GenerateReportDocument)
            {
                // Set the data source for the report
                report.DataSource = MyService.GetMyData();
                // Set the data source for another data item
                var table1 = report.Items.Find("table1", true)[0] as Table;
                table1.DataSource = MyService.GetMyTableData();
            };
            return new InstanceReportSource
            {
                ReportDocument = report
            };
        }
    }
}
````

For the particular scenario, we pass the reports folder in the constructor of the custom ReportSource resolver so that the latter may find the report definitions.           We assume that the *reportId* is the report name, for example, 'MyReportName.trdp' that should be looked for in this folder.           We unpackage the report definition and on the proper request add the data source to the report and to its table. The server-side ReportSource that we return           is *InstanceReportSource* with the modified report object. What deserves to be acknowledged here is that data source assignment,           hence the data fetching, happens only when needed.         

## What happens next?

The server-side ReportSource, along with the parameter values, holds a reference to the report definition.         

In the  [UriReportSource](/reporting/api/Telerik.Reporting.UriReportSource)  this is the physical path to the TRDP/TRDX report or TRBP reportbook           and is held in the *Uri* property.         

For the  [TypeReportSource](/reporting/api/Telerik.Reporting.TypeReportSource)  this is the assembly qualified name of the CLR report definition, for example,           the CS report class inheriting from our class  [Report](/reporting/api/Telerik.Reporting.Report). It is in the *TypeName* property.         

The  [InstanceReportSource](/reporting/api/Telerik.Reporting.InstanceReportSource)  holds directly the report definition instance reference in its          *ReportDocument* property.         

And the  [XmlReportSource](/reporting/api/Telerik.Reporting.XmlReportSource)  has the XML of the report definition in its          *Xml* property. This is the same XML that describes the report and can be found also in the TRDX report definition and           inside the TRDP archive in the *definition.xml* file.         

From this reference to the report definition, the Reporting engine needs to extract an instance of the corresponding report. For example, from the Uri to the TRDP file           it needs to make a *Telerik.Reporting.Report* object. For this reason, it utilizes the            [IReportDocumentResolver](/reporting/api/Telerik.Reporting.Services.IReportDocumentResolver)  interface. There is a default implementation for each of the above           ReportSource types. For example, the document resolver for the *UriReportSource* utilizes the            [ReportPackager](/reporting/api/Telerik.Reporting.ReportPackager)  to instantiate the TRDP reports, and            [ReportXmlSerializer](/reporting/api/Telerik.Reporting.XmlSerialization.ReportXmlSerializer)  to deserialize the TRDX reports.         

In the REST Service, you may specify your own *IReportDocumentResolver* implementation. It should be set to the property           __ReportDocumentResolver__ of the  [ReportServiceConfiguration](/reporting/api/Telerik.Reporting.Services.ReportServiceConfiguration).           Why did we introduce it?         

The main reason was the way the [SubReports]({%slug telerikreporting/designing-reports/report-structure/subreport%}) got resolved by the service.         

Imagine a main report and subreport referenced inside it. When you use a Custom ReportSource Resolver in the REST Service, the main report would be resolved by it.           What about the subreport? Here comes the second example.         

## How to assign data source dynamically to a report and a subreport referenced in it

Let's consider a common scenario, where the main report definition XML would be fetched from a database. It contains a SubReport item with XML definition also held in the database.           In addition, we need to set data source run-time for both the main report and its subreport, and for a table in the subreport.         

How would the Reporting engine resolve the subreport in this case? Usually, the subreport would be referenced in a SubReport item as a server-side ReportSource,           for example, a TRDP report with relative path. This would represent the most common scenario when the reports were designed with the Standalone or Web Report designer.           In this case, the Reporting engine will try to resolve the subreport's UriReportSource in the context of the application. Importantly, the REST Service ReportSource resolver           won't be involved as the SubReport processing happens as part of the main report processing. This means that the Reporting engine will look in a particular folder           of the application, as it will use the default *IReportDocumentResolver* for the *UriReportSource*.           This is fine in some cases. However, in the general scenario, you would like to keep all your reports in the database rather than keeping the main ones in the database           and the subreports in a local folder. For this common scenario, the custom implementation of the *IReportDocumentResolver* would let you           resolve all the report documents in the same manner, for example, from the database. That said, the resolving of any server-side ReportSource          *ReportDocument* would pass through this custom *IReportDocumentResolver* rather than through the default one           for the corresponding ReportSource. This would let you, in the context of the example, fetch all of the report definitions as XML from the database and instantiate them           through the *ReportXmlSerilizer*.         

Here is a sample implementation of the *ReportSourceResolver* and the *ReportDocumentResolver* for the discussed scenario:         

    
````c#
using System.Collections.Generic;
using System.IO;
using Telerik.Reporting;
using Telerik.Reporting.Services;
using Telerik.Reporting.XmlSerialization;
namespace MyReportSourceResolverDemo
{
    public class MyReportSourceResolver : IReportSourceResolver
    {
        public ReportSource Resolve(string reportId, OperationOrigin operationOrigin, IDictionary<string, object> currentParameterValues)
        {
            ReportXmlSerializer xmlSerializer = new ReportXmlSerializer();
            Report reportInstance = (Report)xmlSerializer.Deserialize(new StringReader(MyService.GetMainReportXml(reportId)));
            if (operationOrigin == OperationOrigin.GenerateReportDocument)
            {
                // Set the data source for the main report
                reportInstance.DataSource = MyService.GetMyData();
            }
            var instanceReportSource = new InstanceReportSource
            {
                ReportDocument = reportInstance
            };
            return instanceReportSource;
        }
    }
    public class MyReportDocumentResolver : IReportDocumentResolver
    {
        public IReportDocument Resolve(ReportSource reportSource)
        {
            // The main report is wrapped in an InstanceReportSource by MyReportSourceResolver
            if (reportSource is InstanceReportSource)
            {
                return (reportSource as InstanceReportSource).ReportDocument;
            }
            // the subreport is resolved in the context of the main report SubReport
            else if (reportSource is UriReportSource)
            {
                ReportXmlSerializer xmlSerializer = new ReportXmlSerializer();
                Report report = (Report)xmlSerializer.Deserialize(new StringReader(MyService.GetSubReportXml((reportSource as UriReportSource).Uri)));
                // Set the data source for the subreport
                report.DataSource = MyService.GetMySubreportData();
                // Set the data source for another data item in the subreport
                var table1 = report.Items.Find("table1", true)[0] as Table;
                table1.DataSource = MyService.GetMyTableData();
                return report;
            }
            return null;
        }
    }
}
````

Note that we set the data source for the main report in the *IReportSourceResolver* implementation. The reason is that we can do this only once,           when generating the report document. Remember that the *Resolve* method of the *IReportSourceResolver* gets called           several times during the communication between the viewer and the service. In each of them, the *IReportDocumentResolver* is also called           when resolving the main report. Hence, if we set its data source in the *IReportDocumentResolver.Resolve* method, the data source would be set           every time, which is overhead.         

The resolving of the *SubReport.ReportSource*, on the other hand, happens once while processing the main report, and requires a single call           to its *IReportDocumentResolver*. For that reason, we may set the subreport data source in the latter without worrying this may           decrease the performance. Note that since we assumed the *SubReport.ReportSource* is a *UriReportSource* in the           main report definition, we need to fetch its XML based on the *Uri* property, deserialize it, set the data sources if necessary, and return           the final modified report instance.         

