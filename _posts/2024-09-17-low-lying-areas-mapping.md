---
layout: post
title: "Urban Flooding Study - Locating Potential Low-Lying Spots in Bangalore"
date: 2024-09-19
tags: ["Geospatial"]
---

---
### Background
- On Sep 5, 2022, major thunderstorm struck Bangalore city and IMD recorded 132 mm of rain in a span of 24 hours
- The incessant downpour caused many areas to get submerged including IT parks and residential areas
- India's IT hub reportedly registered a loss of over Rs 225 crores
- Historically, this area has seen rapid development during recent years causing significant built-up and depleting green cover due to which the surface run-off gets accumulated which is worsened by less natural infiltration and swollen drains thereby causing floods.

---
### Causes of Flooding
- Rapid unplanned development 
- Land use changes
- Increase in built-up areas 
- Decrease in vegetation cover
- Low infiltration and percolation
- Increase in surface run-off volume
- Overflow in Storm water drains (SWDs) due to poor Drainage 
- Encroachment of natural bodies e.g. lakes and SWDs
- Climate Change

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
### Case Studies
- Below we present two case studies around Whitefield and Outer Ring Road
- These areas are major IT Hubs and contribute significantly to employment
- Below, we present our validations with the ground truth where we show that the low lying areas identified by the model actually suffered damage during the torrential floods 

#### 1. Whitefield, Borewell Road
- I live in Nallurahalli which is a village in Whitefield.
- This area has seen rapid urbanization with surrounding EPIP zone being a bustling IT hub 
- It is a densely populated residential area with lots of built-up area and very less green cover
- Nallurahalli and surrounding villages suffered significant damages due to Sep 2022 floods
- Some apartments got flooded very bad with parking area fully submerge with neck-deep water levels
- The closeness of these apartments with nearby Storm Water Drain may also have played a part in the flooding.
- It was one of the disaster zones which was visited by the then CM Shri Basavaraj Bommaiah.

**Validation 1 - DNA Apartments**
- DNA apartment residents faced severe challenges during the floods
- Surrounding Storm Water Drain got swollen and water came inside the apartment which filled up the parking area significantly damaging vehicles
- Reportedly NDRF team had to rescue few stranded residents on boats
<img src="{{site.url}}/images/low_lying_areas/dna_apt_damage.png"/>

- Our assessment highlights this location as a low-lying area and thus aligns with the resulting damage.
<img src="{{site.url}}/images/low_lying_areas/dna_opulence.png"/>

**Validation 2 - SS Felicity Homes**
- Residents of SS Felicity homes reported neck-deep water level in their basement parking
- Vehicles were damaged and residents were stranded.
<img src="{{site.url}}/images/low_lying_areas/ss_felicity_damage.png"/>

Our assessment highlights this location as a low-lying area and thus aligns with the resulting damage.
<img src="{{site.url}}/images/low_lying_areas/ss_felicity.png"/>

#### 2. Outer Ring Road (ORR)
- ORR is an arterial road connecting major tech parks in Bellandur and surrounding villages
- The areas surrounding ORR have seen uncontrolled urbanization 
- During the Sep 2022 floods, the ORR was fully submerged with water as high as waist level flowing on the road

<img src="{{site.url}}/images/low_lying_areas/orr_damage.png"/>

Below, we show our assessment also validates this road has many low-lying hotspots which are prone to flooding.

**Validation 1 - Bellandur**
<img src="{{site.url}}/images/low_lying_areas/bellandur.png"/>

**Validation 2 - Devarabisanahalli**
<img src="{{site.url}}/images/low_lying_areas/devarabisanahalli.png"/>

**Validation 3 - Kadubisanahalli**
<img src="{{site.url}}/images/low_lying_areas/kadubisanahalli.png"/>

#### 3. Sarjapura Road
- Rainbow Drive Layout around Sarjapura Road is a luxurious residential area.
- This area got significantly damaged and many residents got stuck in their homes enitrely cut-off from basic amenitites. 
- National Disaster Response Force (NDRF) reportedly rescued 200 residents in boats in this area
- Encroachment of the nearby Halanayakanahalli Lake might have played a key role in the disaster

<img src="{{site.url}}/images/low_lying_areas/rbd_damage.png"/>

**Validation 1 - Rainbow Drive Layout**
<img src="{{site.url}}/images/low_lying_areas/rbd_layout.png"/>


#### 4. Yemalur, Old Airport Road
- Yemalur is home to many luxurious gated communities and villa settlements.
- Most prominent ones are Divyasree 77, Epsilon villas, Sobha Palladian etc with villas costing as high as 10 cr+.
- Many of these layouts were ghosted during the floods as residents took refuge in nearby hotels after being stranded and cut-off from basic necessities.

<img src="{{site.url}}/images/low_lying_areas/yemalur_damage.png"/>

<img src="{{site.url}}/images/low_lying_areas/yemalur_damage1.png"/>

<img src="{{site.url}}/images/low_lying_areas/yemalur_damage2.png"/>

**Validation 1 - Yemalur**
<img src="{{site.url}}/images/low_lying_areas/yemalur.png"/>

---
### Reference
- Implementation and code can be found on Github
https://github.com/gouherdanish/urban_flooding

---
### Conclusion
- Urban flooding is a growing cause of concern in Bangalore
- This app aims to identify the low-lying areas in Bangalore which are at risk of Urban flooding

