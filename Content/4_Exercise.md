### Fundamentals of Geographic Information Systems (GIS)

# Exercise 4: 

## OVERVIEW & PURPOSE

The Ring Rail Line, also known as “Kehärata”, is a railway line that was opened in 2015 in the Helsinki capital region. The new rail route created a connection between Helsinki-Vantaa Airport and the Helsinki region commuter traffic network by joining the existing Vantaankoski and Main line branches. The approximately 18-kilometer-long Ring Rail line connects to the current rail network at the Vantaankoski and Hiekkaharju stations and the track between Viinikkala and Ruskeasanta goes in an underground tunnel.

For the 2015 opening, altogether five new stations were constructed at Leinelä, Helsinki-Vantaa Airport, Aviapolis, Kivistö and Vehkala. Reservations have also been made for four additional stations at Petas, Viinikkala, Ruskeasanta and Lapinkylä to be built later in the second phase. Out of these, the Aviapolis and Airport stations are located underground. Similarly, the reserved stations of Ruskeasanta and Viinikkala will be located underground.

The Ring Rail line improves the accessibility of the airport area and the suburbs along the route, cut down the journey times for over 12 million airport passengers per annum and it is also estimated to create employment for over 60 000 people.

![enter image description here](https://vayla.fi/documents/25230764/35593193/Keh%C3%A4rata_kartta.jpg/a9017019-18d8-4a4d-a601-88b08a32fe87?t=1451313857883)
*Legend in English: Pintarata = Overground, Tunneli = tunnel, Asema = station, Asemavaraus = station reservation, Nykyiset asemat = Current stations, Hämeenlinnanväylän parannus = improvement of Hämeenlinnanväylä*

## OBJECTIVES
This exercise consists of three main objectives:
- Gather and prepare data of the ring rail line for processing
- Use the gathered ring rail line data and provided data to answer questions about the impact of the ring rail line and the airport on the surrounding residents
- Find an optimal location for a new Airport Hotel & Congress Center to the Helsinki-Vantaa Airport area based on accessibility

To complete these you will put the following main skills to use:
- Data gathering and processing
- Buffer analysis
- Overlay analysis

## DATA USED
- Capital region people explanation 

## Completion

Work in pairs or individually. Complete the exercise and submit a short report containing at least the following:

1. Maps of the outcome of the exercise

	- Remember: all maps should have a legend, a scale bar and a north arrow

	- Use the tips from the Crash Course to improve your map

2. A short reflection (200-300 words) on this exercise and your map

## EXERCISE PHASES

### Part 1: The ring rail line visualization
#### 1.1: Getting the data
1. Download 4_Exercise_data from the [Github repository](https://github.com/rowan8k/fundamentals-of-gis/tree/master/Data), save it in a folder for this exercise, and add it to your QGIS project. 
	- Go through the data, what does it describe? What order of layers would be good to work with?

1. Go to Moodle and download the data necessary for the task.

32. We need to have the Ring Rail Line in geospatial format. One option is to digitize it based on a basic map. Other is to find the data online – let’s do that for practice!
	- HRI offers easy access to open data sources between the cities of Helsinki, Espoo, Vantaa and Kauniainen. Follow the link to Helsinki-Region Transport’s public transport lines from 2021-2022 and open the preview in HSL’s website. https://public-transport-hslhrt.opendata.arcgis.comhri.fi/data/en_GB/datasets/hsl-n-linjat-4/

- Picture 

	- As you can see, the dataset is big and we don’t need everything it contains. You could download the whole data and edit it on your computer, but you can also filter the data before downloading it. You’d have to figure out what different attributes mean – in this case the information is in Finnish, so needed code explanations are provided here. You can also check them also through the link below and clicking “Aineiston kuvaus (pdf)”.
		- https://public-transport-hslhrt.opendata.arcgis.com/datasets/HSLHRT::hsln-linjat-4/about
	- You can select features by clicking “Filter data” on the left of the page.
		- JL_LAJI: 12, fright.
		- Filters by local VR traffic
		- VERKKO: 4, filters on train traffic
		- AIK_VOIM: 1, filters on in-use tracks
		- SUUNTA: 1, filter on 1 direction
		- You will notice when you filter on these that the visual output barely changes, we are filtering to get as little duplicate lines as possible so we have to do as little manual adjustments ourselves. For this purpose we are only looking to extract the rough location of the line, and thus for example a single direction is sufficient. 
	- When the selection is done, you can download it by going to the Download tab under the Filter data tabcolumn jl_laji (has information on public transport type (bus, tram, …)
		- Select column value 12 from jl_laji, 12 = VR local trains
	- When the selection is done, you can download it. Click the “toggle filters” to choose only the selected features, and download .

- Picture 

3. If you’d rather data as a Shapefile. 

 ![](https://raw.githubusercontent.com/rowan8k/fundamentals-of-gis/master/Assets/4_Exercise/4_Exercise_data_selection.png)

3. Add the data you downloaded to your QGIS project and inspect the features, what do you notice? 
	- There are still duplicate entries for the Ring Rail Line and there are other entries we do not need and are in the way
	- We want to end up with only the Ring Rail Line, go through the attribute table and find which entry describes the full ring Rail Line, export (Exercise 3, step 4) this to a new layer, you can remove the original.

#### 1.2: Making a map visualization of the new ring rail line
4. For visualization purposes we want to have underground part of the rail as a separate segment. You can do this by splitting the ring rail line feature you exported into multiple features. 
	- We can use Google Satellite imagery (Hint: Crash Course) and the overview map of the ring rail line to find the places where we need to split the ring rail line
		- You might want to change the symbology of your ring rail line to make it more visible for editing
	- Select your Ring Rail Line layer, toggle on editing, and navigate to Edit > Edit Geometry > Split features
		- How to Split and Merge Lines in QGIS: https://www.youtube.com/watch?v=vLL4hdDO1zU
	- Split the line into sections for overground and underground track
		- Create a field which describes whether the track is over- or underground so we can use symbology to differentiate these (Attribute table > New Field)
	- Don't forget to save your edits and toggle off editing

5. Now look at the ring_rail_line_stations and railway_stations layers. For the visualization we would only like to have the stations that are part of the ring rail line, and have them seperated by pre-existing, new, and planned stations. 
	- Let's start by removing the stations that are not part of the ring rail line. Toggle on editing for the railway_stations layer, select the stations we want to remove, and use *Delete Selected* in the editing toolbar (Hint: Exercise 2, step 10) (Don't forget to save edits and toggle off editing).
	- Now take a look at the attribute table of the ring_rail_line_stations layer, as you can see there is already a field for which stations are new or planned, so we can use symbology to differentiate these. 
	- Optionally: Convert the train stations from polygons to points: https://docs.qgis.org/3.16/en/docs/user_manual/processing_algs/qgis/vectorgeometry.html#centroids
		- In this scase, points scale better, are easier to edit, and easier to add labels to

6. Make a visualization to show the ring rail line. Show on your map: where the railway is underground, which stations are new and whatever else you find convenient. 
	- Use categorized symobology to show the different types of railway and stations
	- Don't forget to add a basemap for reference (hint: Use one that contrasts with the data)
	- Optionally you can add labels to the stations either by digitizing or editing one of the ring rail line features (there’s multiple features with same information). The dataset still contains some extra features, you can keep them for visualization or delete them, as you wish. You can see the route in the Ring Rail Line Brochure PDF (see Moodle).

---

### Part 2: The ring rail line analysis
#### 2.1: Analyzing the effects on the people in the study area
As already mentioned, the Ring Rail line improves the commuter traffic network and accessibility of the suburbs along the track, but how does this show on real numbers?

5. Find out how many are notably affected by this by calculating how many people live within a radius of 1 kilometer from the stations opened in 2015? And the new and planned stations combined?
	- You can do this by doing a buffer analysis using the ring_rail_line_stations and capital_region_people data
	- Open the *Buffer* tool from the *Processing Toolbox*
	- Make a 1km buffer around the ring_rail_line_stations (input: ring_rail_line_stations, distance: 1000)
		- To only make a buffer around the stations opened in 2015, select those stations and use the *Selected features only* function in the *Buffer* tool
	- Don't forget to make the temporary layers from the buffer permanent
	- Use *Select by Location* to select the entries from the capital_region_people data that intersect with the buffers
	- Run *Basic Statistics for Fields* to get the total amount of residents living within the buffer (Input layer: Capital_region_people, selected feature only, Field to calculate: ASYHT(Persons in total in the building))
		- Look the output log for the SUM output, this shows the total amount
	- Repeat the same steps for the other buffer500 meters and 1 kilometer from
	- ...the stations opened in 2015?
	- ...the new and planned stations combined?

The Ring Rail Line is likely to increase the attraction for the real estate in the nearby area. This, however, will also raise the question of the noise levels caused by the Aircraft traffic around the Helsinki-Vantaa Airport. Thanks to new improved technology the aircraft noise levels have slightly dropped in recent years.

6. Find out how many people approximately live inside the dB zones: 65 or more and 50 or more in Helsinki region based on :
	- ...the noise levels in 2006? And 
	- ...the noise levels in 2010?
	- This time you don't need to create a buffer, as the dB zones already exist
	- Repeat the same steps as in step 5 to get the number of residents living within the 65 and 50 dB zones in 2006 and 2010
		- Hint: Look at the attribute tables of the noise layers (use *Selected features only* in *Select by location*)

Fill in the answers to the Moodle

*Tip : For the Capital_region_people layer field name metadata, see Appendix in Exercise 4 instructions*

#### 2.2: Planning a new airport hotel & congress center near the ring rail line
7. You work in a team that is supposed to find an optimal location for a new Airport Hotel & Congress Center to the Helsinki-Vantaa Airport area based on accessibility. Your task is to follow the given requirements and make a map visualization of the area(s) to be shown at the company meeting of where the new Hotel & Congress center could possibly be located.

| **The requirements are as follows** |
|--|
|:
- The Hotel & Congress Center has to be located within the maximum **distance of 2 kilometers from the Airport area** (but not inside it) |
|.
- The Hotel & Congress Center has to be easily accessible, thus located **within the radius of 1 kilometer from the new Ring Rail Line stations** (including the reserved ones) |
|
- It has to be **within the radius of 500 meters from the road network**. |
|
- The Hotel & Congress Center has to be **outside the ≥ 55 dB noise zones** caused by the airport traffic. |
|
- The Hotel & Congress Center **cannot be built on already built up areas**. |
|
- The Hotel & Congress Center requires **at least 5000 m2 of free building space**. |

- Start by creating buffers around the airport area, ring rail line stations, and road network, setting the distance based on the requirements
	- You can disolve the result of the buffer to make future steps easier, 
- Use the *Intersection*  tool to combine intersect these buffers to get the areas which are only within 2km of the airport area, within a 1km radius from the stations, and within 500m from the road network. 
	- You have to run this intersect with two buffers first, then run the output of that with the final buffer to get the desired result

*Hint 1: The Multipart to Singlepart- geoprocessing tool can be used to detach the polygons individual features to be able to calculate their areas. This is useful when the overlay operations combine the features, but because their topology information is still stored, they can be separated using this tool.*

*Hint 2: When creating a buffer for the roads, don't forget to make your outputs permanent in between processing
- Now we need to remove the areas that are not suitable from this layer, namely the airport area itself, the stations themselves, the roads themselves, the 55 dB noise zone, and the build up areas. 
	- We can do this using the *Difference* tool, read its description, what does it do? In short, it outputs the parts of the input layer that fall outside the overlay layer. 
	- So we need to run this for all our areas that we do not want to build the hotel (similar to last time we ran *Difference*) and then run it again with its output and the next criteria
		- Use *Selected features only* again where necessary
- Once we have the final suitable areas we need to determine which have at least 5000m2 of free building space
	- Look a include ones near the airport area. A buffer or “Select by location” -tool’s buffering parameter can be helpful. Creating a buffer for all of the roads is unnecessary and takes a lot of time to process.*

8. Try to make the map as clear and informative as possible and insert the finished map with all the necessary map elements to your current layer and its features. Could we select the areas which have at least 5000m2 free space right now? Why not? 
	- Some features that are not physically connected are still 1 feature, so we need to seperate them. 
		- Do this by running the *Promote to Multipart* tool on your current layer, this will seperate the features into seperate parts
	- Let's extract areas of at least 5000m2 using the *Extract by Expression* tool (Hint: Crash Course, step 7)
- Done! You should now have the areas that meet all the criteria

8. Make your final map of the suitable areas for the new airport hotel & congress center

se report. Write also a short reflection on what was done and why.




<!--stackedit_data:
eyJkaXNjdXNzaW9ucyI6eyJWVmhGd2NhNFBxWU9FdERCIjp7In
RleHQiOiIyLjE6IEFuYWx5emluZyB0aGUgZWZmZWN0cyBvbiB0
aGUgcGVvcGxlIGluIHRoZSBzdHVkeSBhcmVhIiwic3RhcnQiOj
g0NzksImVuZCI6ODUzN30sIkR4WTl1c1A1SzFhZ2xZNTkiOnsi
dGV4dCI6IkZpbGwgaW4gdGhlIGFuc3dlcnMgdG8gdGhlIE1vb2
RsZSIsInN0YXJ0IjoxMDgwMCwiZW5kIjoxMDgzM30sIjBPdk03
UTdXUEZEWmRrRmMiOnsidGV4dCI6IiMjIyMgMi4yOiBQbGFubm
luZyBhIG5ldyBhaXJwb3J0IGhvdGVsICYgY29uZ3Jlc3MgY2Vu
dGVyIG5lYXIgdGhlIHJpbmcgcmFpbCBsaW5lIiwic3RhcnQiOj
EwOTQxLCJlbmQiOjExMDIxfSwiWWJDdlI0ZExOMGZvRllNbSI6
eyJ0ZXh0IjoiIyMgREFUQSBVU0VEIiwic3RhcnQiOjIyMzQsIm
VuZCI6MjI0Nn0sIjJ4T1hZa3hYWkhhNmQzTVQiOnsidGV4dCI6
IiMgRXhlcmNpc2UgNDoiLCJzdGFydCI6NTgsImVuZCI6NzF9LC
JFdFByYk5zWUdNWWhPWXRSIjp7InRleHQiOiJNb29kbGUiLCJz
dGFydCI6MzA1MiwiZW5kIjozMDU4fSwicjFuaW14MW1CdWx6YV
hkViI6eyJ0ZXh0IjoiLSBQaWN0dXJlIiwic3RhcnQiOjM2MDks
ImVuZCI6MzYxOH0sIkNVTHg0QmVWRFZzMm5GSUEiOnsidGV4dC
I6Ii0gUGljdHVyZSIsInN0YXJ0Ijo1MTE5LCJlbmQiOjUxMjh9
LCIyUUhBeUpkVTdsVjlDY05zIjp7InRleHQiOiJpZ2l0aXplLC
B1c2UgdGhlIFRvcG9ncmFwaGljIG1hcCByYXN0ZXIgMTogMTAw
IDAwMCBhdmFpbGFibGUgZnJvbSBQYUlUdWxpIGFuZCB04oCmIi
wic3RhcnQiOjU3MzMsImVuZCI6NTczM30sImdEeVJTN25WRGFX
QWZOd0siOnsidGV4dCI6Ik1lcmdlIiwic3RhcnQiOjYzODcsIm
VuZCI6NjM5Mn0sIlBoc1NESXFQaUw3M2x4RVkiOnsidGV4dCI6
Im9vayBhdGx5IGluIiwic3RhcnQiOjEzODI3LCJlbmQiOjEzOD
M1fSwiYnNtR0FNdnNHNHJtSVF1ZyI6eyJ0ZXh0Ijoic2UiLCJz
dGFydCI6MTQ3ODUsImVuZCI6MTQ3ODd9fSwiY29tbWVudHMiOn
siR1VNbUNUSmhLeGs0UFNpTiI6eyJkaXNjdXNzaW9uSWQiOiJW
VmhGd2NhNFBxWU9FdERCIiwic3ViIjoiZ2g6NDAzMDQ3ODgiLC
J0ZXh0IjoiQWRkIHNlY3Rpb24gaW4gbW9vZGxlIHdoZXJlIHRv
IGZpbGwgaW4gYW5zd2VycyBmb3IgdGhlc2UiLCJjcmVhdGVkIj
oxNjg2ODk5MjQ0OTk3fSwiZEtCY0gxZ0dncjJsQXc3cyI6eyJk
aXNjdXNzaW9uSWQiOiJEeFk5dXNQNUsxYWdsWTU5Iiwic3ViIj
oiZ2g6NDAzMDQ3ODgiLCJ0ZXh0IjoiRml4IHJlZmVyZW5jZSIs
ImNyZWF0ZWQiOjE2ODY5ODY0MjkxMDV9LCI2NlNwdEp3Skc4T3
RWcGtiIjp7ImRpc2N1c3Npb25JZCI6IjBPdk03UTdXUEZEWmRr
RmMiLCJzdWIiOiJnaDo0MDMwNDc4OCIsInRleHQiOiJPcHRpb2
5hbGx5IChyZSltb3ZlIHRoaXMgaWYgdG9vIGhhcmQiLCJjcmVh
dGVkIjoxNjg2OTg4NzQ3MzY5fSwiNWJGb0NaWkpSRE9EaHpEWS
I6eyJkaXNjdXNzaW9uSWQiOiJZYkN2UjRkTE4wZm9GWU1tIiwi
c3ViIjoiZ2g6NDAzMDQ3ODgiLCJ0ZXh0IjoiV2hhdCBhcmUgdG
hlIGRhdGEgc291cmNlcz8iLCJjcmVhdGVkIjoxNjg3MTU4MDU4
NzEzfSwiVGhWbEpvT0dkZWZVbjRORCI6eyJkaXNjdXNzaW9uSW
QiOiIyeE9YWWt4WFpIYTZkM01UIiwic3ViIjoiZ2g6NDAzMDQ3
ODgiLCJ0ZXh0IjoiQWRkIHRpdGxlIiwiY3JlYXRlZCI6MTY4Nz
E3MDQyODQwNn0sIkh1MW9sN0dpRjZoQnQzY0kiOnsiZGlzY3Vz
c2lvbklkIjoiRXRQcmJOc1lHTVloT1l0UiIsInN1YiI6ImdoOj
QwMzA0Nzg4IiwidGV4dCI6IkNvcnJlY3QiLCJjcmVhdGVkIjox
Njg2ODk4NDA2ODEyfSwiSXVHN2hOYU9pNFJJYWZLZSI6eyJkaX
NjdXNzaW9uSWQiOiJyMW5pbXgxbUJ1bHphWGRWIiwic3ViIjoi
Z2g6NDAzMDQ3ODgiLCJ0ZXh0IjoiQWRkIHBpY3R1cmUiLCJjcm
VhdGVkIjoxNjg2ODk4NzU1OTQwfSwiV0hPYWxHeDVoeXZFSjhD
RSI6eyJkaXNjdXNzaW9uSWQiOiJDVUx4NEJlVkRWczJuRklBIi
wic3ViIjoiZ2g6NDAzMDQ3ODgiLCJ0ZXh0IjoiQWRkIHBpY3R1
cmUiLCJjcmVhdGVkIjoxNjg2ODk4Nzg3ODA1fSwicEs5aWFGNT
VJaTRQeUk3ZCI6eyJkaXNjdXNzaW9uSWQiOiIyUUhBeUpkVTds
VjlDY05zIiwic3ViIjoiZ2g6MjIxNjgxNTciLCJ0ZXh0IjoidG
hpcyBpcyBhIGxpdHRsZSB1bmNsZWFyLiIsImNyZWF0ZWQiOjE2
ODcyNTQ1NzI4ODJ9LCJFV3h5ZEhicGxPYXZLaWU1Ijp7ImRpc2
N1c3Npb25JZCI6ImdEeVJTN25WRGFXQWZOd0siLCJzdWIiOiJn
aDoyMjE2ODE1NyIsInRleHQiOiJwcm9iYWJseSBiZXR0ZXIgdG
8gZ2l2ZSBpbnN0cnVjdGlvbnMgaGVyZSIsImNyZWF0ZWQiOjE2
ODcyNTQ2NDc0OTh9LCJubFBEQVdUdThieFc2UENtIjp7ImRpc2
N1c3Npb25JZCI6IlBoc1NESXFQaUw3M2x4RVkiLCJzdWIiOiJn
aDoyMjE2ODE1NyIsInRleHQiOiI/IiwiY3JlYXRlZCI6MTY4Nz
I1NTAzOTM2M30sIlJUUGhmMzVWQWRlckFRRTAiOnsiZGlzY3Vz
c2lvbklkIjoiYnNtR0FNdnNHNHJtSVF1ZyIsInN1YiI6ImdoOj
IyMTY4MTU3IiwidGV4dCI6Ij8iLCJjcmVhdGVkIjoxNjg3MjU1
MDk2ODgyfX0sImhpc3RvcnkiOlstMTQwMjQyMTcyMSwtNDI2NT
k0MzMyLC0zNDg1MTQ5NjAsNzkzMjkyMjMzLC05OTIwNjQ5MDIs
LTE1MTI3MDY4MiwxMDQ4MzgwNDAxLC0xODEzNjEyODQxLDMyMD
c1NzI4OSwxMzE5NTA0MjM1LDEyMzgxODQ5ODYsMTQyOTQ1MTQz
OCwxMjEyNTk1ODM2LC0xMzI3NzM1MTUyLC0yNzYwNzIxMTUsLT
MwMjQ2NTIxMiwtMTkyOTUyNjA2MSwyMTAxOTQzODUyLDcyNDYx
NzkwLC0yMDczMzkzMTAzXX0=
-->