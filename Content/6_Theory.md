### Fundamentals of Geographic Information Systems (GIS)

# Theory 6: Thiessen polygons and heatmapping (kernel density estimation)

*By Rowan van der Kaaden*

## Heatmap analysis
Spatial phenomena tend to vary in intensity across a geographical area. When the occurrence of a certain phenomenon is significantly higher in one area than in the surrounding area, you could describe it as a “hotspot” for that phenomenon. For instance, cities are hotspots of buildings and human activity, forests are hotspots of trees etc. If you work for the police, you may use GIS to locate crime hotspots. There are many ways to locate these hotspots: interpolation, cluster analysis, and heatmap analysis, to name a few. Today we’re taking a look at heatmaps and heatmap analysis. 

As the name would suggest, heatmaps describe the “heat,” or intensity, of the phenomenon in question. Heatmaps are generated with point feature data and form a raster layer. In its most rudimentary form, the pixel values of a heatmap depict proximity to other point features within a certain, user-specified, radius. A slightly more advanced form of heatmaps can be given a weight (from the point features’ attribute table) that will influence the pixel values.

For instance, two heatmaps are created out of the same point feature layer (bus stops). One heatmap wasn’t given a weight, so it only looks for other bus stops within a certain radius, e.g. 500 meters. The other was given a weight, e.g. daily passenger amount, from the attribute table of the bus stops layer. To generate this other heatmap, the GIS software not only looks for bus stops within a radius of 500 meters of each other, but also gives the bus stops with higher passenger amounts a higher weight. Thus, the bus stops with a high daily passenger amount will be “favored” in the resulting heatmap raster layer.

A more advanced form of heatmap can also include modification of the kernel shape. Kernel is the statistical function used to give proximity and weight scores to the point features within the radius. Heatmaps are also known as kernel density analysis.

### What happens in Kernel Density Analysis?

To understand the basic idea of Kernel Density Analysis that we use to create a heatmap, you can watch this 9 minutes long video (Point Pattern Analysis Part 5: Kernel Density Estimation): https://www.youtube.com/watch?v=PBZVTjmhl74

When applying the kernel method, QGIS draws a circular neighborhood around each sample point (not each cell) and then applies a math function that goes from 1 at the location of the point to 0 at the neighborhood boundary. Think of a kernel as a smoothly curved surface that is fitted over each point.

