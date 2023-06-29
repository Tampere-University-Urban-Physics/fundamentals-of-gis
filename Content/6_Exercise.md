### Fundamentals of Geographic Information Systems (GIS)

# Exercise 6: Finding the source of a historic disease outbreak

## OVERVIEW & PURPOSE
This exercise uses data from a historical event in London, which many people consider to be the original use of mapping to understand disease transmission. In the 1800’s, London was a rapidly growing city, with huge amounts of migration from rural England to the city. However, the city lacked modern infrastructure, and disease and over-crowding was rife.

In 1854, there was a large cholera outbreak in London, with 120 deaths within three days in the Soho area of the city. Public health officials in the city were convinced that many diseases – including cholera – were spread by ‘miasma’, or toxic air and poor hygiene. However, a doctor and scientist in London, John Snow, was convinced that it was spread through contaminated water, but needed to find evidence to support his theory and convince city officials to close contaminated water pumps. To do this, he surveyed households in the area, plotted the locations of households with deaths, and used this to quantitative data to develop a theory about the contaminated water source.

Henry Whitehead – the local priest – knew the area and residents in Soho well, and initially did not agree with John Snow’s theory about the contaminated water pump because he knew of residents who drank from the well and did not get sick. He began his own study, interviewing residents using a qualitative social science approach, and eventually came to agree with the theory about the contaminated pump. Crucially, it was Whitehead’s interviews that uncovered how the well became contamined.

## OBJECTIVES

In this tutorial, we will learn about importing survey data and using QGIS to create a heatmap using this data, thus recreating one of the most famous epidemiological studies using modern GIS tools. The research question we want to ask ourselves is:
- What water pump was the source of the 1854 cholera outbreak in London?

## DATA USED/NEEDED

## Completion

Work in pairs or individually. Complete the exercise and submit a short report containing at least the following:

1. Maps of the outcome of the exercise

	- Remember: all maps should have a legend, a scale bar and a north arrow

	- Use the tips from the Crash Course to improve your map

2. A short reflection (200-300 words) on this exercise and your map

## EXERCISE PHASES

### Getting familiar with the data

1. Save the downloaded GIS data from Moodle in a folder for this exercise and extract it. The data includes:
	- deathAddresses.csv, a file with the locations of households with cholera deaths and the number of deaths 
	- Study_Area.shp – A shapefile that describes our area of interest.
	- Snow-cholera-map-1_modified - this is a georeferenced image of the map from John Snow’s original report on the cholera outbreak of 1854 
	- Water_Pumps.geojson. This shows the location of water pumps. The file type – a geojson – is an-other standard for representing geographical features, commonly used to transmit data in web applications. 
	- Snow-cholera-map-1_modified.tif, a raster showing a map of the area in 1854.

2. Open QGIS on your computer and start a new project. Save it with a new name under a folder for this exercise.

3. Import the Study_Area.shp file, to add it to your map project. Change the symbology to make the study area hollow (no fill, only borders)

4. Let’s bring in some Google Earth data into our map using XYZ tiles to give the map some more context (Hint: Exercise 2 step 3)


5. Often the data sets that you want to work with will not come as spatial data sets. They might come, for example, from observations during field work that you record in a spreadsheet. In this step we will **add a table of data that contains fields with the latitude and longitude coordinates of the deaths addresses we want to analyze**. In our case, this data comes in a comma-separated values (CSV) file, or a text file that uses a comma to separate values.
	
	- First let’s look at the file with the deaths data in it. If you double-click it in the folder you saved the file in, it should open in Excel or as a text file, and you will see that it has an ID column (OB-JECTID), the number of cases (Num_Cases ), an Address Column (Address), x coordinates (xcoord), and y coordinates (ycoord). These coordinates are helpful, as they let us import the data into GIS.
	- In QGIS, go to the *Delimited Text* section in the *Data Source Manager* 
	- Under File name, browse for the deathAddresses.csv file and select Open.
	- QGIS will read in the file, and should automatically recognize some key features of the file (Figure 2), including:
		- That the file format is comma delimited
		- That the first record in the file contains the names of the columns rather than data within the columns
		- That two of the fields in the file include spatial information relating to the x and y coordinates. Here, the CRS is EPSG:4326 - WGS 84 (make sure that it is set correctly).
	- If you press Add and Close, you should see that the addresses have been added to the QGIS map.

