## Exercise 01 - Page Conversion
### Part 1 - HTML to CFML

In this exercise you will convert a static Goethe.de HTML page to CFML in the style that is used in most of our templates.

1. Unzip [contensintro.zip](../resources/contensintro.zip) into "contensintro" - a new directory in your CF wwwroot. You now have a static version of the Goethe Institut Vietnam country landing page (LP Vietnam) before you. You can view the page at http://localhost:8500/contensintro/gi-vn-start.html
1. Change the file extension of `gi-vn-start.html` to `.cfm`
1. Replace all occurrences of `#` with `##`
1. Wrap the contents of the whole file with `<cfoutput></cfoutput>`
1. Extract the `<head>` into a separate file `_head.cfm` and include it
1. Extract the page header and main navigation into a separate file `_header.cfm` and include it
1. Extract the footer into a separate file `_footer.cfm` and include it
1. Extract all scripts at the end of the body into a separate file `_footerJS.cfm` and include it
1. Extract all locations and include them by doing the following:
	1. Extract `div.lpGal1` into a file `location_maincontent.cfm`
	1. Extract `div.deutscheSprache` into a file `location_deutscheSpracheContent.cfm`
	1. Extract `h2.hdl-start` (the one that says "Culture") as well as `div.gi-teaser-grid-c` (the one that immediately follows the h2 "Culture") into a file `location_szeneDeutschlandContent.cfm`
	1. Extract `h2.hdl-start` (the one that says "Projects") as well as `div.gi-teaser-grid-c` (the one that immediately follows the h2 "Project") into a file `location_deutschlandUndWeltContent.cfm`
	1. Extract the `<aside>` into a separate file `location_rightcontent.cfm`
1. In `_header.cfm` extract `div#hauptNavigation` to a separate file `_mainNavigation.cfm`
1. In  `_header.cfm` extract `div.nav-item-language` to a separate file `_languageNavigation.cfm`
1. Extract all objects inside the `location_*` files into separate files called `ot_*.cfm` (e.g. `ot_teaser.cfm`) and include them
	- To do that, identify object Output types, i.e. portions of similar HTML, differing mainly in text content. Example: one Output type is a teaser with an image, a teaser headline, a kicker and a text.
	- There are 24 objects in total on this page but only 9 Output types - so you should end up with 9 new CFM files, one for each Output type
	- For now, every file should contain the HTML code of all objects

Now you should have a pretty neat collection of files that can be used to create Contens templates and Output types. This will be the next exercise.