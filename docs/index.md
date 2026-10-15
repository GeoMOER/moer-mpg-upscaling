---
title: Upscaling Biodiversity using LiDAR and hyperspectral remote sensing
layout: splash
date: '2018-02-06 13:00:00 +0100'
header:
  overlay_color: "#000"
  overlay_filter: 0.6
  overlay_image: "/assets/images/title/land_use_map_Kili.png"
  caption: 'Image: [Andreas Hemp (2006)](https://doi.org/10.1111/j.1365-2028.2006.00679.x){:target="_blank"}'
  cta_label: Go to course units
  cta_url: "/units.html"
excerpt: Use upscaling methods and multi-sensor data for predicting biodiversity.
feature_row_intro:
- excerpt: Master level course in [Physical Geography](https://www.uni-marburg.de/de/fb19/studium/studiengaenge/m-sc-physische-geographie/herzlich-willkommen-beim-master-physische-geographie){:target="_blank"} at Marburg University
feature_row_ilos:
- image_path: "/assets/images/envobs_ilos.jpg"
  alt: PC monitor laying in the garden of the institute.
  title: Intended learning outcomes
  excerpt: "Template..."
---

{% include feature_row id="feature_row_intro" type="center" %}

Field data collection for understanding complex forest structures is increasingly being complemented by remote sensing methods. However, many remote sensing methods work best at a particular scale.
For example, Sentinel-2 data captures information at a high resolution of 10 m in 2D, while LiDAR (Light Detection and Ranging) describes forest structure most accurately in 3D.
Joint use of remote sensing data from multiple sensors can help us capture complex vegetation structure at both local and regional levels. Such an understanding can support forest conservation and biodiversity management.
This course uses pre-collected data on species richness and environmental parameters such as temperature and soil, collected at plot level, to upscale biodiversity to a regional scale.

# What will be done in the course 
In this course, we will combine airborne LiDAR, hyperspectral and Sentinel-2 data with satellite embeddings from geospatial foundation models to upscale biodiversity measured at plot scale.
The data used in this course is provided by the [Kilimanjaro-SES project](https://kili-ses.de/){:target="_blank"}.


# Intended learning outcomes
At the end of this course you should be able to

* Use R to process and analyse geospatial data,
* Explain the basics of remote sensing and machine learning,
* Process LiDAR, hyperspectral and Sentinel-2 data,
* Explain what satellite embeddings are and use them as predictors,
* Apply upscaling methods to predict biodiversity at regional scale,
* Combine different types of remote sensing data in a single workflow, and
* Design and carry out a reproducible research project on a real-world example.


# Setting

This course takes place in person in the classroom (**F 14 | 00A19**).
The first session will take place on **Friday 16.10.2026 at 09:15 am**. The instructors will explain the course details then.
Course resources are provided in the [ILIAS course environment](https://ilias.uni-marburg.de/goto.php/crs/4717441){:target="_blank"} (only accessible to course members logged in to ILIAS).
{: .notice--info}


# Syllabus

The course encompasses 13 sessions from 16.10.2026 to 12.02.2027.

| Session | Date | Topic | Content |
|---------|------|-------|---------|
||| **Getting started** |
| 01 | 16.10.2026   | First things first                                        | Course introduction, R Basics, GitHub, Classroom 50 |
||| **Remote sensing basics and data**  |
| 02 | 23.10.2026   | Remote sensing basics and Sentinel-2 data                 |Introduction to Kili-SES project, Remote sensing basics, Google Earth Engine basics, Sentinel-2 data preparation and application|
| 03 | 30.10.2026   | LiDAR remote sensing          	                        |Introduction to LiDAR, working with point clouds and deriving LiDAR products|
| 04 | 06.11.2026   | Hyperspectral remote sensing       			|Introduction to hyperspectral data, speclibs and deriving vegetation indices|
| 05 | 13.11.2026 | Foundation models and satellite embeddings | What foundation models and embeddings are, AlphaEarth and TESSERA, embeddings vs. spectral bands and indices, limitations |
| 06 | 20.11.2026 | Hands-on AlphaEarth satellite embeddings | Accessing AlphaEarth embeddings in Google Earth Engine, exploring and extracting them for the Kilimanjaro plots, comparison with classic predictors |
||| **Start research project**  |
| 07 | 27.11.2026   | Set up your research project      		        | Defining project work, research questions and discussing deadlines, open science and data |
||| **Upscaling Methodology**                                                   |	
| 08 | 04.12.2026 | Upscaling workflow 1                                        | What is upscaling, understanding the workflow, data preparation, basics of machine learning models |
||| **No sessions on 11.12. and 18.12.2026 – work on your project outline** |
||| **Christmas Break**                                                         |
| 09 | 15.01.2027 | Upscaling workflow 2 	                            	| Designing a machine learning model, testing and fine tuning the model |	
| 10 | 22.01.2027 | Upscaling workflow 3	                            	| Upscaling : doing predictions |
||| **Assisted working phase** |
| 11 | 29.01.2027   | Feedback on project outline and Work on projects 		| Discussion on project outlines and time for working on projects|
| 12 | 05.02.2027   | Work on your projects                                     | Time for working on projects, individual feedback |
||| **Wrap up** |
| 13 | 12.02.2027   | Work on your projects, wrap up, evaluation, and feedback  | Time for questions and feedback, goodbye |


# Deliverables

The graded course certificate is based on an individual portfolio hosted as a personal repository on GitHub.
The portfolio items are defined in the respective course assignments.
To create your repository, follow the [Classroom 50 setup steps](/moer-mpg-upscaling/unit00/unit00-01_learning_environment.html#classroom-50-and-github). Use the same repository for all assignments.


# Preparation and prerequisites

The course assumes basic knowledge and skills in R and geo-information science.

This course uses additionally provided material for teaching basic R skills, 
which can be found [here](https://geomoer.github.io/moer-base-r/){:target="_blank"}.
{: .notice--success}

## Team

{% for author in site.data.authors %}
  {% include author-profile.html %}
 <br />
{% endfor %}


