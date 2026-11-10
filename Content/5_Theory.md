
### Fundamentals of Geographic Information Systems (GIS)

# Theory 5: Digital Elevation Model, Hillshade, and raster overlay analysis

*By Rowan van der Kaaden*

Let's get back to rasters. As discussed before, rasters are gridded data where each pixel/grid represents some information. We can think of aerial photographs as rasters, where each pixel of the photo tells us the colour of the ground. Or, maybe the pixels tells us the height of the ground, or some kind of climate variable like temperatures. Rasters represent the field view, where the geographical space (each pixel) is filled with some value.

## Digital Elevation Model (DEM)
There are various ways to represent three dimensional surfaces in GIS, including contour maps (which you may have seen before) and Digital Elevation Models. A Digital Elevation Model (DEM) is a three-dimensional representation of the Earth's surface that is created using elevation data. It is a digital representation of the topography of a particular area, typically displayed as a grid of elevation values. 

![](https://github.com/Tampere-University-Urban-Physics/fundamentals-of-gis/blob/master/Assets/5_Theory/5_Theory_DEM_finland.png?raw=true)

DEM data is collected using various methods, including remote sensing techniques such as LiDAR (Light Detection and Ranging), radar, or photogrammetry. These methods involve sending out signals or capturing images from different perspectives to measure the height of the Earth's surface at specific points.

The resulting elevation data is then processed and interpolated to create a continuous representation of the terrain. The grid of elevation values in a DEM is often represented as a raster dataset, where each cell or pixel contains the elevation information for that location.

DEM data has numerous applications in various fields, including geography, geology, environmental science, urban planning, and engineering. It is used for visualizing and analyzing terrain characteristics, mapping landforms, identifying slopes and aspect, hydrological modeling, determining visibility and line of sight, and more. DEMs are essential in creating accurate maps, conducting terrain analysis, and supporting decision-making processes in numerous industries and research domains. 

DEMs are often used as a generic term for Digital Surface Models (DSMs) and Digital Terrain Models (DTMs). A DSM represents the elevation of the earth's surface and all the objects on it. For example, a city would have a variable DSM that shows the changes in heights due to buildings. In contrast, a DTM model represents the ground without any objects on it.

### Sinks
Filling sinks in a DEM is an important preprocessing step that addresses a common issue in elevation data. A sink refers to a depression or low-lying area in the terrain where water can accumulate, such as a small basin or a closed contour. When sinks are present in a DEM, they can introduce inaccuracies and create problems in subsequent analyses or applications. 

The process of filling sinks typically involves identifying the depressions or sinks in the DEM and modifying the elevation values within those areas to ensure a continuous and realistic representation of the terrain. This can be done using various algorithms or techniques, such as the breaching or filling algorithms, which raise the elevation of the sinks until they reach the surrounding terrain level. By filling sinks, the DEM becomes more suitable for accurate and reliable analysis and visualization of the terrain.

(Want to learn more about why and when filling sinks in necessary? Check videos such as: https://www.youtube.com/watch?v=6rcx4OwnryI)

### Hillshade
The hillshade technique is a method used to create a shaded relief representation of a DEM or terrain surface. It simulates the effects of lighting on the terrain to enhance the visual perception of the three-dimensional characteristics of the landscape. The resulting hillshade image provides a sense of depth and helps to emphasize the topographic features of the terrain.

![](https://github.com/Tampere-University-Urban-Physics/fundamentals-of-gis/blob/master/Assets/5_Theory/5_Theory_hillsade_finland.png?raw=true)

The hillshade technique calculates the illumination and shadows on the terrain surface based on the direction of the light source, the elevation values of the DEM, and the slope and aspect of each pixel. 

The hillshade technique is widely used in cartography, geospatial analysis, and visualization applications. It helps in understanding the topography of an area, identifying landforms, visualizing slopes and ridges, and enhancing the aesthetics of terrain maps.

### Slope
A DEM provides the necessary data to perform slope analysis. With a DEM, you can calculate and analyze the slope of the terrain at various locations. Slope analysis allows you to understand the steepness of the landscape, identify areas of high or low slope, and assess the terrain characteristics for various applications.

Slope analysis using a DEM is valuable in a range of applications, including terrain characterization, land management, environmental assessment, and infrastructure planning. It provides insights into the topographic features of the landscape, supporting decision-making processes and aiding in understanding the terrain's behavior and suitability for various purposes.

### Contours
When looking at a topological map, you may also have come across maps that have contours. Contours are another way of visualizing slope (using vectors), with lines which represent similar elevations. The closeness of the contour lines provide an idea of areas with gradual and rapid slope changes, as well as peaks and valleys.

## Raster  analysis
Just like with vectors, we can do analyses and calculations with rasters. Raster overlay analysis is a fundamental geospatial analysis technique in GIS that involves combining and analyzing multiple raster layers to derive new information or gain insights into the relationships between different spatial datasets. It enables you to perform complex operations by overlaying and integrating raster datasets, pixel by pixel, to generate output layers with new attributes or identify areas that meet specific criteria. In QGIS, the Raster Calculator tool is commonly used to male calculations on rasters.

In raster overlay analysis, each raster layer is composed of a grid of cells or pixels, with each cell containing a value representing a specific attribute or measurement. When overlaying two or more raster layers, the values of the corresponding cells are compared and combined based on predefined rules or operations. The most common operations used in raster overlay analysis include:

1.  **Overlay operations**: These operations combine two or more raster layers to create a new output layer. Examples of overlay operations include:
    
    -   *Union*: Combines the values of overlapping cells from multiple layers, often used to identify areas of overlap or convergence.
    -   *Intersection*: Retains only the values of cells where all input layers have non-zero values, useful for identifying areas that satisfy multiple conditions.
    -   *Difference*: Compares the values of cells between layers and outputs the difference or change.

2.  **Mathematical operations**: These operations perform mathematical calculations on the values of corresponding cells in different layers. Common mathematical operations include addition, subtraction, multiplication, division, and statistical calculations like mean, median, or standard deviation.
    
3.  **Boolean operations**: These operations utilize Boolean logic (AND, OR, NOT) to determine the presence or absence of certain conditions or attributes. They help create binary or categorical output layers based on specified criteria. They can also be used to reclassify raster data.

Raster overlay analysis is used in various GIS applications, such as land suitability analysis, environmental modeling, habitat analysis, urban planning, and natural resource management. It allows for the integration of diverse datasets, including DEMs, satellite imagery, land cover maps, and thematic data, to generate valuable information and support decision-making processes. By overlaying and combining raster layers, you can explore spatial relationships, identify patterns, quantify spatial characteristics, and derive meaningful insights from the data.



# Time to get your hands dirty! Move on to the [5th exercise](https://github.com/Tampere-University-Urban-Physics/fundamentals-of-gis/blob/master/Content/5_Exercise.md) to apply this new knowledge
<!--stackedit_data:
eyJkaXNjdXNzaW9ucyI6eyJRYmU0dGF0bkVJcTZ3N0dCIjp7In
RleHQiOiJhbmQgRCIsInN0YXJ0IjozMjMsImVuZCI6MzI4fSwi
Q2lZUzZ6NW93cGk3eXdOZiI6eyJzdGFydCI6MTc4MiwiZW5kIj
oyMjczLCJ0ZXh0IjoiREVNIGRhdGEgaGFzIG51bWVyb3VzIGFw
cGxpY2F0aW9ucyBpbiB2YXJpb3VzIGZpZWxkcywgaW5jbHVkaW
5nIGdlb2dyYXBoeSwgZ2VvbOKApiJ9LCJQTVF0dG1nT25kdWt6
SUdjIjp7InN0YXJ0IjoyNjM3LCJlbmQiOjI2NDYsInRleHQiOi
IjIyMgU2lua3MifSwiWk5mR0NDRkt4SWdhTnExRyI6eyJzdGFy
dCI6NTAxMywiZW5kIjo1MzgyLCJ0ZXh0IjoiU2xvcGUgYW5hbH
lzaXMgdXNpbmcgYSBERU0gaXMgdmFsdWFibGUgaW4gYSByYW5n
ZSBvZiBhcHBsaWNhdGlvbnMsIGluY2x1ZGluZyB0ZeKApiJ9LC
JxTFV3czF2NjdLZUM1S1lGIjp7InN0YXJ0Ijo0Njc4LCJlbmQi
OjQ2ODcsInRleHQiOiIjIyMgU2xvcGUifSwiMm1KMDF1NnpOVU
M3VzNMSiI6eyJzdGFydCI6NTM4NCwiZW5kIjo1Mzk2LCJ0ZXh0
IjoiIyMjIENvbnRvdXJzIn19LCJjb21tZW50cyI6eyIyY2lFc0
tFMGJHYnlYNDVPIjp7ImRpc2N1c3Npb25JZCI6IlFiZTR0YXRu
RUlxNnc3R0IiLCJzdWIiOiJnaDoyMjE2ODE1NyIsInRleHQiOi
Jjb3VsZCBtZW50aW9uIFRJTiBhbmQgb3RoZXJzIHRvbywgSSBn
dWVzcywgYnV0IG1heWJlIHRoaXMgbWFrZXMgaXQgdG9vIGNvbX
BsaWNhdGVkLlxuXG5UaGUgcG9pbnQgaXMgdG8gbWFrZSBzdHVk
ZW50cyByZWFsaXNlIHRoYXQgdGhlcmUgaXMgbW9yZSB0aGFuIG
9uZS5cblxuQ291bGQgYWxzbyBjb25zaWRlciB0YWxraW5nIGFi
b3V0IHRoZSBkaWZmZXJlbmNlcyBiZXR3ZWVuIERpZ2l0YWwgRW
xldmF0aW9uIE1vZGVsLCBEaWdpdGFsIFRlcnJhaW4gTW9kZWws
IERpZ2l0YWwgU3VyZmFjZSBNb2RlbC4uLiIsImNyZWF0ZWQiOj
E2ODgwMzM2NjY1MTl9LCJwb2pXMlhDSW9DaTVCazlNIjp7ImRp
c2N1c3Npb25JZCI6IkNpWVM2ejVvd3BpN3l3TmYiLCJzdWIiOi
JnaDo0MDMwNDc4OCIsInRleHQiOiJBZGQgc29tZSByZWFsIHdv
cmxkIGV4YW1wbGVzLiIsImNyZWF0ZWQiOjE3OTQyOTY5ODMzMD
Z9LCJCdGd5V05iNHJ3ZUNqM0dQIjp7ImRpc2N1c3Npb25JZCI6
IlBNUXR0bWdPbmR1a3pJR2MiLCJzdWIiOiJnaDo0MDMwNDc4OC
IsInRleHQiOiJBZGQgYSB2aXN1YWwgZXhhbXBsZSIsImNyZWF0
ZWQiOjE3OTQyOTcxNzQxODl9LCI2OEdDS2FOQlRkWGxIMTlCIj
p7ImRpc2N1c3Npb25JZCI6IlpOZkdDQ0ZLeElnYU5xMUciLCJz
dWIiOiJnaDo0MDMwNDc4OCIsInRleHQiOiJBZGQgcmVhbCBsaW
ZlIGV4YW1wbGUiLCJjcmVhdGVkIjoxNzk0Mjk3MjU5ODc1fSwi
Yk1yZ3c5ZmZQRjBGMGhpcyI6eyJkaXNjdXNzaW9uSWQiOiJxTF
V3czF2NjdLZUM1S1lGIiwic3ViIjoiZ2g6NDAzMDQ3ODgiLCJ0
ZXh0IjoiQWRkIHZpc3VhbCIsImNyZWF0ZWQiOjE3OTQyOTcyNj
Q3NTh9LCJCdklXWWx6eE9oalI5V0paIjp7ImRpc2N1c3Npb25J
ZCI6IlpOZkdDQ0ZLeElnYU5xMUciLCJzdWIiOiJnaDo0MDMwND
c4OCIsInRleHQiOiJ3aXRoIHBpY3R1cmVzIiwiY3JlYXRlZCI6
MTc5NDI5NzI3MjQ3MX0sIjlJdXJ2MUdyeE1SME9RVjgiOnsiZG
lzY3Vzc2lvbklkIjoiMm1KMDF1NnpOVUM3VzNMSiIsInN1YiI6
ImdoOjQwMzA0Nzg4IiwidGV4dCI6IkFkZCBleGFtcGxlIHZpc3
VhbCIsImNyZWF0ZWQiOjE3OTQyOTcyODE1ODV9fSwiaGlzdG9y
eSI6Wy03NTM1ODc5NTEsLTEzMDk5NDA0NDgsNjU4ODM1MDQsMT
QxNDkyNTg5MywxMzgzNDc0MjA1LDY5MjczMzIyOCwxMjA4NDAw
MjAxLDExMjczNzU1MTksLTE5NDY3NzU0NjEsMTgwOTA1NjcsMj
AxNDEyNzExNywtODg1MjkwMzMzLC04OTYyMjcyODEsOTk0NzIw
MTkzLC0zNTEwNzY1ODAsLTEzOTAzMzIwNTFdfQ==
-->