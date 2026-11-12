---
order: 2
title: "LM | Hyperspectral Remote Sensing"
header:
  image: "/assets/images/teaser/hyp_crop.png"
  caption: 'Image: [**NASA - Hyperion**](https://earthobservatory.nasa.gov/features/EO1Tenth/page3.php){:target="_blank"}'
toc: true
---

In this unit we introduce Hyperspectral Remote Sensing followed by a practical exercise on deriving useful information from spectral data.
<!--more-->

## The EM Spectrum

The EM spectrum consists of a range of all types of electromagnetic radiation. Surface reflectance at different parts of the spectrum or wavelengths are used for identification of properties of earth surface. This variation in the reflectances allows us to differentiate between different surfaces such as forest or water body. For optical remote sensing, we generally use the wavelengths in the visible to infrared range. Optical sensors record the sun radiations reflected from different surfaces on earth along various wavelength intervals. Some may be wide and some may be narrow, thus defining their spectral resolution.

<img src="Electromagnetic_spectrum_-eng.svg.png" width="1280" height="755" align="centre" vspace="10" hspace="20">
Electromagnetic Wave Spectrum. Horst Frank / Phrood / Anony - Horst Frank, Jailbird and Phrood [CC BY-SA 4.0]  via 
[wikimedia.org](https://commons.wikimedia.org/w/index.php?curid=3726606#/media/File:Electromagnetic_spectrum_-eng.svg){:target="_blank"}. 

## What is Hyperspectral RS?

Imaging spectroscopy, or hyperspectral remote sensing, acquires images that contain many contiguous, narrow bands, acquired by airborne or spaceborne sensors, resulting in a continuous spectrum for every pixel. Each band can be as narrow as 5–10 nm, whereas the bands of multispectral sensors like Landsat or Sentinel-2 are typically tens to hundreds of nanometres wide.
Such a spectrum aids in the assessment, identification and quantification of different surfaces on earth.

<img src="spectrum.png" width="1104" height="359" align="centre" vspace="10" hspace="20">
 
Spectrum representation including (A) Multispectral example, with 5 wide bands; and (B) Hyperspectral example consisting of several narrow bands that, usually, extends to hundreds or thousands of them (image not drawn to scale).
Image: [Adão et al 2017](https://www.mdpi.com/2072-4292/9/11/1110#){:target="_blank"}.

## Understanding a spectrum

Hyperspectral imagers use instruments called imaging spectrometers, which work on the principles of spectroscopy. The fundamental property we want to study is the spectral reflectance defined as the ratio of reflected energy to incident energy as a function of wavelength. 
Different surfaces will thus have different reflectances, depending on how the incident radiations are absorbed and scattered. 
Such differences become evident when we plot reflectance vs wavelengths, aka spectral reflectance curves. These curves contain absorption features that act like "spectral fingerprints" of surface materials.
Let's look at the example below to understand a plant spectrum. 

<img src="plant_spectrum.png" width="1104" height="359" align="centre" vspace="10" hspace="20">
Reflectance spectra of different types of green vegetation compared to a spectral signature for senescent leaves.
Image: Govender et al. (2007), reproduced in [Adão et al. 2017](https://www.mdpi.com/2072-4292/9/11/1110#){:target="_blank"}.

## Multispectral vs Hyperspectral

<table>
<tr>
    <th style="background-color: lightgrey">Multispectral</th>
    <th style="background-color: lightgrey">Hyperspectral</th>
</tr>
  <tr>
    <td>Many sensors including Landsat, Sentinel-2 and MODIS</td>
    <td>Fewer sensors, but their number is growing fast: public missions (EnMAP, PRISMA), instruments on the ISS (EMIT, DESIS), commercial satellites (e.g. Tanager, Pixxel) and many airborne and UAV sensors</td>
  </tr>
  <tr>
    <td>A limited number of broad spectral bands</td>
    <td>Hundreds of narrow, contiguous bands that measure the characteristics of surface reflectance</td>
  </tr>
<tr>
    <td>Temporal resolution is relatively high</td>
    <td>Lower temporal resolution; images are often acquired on request</td>
  </tr>
<tr>
    <td>Global, regular coverage</td>
    <td>Smaller spatial coverage, no regular global coverage (yet)</td>
  </tr>
</table>

## Applications of hyperspectral RS

Hyperspectral RS has numerous applications in the field of agriculture, soil sciences, hydrology, marine sciences, biodiversity and conservation, hazard and risk assessment, natural resource management and, increasingly, the detection of greenhouse gas emissions.

Listen to an expert!
<iframe width="1280" height="720" src="https://www.youtube.com/embed/g6zSSjBK1Vk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## EnMAP – the German hyperspectral satellite

EnMAP (Environmental Mapping and Analysis Program) is a German Earth observation satellite, launched on 1 April 2022, that uses imaging spectroscopy to characterize the Earth's surface and record environmental changes.
The mission is managed by the German Space Agency at DLR, the scientific lead is with the GFZ Helmholtz Centre for Geosciences in Potsdam, and the satellite was built by OHB.

<img src="Enmap.jpg" width="570" height="313" align="centre" vspace="10" hspace="20">

EnMAP - the first German hyperspectral sensor in space. Image: [DLR](https://www.enmap.org/){:target="_blank"}.

### EnMAP in numbers

EnMAP records **246 adjacent spectral bands** between 420 and 2450 nm with two spectrometers: one for the visible and near infrared (VNIR) and one for the shortwave infrared (SWIR). The tables below explain what each technical characteristic means.

#### Orbit: where and when EnMAP flies

| Characteristic | Value | What it means |
|---|---|---|
| Orbit / inclination | Sun-synchronous, 97.96° | EnMAP circles the Earth almost over the poles. It passes over every place at the **same local time of day**, so the light conditions are similar and images from different dates can be compared. |
| Equator crossing time | 11:00 ± 18 min (local time) | Images are taken in the late morning: plenty of sunlight, and usually fewer clouds than in the afternoon, especially in the tropics. |
| Revisit time | 27 days (looking straight down, ≤ 5°) / 4 days (tilted, ≤ 30°) | Looking straight down, EnMAP sees the same place only every 27 days. By **tilting sideways** up to 30°, like turning your head to look out of a car window, it can observe a place every 4 days. The downside: from the side, the spectra change slightly, because shadows and the mix of leaves, branches and soil look different. |

#### Instrument: what EnMAP can measure

| Characteristic | VNIR | SWIR | What it means |
|---|---|---|---|
| Spectral range | 420–1000 nm | 900–2450 nm | VNIR covers the colours we see plus the near infrared, where vegetation reflects strongly. SWIR is important for leaf water, minerals, soils and dry vegetation. The ranges overlap slightly, so the spectrum has no gap. |
| Spectral sampling interval | 6.5 nm | 10 nm | The **distance between two neighbouring bands**. A new band every 6.5 or 10 nm gives 246 bands in total; Sentinel-2 has 13. |
| Spectral bandwidth (FWHM) | 8.1 ± 1.0 nm | 12.5 ± 1.5 nm | **How wide each band is.** The bands are slightly wider than their spacing, so neighbours overlap and no part of the spectrum is missed. FWHM ("full width at half maximum") is the technical way to measure this width. |
| Signal-to-noise ratio (SNR) | > 400:1 at 495 nm | > 170:1 at 2200 nm | **How clear the measurement is**: real signal compared with random noise, like a voice versus background crackle on a phone call. It is lower in the SWIR, because the sun delivers much less light there. |
| Spectral calibration accuracy | 0.5 nm | 1 nm | **How precisely we know which wavelength each band measures.** This matters because many absorption features ("spectral fingerprints") are very narrow. |
| Ground sampling distance | 30 m | 30 m | One pixel covers **30 × 30 m** on the ground, like Landsat; Sentinel-2 has 10 m pixels. |
| Swath width | 30 km | 30 km | Each image strip is **30 km wide**. Sentinel-2's strips are about 290 km wide: EnMAP sees a narrow strip in great spectral detail, Sentinel-2 a wide strip in fewer colours. |
| Acquisition length | 1000 km per orbit, 5000 km per day | | EnMAP **cannot record all the time**, mainly because of the energy it needs and the amount of data it can send to the ground. That is why images are taken **on request**, not continuously like Sentinel-2. |

Source: [EnMAP mission brochure, DLR/GFZ](https://www.enmap.org/){:target="_blank"}

**Take-home message:** EnMAP sees the Earth in great spectral detail (246 bands), but in narrow strips and only on request. Sentinel-2 sees wide areas regularly, but with few bands. This is why only a few EnMAP scenes exist for Kilimanjaro, and why we do not use them for upscaling in this course.
{: .notice--info}

**Open data policy:** EnMAP data, the EnMAP-Box software and the online training materials are provided free of charge.

### What can EnMAP see?

Because each pixel contains a full spectrum, EnMAP can identify materials by their absorption features, e.g.

* **minerals** in rocks and soils (e.g. clay minerals, iron content),
* **vegetation** properties such as leaf pigments, water content and dry, non-photosynthetic vegetation,
* **water quality**, e.g. chlorophyll and phycocyanin (a pigment of cyanobacteria) in lakes and coastal waters.

### EnMAP today

After four years in orbit, EnMAP is a mature mission with more than 5,000 registered users, and DLR aims to continue it. Its data are used for raw material exploration, agriculture and food security, climate monitoring, including methane point sources, and resource management ([DLR, March 2026](https://www.dlr.de/de/aktuelles/nachrichten/2026/vierjaehriges-bestehen-der-deutschen-satellitenmission-enmap){:target="_blank"}).

## Other hyperspectral imagers

#### EO-1 Hyperion – the pioneer

The Earth Observing-1 satellite (EO-1) was launched on 21 November 2000 and carried Hyperion, the first civilian hyperspectral imager in space. Hyperion collected 220 spectral channels between 357 and 2576 nm with a bandwidth of 10 nm, at 30 m spatial resolution and a 7.7 km swath. The mission ended in 2017, but its archive is still available.

<img src="hyperion.jpg" width="720" height="240" align="centre" vspace="10" hspace="20">
<img src="hyperion_2.jpg" width="720" height="240" align="centre" vspace="10" hspace="20">

Hyperion images of outcrops near one of the largest mining operations in Jordan (Khirbat en-Nahas). With a natural color image (top), the ores appear uniformly dark, but different rock types can be identified with the many spectral bands in Hyperion (bottom).
Image: [NASA](https://earthobservatory.nasa.gov/features/EO1Tenth/page3.php){:target="_blank"}.

Read more about Hyperion [here](https://www.eoportal.org/satellite-missions/eo-1){:target="_blank"}.

#### AVIRIS – the airborne reference

The Airborne Visible/Infrared Imaging Spectrometer (AVIRIS) has been flown on aircraft since the late 1980s and delivers calibrated images in 224 contiguous spectral bands between 400 and 2500 nm. It has been flown on several platforms, including NASA's high-altitude ER-2 jet and Twin Otter turboprops, and is often used to test methods before they are applied to satellite data.

Read more about AVIRIS [here](https://aviris.jpl.nasa.gov/){:target="_blank"}.

#### Other hyperspectral imagers
* [PRISMA](https://www.eoportal.org/satellite-missions/prisma-hyperspectral){:target="_blank"} – Italian Space Agency (ASI), in orbit since 2019
* [EMIT](https://earth.jpl.nasa.gov/emit/){:target="_blank"} – NASA imaging spectrometer on the ISS, built to map mineral dust sources; also detects methane and CO₂ plumes
* DESIS – DLR/Teledyne imaging spectrometer on the ISS, 235 bands between 400 and 1000 nm
* [HICO](https://www.nasa.gov/news-release/space-station-ocean-imager-available-to-more-scientists/){:target="_blank"} – former ocean imager on the ISS
* [CORAL](https://airbornescience.jpl.nasa.gov/campaign/coral){:target="_blank"} – airborne campaign mapping coral reefs

## Hyperspectral remote sensing today: three trends

Hyperspectral remote sensing is changing fast. Here are three developments worth knowing.

### 1. From a few missions to many satellites

For a long time, only a handful of hyperspectral satellites existed. Today, commercial companies are launching their own:

| Mission | Operator | Since | What makes it special |
|---|---|---|---|
| Tanager-1, Tanager-2 | Planet, with Carbon Mapper | 2024, 2026 | 426 bands at about 5 nm, 30 m; focus on methane and CO₂ |
| Firefly | Pixxel | 2025 | 5 m resolution, more than 150 bands |
| Hyperfield | Kuva Space | 2024 | small satellites for agriculture and environment |

Sources: [Planet](https://www.businesswire.com/news/home/20260930836177/en/Planet-Launches-Suncatcher-Tanager-2-and-18-SuperDove-Satellites){:target="_blank"}, [Directory of hyperspectral satellite operators](https://newspaceeconomy.ca/2026/05/17/directory-of-hyperspectral-satellite-operators/){:target="_blank"}

### 2. Hyperspectral data for everyone: CHIME

ESA is building **CHIME** (Copernicus Hyperspectral Imaging Mission), two satellites planned for around 2029 and 2031 ([Gunter's Space Page](https://space.skyrocket.de/doc_sdat/chime.htm){:target="_blank"}).
Like Sentinel-2 today, CHIME is part of the Copernicus programme, so its data will be free and acquired regularly. It will complement Sentinel-2 for agriculture, soils, biodiversity and land cover mapping.

### 3. Foundation models for hyperspectral data

The idea behind the satellite embeddings in Unit 06 also reaches hyperspectral data. **SpectralEarth** is a training dataset of about 540,000 image patches from more than 11,000 EnMAP scenes, built to pretrain foundation models for hyperspectral images ([SpectralEarth, DLR and TUM](https://elib.dlr.de/214763/){:target="_blank"}).
Such models could make hyperspectral data easier to use, even with few labelled field samples.

## Learn more

The free online course [HYPERedu](https://eo-college.org/){:target="_blank"} (on EO College), developed with the EnMAP mission, teaches the fundamentals of hyperspectral remote sensing with videos, quizzes and EnMAP-Box exercises.
