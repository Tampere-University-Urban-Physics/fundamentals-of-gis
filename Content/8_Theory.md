### Fundamentals of Geographic Information Systems (GIS)

# Theory 8: How do maps lie?

Now that you are getting a bit more experience with the theory and practice of using GIS, you may have realized how many different decisions you need to make about how to show the mapped results. While maps are powerful tools for understanding the world, they are far from objective truths. Every map is a product of human decisions—about what to include, what to emphasize, and what to leave out. These choices can mislead intentionally or unintentionally, turning maps into instruments of distortion. A map is not a mirror of the world, but can shape perception. They may not, in fact, be neutral, factual representations of geography.

Geographer Mark Monmonier wrote an important book that describes how the little modifications we make in our maps — for example the smoothing of geographic features, classification schemes, spatial aggregation of data — all represent little “lies.” He writes, “To portray meaningful relationships for a complex, three-dimensional world on a flat sheet of paper or a video screen, a map must distort reality…There’s no escape from the cartographic paradox: to present a useful and truthful picture, an accurate map must tell white lies.” (1996)

## What are the different ways that maps can lie?

### Projection Distortion
Remember the theory about projections? One of the most common ways maps lie is through projection distortion. The Earth is a three-dimensional sphere, and any attempt to represent it on a flat surface requires a trade-off. Different map projections preserve different qualities—some maintain shape, others preserve area or distance—but none can keep all aspects accurate at once. The Mercator projection, widely used in schools and online maps, preserves navigational angles but greatly distorts the size of landmasses, especially near the poles. This makes countries in the Global North (like Europe) appear far larger than they are, while shrinking regions near the equator like Africa and South America. These visual distortions can reinforce Eurocentric or colonial narratives by inflating the visual significance of northern nations.

### What is included in the map
Another deceptive aspect of maps is selective inclusion and omission. No map can show everything, so cartographers must decide what features to highlight. These choices are often influenced by the map’s intended use, but they can also reflect political, cultural, or ideological biases. For example, a political map might display national borders in a way that supports one side of a territorial dispute, omitting contested claims altogether. Similarly, a city map made for tourists might show major attractions and shopping areas while leaving out lower-income neighborhoods or sites of social tension, effectively erasing parts of the urban experience.

### Design Choices
Maps also lie through symbol manipulation and design choices. Colors, labels, font sizes, and symbols all carry meaning. A map might use red to mark enemy territory, evoking danger or aggression, while using calm blues or greens for allies or home regions. Similarly, the size of a capital city's icon or the boldness of its name can exaggerate its importance. These visual cues guide the reader’s interpretation, often without them realizing it.

### Scale and Zoom
The map scale represents the relationship between distances on a map and the corresponding distances on the ground. It is what changes when you zoom in and out of the map in QGIS. Even scale and zoom can be misleading. A map that zooms in on a specific region can give undue importance to local events, while a zoomed-out global map might minimize critical details. Strategic use of scale can highlight or obscure information, shaping the viewer’s sense of proportion and relevance.

### Aggregation
So far in various exercises, we have aggregated different types of data into larger areas. For example, in the crash course, we aggregated transit stops by small areas in Helsinki. Data are commonly collected at one scale, such as household or neighborhood-level, and reported at much broader scales, such as postcode or municipality. We call this spatial aggregation, and it is often necessary for presenting a big-picture view or preserving privacy.

However, this can lead to issues. One of these is the Ecological Fallacy. The ecological fallacy is a logical error that occurs when someone makes assumptions about individuals based on group-level data or statistics. Simply put, it is the mistake of believing that what is true for a group must also be true for every individual in that group. For example, consider a world map with average income levels. If you were to conclude that every person in a wealthy country has a high income, or every person in a low income country lives in poverty, that would be an ecological fallacy. Essentially, it means that group averages with spatial units do not always represent individual cases. Watch out for the ecological fallacy whenever you are interpreting maps, or you might come to false conclusions about the people who live in a neighborhood or other areas!

The Modifiable Areal Unit Problem (MAUP) is another example of how maps can lie through aggregation. It relates to how the scale at which you look at a geographic pattern can lead you to derive completely different results from the same underlying data - where the boundaries of zones are drawn can easily change data patterns and analysis outcomes. 
We can illustrate MAUP through an analysis of the rate of a disease in a population. To analyse the broader trends in disease prevalence and to protect the privacy of patients, health officials may count the number of cases by city block, postcode, or another zonation. But, dividing spaces into larger areas can change the apparent patterns of disease. For example, see the figure below on the rates of respiratory diseases in Ottawa, Canada. The maps use the same underlying data, and each uses the same classification scheme (quartiles), but have different spatial aggregations. However, these maps look very different, highlighting challenges with MAUP, and how you should be cautious when aggregating and viewing aggregated data.

### Political Lies

Ok, obviously this could be a big section, but let’s focus on a couple of example relating to maps. Political lies in mapping can include e.g. propaganda, campaign advertisements, and even the way we draw borders around contested regions (e.g. how does Google Maps choose to draw the border between Crimea and Ukraine/Russia)? However, here we will focus on something called Gerrymandering.
Gerrymandering is probably the most commonly know way in which maps and mapmaking can be used to influence political results. Here, boundaries are manipulated to favour a certain political party or group. It originates from a portmanteau of the the Governor of Massachusetts in 1812 (Elbridge Gerry) and salamander. Governor Gerry signed a bill that created a district in the Boston area whose shape was compared to a salamander in order to create a partisan voting district that favoured his party.

This can be illustrated by the figure below. Consider a population that is 40% in favour of the yellow party and 60% in favour of the blue party. The blue party should win the election, right? Let’s divide them into five voting districts. Depending on how you draw them will lead to a different electoral result. This effect is called gerrymandering, and you can still see some pretty funky electoral boundaries in some countries because of this.

## Summary

In making maps, we make many decisions - big and small - which can lead to different intepretations of what the map is communicating. Small lies include standard cartographic decisions, including projection, symbolization, standardization, classification, aggregation, and zonation, which can guide the interpretation of maps. Then, there are bigger of lies —gerrymandering, propaganda maps, maps that push a particular political perspective, and misleading advertising maps. The hope is that you are now a critical map reader and map maker that is able to identify and understand the ways that maps lie.


**You can now move on to the [8th exercise](https://github.com/Tampere-University-Urban-Physics/fundamentals-of-gis/blob/master/Content/8_Exercise.md)!**
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3ODY3NTc0MDBdfQ==
-->
