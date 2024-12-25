# Visualize-Global-Formaldehyde-Levels-in-Google-Earth-Engine-with-Sentinel-5P

![Screenshot (47)](https://github.com/user-attachments/assets/68cc0438-c4cb-4d73-8922-a40aa74c79cd)



This script uses Google Earth Engine (GEE) to visualize formaldehyde (HCHO) concentration levels over East Africa during a specified time range. Here's a step-by-step explanation:

1. Import the FAO GAUL Dataset
javascript
Copy code
var dataset = ee.FeatureCollection("FAO/GAUL/2015/level1")
  .filter(ee.Filter.inList('ADM0_NAME', countries));
FAO GAUL Dataset: The dataset contains administrative boundaries worldwide.
Filter for East African Countries: The script selects boundaries for Kenya, Uganda, Tanzania, Rwanda, and Burundi.
2. Display East Africa Boundaries
javascript
Copy code
Map.addLayer(dataset, {color: 'blue'}, 'East Africa Boundary');
Add Boundary Layer: The boundaries of the selected countries are displayed in blue on the map.
3. Load HCHO Data
javascript
Copy code
var collection = ee.ImageCollection('COPERNICUS/S5P/OFFL/L3_HCHO')                   
  .select('tropospheric_HCHO_column_number_density')
  .filterDate('2019-06-01', '2019-06-06');
Dataset: This uses Sentinel-5P (S5P), a satellite that measures atmospheric gases like formaldehyde.
Filter Dates: The script focuses on data from June 1 to June 6, 2019.
Select HCHO Band: The script selects the tropospheric_HCHO_column_number_density band, which provides formaldehyde measurements.
4. Calculate Mean HCHO Levels
javascript
Copy code
var hchoMean = collection.mean().clip(dataset);
Mean Calculation: Combines all images within the specified date range to compute the average concentration of formaldehyde.
Clipping: Restricts the data to the boundaries of East Africa.
5. Visualization Parameters
javascript
Copy code
var band_viz = {
  min: 0.0,
  max: 0.0003,
  palette: ['black', 'blue', 'purple', 'cyan', 'green', 'yellow', 'red']
};
Value Range: The min and max values define the concentration range for visualization.
Color Palette: Colors represent formaldehyde levels, transitioning from black (low) to red (high).
6. Display HCHO Layer
javascript
Copy code
Map.addLayer(hchoMean, band_viz, 'Formaldehyde (HCHO) over East Africa 2019');
Adds the processed HCHO data as a color-coded layer.
7. Center the Map
javascript
Copy code
Map.centerObject(dataset, 5);
Centers the map on East Africa with a zoom level of 5.
8. Add a Title to the Map
javascript
Copy code
var title = ui.Label('Formaldehyde (HCHO) Concentration 2019 in East Africa', {
  fontSize: '20px',
  backgroundColor: 'white',
  padding: '8px',
  position: 'top-center',
  color: 'black'
});
Map.add(title);
Title: Displays the title at the top of the map for context.
9. Create a Legend
javascript
Copy code
var legend = ui.Panel({
  style: {position: 'bottom-right', padding: '8px'}
});
Legend Panel: A UI panel is created at the bottom-right of the map.
Legend Colors and Values: Each color in the palette corresponds to a formaldehyde concentration value. Labels are added to help interpret the map.
10. Final Output
The map shows:
The boundaries of East African countries.
Color-coded formaldehyde concentration levels for June 2019.
A legend for interpretation.
A title for context.
This script can be modified for different regions, time ranges, or pollutants, making it a versatile tool for atmospheric analysis using satellite data.
