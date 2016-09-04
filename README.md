# spatialMapData
---
title: "spatialMapData_Vignette"
author: "JT Lovell"
date: "September 4, 2016"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

### Part 1: Set up the environment
#### Install the package
##### If devtools is not installed, first install this package

```{r, eval = F}
install.packages("devtools")
```

##### Using devtools, install the development version of this data packages from github

```{r, eval = F}
library(devtools)
install_github("jtlovell/spatialMapData")
```

#### Load the package

```{r, include = FALSE}
library(devtools)
```
```{r}
library(spatialMapData)
```

### Part 2: Prepare the user defined data
#### Load some data that will be your spatial points
#####     Could also read in a csv or whatever
#####     Important that 1st two columns are latitude and longitude
######       - labeled as "lat" and "lon"

```{r}
ll<-data.frame(lat = c(35.99115, 42.41962, 31.04338, 
                       32.30290, 27.54986, 38.89690, 39.14070,
                       30.28415, 30.38398, 41.15430, 44.30680),
               lon = c(-97.04649, -85.37127, -97.34950, 
                       -94.97940, -97.88101, -92.21780,
                       -96.63890, -97.78162, -97.72938, 
                       -96.41530, -96.67050),
               label = c("Stillwater", "Hickory Corners", "Temple",
                         "Overton", "Kingsville", "Columbia",
                         "Manhattan", "AustinBFL", "AustinPKLE",
                         "Lincoln", "Brookings"),
               stringsAsFactors=F)
```

#### Make map boundaries
##### Here, I use the spatial points and a buffer of 5 deg lat and 2 deg lon. 

```{r}
extent <- with(ll, c(min(lon-5), max(lon+5), min(lat-2), max(lat+2)))
```

#### make a projection
##### Here i use the extent data to inform the projection

```{r}
proj.parse<-paste0("+proj=poly +lat_0=",
                   mean(extent[3:4]),
                   " +lon_0=",
                   mean(extent[1:2]),
                   " +x_0=0 +y_0=0 +ellps=WGS84 +datum=WGS84 +units=m +no_defs")
proj <- CRS(proj.parse)
```


### Part 3 (Optional): Read in raster data from bioclim (or elsewhere)
#####     Not evaluated here to speed up compiling

```{r, eval = F}
tmin<-getData('worldclim', var='bio', res=2.5)[["bio11"]]
tmin.c <- crop(tmin, extent)
```

### Part 4: Load the polygon / line / point files from the database
#### The package contains 15 processed shapefiles that have been transformed to uprojected spatialdataframes:
1. US Counties

```{r}
data(cb_2014_us_county_500k.shp)
```

2. Rivers

```{r}
data(MajorRivers_dd83.shp)
```

3. Country boundaries

```{r}
data(ne_10m_admin_0_boundary_lines_land.shp)
```

4. State/province boundaries

```{r}
data(ne_10m_admin_1_states_provinces.shp)
```

5. Coasts

```{r}
data(ne_10m_coastline.shp)
```

6. Lake borders

```{r}
data(ne_10m_lakes.shp)
```

7. Land borders

```{r}
data(ne_10m_land.shp)
```

8. Protected land borders

```{r}
data(ne_10m_parks_and_protected_lands_area.shp)
```

9. Protected rivers/coasts

```{r}
data(ne_10m_parks_and_protected_lands_line.shp)
```

10. Protected lands centerpoints

```{r}
data(ne_10m_parks_and_protected_lands_point.shp)
```

11. Protected lands rank data

```{r}
data(ne_10m_parks_and_protected_lands_scale_rank.shp)
```

12. Densely populated area boundaries

```{r}
data(ne_10m_populated_places.shp)
```

13. River centerlines

```{r}
data(ne_10m_rivers_lake_centerlines.shp)
```

14. Roads

```{r}
data(ne_10m_roads.shp)
```

15. Urban area boundaries

```{r}
data(ne_10m_urban_areas.shp)
```
