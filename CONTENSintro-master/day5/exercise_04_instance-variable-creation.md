## Exercise 04 - Instance Variable Creation

The GermanLanguage's child object setting for color is a standard input field right now. This doesn't really achieve the separation of data from its presentation. What color the teasers are should be the responsibility of the output type and be bound to the one object instance only. 

For this purpose Contens provides Instance Variables. As the name suggests, those are specific for an object's *instance*, meaning that multiple occurrences of one object (even on the same page) can have different variable values assigned to them. The values of the instance variables can be read in the Output types.

In this exercise, you will extract the GermanLanguage's Object class setting for color to be an instance variable.

1. Open up the definition of your GermanLanguage child object OC, the one that allows setting the object's color
1. Notice that there isn't an option anymore to delete the row for the color (or any other row). This is because there are already objects of that class that have data input in these rows and Contens unfortunately can't handle deleting them. You have the option to delete all objects of that class and then try again, of course, but in the real world, this may not always be an option. Instead, create a new Group, call it "Unused", set "Initially opened?" to "No" and move the row into that group.  
   Save & close, create & deploy
1. Find out the ID of your GermanLanguage OC and create a new file ``goethe/_customcodes/uniform/class[theId].cfm``
1. By copying from other custom code files (e.g. ``class124.cfm``) for other object classes, add the possibility to set the object instance's color. Use either radio buttons or a select. Consider setting default values.
1. To import the snippet in the `class[theId].cfm` file, find the Object class in the list of OCs in Contens and right-click on it, choosing "Import instance variable xml"
1. Back in the "Pages" tab, to set the instance variables, right-click on an object on the page and select "Instance properties".
1. In code, you can read out the instance variable with ``co.getInstanceVariable('someKey')``. Use this to control the object's color instead of through the content of the object.

**Additional assignment:** Notice that you cannot set instance variables on subobjects. Before, having an input field for the color allowed you to easily set different colors for each subobject independently. However, this *can* be implemented with instance variables of the GermanLanguage object class, too. Explore your options what types of instance variables you can define to make this work - there is more than one way.