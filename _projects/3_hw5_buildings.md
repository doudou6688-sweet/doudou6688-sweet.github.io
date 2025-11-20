---
name: HW5 - Building Inventory Visualizations
tools: [Python, Altair, vega-lite]
image: assets/pngs/buildings.png
description: Interactive visualizations exploring the Building Inventory dataset
custom_js:
  - vega.min
  - vega-lite.min
  - vega-embed.min
  - justcharts
---

# Building Inventory Data Visualizations

This project explores the Building Inventory dataset using interactive visualizations created with Python and Altair.

## Plot 1: Distribution of Buildings by Agency

<vegachart schema-url="{{ site.baseurl }}/assets/json/hw5_plot1_agencies.json" style="width: 100%"></vegachart>

### Description
This visualization shows the top 15 government agencies by their total number of buildings in the inventory. The bar chart is sorted in descending order to easily identify which agencies manage the most facilities.

### Design Choices - Encodings
I used a horizontal bar chart with the following encodings:
- **X-axis (quantitative)**: Count of buildings, using a quantitative scale to show the number of buildings
- **Y-axis (nominal)**: Agency Name: categorical variable sorted by count (descending)
- **Color (quantitative)**: Count of buildings - encoded with the x-axis to provide an additional visual cue
- **Tooltip**: Agency name and building count for detailed on-hover information

### Design Choices - Colormaps
**Color variable**: The color encodes the **count of buildings** for each agency (same as the x-axis), providing redundant encoding that reinforces the quantitative information.

I chose the **Viridis**, because the followings:
- it provides good contrast
- creates a natural visual hierarchy where agencies with more buildings appear more prominent (brighter colors)

The sequential nature of Viridis is appropriate here because we're encoding a quantitative variable (building count) that has a natural ordering from low to high. 

### Data Transformations
For this plot, I performed the following transformations:
1. **Filtering (Python)**: Identified the top 15 agencies by building count in Python, then used Vega-Lite's 'transform_filter' to display only those agencies
2. **Aggregation (Vega-Lite)**: Used Altair's built-in 'count()' function to aggregate buildings by agency
3. **Sorting (Vega-Lite)**: Applied a descending sort based on count to display agencies from highest to lowest
4. **Data loading**: The data is loaded directly from the URL in the browser, keeping the JSON file size small

## Plot 2: Interactive Timeline of Building Acquisitions

<vegachart schema-url="{{ site.baseurl }}/assets/json/hw5_plot2_interactive.json" style="width: 100%"></vegachart>

### Description
This interactive visualization explores the relationship between when buildings were acquired and their size. The top panel is a scatter plot showing individual buildings, and the bottom panel displays a histogram that responds to selections made in the scatter plot.

### Design Choices - Encodings
The scatter plot uses:
- **X-axis (quantitative)**: Year Acquired - temporal scale from 1850 to 2020
- **Y-axis (quantitative)**: Square Footage - using a logarithmic scale to handle the wide range of building sizes
- **Color (conditional)**: Blue for selected points, light gray for unselected
- **Opacity**: Set to 0.5 to show density patterns

The histogram uses:
- **X-axis (quantitative)**: Year Acquired - binned into 30 bins for temporal distribution
- **Y-axis (quantitative)**: Count of buildings
- **Color**: Solid steel blue for consistency with selected points in scatter plot

### Design Choices - Colormaps
**Color variable**: The color encodes the **selection state** of each data point: whether it's inside or outside the brush selection.

Rather than using a traditional sequential or diverging colormap, I used a **conditional binary color scheme**:
- **Selected data** (inside brush): Steel blue: clear and professional, I think
- **Unselected data** (outside brush): Light gray (to emphasize the selection)

The reason of choice: the purpose is to show selection state (a categorical distinction: selected vs. not selected), not to encode an additional quantitative variable. This creates clear visual feedback for the interactive brushing feature.

### Data Transformations
For this plot, I performed several data cleaning and transformation steps:
1. **Outlier filtering (Vega-Lite)**: Used 'transform_filter' to remove buildings with years before 1800 (likely data errors) and negative/zero square footage
2. **Scale transformation (Vega-Lite)**: Applied a logarithmic scale to square footage in the visualization to better show the distribution across multiple orders of magnitude
3. **Data loading**: The data is loaded directly from the URL in the browser, avoiding the need to embed large datasets in the JSON file


## Interactivity

### Brush Selection
The key interactive feature in Plot 2 is the **brush selection** tool. Users can click and drag on the scatter plot to select a rectangular region of interest. This interaction:

1. **Highlights selected points**: Points within the selection turn steel blue while others change to gray
2. **Filters the histogram**: The bottom histogram updates in real-time to show only the temporal distribution of selected buildings
3. **Enables pattern discovery**: Users can explore questions like "How does the size distribution differ between buildings acquired in the 1960s vs. the 2000s?", etc.

This linked interaction makes the visualization more clear and interesting by:
- Allowing users to focus on specific subsets of data without creating multiple plots
- Revealing relationships between temporal patterns and building size that might not be apparent in a static view

and so on...

---

<div class="left">
{% include elements/button.html link="https://raw.githubusercontent.com/UIUC-iSchool-DataViz/is445_data/main/building_inventory.csv" text="The Data" %}
</div>

<div class="right">
{% include elements/button.html link="https://github.com/doudou6688-sweet/doudou6688-sweet.github.io/blob/main/python_notebooks/HW5_building_viz.ipynb" text="The Analysis" %}
</div>
