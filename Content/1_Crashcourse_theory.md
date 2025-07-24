### Fundamentals of Geographic Information Systems (GIS)

# Introduction to Geographic Information System (GIS) and QGIS

*By Rowan van der Kaaden*

## What is GIS and why do we use it?

GIS, which stands for Geographic Information System, is a technology that involves **capturing, storing, analyzing, and presenting geographically referenced data**. It integrates various types of information, such as maps, aerial imagery, and tabular data, into a digital framework that allows users to **view, understand, and analyze spatial patterns and relationships**.

GIS is used in a wide range of fields and industries, including:

1.  **Urban Planning**: GIS helps in analyzing land use patterns, transportation networks, and infrastructure planning. It enables urban planners to make informed decisions about zoning, growth management, and resource allocation.
2.  **Environmental Management**: GIS assists in monitoring and managing natural resources, such as forests, water bodies, and wildlife habitats. It aids in environmental impact assessments, conservation planning, and disaster management.
3.  **Public Health**: GIS is used to analyze disease outbreaks, track the spread of infectious diseases, and plan healthcare facilities and resources. It helps in understanding health disparities, identifying high-risk areas, and optimizing public health interventions.
4.  **Emergency Management**: GIS plays a crucial role in disaster preparedness, response, and recovery. It helps in identifying vulnerable areas, managing evacuation routes, and coordinating emergency services during natural disasters or other crises.
5.  **Transportation and Logistics**: GIS is utilized in optimizing transportation routes, analyzing traffic patterns, and planning efficient logistics networks. It assists in fleet management, supply chain optimization, and location-based services.
6.  **Natural Resource Management**: GIS is employed in assessing and managing natural resources like forests, water resources, minerals, and energy sources. It aids in land use planning, resource exploration, and environmental impact assessment.
7.  **Business and Marketing**: GIS helps businesses in market analysis, site selection, and customer profiling. It assists in identifying target markets, optimizing sales territories, and visualizing market trends.

These are just a few examples of the diverse applications of GIS. This course is not specifically tailored for a specific field, but it aims to give you a fundamental skillset in GIS that you can use to apply GIS in almost any field. 

### GIS workflow

PPDAC is a problem-solving framework commonly used in the field of data analysis and decision-making. It stands for Problem, Plan, Data, Analysis, and Conclusion. This framework provides a systematic approach to tackle complex problems and make informed decisions based on data-driven analysis. While PPDAC is not specifically tied to Geographic Information Systems (GIS), it can be applied within a GIS context to enhance the problem-solving process.

