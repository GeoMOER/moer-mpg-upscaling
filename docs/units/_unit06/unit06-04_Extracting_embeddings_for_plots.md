---
order: 4
title: "EX | Extracting embeddings for the plots"
header:
  image: "/assets/images/title/land_use_map_Kili.png"
  caption: 'Image: [Andreas Hemp (2006)](https://doi.org/10.1111/j.1365-2028.2006.00679.x){:target="_blank"}'
toc: true
---

In this exercise, we extract the mean embedding for each Kilimanjaro plot, export it as a table, and take a first look at it in R. This table becomes an extra set of predictors for the upscaling workflow in Unit 08.

## Step 1: Extract the embeddings for all plots in GEE

Continue in the script from the previous exercise. Each plot is 50 m × 50 m and covers about 25 embedding pixels, so we take the **mean** of all pixels inside each plot, just as we did with NDVI.

```js
// Mean embedding for every plot
var plotEmbeddings = embeddings.reduceRegions({
  collection: plots,
  reducer: ee.Reducer.mean(),
  scale: 10
});

print('First plot:', plotEmbeddings.first());

// Build the list of band names A00 ... A63 for the export
var bands = [];
for (var i = 0; i < 64; i++) {
  bands.push(i < 10 ? 'A0' + i : 'A' + i);
}

// Export the table to Google Drive
Export.table.toDrive({
  collection: plotEmbeddings,
  description: 'kili_plot_embeddings_' + year,
  folder: 'GEE_Folder', // specify your drive folder name
  fileNamePrefix: 'kili_plot_embeddings_' + year,
  fileFormat: 'CSV',
  selectors: ['PlotID'].concat(bands)
});
```

Go to the "Tasks" tab and click "Run". When the task is finished, download the CSV file from your Google Drive into your project folder.

## Step 2: Load the embeddings in R

```r
library(dplyr)
library(ggplot2)

emb <- read.csv("kili_plot_embeddings_2022.csv")

dim(emb)   # one row per plot, PlotID + 64 embedding columns
head(emb[, 1:6])

# the embedding columns
emb_cols <- grep("^A[0-9]{2}$", names(emb), value = TRUE)

# check for plots without values (e.g. outside the dataset coverage)
sum(!complete.cases(emb[, emb_cols]))
```

## Step 3: Do similar plots have similar embeddings?

64 columns are hard to look at. A principal component analysis (PCA) reduces them to a few axes that we can plot. If the embeddings capture the landscape well, plots of the same land-use type should group together.

The first three letters of the PlotID tell us the land-use type (for example `cof` for coffee plantations or `flm` for lower montane forest).

```r
emb$landuse <- substr(emb$PlotID, 1, 3)

pca <- prcomp(emb[, emb_cols])
summary(pca)   # how much variance do the first axes explain?

pca_df <- data.frame(PlotID = emb$PlotID,
                     landuse = emb$landuse,
                     PC1 = pca$x[, 1],
                     PC2 = pca$x[, 2])

ggplot(pca_df, aes(PC1, PC2, colour = landuse)) +
  geom_point(size = 3) +
  theme_minimal() +
  labs(title = "Plots in embedding space (PCA)",
       colour = "Land use")
```

**Questions:**
* Do plots of the same land-use type group together?
* Which land-use types overlap? Does that make sense ecologically?
{: .notice--info}

## Step 4: Embeddings vs. classic predictors

Now compare the embeddings with a predictor you already know. Load the plot-level NDVI from Unit 03 (or any other plot-level predictor you calculated, such as a LiDAR metric) and join it by PlotID.

```r
# replace with your own file and column names
ndvi <- read.csv("plot_ndvi.csv")   # columns: PlotID, mean_ndvi

emb_ndvi <- emb %>% inner_join(ndvi, by = "PlotID")

# correlation of each embedding band with NDVI
cors <- sapply(emb_cols, function(b) cor(emb_ndvi[[b]], emb_ndvi$mean_ndvi))

# the five bands with the strongest correlation
head(sort(abs(cors), decreasing = TRUE), 5)

# and NDVI against the first principal component
ggplot(data.frame(PC1 = pca$x[match(emb_ndvi$PlotID, emb$PlotID), 1],
                  NDVI = emb_ndvi$mean_ndvi),
       aes(PC1, NDVI)) +
  geom_point() +
  theme_minimal()
```

**Questions:**
* Is there a single band that strongly correlates with NDVI? What does that tell you, and what does it *not* tell you?
* What information might the embeddings contain that NDVI does not?
{: .notice--info}

## Step 5: Save the predictor table

Save the table so you can use it in the upscaling workflow in Unit 08.

```r
write.csv(emb[, c("PlotID", emb_cols)],
          "plot_embeddings_2022.csv", row.names = FALSE)
```

<i>The AlphaEarth Foundations Satellite Embedding dataset is produced by Google and Google DeepMind.</i>

## Comments 

You can leave comments below if you have questions or remarks about any of the text or code in this unit. Please copy the corresponding line into your comment to make it easier to answer your questions.

<script src="https://utteranc.es/client.js" 
        repo="GeoMOER/moer-mpg-upscaling"
        issue-term="moer_mpg_upscaling_embeddings_Extracting" 
        theme="github-light" 
        crossorigin="anonymous" 
        async> 
</script> 
