---
order: 3
title: "EX | Exploring AlphaEarth embeddings"
header:
  image: "/assets/images/title/land_use_map_Kili.png"
  caption: 'Image: [Andreas Hemp (2006)](https://doi.org/10.1111/j.1365-2028.2006.00679.x){:target="_blank"}'
toc: true
---

In this exercise, we load the AlphaEarth satellite embeddings for the southern slopes of Kilimanjaro, look at them on the map, and use them to find places that are similar to one of our plots.

## Things you need for this exercise
* Your study area asset from the Sentinel-2 exercise (e.g. "study_area_kili")
* The plot shapefile `BPolygon.shp`, uploaded as a GEE asset (e.g. "kili_plots"). Upload it the same way as the study area: "Assets" → "New" → "Shape files".
* The name of the dataset: "GOOGLE/SATELLITE_EMBEDDING/V1/ANNUAL" (read [here](https://developers.google.com/earth-engine/datasets/catalog/GOOGLE_SATELLITE_EMBEDDING_V1_ANNUAL){:target="_blank"})
{: .notice--info}

## Step 1: Load the embeddings

The dataset is an image collection with one image per year and tile. We filter it by year and study area and then merge the tiles into one image with `mosaic()`.

Choose the year that matches your plot data. Here we use 2022, the same year as our Sentinel-2 scene.

```js
// Replace with the paths of YOUR uploaded assets (see the Assets tab)
var studyArea = ee.FeatureCollection('projects/YOUR-PROJECT/assets/study_area_kili');
var plots = ee.FeatureCollection('projects/YOUR-PROJECT/assets/kili_plots');

// Choose the year
var year = 2022;

// Load the satellite embeddings for that year
var embeddings = ee.ImageCollection('GOOGLE/SATELLITE_EMBEDDING/V1/ANNUAL')
                   .filterDate(year + '-01-01', (year + 1) + '-01-01')
                   .filterBounds(studyArea)
                   .mosaic()
                   .clip(studyArea);

// Look at the band names
print('Band names:', embeddings.bandNames());
```

Check the console: you should see 64 bands, from `A00` to `A63`.

## Step 2: Display embeddings as an RGB image

We cannot look at 64 bands at once, but we can display any three of them as red, green, and blue. The colours have **no physical meaning**. They only show where the embeddings are similar (similar colour) or different (different colour).

```js
Map.centerObject(studyArea, 11);

// Show three embedding bands as RGB
var visParams = {min: -0.3, max: 0.3, bands: ['A01', 'A16', 'A09']};
Map.addLayer(embeddings, visParams, 'Embeddings (A01, A16, A09)');

// Add the plots on top
Map.addLayer(plots, {color: 'yellow'}, 'Plots');
```

**Try it:**
* Change the three bands, for example to `['A05', 'A30', 'A50']`. Which landscape features stand out now?
* Add the true-colour Sentinel-2 image from Unit 03 and switch between the layers. Can you recognise the elevation zones of Kilimanjaro, from savanna and farmland to montane forest?
{: .notice--info}

## Step 3: Similarity search

Now we use the embeddings to answer a simple question: **which places look like my plot?**

1. Pick one plot and calculate its mean embedding.
2. Calculate the dot product between this mean embedding and every pixel.
3. High values mean "similar to my plot".

Pick a plot ID from your plot table. Here we use a lower montane forest plot, `flm1`.

```js
// Choose a reference plot (check the PlotID column of your asset)
var refPlot = plots.filter(ee.Filter.eq('PlotID', 'flm1')).first();
Map.addLayer(ee.FeatureCollection([refPlot]), {color: 'red'}, 'Reference plot');

// Mean embedding of the reference plot
var bandNames = embeddings.bandNames();
var refVector = embeddings.reduceRegion({
  reducer: ee.Reducer.mean(),
  geometry: refPlot.geometry(),
  scale: 10
});

// Turn the mean embedding into a constant image with 64 bands
var refImage = refVector.toImage(bandNames);

// Dot product: multiply band by band and sum up
var similarity = embeddings.multiply(refImage)
                           .reduce(ee.Reducer.sum())
                           .rename('similarity');

Map.addLayer(similarity,
             {min: 0, max: 1, palette: ['white', 'yellow', 'darkgreen']},
             'Similarity to reference plot');
```

The mean of several pixels is slightly shorter than length 1, so the highest values will be a little below 1. This does not matter here, because we only compare places with each other.

**Try it:**
* Where are the most similar places? Do they match the land-use map in the header of this page?
* Repeat the search with a plot of a different land-use type, for example a coffee plantation or a savanna plot.
* Change the year. Do you see places that changed?
{: .notice--info}

## Step 4: Save your script

Save your script (e.g. "embeddings_kili"). We continue in the same script in the next exercise.

## Comments 

You can leave comments below if you have questions or remarks about any of the text or code in this unit. Please copy the corresponding line into your comment to make it easier to answer your questions.

<script src="https://utteranc.es/client.js" 
        repo="GeoMOER/moer-mpg-upscaling"
        issue-term="moer_mpg_upscaling_embeddings_Exploring" 
        theme="github-light" 
        crossorigin="anonymous" 
        async> 
</script> 