6. We have now added the addresses where there were cholera deaths, but it is in a global coordinate system, and this can cause some problems in our analysis because it is in a different coordinate system that the other data that we are going to use. Let’s **reproject it to the same as the study area bounding box** -EPSG:32630 UTM.
	
	- In the Processing Toolbox, search for "reproject" and open the *Reproject layer* tool
	- Choose deathAddresses.csv as the Input layer
	- Set the Target CRS as the Project CRS: EPSG:32630 UTM – WGS 84 / UTM zone 30N 
	- Don't forget to make sure it is permanent after

7. Let’s now **change the symbology of the deaths_locations layer to identify addresses that had multiple deaths**. 
	
	- Remove the original deathsAddresses csv layer
	- Go to the symbology of the new reprojected deaths_locations layer
	- Make the symbology *graduated* and the value "Num_Cases"
	- Change the method to *size*. Rather than changing the color of the symbol based on the number of cases, this will change the size of the symbol. You can change the size range, try out what you think looks best. 
	- Make the classification mode *Natural Breaks (remember what this means?) with three classes

8. We now have the locations of the deaths overlaid on top of a Google image of modern London. But, things have changed a little bit in this area of London since the outbreak. Let’s **bring in an old map of the area**. 
	
	- Import the raster Snow-cholera-map-1_modified.tif, which is an old streetmap of the area.
	- Order the layers in such a way that it makes sense to you 
	- Right-click Snow-cholera-map-1_modified.tif and choose properties. Under the *Transparency* tab, choose a percentage that makes the raster slightly transparent, so you can also see the more modern city underneath. 

---

### Finding the source of the outbreak

9. Now, let’s start trying to identify where the outbreak has come from. First, let’s **bring in data on the location of the water pumps**. 
	
	- Import the file Water_Pumps.geojson
	- Right-click the layer and look at the Attribute Table. What attributes are associated with each water pump?

10. Now, we want to **find out which water pumps is closest to the addresses where there have been deaths recorded**. Thiessen polygons are a method used to divide a space into regions based on their proximity to features. That is, within a Thiessen polygon, all the deaths are closer to the point (in this case a pump) that was used to generate that polygon, than to any other point (or pump) in the feature set. Let’s create a set of Thiessen polygons based upon the locations of the Water Pumps in our project.
	
	- From the processing toolbox, open the *Voronoi polygons* tool 
	- In the Voronoi tool select Water_Pumps as the Input layer
	- Set the Buffer region to 50% (What do you think this means?)

11. Now that you have created the Voronoi polygon layer, we can see how many deaths lay within these polygons – i.e. the count of deaths within the vicinity of each pump. There are various ways to do this. We could use a spatial join to “allocate” each of the deaths to one of the Voronoi polygons, much like we used spatial joins previously, and then sum the number of deaths. So now, though, let’s look at the map visually. **Which polygon do you think has the most deaths? Use the information tool to click on the polygon and find out the name of the pump, and make a note of it**.

12. Another possible way to identify the location of the outbreak could be to **find the spatial mean of the deaths – or the average x- and y-coordinate of all the features in the study area**. It’s useful for tracking changes in the distribution or for comparing the distributions of different types of features. Here, we will use the Mean Center to highlight the distribution of deaths.
	
	- Open the *Mean coordinate(s)* tool from the processing toolbox 
	- Select deaths_locations as the Input vector layer.
	- Set the Weight field as Num_Cases. By setting the weight to be the number of cases, we are accounting for the fact that some addresses had multiple deaths when we calculated the average coordinates. 
	- Click OK to calculate the Mean Center and Close. What water pump is the spatial mean closest to?

13. **Another way to show the distribution of cholera deaths is to create a heatmap**. A heatmap is a data visualization technique that shows magnitude of a phenomenon as color. In QGIS, the Kernel Density Tool calculates a magnitude per unit area from the point features using a kernel function to fit a smoothly tapered surface to each point. The result is a raster dataset which can reveal “hotspots” in the array of point data.
	
	- Open the tool *Heatmap (Kernel Density Estimation)* (Where do you find tools?)
	- Select the deaths_locations layer as the Point Layer.
	- Try out the radius and pixel size options, what settings do you think would be good? (Hint: Radius 50, pixel 1x1)
	- Select Num_Cases as the Weight Field. This will take into account the number of deaths at each address.
	- Press Run