![](https://raw.githubusercontent.com/Tampere-University-Urban-Physics/fundamentals-of-gis/master/Assets/1_CrashCourse_Theory/GIS_PPDAC_v2.drawio.png)

By following the PPDAC framework, you can ensure a structured and systematic approach to problem-solving within the GIS domain. It helps in organizing your thoughts, guiding your analysis, and ensuring that your conclusions are based on robust data analysis.

For more information about the PPDAC Model: http://wiki.gis.com/wiki/index.php/PPDAC_Model

### Spatial data
Spatial data refers to **information that is associated with specific locations or geographic extents**. Vector and raster data are the two fundamental types of spatial data used in Geographic Information Systems (GIS) to represent and analyze spatial information.

![](https://cdn.mindspritesolutions.com/onestopgis/GIS-Theory-and-Techniques/Raster-Data-Analysis/Comparison-of-Vector-and-Raster-based-Data-Analysis/posts/Comparison-of-Vector-and-Raster-Based-Data-Analysis/Figure-shows-comparison-of-Vector-and-Raster.webp)

1. **Vector data** provides a way to represent real world features within the GIS environment. A feature is anything you can see on the landscape. Imagine you are standing on the top of a hill. Looking down you can see houses, roads, trees, rivers, and so on. Each one of these things would be a **feature** when we represent them in a GIS Application. Vector features have **attributes**, which consist of text or numerical information that **describe** the features[^1].  Types of vector data include:
	-  Points: Represent specific locations in space, such as the coordinates of a city or a landmark.
	-  Lines: Represent linear features, such as roads, rivers, or boundaries. Lines are defined by a series of connected nodes.
	-  Polygons: Represent enclosed areas or regions, such as administrative boundaries, land parcels, or thematic zones. Polygons are defined by a series of connected nodes forming a closed shape.

	Vector data provides precise and accurate representations of spatial features. Vector data does not lose resolution the more you zoom in - because it is based on coordinates of points or nodes, the GIS software simply redraws the feature. This type of way of looking at spatial features is often called the **object view**, where space is considered to be empty except when occupied by a feature.

2. **Raster Data** represents spatial information by a continuous surface divided into a grid of cells or pixels. Each cell corresponds to a specific location and contains a value representing a particular attribute. Imagine you are standing on the same hill as in the last example. Rather than identifying different features, you classify every surface. Key characteristics of raster data include:
	-  Grid: The analyzed surface is divided into a regular grid of cells, where each cell represents a specific location or area.
	-  Resolution: Raster data has a spatial resolution that defines the size or extent of each cell. Higher resolution means smaller cell sizes, resulting in more detailed data representation. This means that it has a set resolution, and can be blurry when you zoom in.
	-  Attribute Value: Each cell contains a value that represents a specific attribute, such as elevation, temperature, land cover type, or satellite reflectance.
	
	Raster data is commonly used for continuous and regularly sampled data, such as satellite imagery, digital elevation models (DEMs), or climate data. It is suitable for analyzing continuous phenomena, interpolating values, and performing terrain analysis. Raster data is less suitable for representing discrete features or sharp boundaries. In contrast to object view, raster data represents a **field view** way of looking at spatial data, where every spatial location has something, even if it is a zero value.

In GIS, both vector and raster data have their respective strengths and applications. Vector data is well-suited for representing discrete features and precise geometries, while raster data is ideal for representing continuous surfaces and performing spatial analysis based on cell values. GIS software allows for the integration, analysis, and visualization of both types of data, enabling users to explore and understand spatial information from different perspectives.

(Hungry for more? Further reading on vector and raster data in the QGIS documentation: https://docs.qgis.org/3.28/en/docs/gentle_gis_introduction/vector_data.html, https://docs.qgis.org/3.28/en/docs/gentle_gis_introduction/raster_data.html)

#### Spatial data sources
How and where to find spatial data is discussed in detail here: 

### Coordinate Reference Systems (CRS) and map projections
The Earth is round. Well, not perfectly round, as it is wider at the equator because of the forces of the earth's rotation, so more of an ellipsoid. To be technical, it is a bumpy ellipsoid (because of mountains and other bumpy things) that we call a geoid.

A Geographic coordinate system is a way of describing a location on a round/ellipsoid/geodetic object. You are probably familiar with this coordinate system through longitude and latitude. Consider a point in the centre of the Earth. Draw a line from the centre of the earth to the equator. And another line from the centre of the earth to a location you are interested in. The angle between these two lines that goes North/South is called Latitude. The angle East/West is called Longitude. And both are given in degrees (because that's how we've measured angles in the past). So, the North Pole has a latitue of 90 degrees north, the equator has a latitude of 0 degrees. For longitude, the Greenwich Meridian in southeast London is usually the 0 degree reference point (in you are in London, you can visit it!).

If you have ever had to wrap a round present with wrapping paper, you will know that round surfaces don't translate that well to flat pieces of paper/a computer screen. This is why we need projections. The basic idea of projection is to project the shape of the earth (or a specific area that is big enough to be round and bumpy) onto a flat plane like the 2D maps you see on your computer screens or an atlas. Due to the original shape of the earth, a rough sphere, we will always get some kind of distortion when we project it onto a flat plane. This is shown well here: https://unchartedterritories.tomaspueyo.com/p/maps-distort-how-we-see-the-world 

![](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2f76b16e-edbe-4bc7-8781-f87f389ba29e_1224x1036.png)

Coordinate Reference Systems (CRS) are frameworks that define how geographic locations are represented and referenced in a coordinate system. There are many different CRS which use different Earth shape models and projection methods. The specific CRS used often depends on the region where the data is located, and have been specifically developed to reflect the curvature of the Earth in this location. Rather than longitude or latitude, map locations in CRS are usually given in metres where the X direction represents the location East/West and the Y direction the location North/South.

When we are working with different kind of data, it is common that we have layers that are in different CRS. **Thus, it is important to check that the layers and the project are in the same CRS!** Most GIS data that we get comes with some CRS, if we decide to do something in some other projection, you can reproject data into a different CRS. We will go into how to do this later. 

![Map Projection Families](https://docs.qgis.org/3.4/en/_images/projection_families.png)

Some common CRS include:
- Finland: ETRS-TM35FIN (EPSG:3067) (Shown in the picture below)
- GPS: WGS 84 (EPSG:4326)

![ETRS=TM35FIN](https://upload.wikimedia.org/wikipedia/fi/1/15/ETRSTM35FIN.png)

(Hungry for more? Coordinate Reference Systems are described in more detail in the QGIS documentation: https://docs.qgis.org/3.4/en/docs/gentle_gis_introduction/coordinate_reference_systems.html

You can also search for Youtube videos on this topic:
- CRS in QGIS: https://www.youtube.com/watch?v=EKKXXAahu3w
- Why all world maps are wrong: https://www.youtube.com/watch?v=kIID5FDi2JQ
- How do Map Projections work? https://www.youtube.com/watch?v=NAzy4S4EOwc )
 


### GIS analysis types
The most common GIS (Geographic Information System) analysis types can vary depending on the specific application and industry. However, some of the widely used and common GIS analysis types are:

1.  **Spatial Query and Selection**: This analysis involves selecting or querying spatial features based on their spatial relationship with other features. It helps answer questions like "Which points are within a certain distance of a line?" or "Which polygons intersect a specific area?"
2.  **Spatial Overlay and Intersection**: Overlay analysis combines multiple spatial datasets to create new datasets by intersecting, unioning, or differencing features. It helps identify areas of overlap, calculate areas, and analyze spatial relationships between different layers.
3.  **Buffer Analysis**: Buffer analysis creates a proximity zone around a specific feature or set of features. It helps assess the impact or accessibility of certain locations, such as finding all points within a certain distance of a river or road.
4.  **Network Analysis**: Network analysis focuses on analyzing and modeling networks, such as road networks or utility networks. It includes tasks like route optimization, shortest path analysis, service area analysis, and network connectivity analysis.
5.  **Spatial Interpolation**: Interpolation methods estimate attribute values at unsampled locations based on values observed at nearby sampled locations. It helps create continuous surfaces from point data, such as generating elevation models or estimating pollution levels.
6.  **Hotspot Analysis**: Hotspot analysis identifies statistically significant clusters or concentrations of spatial features. It helps identify areas of high or low values, such as identifying crime hotspots or disease clusters.
7.  **Density Analysis**: Density analysis calculates the density of spatial features within a specific area. It helps identify areas of high or low density, such as population density or density of specific events.
8.  **Geocoding and Geolocation**: Geocoding involves converting addresses or place names into spatial coordinates, while geolocation refers to determining the location of an entity based on its geographic features. These techniques are used for mapping, spatial referencing, and analysis.
9.  **Spatial Regression**: Spatial regression methods explore relationships between spatially referenced variables, considering spatial autocorrelation and spatial dependency. It helps understand how variables interact across space and can be used for predictive modeling or identifying spatial patterns in relationships.
10.  **Thematic Mapping and Cartography**: GIS analysis includes creating visually appealing and informative maps, charts, and graphs to visualize spatial patterns and analysis results effectively.

These are some of the most common GIS analysis types, but the field of GIS offers a vast range of analysis techniques and tools that can be tailored to specific needs and applications. 


## What is QGIS?
In short, QGIS is a GIS application that can be used for various GIS analyses. There are other GIS applications, the other most common of which is ArcGIS. We are using QGIS for this course since it is open-source and free, meaning it is accesible for all, in contrast to ArcGIS which is proprietary software. 

So, you're going to need QGIS for this course! Please take a moment now to ensure you have it installed: https://qgis.org/en/site/forusers/download.html# Our recommendation is to install the Long Term Release (LTR) version, since there is less change of problems with this version. The exercises in this course have been developed for the English language installation of QGIS.

You can find more information about the usage of QGIS here: https://docs.qgis.org/3.28/en/docs/index.html

## What is a good map?

A **good map**: 
1. Has clear and objective symbology that represents the data correctly
2. Is useful for the target audience
3. Has an adequate projection and scale(bar)
4. Has a north arrow
5. Has a legend with explanations of used symbology/colours etc.
6. Notes the data sources, used projection, and credits

Important to keep in mind is that: **maps are images that inherently selective and result from choices made by a cartographer**. There is no such thing as an "objective map". In reality, maps always emphasize some elements while ignoring others (or pushing them to the "background") and there's power involved. The fact that maps are images also means that a map is often accompanied by a caption that tells the reader what they are expected to see on the map. Making good captions is a skill in itself, as captions influence the way in which people interpret the maps (as well as the reality they are attempting to depict).  

Another important thing to keep in mind is to **make your maps accessible to all readers, including those who have some form of color blindness**. This is relatively simple, since QGIS already has a tool build-in to simulate a few color blind examples:

![](https://raw.githubusercontent.com/Tampere-University-Urban-Physics/fundamentals-of-gis/master/Assets/1_CrashCourse_Theory/1_CrashCourse_theory_QGIS_color_blindness.png)

You can get color advice for your maps here: https://colorbrewer2.org/ 


# Time to get your hands dirty! Move on to the [Crash Course exercise](https://github.com/Tampere-University-Urban-Physics/fundamentals-of-gis/blob/master/Content/1_Crashcourse_exercise.md) to get started with (Q)GIS  

[^1]: https://docs.qgis.org/3.28/en/docs/gentle_gis_introduction/vector_data.html
<!--stackedit_data:
eyJkaXNjdXNzaW9ucyI6eyJyZ3F6bUxVTU9MVnc4ZFpGIjp7In
N0YXJ0Ijo1NjEsImVuZCI6NjI1LCJ0ZXh0IjoiR0lTIGlzIHVz
ZWQgaW4gYSB3aWRlIHJhbmdlIG9mIGZpZWxkcyBhbmQgaW5kdX
N0cmllcywgaW5jbHVkaW5nOiJ9LCJKQXJmY3BCa3ozaWN1b0Zn
Ijp7InN0YXJ0Ijo5MDM5LCJlbmQiOjkwNjEsInRleHQiOiIjIy
MgR0lTIGFuYWx5c2lzIHR5cGVzIn0sIk5ZQlVJUTdYZHVZWkd1
YzUiOnsic3RhcnQiOjEyNzgxLCJlbmQiOjEyODAzLCJ0ZXh0Ij
oiIyMgV2hhdCBpcyBhIGdvb2QgbWFwPyJ9LCJReTZ0eTZGSDBr
aTFlOGNFIjp7InN0YXJ0IjozNTIwLCJlbmQiOjM1MzYsInRleH
QiOiIjIyMgU3BhdGlhbCBkYXRhIn0sIm1LMno3ck5ESG9sNDc2
OFgiOnsic3RhcnQiOjU4MTUsImVuZCI6NjE1NCwidGV4dCI6Il
Jhc3RlciBkYXRhIGlzIGNvbW1vbmx5IHVzZWQgZm9yIGNvbnRp
bnVvdXMgYW5kIHJlZ3VsYXJseSBzYW1wbGVkIGRhdGEsIHN1Y2
ggYXPigKYifSwiZERWbGw5UXY4Y1hJSFRTOSI6eyJzdGFydCI6
NDk5MCwiZW5kIjo1MDY4LCJ0ZXh0IjoiVmVjdG9yIGRhdGEgcH
JvdmlkZXMgcHJlY2lzZSBhbmQgYWNjdXJhdGUgcmVwcmVzZW50
YXRpb25zIG9mIHNwYXRpYWwgZmVhdHVyZXMuIn19LCJjb21tZW
50cyI6eyIxZXBNRnA1YVdUVW4xWW1wIjp7ImRpc2N1c3Npb25J
ZCI6InJncXptTFVNT0xWdzhkWkYiLCJzdWIiOiJnaDo0MDMwND
c4OCIsInRleHQiOiJBZGQgcGljdHVyZXMiLCJjcmVhdGVkIjox
Njg3MDY4MDY4NjUxfSwiNXFZWnk2cmhOTzlGQU41ZCI6eyJkaX
NjdXNzaW9uSWQiOiJKQXJmY3BCa3ozaWN1b0ZnIiwic3ViIjoi
Z2g6NDAzMDQ3ODgiLCJ0ZXh0IjoiQWRkIGV4YW1wbGUgcGljdH
VyZXMgYW5kIHN0dWRpZXMiLCJjcmVhdGVkIjoxNjg3MDY4MTIy
MDgzfSwiTzdDQ005RVl6V2lFMjVETCI6eyJkaXNjdXNzaW9uSW
QiOiJOWUJVSVE3WGR1WVpHdWM1Iiwic3ViIjoiZ2g6NDAzMDQ3
ODgiLCJ0ZXh0IjoiQWRkIGdvb2QgYW5kIGJhZCBtYXAgZXhhbX
BsZXMiLCJjcmVhdGVkIjoxNjg3MDY4MTQzNDI3fSwibTV0T2lW
dDhXTnpyRzF0SiI6eyJkaXNjdXNzaW9uSWQiOiJReTZ0eTZGSD
BraTFlOGNFIiwic3ViIjoiZ2g6NDAzMDQ3ODgiLCJ0ZXh0Ijoi
QWRkIGRhdGEgZmlsZSB0eXBlcyBzZWN0aW9uIiwiY3JlYXRlZC
I6MTY4NzA2ODI2NTg0OX0sIkRzYzJ3c1gxR0JaNzRDdmEiOnsi
ZGlzY3Vzc2lvbklkIjoibUsyejdyTkRIb2w0NzY4WCIsInN1Yi
I6ImdoOjQwMzA0Nzg4IiwidGV4dCI6IkFkZCByZWFsIGxpZmUg
ZXhhbXBsZSBvZiByYXN0ZXIgZGF0YSIsImNyZWF0ZWQiOjE2OT
UxMDYzNTEwMTF9LCJlV0xSVE5jakQyQmtYTUZGIjp7ImRpc2N1
c3Npb25JZCI6ImREVmxsOVF2OGNYSUhUUzkiLCJzdWIiOiJnaD
o0MDMwNDc4OCIsInRleHQiOiJBZGQgcmVhbCBsaWZlIGV4YW1w
bGUgb2YgdmVjdG9yIGRhdGEiLCJjcmVhdGVkIjoxNjk1MTA2Mz
Y3NjY4fX0sImhpc3RvcnkiOlstMTMxMjY3NzY4LC0xMDIzNDg0
MDgsLTQ4NDc4MTUwMywtMTcyMTA4NTg4MCw0MjI1OTgzMTYsLT
E4NjgyNDkyNDRdfQ==
-->
