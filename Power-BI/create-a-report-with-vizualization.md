## Create a new dataset

- I just uploaded a new table copied from a Word file onto the Power BI. This is how it looks like.


![power-bi-ui](/pictures/Power-BI/power-bi-ui.PNG "powewr bi ui")


- Then I'll open a PBIX file (Power BI Desktop file) like below:


![pbix-file](/pictures/Power-BI/pbix-file.PNG "PBIX file")


- With all visualizations available in the report, they are all based on the same dataset. Now, based on this dataset, we will create a new visualization report from scratch.


![building-viz-from-scratch](/pictures/Power-BI/building-viz-from-scratch.PNG "building viz from scratch")


- For example, you can create a table from string dataset and add a title to it. Click **format your visual** and go to **General** section. And toggle Title to on to activate the feature. Then enter your title.


![add-title-to-table](/pictures/Power-BI/add-title.PNG "add title to a table")


- And as this table looks too small, we need to increase the font size in **Values** section of the **Visual** panel.


![increase-font-size](/pictures/Power-BI/increase-font-size.PNG "increase font size")


- Also, increase the column header font so that it pops up properly.


![increase-column-header-font](/pictures/Power-BI/increase-column-header-font.PNG "increase column header font")


- Now, we will open a new page and select new fields. First, select Category from the Item table and then Total Units Sales for Last and This Year from the Sales table.


![total-unit-sales-per-category](/pictures/Power-BI/total-unit-sales-per-category.PNG "total unit sales per category")


- Then we will change the table to a **stacked column chart**.


![stacked-column-chart](/pictures/Power-BI/stacked-column-chart.PNG "stacked column chart")


- We can even change this stacked column chart to a **stacked bar chart**.


![stacked-bar-chart](/pictures/Power-BI/stacked-bar-chart.PNG "stacked bar chart")


- Then we can simply copy the stacked bar chart and change it to a tree map. Also change the fields so that the chart reflects Total Unit Sales This Year per Buyer. With tree map, we comparing rectangles in size with each other.


![tree-map](/pictures/Power-BI/tree-map.PNG "tree map")


- Territory field can produce multiple chart types like below. And when we select one territory from the table, then the map shows that particualr territory in its visualization.


![territory-field](/pictures/Power-BI/territory-field.PNG "territory field")


- Change the Map into the Filled Map so when you select a particular territory from the Table the Filled Map will show the total overlay of that territory.


![filled-map](/pictures/Power-BI/filled-map.PNG "filled map")


- Then we will use an interesting feature called **ArcGIS Map**. With the filled map selected, press the ArcGIS Map button. But it will show some error message. Luckily, we have a workaround for this. Delete the filled map and select the ArcGIS Map again. 


- And don't get caught up on what's at the top. We don't have to sign in to use Online or Enterprise data. Instead, we will use our own data as it specifies at the bottom. With the ArcGIS window selected, choose City from the fields.


![ArcGIS-Map](/pictures/Power-BI/arcgis-map.PNG "ArcGIS Map")


