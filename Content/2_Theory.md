### Fundamentals of Geographic Information Systems (GIS)

# Theory 2: Projections, georeferencing & digitizing

*By Rowan van der Kaaden. Updated by Jonathon Taylor*

## Map projections and Coordinate Reference Systems (CRS)

In the previous exercise, you played around with projections a bit. What is a map projection? The Earth is round. Well, not perfectly round, as it is wider at the equator because of the forces of the earth's rotation, so more of an ellipsoid. To be technical, it is a bumpy ellipsoid (because of mountains and other bumpy things) that we call a geoid.

A map projection is used to portray part of the geoid on a flat surface, for example a piece of paper or a computer screen. If you have ever had to wrap a round present with wrapping paper, you will know that round surfaces don't translate that well to flat pieces of paper/a computer screen. This is why we need projections. The basic idea of projection is to project the shape of the earth (or a specific area that is big enough to be round and bumpy) onto a flat plane like the 2D maps you see on your computer screens or an atlas. Due to the original shape of the earth, a rough sphere, we will always get some kind of distortion when we project it onto a flat plane. This is shown well here: https://unchartedterritories.tomaspueyo.com/p/maps-distort-how-we-see-the-world 

![](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2f76b16e-edbe-4bc7-8781-f87f389ba29e_1224x1036.png)


There are numerous different projections that are used, and they all lead to some kind of distortion in some way. depending on the purpose of the map, some distortions are acceptible, others are not; they can all lead to differences in area, shape, direction, or distances shown, with different projctions prioritising soe things over others. There are different models for projections which are useful for describing understanding, and developing map projections (such as plane, cylinder, and cone). 

![Map Projection Families](https://docs.qgis.org/3.4/en/_images/projection_families.png)


A Geographic coordinate system is a way of describing a location on a round/ellipsoid/geodetic object. You are probably familiar with this coordinate system through longitude and latitude. Consider a point in the centre of the Earth. Draw a line from the centre of the earth to the equator. And another line from the centre of the earth to a location you are interested in. The angle between these two lines that goes North/South is called Latitude. The angle East/West is called Longitude. And both are given in degrees (because that's how we've measured angles in the past). So, the North Pole has a latitue of 90 degrees north, the equator has a latitude of 0 degrees. For longitude, the Greenwich Meridian in southeast London is usually the 0 degree reference point (in you are in London, you can visit it!). In the olden days, if you were sailing aorund the world exploring, you may have measured longitude and latitude in degrees, minutes, and seconds (with 60 minutes in a degree, and 60 seconds in a minute). Nowadawys, we tend to use decimal degrees.

When we are working with different kind of data, it is common that we have layers that are in different CRS. **Thus, it is important to check that the layers and the project are in the same CRS!** Most GIS data that we get comes with some CRS, if we decide to do something in some other projection, you can reproject data into a different CRS. This transformation is based on a known mathematical relationship between different CRS, where the location information is converted from one CRS to another.

Coordinate Reference Systems (CRS) are frameworks that define how geographic locations are represented and referenced in a coordinate system. There are many different CRS which use different Earth shape models and projection methods. The specific CRS used often depends on the region where the data is located, and have been specifically developed to reflect the curvature of the Earth in this location. For example, a global CRS is WGS84 (the code is EPSG:4326), which has latitude and longitude. But, if you use local maps, you may be more used to seeing the Finnish ETRS-TM35FIN (EPSG:3067) (Shown in the picture below), where map locations in CRS are given in metres where the X direction represents the location East/West and the Y direction the location North/South.

![ETRS=TM35FIN](https://upload.wikimedia.org/wikipedia/fi/1/15/ETRSTM35FIN.png)

Why does using the CRS matter? Have you even flown a long distance, for example to Asia or North America from Finland? You may have wondered why you have flown so far North/South. I mean, should't you just be going in a straight line based on the planar distance (or the blue line below)?

![](https://raw.githubusercontent.com/Tampere-University-Urban-Physics/fundamentals-of-gis/master/Assets/2_Theory/planar.png)

Well, as established - the world is not planar, it is geodesic. Using planar distances is fine for short distances, but in fact the shortest distance between two points is the geodesic distance. Hence, using planar coordinate systems can introduce errors as you look across larger distances. When you travel long distances in an airplane, you are in fact travelling the shortest route - in this case, we would call this the great circle route.

![](https://raw.githubusercontent.com/Tampere-University-Urban-Physics/fundamentals-of-gis/master/Assets/2_Theory/geodesic.png)

(Hungry for more? Coordinate Reference Systems are described in more detail in the QGIS documentation: https://docs.qgis.org/3.4/en/docs/gentle_gis_introduction/coordinate_reference_systems.html

You can also search for Youtube videos on this topic:
- CRS in QGIS: https://www.youtube.com/watch?v=EKKXXAahu3w
- Why all world maps are wrong: https://www.youtube.com/watch?v=kIID5FDi2JQ
- How do Map Projections work? https://www.youtube.com/watch?v=NAzy4S4EOwc )



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
