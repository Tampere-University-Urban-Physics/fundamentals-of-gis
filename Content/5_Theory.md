
### Fundamentals of Geographic Information Systems (GIS)

# Theory 5: Digital Elevation Model, Hillshade, and raster overlay analysis

## Digital Elevation Model (DEM)
A digital elevation model (DEM) is a three-dimensional representation of the Earth's surface that is created using elevation data. It is a digital representation of the topography of a particular area, typically displayed as a grid of elevation values.

![](https://github.com/rowan8k/fundamentals-of-gis/blob/master/Assets/5_Theory/5_Theory_DEM_finland.png?raw=true)

DEM data is collected using various methods, including remote sensing techniques such as LiDAR (Light Detection and Ranging), radar, or photogrammetry. These methods involve sending out signals or capturing images from different perspectives to measure the height of the Earth's surface at specific points.

The resulting elevation data is then processed and interpolated to create a continuous representation of the terrain. The grid of elevation values in a DEM is often represented as a raster dataset, where each cell or pixel contains the elevation information for that location.

DEM data has numerous applications in various fields, including geography, geology, environmental science, urban planning, and engineering. It is used for visualizing and analyzing terrain characteristics, mapping landforms, identifying slopes and aspect, hydrological modeling, determining visibility and line of sight, and more. DEMs are essential in creating accurate maps, conducting terrain analysis, and supporting decision-making processes in numerous industries and research domains.

###- DEM
	- Filling sinks in a DEM
Filling sinks in a DEM is an important preprocessing step that addresses a common issue in elevation data. A sink refers to a depression or low-lying area in the terrain where water can accumulate, such as a small basin or a closed contour. When sinks are present in a DEM, they can introduce inaccuracies and create problems in subsequent analyses or applications. 

The process of filling sinks typically involves identifying the depressions or sinks in the DEM and modifying the elevation values within those areas to ensure a continuous and realistic representation of the terrain. This can be done using various algorithms or techniques, such as the breaching or filling algorithms, which raise the elevation of the sinks until they reach the surrounding terrain level. By filling sinks, the DEM becomes more suitable for accurate and reliable analysis and visualization of the terrain.

(Want to learn more about why and when filling sinks in necessary? Check videos such as:
	- https://www.youtube.com/watch?v=6rcx4OwnryI)

###
- Hillshade
The hillshade technique is a method used to create a shaded relief representation of a DEM or terrain surface. It simulates the effects of lighting on the terrain to enhance the visual perception of the three-dimensional characteristics of the landscape. The resulting hillshade image provides a sense of depth and helps to emphasize the topographic features of the terrain.

![](https://github.com/rowan8k/fundamentals-of-gis/blob/master/Assets/5_Theory/5_Theory_hillsade_finland.png?raw=true)

The hillshade technique calculates the illumination and shadows on the terrain surface based on the direction of the light source, the elevation values of the DEM, and the slope and aspect of each pixel. 

The hillshade technique is widely used in cartography, geospatial analysis, and visualization applications. It helps in understanding the topography of an area, identifying landforms, visualizing slopes and ridges, and enhancing the aesthetics of terrain maps.

### Slope
A DEM provides the necessary data to perform slope analysis. With a DEM, you can calculate and analyze the slope of the terrain at various locations. Slope analysis allows you to understand the steepness of the landscape, identify areas of high or low slope, and assess the terrain characteristics for various applications.

Slope analysis using a DEM is valuable in a range of applications, including terrain characterization, land management, environmental assessment, and infrastructure planning. It provides insights into the topographic features of the landscape, supporting decision-making processes and aiding in understanding the terrain's behavior and suitability for various purposes.

## Raster overlay analysis
Raster overlay analysis is a fundamental geospatial analysis technique in GIS that involves combining and analyzing multiple raster layers to derive new information or gain insights into the relationships between different spatial datasets. It enables you to perform complex operations by overlaying and integrating raster datasets, pixel by pixel, to generate output layers with new attributes or identify areas that meet specific criteria.

In raster overlay analysis, each raster layer is composed of a grid of cells or pixels, with each cell containing a value representing a specific attribute or measurement. When overlaying two or more raster layers, the values of the corresponding cells are compared and combined based on predefined rules or operations. The most common operations used in raster overlay analysis include:

1.  **Overlay operations**: These operations combine two or more raster layers to create a new output layer. Examples of overlay operations include:
    
    -   *Union*: Combines the values of overlapping cells from multiple layers, often used to identify areas of overlap or convergence.
    -   *Intersection*: Retains only the values of cells where all input layers have non-zero values, useful for identifying areas that satisfy multiple conditions.
    -   *Difference*: Compares the values of cells between layers and outputs the difference or change.

2.  **Mathematical operations**: These operations perform mathematical calculations on the values of corresponding cells in different layers. Common mathematical operations include addition, subtraction, multiplication, division, and statistical calculations like mean, median, or standard deviation.
    
3.  **Boolean operations**: These operations utilize Boolean logic (AND, OR, NOT) to determine the presence or absence of certain conditions or attributes. They help create binary or categorical output layers based on specified criteria.

Raster overlay analysis is used in various GIS applications, such as land suitability analysis, environmental modeling, habitat analysis, urban planning, and natural resource management. It allows for the integration of diverse datasets, including DEMs, satellite imagery, land cover maps, and thematic data, to generate valuable information and support decision-making processes. By overlaying and combining raster layers, you can explore spatial relationships, identify patterns, quantify spatial characteristics, and derive meaningful insights from the data.
- Slope
- Data reclassification
- Raster calculator
- Raster overlay analysis (calculations)


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUzNDI0MDgyLC0xOTQ2Nzc1NDYxLDE4MD
kwNTY3LDIwMTQxMjcxMTcsLTg4NTI5MDMzMywtODk2MjI3Mjgx
LDk5NDcyMDE5MywtMzUxMDc2NTgwLC0xMzkwMzMyMDUxXX0=
-->