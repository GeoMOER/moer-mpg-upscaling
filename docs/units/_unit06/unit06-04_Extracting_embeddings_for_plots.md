---
order: 4
title: "EX | Extracting embeddings for the plots"
header:
  image: "/assets/images/title/land_use_map_Kili.png"
  caption: 'Image: [Andreas Hemp (2006)](https://doi.org/10.1111/j.1365-2028.2006.00679.x){:target="_blank"}'
toc: true
---

In this exercise, we extract the mean embedding for each Kilimanjaro plot, export it as a table, and take a first look at it in R. This table becomes an extra set of predictors for the upscaling workflow in Unit 08.

Along with the embeddings, we also extract two classic predictors, **NDVI** and **elevation**, so we can compare both kinds of data.

## Step 1: Extract the embeddings for all plots in GEE

Continue in the script from the previous exercise. There we calculated the fingerprint of **one** plot with `reduceRegion`. Now we do the same for **all** plots at once.

### 1a. Add NDVI and elevation

Each plot is 50 m × 50 m and covers about 25 embedding pixels. Before we average them, we add two classic predictors as extra bands, so everything is extracted in one go.

```js
// NDVI from a cloud-free Sentinel-2 median of 2022
var ndvi = ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
             .filterDate('2022-01-01', '2023-01-01')
             .filterBounds(studyArea)
             .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 20))
             .median()
             .normalizedDifference(['B8', 'B4'])   // (NIR - Red) / (NIR + Red)
             .rename('NDVI');

// Elevation from the SRTM digital elevation model (30 m)
var elevation = ee.Image('USGS/SRTMGL1_003').rename('elevation');

// Put everything into one image: 64 embedding bands + NDVI + elevation
var predictors = embeddings.addBands(ndvi).addBands(elevation);
```

| Command | What it does |
|---|---|
| `.normalizedDifference(['B8', 'B4'])` | Calculates NDVI for every pixel, as in Unit 03. |
| `ee.Image('USGS/SRTMGL1_003')` | Loads a global elevation model from the Earth Engine catalogue. It has one band, which we rename to `elevation`. |
| `.addBands(...)` | Adds the bands of another image. Our image now has 64 + 2 = 66 bands. |

### 1b. Mean values for every plot

```js
// Mean of every band for every plot
var plotValues = predictors.reduceRegions({
  collection: plots,
  reducer: ee.Reducer.mean(),
  scale: 10
});

print('First plot:', plotValues.first());
```

| Part | What it does |
|---|---|
| `reduceRegions` | Like `reduceRegion` from the last exercise, but for **many areas at once**. Note the **s**: one region → one result; many regions → one result per region. |
| `collection: plots` | **Where**: in every plot. The result is the plot table with 66 new columns. |
| `reducer: ee.Reducer.mean()` | **How**: the average of all pixels inside each plot. |
| `scale: 10` | The pixel size in metres. Elevation (30 m) is resampled to 10 m for this. |

Look at the first plot in the Console: next to `PlotID`, it now has the columns `A00` to `A63`, `NDVI` and `elevation`.

### 1c. Export the table

```js
// The names of the 64 embedding bands, as a normal JavaScript list
var bands = embeddings.bandNames().getInfo();

// Export the table to Google Drive
Export.table.toDrive({
  collection: plotValues,
  description: 'kili_plot_embeddings_2022',
  folder: 'GEE_Folder',                           // your Google Drive folder name
  fileNamePrefix: 'kili_plot_embeddings_2022',
  fileFormat: 'CSV',
  selectors: ['PlotID', 'NDVI', 'elevation'].concat(bands)
});
```

| Part | What it does |
|---|---|
| `bandNames().getInfo()` | Gets the band names `A00` … `A63` as a normal list. `getInfo()` is needed because `selectors` expects a normal JavaScript list, not an Earth Engine list. |
| `description` / `fileNamePrefix` | The name of the task in the Tasks tab and of the file in your Drive. |
| `selectors` | Which columns to export, in this order. Without it, Earth Engine also exports the plot geometry and other columns we don't need. |
| `['PlotID', 'NDVI', 'elevation'].concat(bands)` | Joins two lists: our three column names plus the 64 band names. |

Go to the **Tasks** tab and click **Run**. When the task is finished, download the CSV file from your Google Drive into your project folder.

<details markdown="1">
<summary><b>Another way to build the band names: a loop</b></summary>

You may see this code in other scripts. It builds the same list without asking Earth Engine:

```js
var bands = [];
for (var i = 0; i < 64; i++) {             // repeat for i = 0, 1, … 63
  bands.push(i < 10 ? 'A0' + i : 'A' + i); // add a leading zero below 10: A05, but A23
}
```

`i < 10 ? 'A0' + i : 'A' + i` is a short if-else: "if i is smaller than 10, write A0 + i, otherwise A + i". In R, the same list is `sprintf("A%02d", 0:63)`.
</details>


## Step 2: Load the embeddings in R

