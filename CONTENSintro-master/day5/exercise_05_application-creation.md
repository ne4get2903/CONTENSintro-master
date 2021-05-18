## Exercise 05 - Application Creation

In this exercise you will create an application for a dynamic list of events. 

1. Create an `index.cfm` file in `goethe/_apps/training/[yourname]/dynamicevents` with content that writes "App init successful!"
1. In Contens, go to Administration > System > Modules and click "Add"
1. Fill out / change the following:
	- Module name: "Training - [yourname] - DynamicEvents"
	- Description: "This module displays a dynamic list of events"
	- Code name: training-[yourname]-dynamicevents
	- Module type: Webpage application
	- Controller folder: Project
	- Controller name: `../_apps/training/[yourname]/dynamicevents/index.cfm`
	- Application storage folder: Project
	- File extension of generated page: "cfm"
	- Display mode: Dynamic  
	Save & close
1. Open Objects > Object classes and look for "Application Inline" OC. Edit it.
1. On the Structure tab, in the "app_id" row, add your new module to the "Application filter" list. This is a multiselect field so hold ctrl/cmd while clicking it in order to NOT deselect anything else in the process - important! After that, Save & close
1. Open Modify > Templates and edit your template "Test | VN Startpage ([yourname])"
1. Add a new location called "dynamiceventscontent". Add the "Application Inline" with the OT "Relaunch | Application Inline" to that location.
1. Open your page "VN Start - [yourname]" in the extended or list view
1. Add a new object of type "Application Inline" to the dynamiceventscontent location with your new module
1. Add output for this new location to your `gi-vn-start.cfm` template file. The new location should be displayed in the main column below the rest of the content.
1. Make sure the application is displaying now on your page, you should see the line "App init successful!"
1. Create a file called `appvariables.xml` in the `dynamicevents` directory, beside the `index.cfm`
1. Add the following configuration options to your application using the `appvariables.xml`:
	- Sorting: by event name alphabetically ascending or descending, by event start date ascending (default) or descending, random
	- Maximum number of events to display  
	Take a look at other `appvariables.xml` files in the `_apps/*` directories for inspiration.
1. The `appvariables.xml` now needs to be imported in Contens. Edit your module and replace `../` in the path in "Controller name" with `../../html/c40/goethe/`. Save & close
1. Right-click on the module and select "Import application variables". If you didn't do the previous step, this would now throw an error that the file could not be found.
1. When finished, change the "Controller name" path back to what it was before, i.e. reverse the step before the last one.
1. After importing the application variables, the Object class that you're using to put the application on the page needs to be created and deployed again - in this case OC "Application Inline".
1. Go to your page and edit the "Application Inline" object to see the new configuration options. If you don't see them, try creating a new "Application Inline" object and deleting the old one.
1. To use the application variables in your application code, you just need to read out the struct `stAppParam`.
1. The data for the events comes from the table `training_event_[yourname]` in the app_modules datasource. To get a list, you will use the `datalist` component of the fritz framework (in `modules/model`). In order to create objects of that component and take advantage of the built-in caching mechanisms, you will need a session manager. Create a new session manager in your `index.cfm` application file with
   ``cfml
   new fritz._includes.modulesession("goethe._apps.training.[yourname].dynamicevents")
   ``
1. The other thing you need for the datalist init function is a struct for the `defaults` argument.
1. Defining defaults should always be done globally for the whole application. For that, create a `Application.cfc` in your application's directory. Since we are in the context of a Contens module and the application isn't executed normally as in an ordinary CF environment, we need to stick to some framework standards. Add the following inside your `Application.cfc`:
   
	```cfml
	component accessors="true"
	{
	    property name="sessionmanager";
	
	    this.name = "training-[yourname]-dynamicevents";
	
	    public boolean function onFritzApplicationStart()
	    {
	        return true;
	    }
	
	    public boolean function onFritzSessionStart()
	    {
	        return true;
	    }
	
	    public boolean function onFritzRequestStart(string targetPage = "")
	    {
	        return true;
	    }
	}
	```
 
1. Define your defaults inside the `onFritzApplicationStart` function as follows:
	```cfml
	var _defaults = {
	    settings = {
	        datasource = "app_modules",
	        contensdb = "c40_goethe",
	        contenstargetdb = "c40_goethe"
	    },
	    langtexts = {
	        training_event_[yourname] = {
	            titletext255_ID = "training_text255",
	            descriptionlangtext_ID = "training_langtext"
	        }
	    }
	};
	```
1. Thanks to the way the framework is set up, you can now use the sessionmanager property (with `getSessionManager()`) and use it to pass data around in your application. Write the following in the `onFritzApplicationStart` function to save `_defaults` as an application variable:  
	`getSessionManager().setApplicationVariable("defaults", _defaults);`
1. Also define a global `i18n` object that you can use everywhere in the application:  
	`getSessionManager().setApplicationVariable("i18n", new fritz.modules.factory.textmanager.i18n());`
1. Now you have everything you need to create the `datalist` object. Supply the following arguments:
	- `defaults`: `_sessionmanager.getApplicationVariable("defaults")`
	- `tablename`: `"training_event_[yourname]"`
	- `langId`: use `request.stObjectData.instance.lang_ID` with a fallback to `request.stPageData.page.lang_ID` (it may happen that `stObjectData.instance.lang_ID` is undefined in `request`!)
    - `idname`: `"id"`
    - `sessionmanager`: the `modulesession` object you've created earlier
1. Load the list with `datalist`'s `loadList()` function and retrieve the loaded list with the `getList()` function.
1. Remove the output of the "App init successful!" string and instead output the event list so that it looks similar e.g. to the events on <https://www.goethe.de/ins/vn/en/kul/kus/ideenbooster.html>  
	For formatting the dates and times, use the formatter components under `fritz.modules.factory.formater` (`timeformater`/`dateformater`). For the `format` argument of both `timeformater.formatTime()` and `dateformater.formatDate()` functions, use the translated texts in bundles `ot.formats.time` and `ot.formats.dates` respectively.
1. Events whose end date lies before 2021-03-30 should not be displayed.
1. Implement reacting to the application variables for the maximum number of events and (at least) the random sorting. You can implement this either using ColdFusion functions on the result (array) or on the database level with SQL. For the latter, the `loadList()` function arguments `limit` and `sortfields` could be of interest to you.
1. Publish your page with the application in the english and vietnamese languages. Make sure the page is published as a CFM page.

**Find out:** Does it work putting multiple applications on one page?