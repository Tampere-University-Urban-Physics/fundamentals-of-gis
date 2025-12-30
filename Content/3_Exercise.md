### Fundamentals of Geographic Information Systems (GIS)

# Exercise 3: Socio-spatial differentiation & social segregation

By Tuomas Väisänen, Sara Todorovic, Tatu Leppämäki & Markus Jylhä for USP-303 at the Helsinki University. 

*Updated by Rowan van der Kaaden*

## OVERVIEW & PURPOSE
Today’s theme is socio-spatial differentiation & social segregation and we’re going to be looking at the
Helsinki metropolitan area’s population dynamics.  One of the key components of the city is as Shakespeare so eloquently put in Coriolanus “What is the city, but the people?”

Socio-economic features of the inhabitants of the city vary across geographical space and across time.
Today we are using the Statistics Finland’s Grid Database 2016 about the social structure in 250x250m
grid cells. Our task today is to study socio-spatial differentiation and pinpoint low-education and low-income
grid cells. This could also be an interesting starting point for the analysis if we study segregation
in the region.

To do this, the data needs some clean-up, some fields need to be calculated and the lowest quartiles for
different variables (income, education) have to be found. You will be learning to use the conditional statement
functionality. The results are once again to be visualized on a map and to be
reflected on.

## OBJECTIVES
1. Examining the socio-economic structure of the Helsinki Metropolitan Region and pinpointing the
areas with the lowest educational & income level on a map

2. Familiarizing yourself with raster data and data classification

3. Familiarizing yourself with using expressions and conditional statements
	- Using more complex expressions
	- Using conditional statements to reclassify field values

## DATA USED/NEEDED

1. Statistics Finland’s Grid Database 2016
	- Our data set covers the area of the Helsinki metropolitan region.
	
	- See: http://www.stat.fi/tup/ruututietokanta/index_en.html
	
	- The attribute table’s field names are in Finnish, but there’s a pdf included in the course
data with the translations, explanations for the field names and longer descriptions (in
English, Finnish, and Swedish).

## Completion

Work in pairs or individually. Complete the exercise and submit a short report containing at least the following:

1. Maps of the outcome of the exercise

	- Remember: all maps should have a legend, a scale bar and a north arrow

	- Use the tips from the Crash Course to improve your map

2. A short reflection (200-300 words) on this exercise and your map

## EXERCISE PHASES

### 1.1. Getting familiar with the data

