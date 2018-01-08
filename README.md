# Geospatial-Data-Visualization
This is a workshop in progress.  

This is a geospatial data visualization workshop with hands-on examples using QGIS 2.14-2.18.

# What is Data Visualization?  How does it apply to geospatial data?
Data visualization is the current term used to describe the process of representing data in a way that communicates clearly.

"Data visualization" has been present in the field of geography for centuries as cartography.

# Types of Data

Understanding what type of data you are working with is important.  Where the data shows up on the map is handled by the geometries stored in your spatial data file (shapefile, geojson, geotiff, etc.) but how you choose to represent the attribute (non-spatial) data is up to you.  To communicate clearly, how you represent your data should be driven, in part, by the type of data it is.

| Type | Explanation | Examples |
|------|------|------|
| Nominal | Categories | Land Use Type: agriculture, industrial, residential, etc. |
| Ordinal | Hierarchical Categories | Business Size: small, medium, large |
| Interval | Numbers in which zero has no real meaning, relative distances between numbers has meaning, but ratios between numbers does not make sense | Farenheit temperature measurements |
| Ratio | Numbers witha set zero, ratios between numbers make sense | Weight of harvested biomass |

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
| |**Pattern:** different dashs | **Pattern Density:** dashes at varying distances | **Pattern Density:** dashes at varying distances |
| | |**Line Weight:** vary line width with attribute value |**Line Weight:** vary line width with attribute value |
| | | | |
| Polygon  | **Example:** zoning catgeories | **Example:** habitat conditions - good, moderate, or poor | **Example:** area |
| |**Color:** "random" hues with no implied hierarchy | **Color:** gradients | **Color:** gradients |
| |**Fill Pattern:** diffrent fills for each type | **Fill Pattern Density:** fill density varies with attribute value | **Fill Pattern Density:** fill density varies with attribute value |
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

# Hands On Tutorial

## Data
You'll need to download
1. California Counties (optional, background data)
1. Watershed Boundaries (Polygons)
1. Watersed Centroids (Points)
1. Rivers (Lines)

## Load Data into QGIS

Open QGIS (2.14 or 2.18... yes, this is going to need to be re-written for QGIS 3.0 soon... volunteers to help, please contact Michele).  Start a new project by either clicking on the white rectangle icon (with the corner turned down) or selecting "new" under the Project menu.  You now should have a blank map canvas ready to go.

Load the workshop data into QGIS: Layer menu -> Add Layer -> Add Vector Layer.  Alternatively, you can use the Add Vector Layer button on the toolbar.  Add the following data:
* Flowlines.geojson - rivers and other linear water structures
* WBDHU8_SF.geojson - watershed boundary polygons for the San Fransisco Bay watershed
* WBDHU8_Points_SF.geojson - centroids for the watershed boundaries

