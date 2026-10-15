### Fundamentals of Geographic Information Systems (GIS)

# Exercise 2: Georeferencing & digitizing

By Jonathon Taylor

*Updated by Rowan van der Kaaden* 

## Overview & purpose
Urban areas undergo continuous development, and maps need to be updated regularly to account for new construction, demolition, and land use changes for example. In this tutorial, the objective is to **edit a GIS layer to show urban development**. In this case, we will be looking at the development of the Nokia Arena in the city centre of Tampere, Finland. 

## Objectives

- Georeference the arena plans
- Edit the buildings layer to remove the removed buildings and add new buildings
- Digitize the bus stops from the arena plans

## Data used

- OpenStreetMap: https://www.openstreetmap.org/
	- A free, open geographic database updated and maintained by a community of volunteers via open collaboration.
	- Used as a source for the buildings, roads, and railways.

- Tampere City Nokia arena plans

## Completion

Complete the exercise and submit at least the following:

1. Maps of the outcome of the exercise

	- Remember: all maps should have a legend, a scale bar and a north arrow

	- Use the tips from the Crash Course to improve your map

2. A short reflection (200-300 words) on this exercise and your map

## Exercise phases
### 1.1. Getting familiar with the data
It is good to be familiar with the area you will be working with in this exercise:
- https://en.wikipedia.org/wiki/Nokia_Arena_(Tampere)
- https://goo.gl/maps/MifgU6b51S6QKacf7