14. Edit the Symbology of the heatmap layer in it’s properties. This can include:
	- Changing the symbology to singleband pseudocolour
	- Changing the legend settings to reduce the number of significant figures. In the symbology tab, go to Legend Settings -> Number Format (Customise). Round the Decimal places to 0.

---

### Map visualization 

15. All done! Make a final map in the layout manager, showing the locations of deaths, the water pumps, and the heatmap.
	- You can also make multiple maps, separating the data and making a map with the Thiessen polygons, do what you think is best and argument this in your reflection! 

<!--stackedit_data:
eyJkaXNjdXNzaW9ucyI6eyJ4MTV3V05CSzZKRHc3Vml2Ijp7In
RleHQiOiIjIyBEQVRBIFVTRUQvTkVFREVEIiwic3RhcnQiOjIw
MjIsImVuZCI6MjA0MX0sIndQYkR1OUFJVjdpMnprdXciOnsidG
V4dCI6Imdlb3JlZmVyZW5jZWQiLCJzdGFydCI6MjgwMiwiZW5k
IjoyODE1fSwiM2Q4amtCbnA4OUxaVlltZSI6eyJ0ZXh0IjoiTW
9vZGxlIiwic3RhcnQiOjI1MDksImVuZCI6MjUxNX0sIlE4WGFV
czJ3TXE5dzhZNlYiOnsic3RhcnQiOjE2NjMsImVuZCI6MTY2OS
widGV4dCI6ImJlY2FtZSJ9LCJaTlRNTkZMRzdzazJVcjJ3Ijp7
InN0YXJ0Ijo0MjQ2LCJlbmQiOjQyNDcsInRleHQiOiItIn19LC
Jjb21tZW50cyI6eyJHbmRSM2h4aGRxdjluSHIxIjp7ImRpc2N1
c3Npb25JZCI6IngxNXdXTkJLNkpEdzdWaXYiLCJzdWIiOiJnaD
o0MDMwNDc4OCIsInRleHQiOiJBZGQgc2VjdGlvbiIsImNyZWF0
ZWQiOjE2ODY3MjczNzMzODB9LCJmQmprM2xXZk8xek9RbHB1Ij
p7ImRpc2N1c3Npb25JZCI6IndQYkR1OUFJVjdpMnprdXciLCJz
dWIiOiJnaDo0MDMwNDc4OCIsInRleHQiOiJMZXQgc3R1ZGVudH
MgZG8gdGhpcz8iLCJjcmVhdGVkIjoxNjg2NzI3NTY3MTA4fSwi
YmNsY1dISFJxQVlLVnJ3TCI6eyJkaXNjdXNzaW9uSWQiOiIzZD
hqa0JucDg5TFpWWW1lIiwic3ViIjoiZ2g6NDAzMDQ3ODgiLCJ0
ZXh0IjoiRml4IHJlZmVyZW5jZSIsImNyZWF0ZWQiOjE2ODc3NT
k2MjU5ODV9LCJXdFN0Q2VDWnJDQ1Zra3AyIjp7ImRpc2N1c3Np
b25JZCI6IlE4WGFVczJ3TXE5dzhZNlYiLCJzdWIiOiJnaDoyMj
E2ODE1NyIsInRleHQiOiJjaGFuZ2VkIHRoZSB0ZXh0IHRvIG1h
a2UgaXQgYSBsaXR0bGUgbW9yZSBwcmVjaXNlLlxuXG4oSSB3cm
90ZSB0aGlzIGZvciB0aGUgc29jaWFsIHNjaWVuY2Ugc3R1ZGVu
dHMpIiwiY3JlYXRlZCI6MTY4ODAzNTE0NjIzN30sIjBoeXhXbD
FkQTBWZHJwZjQiOnsiZGlzY3Vzc2lvbklkIjoiWk5UTU5GTEc3
c2syVXIydyIsInN1YiI6ImdoOjIyMTY4MTU3IiwidGV4dCI6In
RoZXJlIGFyZSBvZnRlbiBkYXNoZXMgaW4gb2RkIHBsYWNlcz8i
LCJjcmVhdGVkIjoxNjg4MDM1NTE0MDQ3fX0sImhpc3RvcnkiOl
sxOTA3NjY0NjM4LC04NDMxNzMwNzQsNjA3NDE2NTk3LC0yMDMz
NDU0ODAzLC0zNjUyNTE4ODYsNTkyNTgyMjY0LC0xNTEyNDI3MD
YsLTU3MjcwNTYyOCw3NTAzMzAzODhdfQ==
-->