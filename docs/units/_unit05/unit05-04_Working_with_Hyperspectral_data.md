---
order: 4
title: "EX | Working with hyperspectral data in EnMAP-Box"
header:
  image: "/assets/images/teaser/hyp_crop.png"
  caption: 'Image: [**NASA - Hyperion**](https://earthobservatory.nasa.gov/features/EO1Tenth/page3.php){:target="_blank"}'
toc: true
---

In this exercise, we will use EnMAP data in QGIS with the EnMAP-Box plugin.
<!--more-->

For this exercise, use QGIS version 3.34 or newer and the current EnMAP-Box plugin.
{: .notice--info}

## Getting started with EnMAP-Box

EnMAP-Box is a free, open-source QGIS plugin for visualizing and processing hyperspectral data.

For details about EnMAP-Box, see its [website](https://www.enmap.org/data_tools/enmapbox/){:target="_blank"}.

## Working with EnMAP-Box in QGIS

Follow the steps below to install the EnMAP-Box plugin in QGIS.
If you have not used QGIS before, you can [download it](https://www.qgis.org/en/site/forusers/download.html){:target="_blank"} and use its [training manual](https://docs.qgis.org/3.34/en/docs/training_manual/index.html){:target="_blank"}.

- Installing EnMAP-Box
In QGIS, open "Plugins" and then "Manage and Install Plugins". Search for "EnMAP-Box" and install the plugin shown below.

<img src="enamp_box_plugin.png" width="1280" height="755" align="centre" vspace="10" hspace="20">
Image: EnMAP-Box plugin. Source: Netra Bhandari

- Next, in the processing tools menu look for the EnMAP-Box, or search for the icon of EnMAP on the toolbar.

<img src="enamp_box_plugin_icon.png" width="1280" height="755" align="centre" vspace="10" hspace="20">
Image: EnMAP-Box plugin. Source: Netra Bhandari

- You will be prompted with a new window. 
If you are asked to install dependencies, follow the [installation guide](https://enmap-box.readthedocs.io/en/latest/usr_section/usr_installation.html){:target="_blank"}.

<img src="enamp_box_gui.png" width="1280" height="755" align="centre" vspace="10" hspace="20">
Image: EnMAP-Box GUI. Source: Netra Bhandari

- EnMAP-Box features

EnMAP-Box offers a variety of tools and applications. Take time to explore them, and consult the [user manual](https://enmap-box.readthedocs.io/en/latest/usr_section/usr_manual/usr_manual.html){:target="_blank"} for details.
For this unit we largely focus on the following:
	- Map view
	- Spectral library view

## Task 1

EnMAP-Box includes an example dataset. In its window, open "Project" and select "Add Example Data".

You will be prompted by a dialogue box "Download testdata from 
https://github.com/EnMAP-Box/enmap-box-exampledata/releases/download/v1.1/exampledata.zip?" . Click on "Yes". This loads in the example data on your computer.

Once executed, in your data source there will be the following layers: 

	- Raster - "aerial potsdam" (showing an aerial view, a 4 band raster) , "enmap_potsdam" (showing a hyperspectral view, a 224 band raster)
	- Vector - "EnMap Spectral Response Function", "landcover_potsdam_point", "landcover_potsdam_polygon" and "vegetation_fraction"

- Let's load a new map view, and check the layer properties. To get a true color composite you can set your RGB bands as Band 047 (RED), Band 032 (GREEN), and Band 14 (BLUE).

- Next we look at pixel based information.

For this click on 'Identify a cursor location" on the tool bar shown with and arrow followed by "identify cursor location values" as shown in the image below.
To look at the spectral profile click on "identify pixel profiles and show them in a spectral library". 
Now take some time to explore the different tools provided in the spectral library viewer.
 
<img src="enmap_box_pixelinfo_1.png" width="1280" height="755" align="centre" vspace="10" hspace="20">
Image: EnMAP-Box pixel and spectral information. Source: Netra Bhandari

Next we will load a polygon data (vector). 

Select, drag and drop the "landcover_potsdam_polygon" layer in the mapview. 
Let's get the spectral profile for vegetation.


<img src="veg_speclib_1.png" width="1280" height="755" align="centre" vspace="10" hspace="20">
Image: EnMAP-Box vegetation spectral library. Source: Netra Bhandari

Since the spectral library is a vector layer, it has an attribute table, which we can edit. This vector layer can then also be added to the mapview and can be exported as well.

<img src="veg_speclib_att_table_1.png" width="1280" height="755" align="centre" vspace="10" hspace="20">
Image: EnMAP-Box vegetation spectral library attribute table. Source: Netra Bhandari

Based on the above instructions, load and understand the spectral profile of the following surfaces.
- roof
- pavement
- low vegetation
- tree
- soil
- water

Compare the spectral profiles in aerial imagery and EnMAP imagery.

## Task 2

A few EnMAP scenes are provided [here](https://next.hessenbox.de/index.php/f/808342192){:target="_blank"}. You can load files ending with "spectral_image_swir" or "spectral_image_vnir".
Along with this , a shapefile with 3 classes is given as well.

To add an EnMAP product, select **Project → Add product**, then choose the appropriate product (EnMAP L1B for this exercise). Select your metadata file and name the SWIR and VNIR layers, for example, "swir_kili.vrt" and "vnir_kili.vrt".
{: .notice--info}

- What is the spectral resolution of this image?
- Get the spectral profiles (take a screenshot of the spectra ) for each class and also save it as geopackage and ENVI formats.

This unit is for demonstration. We will not use hyperspectral data in the upscaling process because there are too few EnMAP scenes for Kilimanjaro.
{: .notice--info}