Optional: Add any other reference data you might find helpful, such as the [California County Boundaries](http://frap.fire.ca.gov/data/frapgisdata-sw-counties_download) or [Natural Earth Data](http://www.naturalearthdata.com/) datasets.

Now order the data in your Layers Panel on the left side of the window so you can see everything - polygons on the bottom, then lines, then points on the top.  Remember that you can grab and drag the items in the Layers Panel to move them up or down in the order.  The order you see is the order the layers appear on the map canvas. (I personally like to think of it like a craft project... big opaque construction paper on the lower levels, stacked up with strings, and stickers.  You could certainly put the paper on top, but you won't see the strings/lines or the stickers/points then.)  Don't worry if the default colors QGIS picked are unattractive.  The rest of this workshop is about styling data.


## Lines
Lines represents items that are linear in nature & are thin relative to the map scale. Length is important but not width.  Examples include roads or rivers.  At large scales, these usually become polygons because the width becomes important as you zoom in.

In this section, we'll be working with the Flowlines.geojson file.  It's a subset of the NHD Flowlines shapefile and only has lines that intersect with the San Francsisco Bay HUC 4 watershed boundary (otherwise it would be too much to deal with today).  For this workshop, I've added a few other columns as well to facilitate learning.   

### Nominal
Let's look at the attribute table for the Flowlines data.  Right click on the layer in the Layers Panel and choose Open Attribute Table.  The name column (GNIS_NAME) is one of the easier to understand nominal data columns in this dataset so we'll work with this column for now.  Scroll through the data to get an idea of what kinds of information is in this name column.  It looks like some items are named and others are not.  Some names appear multiple times.

Close the attribute table.

Let's explore this data some more through visualizations.  Right click on the layer in the Layers Panel again, but this time, pick Properties to open the Layer Properties.  There's a lot of options in this particular interface (notice all the "tabs" on the left side of the window?).  Click on the Style tab.

The default style for lines is a single Simple Line of a randomly chosen color (mine is currently hot pink and a little hard on the eyes).  In the Color drop down (mine is hot pink, yours will be whatever color QGIS picked for you) pick "choose color" to open the Select Color dialog or just click in the colored box to open the same dialog.  Tabs across the top of the Select Color dialog give you an array of options for picking a color.  Pick one that you like in any way you like (one fun option is to look up an HTML color that you like from a website and put that into the HTML Notation box... #457a94 is a nice blue...).  Click the Apply button to see the affect on your canvas.

You can make more complicated markers by stacking up different lines.  Let's make our current line rather wide - change the width to 2.0.  Add a symbol layer with the + button below the white box that shows you the line.  It should have added a layer to your line above your wide blue line.  Making sure the new line is selected in the list of line layers, pick a new color.  I chose white.  Change the Pen Style to a Dot Line.  Ok, this style doesn't really work at the scale of the whole watershed, but if you click OK and zoom in to one section, you'll be better able to see the affects.




### Ordinal

Most important to understanding what's here is knowing what the FCODE column means.  The metadata for this column is available on the [USGS' FCODE metadata page](https://nhd.usgs.gov/userGuide/Robohelpfiles/NHD_User_Guide/Feature_Catalog/Hydrography_Dataset/Complete_FCode_List.htm).  You'll notice that this particular column is Nominal Data, or categories.

Wetlands Professional Services offers a clear explanation of [intermittent vs. ephemeral streams](http://www.wetlandsprofessional.com/intermittent-and-ephemeral-streams.html), as does the [USGS Water Basics Glossary](https://water.usgs.gov/water-basics_glossary.html).

### Interval/Ratio

Length_Meters column


## Polygons
Polygons represents items that are large relative to the map scale.  Examples include parks, lakes, or parcels.

For polygon data, we'll mainly be working with the HUC 8 watershed boundary data (WBDHU8.geojson).

### Nominal

NAME column

### Ordinal

Rank_Acres

### Interval/Ratio

AREAACRES or AREASQKM

## Points
Points represent data that are small relative to the map scale.  Examples include cities on a map of North America or 
store locations at County Level  They have a location but no other dimension.  Because points are one-dimension (a single location), they are somewhat different than lines and polygons in the way they can be represented.  They are the only geometry that can be intuitively represented with an icon.  Icons or markers in general can vary in size without compromizing the geometry itself (you can't increase the size of a polygon or length of a line without implications to the data). 

To learn about points, we will use the centroids of the watershed polygons (WBDHU8_Points_SF).  The data is the same, but rather than working with polygons, we will be working with points that are the center of the original polygons.  Points allow us a completely different set of ways to visualiz the data.

### Nominal

NAME column

### Ordinal

Rank_Acres

### Interval/Ratio

AREAACRES or AREASQKM


## Raster
Rasters are much better at representing continuous data (ratio/interval data), such as temperature, but can be used to represent discrete data (nominal and ordinal) as well.  It is common, for example, to find land cover data with nominal cover categories in raster format.

### Nominal

### Ordinal

### Interval/Ratio




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

1. [Python Graph Gallery](https://python-graph-gallery.com/)
1. [D3 Gallery](https://github.com/d3/d3/wiki/Gallery)

## Colors

1. [Color Brewer](http://colorhunt.co/)
1. [Adobe Color](https://color.adobe.com/create/color-wheel/)
1. [Color Hunt](http://colorhunt.co/)

## Possible Papers

1. http://www.sciencedirect.com/science/article/pii/S0924271615002439
