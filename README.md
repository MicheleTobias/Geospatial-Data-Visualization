# Geospatial-Data-Visualization  

This is a geospatial data visualization workshop with hands-on examples using [QGIS 2.14-2.18](https://qgis.org/en/site/forusers/download.html).

The goals of this workshop are that you will learn basic techniques for visualization techniques appropriate for each type of data and each type of geometry.

# What is Data Visualization?  How does it apply to geospatial data?
Data visualization is the current term used to describe the process of representing data in a way that communicates clearly.

The concept of "Data visualization" has been present in the field of geography for centuries as cartography.

The purpose of data visualization is to tell a story.  As the cartographer or data scientist, your goal is to identify the story the data tells and to make visualization choices to guide the reader/viewer to understand the story and engage with the data.

# Types of Data

Understanding what type of data (also known as "measurement scales") you are working with is important.  *Where* the data shows up on the map is driven by the geometries stored in your spatial data file (shapefile, geojson, geotiff, etc.) but how you choose to represent the *attribute* (non-spatial) data is up to you.  To communicate clearly, how you represent your data should be driven, in part, by the type of data it is.

| Type | Explanation | Examples |
|------|------|------|
| Nominal | Categories | land use type: agriculture, industrial, residential, etc. |
| Ordinal | Hierarchical Categories | business size: small, medium, large |
| Interval | Numbers in which zero has no real meaning, relative distances between numbers has meaning, but ratios between numbers does not make sense | Fahrenheit temperature measurements |
| Ratio | Numbers with a set zero, ratios between numbers make sense | weight of harvested biomass |

Reference: [University of Colorado's Cartographic Communication Site](https://www.colorado.edu/geography/gcraft/notes/cartocom/cartocom_f.html)

# Visualization Considerations for Each Type of Data

| Type | Nominal | Ordinal | Interval & Ratio |
|------|------|------|------|
| | Unranked categories | Ranked or ordered categories | Continuous numbers |
| | | | |
| Point | **Example:** type of business | **Example:** business size - small, medium, or large | **Example:** gross yearly income |
| |**Color:** "random" hues with no implied hierarchy | **Color:** gradients | **Color:** gradients |
| |**Shape:** varied icons | **Size:** gradient in marker size | **Size:** gradient in marker size |
| | | | |
| Line  | **Example:** type of road - paved, unpaved, etc. | **Example:** stream order | **Example:** length |
| |**Color:** "random" hues with no implied hierarchy | **Color:** gradients | **Color:** gradients |
| |**Pattern:** different dashes | **Pattern Density:** dashes at varying distances | **Pattern Density:** dashes at varying distances |
| | |**Line Weight:** vary line width with attribute value |**Line Weight:** vary line width with attribute value |
| | | | |
| Polygon  | **Example:** zoning categories | **Example:** habitat conditions - good, moderate, or poor | **Example:** area |
| |**Color:** "random" hues with no implied hierarchy | **Color:** gradients | **Color:** gradients |
| |**Fill Pattern:** different fills for each type | **Fill Pattern Density:** fill density varies with attribute value | **Fill Pattern Density:** fill density varies with attribute value |
| | | | |
| Raster  | **Example:** land use/cover | **Example:** fire risk - high, medium, low | **Example:** rainfall totals |
| |**Color:** "random" hues with no implied hierarchy | **Color:** gradients | **Color:** gradients |
| | | | |

# Scale

The choice to display data as a point, line, or polygon depends on the scale of your map.

Try this: Draw UC Davis at the scale of...
* Your program's building
* The city
* Yolo County
* North America

How does your representation change with scale?  The campus probably shifted from many polygons, to a single polygon, to a point.

------------------------------------------------------------------------------------------------------

# Hands On Tutorial

Now we'll work on the hands-on section of this tutorial.  The key take-home message is to think about what type of data you are working with and how best to represent it.

## Data
You'll need to download the following data from the data folder in this repository or from **[this Box folder](https://ucdavis.box.com/s/sh73tlyvewlq4htfbu56l591wh2q5mi4)**:

1. Watershed Boundaries (Polygons)
1. Watershed Centroids (Points)
1. Rivers (Lines)
1. Digital Elevation Model of San Francisco (Raster)

## Load Data into QGIS

Open QGIS (2.14 or 2.18... these instructions should mostly work for QGIS 3.0 soon with the exception of loading data).  Start a new project by either clicking on the white rectangle icon (with the corner turned down) or selecting "new" under the Project menu.  You now should have a blank map canvas ready to go.

Load the workshop data into QGIS: Layer menu -> Add Layer -> Add Vector Layer.  Alternatively, you can use the Add Vector Layer button on the toolbar.  Add the following data:
* Flowlines.geojson - rivers and other linear water structures
* WBDHU8_SF.geojson - watershed boundary polygons for the San Francisco Bay watershed
* WBDHU8_Points_SF.geojson - centroids for the watershed boundaries

Optional: Add any other reference data you might find helpful, such as the [California County Boundaries](http://frap.fire.ca.gov/data/frapgisdata-sw-counties_download) or [Natural Earth Data](http://www.naturalearthdata.com/) datasets.

Now order the data in your *Layers Panel* on the left side of the window so you can see everything - polygons on the bottom, then lines, then points on the top.  Remember that you can grab and drag the items in the *Layers Panel* to move them up or down in the order.  The order you see is the order the layers appear on the map canvas. (I personally like to think of it like a craft project... big opaque construction paper on the lower levels, stacked up with strings (lines), and stickers (points).  You could certainly put the paper on top, but you won't see the strings/lines or the stickers/points then.)  Don't worry if the default colors QGIS picked are unattractive.  The rest of this workshop is about styling data.


## Lines
Lines represents items that are linear in nature & are thin relative to the map scale. Length is important but not width.  Examples include roads or rivers.  At large scales, these usually become polygons because the width becomes important as you zoom in.

In this section, we'll be working with the *Flowlines.geojson* file which contains things like streams, pipelines, canals, and other linear entities that carry water from one place to another.  It's a subset of the [NHD Flowlines](https://nhd.usgs.gov/wbd.html) shapefile and only has lines that intersect with the San Francisco Bay [HUC 4 watershed boundary](https://water.usgs.gov/GIS/huc.html) (otherwise it would be too much to deal with today).  For this workshop, I've added a few other columns as well to facilitate learning.   

### Nominal
Let's look at the attribute table for the Flowlines data.  Right click on the layer in the *Layers Panel* and choose *Open Attribute Table*.  The name column (*GNIS_NAME*) is one of the easier to understand nominal data columns in this dataset so we'll work with this column for now.  Scroll through the data to get an idea of what kinds of information is in this name column.  It looks like some items are named and others are not.  Some names appear multiple times.

Close the attribute table.

Let's explore this data some more through visualizations.  

#### Single Sybol

We'll start with a single symbol for all the lines to get familiar with this interface.

 * Right click on the layer in the *Layers Panel* again, but this time, pick *Properties* to open the *Layer Properties*.  There's a lot of options in this particular interface (notice all the "tabs" on the left side of the window?).  
 * Click on the *Style* tab.
 * The default style for lines is a single *Simple Line* of a randomly chosen color (mine is currently hot pink and a little hard on the eyes).  In the *Color* drop down, pick *choose color* to open the *Select Color dialog* or just click in the colored box to open the same dialog.  
 * Tabs across the top of the *Select Color dialog* give you an array of options for picking a color.  Pick one that you like in any way you like (one fun option is to look up an HTML color that you like from a website and put that into the *HTML Notation* box... #457a94 is a nice blue...).  
 * Click the *Apply* button to see the effect on your map canvas.

You can make more complicated markers by stacking up different lines.  
 * Let's make our current line rather wide - change the width to 2.0.  
 * Add a symbol layer with the *+* button below the white box that shows you the line.  It should have added a layer to your line above your wide blue line.  
 * Making sure the new line is selected in the list of line layers, pick a new color.  I chose white.  
 * Change the *Pen Style* to a *Dot Line*.  
 * Click *Apply* to see the effect in your map canvas.  
 * Ok, this style doesn't really work at the scale of the whole watershed, but if you click *OK* to close the dialog, then zoom in to one section, you'll be better able to see the affects.

![alt text](https://github.com/MicheleTobias/Geospatial-Data-Visualization/blob/master/images/Line_SingleSymbol.PNG "Single symbol stacked line marker for rivers")

So we've seen that having just one marker type sometimes doesn't help us understand our data very well, but now we know one way we can customize how our data looks.

#### By Name

Let's try visualizing our waterways by their names.
 * Open the Layer Properties for your Flowlines layer again and back to the Style tab.
 * From the drop-down menu at the top of the menu that currently says *single symbol*, pick *Categorized*.  This will erase the style we made previously, but that's ok.
 * From the *Column* drop-down, pick *GNIS_NAME*, which has the name of the water body.  
 * For the *Color Ramp*, pick *Random Colors*.  We don't want to imply that there is any hierarchical relationship between the streams so *Random Colors* is a good choice.  
 * Click *Classify* under the big white box to show all the categories.  
 * Click *Apply* to see what this looks like.  You may want to zoom in to one area.

In mine I'm noticing there are a lot of lines of one color (mine are pink).  You probably have a lot of lines in a different color.  If I scroll down to the bottom of my list of symbols, I can see that the unnamed (blank) streams were assigned to a shade of pink.  I'd like to change this.  
 * Double click on the line symbol to open the Symbol Selector dialog.  
 * Because these are unnamed, I want them to be gray. This dialog works in a very similar way to the dialog we worked with earlier. Pick your new color, then click *OK* in this dialog.  
 * In the *Layer Properties* dialog, click *Apply* to see how that changed.  Ok! Now I can easily see what's named and what's not and also which line segments belong to the same river or stream.

Now let's make those named streams stand out a little more with some visual hierarchy.  In the *Layer Properties*, you've seen that you can change any of the symbols one at a time.  But we have a lot of named streams and I would guess you don't want to change each of these one at a time.  We can change many or all of them at once.  
 * In the list of streams, single click on the first entry to highlight it.  
 * Scroll down to the bottom of the list.  Hold down the Shift key and click on the last **named** creek (not the blank entry) to highlight all of the named creeks. 
 * Right click on any of the highlighted names, and select *Change Width*.  
 * Change the number to 0.50 and click *OK*.  
 * Click *Apply* in the *Layer Properties* dialog to see how that changed.

![alt text](https://github.com/MicheleTobias/Geospatial-Data-Visualization/blob/master/images/Line_NominalData.PNG "Nominal line data")

The exercise we just worked through is an excellent example of how scale changes how you represent data.  Zooming in on this data to roughly city-level scale, this example makes sense, but if I try to use this same visualization at the scale of the entire dataset or even the state, it quickly loses its ability to communicate well (in fact, it starts to look like party streamers, which isn't what we want).

![alt text](https://github.com/MicheleTobias/Geospatial-Data-Visualization/blob/master/images/Line_NominalData_ZoomOut.PNG "Nominal line data zoomed out to show that at a smaller map scale, the lines no longer look like rivers")



### Ordinal

The *FCODE* column classifies each line segment as different kinds of linear water bodies like rivers or canals.  The metadata for this column is available on the [USGS' FCODE metadata page](https://nhd.usgs.gov/userGuide/Robohelpfiles/NHD_User_Guide/Feature_Catalog/Hydrography_Dataset/Complete_FCode_List.htm).  

Let's focus on the Rivers/Streams line type (FTYPE 460 or FCODE 46006, 46003, and 46007).  This subset of the data is Ordinal Data, or categories that imply a hierarchy.  Perennial streams (FCODE 46006) have water all year, while Intermittend streams (FCODE 46003) have water some of the year, and Ephemeral streams (FCODE 46007) have water only occasionally.  For a deeper understanding of the difference between these kinds of streams, Wetlands Professional Services offers a clear explanation of [intermittent vs. ephemeral streams](http://www.wetlandsprofessional.com/intermittent-and-ephemeral-streams.html), as does the [USGS Water Basics Glossary](https://water.usgs.gov/water-basics_glossary.html).

But we know that our dataset has more than just the 3 kinds of streams we are interested in.  How do we isolate just the data we want to see?  As you saw in the Line Nominal Data exercise above, we could categorize the data based on FCODE, and then work with the way each category is styled to highlight just the rivers/streams.  But let's look at a different way: *Rule-Based Classification*. (For those of you who saw the *Graduated Classification* option on the menu and thought "What about that one? It's for visualizing hierarchy in data, right?" I'd respond that you're definitely thinking like a cartographer, but the Graduated option is for numerical data where the numbers can be compared as numbers.  Our numbers really aren't numbers but categories and while we know perennial > intermittent > ephemeral, their FCODEs don't work that way. 46006 > 46003 but 46003 !> 46007.  We'll use Graduated later, don't worry!)

Open up the *Layer Properties* for your Flowlines data again and go to the *Style tab*.  From the drop-down menu at the top, choose *Rule-based*.

Let's make start by showing just the perennial streams.  
* Add a rule by clicking the green *+* button just below the big white box to open the Edit Rule dialog.
* In the *Label* box, type *Perennial Streams* so we can remember what this rule does.
* In the *Filter* section, click the *...* button to open the Expression String Builder dialog.  This dialog works in the same way as the Expression Builder for the Attribute table, for those of you familiar with that tool.  We'll write an expression that, if true for a given line segment, will show the line.  If it's not true for a particular segment, it won't display it.
* Expand the *Fields and Values* list (in the middle section of the dialog) by clicking the arrow next to it.
* Double click the *FCODE* field to add it to the expression box on the left.
* Click the *=* button above the expression box (or just type it in... QGIS isn't as picky as ArcGIS about this)
* Then type *46006*
* Your expression should say *"FCODE"  =  46006*  and if it's a valid expression, the statement below the expression box will say "Output preview: 0".  If there's an error, it will tell you so.
* Click *OK* to close the *Expression String Builder Dialog* and go back to the *Edit Rule dialog*.  Notice that now there's text in the *Filter* box.
* Now adjust the color of your line to represent a stream better than whatever the default color was (why is mine pink again?).  I'm going to pick a dark blue and make the pen width a little thicker... let's say 0.5 to start.
* Click *OK* to close the *Edit Rule Dialog*, then *Apply* in the *Layer Properties* to see how our new rule looks on the map canvas.

![alt text](https://github.com/MicheleTobias/Geospatial-Data-Visualization/blob/master/images/Line_Ordinal_PerennialStreams.PNG "Choppy lines representing river segments")

Looks good, but let's add the other two categories.  It's the same process as before but change the *FCODE* number to match the stream type, and pick a different color to represent each stream type.  Need help picking colors?  Pop over to the [Color Brewer website](http://colorbrewer2.org) for assistance.  Those HEX codes can be used in the QGIS color picking dialog *HTML Notation box* to get the exact color.  I'm using a dark blue for perennial, a medium blue for intermittent, and a light blue for ephemeral.

Your rules should look roughly like this:

![alt text](https://github.com/MicheleTobias/Geospatial-Data-Visualization/blob/master/images/Line_Ordinal_Solid_LayerProperties.PNG "Layer Properties dialog window showing the three rules to make the three categories")

The results of the rules shown above:

![alt text](https://github.com/MicheleTobias/Geospatial-Data-Visualization/blob/master/images/Line_Ordinal_SolidStreams.PNG "Three different kinds of streams shown with three shades of blue")

The example we just worked though used solid colors to style each category of lines.  But you have other options!  Try picking one shade of blue for all the lines, but vary the pattern - solid, dashed, and dotted.  Or keep all three lines solid but vary the width.  Which one tells the story better for you?  What would you change about your choices if you change your map scale (zoom in or out)?

Varying the line width could looks like this:

![alt text](https://github.com/MicheleTobias/Geospatial-Data-Visualization/blob/master/images/Line_Ordinal_VaryLineWeight.PNG "Lines of the same shade of blue but different line weights")


### Interval/Ratio

Now, if we want to explore stream segments by their length, we'll be working with Ratio Data.  Interval Data is dealt with in very similar ways, so we'll skip that for now.  The length of the stream segments in meters is stored in the *Length_Meters* column.

Let's explore the stream segments by length:

* Open the *Layer Properties* for the *Flowlines* data once again and go back to the *Style tab*. (You've done this 3 times now, which pretty much makes you a pro!)

* Change the drop-down at the top to *Graduated*.  (See! I told you we'd get there!)  Looking at this new dialog, you might start to see some similarities to the other interfaces we've worked with so far.

* From the *Column* drop-down, pick *Length_Meters*.

* Set the *Color Ramp* to *Blues*.  Note that you can change the classification method with the *Mode* drop-down under the white box on the left and the number of classes on the right.  Let's leave the *Mode* as *Equal Interval* and the number of *Classes* as 5.  For each dataset you work with, you will want to think about which options make the most sense for your data.

![alt text](https://github.com/MicheleTobias/Geospatial-Data-Visualization/blob/master/images/Line_Ratio_Interface_EqualInterval.PNG "Interface showing Equal Interval classes")

* Click *Classify*.  Notice that the color ramp has low values in a light color and higher values in a darker color.  This makes sense for our data, but if you needed the ramp reversed, click the *Invert* check box near the Color Ramp selector.  

![alt text](https://github.com/MicheleTobias/Geospatial-Data-Visualization/blob/master/images/Line_Ratio_EqualInterval.PNG "Map showing the results of the equal interval classification - mostly one color")

* Click *Apply* to see how this looks.  Yikes! Almost everything appears to have fallen in the first category.  Let's investigate.  Click the *Histogram* tab just above the box showing the classes.  If it appears to be blank, click the *Load Values* button.  It seems that indeed most of our lengths are generally small with a few larger ones.  Maybe a new classification scheme is in order.

![alt text](https://github.com/MicheleTobias/Geospatial-Data-Visualization/blob/master/images/Line_Ratio_Interface_Histogram.PNG "Histogram interface")

* Click back to the *Classes* tab instead of the *Histogram*.  Pick *Natural Breaks (Jenks)* from the *Mode* drop-down.  The classes should automatically update.  Click the *Apply* button to see what it did.  This looks better to help us see the variation in the data.

![alt text](https://github.com/MicheleTobias/Geospatial-Data-Visualization/blob/master/images/Line_Ratio_NaturalBreaks.PNG "Map showing the results of the Natural Breaks classification - more color variation")

**Bonus:** The example we've been working through shows all the line segments, regardless of whether they are streams or irrigation ditches.  What if we just want to show the streams?

Back in the *Layer Properties' Style Tab*, notice that to the right of the *Column* drop-down is a button with a Greek epsilon (a scrip E).  Click the *Epsilon* button to open the *Expression Dialog*.  (This looks pretty familiar by now, right?)

In this case, we will need to use an *If Statement* to tell QGIS that if a line has a certain FTYPE (460), we want it displayed, but if it has a different FTYPE, we don't want to see it.

Expand the *Conditionals list* (in the middle of the window).  Click once on "if" to display an explanation of what this function does and how to use it.  *If* takes three arguments: the condition we want to consider (the FTYPE should be 460), what to display if that condition is true (the Length_Meters value from the attribute table), and finally what to do if the condition isn't true (display nothing... make it null).

Now that we know what *if* does, let's build our query.  Delete any text in the Expression box.  Double click *if* in the Conditionals list to send it over to the Expressions box.  Expand the *Fields and Values* list, and double click *FTYPE* to send it over.  Type in = 460, then a comma and a space.  That takes care of the conditional statement.  Now let's double click *Length_Meters* to send it over to the Expressions box.  This is what we want to display if the condition is true.  Add a comma and a space to the query text.  Finally, we need to tell it what to do if the condition is false.  Type *NULL* and close the parentheses.  Your query should look like this:  *if(  "FTYPE" = 460, Length_Meters, NULL)*  Check the below the Expressions Box to make sure there's no errors.  Then click *OK* in this dialog, and *Apply* in the Layer Properties.

![alt text](https://github.com/MicheleTobias/Geospatial-Data-Visualization/blob/master/images/Line_Ratio_NaturalBreaks_JustStreams.PNG "Map showing the results of the Natural Breaks classification but with just the streams")


## Polygons
Polygons represents items that are large relative to the map scale.  Examples include parks, lakes, or parcels.

For polygon data, we'll mainly be working with the HUC 8 watershed boundary data (*WBDHU8.geojson*). You can turn off or remove the flowlines dataset.  We won't need it for the rest of the tutorial.

Open the attribute table for your watershed polygon layer.  You may not be certain what every column means, but you can get an idea of the kind of data available and what might be useful... name, area and length columns, ranking of watershed size...  and what type of data each column holds.  When you're done, close the attribute table.

### Nominal

Let's start exploring how Nominal Data works with polygons.  
* Open up the *Layer Properties* and go to the *Style* tab.  
* Categorize your watershed boundaries by the *NAME* column.  Feel free to uncheck or remove the blank category (usually reserved for "other").  What's the best color scheme to use?  Because this data is Nominal, we don't want to imply a relationship, but rather that each polygon is simply different from the others.  [Color Brewer](http://colorbrewer2.org) is a great place to get ideas.  
* Choose 6 classes, then pick the *Qualitativ*e radio button.  Because of the nature of the data, we are likely to display these polygons as relatively large compared to the other map items, I would encourage you to use pale (unsaturated) rather than saturated colors.

We probably don't want to add these to the legend (referring back and forth would be annoying to the reader), but we want the reader to know what they are looking at, so let's quickly label our watersheds with text.
* Click on the *Labels* tab in the Layer Properties.
* On the top drop-down menu, pick "Show labels for this layer".
* For "Label with", pick the *NAME* column.
* Explore the other options available, picking what you like (text on maps could be another complete workshop for later so we won't get into details in this workshop).
* Click the Apply button as you go to see what your changes look like.  Continue to try options and when you're happy, you can click *OK*.

Here's one possible version of this map:

![alt text](https://github.com/MicheleTobias/Geospatial-Data-Visualization/blob/master/images/Polygon_Nominal_Filled.PNG "Map showing watersheds boundaries filled with a different color for each polygon")

Continue to explore options in the *Style* interface.  Change up the Symbol to see what kinds of effects you can generate.  One option is to apply the colors just to the outlines of the polygons.  Or add a semi-transparent fill, which could be useful for showing data underneath like a terrain model.

Explore options to create new effects:

![alt text](https://github.com/MicheleTobias/Geospatial-Data-Visualization/blob/master/images/Polygon_Nominal_Filled_Transparency.PNG "Map showing watersheds boundaries filled with a different color for each polygon and line")

### Ordinal

Working with polygon Ordinal Data is very similar to working with line Ordinal Data.  Rather than walking you through very similar steps again, I'll challenge you to try this on your own!  The column *Rank_Acres* ranks the size of the polygons with 1 being the largest and 6 being the smallest in size.  Should the numbers be treated as numbers or are they really categories with a numeric label?  Should you use the Categorized or the Graduated option for this kind of data?

### Interval/Ratio
Like Ordinal Data, working with Interval/Ratio Data in polygons is very similar to working with lines, as we did earlier.  The *AREAACRES* or *AREASQKM* both have Interval/Ratio Data (the first being area in acres and the second being area in square kilometers).  Using the *Graduated* option in the Style Tab, explore and find a visualization for this data that suits the data well.  What story to you want to present to the viewer?  How can you best help the viewer see that story?

## Points
Points represent data that are small relative to the map scale.  Examples include cities on a map of North America or store locations at County Level.  They have a location but no other dimension.  Because points are one-dimension (a single location), they are somewhat different than lines and polygons in the way they can be represented.  They are the only geometry that can be intuitively represented with a single icon.  Icons or markers in general can vary in size without compromising the geometry itself (you can't increase the size of a polygon or length of a line without implications to the data). 

To learn about points, we will use the centroids of the watershed polygons (*WBDHU8_Points_SF*).  The attribute data is the same as the polygons we just worked with, but rather than working with polygons, we will be working with points that are the center of the original polygons.  Points allow us a completely different set of ways to visualize the data.

### Nominal

The *NAME* column contains nominal data in the watershed centroids layer.

In previous sections, you've seen how to change the look of a single symbol.  Point markers function in much the same way as lines and polygons in terms of *making* markers.  You can make increasingly complex markers by layering simple shapes.  For example, you can make a target symbol by layering increasingly smaller circles on top of one another and alternating between light and dark color fills.

Alternatively, with points, you have the option of working with meaningful icons.  For example, if you were making a hiking map, you may use icons to indicate the location of the trailhead, restrooms, and ranger station.  Icons can communicate quickly without text labels but only work easily with point data.  The US National Parks system has developed an outstanding [intuitive marker set](https://www.nps.gov/maps/tools/symbol-library/).

QGIS comes with a limited number of icons and symbols in the default installation.  Try out some of the symbols in the Single Symbol option of the Style Tab.  None of these are expecially good for the watershed data we are working with, but it's good to know that the option is there for other datasets you work with.  Also note that icons are also available to you in the Categorized option as well.  If you select the basic symbol shape you want before you classify your data, when you click the classify button, color palette you select will be applied to the shape you chose.

Things to explore later if you're interested: You can [make your own custom SVG markers](https://github.com/MicheleTobias/OpenSourceCartography) or use styles developed by other using the [Resources Sharing Plug-In](http://www.akbargumbira.com/qgis_resources_sharing/).


### Ordinal

Ordinal point data follows pretty much the same formula we've worked with using polygon and line data.  Use the *Rank_Acres* column to make a categorized point visualization of the ranked size of the watersheds.  As you may notice, for this particular dataset, a point representation of this data is not very compelling.  Let's move on to Interval/Ratio data.

### Interval/Ratio

You've already seen how to categorize your data based on the data in a column of you attribute table, showing different classes as different colored markers.  With points you have the unique opportunity to change the size of the entire marker.  

Let's categorize our watershed centroids based on the size of the watershed and visualize the data using different sized circles.

* In the *Style Tab* of the *Layer Properties* for your centroids layer, select *Graduated* from the drop-down menu at the top.  For the column to base the classification on, select either *AREAACRES* or *AREASQKM*.  
* For the *Symbol*, let's pick a circle (or pick any shape you like, but circles are fairly easy to compare visually) and choose a fill color that you think represents the data well.  
* Now here's the key to making this different than what we've done before: from the *Method* drop-down, pick *Size* instead of *color*.  This will allow us to vary the size of the symbol rather than the color.
* The rest of the interface should look familiar from before.  Pick the *Mode* for the classification method that works well for this data.  Use the *Histogram* tab to help you understand the distribution of this particular data.
* When you're ready to see how your classification looks, click the *Apply* button.

### Combining Data Types

The type of data needs to drive your choices for visualization, but don't hesitate to combine different kinds of data into one map or graphic to help tell the story you are trying to convey.  Point data can be displayed on top of, and in addition to, similar data presented as polygons.  For example, you can display your centroid data on top of your watershed polygons to show two different aspects of the data we have about the watersheds.

![alt text](https://github.com/MicheleTobias/Geospatial-Data-Visualization/blob/master/images/Combining_Data_Types.PNG "Map showing watersheds boundaries filled with a different color for each polygon, graduated circles representing acrage, and text labels")

Text labels can also help viewers interpret what they see.  Graduated circles help get a general idea of the watershed size compared to other nearby watersheds, but a text label with the size in acres can also be informative and helpful.

## Raster Data
Raster data (grids) can also have data values that are Nominal, Ordinal, or Interval/Ratio.  The skills you've learned today about visualizing vector data also apply to raster data.  

### Interval & Ratio
Let's start our raster investigation with inteval & ratio data.

You can remove the previous data from your map canvas or uncheck the boxes in the Layers Panel.

Load some raster data:
1. Click the Add Raster Layer button (it looks like a checker board).
1. Navigate to where you saved your workshop data and select the DEM_SF.tif file.
1. Click *Open*.

Now you should see a gray scale image that roughly looks like the San Francisco peninsula.  This is a Digital Elevation Model (DEM).  Each cell in the raster contains a number representing the elevation at that location.  

Let's style this data:
1. Open the Layer Properties for your DEM (right click on the layer in the Layers Panel and select *Properties*).
1. Click on the *Style* tab on the left side.
1. For the *Render type* drop down, select *Singleband pseudocolor*.
1. For the *Color*, select *New color ramp*
1. Select *cpt-city* from the drop down and click *Ok*
1. Pick *Topography* from the options on the left.
1. Pick *c3t1* for our gradient.  Or select another scheme you think will work well for representing topography.  Click *Ok*.
1. Next, you'll see a dialog asking what you want to call your new gradient.  I suggest leaving the default name, but you can call it what you like.  Click *Ok* when you're done.
1. You'll see that the color ramp in the Layers Properties has updated.  Click *Apply* to see what it looks like with our data.

This is good, but I think it would be easier to understand if we set the water to a more intuitive color.  Yes, this means our color ramp will have a disconnect in it, but I think it will help us visually interpret the data better.
1. In the Layer Properties, double click on the color box representing the lowest value to open the Change Color dialog.  Select a shade of blue that you lick and click *Ok*.  And then click *Apply* in the Layer Properties to see how it looks.  
1. Continue to adjust the colors until you are happy with the colors, then click *Ok*.

![alt text](https://github.com/MicheleTobias/Geospatial-Data-Visualization/blob/master/images/Raster_Continuous.PNG "Digital Elevation Model with a continuous color ramp")

### Nominal and Ordinal
As we've seen with the vector data, data visualization with nominal and ordinal data (essentially categorized data) is very similar.

Let's make some categories in our DEM layer.  We'll have 4 categories of elevation: low, medium, high, and water.  We could make a new layer with the Raster Calculator, but let's just work with the data visually and not make more data to store.
1. Open the Layer Properties for our DEM layer and go to the *Style* tab.
1. Expand the *Load min/max values* area of the dialog.  Select *Min/max*, change the *Accuracy* drop down to *Actual (slower)*, and then click *Load*.  This will ensure that the program uses all of the values in our raster to make categories and not a subsample of values.
1. Below the large box where the color ramp appears, find the *Mode* dropdown menu and select *Equal interval*.  To the right of that, change the *Classes* box to 4.
1. We can change the numbers in the *Value* and *Label* columns of the color ramp to adjust the break points of the ramp.  For the *Value* column, change the breaks to 1, 50, 150, and leave the final one as *inf*.  Notice how the Label column changes.
1. In the *Label* column, change the labels to "water", "low", "medium", and "high".  
1. We can change the colors as well.  Because these are categories, we can pick 4 different colors, but also because there is a hierarchical relationship between the low, medium, and high categories, we might consider a gradient of sorts.  I chose a blue for the water, green for low, yellow for medium, and brown for high.
1. Finally, change the *Interpolation* type to *Discrete* so the 

![alt text](https://github.com/MicheleTobias/Geospatial-Data-Visualization/blob/master/images/Raster_Ordinal_Properties.PNG "Layer Properties for the DEM color selection")

![alt text](https://github.com/MicheleTobias/Geospatial-Data-Visualization/blob/master/images/Raster_Ordinal.PNG "DEM categorized into water and low, medium, and high elevations.")


### Continue Learning
To learn more about how to load and symbolize raster data, see [Module 8 of the QGIS Training Manual](https://docs.qgis.org/2.18/en/docs/training_manual/rasters/index.html).  Remember that how you choose to symbolize the data should be based on the type of data you are working with.

# Conclusion

You have looked at a number of options for visualizing data in QGIS 2.18 based on the type of data you are working with.  The principles and guidelines you have learned are applicable in any GIS and to any data visualization project (perhaps in R?).  The key is to think about what kind of data you are working with (Nominal, Ordinal, Interval, or Ratio) and think about what is the best way to display that data in a way that communicates effectively with your audience.  Sometimes the answer is to combine data into one image.  Sometimes, you need to simplify.  It's always a good idea to get feedback from friends or colleagues to make sure your product communicates to others what you think it does.

Continue to explore! Below you will find more resources and sites for inspiration for your next project.


# Additional Resources:

## Guides & Toolkits

1. [University of Colorado's Cartographic Communication Site](https://www.colorado.edu/geography/gcraft/notes/cartocom/cartocom_f.html)
1. [Ordinance Survey Geo Data Viz Toolkit](https://github.com/OrdnanceSurvey/GeoDataViz-Toolkit)
1. [Visualizing Data's Resource List](http://www.visualisingdata.com/resources/)

## Tutorials

1. [QGIS 2.18 Training Manual](https://docs.qgis.org/2.18/en/docs/training_manual/)
1. [Lyzi Diamond's Site](http://lyzidiamond.com/)
1. [Alberto Cairo's Tutorials](http://www.thefunctionalart.com/p/instructors-guide.html)

## Galleries

1. [QGIS Map Gallery](https://www.flickr.com/groups/qgis/pool/)
1. [Python Graph Gallery](https://python-graph-gallery.com/)
1. [D3 Gallery](https://github.com/d3/d3/wiki/Gallery)

## Colors

1. [Colorzilla browser plugin](http://www.colorzilla.com/) - select colors directly from webpages, then paste color codes into QGIS color interfaces
1. [Color Brewer](http://colorhunt.co/)
1. [Adobe Color](https://color.adobe.com/create/color-wheel/)
1. [Color Hunt](http://colorhunt.co/)

## Academic Papers

1. http://www.sciencedirect.com/science/article/pii/S0924271615002439
