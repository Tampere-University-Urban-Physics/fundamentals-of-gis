### Fundamentals of Geographic Information Systems (GIS)

# Theory 2: Georeferencing & digitizing

*By Rowan van der Kaaden. Updated by Jonathon Taylor*

## More about good maps

In the last theory section, we touched upon some principles for good maps. For example, they should have a scale bar, a north arrow, and a be suitable for colour blind people. They way we present maps can help them be interpreted, and can help guide the message we want to communicate.

Below is an example of a (purposely-made) bad map of air pollution across Great Britain. Have a look and try to list all the things that you think are bad about it.


![](https://raw.githubusercontent.com/Tampere-University-Urban-Physics/fundamentals-of-gis/master/Assets/2_Theory/bad_map.png)

There's a lot going on there, and just by looking at it you may instinctively feel like it doesn't quite make sense. How many of these things did you notice:
- It does not look good. Not at all. 
- Where did the data come from? There should be some kind of acknowledgement of it somewhere...
- The legend title doesn't make sense. This is in fact a map of particulate air pollution below 2.5um in size, but from the title you would not get that. There aren't even units.
- The numbers in the legend have a huge number of significant figures. There is no measurement device on earth that can get a value this precise. Do viewers really need this kind of precision, anyway?
- In addition to being ugly, the colours don't make sense. We have (kind of) established that this is a map of air pollution concentrations, but the colours are random and don't show any kind of sequence from low to high concentrations.
- Why is the outline red?!
- There is no scale bar or north arrow.

We can do better than this. We can choose colours that are appropriate for the data, whether it is sequential (like our air pollution data), diverging (e.g. deviations of differences from a central value of midpoint), or qualitative data (like categories). You can choose the approriate number of classes to display the data, so there are not too many different categories. Again, Colorbrewer is a really useful reference for helping you find good colours for the type of data you want to show. Visit colorbrewer and play with the different sequential, diverging, or qualitative suggestions they show. Some show variations by changing the hue (particularly diverging or qualitative) while sequential also has options just to change the brightness of the colour.

Below is a (hopefully) improved version of our ugly ugly map. Notice how the issues we have identified have been corrected? Let's aim for good maps.

## Spatial data sources

Spatial data is available through various sources, some of which are restricted, but a good amount is freely available. Common providers of spatial data are governmental institutions, local governments(cities and municipalities), education institutes, crowd sourced data, among others. Even some commercial companies provide open data. But there is also data that is restricted, for example because of commercial motivation, but also privacy or other reasons. It is however good to practice **open science** with GIS as well to make the data and research as accessible as possible.   

Data can also come in a variety of formats. Vector data can be saved in formats such as a shapefile (.shp) which is a proprietary format from ArcGIS. If you want to work with Google Earth, then you would use the Google vector formats of .kml or .kmz. And there are many other forms, too. Raster data can be saved as a geoTiff (.tiff) file, .jpeg2000, netCDF or others. Sometimes, geospatial data is saved in a geopackage, which can have different vectors and rasters in the same file. QGIS is pretty good at knowing how to open these many different file formats.

Saving data on your own laptop can be convenient, but it also takes up memory and doesn't get updated unless you do this yourself (spatial data changes often). Sometimes saving the data on your own laptop might be not permitted. So, often you may connect to a external database (for example online) which is used to store large amounts of GIS data and which should be updated by the provider.

Examples of data sources are:
| Provider | Data | Note |
|--|--|--|
| City of Tampere | https://kartat.tampere.fi/oskari/?lang=en | Various datasets related to the city |
| Paituli | https://paituli.csc.fi/ | Collection of various Finnish data providers, mostly governmental institutes |
| Protected planet | https://www.protectedplanet.net/country/BRA | Worldwide collection of protected areas  |
| OpenStreetMap | https://www.openstreetmap.org/about | Community maintained data |
| Natural Earth | https://www.naturalearthdata.com/ | Various international datasets |

Data can often be accessed by downloading the dataset locally and importing the data, as we have done in the crash course. However, some providers allow you to download data directly to your GIS software using an online connection. While this has some benefits for the user and provider, it can cause issues when there's any connection issues. Thus, we recommend downloading and saving locally the datasets whenever possible. Some examples of online data access methods are:
- WMS/WMTS (For raster data viewing)
- WFS
- WCS
- XYZ

It is important when choosing your data to **be critical of the data you use!** Ask questions like:
- **Who** provides this data?
- How **accurate** is it?
- How **old** is it?

And be particularly careful when joining data from different sources! As these for example might not be on the same scale, CRS, units, or accuracy. 

## Digitizing

Where does GIS data come from? We can go out and collect it ourselves, like surveyors do. Maybe we attach a GPS system to a bird, and use that to track location for nature conservation. Or maybe, we trace over a building outline from an aerial photograph. Or, perhaps we have some spatial data (like a plan for a new development) but it is not able to be analysed because the drawing doesn't have the coordinate information yet? Or, if the data we need isn't already available or outdated? That's when we start gathering or making our own data, and where digitizing comes in.  

When some kind of urban development happens, the data we use in GIS needs to updated. Someone has to go in and update the changes to the roads, buildings, and others. This can be done by digitizing the changes, which is the process of **converting geographic data into digital form**. 

For this practice we are going to use the Nokia Arena development in Tampere, as you can see below, there have been a significant amount of changes with this project.

![](https://raw.githubusercontent.com/Tampere-University-Urban-Physics/fundamentals-of-gis/master/Assets/2_Theory/GIS_theory1_example.png)

And if we look at the data set we are working with, you can see that some of the demolished buildings are still present and new buildings lacking. 

![](https://raw.githubusercontent.com/Tampere-University-Urban-Physics/fundamentals-of-gis/master/Assets/2_Theory/QGIS_theory1_nokia_outdated.png)

In practice, updating this new development would consist of removing the old buildings from the data, and digitizing the new buildings, which would mean making new buildings in the data by **tracing** the buildings from some kind of reference picture. We can use GIS to edit different layers, selecting and deleting vector features that we no longer want, and add new features (and attributes) that we do want.

In this case we could potentially use the google imagery of the area, as this has already been updated. But this is not always the case, and satellite imagery is not the most accurate if the resolution is not high enough. To get the most accuracy, we would survey the area and record the locations using high-accuracy equipment. But for our purpose we don't require high accuracy and we will use the following project plan as a source for the digitizing. 

![](https://raw.githubusercontent.com/Tampere-University-Urban-Physics/fundamentals-of-gis/master/Assets/2_Theory/GIS_theory1_plan.png)

This source is however missing geographic data, as it is just a picture taken from a PDF. To align this picture with our geographic data, we need to apply a process called georeferencing - where we add spatial information to something to enable it to be located on earth. 

## Georeferencing

Georeferencing is essentially giving data without any CRS, a CRS. But more specifically, it is the process of **associating geographic data (coordinates) to a digital or physical object**, such as a map, image, or dataset, in our case a project plan. The goal of georeferencing is to establish a spatial reference for the object, enabling it to be positioned correctly within a coordinate system - essentially, applying spatial data to something that currently lacks it. It is essential for integrating different sources of geospatial data, enabling them to be overlaid and analyzed together. By assigning geographic coordinates or a coordinate system to an object, it becomes possible to accurately locate and spatially relate features, points, or areas represented within that object.

In practice, georeferencing involves identifying a set of control points on the object and matching them to corresponding locations on a reference map or in some form of software, such as QGIS. These control points represent identifiable features, such as road intersections or landmarks. A specific algorithm is then used by the GIS software to transforms the object to get it to match the reference system. This transformation can include rotating, stretching, or skewing the image you are referencing. Control points can be corners of easily identifiable buildings, or other features, and the more control points, and the more accurately they are placed, the better the match.

In our case we can use the buildings that remained unchanged and are on the project plan for reference to georeference the project plan, which we can then use to digitize the new buildings. 

# Time to get your hands dirty! Move on to the [2nd exercise](https://github.com/Tampere-University-Urban-Physics/fundamentals-of-gis/blob/master/Content/2_Exercise.md) to apply this new knowledge

<!--stackedit_data:
eyJkaXNjdXNzaW9ucyI6eyI1UHlscWNNVWkwdWQxR2pWIjp7In
RleHQiOiJHZW9yZWZlcmVuY2luZyIsInN0YXJ0Ijo0MjA5LCJl
bmQiOjQyMjN9fSwiY29tbWVudHMiOnsiWFE0d1B0MjZjdUhOck
t0eCI6eyJkaXNjdXNzaW9uSWQiOiI1UHlscWNNVWkwdWQxR2pW
Iiwic3ViIjoiZ2g6MjIxNjgxNTciLCJ0ZXh0IjoiVGhpcyB0aG
VvcnkgcmVhZHMgYSBsaXR0bGUgbW9yZSBsaWtlIGFuIGV4ZXJj
aXNlLiBJZiBwb3NzaWJsZSwgSSB3b3VsZCB1c2UgdGhpcyBvcH
BvcnR1bml0eSB0byBwcm92aWRlIG1vcmUgYmFja2dyb3VuZCBv
biBnZW9ncmFwaGljIGNvb3JkaW5hdGUgc3lzdGVtcywgcHJvam
VjdGVkIGNvb3JkaW5hdGUgc3lzdGVtcywgYW5kIGEgZmV3IHNl
bnRlbmNlcyBhYm91dCBob3cgR0lTIHRvb2xzIHNob3VsZCBiZS
BhYmxlIHRvIHRyYW5zZm9ybSBiZXR3ZWVuIHRoZW0uIiwiY3Jl
YXRlZCI6MTY4NjczMTM2MjI1OH19LCJoaXN0b3J5IjpbLTc3MD
gwMjU2MSwyMTQ0OTg0MjAsODk5MTA3NTEsMTQwNTU3NTA0Niw1
NjY0MDQ1NDQsNzM2NDkzOTc0LDEwOTI0MzM3MDVdfQ==
-->
