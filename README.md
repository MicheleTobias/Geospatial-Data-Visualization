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

# Hands On Tutorial

## Data
You'll need to download
1. California Counties (optional, background data)
1. Watershed Boundaries (Polygons)
1. Watersed Centroids (Points)
1. Rivers (Lines)




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
