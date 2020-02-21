# Flood Risk Evaluation using FEMA's 7 Lifelines

#### Project Team: 
- Megha Zavar
- Magnus Bigelow
- John Wertz
- Scott Rosengrants

Analysis of flooding risk and comprehensive risk of Census tracts in the Denver metro area based on FEMA's 7 Lifelines.

### Problem Statements/ Objectives: 

**Problem Statement:** Prior to and during a disaster, it is important to understand the projected and actual effects of the event on the community, including its economic effects on critical services. FEMA has identified seven “lifelines” that require attention during a disaster: (1) safety and security; (2) Food, water, sheltering; (3) Health and medical; (4) Energy (power, fuel); (5) Communications; (6) Transportation; (7) hazardous waste. 

**Objective 1:** Collect business profiles from the Google Places API

**Objective 2:** Classify the businesses as FEMA Lifelines

**Objective 3:** Calculate flood risk of the geographic area

**Objective 4:** Identiy Risk Score for each Census tract in the effected disaster zone

**Objective 5:** Provide FEMA with an interactive dashboard to analyze the calculated risk of each Census tract


### Repo Structure: 
Included in this repository:
- Executive Summary
- code folder
  - Jupyter notebooks
    - census_to_zone.ipynb (assigns zones to each census tract, calculates final risk score)
    - define_zones.ipynb (creates zones based using DBSCAN)
    - flood_zones.ipynb ()
    - google_elevation_api.ipynb ()
    - google_places_nearby_EDA.ipynb
    - lifeline_defined_lists.ipynb ()
    - lifeline_zone_assignment.ipynb ()
    - scoring_zones.ipynb ()
- data folder (contains all datasets in csv,shp,json formats)
- images folder (contains visualizations of modeleling process)
- presentation slides (pdf format) for Light Technologies stakeholders
   
### Data Dictionary: 

|Variable Name|Type|Decription|
|---|---|---|
|geometry|	object|	Latitude and Longitude and box geo information|
|latitude|	float|	Latitude info extracted from geometry data|
|longitude|	float|	Longitude info extracted from geometry data|
|distance|	float|	The distance from point of origin (km)|
|elevation|	int|	The elevation from sea level (m)|
|name|	object|	Business name|
|types|	object|	Type of business|
|vicinity|	object|	Business address|
|lifeline_number|	Int|	Number assignment for lifeline (1-7)|
|lifeline_category|	object|	Description of lifeline|
   
### Executive Summary: 
See the included Executive Summary included in the repo. 
