---
order: 4
title: "EX | Sentinel-2 Preparation"
header:
  image: "/assets/images/teaser/ndvi.jpg"
  caption: 'Image: [**Environmental Informatics Marburg**](https://www.uni-marburg.de/en/fb19/disciplines/physisch/environmentalinformatics){:target="_blank"}'
toc: true
---

## Things you need for this exercise
* A shapefile of the study area, buffered to 8000 m (see below)
* The name of the Sentinel-2 image collection: "COPERNICUS/S2_SR_HARMONIZED" (read [here](https://developers.google.com/earth-engine/datasets/catalog/COPERNICUS_S2_SR_HARMONIZED){:target="_blank"})
* A folder in your Google Drive to store the exported image
{: .notice--info}


## Step 1: Upload the study area

Download the buffered shapefile for the southern slopes of Kilimanjaro [here](https://next.hessenbox.de/index.php/f/808342192){:target="_blank"}. We use the buffered area to avoid the error regarding "multiple geometries".

* Open the GEE Code Editor, go to "Assets", click "New" and upload your shapefile with a suitable name (e.g. "study_area_kili").
* Upload the files ".shp", ".shx", ".dbf" and ".prj", or one ".zip" containing them. The file format ".qmd" is not supported.
* Once the shapefile is available as an asset, you can start coding.

## Step 2: Plot an RGB image

Plot an RGB image of the Sentinel-2 scene for the southern slopes of Kilimanjaro on 13 January 2022.

```js
// Define the study area – replace with the path of YOUR uploaded asset (see the Assets tab)
var studyArea = ee.FeatureCollection('projects/YOUR-PROJECT/assets/study_area_kili');
// Import Sentinel-2 data and filter by date and study area
var startDate = '2022-01-13';
var endDate = '2022-01-14'; // One day after the start date
var sentinel2 = ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
                  .filterDate(startDate, endDate)
                  .filterBounds(studyArea);
// Clip the images to the study area
var clippedImage = sentinel2.median().clip(studyArea);
// Visualization parameters for RGB
var visParams = {bands: ['B4', 'B3', 'B2'], min: 0, max: 3000};
// Add the RGB layer to the map
Map.addLayer(clippedImage, visParams, 'RGB of Study Area');
// Optionally, add the study area outline to the map
Map.addLayer(studyArea, {color: 'red'}, 'Study Area');
// Center the map on the study area
Map.centerObject(studyArea, 10); // Adjust zoom level as needed
```

## Step 3: Export the image to Google Drive

```js
// Export the full-band image to your Google Drive
Export.image.toDrive({
  image: clippedImage,
  description: 'Sentinel2_FullBand_Image',
  folder: 'GEE_Folder', // your Google Drive folder name
  fileNamePrefix: 'Sentinel2_FullBand',
  region: studyArea.geometry(),
  crs: 'EPSG:32737', // UTM zone 37S, the projection of our study area
  scale: 10, // Sentinel-2 resolution in metres
  maxPixels: 1e9,
  fileFormat: 'GeoTIFF'
});
```

* When you click "Run", the "Tasks" tab on the right side of the editor turns yellow. Open it and click "RUN" next to your task.
* Confirm the settings in the dialog and start the export. Depending on the size, this can take a few minutes.
* Large images are exported in several tiles. We merge them in the next exercise.
* Tip: if you have many images to export, follow the instructions [here](https://benny.istan.to/blog/20220319-batch-task-execution-in-google-earth-engine-code-editor){:target="_blank"}.

<img src="task_gee.png" width="1280" height="755" align="centre" vspace="10" hspace="20">