![](http://www.geography.hunter.cuny.edu/~jochen/gtech361/lectures/lecture11/concepts/Kernel%20density%20calculations_files/image001.gif)
When a kernel function is applied to each data point, the effect is like that of an elevation surface, except that the density value for each cell is calculated by adding the values of all the kernel surfaces where they overlay the cell center.

![](http://www.geography.hunter.cuny.edu/~jochen/gtech361/lectures/lecture11/concepts/Kernel%20density%20calculations_files/image002.gif)
The kernel function generally creates a smoother-looking surface than one created with the simple method[^1].

If you still have difficulty understanding this concept, try out this video: https://www.youtube.com/watch?v=x5zLaWT5KPs

Otherwise, don't worry about it for now and move on! 

## Thiessen polygons/Voronoi diagram

Thiessen polygons, also known as Voronoi polygons or Voronoi diagrams, are a method used to divide a space into regions based on proximity to a set of input points or data. Each polygon represents the area that is closest to a specific point in the dataset.

Here's how Thiessen polygons work:

1.  Input points: The first step is to have a set of input points. These points can represent any geographic feature, such as cities, weather stations, or sampling locations.
    
2.  Nearest neighbor determination: For each input point, the nearest neighboring points are identified. This is typically done using a distance-based algorithm, such as Euclidean distance or Manhattan distance.
    
3.  Construction of polygons: Once the nearest neighbors are determined, Thiessen polygons are constructed. Each polygon is defined by connecting the input point to its neighboring points, forming a boundary that encloses the area closest to that particular input point.
    
4.  Polygon attributes: Each Thiessen polygon is associated with the input point it originated from. This allows for the transfer of attributes or characteristics from the input points to their respective polygons. For example, if the input points represent weather stations, the Thiessen polygons can be used to determine which areas are influenced by each station.

![](https://pro.arcgis.com/en/pro-app/latest/tool-reference/analysis/GUID-6231F564-FA42-435F-A4A7-CE6A88167144-web.jpg)[^2]

Thiessen polygons have various applications in GIS analysis and spatial modeling. They can be used for proximity analysis, such as determining service areas or catchment areas around facilities. They are also useful for interpolation, as they provide a way to estimate values at unsampled locations based on the values at the input points within each polygon.

# Time to get your hands dirty! Move on to the [6th exercise](https://github.com/Tampere-University-Urban-Physics/fundamentals-of-gis/blob/master/Content/6_Exercise.md) to apply this new knowledge

[^1]: http://www.geography.hunter.cuny.edu/~jochen/gtech361/lectures/lecture11/concepts/Kernel%20density%20calculations.htm
[^2]:https://pro.arcgis.com/en/pro-app/latest/tool-reference/analysis/create-thiessen-polygons.htm
<!--stackedit_data:
eyJkaXNjdXNzaW9ucyI6eyJTem0yVGd1RUxkNkl2SFI1Ijp7In
RleHQiOiIjIyBIZWF0bWFwIGFuYWx5c2lzIiwic3RhcnQiOjE2
MCwiZW5kIjoxNzl9LCJmcHVVdk9kYnlqRkY5N1ZTIjp7InN0YX
J0IjoyNTIsImVuZCI6NTIxLCJ0ZXh0IjoiV2hlbiB0aGUgb2Nj
dXJyZW5jZSBvZiBhIGNlcnRhaW4gcGhlbm9tZW5vbiBpcyBzaW
duaWZpY2FudGx5IGhpZ2hlciBpbiBvbmUgYXJlYeKApiJ9LCJ3
QldTTlZQb3FqalJGbjVLIjp7InN0YXJ0Ijo5NDQsImVuZCI6MT
IzMSwidGV4dCI6IkluIGl0cyBtb3N0IHJ1ZGltZW50YXJ5IGZv
cm0sIHRoZSBwaXhlbCB2YWx1ZXMgb2YgYSBoZWF0bWFwIGRlcG
ljdCBwcm94aW1pdHkgdG/igKYifSwiU1RHeFRnZWh1R0JVcDJO
aCI6eyJzdGFydCI6MTIzMywiZW5kIjoxODUzLCJ0ZXh0IjoiRm
9yIGluc3RhbmNlLCB0d28gaGVhdG1hcHMgYXJlIGNyZWF0ZWQg
b3V0IG9mIHRoZSBzYW1lIHBvaW50IGZlYXR1cmUgbGF5ZXIgKG
J1c+KApiJ9LCI5Nkh6M3FzMHNxY0dmRkhtIjp7InN0YXJ0Ijoy
NjkxLCJlbmQiOjI4MjgsInRleHQiOiIhW10oaHR0cDovL3d3dy
5nZW9ncmFwaHkuaHVudGVyLmN1bnkuZWR1L35qb2NoZW4vZ3Rl
Y2gzNjEvbGVjdHVyZXMvbGVjdHVyZTExL2Nv4oCmIn0sIjVPTn
dMaVZzYkU4QzBuejkiOnsic3RhcnQiOjMwNzUsImVuZCI6MzIx
MiwidGV4dCI6IiFbXShodHRwOi8vd3d3Lmdlb2dyYXBoeS5odW
50ZXIuY3VueS5lZHUvfmpvY2hlbi9ndGVjaDM2MS9sZWN0dXJl
cy9sZWN0dXJlMTEvY2/igKYifSwiVmFUak55Z3ZXdXE0aEMzRS
I6eyJzdGFydCI6NDg3NSwiZW5kIjo0OTk0LCJ0ZXh0IjoiIVtd
KGh0dHBzOi8vcHJvLmFyY2dpcy5jb20vZW4vcHJvLWFwcC9sYX
Rlc3QvdG9vbC1yZWZlcmVuY2UvYW5hbHlzaXMvR1VJRC02MjMx
RuKApiJ9fSwiY29tbWVudHMiOnsiUFFkQXEwc285OGxSZkRoNy
I6eyJkaXNjdXNzaW9uSWQiOiJTem0yVGd1RUxkNkl2SFI1Iiwi
c3ViIjoiZ2g6NDAzMDQ3ODgiLCJ0ZXh0IjoiQWRkIHBpY3R1cm
UiLCJjcmVhdGVkIjoxNjg3NzY4MDM3NzE5fSwiTnBYZkRTbWtM
UklUNjRYRiI6eyJkaXNjdXNzaW9uSWQiOiJmcHVVdk9kYnlqRk
Y5N1ZTIiwic3ViIjoiZ2g6NDAzMDQ3ODgiLCJ0ZXh0IjoiQWRk
IGZpZ3VyZXMiLCJjcmVhdGVkIjoxNzk0ODE1OTkwMzg2fSwiRj
IzRXVPN1FFc2JoNGo4NyI6eyJkaXNjdXNzaW9uSWQiOiJ3QldT
TlZQb3FqalJGbjVLIiwic3ViIjoiZ2g6NDAzMDQ3ODgiLCJ0ZX
h0IjoiQWRkIGZpZ3VyZXMiLCJjcmVhdGVkIjoxNzk0ODE2MDU1
MzYyfSwiQ1k5THZaNlgxVTdvejVuTyI6eyJkaXNjdXNzaW9uSW
QiOiJTVEd4VGdlaHVHQlVwMk5oIiwic3ViIjoiZ2g6NDAzMDQ3
ODgiLCJ0ZXh0IjoiQWRkIGZpZ3VyZXMiLCJjcmVhdGVkIjoxNz
k0ODE2MDk5MTY1fSwiazJXVzQyb252dkl1YlN1NCI6eyJkaXNj
dXNzaW9uSWQiOiI5Nkh6M3FzMHNxY0dmRkhtIiwic3ViIjoiZ2
g6NDAzMDQ3ODgiLCJ0ZXh0IjoiVXBkYXRlIHNvdXJjZSBvZiB0
aGlzIHdpdGggb3VyIG93bi4iLCJjcmVhdGVkIjoxNzk0ODE2Mz
AwMjM4fSwiRThuM0hoQkNTbFlyMXJkRyI6eyJkaXNjdXNzaW9u
SWQiOiI1T053TGlWc2JFOEMwbno5Iiwic3ViIjoiZ2g6NDAzMD
Q3ODgiLCJ0ZXh0IjoiVXBkYXRlIHNvdXJjZSBvZiB0aGlzIHdp
dGggb3VyIG93bi4iLCJjcmVhdGVkIjoxNzk0ODE2MzAzMzI0fS
wiWFM2Vm5yZDQ2VVpHREhpTSI6eyJkaXNjdXNzaW9uSWQiOiJW
YVRqTnlndld1cTRoQzNFIiwic3ViIjoiZ2g6NDAzMDQ3ODgiLC
J0ZXh0IjoiVXBkYXRlIHRoaXMgd2l0aCBsb2NhbCBzb3VyY2Uu
IiwiY3JlYXRlZCI6MTc5NDgxNjQzNDk1NX19LCJoaXN0b3J5Ij
pbLTE5NTMzMDIwOTgsLTk5ODc4MjYzOCw1NzI4NjU0NzUsNjUy
ODk1NzY0LC0xNzMyNzU5MzExLDExMjQ1MDg0ODgsLTY5MTM3OD
UwOV19
-->