```r
library(dplyr)
library(ggplot2)

emb <- read.csv("E:/upscaling_course_WS_26/kili_plot_embeddings_2022.csv")  # adjust the path

dim(emb)            # number of plots, number of columns (PlotID + NDVI + elevation + 64)
head(emb[, 1:6])

# the embedding columns
emb_cols <- grep("^A[0-9]{2}$", names(emb), value = TRUE)
length(emb_cols)    # should be 64

# check for plots without values (e.g. outside the dataset coverage)
sum(!complete.cases(emb[, emb_cols]))
```

| Part | What it does |
|---|---|
| `grep("^A[0-9]{2}$", names(emb), value = TRUE)` | Finds all column names that look like "A + two digits": `^` = start of the name, `A`, `[0-9]{2}` = exactly two digits, `$` = end of the name. So `A05` matches, but `PlotID` and `NDVI` do not. |
| `complete.cases(...)` | `TRUE` for rows without missing values. `sum(!...)` counts the rows **with** missing values. It should be 0. |


## Step 3: Do similar plots have similar embeddings?

### What is a PCA?

Each plot is described by 64 numbers. You can think of every plot as a point in a space with 64 axes, but we can only draw two or three axes.

A **principal component analysis (PCA)** rotates this space to find the **directions in which the plots differ most**:

* **PC1** (the first principal component) is the direction along which the plots are most spread out.
* **PC2** is the next most spread-out direction, at right angles to PC1, and so on, up to PC64.

Each principal component is a combination of all 64 bands. By plotting PC1 against PC2, we get the **best possible 2D picture** of how the plots relate to each other.

**Think of a shadow:** if you hold a 3D object up to a lamp, its shadow is a 2D picture of it. A PCA turns the object until the shadow shows as much of its shape as possible.
{: .notice--info}

### Why do we do a PCA here?

1. **To see the data.** 64 columns are impossible to look at; a PCA plot shows all plots at once.
2. **To check the embeddings.** If they capture the landscape well, plots of the same land-use type should lie close together.
3. **To find out how much information the 64 bands really contain.** Many bands vary together. The PCA tells us how many independent "directions" we actually need.

### Run the PCA

The first three letters of the PlotID tell us the land-use type (for example `cof` for coffee plantations or `flm` for lower montane forest).

```r
emb$landuse <- substr(emb$PlotID, 1, 3)   # e.g. "cof1" -> "cof"

pca <- prcomp(emb[, emb_cols])            # PCA of the 64 embedding columns
summary(pca)                              # how much variance does each axis explain?
```

### What does "variance explained" mean?

`summary(pca)` shows, for each axis, the **Proportion of Variance**: the share of all differences between the plots that lies along that axis. **Cumulative Proportion** adds them up. In our data:

| Axes | Variance | Cumulative | Meaning |
|---|---|---|---|
| PC1 | 30.9% | 30.9% | the biggest single difference between plots |
| PC2 | 27.0% | **57.9%** | a PC1–PC2 plot shows **58%** of all differences |
| PC1–PC6 | | 81.6% | six axes hold four-fifths |
| PC1–PC13 | | 95.0% | |
| PC14–PC64 | | 5.0% | the last 51 axes together hold almost nothing |

**What this tells us:**
* The 64 bands are **highly redundant** for our plots: 13 axes capture 95% of the differences.
* The 2D picture is a good but **incomplete** summary. Two plots that look close in it can still differ along PC3, PC4 and further axes.
* We have about 60 plots and 64 predictors. A model can easily **overfit** with so many predictors. Using the first 10–15 principal components instead of all 64 bands is one way to reduce that risk.

### Plot the plots in embedding space

To make the groups easier to see, we combine the land-use types into four zones and draw an ellipse around each zone. For this we use the package `ggforce`.

```r
# install.packages("ggforce")   # only once
library(ggforce)

# PCA scores of the plots, plus their land-use type
scores <- data.frame(pca$x[, 1:2], landuse = emb$landuse)

# group the land-use types into zones
scores <- scores |>
  mutate(zone = case_when(
    landuse %in% c("flm", "foc", "fod", "fpo", "fpd") ~ "Montane forest",
    landuse %in% c("fer", "fed", "hel")               ~ "Erica & Helichrysum zone",
    landuse %in% c("cof", "hom")                      ~ "Coffee & homegardens",
    landuse %in% c("mai", "sav", "gra")               ~ "Lowland: maize, savanna, grassland"
  ))

table(scores$zone, useNA = "ifany")   # every plot should have a zone (no NA)

# share of variance on each axis, for the axis labels
var_exp <- round(100 * summary(pca)$importance[2, 1:2], 1)

ggplot(scores, aes(PC1, PC2)) +
  geom_mark_ellipse(aes(fill = zone, label = zone),
                    alpha = 0.08, colour = "grey50",
                    expand = unit(3, "mm"), label.fontsize = 9,
                    con.cap = 0, show.legend = FALSE) +
  geom_point(aes(colour = landuse), size = 3) +
  labs(title = "Plots in embedding space (PCA)",
       x = paste0("PC1 (", var_exp[1], "% of variance)"),
       y = paste0("PC2 (", var_exp[2], "% of variance)"),
       colour = "Land use") +
  theme_minimal()
```

