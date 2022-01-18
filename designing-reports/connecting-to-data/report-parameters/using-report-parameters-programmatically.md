---
title: Using Report Parameters programmatically
page_title: Using Report Parameters programmatically | for Telerik Reporting Documentation
description: Using Report Parameters programmatically
slug: telerikreporting/designing-reports/connecting-to-data/report-parameters/using-report-parameters-programmatically
tags: using,report,parameters,programmatically
published: True
position: 3
---

# Using Report Parameters programmatically

In the report viewing application you can populate the values of          __ReportParameters__  collection members prior to displaying the report.

{{source=CodeSnippets\CS\API\Telerik\Reporting\Processing\ParameterSnippets.cs region=Pass_ReportParameter}}
````C#
Report1 report = new Report1();
report.ReportParameters["ManagerID"].Value = "123";
````
{{source=CodeSnippets\VB\API\Telerik\Reporting\Processing\ParameterSnippets.vb region=Pass_ReportParameter}}
````VB
Dim report As New Report1()
report.ReportParameters("ManagerID").Value = "123"
````

At runtime you can access the report parameters through the [Telerik.Reporting.Processing.Report.Parameters](/reporting/api/Telerik.Reporting.Processing.Report#Telerik_Reporting_Processing_Report_Parameters) dictionary. Each [Parameter](/reporting/api/Telerik.Reporting.Processing.Parameter) object contains resolved available values, current value and label. The label returns the currently selected DisplayMember from the available values (if available values are defined).

>note Prior to version 2010 Q1 the processing report exposes a dictionary containing only the current values of the report parameters.


##Example:

1. Create a __Telerik Report__  

1. Use the instructions from the [How to Connect to a SQL Database]({%slug telerikreporting/designing-reports/connecting-to-data/data-source-components/sqldatasource-component/how-to-connect-to-a-sql-database%}) article to bind to the __uspGetManagerEmployees__ stored procedure from the __AdventureWorks__ table.

1. Leave the Value for the __@ManagerID__ data source parameter empty and finish the wizard.

1. Create the following layout for the report  

  ![](images/DesignParameters008.png)

1. [Add a report 
   parameter]({%slug telerikreporting/designing-reports/connecting-to-data/report-parameters/how-to-add-report-parameters%}) and name it __ManagerID__.

1. Change its Type to Integer.

1. Set its Visible property to True.

1. Expand the __AvailableValues__ and bind it to __SqlDataSource__ component with
  the following query:

{{source=CodeSnippets\CS\API\Telerik\Reporting\Processing\ParametersSqlDataSourceQuery.sql}}
  ````SQL
SELECT  M.ManagerID, C.FirstName + ' ' + C.LastName AS Name
FROM    (SELECT DISTINCT ManagerID
	     FROM   HumanResources.Employee) AS M INNER JOIN
	            HumanResources.Employee AS E ON M.ManagerID = E.EmployeeID INNER JOIN
	            Person.Contact AS C ON E.ContactID = C.ContactID
````

1. Set the __ValueMember__  to __= Fields.ManagerID__.

1. Set the __DisplayMember__  to __= Fields.Name__.

1. Add grouping for the report with __= Fields.ManagerID__  as grouping expression.

1. Wire the [NeedDataSource event]({%slug telerikreporting/designing-reports/connecting-to-data/data-items/using-the-needdatasource-event-to-connect-data%}) of the report.

1. In the report.cs file set the __DataSource__  property
   to __null__  ( __Nothing__  in VB.NET) 
   so that NeedDataSource event is fired.

1. Add the following code to the NeedDataSource event handler:

{{source=CodeSnippets\CS\API\Telerik\Reporting\Processing\ParameterSnippets.cs region=Pass_Parameter_In_NeedDataSource}}
  ````C#
private void Report1_NeedDataSource(object sender, System.EventArgs e)
{
    //Take the Telerik.Reporting.Processing.Report instance
    Telerik.Reporting.Processing.Report report = (Telerik.Reporting.Processing.Report)sender;

    // Transfer the value of the processing instance of ReportParameter
    // to the parameter value of the sqlDataSource component
    this.sqlDataSource1.Parameters[0].Value = report.Parameters["ManagerID"].Value;

    // Set the SqlDataSource component as it's DataSource
    report.DataSource = this.sqlDataSource1;
}
````

{{source=CodeSnippets\VB\API\Telerik\Reporting\Processing\ParameterSnippets.vb region=Pass_Parameter_In_NeedDataSource}}
  ````VB
Private Sub Report1_NeedDataSource(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.NeedDataSource
    'Take the Telerik.Reporting.Processing.Report instance
    Dim report As Telerik.Reporting.Processing.Report = DirectCast(sender, Telerik.Reporting.Processing.Report)

    ' Transfer the value of the processing instance of ReportParameter
    ' to the parameter value of the sqlDataSource component
    Me.sqlDataSource1.Parameters(0).Value = report.Parameters("ManagerID").Value

    ' Set the SqlDataSource component as it's DataSource
    report.DataSource = Me.sqlDataSource1
End Sub
````

1. [Display the Report]({%slug telerikreporting/quickstart/displaying-reports-in-winforms-report-viewer%}) in a report viewer.

1. Select a parameter from the available values in the parameter editor 
   and click __Preview__.  

  ![](images/DesignParameters009.png)