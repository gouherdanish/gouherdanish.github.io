---
layout: post
title: "Urban Flooding Study - Locating Potential Low-Lying Spots in Bangalore"
date: 2024-09-19
tags: ["Geospatial"]
---

---
### Background
- During the Bangalore floods in Sep 2022, many areas got submerged including IT parks and residential areas
- I live in Nallurahalli village in Whitefield zone, East Bangalore which suffered significant flooding during Sep 2022 floods
- It was one of the disaster zones which was visited by the then CM Shri BS Yediyurappa.
- Historically, this area has seen rapid development during recent years causing significant built-up and depleting green cover due to which the surface run-off gets accumulated which is worsened by less natural infiltration and swollen drains thereby causing floods.

---
### Causes of Flooding
- There are several reasons being cited for increase in urban flooding instances in Bangalore
    - Rapid unplanned development 
    - Land use changes
    - Increase in built-up areas 
    - Decrease in vegetation cover
    - Low infiltration and percolation
    - Increase in surface run-off volume
    - Overflow in Storm water drains (SWDs) due to poor Drainage 
    - Encroachment of natural bodies e.g. lakes and SWDs

---
### Data Source
- Elevation Data 
    - DEM data is available as open-source from SRTM mission (Shuttle Radar Topography Mission)
    - The resolution of the data is 30m (1 arc second)
    - Sample data for the study region was downloaded from USGS 
        - https://earthexplorer.usgs.gov/
- Bangalore Village Boundaries
    - This data was downloaded from KGIS website
        - https://kgis.ksrsac.in/kgis/downloads.aspx

---
### Image Segmentation Process
- Download DEM data in tiff format 
- Read DEM using Rioxarray in Python
- Perform pixel-wise segmentation
- Filter out low-lying pixels 
    - having lower elevation than neighboring pixels
- Save as point shapefile

--- 
### Streamlit App
- The app is started by running 
```
streamlit run app.py
```

- This is the landing page
<img src="{{site.url}}/images/low_lying_areas/landing_page.png"/>

- The user can select his village from the dropdown
<img src="{{site.url}}/images/low_lying_areas/dropdown.png"/>

- After selecting village, the app shows the data for that village
<img src="{{site.url}}/images/low_lying_areas/nallurahalli.png"/>


Note: 
- Online raster segmentation is expensive operation and causes latency on user's end. Therefore, the app uses static segmentation currently by default
- The app is not deployed on server and currently works in local

---
### Validation

- Nallurahalli is a locality in Whitefield where I live and it suffered significant flooding during Sep 2022 floods
- It was one of the disaster zones which was visited by the then CM Shri BS Yediyurappa.

