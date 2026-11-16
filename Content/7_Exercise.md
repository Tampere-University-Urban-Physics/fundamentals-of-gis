
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
	- Change the symbology in such a way that you can see your basemap under it (e.g. remove the fill or change the transparency)  

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
	- Press *OK*. This will create a new vector layer that at first glance looks the same as the postcode layer. However, it now has the summary statistics for the raster joined to each postcode.

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
eyJkaXNjdXNzaW9ucyI6eyI4SGdnbURhVlAyTExsdjFCIjp7In
RleHQiOiJEb2VzIGFueXdoZXJlIGV4Y2VlZCB0aGUgRVUgQWly
IFF1YWxpdHkgRGlyZWN0aXZlIGxpbWl0IG9mIGFuIGFubnVhbC
BhdmVyYWdlIG9m4oCmIiwic3RhcnQiOjU1NzMsImVuZCI6NTY4
M30sIllTOXhHeVZoblZGR3VveXQiOnsidGV4dCI6Ii0gV2hhdC
BpcyB0aGUgcG9zdGNvZGUgd2l0aCB0aGUgaGlnaGVzdCBOTzIg
Y29uY2VudHJhdGlvbnM/IFxuXHQtIFdoYXQgaXMgdGhlIHBvc+
KApiIsInN0YXJ0Ijo5MTg0LCJlbmQiOjk0NjN9LCJySGx2ZlF5
N0hwcDA4S1V2Ijp7InRleHQiOiJXaGF0IHBvc3QtY29kZSBoYW
QgdGhlIG1vc3QgTk8yIG1vbml0b3JzPyBIb3cgbWFueSB3ZXJl
IHRoZXJlPyIsInN0YXJ0IjoxMDMwMCwiZW5kIjoxMDM2Mn0sIm
FaaUFrVTFVQ3A1bnRwMVkiOnsidGV4dCI6IiMjIE9WRVJWSUVX
ICYgUFVSUE9TRSIsInN0YXJ0IjoxNTMsImVuZCI6MTc0fSwiaG
pENmhhakdmSVE2M1hFbiI6eyJ0ZXh0IjoiMTMuIFdlIGNhbiBh
bHNvIGpvaW4gdGhlIGFpciBwb2xsdXRpb24gbW9uaXRvcmluZy
BzdGF0aW9ucyBwb2ludCBkYXRhIHRvIHRoZSBwb+KApiIsInN0
YXJ0Ijo5NDY2LCJlbmQiOjEwMTk2fSwiVXJ2SFJTYmxqeTlzSF
F4QyI6eyJ0ZXh0IjoiMTEuIExldOKAmXMgbm93IGV4YW1pbmUg
dGhlIGNvbmNlbnRyYXRpb25zIGluIGRpZmZlcmVudCBwb3N0Y2
9kZXMgaW4gSGVsc2lua2kuIFRv4oCmIiwic3RhcnQiOjgyMDYs
ImVuZCI6ODg5Nn0sIjl5Zno3T0NHaFFHcVhsWGEiOnsic3Rhcn
QiOjM3ODQsImVuZCI6NDAwNCwidGV4dCI6Ii0gV2hhdCBhcmUg
dGhlIGNvbmNlbnRyYXRpb25zIG9mIG5pdHJvZ2VuIGRpb3hpZG
UgKE5PMikgYWlyIHBvbGx1dGlvbiBpbiB0aGUgSGXigKYifSwi
ek1lS01oaHRHTUlnYkNCZyI6eyJzdGFydCI6NDM5NSwiZW5kIj
o0NzEyLCJ0ZXh0IjoiMS4gRG93bmxvYWQgN19FeGVyY2lzZV9k
YXRhLnppcCBmcm9tIHRoZSBbR2l0aHViIHJlcG9zaXRvcnldKG
h0dHBzOi8vZ2l0aHViLmNvbeKApiJ9fSwiY29tbWVudHMiOnsi
VlNhY2hCWUN1MlZid2ZJaCI6eyJkaXNjdXNzaW9uSWQiOiI4SG
dnbURhVlAyTExsdjFCIiwic3ViIjoiZ2g6NDAzMDQ3ODgiLCJ0
ZXh0IjoiQWRkIHNlY3Rpb24gaW4gbW9vZGxlIHRvIGZpbGwgdG
hlc2Ugb3V0IiwiY3JlYXRlZCI6MTY4Nzc2OTAxMTE1MH0sImIy
U25KeUNZdGx6bWVLb2siOnsiZGlzY3Vzc2lvbklkIjoiWVM5eE
d5VmhuVkZHdW95dCIsInN1YiI6ImdoOjQwMzA0Nzg4IiwidGV4
dCI6IkFkZCBzZWN0aW9uIGluIG1vb2RsZSB0byBmaWxsIHRoZX
NlIG91dCIsImNyZWF0ZWQiOjE2ODc3Njk4OTIxNTd9LCJZMXBh
ck9XUml5Y0tEaWlCIjp7ImRpc2N1c3Npb25JZCI6InJIbHZmUX
k3SHBwMDhLVXYiLCJzdWIiOiJnaDo0MDMwNDc4OCIsInRleHQi
OiJBZGQgc2VjdGlvbiBpbiBNb29kbGUgdG8gZmlsbCB0aGlzIG
91dCIsImNyZWF0ZWQiOjE2ODc3NzAxMTg3NTd9LCJKRmUxSmpY
YjVEcW1seGUwIjp7ImRpc2N1c3Npb25JZCI6ImFaaUFrVTFVQ3
A1bnRwMVkiLCJzdWIiOiJnaDo0MDMwNDc4OCIsInRleHQiOiJB
ZGQgcGljdHVyZSIsImNyZWF0ZWQiOjE2ODc4NDQ3OTkyNjh9LC
JaVkVUdHlLYTYwUjFvbmlmIjp7ImRpc2N1c3Npb25JZCI6Imhq
RDZoYWpHZklRNjNYRW4iLCJzdWIiOiJnaDo0MDMwNDc4OCIsIn
RleHQiOiJSZW1vdmUgaW5zdHJ1Y3Rpb25zIHRvIGNoYWxsZW5n
ZSBzdHVkZW50cz8iLCJjcmVhdGVkIjoxNjg3OTM1MjgyNzk3fS
wicDlQWW1sNE9OdG5Ya3lUNSI6eyJkaXNjdXNzaW9uSWQiOiJV
cnZIUlNibGp5OXNIUXhDIiwic3ViIjoiZ2g6NDAzMDQ3ODgiLC
J0ZXh0IjoiUmVtb3ZlIGluc3RydWN0aW9ucyB0byBjaGFsbGVu
Z2Ugc3R1ZGVudHM/IiwiY3JlYXRlZCI6MTY4NzkzNTI4NjgxMn
0sInNGcUw2dE0xb2YwcW5RZEciOnsiZGlzY3Vzc2lvbklkIjoi
YVppQWtVMVVDcDVudHAxWSIsInN1YiI6ImdoOjQwMzA0Nzg4Ii
widGV4dCI6IkFsc28gbW92ZSB0byB0aGVvcnk/IiwiY3JlYXRl
ZCI6MTc5NDgyMTM5MjE1NX0sImlveFpCd203ZFA0MjZIbkkiOn
siZGlzY3Vzc2lvbklkIjoiYVppQWtVMVVDcDVudHAxWSIsInN1
YiI6ImdoOjQwMzA0Nzg4IiwidGV4dCI6IkFsc28sIHNvIGxvbm
csIG1ha2UgdGhpcyBtb3JlIGNvbmNpc2UiLCJjcmVhdGVkIjox
Nzk0ODIxNDg2NDE2fSwiOU1VYzEzbUZnM1ZQQWRkdSI6eyJkaX
NjdXNzaW9uSWQiOiI5eWZ6N09DR2hRR3FYbFhhIiwic3ViIjoi
Z2g6NDAzMDQ3ODgiLCJ0ZXh0IjoiQWRkIHNlY3Rpb24gaW4gbW
9vZGxlIHRvIGZpbGwgdGhlc2Ugb3V0IiwiY3JlYXRlZCI6MTc5
NDgyMTUzMTA3NH0sIlFRblIxZ3B6WTIzTmxRYXgiOnsiZGlzY3
Vzc2lvbklkIjoiek1lS01oaHRHTUlnYkNCZyIsInN1YiI6Imdo
OjQwMzA0Nzg4IiwidGV4dCI6IkFkZCBkYXRhIHNlY3Rpb24iLC
JjcmVhdGVkIjoxNzk0ODIxNTQ0NjQyfX0sImhpc3RvcnkiOlst
OTQxNjkzNjU5LDIwNzY2Nzg0NTFdfQ==
-->