1. Download 2_Exercise_data.zip from the [Github repository](https://github.com/Tampere-University-Urban-Physics/fundamentals-of-gis/tree/master/Data), save it in a folder for this exercise, and extract the contents from the zip. 

2. Open QGIS and load the tampere_buildings.shp, tampere_roads.shp, and tampere_railway.shp into QGIS (See Crash Course exercise 2.1 if you don't remember), the project CRS will change to the CRS of these layers (ETS89/TM35FIN).
	- *TIP: Select all your vector layers at once by arranging the files by file type when you are adding your layers. Click on the file type header > click on the first SHP file in the list > press and hold SHIFT key > click on the last SHP file on the list.*

3. Add Google Satellite imagery:  Double-click the Google Satellite in the browser window. The layer will then show up in the layer panel and in your map. 
	- If the Google Satellite option is not present in your browser: On the browser window, right-click XYZ tiles and choose New Connection. Give it a name (like Google Satellite), and enter the URL for the data: 
https://mt1.google.com/vt/lyrs=s&x={x}&y={y}&z={z}
Press OK



4. Take a moment to examine the data you added now, what is it about? What kind of attributes does the data have?
	- Can you find the location of where the Nokia Arena is constructed? (hint: the coordinates are 328300 east, 6822050 north in the ETS89 / TM35FIN(E,N) coordinate system. Does the arena and new development exist in the tampere_buildings layer? What about in the Google satellite imagery?

We have a vector layer with the buildings in central Tampere, and a vector layer showing the roads – both of which originally came from OpenStreetMap. This data is old – it shows the area before the development of the arena. In addition, we have a raster image file that shows the designs for the Arena, taken from a planning document. Unfortunately, this raster is not spatially referenced – there is no spatial information associated with each raster cell.

So, the tampere_buildings data is old and we need to update it – there are now new buildings, and an old building has been demolished. City mappers have various ways of updating the spatial databases. Some-times, maps are updated by new aerial imagery. Sometimes, they are updated based on building plans submitted to the city. And, sometimes they might be updated based on measurements carried out by surveyors. In this case, we are going to use an image from the city planning department and add it to our buildings layer, as well as remove some buildings that no longer exist.

---

### 1.2. Georeferencing the arena plans 

1. In the downloaded data, there is a file - nokia_areena_plan.tif – that has been saved from a pdf file downloaded off the Tampere City website. It is a raster file, but it doesn’t have any spatial information associated with the grid cells or pixels. That means that QGIS doesn’t know where the raster is – it cannot project it onto a map. Luckily, there are methods for providing this information to QGIS. This process is called Georeferencing, where we assign geographic coordinates to a digital image by identifying and matching recognisable features (or control points) that exist in the spatial and non-spatial data.
2. Let’s **georeference this image**. To open the georeferencer tool:
	- From the main window, choose *Layers* -> *Georeferencer*. This will open up the *Georeferencer* window

	- In the *Georeferencer* window, choose *File* -> *Open Raster*, and choose the nokia_areena_plan.tif from the folder for this exercise. This will load the image into the *Georeferencer* window. Now, we can provide spatial data to ‘reference’ the image!

3. Let’s do at least four reference points. The more locations, the more spread-out, and the more precise the better the georeferencing result will be. To Georeference: 

	- Click an easily identifiable location point on the nokia_areena_plan.tiff in the *Georeferencer* window, such as the corner of an easily recognizable building. You can zoom in and out of the window to get a more precise location.
	
	- After clicking, it will bring up a dialog that says *Enter Map Coordinates*. You can enter them manually if you know what the coordinates of that location should be. But, it is much simpler to select *From Map Canvas*. This makes the *Georeferencer* window disappear (don’t worry).

	- Next, click on the same easily identifiable location on the QGIS map canvas. This tells the *Georeferencer* that the coordinates of this location are the same as those at that location in the nokia_areena_plan.tiff

	- After clicking on the QIS map canvas, you should see the *Georeferencer* reappear with the coordinates from the location where you clicked. Press OK.

	- Repeat this at least three more times in different locations, preferably well distributed across the map.

4. When you are finished adding reference points, you can now perform the georeferencing calculation for the image.

	- Press the *Settings* button ![](https://raw.githubusercontent.com/Tampere-University-Urban-Physics/fundamentals-of-gis/master/Assets/2_Exercise/QGIS_georeferencer_settings.png) on the *Georeferencer* menu, and set the *Transformation type* to *Projective*, set the *Target SRS* to the *Project CRS*, give your *Output Raster* an informative name (e.g. arena_plan_modified), and save it in your folder. The *Transformation type* describes the type of mathematical algorithm used to modify (like rotate, twist, skew) the raster. Press OK.

	- Press the *Run* arrow in the *Georeferencer* menu, and the georeferencing will start. It will show you the progress on the georeferencing.

5. Once the calculation is complete, close the *Georeferencer* window (there is no need to save the GCP Points). The new raster that has been georeferenced is saved and you will need add it to your QGIS map now (Hint: Use the Data Source Manager and add it as a raster instead of a vector). Once it's been added, you can drag it below the building, road, and railway layers to see how it fits. Now would be a good time to save your QGIS project again.
	- Check whether the plans line up with the buildings layer (it doesn't have to be perfect for this exercise)! If it doesn't retry the georeferencing with more points and more spacing between them

---

### 1.3. Editing the layers

1. Let’s now **remove a building from the buildings layer that no longer exists and add the arena and some surrounding buildings**. This involves editing the tampere_buildings layer.

2. You may need to enable the *Digitizing Toolbar*, which enables you to edit the vector files. To do this, choose *View* -> *Toolbars* and make sure *Digitizing Toolbar* is selected. 

3. Click on the tampere_buildings layer in the layers panel to highlight it, then toggle editing on ![](https://docs.qgis.org/3.28/en/_images/mActionToggleEditing.png)  in the *Editing Toolbar*.

Editing toolbar:
![](https://raw.githubusercontent.com/Tampere-University-Urban-Physics/fundamentals-of-gis/master/Assets/2_Exercise/QGIS_editing_toolbar.png)

4. First, let’s remove a building that no longer exists. There is one building at the south of the arena that has been demolished.

	- First, choose the *Select* tool ![](https://raw.githubusercontent.com/Tampere-University-Urban-Physics/fundamentals-of-gis/master/Assets/2_Exercise/QGIS_select_tool.png) (if you don’t see it, you may need to add the Selection Toolbar like you did for the Digitizing Toolbar).

	- Select the building to be removed by clicking it. It will turn yellow when selected.

	- Press *Delete* on your keyboard. The building is now deleted.


5. Now, let’s add some new buildings to the layer.
	
	- Select the *Add Polygon Feature* in the *Editing Toolbar*.
	
	- Click around the corners of the raster drawing of all the new buildings (e.g. the arena), gradually drawing a new polygon in the tampere_buildings layer. When you are finished, right-click and the polygon will close. You then get the opportunity to add new Attributes. Add attributes if you want (such as the name of the stadium), then press OK.
	
	- When you are finished adding the new buildings, press the *Save* symbol in the Digitizing Toolbar, and toggle off *editing*.


6. Now, let’s **create a new layer with bus stops around the arena**. These are marked in red in the arena_plan_modified.tiff. GIS data on bus stops will be available from Tampere City or OpenStreetMaps, but here we will make our own layer to practice how this is done.

7. First, let’s create a new layer for the bus stops. In the previous step, we learned how to edit an existing layer, but we can also make a new layer ourselves. In this case, we want points to represent bus stops, so we need to create a new point layer.

	- On the menu, choose *Layer* -> *Create Layer* -> *New Shapefile Layer*

	- Give the new layer an informative filename, like "tampere_bus_stops", and save it in your folder

	- Set the *Geometry type* to be *Point*

	- Under *Additional Dimensions*, set the coordinate system to be ETS89 / TM35FIN(E,N) so that it is in the same coordinate system as the other layers

	- When creating a new layer, you can specify what fields you want to be included in the attributes. There is a default field called ID already. Create a new field. Under *Name* write "road_locat". This will contain the name of the road where the bus stop is located. 

	- Under *Type*, make it *Text*

	- Press *Add to Fields List*, and it will appear in the list of fields to be created

	- Press OK


8. Now, we can edit the new tampere_bus_stops layer we have created to add a location and road name. This happens in a similar way to how we edited the buildings layer, but we are also going to use the identify features tool to find out the name of the roads where each bus stop is located and add it to the tampere_bus_stops attribute table as we add points.

	- Select tampere_bus_stops in the layers panel

	- Toggle *Editing* on in the *Digitization Toolbar*

	- Find a red point on the georeferenced raster and select the tampere_roads layer in the layers panel.

	- Using the *Identify features* tool, click on a line feature in the tampere_roads layers where the bus stop is located and scroll through the attributes until you find the name of the road

	- Next, select the tampere_bus_stops layer again. Choose *Add Point Feature* from the *Digitization Toolbar*. You don’t need to close the *Identify features* window to do this.

	- Click on the red dot in the georeferenced raster. This will add a point, and prompt you to add attributes for the fields. In ID enter numbers (1 for the first point, 2 for the second…) and the name of the road in road_locat. Press OK.

	- Repeat this for each bus stop.

	- *Save the Edits* in the Digitization Toolbar and toggle off *editing*.

---

### 1.4. Map output

1. You’ve finished your edits. Remove the georeferenced raster (you no longer need it) and make a nice informative map that shows the new buildings and the bus stops. You can choose whether or not to include the Google Satellite data. Insert a scale bar to show the distances in the map, a North Arrow, and a title.
	- **Hint**: Look at the crash course exercise for instructions on how to make a map
	- Don't forget to change the symbology of your layers to your liking!
	- The layer names are good for saving data, but the underscores are not ideal for the legend. Try renaming them in the layer panel.
	


<!--stackedit_data:
eyJkaXNjdXNzaW9ucyI6eyJGTXBrOTlHWEFKTzhFNEhOIjp7In
N0YXJ0Ijo4MDA3LCJlbmQiOjgwMjUsInRleHQiOiIqRGlnaXRp
emluZyBUb29sYmFyKi4ifSwiOHlxQXlyV2dCbjRJM0ZsdiI6ey
JzdGFydCI6NDEzNCwiZW5kIjo0MTczLCJ0ZXh0IjoiIyMjIDEu
Mi4gR2VvcmVmZXJlbmNpbmcgdGhlIGFyZW5hIHBsYW5zIn0sIk
1RUUFYNFQzU1pDZzVVUGciOnsic3RhcnQiOjMzMTMsImVuZCI6
MzU0NiwidGV4dCI6IkluIGFkZGl0aW9uLCB3ZSBoYXZlIGEgcm
FzdGVyIGltYWdlIGZpbGUgdGhhdCBzaG93cyB0aGUgZGVzaWdu
cyBmb3IgdGhlIEFyZW5hLOKApiJ9LCJvTXpoQjNnejVpaUE0SX
NoIjp7InN0YXJ0IjozNTQ4LCJlbmQiOjQxMjcsInRleHQiOiJT
bywgdGhlIHRhbXBlcmVfYnVpbGRpbmdzIGRhdGEgaXMgb2xkIG
FuZCB3ZSBuZWVkIHRvIHVwZGF0ZSBpdCDigJMgdGhlcmUgYXJl
IG5vd+KApiJ9LCJaRGRlakpzc1FTc0JkcXIzIjp7InN0YXJ0Ij
o0MTc5LCJlbmQiOjQ3OTksInRleHQiOiJJbiB0aGUgZG93bmxv
YWRlZCBkYXRhLCB0aGVyZSBpcyBhIGZpbGUgLSBub2tpYV9hcm
VlbmFfcGxhbi50aWYg4oCTIHRoYXQgaGFzIGJlZW7igKYifSwi
MW5CU3dIZ3E0emM1aG9PaCI6eyJzdGFydCI6NTIzMywiZW5kIj
o1NDAxLCJ0ZXh0IjoiMy4gTGV04oCZcyBkbyBhdCBsZWFzdCBm
b3VyIHJlZmVyZW5jZSBwb2ludHMuIFRoZSBtb3JlIGxvY2F0aW
9ucywgdGhlIG1vcmUgc3ByZWFk4oCmIn0sInJlMUkybXluOE45
OE8yYWwiOnsic3RhcnQiOjY3MzgsImVuZCI6NzA2NCwidGV4dC
I6InNldCB0aGUgKlRyYW5zZm9ybWF0aW9uIHR5cGUqIHRvICpQ
cm9qZWN0aXZlKiwgc2V0IHRoZSAqVGFyZ2V0IFNSUyogdG8gdG
hlICpQcm/igKYifSwiUE9PMHpnd09Va2E1bU1CMyI6eyJzdGFy
dCI6NzA2OSwiZW5kIjo3MjA3LCJ0ZXh0IjoiUHJlc3MgdGhlIC
pSdW4qIGFycm93IGluIHRoZSAqR2VvcmVmZXJlbmNlciogbWVu
dSwgYW5kIHRoZSBnZW9yZWZlcmVuY2luZyB3aWxs4oCmIn0sIm
N0ZDFpaWNDVG01dGFnODAiOnsic3RhcnQiOjcyODAsImVuZCI6
NzMxOSwidGV4dCI6InRoZXJlIGlzIG5vIG5lZWQgdG8gc2F2ZS
B0aGUgR0NQIFBvaW50cyJ9LCJLS0dJNkhUWVB0eThlNTlNIjp7
InN0YXJ0Ijo3MzIyLCJlbmQiOjc0OTksInRleHQiOiJUaGUgbm
V3IHJhc3RlciB0aGF0IGhhcyBiZWVuIGdlb3JlZmVyZW5jZWQg
aXMgc2F2ZWQgYW5kIHlvdSB3aWxsIG5lZWQgYWRkIGl0IHRv4o
CmIn0sImVGNkIwN3Fud3MwbkJpdlIiOnsic3RhcnQiOjc3MjIs
ImVuZCI6Nzc2OSwidGV4dCI6Iml0IGRvZXNuJ3QgaGF2ZSB0by
BiZSBwZXJmZWN0IGZvciB0aGlzIGV4ZXJjaXNlIn0sIkU4RXJX
b1ZjQnNsNjRES2EiOnsic3RhcnQiOjgyNjMsImVuZCI6ODQ1Ny
widGV4dCI6IjMuIENsaWNrIG9uIHRoZSB0YW1wZXJlX2J1aWxk
aW5ncyBsYXllciBpbiB0aGUgbGF5ZXJzIHBhbmVsIHRvIGhpZ2
hsaWdodCBpdCwgdGjigKYifSwiVlFudUpySlFmMHZBbmx3byI6
eyJzdGFydCI6OTEyMSwiZW5kIjo5MTg0LCJ0ZXh0IjoiLSBQcm
VzcyAqRGVsZXRlKiBvbiB5b3VyIGtleWJvYXJkLiBUaGUgYnVp
bGRpbmcgaXMgbm93IGRlbGV0ZWQuIn0sIlpNRDFyU0lKQjFoWk
1IUlQiOnsic3RhcnQiOjkzMDUsImVuZCI6OTY1MiwidGV4dCI6
Ii0gQ2xpY2sgYXJvdW5kIHRoZSBjb3JuZXJzIG9mIHRoZSByYX
N0ZXIgZHJhd2luZyBvZiBhbGwgdGhlIG5ldyBidWlsZGluZ3Mg
KGUuZy7igKYifSwiRDBOazFFbG5mdW9wT1IzMSI6eyJzdGFydC
I6OTI0MSwiZW5kIjo5MzAxLCJ0ZXh0IjoiLSBTZWxlY3QgdGhl
ICpBZGQgUG9seWdvbiBGZWF0dXJlKiBpbiB0aGUgKkVkaXRpbm
cgVG9vbGJhciouIn0sIld2cldYdFRrSmNxalRta2giOnsic3Rh
cnQiOjk5MTIsImVuZCI6MTAwNTQsInRleHQiOiJHSVMgZGF0YS
BvbiBidXMgc3RvcHMgd2lsbCBiZSBhdmFpbGFibGUgZnJvbSBU
YW1wZXJlIENpdHkgb3IgT3BlblN0cmVldE1hcHMsIGJ14oCmIn
0sIkh3VGcycTI4bDl0Q2FNY3giOnsic3RhcnQiOjEwNDg5LCJl
bmQiOjEwNTI4LCJ0ZXh0IjoiLSBTZXQgdGhlICpHZW9tZXRyeS
B0eXBlKiB0byBiZSAqUG9pbnQqIn0sImlodWlMVjVPOXBHNGlq
S2siOnsic3RhcnQiOjExMDczLCJlbmQiOjExMDgzLCJ0ZXh0Ij
oiLSBQcmVzcyBPSyJ9LCJ2T3VPdGpIZ2U3S3hmWm9vIjp7InN0
YXJ0IjoxMTA4NiwiZW5kIjoxMTA4OCwidGV4dCI6IjguIn0sIl
FRZndWYzJ3SGw2SVBPMjAiOnsic3RhcnQiOjEyNTY4LCJlbmQi
OjEyNjM1LCJ0ZXh0IjoiWW91IGNhbiBjaG9vc2Ugd2hldGhlci
BvciBub3QgdG8gaW5jbHVkZSB0aGUgR29vZ2xlIFNhdGVsbGl0
ZSBkYXRhLiJ9LCJUZ05Kb29IYnhKVDk4aFJyIjp7InN0YXJ0Ij
oxMjM4MCwiZW5kIjoxMjM5OSwidGV4dCI6IiMjIyAxLjQuIE1h
cCBvdXRwdXQifX0sImNvbW1lbnRzIjp7IkkydkpoUlR1aWNiQ1
VSaUsiOnsiZGlzY3Vzc2lvbklkIjoiRk1wazk5R1hBSk84RTRI
TiIsInN1YiI6ImdoOjQwMzA0Nzg4IiwidGV4dCI6IkFkZCBwaW
N0dXJlIiwiY3JlYXRlZCI6MTY4NzA2NzA0NDk4Nn0sIm5XSjcx
VFQ5QkxhcGZQVkYiOnsiZGlzY3Vzc2lvbklkIjoiOHlxQXlyV2
dCbjRJM0ZsdiIsInN1YiI6ImdoOjQwMzA0Nzg4IiwidGV4dCI6
IkFkZCBwaWN0dXJlcyIsImNyZWF0ZWQiOjE3NzQ2MDMzODUzOD
R9LCJudktIUEY2aVFUdWFYdDF5Ijp7ImRpc2N1c3Npb25JZCI6
Ik1RUUFYNFQzU1pDZzVVUGciLCJzdWIiOiJnaDo0MDMwNDc4OC
IsInRleHQiOiJSYXN0ZXIgaXMgbm90IGFkZGVkIHlldCBhdCB0
aGlzIHN0YWdlLiIsImNyZWF0ZWQiOjE3OTIwNDYxNzg2ODB9LC
IwazhzcWZsazZQQVg4SnlLIjp7ImRpc2N1c3Npb25JZCI6Im9N
emhCM2d6NWlpQTRJc2giLCJzdWIiOiJnaDo0MDMwNDc4OCIsIn
RleHQiOiJNYWtlIHRoaXMgbW9yZSBjb25jaXNlIiwiY3JlYXRl
ZCI6MTc5MjA0NjIxNDMyNX0sIkp4ZTZvZkRnM3VxVGx3bHkiOn
siZGlzY3Vzc2lvbklkIjoiWkRkZWpKc3NRU3NCZHFyMyIsInN1
YiI6ImdoOjQwMzA0Nzg4IiwidGV4dCI6IkZpbmQgYSB3YXkgdG
8gdGVzdCB0aGUga25vd2xlZGdlIG9mIHRoaXMgaW4gdGhlIHN0
dWRlbnRzLiIsImNyZWF0ZWQiOjE3OTIwNDYyOTc2MjV9LCJEVj
ExbUk3ZjFzYnh4OVhZIjp7ImRpc2N1c3Npb25JZCI6IjFuQlN3
SGdxNHpjNWhvT2giLCJzdWIiOiJnaDo0MDMwNDc4OCIsInRleH
QiOiJJIGZlZWwgbGlrZSB0aGlzIGp1bXBzIHRvbyBxdWlja2x5
IGludG8gdGhlIGFwcGxpY2F0aW9uLCBzaG91bGQgZXhwbGFuaS
BhIGJpdCBtb3JlLiIsImNyZWF0ZWQiOjE3OTIwNDY0NTk5MTh9
LCJaWDBvdjhYRVdIc3FyQmF2Ijp7ImRpc2N1c3Npb25JZCI6In
JlMUkybXluOE45OE8yYWwiLCJzdWIiOiJnaDo0MDMwNDc4OCIs
InRleHQiOiJFeHBsYWluIHdoYXQgdGhpcyBkb2VzLiIsImNyZW
F0ZWQiOjE3OTIwNDY2MDE3NjF9LCJXamU3alhmanRGelBVMDhG
Ijp7ImRpc2N1c3Npb25JZCI6IlBPTzB6Z3dPVWthNW1NQjMiLC
JzdWIiOiJnaDo0MDMwNDc4OCIsInRleHQiOiJBZGQgc3ltYm9s
IiwiY3JlYXRlZCI6MTc5MjA0NjY1ODI1NX0sImM3bmlsNzV0Q0
xrQnFST3AiOnsiZGlzY3Vzc2lvbklkIjoicmUxSTJteW44Tjk4
TzJhbCIsInN1YiI6ImdoOjQwMzA0Nzg4IiwidGV4dCI6IkFkZC
BhIG5vdGUgdG8gbWFrZSBzdXJlIHRhcmdldCByZXNvbHV0aW9u
IGlzIG5vdCBzZXQuIiwiY3JlYXRlZCI6MTc5MjA0NjczMDQ2Nn
0sInhuNGNSOVJ1MW9sQ3JIVDIiOnsiZGlzY3Vzc2lvbklkIjoi
Y3RkMWlpY0NUbTV0YWc4MCIsInN1YiI6ImdoOjQwMzA0Nzg4Ii
widGV4dCI6IkV4cGxhaW4gd2h5LCBhbmQgd2hhdCBhcmUgR0NQ
IHBvaW50cyIsImNyZWF0ZWQiOjE3OTIwNDY3ODM0MDV9LCIxSD
B0NWdDdUZhcnBaUUhqIjp7ImRpc2N1c3Npb25JZCI6IktLR0k2
SFRZUHR5OGU1OU0iLCJzdWIiOiJnaDo0MDMwNDc4OCIsInRleH
QiOiJJdCB3YXMgYXV0b21hdGljYWxseSBhZGRlZCBpbiB0ZXN0
aW5nLCBhZGQgdGhpcyBhcyBhIGhpbnQuIiwiY3JlYXRlZCI6MT
c5MjA0NjgwOTc5MH0sIlNPaFYySnBUSTk3bDlTc3MiOnsiZGlz
Y3Vzc2lvbklkIjoiZUY2QjA3cW53czBuQml2UiIsInN1YiI6Im
doOjQwMzA0Nzg4IiwidGV4dCI6IkFzayB0aGVtIHRvIHF1ZXN0
aW9uIHRoZSBhY2N1cmFjeSIsImNyZWF0ZWQiOjE3OTIwNDY4NT
Y1Nzl9LCJFamVoQzRJb1JTajF3a3p3Ijp7ImRpc2N1c3Npb25J
ZCI6IkU4RXJXb1ZjQnNsNjRES2EiLCJzdWIiOiJnaDo0MDMwND
c4OCIsInRleHQiOiJFeHBsYWluIHRoYXQgdGhlIHNlbGVjdGVk
IGxheWVyIGlzIHRoZSBvbmUgeW91IGFyZSB3b3JraW5nIGluLi
IsImNyZWF0ZWQiOjE3OTIwNDY5ODM3MTl9LCJlYmI2MnJzRVR4
Q0JTR3JqIjp7ImRpc2N1c3Npb25JZCI6IlZRbnVKckpRZjB2QW
5sd28iLCJzdWIiOiJnaDo0MDMwNDc4OCIsInRleHQiOiJBZGQg
XCJEZWxldGUgU2VsZWN0ZWRcIiBvcHRpb24iLCJjcmVhdGVkIj
oxNzkyMDQ3MDUzMDQzfSwiWWhjRGRVZ3RwZUNvNFBhTiI6eyJk
aXNjdXNzaW9uSWQiOiJaTUQxclNJSkIxaFpNSFJUIiwic3ViIj
oiZ2g6NDAzMDQ3ODgiLCJ0ZXh0IjoiQWRkIGEgbm90ZSBvbiBo
b3cgdGhlIG1vcmUgcG9pbnRzLCB0aGUgbW9yZSBhY2N1cmF0ZS
wgd2l0aCBhIHJvdW5kIG9iamVjdCBsaWtlIHRoZSBhcmVuYSIs
ImNyZWF0ZWQiOjE3OTIwNDcwODU5MDd9LCJyVTlQeUg2dnQwbl
Y2bU0xIjp7ImRpc2N1c3Npb25JZCI6IkQwTmsxRWxuZnVvcE9S
MzEiLCJzdWIiOiJnaDo0MDMwNDc4OCIsInRleHQiOiJBZGQgaW
NvbiIsImNyZWF0ZWQiOjE3OTIwNDcwOTg2NDd9LCJIM2dtZE5G
UERjRTNVTVlqIjp7ImRpc2N1c3Npb25JZCI6Ild2cldYdFRrSm
NxalRta2giLCJzdWIiOiJnaDo0MDMwNDc4OCIsInRleHQiOiJB
ZGQgdGhpcyBhcyBhIG5vdGUuIiwiY3JlYXRlZCI6MTc5MjA0Nz
M1NjI1Mn0sIkhyQ2JpNzA1UWhOelVmcWsiOnsiZGlzY3Vzc2lv
bklkIjoiSHdUZzJxMjhsOXRDYU1jeCIsInN1YiI6ImdoOjQwMz
A0Nzg4IiwidGV4dCI6IkFkZCBzb21lIHJlZmVyZW5jZSB0byBk
aWZmZXJlbmNlIGdlb21ldHJ5IHR5cGVzIiwiY3JlYXRlZCI6MT
c5MjA0NzQzODUzMX0sIjR3c0JkRUFVWDIxbDQxUW8iOnsiZGlz
Y3Vzc2lvbklkIjoiaWh1aUxWNU85cEc0aWpLayIsInN1YiI6Im
doOjQwMzA0Nzg4IiwidGV4dCI6IkFkZCBzdW1tYXJ5IG9mIHdo
YXQgdGhleSBqdXN0IGRpZC4iLCJjcmVhdGVkIjoxNzkyMDQ3ND
gxNzg4fSwiMXRjRUQxZDEyeG5wVU84YyI6eyJkaXNjdXNzaW9u
SWQiOiJpaHVpTFY1TzlwRzRpaktrIiwic3ViIjoiZ2g6NDAzMD
Q3ODgiLCJ0ZXh0IjoiVGhlIGxvb3NlIHN0ZXBzIG1ha2UgbWUg
bG9zZSB0cmFjayBvZiB0aGUgYmlnIHBpY3R1cmUgYXQgbGVhc3
QuIiwiY3JlYXRlZCI6MTc5MjA0NzUxMDkyMn0sIk9aSU9ZNWgx
R1Y3RXBWejQiOnsiZGlzY3Vzc2lvbklkIjoidk91T3RqSGdlN0
t4ZlpvbyIsInN1YiI6ImdoOjQwMzA0Nzg4IiwidGV4dCI6IkFk
ZCBhIG5vdGUgdGhhdCB0aHJvdWdob3V0IHRoZSBwcm9jZXNzIH
lvdSBjYW4gZW5hYmxlL2Rpc2FibGUgbGF5ZXJzIHRvIG1ha2Ug
dGhlIHdvcmsgZWFzaWVyLCBoZXJlIGZvciBleGFtcGxlIHlvdS
BqdXN0IG5lZWQgdGhlIHJvYWRzLCBidXMgc3RvcHMsIGFuZCBy
YXN0ZXIsIHNvIHlvdSBjYW4gZGlzYWJsZSBvdGhlcnMgdG8gbW
FrZSBpdCBlYXNpZXIuIiwiY3JlYXRlZCI6MTc5MjA0NzY0MzMy
Nn0sImtpU29WRXlaZjAxa3FDOFYiOnsiZGlzY3Vzc2lvbklkIj
oiUVFmd1ZjMndIbDZJUE8yMCIsInN1YiI6ImdoOjQwMzA0Nzg4
IiwidGV4dCI6Ik1ha2UgdGhpcyBhIHF1ZXN0aW9uLCBcImRvIH
lvdSB0aGluayBpdCB3b3VsZCBiZSBnb29kIHRvIGtlZXAgdGhl
IGdvb2dsZSBzYXRlbGxpdGUgZGF0YT9cIiIsImNyZWF0ZWQiOj
E3OTIwNDc5Mjg2NDZ9LCJqb0M3M2ZGVkJkN2R0RXJ4Ijp7ImRp
c2N1c3Npb25JZCI6IlRnTkpvb0hieEpUOThoUnIiLCJzdWIiOi
JnaDo0MDMwNDc4OCIsInRleHQiOiJNYWtlIHRoZW0gdXNlIHRo
ZSBmaWVsZHMgdGhleSBtYWRlIGluIHRoZSBvdXRwdXQsIHdpdG
ggbGFiZWxzIGZvciBleGFtcGxlLiIsImNyZWF0ZWQiOjE3OTIw
NDc5OTYzMjl9fSwiaGlzdG9yeSI6Wy0xOTcwNDE1MzY0LDg3OT
IyMjc0NCwxNzc1MTYxNDYwLC0xNzYxMTgwODQ0LC01MDk2MDM1
NDksLTY5NzUyOTMyNCwtOTQ0MTI4OTQ0LC0xNDU5Mjc4MTgxLC
0xOTE4OTMzODMzLC03MzIwNzM4OTMsOTUyOTYyODU3LC0zMzQz
ODMyMDksLTIwMTUwNDk1NzksMTI1NTc1MTA0NiwtOTY4MTEwMT
MzLC0yMTQ2OTEwNTg2LC04MTYyNDI4MDFdfQ==
-->