## Exercise 07 - Form Creation

In this exercise you will create a new Contens form for CMS backend IO of the `training_event_[yourname]` table. In the real world, these forms are used by editors to manage all kinds of application data.

There are several steps to a successful form generation. In the last exercise you've covered step 1, generating model files. Now we'll cover all the other steps. Since we're using the generated model files for Contens backend applications now, we need to configure the model generator a little differently.

1. Edit the model generator for `training_event_[yourname]`.
1. On the first page, for "Automatically initialize data source in model file", set "application variable containing alternative data source information" and input "app_modules" into the text field.
1. Go to page 6 and set "Insert label management" to Session. Save & close.
1. Generate and deploy the model files.
1. Next up, creating the module for the backend form. On System > Modules click "Add".
1. Change the following:
	- Module name: "Training_[yourname]_dynamicevents"
	- Description: "Training - Dynamic Events"
	- Code name: "training_[yourname]_dynamicevents"
	- Module type: "Webpage application (Frontend)" (yes, for Contens this still counts as a frontend application even though we call it a backend one)
	- Controller folder: Project
	- Controller name: `training_[yourname]_dynamicevents.cfm`
	- Main table: training_event_[yourname]
	- Application storage folder: `Project /_apps/`
	- File extension of generated page: "cfm"
	- Display mode: dynamic  
	Save & Close
1. Now we'll create the actual form. On Generators > Forms click "Add".
1. Change the following:
	- Form name: "Training - [yourname] - DynamicEvents"
	- UI file path: "training/[yourname]/dynamicevents/"
	- Controller file name: "training_[yourname]_dynamicevents"
	- Language setting: Editor object languages
	- Application: Training_[yourname]_dynamicevents
	- Table: training_event_[yourname]
1. Switch to the "List structure" tab first.
	1. Add a "List header" element and set "Headline" to "Dynamic Events". 
	1. As a child of the new "List header", add a "List layout" element. Set "Layout name" to "List".
	1. As a child of the new "List layout", add the following "Layout elements": ID, Title, Start and End
		- In "List of database table fields" you need to prefix the table column with the table, so ID would be `training_event_[yourname].id`
		- Table field of columns that reference text tables are suffixed with a "txt". E.g. "titletext255_IDtxt"
		- In "Formatted output" you can put any CFML code that will render the field of the specific column. The data comes from the `qList` query scope.
		- Set the ID column as "Primary sort column" and the others as "Sortable column" as appropriate
1. When the list is done, switch to the "Form structure" tab.
	1. Insert a "Form page" element. In "Tabs_headline" set "training_event_[yourname]". Set "useButton_Apply" and "useButton_Save" both to "yes".
	1. As a child of the new "Form page", add a "Global Form Section" element. Set "Section" and "Section headline" to "training_event_[yourname] Container".
	1. As a child of the new "Global Form Section" element, add a "Field Group" element. Set "Section" and "Section headline" to "General".
	1. As a child of the new "Global Form Section", add rowtypes for: ID, Title, Description, Start, End and Link
		- Use the appropriate rowtypes. Description can be a "FCK Editor", ID is only "Anzeige (Display)", etc.
		- For each rowtype, choose a "simple" Output type, when available. For "Anzeige" choose `display.cfm`.
		- In "Rowkeyname" and "column_text1" of each rowtype set the current column name (no "txt" suffixes this time).
		- Always set the "title" of each rowtype, this is what the editors will see when editing / inserting a record.
		- Set "isRequired", "Multilingual", "maxLength" etc. as appropriate.
1. When finished with the form setup, press Save & Close. In the list of forms, right-click on your new form and select "Generate and deploy". Another way to generate the forms is using the `generate_form` tool located in the `tools` directory. If you encounter any issues while generating the forms in Contens, using this tool can reveal more information about the problem as well as the location of the log file.
1. The next step is creating the menu entry to display the form in the Content library tab. On Modify > Menu items click "Add".
1. Change the following:
	- Menu item: "DynamicEvents - [yourname]"
	- Status bar text: "DynamicEvents"
	- Position in menu tree: select the "Training" node under "Content" and set "insert item" to "at sub level"
	- Application: Training_[yourname]_dynamicevents
1. Now what's left is to set the proper access rights so that anybody can view and click the menu entry, add new records and edit old ones (and possibly delete some records). On Security > Actions click "Add".
1. Change the following:
	- Application: Training_[yourname]_dynamicevents
	- Description: "Training_[yourname]_dynamicevents menu"
	- Table: training_event_[yourname]
	- Position in action tree: at sub level of CONTENS ROOT
	- System rights assignable: Yes
	- Code name: "Training_[yourname]_dynamicevents menu"
1. Create additional actions and set them all at sub-level of your new "menu" action. For codename (and description) replace "menu" with "new" (for the create-action) or "edit" (for the edit-action). Other settings are the same as for "menu". 
1. This should now all be setup so that it's possible to view the records, add new and edit old ones. Test it out! See below for tips on debugging if something doesn't work.

**Additional assignments:**
- Since we've changed the generated models, it's probably not possible anymore to use the `datamanager` in your (frontend) application code. Change the code so that the application still works as expected (even in manage mode) - you will probably need to use the other `model/orm` components of the fritz framework for the complete functionality.
- When editing a `training_event_[yourname]` record in the backend, the header will at this point probably just contain the record's ID. Figure out how you can replace it with the event title. The existing backend applications like UaK or Theatre Library may give away the configuration needed for this.
- Add deleting of entries! For this, besides adding the appropriate Action(s), you'll probably need to create a `customcode` file for your form. Take a look at `goethe/_customcodes/uniform/theatre_play.cfm` and copy the `xmldata` snippet to your own `customcode` file. When changing the `customcode` file, you need to re-generate the form.

**Debugging and general hints:**
- There are many parts that make the forms work: model generator, module, form, menu entry, access rights (actions). If you don't see a change when modifying one, it could help recreating/reloading/purging the other parts as well. For example some model generator changes require you to also re-generate the form.
- Having the model generator or form configurations opened in two separate browser tabs can lead to weird errors.
- Use the `purgefactory` tool (`tools` directory) to purge files that seem to be cached. You can purge generated model files and also forms. Do a page search for your module's/form's/models' name to find what you can purge. Never click "purge all".
- Some error messages contain stack traces. It may help to inspect the generated files using the `filer` tool.
- Generated files can be deleted manually, if you have a feeling that a re-generation doesn't overwrite the old ones correctly. To see what the generated files are, use the `generate_form` or `generateallmodelfiles` tools.
- When you've tried everything from the above, but you still have a feeling that something is being cached, and your changes aren't being reflected, you can use the `reloadapp` tool (also in the `tools` directory) as a last resort. This re-initializes Contens as a whole and in some cases can get rid of some cached settings and/or files. **NEVER** use this tool on the live system! It can disable the system for many minutes and there's people working with the CMS everywhere around the world and around the clock! Also be warned, opening the reloadapp tool page will *immediately* start the re-init, there's no "Go" button.