
### Fundamentals of Geographic Information Systems (GIS)

# Exercise 7: Air pollution analysis

By Jonathon Taylor

*Updated by Rowan van der Kaaden* 

## OVERVIEW & PURPOSE

Air pollution analysis is a crucial aspect of understanding the distribution and impact of pollutants on human health and the environment. Air pollution has become a pressing global concern due to its detrimental effects on both society and ecosystems. The analysis of air pollution provides valuable insights into the spatial patterns, sources, and concentrations of pollutants, enabling informed decision-making for mitigation strategies and policy development.

Air pollution analysis aims to assess the levels of pollutants such as particulate matter, nitrogen dioxide, sulfur dioxide, and ozone in the atmosphere. These pollutants are associated with adverse health effects, including respiratory and cardiovascular diseases, reduced lung function, and increased mortality rates. Additionally, air pollution can have significant social implications, particularly for vulnerable populations residing in highly polluted areas. Environmental justice issues arise when certain communities, often marginalized or low-income, are disproportionately exposed to higher levels of air pollution, exacerbating existing health disparities.

Understanding the social implications of air pollution is crucial for promoting environmental justice and equitable decision-making. Marginalized communities often face a higher burden of pollution due to factors such as proximity to industrial areas, highways, or other pollution sources. Air pollution exacerbates existing socio-economic disparities by compromising the quality of life, limiting economic opportunities, and perpetuating health inequalities. By conducting air pollution analysis and incorporating spatial interpolation techniques, decision-makers and researchers can identify and address these social implications, striving for equitable policies and interventions to protect vulnerable populations.

A challenge with environmental measurements is that there can only be a finite number of locations where you can take measurements. In the case of air pollution, high quality measurement stations are very expensive, and require regular maintenance. Spatial interpolation techniques, facilitated by GIS, play a vital role in air pollution analysis. Interpolation enables the estimation of pollutant concentrations at locations where direct measurements are unavailable or sparse. By utilizing mathematical models and available data points, spatial interpolation creates continuous surfaces or maps of pollutant concentrations, providing a comprehensive view of air pollution patterns across the study area. This spatially interpolated information helps identify pollution hotspots, characterize exposure gradients, and assess the spatial distribution of health risks associated with air pollution.

Air pollution analysis using spatial interpolation techniques is essential for understanding the spatial distribution and impact of pollutants on human health and society. By leveraging GIS and interpolation methods, researchers and policymakers can identify pollution hotspots, assess health risks, and promote environmental justice. These analyses contribute to evidence-based decision-making, enabling the development of targeted interventions and policies to mitigate air pollution's adverse effects on both individuals and communities.

## OBJECTIVES

In this tutorial, we will learn about spatial interpolation using QGIS. We will work with point data from air pollution monitoring stations in Helsinki to estimate air pollution levels in locations where there are no stations available. We will be asking ourselves the following research questions:

- What are the concentrations of nitrogen dioxide (NO2) air pollution in the Helsinki region?
- Do any areas exceed the EU recommended limit?
- What is the concentration in the wealthiest postcode? And the least wealthy?

## Completion

Complete the exercise and submit a short report containing at least the following:

1. Maps of the outcome of the exercise

	- Remember: all maps should have a legend, a scale bar and a north arrow

	- Use the tips from the Crash Course to improve your map

2. A short reflection (200-300 words) on this exercise and your map

## EXERCISE PHASES

### Part 1 - Loading data

