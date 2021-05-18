## Exercise 06 - Model generation

In this exercise you will generate model files for the `training_event_[yourname]` table using the Contens model generator. Later these model files will be used for implementing a backend application for the table, but for now, you will test the generated models using your `dynamicevents` (frontend) application.

1. In Contens, go to Administration > Generators > Model Generator and click "Add".
1. Select `app_modules` as the table source. Select `training_event_[yourname]` as the table. If you're not able to find it, a model generator for this table exists already. In that case, find it in the list and edit it.
1. Make sure the following settings are made:
	- Page 1:
		- Table type: Admin table
		- File location: Module/Project model
		- Automatically initialize data source in model file: application variable containing alternative data source information - `goethe._apps.training.[yourname].dynamicevents`
		- Allocate access rights at record level: No, all records accessible
		- Activate caching: No
		- Estimated average data volume: <= 500
	- Page 2:
		- PK: column `id`
		- Stand.Read: all columns
		- Ext.Filter: columns `start`, `end` and `titletext255_ID`
		- FT-Search: columns `titletext255_ID` and `descriptionlangtext_ID`
		- Langtext: `training_text255` for `titletext255_ID`, `training_langtext` for `descriptionlangtext_ID`
		- FK Table: no columns
		- Autonumber: column `id`
	- Page 3: leave blank
	- Page 4:
		- Sort order (column list) and label: in the first row put "id" in every field
		- List view Pulldown-Filter SQL-Code: leave blank
	- Page 5: leave blank
	- Page 6:
		- Label column: "id"
		- Singular column(s) where a getIdBy...-method can be created: leave blank
		- Insert tree management: No
		- Insert sort order management: No
		- Insert checkout management: Yes
		- Insert label management: Session
		- Detect ID for new data sets: SQL MAX(pk)+1
		- Insert archive management: No

	At the end, Save & close
1. Generate and deploy the model files by right-clicking on the new model generator in the list and selecting "Generate and deploy model files". Another way to generate them is using the `generateallmodelfiles` tool located in the `devtools` directory. If you encounter any issues while generating the model files in Contens, using this tool can reveal more information about the problem.
1. Verify that the model files are now located under `goethe/_modules/model/training_event_[yourname]/`. Use the `filer` tool!
1. Although the generated model files are best used by Contens for the backend applications, sometimes they can be helpful outside that context as well. Edit your `dynamicevents` application file.
1. Add a check for the URL parameter `manage`. When it is not present or when it evaluates to false or when the application is being displayed on the published page (the last condition can be checked using `request.stPagedata.info.viewmode < 1`), the application displays the events using the datalist framework component as before and in the usual style. Otherwise, we switch to a "manage" mode for that request.
1. In manage mode, instantiate the generated `datamanager` with  
	`new goethe._modules.model.training_event_[yourname].training_event_[yourname]datamanager()`
1. Before you can call functions on the `datamanager`, you need to make some additional configuration so that the model files know about your datasource. Open your `Application.cfc` and set the following application variables just like you set the variable `defaults` using the session manager:
	- `sDatasource`: `"app_modules"`
    - `sDbSys`: `"mysql"`
    - `sModelMapping`: `"goethe._modules."`
1. Now in your application files you can call the `findAll()` function of the `datamanager`. You'll have to supply the `editor_ID` parameter, but you can set it to "1". The result of this function is a struct containing your query result at the key `qList`. When in manage mode, the event limit application variable should be ignored (all events in the DB should be displayed), and the events should be displayed as a query dump only.
1. In manage mode, it should also be possible to insert new events. Implement a form that is shown only when in manage mode. The form should provide input fields for all fields of the `training_event_[yourname]` table, except for the ID (it's auto-generated).
1. In manage mode, to save event data that was input in the form, use the `datamanager` function `saveData()`. You must call it with a struct that contains a key for every column in the `training_event_[yourname]` table (note that for saving texts, the column name receives a suffix "txt", and the value is a struct with langID as key and the text as value), like this:

	```cfml
	{
	    id = 0,
	    start = "[start datetime of the event]",
	    end = "[end datetime of the event]",
	    link = "[event link]",
	    titletext255_IDtxt = { "[langId of the input text]" = "[title of the event in that language]" },
	    descriptionlangtext_IDtxt = { "[langId of the input text]" = "[description of the event in that language]" }
	}
	```
1. Make sure that newly added events are displayed during the same request. So every database operation in your application file should happen before any output.