## Exercise 02 - Page Conversion
### Part 2 - Creation of Output types and Templates in Contens

In this exercise you will create real Contens Output type and Templates that can be used anywhere in the CMS. For this, you will need the files from the last exercise.

1. Create a new Git branch from the branch "liveserver" and name it "training-[yourname]" (e.g. "training-tom")
1. Create the following new directories:
	- `goethe/_templates/training/[yourname]`
	- `goethe/_outputtypesextern/training/[yourname]`
1. Copy all `ot_*.cfm` files to the `goethe/_outputtypesextern/training/[yourname]` directory
1. Copy the rest of the CFM files that you've created to the `goethe/_templates/training/[yourname]/` directory.
   > **Note:** DON'T copy the resources directory, this was only so the page is displaying nicely on your local CF server!
1. Add the following as the first line to `gi-vn-start.cfm`:  
   ```cfml
   <cfinclude template="../../goethe/_includes.cfm">
   ```
1. Find where `resources/styles.001.css` is linked and change the `href` to `#resourcepath#css/styles.001.css`. The variable `resourcepath` is defined in the `_includes.cfm` file.
1. Now that the `ot_*.cfm` files are in a different directory, update their `cfinclude` paths in all of your `location_*.cfm` files. They should most likely begin with `../../../_outputtypesextern/training/[yourname]/`.
1. Replace all occurrences of <https://www.goethe.de/resources/relaunch/> in all your files by `#resourcepath#`
1. Commit and push your branch
1. Merge your branch into the "testserver" branch and change back to your branch
1. Go to <https://cms-test.goethe.de/goethe/contens/index.cfm#administration> (third tab)
1. On the "Administration" tab, select Modify > Output types and click the "Add" button
1. Fill in the following:
	- Name of output type: "Test | VN Startpage Teaser ([yourname])")
	- Path and file name: the path to your `ot_*.cfm` file that contains the code for one of the three Teasers at the very bottom of the page (should be like `training/[yourname]/ot_...`)
	- Object class: Teaser
	- Channel: add "HTML" and "Mobil" - both with "This template" as Substitute)
	- Use in Richtext editor: No  
 	Then Save & close
1. Select Modify > Templates and click the "Add" button
1. Fill in
	- Template name: "Test | VN Startpage ([yourname])")
	- Path and file name: `training/[yourname]/gi-vn-start.cfm`
	- Category: Main content templates > Main content
	- Channel: add "HTML" and "Mobil" - both with "This template" as Substitute  
	- On the Locations tab, add location "deutschlandUndWeltContent" that supports Teasers with Output type "Test | VN Startpage Teaser ([yourname])".  
    Save & close
1. Open your `gi-vn-start.cfm` file and find where `location_deutschlandUndWeltContent.cfm` is included via `<cfinclude>`
1. Replace that `<cfinclude>` by the contents of the `location_deutschlandUndWeltContent.cfm` file - in other words, dissolve the include
1. Replace all `cfincludes` of `ot_*.cfm` files with this one line:  
	```cfml
	<contens:location locationname="deutschlandUndWeltContent" />
 	```
	From now on, Contens will manage including the Output type CFMs in your template right where this `<contens:location>` tag is placed.
1. Add this as the first line in the file, otherwise that custom tag will not work:
   ```cfml
   <cfimport taglib="/contenscmstaglib/generator/pages/tags" prefix="contens">
   ```
   > **Note:** If you want to inspect the custom tag's code, you can. Hint: The logical path `/contenscmstaglib` is mapped via CF Admin to `/contens` in the wwwroot.
1. Since the `ot_*.cfm` files from the dissolved `location_deutschlandUndWeltContent.cfm` include will now be ready to be registered as Contens Output types, insert the following line as the first line in each of them:
   ```cfml
   <cfinclude template="../../goethe/_includes.cfm">
   ```
   This provides some common Output type functionality and sets useful variables.
1. Commit, push and merge into testserver
1. In Contens, create a new page in the site "Relaunch_Test" below "Start > Testverzeichnisse B&L > Training" called "VN Start - [yourname]". Folder name should be "vn_start_[yourname].cfm"
1. Open the Page properties and select the Format tab. There, choose "Test | VN Startpage ([yourname])" as the page's template. Save & close
1. Notice that the three Teasers at the very bottom of the page are not there anymore. That's because there are no objects in the location on the page.
1. Click somewhere in that location to create a new object (class: Teaser). Alternatively go to the List or Extended view where you can see the location and create an object from there.
1. Input some values in the required fields. For some fields you can select existing objects from the content library (especially useful for images). Save & close.
1. There should now be one teaser, however the data that you just input will not be displayed, since the Output type isn't getting the data from the object.
1. Open the Output type file and start replacing the hardcoded content with calls to `co.content("someKey")`
1. Determine the right keys for the data in Contens. Standard way: go to the object class definition (Administration > Objects > Object classes > Teaser > Structure) and find each field's "Keyname". "Hacky" way: open the object view, inspect the input rows and look for the ID attribute.
1. Use the following `co` functions to get the data from the objects:
	- `content()` - regular content, like single- or multiline text, radio buttons
	- `subObj()` - gets a complex subobject which in turn can be used to call `content()` on, for example like this: `co.subObj("image").content("image_file.link")`
	- `contentLength()` - gets the number of subobjects, or the length of the content from `content()` (for simple data) - useful for `cfif` and `cfloop`!
	- When accessing links, use `co.content("link.href")` to get the URL and `link.target` to get the target. When accessing images, use `co.content("image_file.link")` to get the image URL.
1. At the end each `ot_*.cfm` file should contain non-redundant CFML code to display any object of that class.

Always pay special attention to what branch you are working on! Commit code directly *only* into your branch ("training-[yourname]") and merge only to "testserver"!

**Additional assignment:** Register every Output type that you've created and generalize its code by getting the data from an object. Add the other locations to your template as well with the newly registered Output types. Hint: there are three Object classes represented on the page: Teaser, Article and Social Media Teaser. You can ask your colleagues if you have questions about accessing object data and general Template / Output type stuff!