| Part | What it does |
|---|---|
| `pca$x[, 1:2]` | The position of every plot on PC1 and PC2 (its "scores"). |
| `case_when(...)` | Like a list of if-else rules: if the land use is one of these codes, the zone is "Montane forest", and so on. |
| `geom_mark_ellipse(...)` | Draws a labelled ellipse around the plots of each zone. |
| `geom_point(...)` | Draws the plots, coloured by land-use type. |

**Error `could not find function "from_theme"`?** Your `ggforce` is newer than your `ggplot2`. Update ggplot2 with `install.packages("ggplot2")` and restart R (Session → Restart R). If that does not help, replace the `geom_mark_ellipse(...)` lines with `stat_ellipse(aes(group = zone, fill = zone), geom = "polygon", alpha = 0.1, colour = "grey50")`, which needs no extra package.
{: .notice--warning}

<img src="pca_embeddings.png" width="960" height="800" align="centre" vspace="10" hspace="20">
<i>The Kilimanjaro plots in embedding space: the first two principal components of the 2022 AlphaEarth embeddings, coloured by land-use type.</i>

**Questions:**
* Do plots of the same land-use type group together?
* Which land-use types overlap? Does that make sense ecologically?
* What could PC1 and PC2 stand for? Look at which plots lie on the left and right, at the top and bottom.
{: .notice--info}

<details markdown="1">
<summary><b>Show what the plot tells us</b></summary>

* **Plots of the same land-use type group together**, and the groups form four zones: montane forests, the *Erica* and *Helichrysum* zone, coffee and homegardens, and the lowland maize, savanna and grassland plots.
* **Overlaps make sense ecologically:** coffee and homegardens lie close together (coffee is often grown in homegardens), and so do the different forest types. One homegarden plot lies next to the forest plots: probably a very tree-rich homegarden.
* **PC1 (left to right)** runs from the high-elevation plots (*Erica*, *Helichrysum*, montane forest) on the left to the lowland plots (farmland, savanna) on the right. It probably reflects the **elevation and climate gradient**.
* **PC2 (top to bottom)** runs from tree-covered plots (forests, coffee, homegardens) at the top to open vegetation (savanna, maize, grassland, *Helichrysum*) at the bottom. It probably reflects **tree cover or vegetation structure**.

This is an interpretation, not yet a proof. We test it in Step 4.
</details>


## Step 4: Embeddings vs. classic predictors

The CSV already contains NDVI and elevation for every plot. We now check whether the embeddings capture these known gradients.

```r
# add the classic predictors to the PCA scores (same row order as emb)
scores$NDVI      <- emb$NDVI
scores$elevation <- emb$elevation

# Does PC1 follow elevation? Does PC2 follow NDVI?
cor(scores$PC1, scores$elevation)
cor(scores$PC2, scores$NDVI)

ggplot(scores, aes(elevation, PC1, colour = zone)) +
  geom_point(size = 3) +
  theme_minimal() +
  labs(x = "Elevation (m)", y = "PC1", colour = "Zone")

ggplot(scores, aes(NDVI, PC2, colour = zone)) +
  geom_point(size = 3) +
  theme_minimal() +
  labs(x = "NDVI", y = "PC2", colour = "Zone")

# correlation of each single embedding band with NDVI
cors <- sapply(emb_cols, function(b) cor(emb[[b]], emb$NDVI))

# the five bands with the strongest correlation (positive or negative)
head(sort(abs(cors), decreasing = TRUE), 5)
```

| Part | What it does |
|---|---|
| `cor(x, y)` | The correlation between two variables: close to 1 or −1 = strong relationship, close to 0 = no linear relationship. |
| `sapply(emb_cols, function(b) ...)` | Repeats the correlation for every one of the 64 bands and collects the results. |
| `sort(abs(cors), decreasing = TRUE)` | Sorts the bands by the strength of the correlation, ignoring the sign. |

**Questions:**
* Is PC1 strongly related to elevation, and PC2 to NDVI? Does that confirm the interpretation from Step 3?
* Is there a single band that strongly correlates with NDVI? What does that tell you, and what does it *not* tell you?
* What information might the embeddings contain that NDVI and elevation do not?
{: .notice--info}

<details markdown="1">
<summary><b>Show hints for the answers</b></summary>

* If the correlations are strong (for example above 0.7 or below −0.7), the main axes of the embeddings reflect gradients we already know. That is an **indirect way to explain** what the embeddings capture, and a good answer to the reviewer question from the discussion.
* A single band can correlate strongly with NDVI. But that does **not** mean this band "is" NDVI or measures greenness. Bands have no physical meaning, and the same information is spread across many bands.
* Embeddings combine many sensors and a **whole year**: seasonal changes, radar information on structure and moisture, and LiDAR information on canopy height. A single NDVI value from a median image contains none of this.
</details>

**Did you know?** In the upscaling units, you will be able to compare three models: one with classic predictors (NDVI, elevation, …), one with embeddings only, and one with both. This exercise is the first step towards that comparison.
{: .notice--success}


## Step 5: Save the predictor table

Save the table so you can use it in the upscaling workflow in Unit 08.

```r
write.csv(emb[, c("PlotID", "NDVI", "elevation", emb_cols)],
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
