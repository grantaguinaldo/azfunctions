## Cosmos DB Install 

 Go to Cosmos DB and add `Azure Cosmos DB` to your resource group.
 
 Name your service `Enter Account Name`, select `API` as `Core (SQL)`, `Notebooks Preview = Off`,  `Location = (US) West US`, `Account Type = Non-Production`, `Geo-Redunsance = Disable`, `Multi-region Writes = Disable`. 

Once you have these settings, click  `Review + create`.
 
Review the instance and click `Create`.

Inside of `__init__.py`, we'll have to modify the function to include `documentsMetadata:func.Out[func.Document]` as part of the input into the `main` function. 

Inside of `function.json` you'll need to add as a binding: 
`{
"type":  "cosmosDB",
"direction":  "out",
"name":  "documentsMetadata",
"databaseName":  "nasakmaskills",
"collectionName":  "documents",
"connectionStringSetting":  "nasakmaskillsCosmosDB"`

Be sure that you add the connection string to `local.settings.json`

### Timestamp: 36:30
Need to ask about how to set the connection string as an `os` variable in Azure. 
- Go to Azure Function and set configuration under:
	- Configured features -> Configuration
	- The variables in `Application Settings` are accessible from `os.environ`.

Under `Connection strings` is where you add the Cosmos DB connection string. `+ New connection string`From there, you add in your connection string from Cosmos DB.

Need to update connection string in `local.settings.json` you'll need to add a new key  to hold the connection string. Timestamp 38:03

`{
"IsEncriped: false,
"Values" : {
	"AzureWebJobsStorage":,
    "FUNCTIONS_WORKER_RUNTIME":
    "documentsCosmosDB" <enter connection string from Cosmos DB>
	}
}`

Go to Azure Portal and into the Cosmos DB instance. 
Add database name from `function.json` file. In this case it's `azfuncCosmosDB`

Check `Provision database throughput`.
`Throughput Manual`
`Container id`is the name of the `collectionName` inside of the `function.json` configuration. 
In this case, the collectionName is "documents".

### Timestamp: 42.24.

Under Partition key add in `/paritionKey`
**Question** How to add more metadata fields to the partitionKey?
Press `OK`

Write to CosmosDB from the az functions script by:

`outdata['id'] = '0'`
`outdata = ['partitionKey'] = outdata['id']`
`documentsMetadata.set(func.Document.from_dict(outdata))`

### Timestamp: 45:34

To write to the CosmosDB instance, we need to add another block to the `function.json` file:

` {
"type":  "cosmosDB",
"direction":  "in",
"name":  "stateMetadata",
"databaseName":  "azfuncCosmosDB",
"collectionName":  "documents",
"connectionStringSetting":  "documentsCosmosDB",
"sql": "SELECT * FROM c WHERE c.id='state'"
} `

### Timestamp: 47:56

In the az function itself, we need to add `stateMetadata: func.InputStream` into the main function block as an input. 

Inside of the main az function, you need to call `stateMetadata.` to work with the state inside of the function. 

The work flow inside of the az function code is:

`Read data from Cosmos DB input, stateMetadata`

generate metadata output
`outdata['id'] = 'state'`

How to have CosmosDB run the function from the last state?

### Timestamp: 52:41.

End
