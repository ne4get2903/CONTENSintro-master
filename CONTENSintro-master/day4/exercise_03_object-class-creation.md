## Exercise 03 - Object Class Creation

In this exercise you will re-create the whole contents of the "deutscheSpracheContent" location as one Object class. The Object class will need:

- A link input for the title ("German language")
- A text line input for the kicker
- An object input where you can insert up to three items. For the three items create another Object class with the following rows:
	- A Link for the title
	- Two links that are displayed underneath the title
	- Radio buttons to change the color. Possible values: green, blue, red
- A link for the bottom gray area
- A textfield (rich text) for the bottom gray area
- An optional image field that can be displayed in the bottom gray area (full width) - not seen on the original page


1. Go to Administration > Objects > Object classes and click the "Add" button
1. On the Properties tab, change the following:
	- Class name: "Training - [yourname] - GermanLanguage"
	- Class description: "Training - GermanLanguage"
	- Code name: "training_[yourname]_germanlang"
	- Category: Simple classes
1. On the Object tab, change the following:
	- Primary category pre-setting: "Für alle freigegeben" (near the bottom of the list)
	- Assign primary category: never
	- Assign page category: yes
1. On the Structure tab, configure the rows
	- For a link, use the row type "Link"
	- For a text line, use "Textline"
	- For an object input, use "Object" and in "selectclasses" set the desired object class (note: "selectclasses" is a multiselect field)
	- For a rich text field, use "FCK Editor"
	- For radio buttons, use "xRadiobuttons"
	- For an image, use "Object" and set the "selectclasses" object class to "Bild"
1. You need to set "Use as label" to `true` on one row (and only one) so that the CMS has something to show in the list views. If your new object class doesn't have a good row for this yet, it's customary to create a new Texline row just for this purpose. Remember, the rows you define here are only *possible* inputs, they don't have to be displayed in your Output type, if you don't want or need to.
1. Set some row types to required, as appropriate. Set some row types to multilingual, as appropriate (rule of thumb: if you are displaying the entered data as it is on the page, it should be multilingual).
1. Increase the number in the setting "subobjectlevel" of your Object row type to at least 2 or 3 (to be sure). If you don't do this, you will probably run into problems when accessing deeply nested subobjects.
1. When you're finished with the Object class configuration, Save & close.
1. Right-click on the new Object class in the list and select "Create and deploy class". This is important, otherwise you will not see or be able to use the new rows anywhere!
1. Create a new Output type file in the usual location (remember: branch "training-[yourname]"). Use the new Object class row's Keynames to get the data from an object. For the start you can just output one field (using `co.content("someKey")`) so that you can see something once you add an object to the template.
1. Register the Output type in Contens for the Object class. Name the OT "Test | VN Startpage Germanlanguage [yourname]"
1. Edit the "Test | VN Startpage ([yourname])" template in Contens. If not already present, create a new location "deutscheSpracheContent" with the only possible Object class being "Training - [yourname] - GermanLanguage" and with the new OT "Test | VN Startpage Germanlanguage [yourname]". Set the "Max. number of objects" to 1.
1. Save the template location, then Save & Close the template. Pay attention to this step, just hitting "Save & Close" at the very bottom will *not* save your changes to the location!
1. Open your test page with that template and switch to "List" view.
1. Right-click on the Location "deutscheSpracheContent" and create a new Object of the class "Training - [yourname] - GermanLanguage". Input all data that is necessary to display it like in the original and also add an image in the extra row that you've added before (you can upload one yourself or use one from the content library). Save & close
1. If you don't see your new Object in the Standard view, you are probably still missing the `<contens:location>` tag for the location "deutscheSpracheContent" in your template file. If so, add it now.
1. If you haven't already, edit your new Output type file and add the output for all rows of your new Object class so that it looks like the original. Also output the image - place it inside the gray box at the bottom. You can copy the HTML from the original or, as an exercise, come up with it yourself. Maybe even improve it!
1. Test different data in your object. Try setting the radio buttons on each teaser to a different color. Does it still look well even when you input very long strings?