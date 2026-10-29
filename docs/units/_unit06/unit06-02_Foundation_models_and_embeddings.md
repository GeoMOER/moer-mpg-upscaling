---
order: 2
title: "LM | Foundation models and satellite embeddings"
header:
  image: "/assets/images/title/land_use_map_Kili.png"
  caption: 'Image: [Andreas Hemp (2006)](https://doi.org/10.1111/j.1365-2028.2006.00679.x){:target="_blank"}'
toc: true
---

## Why a new kind of remote sensing data?

In the previous units you worked with three very different data sources:

* **Sentinel-2** (Unit 03): you filtered scenes by date, dealt with clouds, and calculated vegetation indices such as NDVI.
* **LiDAR** (Unit 04): you normalised point clouds and derived structural metrics such as canopy height.
* **Hyperspectral data** (Unit 05): you worked with hundreds of narrow bands and spectral libraries.

Each source needed its own preprocessing, and each time *you* decided which features (indices, metrics) might matter for your question. This is called **feature engineering**. It takes time, needs expert knowledge, and the result depends on the choices you make.

Satellite embeddings take a different route: a model looks at many data sources at once and learns *by itself* how to summarise each location.

## What is a foundation model?

A **foundation model** is one large model that is trained once on a huge amount of data, mostly **without labels**, and is then reused for many different tasks.

You already know an example: the large language models behind chatbots. They were trained on enormous amounts of text to predict missing words. Nobody told them what grammar or facts are, yet they learned useful patterns that can then be applied to translation, summaries, or question answering.

Geospatial foundation models do the same with Earth observation data. Instead of predicting missing words, they learn, for example, to reconstruct missing or masked satellite observations. To do this well, the model has to "understand" what a location typically looks like across sensors and seasons.

## From pixels to embeddings

An **embedding** is a list of numbers that describes a location. You can think of it as the model's compressed "summary" of everything it has seen about that place over a year: its spectral signature, its seasonal changes, its structure, its terrain.

The key idea is **similarity**:

* Places with **similar embeddings** look and behave alike (for example, two coffee plantations at similar elevations).
* Places with **very different embeddings** are different (for example, a coffee plantation and a montane forest).

Because each embedding is just a vector of numbers, we can compare locations with simple maths. For unit-length vectors, the **dot product** of two embeddings tells us how similar they are: values close to 1 mean very similar, lower values mean less similar.

Embeddings can then be used like any other predictor: in classification, regression, clustering, or change detection. This is why they are interesting for our upscaling workflow.

## AlphaEarth Foundations

**AlphaEarth Foundations** is a geospatial foundation model developed by Google DeepMind (Brown et al. 2025). It combines many data sources, including

* optical imagery (Sentinel-2, Landsat),
* radar (Sentinel-1),
* spaceborne LiDAR (GEDI),
* climate data, elevation, and other sources.

The resulting **Satellite Embedding dataset** is available in Google Earth Engine:

| Property | Value |
|----------|-------|
| Earth Engine ID | `GOOGLE/SATELLITE_EMBEDDING/V1/ANNUAL` |
| Spatial resolution | 10 m |
| Temporal resolution | one layer per year, from 2017 onwards |
| Bands | 64 bands, named `A00` to `A63` |
| Values | between -1 and 1; each pixel vector has length 1 |
| Licence | CC-BY 4.0 |

So instead of 13 Sentinel-2 bands for one date, you get 64 numbers per 10 m pixel that summarise a whole year from many sensors. Clouds and data gaps are already handled by the model.

## Embeddings vs. spectral bands and indices

| | Spectral bands and indices | Satellite embeddings |
|---|---|---|
| **What a value means** | Physical meaning (e.g. reflectance in the red band; NDVI relates to green biomass) | No physical meaning for a single band; only the full vector is meaningful |
| **Preprocessing** | Cloud masking, compositing, index calculation – done by you | Already done by the model |
| **Data sources** | Usually one sensor at a time | Many sensors combined |
| **Time** | One date or a composite you choose | One summary per year |
| **Interpretability** | High: you can explain *why* a model uses NDVI | Low: hard to explain *why* a model uses `A23` |
| **Reproducibility** | You control every step | Depends on the model version provided by Google |

Neither is "better" in general. Embeddings are often very strong predictors, especially when only few field observations are available. Classic predictors are easier to interpret and to justify ecologically. In the upscaling sessions you will be able to compare both.

## Limitations and responsible use

Before using embeddings in your research project, keep the following in mind:

* **No physical meaning.** Do not interpret single bands like `A12` as "vegetation" or "moisture". Use all 64 bands together.
* **Black box.** The model learned its own features. It is hard to explain which property of the landscape drives a prediction.
* **Training data and bias.** The model was trained on data that are not evenly distributed across the globe. Check whether embeddings work well for *your* region and question rather than assuming they do.
* **Annual summaries.** One embedding per year cannot show short-term events or a specific season you might be interested in.
* **Dependency on a provider.** The dataset is produced and hosted by a company. Future versions may change, so always note the dataset version and year you used.
* **Spatial scale.** Our plots are 50 m × 50 m, while embeddings are 10 m pixels. You will need to aggregate them, just as you did with NDVI.

## Other embedding models: TESSERA

AlphaEarth is not the only option. **TESSERA** (Feng et al. 2025) is an open foundation model developed at the University of Cambridge. It uses only Sentinel-1 and Sentinel-2 time series and produces 128-dimensional embeddings at 10 m resolution. The model, code, and embeddings are openly available and can be downloaded with the Python package `geotessera`.

**Discussion question:** Which would you choose for a scientific study: an embedding from a model that combines many data sources but is maintained by a company, or one that uses fewer data sources but is fully open? What does your choice mean for reproducibility?
{: .notice--info}

## Discussion

Discuss the following questions in small groups:

1. Which steps from Units 03–05 would you no longer need to do if you used embeddings? Which knowledge from those units would still be important?
2. Your model predicts plant species richness well using embeddings, but a reviewer asks *why*. How would you answer?
3. Where on Kilimanjaro might embeddings work less well, and why?

## References and further reading

* Brown, C. F., et al. (2025). AlphaEarth Foundations: An embedding field model for accurate and efficient global mapping from sparse label data. [arXiv:2507.22291](https://arxiv.org/abs/2507.22291){:target="_blank"}
* Feng, Z., et al. (2025). TESSERA: Temporal Embeddings of Surface Spectra for Earth Representation and Analysis. [arXiv:2506.20380](https://arxiv.org/abs/2506.20380){:target="_blank"}
* [Satellite Embedding dataset in the Earth Engine Data Catalog](https://developers.google.com/earth-engine/datasets/catalog/GOOGLE_SATELLITE_EMBEDDING_V1_ANNUAL){:target="_blank"}

<i>The AlphaEarth Foundations Satellite Embedding dataset is produced by Google and Google DeepMind.</i>

## Comments 

You can leave comments below if you have questions or remarks about any of the text or code in this unit. Please copy the corresponding line into your comment to make it easier to answer your questions.

<script src="https://utteranc.es/client.js" 
        repo="GeoMOER/moer-mpg-upscaling"
        issue-term="moer_mpg_upscaling_embeddings_Concepts" 
        theme="github-light" 
        crossorigin="anonymous" 
        async> 
</script> 