1. Download 3_Exercise_data.zip from the [Github repository](https://github.com/Tampere-University-Urban-Physics/fundamentals-of-gis/tree/master/Data), save it in a folder for this exercise, and extract the contents from the zip. Add the data to your GIS project.

2. **Go through the attribute table and use the pdf-files to decipher the meanings of each field**. The attribute table consists of a few themes (recognizable by the prefixes) and slightly over 100 fields.
	- The fields we’re using depict the following:
		- ko_perus (Inhabitants with no qualification after basic-level studies, education theme’s prefix ko_)
		- hr_pi_tul (Inhabitants belonging to the lowest income category, Inhabitants’ income theme’s prefix hr_)
		- Under each theme, eg. education (prefix ko_) the first field has the total value of each grid cell (feature) in that field. We’ll need these total fields in the analysis:
			- Level of education: ko_ika18y (Total number of people in grid cell, aged 18 or over)
			- Level of income: hr_tuy (Total number of people in grid cell, aged 18 or over)
			Note: ko_ika18y and hr_tuy both contain the same values. However, for clarity you might want to use the one with the same prefix as the one you are comparing it to.
	- Background map.
		- You can use for example satellite imagery like we did in Exercise 2, or OpenStreetMaps, which you can add using the same steps as for google satellite imagery. 
		- Tip: Remember that if you first bring in your data (from the practical folder), the background map should project automatically to the right coordinate system.

--- 

### 1.2. Cleaning up the data

1. As you browsed the attribute table of the data, you might’ve noticed that **some fields have
the value -1. This is because the population within the grid cell is sufficiently small that providing the value would violate data privacy protection laws. You’ll have to clean up the data** and there are a few
ways to do this. Here we’re focusing on selections.
	- -1 is a placeholder value for NoData or NotAvailable and not an actual value. It does not mean that there are actually -1 people in the cell...
	- The Basic-level studies field (ko_perus) depicts the amount of people whose highest achieved level of education is basic-level studies. It has values of -1 for those grids that have less than 10 people over 18 years old (ko_ika18y).
	- And same thing for the low-income variables hr_pi_tul and hr_tuy.

2. **You could use select by expression** to select the features that have values in the two wanted fields (ko_perus, hr_pi_tul) that are 0 or greater AND have people living in them (ko_ika18y, hr_tuy):
![enter image description here](https://raw.githubusercontent.com/Tampere-University-Urban-Physics/fundamentals-of-gis/master/Assets/3_Exercise/3_Exercise_datacleanup_expression.png)
	- Hint: Crash Course 2.1 step 3
	- After the selection, save the selected features as a new shapefile (Right click layer with selection > *Export* > *Save selected features as…*).
		- Don't forget to give the new file an informative name and save it in your folder! 

	*Tip: If you are confused with the formula, try reading it aloud: “Select the features where X is greater than 0 AND Y is greater or equal than 0, AND…”*

3. **You should end up with a new layer without any -1 values in the four wanted fields.**

---

### 1.3. Calculating the proportions

1. **In the newly created layer, use the *field calculator* to calculate new fields with the percentage of the population of different education or income levels**
	- Calculate the proportions of the wanted fields 
		- Make sure to use the correct variables when calculating. To get the percentage, we want to divide by the total number field of each theme in the calculation.
			- Level of education: ko_ika18y (Aged 18 or over, total)
			- Level of income: hr_tuy (Aged 18 or over, total)
		- For example: to calculate the share of residents with only a basic-level studies in grid cells: (ko_perus / ko_ika18y) * 100 (Studied value divided by total value gives the share, *100 makes the result as %). For income, use the hr_pi_tul and hr_tuy fields.
		- Make sure the field type is set to decimal numbers and make the values have at least 2 decimal numbers (precision).
		- Name the fields accordingly. Note: Shapefile allows only 10 characters in the field name and no spaces.
	- Hint: Crash Course 2.1 step 7

2. **You should end up with two new fields that all have percentage values.**

--- 

### 1.4. Quantiles and reclassification

1. **Our newly created proportion fields are tempting, but let’s take it to the next level with
quantiles**
	- In the Layer Properties Symbology-tab and under the *Graduated* symbols you can find different classification modes.
	- When classifying data with *Equal Count* as the classification method and setting the number of classes to four, each class is called a quartile.
		- Thus, each class has the same number of entries (hence equal count)

2. **We’re going to have to utilize QGIS’s visualization tools to find out the class boundaries for
quantiles within QGIS.** You could also use any statistical analysis application (Excel, LibreCalc,
SPSS etc.) to find out the boundaries.
	- So, open the *symbology* tab of the layer, select *graduated*, select the correct field
		- E.g., Basic-level education proportion
	-  Switch mode to *Quantiles (Equal Count)* and drop the number of classes from 5 down to 4. Click *Classify*
		- Make notes about the class boundaries because you’re going to use them soon. For Basic-level education they look something like this:
![enter image description here](https://raw.githubusercontent.com/Tampere-University-Urban-Physics/fundamentals-of-gis/master/Assets/3_Exercise/3_Exercise_data_classification_edu.png)
		- Do the same for the remaining proportion field (level of income percentage) and make notes about the class boundaries for that field as well.

3. **Now that we have the boundaries for the two classifications, it’s time to reclassify the data
into four classes.**
	- Open the *attribute table* and *field calculator*, we’re creating new fields for the reclassified values using a slightly more complex conditional statement this time. 
		- Note! Do all the analyses in the same layer so that you get the new columns in the same layer.
	- The logic behind the numbering of the classes should be the same across all the reclassifications. In this instance our logic can be as follows: class 1 is the lowest (e.g. lowest share of inhabitants with no qualification after basic-level studies) and class 4 is the highest quartile (e.g. highest share of inhabitants with no qualification after basic level studies).
	- Let’s use the conditional statement first for basic-level education proportions. This is where you use the class boundary notes. The expression looks something like this:
![enter image description here](https://raw.githubusercontent.com/Tampere-University-Urban-Physics/fundamentals-of-gis/master/Assets/3_Exercise/3_Exercise_reclassification.png)
	- NOTE: Use a dot as a decimal separator instead of comma.
	- Check the *attribute table* that the values are correct (click the new column field name to order the values in order to see the highest and lowest values, you should only have numbers 1, 2, 3 and 4).
		- If the fields turn out empty , check that “Only update X selected features” is ticked off in the field calculator.
	- Do the same for the remaining proportion field.
		- Again, read the expression aloud if you don’t understand it!

4. **Now that you’ve reclassified the proportions into classes 1, 2, 3 or 4 it’s time to find the low
education & low-income areas in Helsinki metropolitan region.**
	
	- Select the grid squares that have been given the classification 4 in both of the reclassified fields
		- Do you remember how to select attributes based on their value? Hint: check section 1.2 in this exercise, step 2.
	- Save the selection as a new layer and give it an informative name
	- Compose a map of the results

---

### 1.5. Map outputs

1. Time to make your maps! Think of which maps would be good to describe this analysis. What information do you want to communicate? **Hints:**
	- Make two maps with graduated symbology showing quantiles, one for income and one for education
	- Make a map highlighting grid squares where both income level and education are classed 4 to highlight issues with social segregation
	- Think about whether you should include the data we removed earlier(-1 entries), would it be valuable to the viewer of the map? Is the map accurate without them? How would you visualize them? Justify your choices in the reflection.

Don't forget to include the requirements for a good map (See Crash Course)! 

2. Optional: You can use the plugin we installed during the Crash Course, QuickMapServices (QMS), to get more basemap options
	- You can open the *Search QMS* panel using this icon, it should open below your toolbox:
![enter image description here](https://raw.githubusercontent.com/Tampere-University-Urban-Physics/fundamentals-of-gis/master/Assets/3_Exercise/3_Exercise_QMS_1.png)
	-  Here are some examples of basemaps you could use:
![enter image description here](https://raw.githubusercontent.com/Tampere-University-Urban-Physics/fundamentals-of-gis/master/Assets/3_Exercise/3_Exercise_QMS_2.png)
	- Note: These are online sources, which means they might be slow and/or limited to certain scales


---

### Optional: Refining the analysis 

1. The analysis we’ve done above is quite coarse. You can refine it by using the following fields:
	- te_vuok_as (Households living in rental dwellings, Household life theme’s prefix te_)
		- Household life: te_taly (Households, total)
	- pt_tyott (Unemployed, Main type of activity theme’s prefix pt_)
		- Main type of activity: pt_tyovy (Labour force, total)

2.  You should use the original grid data and not the one you’ve been working with. Why?
		- You’ll have an untampered dataset and won’t lose any progress you’ve made with education and income data due to unfortunate errors. 
		- Note: The education and income fields’ total number fields have the population 18 years old and older (can you think of why?), but the totals for household and unemployment fields use different metrics: Total number of households and total number of available workforces.
		- Before tampering with the original grid file, you should save it as a new file, so you have a clean backup to fall back on. 

3. When you’ve reclassified the proportions to the quartiles, you can join the layers using the id_nro field. Joining the layers means that you combine data from different layers. The layers to be joined need to have an attribute that is shared (here id_nro field), which is used as the spatial relationship of the layers. 
	- You can select which fields to join, so you don’t have to join all the fields, just the
reclassified fields for rented dwellings and unemployment.

4. Compose a map of the results
	- How do the refined results differ from the coarser results?
	- Are these refined results enough to delineate segregated areas?
		- Would you need additional data? What data?

 



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1OTEzMDEyODQsLTE4MDA2NjQzNSw5Nj
cwOTQwNTksNTg1Njc1OTgxLC0xOTc0MjgzNDM1LC0yMDU4OTM5
NzY1LDEyODE0NDUyNywyNjU5ODc0MzAsMTk4MTA4NzY3LDczNj
QxOTcwOCwxMjIxMjY0NDMyLDE4MjUwMzcwNDAsLTM5MTg4MjA1
MCwtMTYzNzYwNDE3OSwtMTU2ODc2OTc2OSwxMDc1NTg4OTUyLC
00Njk2NDE2ODIsLTI2OTU1ODc4NSwtMTM2NDE3NDk5MywxNzI2
ODEzNjM0XX0=
-->
