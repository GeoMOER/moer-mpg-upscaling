---
order: 3
title: "EX | Working with LiDAR data"
header:
  image: "/assets/images/teaser/lidar_coffee.jpg"
  caption: 'Image: [**Environmental Informatics Marburg**](https://www.uni-marburg.de/en/fb19/disciplines/physisch/environmentalinformatics){:target="_blank"}'
toc: true
---

## Example: LiDAR point clouds I/O and clipping

Since the LiDAR data can be quite large and needs higher processing power, only a small example is taken here. 
We will be using point cloud data and derive usable lidar metrics using the lidR package. 

Please refer [here](https://r-lidar.github.io/lidRbook/){:target="_blank"} to get a detailed idea of what all can be done using LiDAR data.

## Downloading LiDAR point cloud data

You can download the example point cloud data [here](https://next.hessenbox.de/index.php/f/808342192){:target="_blank"}

## Input/Output, Clipping and Visualizing point clouds

This following code shows you a simple input/output command and lidR::clip_roi(), which is similar to raster::crop and a simple 3D visualization.

```r
# load the following libraries

library(lidR)
library(sf)
library(terra) # the new lidR updates depend on terra functions
library(gstat)

# load in the example study area
aoi <- st_read("data/lidarArea.gpkg")

# load in the example point cloud file 
point_cloud <- readLAS("data/flm1.las") #point cloud for Lower Montane forest in Mt. Kilimanjaro

#check class 
class(point_cloud) #"LAS"

# set crs to the point cloud
st_crs(point_cloud) <- 32737 # WGS 84 , UTM37S

# crop out study area
flm1 <- clip_roi(point_cloud,aoi) #function 'lasclip' has been deprecated

# have a look at Lidar data structure
head(flm1@data)
       X       Y        Z    Intensity ReturnNumber NumberOfReturns ScanDirectionFlag EdgeOfFlightline Classification Synthetic_flag Keypoint_flag Withheld_flag ScanAngleRank UserData PointSourceID
# 303983.1 9649715 1903.269         0            3               3                 0                0              2          FALSE         FALSE         FALSE            -5        0             0
# 303983.1 9649715 1903.400         0            6               6                 0                0              0          FALSE         FALSE         FALSE            -5        0             0

# have a look at the range of intensity
range(flm1@data$Intensity)
# 0 34930

# unique classification 
unique(flm1@data$Classification) # ground data is class 2, 0 is unidentified
# 2 0

#to clean up the RAM
rm(point_cloud) #removes larger point cloud and frees up our space
gc() #garbage collector for cleaning the RAM

# 3D visualization
plot(flm1) #plotting las files will open a new window , you can move the data in 360 degrees to notice the vegetation structure

#save the output
writeLAS(flm1, "./data/flm1_clipped.las")

```

## Task 1

* Plot the LiDAR data again, but experiment with different columns for the coloring of the points
* What information could you gain from these plots?
* Filter the LiDAR data. Only keep points above an intensity value of 300. (Hint- use `lidR::filter_poi`)
* Plot them. What points are these? What could you do with those points?
* Filter the LiDAR data. Only keep the first returns. (Hint- use `lidR::filter_poi`)
* Plot them. What points are these? What could you do with those points?



