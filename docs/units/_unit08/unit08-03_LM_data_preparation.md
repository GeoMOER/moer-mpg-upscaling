---
order: 3
title: "LM | Data preparation"
header:
  image: "/assets/images/teaser/unit7.jpg"
  caption: 'Image: [**WHC UNESCO**](https://whc.unesco.org/en/list/403/){:target="_blank"}'
toc: true
---

## Data preparation 

In this part, you will use your previously acquired skills to prepare data for designing a suitable model for predicting plant species richness.
To predict plant species richness, we need a set of predictors. An important note to keep in mind is that we will use these predictors for upscaling, thus we choose only those variables for which we can find a suitable remote sensing proxy. 

In the data preparation task, you will:

* Think of possible factors that influence the plant species richness in your study region (Hint: do a quick google search)
* Get a dataframe ready with all the predictors at plot level
* Perform data cleaning to remove any missing values


<img src="data_prep.png" width="1500" height="500" align="centre" vspace="10" hspace="20">
Source: Netra Bhandari


## Predictors for plant richness
Plant richness and abundance is an indicator of multiple Nature's Contributions to people, such as carbon storage, habitat creation and maintenance (regulating NCPs), supplying learning and inspiration (non-material NCPs) and providing timber and fuelwood (material NCPs).

Some of the important drivers of plant richness are elevation, mean minimum temperature, NDVI (as a proxy for net primary productivity), pH, slope and aspect. Land use intensity is also a major driver of plant richness (Peters et al 2019). However, we limit our study in this exercise to only those drivers for which previously known remote sensing proxies are available.

## Predictors and response put together

<img src="pred_response.png" width="1500" height="500" align="centre" vspace="10" hspace="20">
Source: Netra Bhandari