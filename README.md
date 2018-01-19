# AzureDataLakeStore-OMS-Monitoring-template
# Azure Data Lake Store Activity Logs
Azure Activity logs is available on subscription level. This includes a range of data from Azure Resource Manager operational data to updates on Service Health events. Activity Logs provide data about the operations on a resource. 
Activity Logs can be observed from DLS resource overview pane.
Client IP and username information collected from activity logs json
Diagnostics Logs Using OMS Workspace and Storage Account
Diagnostics logs can be configured using OMS Log Analytics , Event Hub or Azure Storage Account.
Process to enable Diagnostics Logs:
1	Create an OMS Log Analytics account if it’s a new requirement. Raise a request with Cloud Central operation with Charge code approval for creation of OMS workspace.
2	When Log Analytics is available, configure diagnostics settings as below by selecting newly created workspace.
3	Select “Archive to a Storage Account” and Keep the logs in Storage Account by selecting a storage account which has been created for same purpose.
4	Click on Save.
5	Finally changes will take effect on the Diagnostics settings.
# Retention of Logs
We can configure retention of logs on only Storage account. Retention of logs in OMS Log Analytics depends on OMS log retention settings managed separately.
Storage Account retention period :
Audit Log : 180 Days.
Requests : 30 Days.
All Metrics : 30 Days.
#Log Analysis
OMS Log analysis process :
Data Type	
All data collected for Data Lake Store
	search *
| search "*MICROSOFT.DATALAKESTORE*"
Type of Data sent to OMS
search *
| search "*MICROSOFT.DATALAKESTORE*"
| summarize by Type

Type of Data sent to OMS	search *
| search "*MICROSOFT.DATALAKESTORE*"
| summarize by Type
Number of Data Lake reporting to DataLake	search *
| search "*MICROSOFT.DATALAKESTORE*"
| where ( Type == "AzureDiagnostics" or Type == "AzureMetrics" )  
|  summarize AggregatedValue = count() by Resource | count
Search by Operation Type	search * | search "*MICROSOFT.DATALAKESTORE*" | where ( Type == "AzureDiagnostics" or Type == "AzureMetrics" )  |  where ( Category == "Audit" or Category == "Requests" ) | summarize AggregatedValue = count() by OperationName
Success Operation Count	search * | search "*MICROSOFT.DATALAKESTORE*" | where ( Type == "AzureDiagnostics" or Type == "AzureMetrics" ) | where ( Category == "Audit" or Category == "Requests") | where ( ResultType == "Success" )  
|  summarize AggregatedValue = count()
Failure Operation Count	search * | search "*MICROSOFT.DATALAKESTORE*" | where ( Type == "AzureDiagnostics" or Type == "AzureMetrics" ) | where ( Category == "Audit" or Category == "Requests") | where ( ResultType == "Failure" ) 
|  summarize AggregatedValue = count()
Identity Users for last 24 Hours	search *  and TimeGenerated > ago(24h)| search "*MICROSOFT.DATALAKESTORE*" | where ( Type == "AzureDiagnostics") | summarize AggregatedValue = count() by identity_s
Data Lake Metrics for Data Written	search *
| search "*MICROSOFT.DATALAKESTORE*"
| where ( Type == "AzureMetrics" )
| where ( MetricName == "DataWritten" )
|  summarize AggregatedValue = count() by Resource
All Metrics collected for all Data Lake Store	search *
| search "*MICROSOFT.DATALAKESTORE*"
| where ( Type == "AzureMetrics" ) | summarize AggregatedValue = count() by MetricName
	
	
6.5	OMS Custom Solution	
See .omsview for custom solution developed for Data lake Store monitoring.
Solution should be Viewed as below:
Overview Tile and Dash Board	View
DISTRIBUTION BY DATA LAKE STORE	 
DISTRIBUTION BY OPERATION TYPE	 
USER AUDIT INFORMATION	 
DATA LAKE STORE METRICS	 
DISTRIBUTION BY METRICS	 
	