1. Download 7_Exercise_data.zip from the [Github repository](https://github.com/Tampere-University-Urban-Physics/fundamentals-of-gis/tree/master/Data), save it in a folder for this exercise, and extract the contents from the zip.  We have data on the location of air pollution monitoring sites in the Helsinki region.

2. Load the file *Air_pollution_monitoring_points.shp* into your QGIS project.

3. Examine the attribute table of the layer you added, what do you see?
	-  We can see that each point has some data associated with it, including the nitrogen dioxide concentration (NO2), the year of the measurement (Vuosi), the type of measurement station (Tyyppi), the location, and more.

4. Consider if it would be helpful to have some context during this analysis, maybe add aerial imagery or some other basemap for now? 

5. Let’s also add some postcode boundaries. Add the layer *Postcodes_air_pollution.shp* to the QGIS project.

6. Let’s change the symbology of the postcode boundaries to make it so that you can see the basemap beneath them.
	- Change the symbology in such a way that you can see your basemap under it.

7. Let’s also change the symbology of the monitoring stations
	- Let’s make these a graduated symbol. Choose NO2 as the *Value*.  You will now see a map with the monitoring stations colored according to the monitored NO~2~ levels. Does anywhere exceed the 2008 EU Air Quality Directive limit of an annual average of 40 micrograms per cubic metre? The new EU Air Quality Directive has targets to meet 20 micrograms per cubic metre. Does anywhere exceed this? The World Health Organisation has a target limit of 10 micrograms per cubic metre. Does anywhere exceed this value?

---

### Part 2 - Spatial Interpolation

8. Now that we have our air monitoring station data loaded, we can see their locations and have an idea of what the annual average NO2 levels recorded at these stations are. But what about where there are no monitoring stations? Can we estimate what the concentrations are in other areas, based on the closest stations? GIS has the ability to interpolate between these data points to estimate concentrations elsewhere.

9. Let’s interpolate NO2 concentration between the monitoring stations. 
	-   Open the *IDW Interpolation tool*, in the *IDW Interpolation tool*, set:
		- *Vector Layer*: Air_pollution_monitoring_points, this tells QGIS that you want to interpolate from this vector dataset.
		- *Interpolation Attribute*: NO2, the value associated with the monitoring stations that we want to interpolate.
		- Click the + symbol to add it to the list of items to interpolate
		- *Extent*: click the down arrow to the right of the extent box and choose *Calculate from Layer* > Air_pollution_monitoring_points. This will limit the interpolation to the areas covered by the monitoring stations – where we have data. Otherwise, the tool may try to interpolate over a wider area, and could lead to errors when it tries to calculate concentrations where we do not have data.
		- *Output Raster Size* will define the spatial resolution of the raster the interpolation produces – meaning the size of the grid (or pixels). Too small a grid size, and you create a very large file and the results could be misleading (i.e. it could make people think we know the pollution in a 1m2 area, which we don’t). Too large, and we won’t get a good idea of the variation because the grid will be so large. We don’t have many monitoring stations, and it is computationally more difficult to produce a large raster, so let’s make it quite coarse. Set the X and Y pixel size as 50 for both. What kind of unit do you think this is?
	- Run the tool

10. The interpolation has produced a raster from point data. We can see how the IDW algorithm has estimated the concentration of air pollution between the different points as a continuous grid, where each pixel has a value. In reality, the spatial concentration of air pollution is more varying, with differences due to the presence of pollution emission sources, urban canyons, wind, and other factors, and will vary over time as well. But, this gives us an initial estimate of how concentrations change across Helsinki.

11. Change the symbology. Try Singleband psuedocolor. What about the labels? Do we know pollution to this level of precision? The data from the stations had no decimal places, while the interpolated layer has many. **Hint** Reduce the Label precision in the Symbology to the same as the data you used to create the interpolation.

---

### Part 3 - Social factors and exposures

11. Let’s now examine the concentrations in different postcodes in Helsinki. To do this, we can calculate raster statistics like we did in in a previous tutorial. 
	- Open the *Zonal Statistics* tool
	- Under *Input Layer*, select Postcodes_air_pollution. This tells QGIS that you want to calculate statistics within these postcode boundaries (the ‘zones’)
	- Under *Raster Layer*, select your Interpolated air pollution data. This tells QGIS that you want to calculate statistics from this layer.
	- This will create a new vector layer that at first glance looks the same as the postcode layer. However, it now has the summary statistics for the raster joined to each postcode.

12. Open the *Attribute Table* of the new layer. You will see a column on the right that shows the average concentration. There is also a column called tr_mtu, which is the median income of households. By clicking on the column heading, you can sort the data ascending or descending. 
	- What is the postcode with the highest NO2 concentrations? 
	- What is the postcode with the lowest? 
	- What are the concentrations in the postcode with the highest median income? 
	- What is the pollution in the postcodes with the lowest median income (ignoring those with 0)?
	- How many postcodes have an average concentration that exceeds limit values of 40, 20, and 10 micrograms per cubic metre?

13. We can also join the air pollution monitoring stations point data to the postcodes. This can help us identify the number of monitoring stations in postcodes, and estimate the average concentrations based on only the monitoring stations. To do so, we will perform a spatial join.
	- Open the *Join Attributes by Location (Summary)* tool
	- Under *Input*, choose the layer you produced when you calculated the Zonal Statistics 
	- Under *Join Layer*, choose Air_pollution_monitoring_points
	- Select which *Geometric predicate* you think is best (Hint: Theory 4)
	- Under *Fields to summarize*, press the box on the right and choose NO2
	- Under *Summaries to calculate*, press the box on the right and choose *Count* and *Mean*

14. This will have created a new layer, with the default name Joined Layer. Open the attribute table. What post-code had the most NO2 monitors? How many were there?

---

### Map visualization  

15. You’ve now answered your research questions. Make a nice informative map, showing the interpolated air pollution raster, the air pollution monitoring points, and the postcode boundaries. Insert a legend for concentrations, so we know how the air pollution is varying across the city. In your reflection, consider how accurate or realistic you think the interpolation is.
    
<!--stackedit_data:
eyJkaXNjdXNzaW9ucyI6eyJhWmlBa1UxVUNwNW50cDFZIjp7In
RleHQiOiIjIyBPVkVSVklFVyAmIFBVUlBPU0UiLCJzdGFydCI6
MTUzLCJlbmQiOjE3NH0sIjl5Zno3T0NHaFFHcVhsWGEiOnsic3
RhcnQiOjM3ODQsImVuZCI6NDAwNCwidGV4dCI6Ii0gV2hhdCBh
cmUgdGhlIGNvbmNlbnRyYXRpb25zIG9mIG5pdHJvZ2VuIGRpb3
hpZGUgKE5PMikgYWlyIHBvbGx1dGlvbiBpbiB0aGUgSGXigKYi
fSwiek1lS01oaHRHTUlnYkNCZyI6eyJzdGFydCI6NDM5NSwiZW
5kIjo0NzEyLCJ0ZXh0IjoiMS4gRG93bmxvYWQgN19FeGVyY2lz
ZV9kYXRhLnppcCBmcm9tIHRoZSBbR2l0aHViIHJlcG9zaXRvcn
ldKGh0dHBzOi8vZ2l0aHViLmNvbeKApiJ9LCJUT1hIMjFOSmlT
WFFkdDUyIjp7InN0YXJ0Ijo2NTYyLCJlbmQiOjgwNjksInRleH
QiOiI5LiBMZXTigJlzIGludGVycG9sYXRlIE5PMiBjb25jZW50
cmF0aW9uIGJldHdlZW4gdGhlIG1vbml0b3Jpbmcgc3RhdGlvbn
MuIFxuXHQtICAgT+KApiJ9LCJDTFFNaFNJanJ1dzkxWEJmIjp7
InN0YXJ0Ijo5OTM5LCJlbmQiOjEwMzQyLCJ0ZXh0IjoiLSBXaG
F0IGlzIHRoZSBwb3N0Y29kZSB3aXRoIHRoZSBoaWdoZXN0IE5P
MiBjb25jZW50cmF0aW9ucz8gXG5cdC0gV2hhdCBpcyB0aGUgcG
9z4oCmIn0sIm81WFl2Y09iWnlRNlgyWHoiOnsic3RhcnQiOjEx
MDc2LCJlbmQiOjExMjQwLCJ0ZXh0IjoiMTQuIFRoaXMgd2lsbC
BoYXZlIGNyZWF0ZWQgYSBuZXcgbGF5ZXIsIHdpdGggdGhlIGRl
ZmF1bHQgbmFtZSBKb2luZWQgTGF5ZXIuIE9wZeKApiJ9fSwiY2
9tbWVudHMiOnsiSkZlMUpqWGI1RHFtbHhlMCI6eyJkaXNjdXNz
aW9uSWQiOiJhWmlBa1UxVUNwNW50cDFZIiwic3ViIjoiZ2g6ND
AzMDQ3ODgiLCJ0ZXh0IjoiQWRkIHBpY3R1cmUiLCJjcmVhdGVk
IjoxNjg3ODQ0Nzk5MjY4fSwic0ZxTDZ0TTFvZjBxblFkRyI6ey
JkaXNjdXNzaW9uSWQiOiJhWmlBa1UxVUNwNW50cDFZIiwic3Vi
IjoiZ2g6NDAzMDQ3ODgiLCJ0ZXh0IjoiQWxzbyBtb3ZlIHRvIH
RoZW9yeT8iLCJjcmVhdGVkIjoxNzk0ODIxMzkyMTU1fSwiaW94
WkJ3bTdkUDQyNkhuSSI6eyJkaXNjdXNzaW9uSWQiOiJhWmlBa1
UxVUNwNW50cDFZIiwic3ViIjoiZ2g6NDAzMDQ3ODgiLCJ0ZXh0
IjoiQWxzbywgc28gbG9uZywgbWFrZSB0aGlzIG1vcmUgY29uY2
lzZSIsImNyZWF0ZWQiOjE3OTQ4MjE0ODY0MTZ9LCI5TVVjMTNt
RmczVlBBZGR1Ijp7ImRpc2N1c3Npb25JZCI6Ijl5Zno3T0NHaF
FHcVhsWGEiLCJzdWIiOiJnaDo0MDMwNDc4OCIsInRleHQiOiJB
ZGQgc2VjdGlvbiBpbiBtb29kbGUgdG8gZmlsbCB0aGVzZSBvdX
QiLCJjcmVhdGVkIjoxNzk0ODIxNTMxMDc0fSwiUVFuUjFncHpZ
MjNObFFheCI6eyJkaXNjdXNzaW9uSWQiOiJ6TWVLTWhodEdNSW
diQ0JnIiwic3ViIjoiZ2g6NDAzMDQ3ODgiLCJ0ZXh0IjoiQWRk
IGRhdGEgc2VjdGlvbiIsImNyZWF0ZWQiOjE3OTQ4MjE1NDQ2ND
J9LCI4ejZYeEJjZHdoSUg5QUg4Ijp7ImRpc2N1c3Npb25JZCI6
IlRPWEgyMU5KaVNYUWR0NTIiLCJzdWIiOiJnaDo0MDMwNDc4OC
IsInRleHQiOiJUcnkgdG8gbWFrZSB0aGlzIG1vcmUgY29uY2lz
ZSIsImNyZWF0ZWQiOjE3OTQ4MjI0MjI5NDJ9LCJsRmNpU2xSd2
JEQ3ZqZkRhIjp7ImRpc2N1c3Npb25JZCI6IkNMUU1oU0lqcnV3
OTFYQmYiLCJzdWIiOiJnaDo0MDMwNDc4OCIsInRleHQiOiJBZG
Qgc2VjdGlvbiBpbiBtb29kbGUgdG8gYW5zd2VyIHRoZXNlIiwi
Y3JlYXRlZCI6MTc5NDgyMjU1NzQ4NX0sIkRqcG9NR2RFQlN0UW
dWNmUiOnsiZGlzY3Vzc2lvbklkIjoibzVYWXZjT2JaeVE2WDJY
eiIsInN1YiI6ImdoOjQwMzA0Nzg4IiwidGV4dCI6IkFkZCBzZW
N0aW9uIGluIG1vb2RsZSB0byBhbnN3ZXIgdGhlc2UuIiwiY3Jl
YXRlZCI6MTc5NDgyMjYxMTEwN319LCJoaXN0b3J5IjpbLTY3NT
AzMTUxMiwtMTk0OTUzMjUyOSw5NzA3NDQxMjEsLTk0MTY5MzY1
OSwyMDc2Njc4NDUxXX0=
-->